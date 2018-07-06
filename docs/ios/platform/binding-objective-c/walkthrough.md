---
title: 'Exemplarische Vorgehensweise: Binden einer iOS Objective-C-Bibliothek'
description: Dieser Artikel bietet eine praktische exemplarischen Vorgehensweise erstellen Sie eine Xamarin.iOS-Bindung für eine vorhandene Objective-C-Bibliothek, InfColorPicker. Sie erfahren, wie z. B. Kompilieren einer statischen Bibliothek für Objective-C, binden sie und Verwenden der Bindung in einer Xamarin.iOS-Anwendung.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 8285a82920f0d95a88855c5257535048c6de41d5
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854856"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Exemplarische Vorgehensweise: Binden einer iOS Objective-C-Bibliothek

_Dieser Artikel bietet eine praktische exemplarischen Vorgehensweise erstellen Sie eine Xamarin.iOS-Bindung für eine vorhandene Objective-C-Bibliothek, InfColorPicker. Sie erfahren, wie z. B. Kompilieren einer statischen Bibliothek für Objective-C, binden sie und Verwenden der Bindung in einer Xamarin.iOS-Anwendung._

Bei der Arbeit an iOS können Fällen auftreten, in dem Sie ein Objective-C-Bibliotheken von Drittanbietern nutzen möchten. In diesen Fällen können Sie eine Xamarin.iOS _Bindungsprojekt_ zum Erstellen einer [C#-Bindung](~/cross-platform/macios/binding/overview.md) , mit denen Sie die Bibliothek in Ihr Xamarin.iOS-Anwendungen zu nutzen.

Im Allgemeinen können Sie im iOS-Ökosystem Bibliotheken 3 Arten finden:

* Als vorkompilierte statische Bibliothek mit `.a` Erweiterung zusammen mit der einem oder mehreren Headern (h-Dateien). Z. B. [Googles Analytics-Bibliothek](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Als vorkompilierte Framework. Dies ist nur ein Ordner mit der statischen Bibliothek, Header und manchmal zusätzliche Ressourcen mit `.framework` Erweiterung. Z. B. [AdMob Googles-Bibliothek](https://developers.google.com/admob/ios/download).
* Einfach nur als Quellcodedateien. Z. B. eine Bibliothek, die nur `.m` und `.h` Objective-C-Dateien.

In der ersten und zweiten Szenario stehen bereits eine vorkompilierte CocoaTouch statische Bibliothek daher in diesem Artikel wir uns auf das dritte Szenario konzentrieren. Beachten Sie, bevor Sie beginnen, erstellen Sie eine Bindung, überprüfen Sie immer die Lizenz, die mit der Bibliothek, um sicherzustellen, dass Sie bindet, bindet es kostenlos bereitgestellt.

Dieser Artikel enthält eine schrittweise Anleitung zum Erstellen einer Bindung-Projekts mithilfe des open Source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C-Projekt als ein Beispiel, jedoch alle Informationen in diesem Handbuch mit einem angepasst werden kann Objective-C-Bibliotheken von Drittanbietern. Die InfColorPicker-Bibliothek bietet es sich um einen wieder verwendbaren View-Controller, der dem Benutzer ermöglicht, wählen Sie eine Farbe anhand der HSB-Darstellung, die Farbauswahl Benutzerfreundlicher zu machen.

[![](walkthrough-images/run01.png "Beispiel für die InfColorPicker-Bibliothek, die unter iOS")](walkthrough-images/run01.png#lightbox)

Ich werde die notwendigen Schritte zum Nutzen dieser bestimmten Objective-C-API in Xamarin.iOS:

- Erstellen Sie zunächst eine statische Objective-C-Bibliothek, die mithilfe von Xcode.
- Anschließend werden wir diese statische Bibliothek mit Xamarin.iOS binden.
- Als Nächstes zeigen, wie Objective Sharpie die Workload reduzieren können, mittels automatischer Generierung von einige (aber nicht alle) der erforderlichen API-Definitionen durch die Xamarin.iOS-Bindung erforderlich.
- Schließlich erstellen wir eine Xamarin.iOS-Anwendung, die die Bindung verwendet.

Die beispielanwendung wird mithilfe eines starkes Delegaten für die Kommunikation zwischen der InfColorPicker-API und unseren c#-Code veranschaulicht. Nachdem wir einen starken Delegaten mit gesehen haben, wird die Verwendung von schwachen Delegaten für die gleichen Aufgaben behandelt.

## <a name="requirements"></a>Anforderungen

In diesem Artikel wird davon ausgegangen, dass Sie eine gewisse Vertrautheit mit Xcode und Objective-C-Sprache und Sie gelesen haben unsere [Binden von Objective-C](~/cross-platform/macios/binding/index.md) Dokumentation. Darüber hinaus ist Folgendes erforderlich, um die aufgeführten Schritte ausführen:

-  **Xcode und iOS SDK** -Apple Xcode und die neueste iOS-API müssen installiert und konfiguriert werden, auf dem Computer des Entwicklers.
-  **[Xcode-Befehlszeilentools](#Installing_the_Xcode_Command_Line_Tools)**  -der Xcode-Befehlszeilentools für die derzeit installierte Version von Xcode (siehe unten für Installationsdetails) installiert werden.
-  **Visual Studio für Mac oder Visual Studio** – die neueste Version von Visual Studio für Mac oder Visual Studio installiert und konfiguriert werden, auf dem Entwicklungscomputer installiert werden soll. Eine Apple Mac ist erforderlich, für die Entwicklung einer Xamarin.iOS-Anwendung, und bei Verwendung von Visual Studio muss eine Verbindung zum [eine Xamarin.iOS-buildhost](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **Die neueste Version des Ziel-Sharpie** -eine aktuelle Kopie des Ziel-Sharpie Tools heruntergeladen [hier](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Wenn Sie bereits Ziel Sharpie installiert haben, können Sie es auf die neueste Version aktualisieren, mit der `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Installieren die Xcode-Command-Line-Tools

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


Wie bereits erwähnt, wir verwenden Xcode-Befehlszeilentools (insbesondere `make` und `lipo`) in dieser exemplarischen Vorgehensweise. Die `make` Befehl ist ein sehr häufig Unix-Dienstprogramm, das die Kompilierung von ausführbaren Programmen und Bibliotheken mit automatisieren, wird eine _Makefile_ , der angibt, wie das Programm erstellt werden soll. Die `lipo` Befehl eine OS X-Befehlszeilen-Hilfsprogramm zum Erstellen von Multi-Architektur von Dateien, wird es fassen Sie mehrere `.a` -Dateien in eine Datei, die von der alle Hardwarearchitekturen verwendet werden kann.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Wie bereits erwähnt, wir verwenden Xcode-Befehlszeilentools für die **Mac-Buildhost** (insbesondere `make` und `lipo`) in dieser exemplarischen Vorgehensweise. Die `make` Befehl ist ein sehr häufig Unix-Dienstprogramm, das die Kompilierung von ausführbaren Programmen und Bibliotheken mit automatisieren, wird eine _Makefile_ , gibt an, wie das Programm zu erstellen. Die `lipo` Befehl eine OS X-Befehlszeilen-Hilfsprogramm zum Erstellen von Multi-Architektur von Dateien, wird es fassen Sie mehrere `.a` -Dateien in eine Datei, die von der alle Hardwarearchitekturen verwendet werden kann.


-----

Gemäß den Apple [erstellen, die über die Befehlszeile mit Xcode – häufig gestellte Fragen von](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) Dokumentation in OS X 10.9 und höher, wird die **Downloads** Bereich von Xcode **Voreinstellungen** Dialogfeld nicht unterstützt mehr die Download-Befehlszeilentools.

Sie müssen eine der folgenden Methoden verwenden, um die Tools zu installieren:

- **Installieren von Xcode** – bei der Installation von Xcode, es wird zusammen mit allen Befehlszeilentools ausgeliefert. Shims in OS X 10.9 (installiert `/usr/bin`), können alle Tool im "zuordnen" `/usr/bin` an das entsprechende Tool in Xcode. Z. B. die `xcrun` Befehl, der Sie gefunden oder einem beliebigen Tool in Xcode über die Befehlszeile ausgeführt werden kann.
- **Die Terminal-Anwendung** -aus der Terminal-Anwendung, können Sie die Befehlszeilentools installieren, indem Sie mit der `xcode-select --install` Befehl:
    - Die Terminal-Anwendung zu starten.
    - Typ `xcode-select --install` , und drücken Sie **EINGABETASTE**, z.B.:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Werden Sie aufgefordert, die Befehlszeilentools installieren, klicken Sie auf die **installieren** Schaltfläche: [ ![ ] (walkthrough-images/xcode01.png "Installieren von Tools über die Befehlszeile")](walkthrough-images/xcode01.png#lightbox)

    - Die Tools heruntergeladen und installiert, die von Apple Servern: [ ![ ] (walkthrough-images/xcode02.png "Herunterladen von Tools")](walkthrough-images/xcode02.png#lightbox)

- **Downloads für Apple-Entwickler** -das Befehlszeilentools-Paket steht die [Downloads für Apple-Entwickler]() Webseite. Melden Sie sich mit Ihrer Apple-ID, und klicken Sie dann zu suchen und Laden Sie die Befehlszeilentools: [ ![ ] (walkthrough-images/xcode03.png "suchen die Befehlszeilentools")](walkthrough-images/xcode03.png#lightbox)

Mit der Befehlszeilentools installiert sind sind wir bereit, mit der exemplarischen Vorgehensweise fortzufahren.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise wird die folgenden Schritte behandelt:

- **[Erstellen Sie eine statische Bibliothek](#Creating_A_Static_Library)**  – dieser Schritt umfasst das Erstellen einer statischen Bibliothek aus der **InfColorPicker** Objective-C-Code. Die statische Bibliothek hat die `.a` Dateierweiterung und werden in der .NET-Assembly des Library-Projekts eingebettet werden.
- **[Erstellen Sie ein Projekt der Xamarin.iOS-Bindung](#Create_a_Xamarin.iOS_Binding_Project)**  – Sobald wir eine statische Bibliothek verfügen, verwenden wir es zum Erstellen eines Projekts der Xamarin.iOS-Bindung. Das bindungsprojekt besteht aus der statischen Bibliothek, die wir gerade erstellt haben und die Metadaten in Form von C#-Code, der erklärt, wie die Objective-C-API verwendet werden kann. Diese Metadaten wird häufig als die API-Definitionen bezeichnet. Wir verwenden **[Ziel Sharpie](#Using_Objective_Sharpie)** damit wir mit der API-Definitionen erstellen können.
- **[Die API-Definitionen zu normalisieren](#Normalize_the_API_Definitions)**  – Ziel Sharpie ist gut für uns dabei zu unterstützen, aber nicht alles. Besprochen werden einige Änderungen, die wir benötigen, um auf die API-Definitionen zu machen, bevor sie verwendet werden können.
- **[Verwenden Sie die Bindungsbibliothek](#Using_the_Binding)**  -schließlich erstellen wir eine Xamarin.iOS-Anwendung veranschaulichen, wie Sie das Projekt neu erstellte Bindung verwenden.

Nun, da wir wissen, welche Schritte erforderlich sind, betrachten wir nun den Rest der exemplarischen Vorgehensweise.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Erstellen einer statischen Bibliothek

Wenn wir uns den Code für InfColorPicker in Github ansehen:

[![](walkthrough-images/image02.png "Überprüfen Sie den Code für InfColorPicker in Github")](walkthrough-images/image02.png#lightbox)

Wir können die folgenden drei Verzeichnisse im Projekt sehen:

- **InfColorPicker** : Dieses Verzeichnis enthält die Objective-C-Code für das Projekt.
- **PickerSamplePad** : Dieses Verzeichnis enthält ein Beispielprojekt für das iPad.
- **PickerSamplePhone** : Dieses Verzeichnis enthält ein Beispielprojekt für das iPhone.

Wir laden Sie das Projekt InfColorPicker [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) und Entzippen Sie sie in das Verzeichnis der unsere auswählen. Öffnen Sie das Xcode-Ziel für `PickerSamplePhone` -Projekt sehen Sie die folgende Projektstruktur im Xcode Navigator:

[![](walkthrough-images/image03.png "Die Projektstruktur im Xcode-Navigator")](walkthrough-images/image03.png#lightbox)

Dieses Projekt erreicht die Wiederverwendung von Code durch das direkte Hinzufügen des InfColorPicker-Quellcodes (in Rot) in jedem Beispielprojekt. Der Code für das Beispielprojekt ist im blauen Rahmen. Da dieses Projekt keine uns mit einer statischen Bibliothek, ist es erforderlich, für uns erstellen Sie ein Xcode-Projekt, um die statische Bibliothek zu kompilieren.

Der erste Schritt besteht für uns den Quellcode InfoColorPicker der statischen Bibliothek hinzufügen. Lassen Sie uns hierzu gehen Sie folgendermaßen vor:

1. Starten Sie Xcode.
2. Von der **Datei** Menü die Option **neu** > **Projekt...** :

    [![](walkthrough-images/image04.png "Starten eines neuen Projekts")](walkthrough-images/image04.png#lightbox)
3. Wählen Sie **Framework und Bibliothek**, **Cocoa Touch statische Bibliothek** Vorlage, und klicken Sie auf die **Weiter** Schaltfläche:

    [![](walkthrough-images/image05.png "Wählen Sie die statische Bibliothek mit Cocoa Touch-Vorlage")](walkthrough-images/image05.png#lightbox)

4. Geben Sie `InfColorPicker` für die **Projektname** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](walkthrough-images/image06.png "Geben Sie den Namen des Projekts InfColorPicker")](walkthrough-images/image06.png#lightbox)
5. Wählen Sie einen Speicherort zum Speichern Sie das Projekt, und klicken Sie auf die **OK** Schaltfläche.
6. Nun müssen wir die Quelle aus dem Projekt InfColorPicker statische Bibliothek hinzufügen. Da die **InfColorPicker.h** Datei bereits in der statischen Bibliothek (standardmäßig), Xcode gestattet es uns nicht um ihn zu überschreiben. Von der **Finder**, navigieren Sie zu den InfColorPicker Quellcode im ursprünglichen Projekt, das wir von GitHub entpackt, alle InfColorPicker Dateien kopieren und fügen Sie sie in unserem neuen statischen Bibliothek-Projekt:

    [![](walkthrough-images/image12.png "Kopieren Sie alle Dateien InfColorPicker")](walkthrough-images/image12.png#lightbox)

7. Zurück zu Xcode, klicken Sie mit der rechten Maustaste auf die **InfColorPicker** Ordner, und wählen **Hinzufügen von Dateien zu "InfColorPicker..."**:

    [![](walkthrough-images/image08.png "Hinzufügen von Dateien")](walkthrough-images/image08.png#lightbox)

8. Das Dialogfeld "Dateien hinzufügen", navigieren Sie zu den InfColorPicker Quellcodedateien, die wir gerade kopiert haben, wählen sie alle aus, und klicken Sie auf die **hinzufügen** Schaltfläche:

    [![](walkthrough-images/image09.png "Wählen Sie alle aus, und klicken Sie auf die Schaltfläche \"hinzufügen\"")](walkthrough-images/image09.png#lightbox)

9. Der Quellcode wird in unserem Projekt kopiert werden:

    [![](walkthrough-images/image10.png "Der Quellcode wird in das Projekt kopiert werden")](walkthrough-images/image10.png#lightbox)

10. Wählen Sie aus der Xcode-Projektnavigator der **InfColorPicker.m** Datei, und kommentieren Sie die letzten beiden Zeilen (aufgrund der Art, die diese Bibliothek geschrieben wurde, wird diese Datei wird nicht verwendet wird):

    [![](walkthrough-images/image14.png "Bearbeiten der Datei InfColorPicker.m")](walkthrough-images/image14.png#lightbox)

11. Nun müssen wir überprüfen, ob alle Frameworks, die von der Bibliothek erforderlich. Sie finden diese Informationen entweder in der Infodatei oder durch Öffnen einer der Beispielprojekte bereitgestellt. Dieses Beispiel verwendet `Foundation.framework`, `UIKit.framework`, und `CoreGraphics.framework` wir hinzufügen.

12. Wählen Sie die **InfColorPicker Ziel > Build Phases** und erweitern Sie die **Binärdatei mit Verknüpfungsbibliotheken** Abschnitt:

    [![](walkthrough-images/image16b.png "Erweitern Sie den Abschnitt für die Binärdatei mit Bibliotheken verknüpfen")](walkthrough-images/image16b.png#lightbox)

13. Verwenden der **+** Schaltfläche Öffnen Sie das Dialogfeld, sodass Sie zum Hinzufügen der erforderlichen Frames-Frameworks, die oben aufgeführten:

    [![](walkthrough-images/image16c.png "Fügen Sie die erforderlichen Frames Frameworks oben aufgelisteten")](walkthrough-images/image16c.png#lightbox)

14. Die **Binärdatei mit Verknüpfungsbibliotheken** Abschnitt sollte jetzt wie folgt aussehen:

    [![](walkthrough-images/image16d.png "Im Abschnitt für die Binärdatei mit Bibliotheken verknüpfen")](walkthrough-images/image16d.png#lightbox)

An diesem Punkt sind wir schließen, aber noch nicht ganz fertig. Die statische Bibliothek erstellt wurde, aber wir müssen, erstellen Sie sie zum Erstellen einer Fat binary, alle erforderlichen-Architekturen für iOS-Gerät und iOS-Simulator umfasst.

### <a name="creating-a-fat-binary"></a>Erstellen einer Fat-Binärdatei

Alle iOS-Geräte verfügen über Prozessoren, die von ARM-Architektur unterstützt werden und, die im Laufe der Zeit entwickelt wurden. Jede neue Architektur werden neue Anweisungen und andere Verbesserungen bei gleichzeitiger Rückwärtskompatibilität Kompatibilität hinzugefügt. Auf iOS-Geräten haben armv6 "," armv7 "," armv7s "," arm64-Befehlssätze – Obwohl [verwenden wir nicht mehr armv6](~/ios/deploy-test/compiling-for-different-devices.md). IOS-Simulator basiert nicht auf ARM und statt diesen schweren ist, eine X86- und mit Strom versorgt x86_64-Simulator. Wir, die für uns bedeutet, dass ist, dass wir Bibliotheken bereitstellen müssen, für jede Anweisung legt diese fest.

Fat-Bibliothek ist `.a` Datei, die jede unterstützte Architektur enthält.

Erstellen eine Fat binary ist drei Schritten:

- Kompilieren Sie eine ARM-7 und ARM64-Version von der statischen Bibliothek an.
- Kompilieren einer X86- und x84_64-Version von der statischen Bibliothek an.
- Verwenden der `lipo` Befehlszeilentool zum Kombinieren der zwei statischen Bibliotheken.

Während dieser drei Schritte recht einfach sind, und es erforderlich sein kann, sie in der Zukunft wiederholen, wenn die Bibliothek für Objective-C-Updates erhält oder Fehlerbehebungen ist erforderlich. Wenn Sie diese Schritte automatisieren möchten, vereinfachen sie die künftige Wartung und die Unterstützung des iOS-Projekts Bindung.

Es gibt viele Tools zur Automatisierung solcher Aufgaben – ein Shellskript [Rake](http://rake.rubyforge.org/), [Xbuild](http://www.mono-project.com/docs/tools+libraries/tools/xbuild/), und [stellen](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Wenn wir die Xcode-Befehlszeilentools installiert haben, installiert es auch stellen, sodass das Buildsystem, die in dieser exemplarischen Vorgehensweise verwendet werden. Hier ist ein **Makefile** , mit denen Sie können eine freigegebene Bibliothek von Multi-Architektur zu erstellen, die auf einem iOS-Gerät und der Simulator für die alle Bibliotheken funktionieren:

```bash
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```

Geben Sie die **Makefile** Befehle im nur-Text-Editor Ihrer Wahl, und aktualisieren Sie die Abschnitte mit **YOUR-Projektname** mit dem Namen des Projekts. Es ist auch wichtig, um sicherzustellen, dass wir Sie fügen Sie die Anweisungen oben, dass die Registerkarten in den Anweisungen beibehalten wurden.

Speichern Sie die Datei mit dem Namen **Makefile** an demselben Speicherort wie die statische Bibliothek Xcode InfColorPicker wir oben erstellt haben:

[![](walkthrough-images/lib00.png "Speichern Sie die Datei mit dem Namen Makefile")](walkthrough-images/lib00.png#lightbox)

Öffnen Sie die Terminal-Anwendung auf Ihrem Mac, und navigieren Sie zum Speicherort Ihrer Makefiles. Typ `make` in das Terminal aus, drücken Sie die **EINGABETASTE** und **Makefile** ausgeführt wird:

[![](walkthrough-images/lib01.png "Makefile-Beispielausgabe")](walkthrough-images/lib01.png#lightbox)

Beim Ausführen von Stellen sehen Sie eine Menge Text Scrollen durch. Wenn alles ordnungsgemäß funktioniert, sehen Sie die Wörter **BUILDVORGANG war erfolgreich** und `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` und `libInfColorPickerSDK.a` Dateien kopiert werden, am gleichen Speicherort wie die **Makefile**:

[![](walkthrough-images/lib02.png "Mit dem Makefile generierten Dateien LibInfColorPicker-armv7.a, LibInfColorPicker-i386.a und libInfColorPickerSDK.a")](walkthrough-images/lib02.png#lightbox)

Sie können die Architekturen in Ihre Fat-Binärdatei mithilfe des folgenden Befehls überprüfen:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Dies sollte Folgendes angezeigt:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

Wir haben den ersten Schritt für die iOS-Bindung an diesem Punkt abgeschlossen, erstellen Sie eine statische Bibliothek mit Xcode und die Xcode-Befehlszeilentools `make` und `lipo`. Lassen Sie uns mit dem nächsten Schritt fortfahren, und verwenden Sie **Ziel-Sharpie** zum Automatisieren der Erstellung der API-Bindungen für uns.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Erstellen Sie eine Xamarin.iOS Projekt wird gebunden

Bevor wir verwenden können, **Ziel-Sharpie** des Bindungsvorgangs automatisieren möchten, müssen wir zum Erstellen eines Projekts des Xamarin.iOS-Bindung, die API-Definitionen aufnehmen soll (, die wir verwenden **Ziel-Sharpie** , uns zu helfen Build), und erstellen Sie die C#-Bindung für uns.

Lassen Sie uns wie folgt vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac.
1. Von der **Datei** , wählen Sie im Menü **neu** > **Projektmappe...** :

    ![](walkthrough-images/bind01.png "Starten einer neuen Projektmappe")

1. Wählen Sie im Dialogfeld "neue Projektmappe" **Bibliothek** > **iOS-Projekts Bindung**:

    ![](walkthrough-images/bind02.png "Wählen Sie die iOS-Projekts binden")

1. Klicken Sie auf die **Weiter** Schaltfläche.

1. Geben Sie "InfColorPickerBinding" als die **Projektname** , und klicken Sie auf die **erstellen** Schaltfläche, um die Projektmappe zu erstellen:

    ![](walkthrough-images/bind02a.png "Geben Sie den Namen des Projekts InfColorPickerBinding")

Die Lösung erstellt werden und werden zwei Standarddateien enthalten:

![](walkthrough-images/bind03.png "Die Projektmappenstruktur im Projektmappen-Explorer")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Starten Sie Visual Studio.

1. Von der **Datei** , wählen Sie im Menü **neu** > **Projekt...** :

    ![Starten eines neuen Projekts](walkthrough-images/bind01vs.png "Starten eines neuen Projekts")

1. Wählen Sie im Dialogfeld "Neues Projekt" **Visual c# > iPhone & iPad > iOS-Bindungsbibliothek (Xamarin)**:

    [![Wählen Sie die iOS-Bindungsbibliothek](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. Geben Sie "InfColorPickerBinding" als die **Namen** , und klicken Sie auf die **OK** Schaltfläche, um die Projektmappe zu erstellen.

Die Lösung erstellt werden und werden zwei Standarddateien enthalten:

![](walkthrough-images/bind03vs.png "Die Projektmappenstruktur im Projektmappen-Explorer")

-----

- **ApiDefinition.cs** – diese Datei enthält die Verträge, die definieren, wie Objective-C-APIs in C# -Code umschlossen wird.
- **Structs.cs** : Diese Datei enthält alle Strukturen oder-Enumerationswerte fest, die die Schnittstellen und Delegaten erforderlich sind.

Wir arbeiten mit diesen zwei Dateien weiter unten in dieser exemplarischen Vorgehensweise. Zunächst müssen wir die InfColorPicker-Bibliothek für das bindungsprojekt hinzuzufügen.

### <a name="including-the-static-library-in-the-binding-project"></a>Einschließen der statischen Bibliothek in der Bindungsprojekt

Nachdem wir unsere Bindung Basisprojekt bereit haben, müssen Sie die Fat Binary-Bibliothek hinzufügen wir oben, für erstellt die **InfColorPicker** Bibliothek.

Um die Bibliothek hinzuzufügen, gehen Sie wie folgt vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Mit der rechten Maustaste auf die **Native Verweise** Ordner in der Projektmappe, und wählen Sie **Native Verweise hinzufügen**:

    ![](walkthrough-images/bind04a.png "Native Verweise hinzufügen")

1. Navigieren Sie zu der Fat Binary wir vorher getroffen haben (`libInfColorPickerSDK.a`), und drücken Sie die **öffnen** Schaltfläche:

    ![](walkthrough-images/bind05.png "Wählen Sie die Datei libInfColorPickerSDK.a")
1. Die Datei wird im Projekt enthalten sein:

    ![](walkthrough-images/bind04.png "Eine Datei einschließen")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopieren der `libInfColorPickerSDK.a` aus Ihrem **Mac-Buildhost** und fügen Sie ihn in das bindungsprojekt.

1. Mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen > Vorhandenes Element...** :

    ![](walkthrough-images/bind04vs.png "Hinzufügen einer vorhandenen Datei")

1. Navigieren Sie zu der `libInfColorPickerSDK.a` , und drücken Sie die **hinzufügen** Schaltfläche:

    ![](walkthrough-images/bind05vs.png "Hinzufügen von libInfColorPickerSDK.a")

1. Die Datei wird im Projekt enthalten sein.

-----

Wenn die **.a** Datei wird dem Projekt hinzugefügt, Xamarin.iOS wird automatisch festgelegt. die **Buildvorgang** der Datei, die **ObjcBindingNativeLibrary**, und erstellen Sie eine spezielle Datei wird aufgerufen, `libInfColorPickerSDK.linkwith.cs`.


Diese Datei enthält die `LinkWith` Attribut, Xamarin.iOS mitteilt, wie Handle der statischen Bibliothek, die wir soeben hinzugefügt. Der Inhalt dieser Datei werden in den folgenden Codeausschnitt dargestellt:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

Die `LinkWith` Attribut identifiziert die statische Bibliothek für das Projekt und einige wichtige Linker-Flags.


Im nächsten Schritt erforderlich ist, wird die API-Definitionen für das Projekt InfColorPicker erstellt. Für die Zwecke dieser exemplarischen Vorgehensweise verwenden wir Ziel Sharpie zum Generieren der Datei **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Verwenden von objektive Sharpie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


Objektive Sharpie ist eine Kommandozeile Tool (bereitgestellt von Xamarin), die beim Erstellen der Definitionen, die zum Binden einer 3rd Party Objective-C-Bibliothek in c# erforderliche unterstützen. In diesem Abschnitt verwenden wir Ziel Sharpie zum Erstellen der anfänglichen **ApiDefinition.cs** für das Projekt InfColorPicker.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Objektive Sharpie ist eine Kommandozeile Tool (bereitgestellt von Xamarin), die beim Erstellen der Definitionen, die zum Binden einer 3rd Party Objective-C-Bibliothek in c# erforderliche unterstützen. In diesem Abschnitt verwenden wir Ziel Sharpie auf unsere **Mac-Buildhost** zum Erstellen der anfänglichen **ApiDefinition.cs** für das Projekt InfColorPicker.


-----

Laden Sie zum Einstieg wir Ziel Sharpie-Installationsdatei wie in diesem [Handbuch](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Führen Sie das Installationsprogramm aus, und befolgen Sie alle eingabeaufforderungen auf dem Bildschirm aus der Installations-Assistent zum Ziel Sharpie auf unsere Entwicklungscomputer zu installieren.

Nach wir erfolgreich Ziel Sharpie haben installiert haben, lassen Sie uns die Terminal-app starten und geben Sie den folgenden Befehl auf alle Tools, Hilfe, die es bereitstellt, um die Bindung zu unterstützen:

```bash
sharpie -help
```

Wenn wir den oben angegebenen Befehl ausführen, wird die folgende Ausgabe generiert:

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:

  xcode    Get information about Xcode installations and available SDKs.

  bind     Create a Xamarin C# binding to Objective-C APIs
Europa:Resources kmullins$
```

In dieser exemplarischen Vorgehensweise wird die folgende Ziel Sharpie-Tools verwenden:

- **Xcode** : dieses Tools bietet uns die Informationen zu unseren aktuellen Xcode-Installation und die Versionen von iOS und Mac-APIs, die es installiert haben. Wir werden diese Informationen später verwenden werden, wenn wir unsere Bindungen generieren.
- **Binden Sie** -verwenden wir dieses Tool analysiert die **h** Dateien im Projekt InfColorPicker in die Ausgangs- **ApiDefinition.cs** und **StructsAndEnums.cs** Dateien.

Geben Sie den Namen des Tools, um Hilfe für ein bestimmtes Ziel Sharpie-Tool zu erhalten, und die `-help` Option. Z. B. `sharpie xcode -help` gibt die folgende Ausgabe zurück:

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, --help                 Show detailed help
  -v, --verbose              Be verbose with output
      --sdks                 List all available Xcode SDKs. Pass -verbose for
                               more details.
Europa:Resources kmullins$
```

Bevor wir den Bindungsprozess beginnen können, müssen wir Informationen über unsere aktuellen installierten SDKs erhalten Sie mithilfe des folgenden Befehls in das Terminal `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

In den oben genannten wir sehen, dass wir haben die `iphoneos9.3` SDK auf dem Computer installiert. Mit diesen Informationen vorhanden, wir können das Projekt InfColorPicker analysieren `.h` -Dateien in den ersten **ApiDefinition.cs** und `StructsAndEnums.cs` für das Projekt InfColorPicker.

Geben Sie den folgenden Befehl in der die Terminal-app:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

In denen `[full-path-to-project]` ist der vollständige Pfad zu dem Verzeichnis, in denen die **InfColorPicker** Xcode-Projektdatei auf unseren Computer befindet, und [Iphone-Betriebssystem] ist das iOS-SDK, die es installiert haben, siehe die `sharpie xcode -sdks` Befehl. Beachten Sie, dass in diesem Beispiel wir übergeben haben  **\*h** als Parameter verwendet, darunter *alle* die Header-Dateien in diesem Verzeichnis – normalerweise Sie sollten dies nicht, aber stattdessen sorgfältig lesen der Headerdateien der obersten Ebene gefunden **h** Datei verweist auf alle anderen relevanten Dateien, und übergeben Sie einfach, die zum Ziel Sharpie.

Die folgenden [Ausgabe](walkthrough-images/os05.png) im Terminal generiert werden:

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

Und die **InfColorPicker.enums.cs** und **InfColorPicker.cs** Dateien in unser Verzeichnis erstellt werden:

[![](walkthrough-images/os06.png "Die InfColorPicker.enums.cs und InfColorPicker.cs-Dateien")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


Öffnen Sie beide Dateien an, in der Bindung-Projekt, dem wir zuvor erstellt haben. Kopieren Sie den Inhalt von der **InfColorPicker.cs** Datei, und fügen Sie ihn in die **ApiDefinition.cs** Ersetzen der vorhandenen Datei `namespace ...` Codeblock durch den Inhalt der der  **InfColorPicker.cs** Datei (verlassen die `using` Anweisungen intakt):

![](walkthrough-images/os07.png "Die Datei InfColorPickerControllerDelegate")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Öffnen Sie beide Dateien an, in der Bindung-Projekt, dem wir zuvor erstellt haben. Kopieren Sie den Inhalt von der **InfColorPicker.cs** Datei (aus der **Mac-Buildhost**), und fügen Sie ihn in die **ApiDefinition.cs** Ersetzen der vorhandenen Datei `namespace ...` Codeblock, mit dem Inhalt der **InfColorPicker.cs** Datei (verlassen die `using` Anweisungen intakt).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>Die API-Definitionen zu normalisieren

Objektive Sharpie manchmal hat ein Problem übersetzen `Delegates`, sodass die Definition der ändern müssen die `InfColorPickerControllerDelegate` Schnittstelle, und Ersetzen Sie die `[Protocol, Model]` Zeile mit den folgenden:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Damit die Definition sieht wie folgt vor:

[![](walkthrough-images/os11.png "Die definition")](walkthrough-images/os11.png#lightbox)

Als Nächstes nehmen wir das gleiche mit dem Inhalt der der `InfColorPicker.enums.cs` Datei, kopieren und Einfügen in die `StructsAndEnums.cs` Datei verlassen die `using` intakt Anweisungen:

[![](walkthrough-images/os09.png "Der Inhalt der StructsAndEnums.cs Datei ")](walkthrough-images/os09.png#lightbox)

Finden Sie auch, dass das Ziel Sharpie die Bindung mit dem mit Anmerkungen versehen wurde `[Verify]` Attribute. Diese Attribute geben, Sie sicherstellen sollten, dass das Ziel Sharpie hat die richtige durch Vergleichen der Bindung mit der ursprünglichen C/Objective-C-Deklaration (die in einem Kommentar über der gebundenen Deklaration bereitgestellt wird). Nachdem Sie die Bindungen überprüft haben, sollten Sie das Attribut überprüfen entfernen. Weitere Informationen finden Sie in der [überprüfen](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) Guide.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


An diesem Punkt sollte das bindungsprojekt abgeschlossen und kann zum Erstellen. Wir erstellen das bindungsprojekt aus, und stellen Sie sicher, dass wir keine Fehler hat geführt:

[Erstellen Sie das bindungsprojekt, und stellen Sie sicher, dass keine Fehler vorliegen](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


An diesem Punkt sollte das bindungsprojekt abgeschlossen und kann zum Erstellen. Wir erstellen das bindungsprojekt aus, und stellen Sie sicher, dass wir keine Fehler hat geführt.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Unter Verwendung der Bindung

Um das Erstellen einer iPhone-beispielanwendung mit iOS oben erstellten Bibliothek binden, gehen Sie wie folgt vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. **Erstellen von Xamarin.iOS-Projekt** -fügen Sie ein neues Xamarin.iOS-Projekt namens **InfColorPickerSample** der Projektmappe, wie in den folgenden Screenshots gezeigt:

    ![](walkthrough-images/use01.png "Eine Einzelansicht-App hinzufügen")

    ![](walkthrough-images/use01a.png "Den Bezeichner festlegen")

1. **Hinzufügen der Verweis auf das Projekt Bindung** -Update die **InfColorPickerSample** Projekt so, dass sie einen Verweis auf die **InfColorPickerBinding** Projekt:

    ![](walkthrough-images/use02.png "Verweis auf das Bindungsprojekt hinzufügen")

1. **Erstellen Sie die iPhone-Benutzeroberfläche** -einen Doppelklick auf die **MainStoryboard.storyboard** Datei die **InfColorPickerSample** Projekt in der iOS-Designer bearbeiten. Hinzufügen einer **Schaltfläche** an die Ansicht und nennen Sie sie `ChangeColorButton`, wie im folgenden gezeigt:

    ![](walkthrough-images/use03.png "Hinzufügen einer Schaltfläche an die Ansicht")

1. **Hinzufügen der InfColorPickerView.xib** -die InfColorPicker Objective-C-Bibliothek enthält eine **XIB** Datei. Xamarin.iOS enthält keine dies **XIB** Bindung im Projekt verursacht Laufzeitfehler in unserer beispielanwendung. Dieses Problem zu umgehen ist zum Hinzufügen der **XIB** Datei, die unsere Xamarin.iOS-Projekt. Wählen Sie die Xamarin.iOS-Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen > Dateien hinzufügen**, und fügen die **XIB** Datei wie im folgenden Screenshot gezeigt:

    ![](walkthrough-images/use04.png "Fügen Sie der InfColorPickerView.xib hinzu.")

1. Wenn Sie aufgefordert, kopieren Sie die **XIB** -Datei in das Projekt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **Erstellen von Xamarin.iOS-Projekt** -Hinzufügen eines neuen Xamarin.iOS-Projekts mit dem Namen **InfColorPickerSample** mithilfe der **Einzelansicht-App** Vorlage:

    [![iOS-App (Xamarin)-Projekt](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![Vorlage auswählen](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **Hinzufügen der Verweis auf das Projekt Bindung** -Update die **InfColorPickerSample** Projekt so, dass sie einen Verweis auf die **InfColorPickerBinding** Projekt:

    ![](walkthrough-images/use02vs.png "Verweis auf das Bindungsprojekt hinzufügen")

1. **Erstellen Sie die iPhone-Benutzeroberfläche** -einen Doppelklick auf die **MainStoryboard.storyboard** Datei die **InfColorPickerSample** Projekt in der iOS-Designer bearbeiten. Hinzufügen einer **Schaltfläche** an die Ansicht und nennen Sie sie `ChangeColorButton`, wie im folgenden gezeigt:

    ![](walkthrough-images/use03vs.png "Erstellen Sie die iPhone-Benutzeroberfläche")

1. **Hinzufügen der InfColorPickerView.xib** -die InfColorPicker Objective-C-Bibliothek enthält eine **XIB** Datei. Xamarin.iOS enthält keine dies **XIB** Bindung im Projekt verursacht Laufzeitfehler in unserer beispielanwendung. Dieses Problem zu umgehen, besteht darin, hinzuzufügen der **XIB** Datei, die unsere Xamarin.iOS-Projekt aus unserer **Mac-Buildhost**. Wählen Sie die Xamarin.iOS-Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen** > **vorhandenes Element...** , und fügen die **XIB** Datei.

-----

Als Nächstes werfen wir einen kurzen Blick auf die Protokolle in Objective-C und wie wir sie in der Bindung und c#-Code behandeln.

### <a name="protocols-and-xamarinios"></a>Protokolle und Xamarin.iOS

In Objective-C ein Protokoll-Methoden (oder Nachrichten) definiert, die unter bestimmten Umständen verwendet werden kann. Vom Konzept her sind diese Schnittstellen in c# sehr ähnlich. Ein Hauptunterschied zwischen einer Objective-C-Protokoll und eine C#-Schnittstelle ist, dass Protokolle optionale Methoden - Methoden, die eine Klasse nicht haben können implementiert. Objective-C verwendet die @optional Schlüsselwort verwendet, um anzugeben, welche Methoden optional sind. Weitere Informationen zu Protokollen finden Sie unter [Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** wurde von einem solchen Protokoll, in den folgenden Codeausschnitt gezeigt:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Dieses Protokoll wird verwendet, indem **InfColorPickerController** um Clients zu informieren, dass der Benutzer, eine neue Farbe und dass ausgewählt hat die **InfColorPickerController** abgeschlossen ist. Objektive Sharpie dieses Protokoll zugeordnet, wie im folgenden Codeausschnitt gezeigt:

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

Wenn die bindungsbibliothek kompiliert wird, erstellt Xamarin.iOS eine abstrakte Basisklasse namens `InfColorPickerControllerDelegate`, der diese Schnittstelle mit virtuellen Methoden implementiert.

Es gibt zwei Möglichkeiten, dass wir diese Schnittstelle in einer Xamarin.iOS-Anwendung implementieren können:

- **Delegieren starke** -mit einem starken Delegaten umfasst das Erstellen einer C#-Klasse, Unterklassen `InfColorPickerControllerDelegate` und die entsprechenden Methoden überschreibt. **InfColorPickerController** für die Kommunikation mit den Clients eine Instanz dieser Klasse verwenden.
- **Schwache Delegaten** -ein schwacher Delegat ist ein etwas anderes Verfahren, das umfasst das Erstellen einer öffentlichen Methode für eine Klasse (z. B. `InfColorPickerSampleViewController`) und klicken Sie dann diese Methode, um die `InfColorPickerDelegate` Protokoll über eine `Export` Attribut.

Starke Delegaten bieten Intellisense, typsicherheit und eine bessere Kapselung. Aus diesen Gründen sollten Sie starke Delegaten verwenden, können Sie anstatt eines schwachen Delegaten.

In dieser exemplarischen Vorgehensweise wird erörtert, beide Verfahren: Implementieren zuerst einen starke Delegaten, und klicken Sie dann erläutert, wie Sie einen schwachen Delegaten implementieren.

### <a name="implementing-a-strong-delegate"></a>Implementieren ein sicheres Delegat

Fertig stellen die Xamarin.iOS-Anwendung mit einem starken Delegaten auf reagieren die `colorPickerControllerDidFinish:` Nachricht:

**Unterklasse InfColorPickerControllerDelegate** -fügen Sie eine neue Klasse mit dem Namen `ColorSelectedDelegate`. Bearbeiten Sie die Klasse, sodass sie den folgenden Code:

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
    public class ColorSelectedDelegate:InfColorPickerControllerDelegate
    {
        readonly UIViewController parent;

        public ColorSelectedDelegate (UIViewController parent)
        {
            this.parent = parent;
        }

        public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
        {
            parent.View.BackgroundColor = controller.ResultColor;
            parent.DismissViewController (false, null);
        }
    }
}
```

Xamarin.iOS verbindet den Objective-C-Delegaten durch das Erstellen einer abstrakten Klasse namens `InfColorPickerControllerDelegate`. Unterklasse diesen Typ und die außer Kraft setzen der `ColorPickerControllerDidFinish` Methode, um den Wert der Zugriff auf die `ResultColor` Eigenschaft `InfColorPickerController`.

**Erstellen Sie eine Instanz des ColorSelectedDelegate** -der Ereignishandler benötigen eine Instanz von der `ColorSelectedDelegate` Typ, den wir im vorherigen Schritt erstellt haben. Bearbeiten Sie die Klasse `InfColorPickerSampleViewController` und fügen Sie die folgende Instanzvariable der Klasse:

```csharp
ColorSelectedDelegate selector;
```

**Initialisieren Sie die Variable ColorSelectedDelegate** –, um sicherzustellen, dass `selector` ist eine gültige Instanz ist, aktualisieren Sie die Methode `ViewDidLoad` in `ViewController` entsprechend den folgenden Codeausschnitt:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**Implementieren Sie die Methode HandleTouchUpInsideWithStrongDelegate** – als Nächstes implementieren Sie den Ereignishandler für, wenn der Benutzer berührt **ColorChangeButton**. Bearbeiten Sie `ViewController`, und fügen Sie die folgende Methode hinzu:

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

Wir rufen Sie zunächst eine Instanz von `InfColorPickerController` über eine statische Methode, und stellen, die unsere starken Delegat über die Eigenschaft über Instanz `InfColorPickerController.Delegate`. Diese Eigenschaft wurde automatisch für uns vom Ziel Sharpie generiert. Schließlich rufen wir `PresentModallyOverViewController` zum Anzeigen der Ansicht `InfColorPickerSampleViewController.xib` , damit der Benutzer eine Farbe auswählen kann.

**Führen Sie die Anwendung** – an diesem Punkt sind wir mit unserem Code fertig. Wenn Sie die Anwendung ausführen, Sie sollten möglicherweise so ändern Sie die Farbe des Hintergrunds der `InfColorColorPickerSampleView` wie in den folgenden Screenshots gezeigt:

[![](walkthrough-images/run01.png "Ausführen der Anwendung")](walkthrough-images/run01.png#lightbox)

Herzlichen Glückwunsch! An diesem Punkt haben Sie erfolgreich erstellt und eine Objective-C-Bibliothek für die Verwendung in einer Xamarin.iOS-Anwendung gebunden wird. Als Nächstes betrachten wir die Verwendung von schwacher Delegaten.

### <a name="implementing-a-weak-delegate"></a>Implementieren einen schwachen Delegaten

Anstelle von Unterklassen aus einer Klasse, die an die Objective-C-Protokoll für Delegaten gebunden, Xamarin.iOS können Sie auch die Protokollmethoden in einer beliebigen Klasse zu implementieren, die abgeleitet `NSObject`, versehen Sie Ihre Methoden mit dem `ExportAttribute`, und klicken Sie dann liefern die entsprechenden Selektoren ein. Wenn Sie diesen Ansatz verwenden, weisen Sie eine Instanz dieser Klasse die `WeakDelegate` Eigenschaft anstatt auf die `Delegate` Eigenschaft. Ein schwacher Delegat bietet Ihnen die Flexibilität, Ihre Delegatklasse Sie einer anderen Vererbungshierarchie zu nutzen. Sehen Sie das Implementieren und Verwenden von schwachen Delegaten in der Xamarin.iOS-Anwendung an.

**Erstellen Sie Ereignishandler für TouchUpInside** -erstellen wir einen neuen Ereignishandler für die `TouchUpInside` -Ereignis der Schaltfläche Hintergrundfarbe ändern. Dieser Handler wird die gleiche Funktion wie das Ausfüllen der `HandleTouchUpInsideWithStrongDelegate` Handler, dass wir im vorherigen Abschnitt erstellt haben, aber einen schwachen Delegaten anstatt eines starken Delegaten verwenden. Bearbeiten Sie die Klasse `ViewController`, und fügen Sie die folgende Methode hinzu:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Aktualisieren von ViewDidLoad** -ändern wir `ViewDidLoad` so, dass sie den Ereignishandler verwendet, die wir gerade erstellt haben. Bearbeiten Sie `ViewController` , und ändern Sie `ViewDidLoad` , die im folgenden Codeausschnitt ähnelt:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Behandeln der ColorPickerControllerDidFinish: Nachricht** – im Falle der `ViewController` ist abgeschlossen ist, iOS sendet die Nachricht `colorPickerControllerDidFinish:` auf die `WeakDelegate`. Wir müssen zum Erstellen einer c#-Methode, die diese Nachricht verarbeiten kann. Zu diesem Zweck erstellen wir eine C#-Methode und verzieren klicken Sie dann mit der `ExportAttribute`. Bearbeiten Sie `ViewController`, und fügen Sie der Klasse die folgende Methode hinzu:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Führen Sie die Anwendung aus. Sie sollten jetzt Verhalten sich genau wie gewohnt, aber es eines schwaches Delegaten anstelle der starken Delegat mithilfe wird. An diesem Punkt haben Sie diese exemplarische Vorgehensweise erfolgreich abgeschlossen. Sie verfügen nun einen Überblick über das Erstellen und nutzen eine Xamarin.iOS-Bindung-Projekt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie der Prozess zum Erstellen und verwenden eine Xamarin.iOS-Bindung-Projekt. Zunächst erläutert wie Sie eine vorhandene Objective-C-Bibliothek in einer statischen Bibliothek zu kompilieren. Klicken Sie dann behandelt es zum Erstellen eines Projekts der Xamarin.iOS-Bindung, und das Ziel Sharpie verwenden, um die API-Definitionen für die Objective-C-Bibliothek zu generieren. Erläutert das Aktualisieren und optimieren die generierte API-Definitionen, um sie für die öffentliche Nutzung geeignet machen. Nachdem die Xamarin.iOS-Bindung des Projekts abgeschlossen wurde, versuchten wir uns mit der Nutzung von dieser Bindung in einer Xamarin.iOS-Anwendung mit dem Schwerpunkt auf starken Delegaten mit schwachen Delegaten auf.

## <a name="related-links"></a>Verwandte Links

- [Beispiel für blobbindung (Beispiel)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Informationen zu](~/cross-platform/macios/binding/overview.md)
- [Bindung Typen – Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin für Objective-C-Entwickler](~/ios/get-started/objective-c-developers/index.md)
- [Frameworkentwurfsrichtlinien](http://msdn.microsoft.com/library/ms229042.aspx)
- [Xamarin University-Kurs: Erstellen einer Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen einer Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
