---
title: 'Exemplarische Vorgehensweise: Binden einer iOS Swift-Bibliothek'
description: Dieser Artikel enthält eine praktische Exemplarische Vorgehensweise zum Erstellen einer xamarin. IOS-Bindung für ein vorhandenes SWIFT-Framework (Gigya). Es behandelt Themen wie das Kompilieren eines SWIFT-Frameworks, die Bindung und die Verwendung der Bindung in einer xamarin. IOS-Anwendung.
ms.prod: xamarin
ms.assetid: B563ADE9-D0E3-4BC3-A288-66D2296BE706
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: 3c63b1a4ed58b0efcc510085934a5380e6049ae7
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853155"
---
# <a name="walkthrough-bind-an-ios-swift-library"></a>Exemplarische Vorgehensweise: Binden einer iOS Swift-Bibliothek

> [!IMPORTANT]
> Wir untersuchen derzeit die benutzerdefinierte Bindungs Verwendung auf der xamarin-Plattform. Nehmen Sie an [**dieser Umfrage**](https://www.surveymonkey.com/r/KKBHNLT) Teil, um zukünftige Entwicklungsbemühungen zu informieren.

Xamarin ermöglicht mobilen Entwicklern das Erstellen von plattformübergreifenden, nativen mobilen Umgebungen mithilfe von Visual Studio und c#. Die IOS Platform SDK-Komponenten können standardmäßig verwendet werden. In vielen Fällen möchten Sie jedoch auch Drittanbieter-sdgs verwenden, die für diese Plattform entwickelt wurden, die xamarin über Bindungen ermöglicht. Wenn Sie ein Ziel-C-Framework von Drittanbietern in Ihre xamarin. IOS-Anwendung integrieren möchten, müssen Sie eine xamarin. IOS-Bindung für die Anwendung erstellen, bevor Sie Sie in Ihren Anwendungen verwenden können.

Die IOS-Plattform wird zusammen mit den nativen Sprachen und Tools ständig weiterentwickelt, und SWIFT ist einer der dynamischsten Bereiche in der IOS-Entwicklungswelt. Es gibt eine Reihe von SDK von Drittanbietern, die bereits von Ziel-C zu SWIFT migriert wurden und neue Herausforderungen darstellen. Obwohl der SWIFT-Bindungsprozess dem Ziel-C ähnelt, sind zusätzliche Schritte und Konfigurationseinstellungen erforderlich, um eine xamarin. IOS-Anwendung, die für den AppStore akzeptabel ist, erfolgreich zu erstellen und auszuführen.

Das Ziel dieses Dokuments besteht darin, ein allgemeines Verfahren für die Lösung dieses Szenarios aufzuzeigen und eine ausführliche Schritt-für-Schritt-Anleitung mit einem einfachen Beispiel bereitzustellen.

## <a name="background"></a>Hintergrund

Swift wurde anfänglich von Apple in 2014 eingeführt und ist nun in Version 5,1 mit Einführung von Frameworks von Drittanbietern enthalten. Sie haben einige Optionen für die Bindung eines SWIFT-Frameworks, und in diesem Dokument wird der Ansatz mit dem mit der Ziel-C generierten Schnittstellen Header beschrieben. Der-Header wird automatisch von den Xcode-Tools erstellt, wenn ein Framework erstellt wird, und es wird als Möglichkeit verwendet, von der verwalteten Welt mit der SWIFT-Welt zu kommunizieren.

## <a name="prerequisites"></a>Voraussetzungen

Um diese exemplarische Vorgehensweise abzuschließen, benötigen Sie Folgendes:

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Objektive Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started#installing-objective-sharpie)
- [AppCenter-CLI](https://docs.microsoft.com/appcenter/test-cloud/) (optional)

## <a name="build-a-native-library"></a>Erstellen einer nativen Bibliothek

Der erste Schritt besteht im Erstellen eines nativen SWIFT-Frameworks mit aktiviertem Ziel-C-Header. Das Framework wird normalerweise von einem Entwickler von Drittanbietern bereitgestellt, und der Header ist im folgenden Verzeichnis in das Paket eingebettet: ** \<FrameworkName> . Framework/Headers/ \<FrameworkName> -Swift. h**.

Dieser Header macht die öffentlichen Schnittstellen verfügbar, die zum Erstellen von xamarin. IOS-Bindungs Metadaten und zum Generieren von c#-Klassen verwendet werden, die die SWIFT Framework-Member verfügbar machen. Wenn der Header nicht vorhanden ist oder eine unvollständige öffentliche Schnittstelle aufweist (z. b. werden keine Klassen/Member angezeigt), haben Sie zwei Möglichkeiten:

- Aktualisieren Sie den SWIFT-Quellcode, um den Header zu generieren, und markieren Sie die erforderlichen Member mit dem `@objc` Attribut.
- Erstellen eines Proxy-Frameworks, in dem Sie die öffentliche Schnittstelle steuern und alle Aufrufe an das zugrunde liegende Framework überstellen

In diesem Tutorial wird der zweite Ansatz beschrieben, da er weniger Abhängigkeiten von Quellcode von Drittanbietern aufweist, der nicht immer verfügbar ist. Ein weiterer Grund, den ersten Ansatz zu vermeiden, ist der zusätzliche Aufwand, der zur Unterstützung zukünftiger frameworkänderungen erforderlich ist Nachdem Sie mit dem Hinzufügen von Änderungen am Quellcode von Drittanbietern begonnen haben, sind Sie dafür verantwortlich, diese Änderungen zu unterstützen und ggf. mit jedem zukünftigen Update zusammenzuführen.

In diesem Tutorial wird z. b. eine Bindung für das [Gigya SWIFT SDK](https://developers.gigya.com/display/GD/Swift+SDK) erstellt:

1. Öffnen Sie Xcode, und erstellen Sie ein neues SWIFT-Framework, das als Proxy zwischen xamarin. IOS-Code und einem SWIFT-Framework von Drittanbietern verwendet wird. Klicken Sie auf **Datei > neues > Projekt** , und befolgen Sie die Schritte im Assistenten:

    ![Xcode-Framework-Projekt erstellen](walkthrough-images/xcode-create-framework-project.png)

    ![Xcode Name Framework-Projekt](walkthrough-images/xcode-name-framework-project.png)

1. Laden Sie das [Gigya-Framework](https://developers.gigya.com/display/GD/Swift+SDK) von der Entwickler Website herunter, und entpacken Sie es. Zum Zeitpunkt des Schreibens ist die neueste Version [Gigya SWIFT SDK 1.0.9](https://downloads.gigya.com/predownload?fileName=Swift-Core-framework-1.0.9.zip)

1. Wählen Sie im Projektdateien-Explorer den **swiftframeworkproxy** aus, und wählen Sie dann die Registerkarte Allgemein aus

1. Verschieben Sie das Paket " **Gigya. Framework** " per Drag & Drop in die Liste Xcode-Frameworks und Bibliotheken auf der Registerkarte Allgemein, und aktivieren Sie die Option **Elemente bei Bedarf kopieren** , während Sie das Framework

    ![Xcode-Kopier Framework](walkthrough-images/xcode-copy-framework.png)

    Stellen Sie sicher, dass das SWIFT-Framework dem Projekt hinzugefügt wurde, da andernfalls die folgenden Optionen nicht verfügbar sind.

1. Stellen Sie sicher, dass die Option **nicht einbetten** ausgewählt ist, die später manuell gesteuert wird:

    [![Xcode-donotembed-Option](walkthrough-images/xcode-donotembed-option.png)](walkthrough-images/xcode-donotembed-option.png#lightbox)

1. Stellen Sie sicher, dass die Option Buildeinstellungen **immer SWIFT-Standard Bibliotheken einbetten**. dazu gehören SWIFT-Bibliotheken, für die das Framework auf Nein gesetzt ist. Sie werden später manuell gesteuert, welche SWIFT-dylics in das endgültige Paket aufgenommen werden:

    [![Xcode-Option ' false ' immer einbetten](walkthrough-images/xcode-alwaysembedfalse-option.png)](walkthrough-images/xcode-alwaysembedfalse-option.png#lightbox)

1. Stellen Sie sicher, dass die Option **Bitcode aktivieren** auf **Nein**festgelegt ist. Ab sofort enthält xamarin. IOS keinen Bitcode, während Apple erfordert, dass alle Bibliotheken dieselben Architekturen unterstützen:

    [![Xcode enable Bitcode false (Option)](walkthrough-images/xcode-enablebitcodefalse-option.png)](walkthrough-images/xcode-enablebitcodefalse-option.png#lightbox)

    Sie können überprüfen, ob für das resultierende Framework die Bitcode-Option deaktiviert ist, indem Sie den folgenden Terminal Befehl für das Framework ausführen:

    ```bash
    otool -l SwiftFrameworkProxy.framework/SwiftFrameworkProxy | grep __LLVM
    ```

    Die Ausgabe sollte leer sein, andernfalls sollten Sie die Projekteinstellungen für die jeweilige Konfiguration überprüfen.

1. Stellen Sie sicher, dass die Option **Ziel-C-generierter Schnittstellen Header Name** aktiviert ist, und gibt einen Header Namen an. Der Standardname lautet ** \<FrameworkName> -Swift. h**:

    [![Xcode-objectice-c-Header aktiviert (Option)](walkthrough-images/xcode-objcheaderenabled-option.png)](walkthrough-images/xcode-objcheaderenabled-option.png#lightbox)

1. Machen Sie die gewünschten Methoden verfügbar, markieren Sie Sie mit dem `@objc` Attribut, und wenden Sie die unten definierten Regeln an Wenn Sie das Framework ohne diesen Schritt erstellen, ist der generierte Ziel-C-Header leer, und xamarin. IOS kann nicht auf die Mitglieder des Swift-Frameworks zugreifen. Machen Sie die Initialisierungs Logik für das zugrunde liegende Gigya SWIFT SDK verfügbar, indem Sie eine neue SWIFT-Datei **swiftframeworkproxy. Swift** erstellen und den folgenden Code definieren:

    ```swift
    import Foundation
    import UIKit
    import Gigya

    @objc(SwiftFrameworkProxy)
    public class SwiftFrameworkProxy : NSObject {

        @objc
        public func initFor(apiKey: String) -> String {
            Gigya.sharedInstance().initFor(apiKey: apiKey)
            let gigyaDomain = Gigya.sharedInstance().config.apiDomain
            let result = "Gigya initialized with domain: \(gigyaDomain)"
            return result
        }
    }
    ```

    Einige wichtige Hinweise zum obigen Code:

    - Importieren Sie das Gigya-Modul hier aus dem ursprünglichen Gigya-SDK von Drittanbietern, und greifen Sie jetzt auf alle Member des Frameworks zu.
    - Markieren Sie die swiftframeworkproxy-Klasse mit dem- `@objc` Attribut, das einen Namen angibt; andernfalls wird ein eindeutiger nicht lesbarer Name generiert, z `_TtC19SwiftFrameworkProxy19SwiftFrameworkProxy` . b.. Der Typname sollte eindeutig definiert werden, da er später anhand seines Namens verwendet wird.
    - Erben Sie die Proxy Klasse von `NSObject` , andernfalls wird Sie nicht in der Ziel-C-Header Datei generiert.
    - Markieren Sie alle Member, die als verfügbar gemacht werden sollen `public` .

1. Ändern Sie die Schema-Buildkonfiguration von **Debug** in **Release**. Öffnen Sie hierzu das Dialogfeld **Xcode-> Ziel > Schema bearbeiten** , und legen Sie dann die **buildkonfigurationsoption** auf **Release**fest:

    ![Xcode-Bearbeitungs Schema](walkthrough-images/xcode-edit-scheme.png)

    ![Xcode-Bearbeitungs Schema Version](walkthrough-images/xcode-edit-scheme-release.png)

1. An diesem Punkt kann das Framework erstellt werden. Erstellen Sie das Framework für Simulator-und Geräte Architekturen, und kombinieren Sie die Ausgaben dann als einzelnes frameworkpaket. Identifizieren Sie die installierten SDK-Versionen, um den Quellcode mit dem **xcodebuild** -Tool zu erstellen:

    ```bash
    xcodebuild -showsdks
    ```

    Die Ausgabe sieht etwa wie folgt aus:

    ```bash
    iOS SDKs:
            iOS 13.2                        -sdk iphoneos13.2
    iOS Simulator SDKs:
            Simulator - iOS 13.2            -sdk iphonesimulator13.2
    macOS SDKs:
            DriverKit 19.0                  -sdk driverkit.macosx19.0
            macOS 10.15                     -sdk macosx10.15
    tvOS SDKs:
            tvOS 13.2                       -sdk appletvos13.2
    tvOS Simulator SDKs:
            Simulator - tvOS 13.2           -sdk appletvsimulator13.2
    watchOS SDKs:
            watchOS 6.1                     -sdk watchos6.1
    watchOS Simulator SDKs:
            Simulator - watchOS 6.1         -sdk watchsimulator6.1
    ```

    Wählen Sie eine gewünschte IOS SDK-und IOS Simulator SDK-Version aus, in diesem Fall Version 13,2, und führen Sie den Build mit dem folgenden Befehl aus:

    ```bash
    xcodebuild -sdk iphonesimulator13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    xcodebuild -sdk iphoneos13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    ```

    > [!TIP]
    > Wenn Sie über einen Arbeitsbereich anstelle von Project verfügen, erstellen Sie den Arbeitsbereich, und geben Sie das Ziel als erforderlichen Parameter an. Sie möchten auch ein Ausgabeverzeichnis angeben, da dieses Verzeichnis für Arbeitsbereiche anders ist als bei Projektbuilds.

    > [!TIP]
    > Sie können auch [das](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L3-L14) Hilfsskript verwenden, um das Framework für alle anwendbaren Architekturen zu erstellen oder es einfach aus dem Xcode-Wechsel Simulator und-Gerät in der Zielauswahl zu erstellen.

1. Es gibt zwei SWIFT-Frameworks, eine für jede Plattform, die Sie als ein einzelnes Paket kombinieren, das später in ein xamarin. IOS-Bindungs Projekt eingebettet werden soll. Zum Erstellen eines FAT-Frameworks, das beide Architekturen kombiniert, müssen Sie die folgenden Schritte ausführen. Das frameworkpaket ist nur ein Ordner, sodass Sie alle Arten von Vorgängen ausführen können, z. b. das Hinzufügen, entfernen und Ersetzen von Dateien:

    - Navigieren Sie zum Buildausgabeordner mit den Unterordnern " **Release-iPhoneOS** " und " **Release-iphonesimulator** ", und kopieren Sie eines der Frameworks als anfängliche Version der endgültigen Ausgabe (FAT Framework).

        ```bash
        cp -R "Release-iphoneos" "Release-fat"
        ```

    - Kombinieren von Modulen aus einem anderen Build mit den FAT Framework-Modulen

        ```bash
        cp -R "Release-iphonesimulator/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/" "Release-fat/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/"
        ```

    - Kombinieren der iPhoneOS + das iphonesimulator-Konfiguration als FAT-Framework

        ```bash
        lipo -create -output "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphoneos/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphonesimulator/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

    - Überprüfen der Ergebnisse

        ```bash
        lipo -info "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

        Die Ausgabe sollte Folgendes Renderingergebnis enthalten, das den Namen des Frameworks und der enthaltenen Architekturen widerspiegelt:

        ```bash
        Architectures in the fat file: Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy are: x86_64 arm64
        ```

    > [!TIP]
    > Wenn Sie nur eine einzelne Plattform unterstützen möchten (z. b. Wenn Sie eine APP erstellen, die nur auf einem Gerät ausgeführt werden kann), können Sie den Schritt überspringen, um die FAT-Bibliothek zu erstellen und das Ausgabe Framework aus dem zuvor erstellten gerätebuild zu verwenden.

    > [!TIP]
    > Sie können auch [das](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L16-L24) Hilfsskript verwenden, um das FAT-Framework zu erstellen, mit dem alle obigen Schritte automatisiert werden.

## <a name="prepare-metadata"></a>Vorbereiten von Metadaten

Zu diesem Zeitpunkt sollte das Framework mit dem Ziel-C generierten Schnittstellen Header bereit für die Verwendung durch eine xamarin. IOS-Bindung sein.  Der nächste Schritt besteht im Vorbereiten der API-Definitions Schnittstellen, die von einem Bindungs Projekt zum Generieren von c#-Klassen verwendet werden. Diese Definitionen können manuell oder automatisch durch das Ziel- [Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/) -Tool und die generierte Header Datei erstellt werden. Verwenden Sie Sharpie, um die Metadaten zu generieren:

1. Laden Sie das neueste [Ziel-Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/) -Tool von der offiziellen Download Website herunter, und installieren Sie es, indem Sie den Assistenten befolgen. Sobald die Installation abgeschlossen ist, können Sie Sie überprüfen, indem Sie den Befehl "Sharpie" ausführen:

    ```bash
    sharpie -v
    ```

1. Generieren Sie Metadaten mithilfe von Sharpie und der automatisch generierten Ziel-C-Header Datei:

    ```bash
    sharpie bind --sdk=iphoneos13.2 --output="XamarinApiDef" --namespace="Binding" --scope="Release-fat/SwiftFrameworkProxy.framework/Headers/" "Release-fat/SwiftFrameworkProxy.framework/Headers/SwiftFrameworkProxy-Swift.h"
    ```

    Die Ausgabe spiegelt die generierten Metadatendateien wider: **ApiDefinitions.cs** und **StructsAndEnums.cs**. Speichern Sie diese Dateien für den nächsten Schritt, um Sie zusammen mit den nativen verweisen in ein xamarin. IOS-Bindungs Projekt einzubeziehen:

    ```bash
    Parsing 1 header files...
    Binding...
        [write] ApiDefinitions.cs
        [write] StructsAndEnums.cs
    ```

    Das Tool generiert c#-Metadaten für jeden verfügbar gemachten Ziel-C-Member, der in etwa wie der folgende Code aussieht. Wie Sie sehen können, kann Sie manuell definiert werden, da Sie über ein lesbares Format und eine einfache Zuordnung von Membern verfügt:

    ```csharp
    [Export ("initForApiKey:")]
    string InitForApiKey (string apiKey);
    ```

    > [!TIP]
    > Der Name der Header Datei ist möglicherweise anders, wenn Sie die Xcode-Standardeinstellungen für den Header Namen geändert haben. Standardmäßig weist Sie den Namen eines Projekts mit dem Suffix **-SWIFT** auf. Sie können die Datei und ihren Namen immer überprüfen, indem Sie zum Ordner "Headers" des frameworkpakets navigieren.

    > [!TIP]
    > Im Rahmen des Automatisierungsprozesses können Sie [das](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L35) Hilfsskript zum automatischen Generieren von Metadaten verwenden, sobald das FAT-Framework erstellt wird.

## <a name="build-a-binding-library"></a>Erstellen einer Bindungs Bibliothek

Der nächste Schritt besteht darin, ein xamarin. IOS-Bindungs Projekt mithilfe der Visual Studio-Bindungs Vorlage zu erstellen, erforderliche Metadaten hinzuzufügen, Native Verweise zu erstellen und dann das Projekt zu erstellen, um eine verwendbare Bibliothek zu erstellen:

1. Öffnen Sie Visual Studio für Mac, und erstellen Sie ein neues xamarin. IOS-Bindungs Bibliotheksprojekt, benennen Sie es mit einem Namen, in diesem Fall swiftframeworkproxy. Binding, und schließen Sie den Assistenten ab. Die xamarin. IOS-Bindungs Vorlage befindet sich unter folgendem Pfad: **IOS-> Bibliothek > Bindungs Bibliothek**:

    ![Visual Studio-Bindungs Bibliothek erstellen](walkthrough-images/visualstudio-create-binding-library.png)

1. Löschen Sie vorhandene Metadatendateien **ApiDefinition.cs** und **structs.cs** , da Sie vollständig durch die Metadaten ersetzt werden, die vom Ziel-Sharpie-Tool generiert werden.
1. Kopieren Sie die von Sharpie generierten Metadaten in einem der vorherigen Schritte, wählen Sie im Eigenschaften Fenster die folgende Buildaktion aus: **objbindingapidefinition** für die Datei **ApiDefinitions.cs** und **objbindingcoresource** für die **StructsAndEnums.cs** -Datei:

    ![Visual Studio-Projektstruktur Metadaten](walkthrough-images/visualstudio-project-structure-metadata.png)

    Die Metadaten selbst beschreiben alle verfügbar gemachten Ziel-C-Klassen und-Member mithilfe der Programmiersprache c#. Die ursprüngliche Ziel-C-Header Definition kann neben der c#-Deklaration angezeigt werden:

    ```csharp
    // @interface SwiftFrameworkProxy : NSObject
    [BaseType (typeof(NSObject))]
    interface SwiftFrameworkProxy
    {
        // -(NSString * _Nonnull)initForApiKey:(NSString * _Nonnull)apiKey __attribute__((objc_method_family("none"))) __attribute__((warn_unused_result));
        [Export ("initForApiKey:")]
        string InitForApiKey (string apiKey);
    }
    ```

    Obwohl es sich um einen gültigen c#-Code handelt, wird er nicht wie folgt verwendet, sondern stattdessen von xamarin. IOS-Tools verwendet, um c#-Klassen basierend auf dieser Metadatendefinition zu generieren. Daher erhalten Sie anstelle der-Schnittstelle swiftframeworkproxy eine c#-Klasse mit dem gleichen Namen, die durch ihren xamarin. IOS-Code instanziiert werden kann. Diese Klasse ruft Methoden, Eigenschaften und andere Member ab, die von den Metadaten definiert werden, die Sie in einer c#-Weise aufzurufen.

1. Fügen Sie den nativen Verweis auf das generierte frühere FAT-Framework sowie jede Abhängigkeit dieses Frameworks hinzu. Fügen Sie in diesem Fall beide swiftframeworkproxy-und nativen Gigya-Framework-Verweise zum Bindungs Projekt hinzu:

    - Um native frameworkverweise hinzuzufügen, öffnen Sie Finder und navigieren zum Ordner mit den Frameworks. Ziehen Sie die Frameworks unter den Speicherort für Native Verweise in der Projektmappen-Explorer. Alternativ können Sie die Kontextmenü Option im Ordner Native Verweise verwenden und auf **nativen Verweis hinzufügen** klicken, um die Frameworks zu suchen und hinzuzufügen:

    ![Native Verweise in Visual Studio-Projektstruktur](walkthrough-images/visualstudio-project-structure-nativerefs.png)

    - Aktualisieren Sie die Eigenschaften jeder nativen Referenz, und überprüfen Sie drei wichtige Optionen:

        - Smart Link = true festlegen
        - Force Load = false festlegen
        - Festlegen der Liste der Frameworks, die zum Erstellen der ursprünglichen Frameworks verwendet werden In diesem Fall hat jedes Framework nur zwei Abhängigkeiten: Foundation und UIKit. Legen Sie diese Einstellung auf das Feld Frameworks fest:

        ![Visual Studio nativeref-Proxy Optionen](walkthrough-images/visualstudio-nativeref-proxy-options.png)

        Wenn Sie zusätzliche Linker-Flags angeben müssen, legen Sie diese im Feld Linker Flags fest. Behalten Sie in diesem Fall den Wert leer.

    - Geben Sie bei Bedarf zusätzliche Linker-Flags an. Wenn die Bibliothek, die Sie binden, nur die Ziel-C-APIs verfügbar macht, aber intern SWIFT verwendet, werden möglicherweise Probleme wie die folgenden angezeigt:

        ```Console
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftQuartzCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCoreImage
        ```

        In den Eigenschaften des Bindungs Projekts für die native Bibliothek müssen den Linker-Flags die folgenden Werte hinzugefügt werden:

        ```Linker
        L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphonesimulator/ -L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos -Wl,-rpath -Wl,@executable_path/Frameworks
        ```

        Die ersten beiden Optionen (die  `-L ...`   ) teilen dem systemeigenen Compiler mit, wo die SWIFT-Bibliotheken zu finden sind. Der Native Compiler ignoriert Bibliotheken, die nicht die richtige Architektur aufweisen. Dies bedeutet, dass es möglich ist, den Speicherort sowohl für simulatorbibliotheken als auch für Geräte Bibliotheken gleichzeitig zu übergeben, sodass er sowohl für Simulator-als auch für Geräte Builds funktioniert (diese Pfade sind nur für IOS korrekt; für tvos und watchos müssen Sie aktualisiert werden). Ein Nachteil ist, dass dieser Ansatz erfordert, dass sich der korrekte Xcode in/Application/Xcode.app befindet, wenn der Consumer der Bindungs Bibliothek über Xcode an einem anderen Speicherort verfügt, funktioniert er nicht. Die alternative Lösung besteht darin, diese Optionen in den zusätzlichen mberührungs-Argumenten in den IOS-Buildoptionen des ausführbaren Projekts () hinzuzufügen `--gcc_flags -L... -L...` . Mit der dritten Option speichert der Native Linker den Speicherort der SWIFT-Bibliotheken in der ausführbaren Datei, damit das Betriebssystem Sie finden kann.

1. Die letzte Aktion besteht darin, die Bibliothek zu erstellen und sicherzustellen, dass keine Kompilierungsfehler vorliegen. Sie werden häufig feststellen, dass Bindungs Metadaten, die vom Ziel-Sharpie erzeugt werden, mit dem-Attribut kommentiert werden  `[Verify]`   . Diese Attribute geben an, dass Sie sicherstellen sollten, dass Ziel-Sharpie das richtige ist, indem Sie die Bindung mit der ursprünglichen Ziel-C-Deklaration vergleichen (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt wird). Weitere Informationen zu Membern, die mit dem-Attribut gekennzeichnet sind, finden Sie unter [folgendem Link](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/platform/verify). Nachdem das Projekt erstellt wurde, kann es von einer xamarin. IOS-Anwendung verwendet werden.

## <a name="consume-the-binding-library"></a>Verwenden der Bindungs Bibliothek

Der letzte Schritt besteht darin, die xamarin. IOS-Bindungs Bibliothek in einer xamarin. IOS-Anwendung zu verwenden. Erstellen Sie ein neues xamarin. IOS-Projekt, fügen Sie der Bindungs Bibliothek einen Verweis hinzu, und aktivieren Sie das Gigya SWIFT SDK:

1. Erstellen Sie ein xamarin. IOS-Projekt. Sie können die **IOS-> App > Einzelansicht-App** als Ausgangspunkt verwenden:

    ![Visual Studio-app neu](walkthrough-images/visualstudio-app-new.png)

1. Fügen Sie dem zuvor erstellten Ziel Projekt oder der zuvor erstellten DLL einen Bindungs Projekt Verweis hinzu. Behandeln Sie die Bindungs Bibliothek als reguläre xamarin. IOS-Bibliothek:

    ![Visual Studio-app-Refs](walkthrough-images/visualstudio-app-refs.png)

1. Aktualisieren des Quellcodes der APP und Hinzufügen der Initialisierungs Logik zum primären ViewController, der das Gigya SDK aktiviert

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
        var proxy = new SwiftFrameworkProxy();
        var result = proxy.InitForApiKey("APIKey");
        System.Diagnostics.Debug.WriteLine(result);
    }
    ```

1. Erstellen Sie eine Schaltfläche mit dem Namen **btnlogin** , und fügen Sie den folgenden Schaltflächen-Click-Handler hinzu, um einen Authentifizierungs Fluss zu aktivieren

    ```csharp
    private void btnLogin_Tap(object sender, EventArgs e)
    {
        _proxy.LoginWithProvider(GigyaSocialProvidersProxy.Instagram, this, (result, error) =>
        {
            // process your login result here
        });
    }
    ```

1. Führen Sie die app in der Debugausgabe aus, um die folgende Zeile anzuzeigen: `Gigya initialized with domain: us1.gigya.com` . Klicken Sie auf die Schaltfläche, um den Authentifizierungs Ablauf zu aktivieren:

    [![SWIFT-Proxy Ergebnis](walkthrough-images/swiftproxy-result.png)](walkthrough-images/swiftproxy-result.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine xamarin. IOS-APP und eine Bindungs Bibliothek erstellt, die ein Swift-Framework nutzt. Die obige Anwendung kann unter IOS 12.2 und höher ausgeführt werden, da Apple ab dieser IOS-Version die [ABI-Stabilität eingeführt](https://swift.org/blog/swift-5-1-released/) hat und jeder IOS-Start von 12,2 und mehr SWIFT-Laufzeitbibliotheken enthält, die zum Ausführen der mit SWIFT 5.1 + kompilierten Anwendung verwendet werden können. Wenn Sie Unterstützung für frühere IOS-Versionen hinzufügen müssen, müssen Sie einige weitere Schritte ausführen:

1. Um Unterstützung für IOS 12,1 und früher hinzuzufügen, möchten Sie bestimmte SWIFT-dylisb zur Kompilierung Ihres Frameworks bereitstellen. Verwenden Sie das nuget-Paket [xamarin. IOS. swiftruntimesupport](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/) , um die erforderlichen Bibliotheken mit ihrer IPA-Datei zu verarbeiten und zu kopieren. Fügen Sie dem Ziel Projekt den nuget-Verweis hinzu, und erstellen Sie die Anwendung neu. Es sind keine weiteren Schritte erforderlich, das nuget-Paket installiert bestimmte Aufgaben, die mit dem Buildprozess ausgeführt werden, identifizieren Sie die erforderlichen SWIFT-dylisb, und Verpacken Sie Sie mit der endgültigen IPA-Datei.

1. Um die APP an den App Store zu übermitteln, verwenden Sie Xcode und die Option "verteilen", wodurch die IPA-Datei aktualisiert wird, und der **swiftsupport** -Ordner dylisb, damit Sie vom AppStore akzeptiert wird:

    Die APP wird archiviert. Wählen Sie im Menü Visual Studio für Mac die Option **Build > Archive für die Veröffentlichung**aus:

    ![Visual Studio-Archiv zum Veröffentlichen](walkthrough-images/visualstudio-archiveforpublishing.png)

    Mit dieser Aktion wird das Projekt erstellt und dem Planer erreicht, auf das Xcode für die Verteilung zugreifen kann.

    Die Verteilung über Xcode. Öffnen Sie Xcode, und navigieren Sie zum **Fenster > Organisator** -Menüoption:

    ![Visual Studio-Archive](walkthrough-images/visualstudio-archives.png)

    Wählen Sie das im vorherigen Schritt erstellte Archiv aus, und klicken Sie auf die Schaltfläche App verteilen. Führen Sie den Assistenten aus, um die Anwendung in den AppStore hochzuladen.

1. Dieser Schritt ist optional, aber es ist wichtig, zu überprüfen, ob Ihre APP unter IOS 12,1 und früher sowie 12,2 ausgeführt werden kann. Dies ist mit der Test Cloud-und UITest-Framework möglich. Erstellen Sie ein UITest-Projekt und einen grundlegenden UI-Test, der die APP ausführt:

    - Erstellen Sie ein UITest-Projekt, und konfigurieren Sie es für Ihre xamarin. IOS-Anwendung:

        ![Visual Studio UITest neu](walkthrough-images/visualstudio-uitest-new.png)

        > [!TIP]
        > Weitere Informationen zum Erstellen eines UITest-Projekts und zum Konfigurieren des Projekts für Ihre App finden Sie unter [folgendem Link](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

    - Erstellen Sie einen grundlegenden Test, um die APP auszuführen und einige der SWIFT SDK-Funktionen zu verwenden. Mit diesem Test wird die App aktiviert, versucht, sich anzumelden, und drückt dann die Schaltfläche "Abbrechen":

        ```csharp
        [Test]
        public void HappyPath()
        {
            app.WaitForElement(StatusLabel);
            app.WaitForElement(LoginButton);
            app.Screenshot("App loaded.");
            Assert.AreEqual(app.Query(StatusLabel).FirstOrDefault().Text, "Gigya initialized with domain: us1.gigya.com");

            app.Tap(LoginButton);
            app.WaitForElement(GigyaWebView);
            app.Screenshot("Login activated.");

            app.Tap(CancelButton);
            app.WaitForElement(LoginButton);
            app.Screenshot("Login cancelled.");
        }
        ```

        > [!TIP]
        > Weitere Informationen zum uitests-Framework und zur Benutzeroberflächen Automatisierung finden Sie unter [folgendem Link](https://docs.microsoft.com/appcenter/test-cloud/uitest/).

    - Erstellen Sie eine IOS-app in App Center, erstellen Sie einen neuen Testlauf mit einem neuen Gerät, das zum Ausführen des Tests festgelegt ist:

        ![Visual Studio App Center neu](walkthrough-images/visualstudio-appcenter-new.png)

        > [!TIP]
        > Weitere Informationen zu AppCenter-Test Cloud finden Sie unter [folgendem Link](https://docs.microsoft.com/appcenter/test-cloud/).

    - Installieren der AppCenter-CLI

        ```bash
        npm install -g appcenter-cli
        ```

        > [!IMPORTANT]
        > Stellen Sie sicher, dass der Knoten v 6.3 oder höher installiert ist.

    - Führen Sie den Test mit dem folgenden Befehl aus. Stellen Sie außerdem sicher, dass Ihre AppCenter-Befehlszeile aktuell angemeldet ist.

        ```bash
        appcenter test run uitest --app "Mobile-Customer-Advisory-Team/SwiftBinding.iOS" --devices a7e7cb50 --app-path "Xamarin.SingleView.ipa" --test-series "master" --locale "en_US" --build-dir "Xamarin/Xamarin.SingleView.UITests/bin/Debug/"
        ```

    - Überprüfen Sie das Ergebnis. Navigieren Sie im AppCenter-Portal zur **app > Test > Testläufe**:

        ![Visual Studio AppCenter UITest-Ergebnis](walkthrough-images/visualstudio-appcenter-uitest-result.png)

        Wählen Sie den gewünschten Testlauf aus, und überprüfen Sie das Ergebnis:

        ![Visual Studio AppCenter UITest-Ausführungen](walkthrough-images/visualstudio-appcenter-uitest-runs.png)

Sie haben eine einfache xamarin. IOS-Anwendung entwickelt, die ein natives SWIFT-Framework über eine xamarin. IOS-Bindungs Bibliothek verwendet. Das Beispiel bietet eine einfache Möglichkeit, das ausgewählte Framework zu verwenden, und in der realen Anwendung müssen Sie mehr APIs verfügbar machen und Metadaten für diese APIs vorbereiten. Durch das Skript zum Generieren von Metadaten werden zukünftige Änderungen an den Framework-APIs vereinfacht.

## <a name="related-links"></a>Verwandte Links

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Visual Studio für Mac](https://visualstudio.microsoft.com/downloads)
- [Objektive Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/)
- [Sharpie-Metadatenüberprüfung](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/platform/verify)
- [Bindungs Ziel-C-Framework](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough)
- [Gigya IOS SDK (SWIFT-Framework)](https://developers.gigya.com/display/GD/Swift+SDK)
- [SWIFT 5,1 ABI-Stabilität](https://swift.org/blog/swift-5-1-released/)
- [SWiF truntimesupport-nuget](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)
- [Xamarin UITest-Automatisierung](https://docs.microsoft.com/appcenter/test-cloud/uitest/)
- [Xamarin. IOS UITest-Konfiguration](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [AppCenter-Test Cloud](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [Beispielprojektrepository](https://github.com/alexeystrakh/xamarin-binding-swift-framework)
