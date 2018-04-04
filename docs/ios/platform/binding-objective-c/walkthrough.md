---
title: 'Exemplarische Vorgehensweise: Binden einer iOS-Bibliothek für Objective-C'
description: Dieser Artikel bietet eine praktische Exemplarische Vorgehensweise zur Erstellung einer Xamarin.iOS Bindung für eine vorhandene Objective-C-Bibliothek InfColorPicker. Es umfasst Themen wie z. B. Kompilieren einer statischen Bibliothek für Objective-C gebunden wird, und Verwenden der Bindung in einem Xamarin.iOS-Anwendung.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7a25aa1043dcaf52406059d3fa184da36dc4875e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Exemplarische Vorgehensweise: Binden einer iOS-Bibliothek für Objective-C

_Dieser Artikel bietet eine praktische Exemplarische Vorgehensweise zur Erstellung einer Xamarin.iOS Bindung für eine vorhandene Objective-C-Bibliothek InfColorPicker. Es umfasst Themen wie z. B. Kompilieren einer statischen Bibliothek für Objective-C gebunden wird, und Verwenden der Bindung in einem Xamarin.iOS-Anwendung._

Wenn Sie auf iOS zu arbeiten, können Fällen auftreten, in dem Sie eine Drittanbieter-Objective-C-Bibliothek nutzen möchten. In solchen Situationen können Sie eine Xamarin.iOS _Bindungsprojekt_ zum Erstellen einer [C#-Bindung](~/cross-platform/macios/binding/overview.md) wird, mit denen Sie die Bibliothek in Ihren Anwendungen Xamarin.iOS verwendet.

Im Allgemeinen können Sie die iOS-Ökosystem Bibliotheken in 3 Arten finden:

* Als vorkompilierte statische Bibliothek mit `.a` Erweiterung zusammen mit einem oder mehreren seiner Headern (h-Dateien). Beispielsweise [Google Analytics-Bibliothek](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Als vorkompilierte Framework. Dies ist nur ein Ordner mit der statischen Bibliothek, Header und manchmal zusätzliche Ressourcen mit `.framework` Erweiterung. Beispielsweise [Googles AdMob Bibliothek](https://developers.google.com/admob/ios/download).
* Kurz danach Quellcodedateien. Angenommen, eine Bibliothek, die nur `.m` und `.h` Objective-C-Dateien.

In der ersten und zweiten Szenario werden bereits eine vorkompilierte CocoaTouch statische Bibliothek, damit in diesem Artikel wir uns auf das dritte Szenario konzentrieren werden. Beachten Sie, bevor Sie beginnen, erstellen Sie eine Bindung, überprüfen Sie immer die Lizenz bereitgestellt, mit der Bibliothek, damit Sie frei, um es gebunden sind.

Dieser Artikel enthält eine schrittweise Anleitung zum Erstellen einer Bindung-Projekts mithilfe der open Source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C-Projekt als ein Beispiel, jedoch alle Informationen in diesem Handbuch mit allen für die Verwendung angepasst werden kann Objective-C-Bibliothek eines Drittanbieters. Die InfColorPicker-Bibliothek stellt eine wiederverwendbare modellansichtcontroller, die dem Benutzer ermöglicht, wählen Sie eine Farbe anhand der HSB-Darstellung Farbauswahl Benutzerfreundlicher machen.

[![](walkthrough-images/run01.png "Beispiel für die InfColorPicker-Bibliothek, die unter iOS")](walkthrough-images/run01.png#lightbox)

Alle notwendigen Schritte zum Nutzen dieser bestimmten Objective-C-API in Xamarin.iOS beschrieben:

- Erstellen Sie zunächst eine statische Objective-C-Bibliothek, die mithilfe von Xcode.
- Binden Sie dann diese statische Bibliothek mit Xamarin.iOS.
- Als Nächstes zeigen wie Objective Sharpie Arbeitslast mittels automatischer Generierung von einige (aber nicht alle) verringern der erforderlichen API-Definitionen, die von der Bindung Xamarin.iOS erforderlich.
- Schließlich erstellen wir eine Xamarin.iOS-Anwendung, die die Bindung verwendet.

Die beispielanwendung wird mithilfe eines starkes Delegaten für die Kommunikation zwischen der InfColorPicker-API und C#-Code veranschaulicht. Nachdem wir einen starken Delegaten mit gesehen haben, müssen wir von schwachen Delegaten verwenden, um die Aufgaben behandelt.

## <a name="requirements"></a>Anforderungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Xcode und Objective-C-Sprache vertraut sein, und Sie gelesen haben unsere [Objective-C binden](~/cross-platform/macios/binding/index.md) Dokumentation. Darüber hinaus ist Folgendes erforderlich, um die Schritte dargestellt:

-  **Xcode und iOS SDK** -Apple Xcode und die neueste iOS-API installiert und auf dem Computer des Entwicklers konfiguriert werden müssen.
-  **[Xcode-Befehlszeilentools](#Installing_the_Xcode_Command_Line_Tools)**  -die Xcode-Befehlszeilentools für die derzeit installierte Version von Xcode (siehe unten für Installationsdetails) installiert werden muss.
-  **Visual Studio für Mac oder Visual Studio** -die neueste Version von Visual Studio für Mac oder Visual Studio installiert und auf dem Entwicklungscomputer konfiguriert werden. Eine Apple-Mac ist für die Entwicklung einer Anwendung Xamarin.iOS und bei Verwendung von Visual Studio müssen Sie mit verbunden sein [eine Xamarin.iOS Host erstellen.](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **Die neueste Version des Ziel-Sharpie** -eine aktuelle Kopie des Ziels Sharpie Tools heruntergeladen [hier](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Wenn Sie bereits Ziel Sharpie installiert haben, können Sie es auf die neueste Version aktualisieren, mit der `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Installieren den Befehl Xcode-Befehlszeilentools

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


Wie bereits erwähnt, wir müssen werden mithilfe von Xcode-Befehlszeilentools (insbesondere `make` und `lipo`) in dieser exemplarischen Vorgehensweise. Die `make` Befehl ist ein sehr gängiges Unix-Hilfsprogramm, die die Kompilierung von ausführbaren Programmen und Bibliotheken Automatisierung mithilfe von wird eine _Makefile_ , der angibt, wie das Programm erstellt werden soll. Die `lipo` Befehl wird eine OS X-Befehlszeilen-Hilfsprogramm zum Erstellen von Dateien mit mehreren Architektur; es werden mehrere kombiniert `.a` Dateien in eine Datei, die durch alle Hardware-Architekturen verwendet werden kann.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Wie oben erwähnt, wir müssen werden mithilfe von Xcode-Befehlszeilentools für die **Mac-Buildhost** (insbesondere `make` und `lipo`) in dieser exemplarischen Vorgehensweise. Die `make` Befehl ist ein sehr gängiges Unix-Hilfsprogramm, die die Kompilierung von ausführbaren Programmen und Bibliotheken Automatisierung mithilfe von wird eine _Makefile_ , gibt an, wie das Programm erstellen. Die `lipo` Befehl wird eine OS X-Befehlszeilen-Hilfsprogramm zum Erstellen von Dateien mit mehreren Architektur; es werden mehrere kombiniert `.a` Dateien in eine Datei, die durch alle Hardware-Architekturen verwendet werden kann.


-----

Gemäß den Apple [erstellen über die Befehlszeile mit Xcode FAQ](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) Dokumentation, in der OS X 10.9 und höher, wird die **Downloads** Bereich von Xcode **Voreinstellungen** Dialogfeld nicht unterstützt mehr herunterladen Befehlszeilentools.

Sie müssen eine der folgenden Methoden verwenden, um die Tools zu installieren:

- **Installieren Sie Xcode** – bei der Installation von Xcode, er wird zusammen mit allen Befehlszeilentools ausgeliefert. In OS X 10.9 Shims (im installiert `/usr/bin`), können jedes Tool in enthaltenen zuordnen `/usr/bin` an das entsprechende Tool in Xcode. Z. B. die `xcrun` Befehl, dem Sie gefunden oder jedem anderen Tool innerhalb von Xcode über die Befehlszeile ausgeführt werden kann.
- **Die Terminal-Anwendung** -der Terminal-Anwendung, können Sie die Befehlszeilentools installieren, durch Ausführen der `xcode-select --install` Befehl:
    - Starten Sie die Terminal-Anwendung.
    - Typ `xcode-select --install` , und drücken Sie **EINGABETASTE**, beispielsweise:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Werden Sie aufgefordert, die-Befehlszeilentools zu installieren, klicken Sie auf die **installieren** Schaltfläche: [ ![ ] (walkthrough-images/xcode01.png "Installieren der Tools über die Befehlszeile")](walkthrough-images/xcode01.png#lightbox)

    - Die Tools heruntergeladen und über das Apple Server installiert: [ ![ ] (walkthrough-images/xcode02.png "die Tools herunterladen")](walkthrough-images/xcode02.png#lightbox)

- **Downloads für Entwickler von Apple** -Paket die Befehlszeilentools zur Verfügung steht die [Downloads für Entwickler von Apple]() Webseite. Melden Sie sich mit Ihrer Apple-ID, und klicken Sie dann zu suchen und Herunterladen der Befehlszeilentools zur Verfügung: [ ![ ] (walkthrough-images/xcode03.png "suchen die-Befehlszeilentools")](walkthrough-images/xcode03.png#lightbox)

Mit den Befehlszeilentools installiert sind wir auf mit der exemplarischen Vorgehensweise fortfahren.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise wird beschrieben, die folgenden Schritte aus:

- **[Erstellen Sie eine statische Bibliothek](#Creating_A_Static_Library)**  – dieser Schritt umfasst das Erstellen einer statischen Bibliothek von der **InfColorPicker** Objective-C-Code. Die statische Bibliothek müssen die `.a` Dateierweiterung und werden in der .NET-Assembly des Library-Projekts eingebettet werden.
- **[Erstellen Sie ein Projekt der Xamarin.iOS Bindung](#Create_a_Xamarin.iOS_Binding_Project)**  -Sobald wir eine statische Bibliothek verfügen, wird verwendet, um ein Xamarin.iOS Bindung-Projekt erstellen. Das bindungsprojekt besteht aus der statischen Bibliothek gerade erstellten und Metadaten in Form von C#-Code, der erklärt, wie die Objective-C-API verwendet werden kann. Diese Metadaten wird häufig als die API-Definitionen bezeichnet. Wir verwenden **[Ziel Sharpie](#Using_Objective_Sharpie)** helfen Sie uns mit der API-Definitionen zu erstellen.
- **[Die API-Definitionen zu normalisieren,](#Normalize_the_API_Definitions)**  – Ziel Sharpie eignet sich gut unterstützen Sie uns, aber nicht alle Funktionen. Einige Änderungen erläutert, die wir benötigen, um auf die API-Definitionen zu machen, bevor sie verwendet werden können.
- **[Verwenden Sie die Bindung Library](#Using_the_Binding)**  -wir erstellen schließlich eine Xamarin.iOS-Anwendung, um mit unserer neu erstellte bindungsprojekt anzuzeigen.

Nun, dass wir besser verstehen, welche Schritte erforderlich sind, fahren wir mit dem Rest der exemplarischen Vorgehensweise.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Erstellen eine statische Bibliothek

Wenn wir den Code für InfColorPicker in Github überprüfen:

[![](walkthrough-images/image02.png "Überprüfen Sie den Code für InfColorPicker in Github")](walkthrough-images/image02.png#lightbox)

Die folgenden drei Verzeichnisse im Projekt ersichtlich:

- **InfColorPicker** -dieses Verzeichnis enthält den Objective-C-Code für das Projekt.
- **PickerSamplePad** -dieses Verzeichnis enthält ein Beispielprojekt für das iPad.
- **PickerSamplePhone** -dieses Verzeichnis enthält ein Beispielprojekt für das iPhone.

Wir laden die InfColorPicker-Projekt aus [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) und Entpacken Sie es in das Verzeichnis unsere Wahl. Das Xcode-Ziel für öffne `PickerSamplePhone` Projekt sehen Sie die folgenden Projektstruktur im Xcode Navigator:

[![](walkthrough-images/image03.png "Die Projektstruktur in Xcode-Navigator")](walkthrough-images/image03.png#lightbox)

Dieses Projekt erreicht Wiederverwendung des Codes durch die direkt in jeder Beispielprojekt Hinzufügen des InfColorPicker Quellcodes (das rote Feld markiert). Der Code für das Beispielprojekt befindet sich innerhalb der blauen. Da dieses Projekt keine uns mit einer statischen Bibliothek bietet, ist es notwendig, damit wir erstellen eine Xcode-Projekt, um die statische Bibliothek zu kompilieren.

Der erste Schritt ist für uns InfoColorPicker Quellcode in der statischen Bibliothek hinzufügen. Lassen Sie uns dazu gehen Sie folgendermaßen vor:

1. Starten Sie Xcode.
2. Aus der **Datei** Menü die Option **neu** > **Projekt...** :

    [![](walkthrough-images/image04.png "Starten eines neuen Projekts")](walkthrough-images/image04.png#lightbox)
3. Wählen Sie **Framework & Bibliothek**, **Kakao berühren statische Bibliothek** Vorlage, und klicken Sie auf die **Weiter** Schaltfläche:

    [![](walkthrough-images/image05.png "Wählen Sie die Vorlage Kakao berühren statische Bibliothek")](walkthrough-images/image05.png#lightbox)
4. Geben Sie `InfColorPicker` für die **Projektname** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](walkthrough-images/image06.png "Geben Sie InfColorPicker für den Namen des Projekts")](walkthrough-images/image06.png#lightbox)
5. Wählen Sie einen Speicherort zum Speichern Sie das Projekt, und klicken Sie auf die **OK** Schaltfläche.
6. Nun müssen wir die Quelle aus dem Projekt InfColorPicker unsere statisches Bibliotheksprojekt hinzufügen. Da die **InfColorPicker.h** Datei bereits in unserem statischen Bibliothek (standardmäßig), Xcode lässt nicht uns zu überschreiben. Aus der **Finder**, navigieren Sie zu den InfColorPicker Quellcode in das ursprüngliche Projekt ein, die wir von GitHub entzippt, kopieren Sie alle Dateien InfColorPicker und fügen Sie sie in unserer neuen statisches Bibliotheksprojekt:

    [![](walkthrough-images/image12.png "Kopieren Sie alle Dateien InfColorPicker")](walkthrough-images/image12.png#lightbox)

7. Zum Xcode zurückkehren, klicken Sie mit der rechten Maustaste auf die **InfColorPicker** Ordner, und wählen **Hinzufügen von Dateien zur "InfColorPicker..."**:

    [![](walkthrough-images/image08.png "Hinzufügen von Dateien")](walkthrough-images/image08.png#lightbox)

8. Navigieren Sie aus dem Dialogfeld Hinzufügen von Dateien zu InfColorPicker Quellcodedateien, die wir gerade kopiert haben, markieren sie alle aus, und klicken Sie auf die **hinzufügen** Schaltfläche:

    [![](walkthrough-images/image09.png "Wählen Sie alle aus, und klicken Sie auf die Schaltfläche "hinzufügen"")](walkthrough-images/image09.png#lightbox)

9. Der Quellcode wird in unserem Projekt kopiert werden:

    [![](walkthrough-images/image10.png "Der Quellcode wird in das Projekt kopiert werden")](walkthrough-images/image10.png#lightbox)

10. Wählen Sie im Navigator Xcode-Projekt die **InfColorPicker.m** Datei, und kommentieren Sie die letzten beiden Zeilen (aufgrund der Weise, die diese Bibliothek geschrieben wurde, wird diese Datei wird nicht verwendet):

    [![](walkthrough-images/image14.png "Bearbeiten der Datei InfColorPicker.m")](walkthrough-images/image14.png#lightbox)

11. Jetzt müssen wir überprüfen, ob es keines der Frameworks, die Bibliothek erforderlich sind. Diese Informationen finden in der Readme-Datei, oder öffnen Sie die Beispielprojekte bereitgestellt. Dieses Beispiel verwendet `Foundation.framework`, `UIKit.framework`, und `CoreGraphics.framework` wir also hinzufügen.

12. Wählen Sie die **InfColorPicker Ziel > Build Phases** und erweitern Sie die **Link Binärdatei mit Bibliotheken** Abschnitt:

    [![](walkthrough-images/image16b.png "Erweitern Sie den Abschnitt Link Binärdatei mit Bibliotheken")](walkthrough-images/image16b.png#lightbox)

13. Verwenden der **+** Schaltfläche Öffnen Sie das Dialogfeld, sodass Sie die erforderlichen Frames Frameworks, die oben aufgeführten hinzufügen:

    [![](walkthrough-images/image16c.png "Fügen Sie, dass die erforderlichen Frames Frameworks oben aufgeführten hinzu.")](walkthrough-images/image16c.png#lightbox)

14. Die **Link Binärdatei mit Bibliotheken** Abschnitt sollte jetzt wie der folgenden Abbildung aussehen:

    [![](walkthrough-images/image16d.png "Im Abschnitt Link Binärdatei mit Bibliotheken")](walkthrough-images/image16d.png#lightbox)

Wir sind zu diesem Zeitpunkt schließen, aber wir nicht ganz fertig sind. Die statische Bibliothek erstellt wurde, aber wir benötigen, erstellen Sie sie zum Erstellen einer Fat binäre, die alle erforderlichen Architekturen für iOS-Gerät und iOS-Simulator umfasst.

### <a name="creating-a-fat-binary"></a>Erstellen eine Fat-Binärdatei

Alle iOS-Geräte haben Prozessoren Karten von ARM-Architektur, die mit der Zeit selbst entwickelt haben. Jede neue Architektur hinzugefügt neue Anweisungen und andere Optimierungen, während Sie gleichzeitig Abwärtskompatibilität Kompatibilität. Auf iOS-Geräte liegen armv6, armv7, armv7s, arm64 Anweisungssets – Obwohl [verwenden wir nicht mehr armv6](~/ios/deploy-test/compiling-for-different-devices.md). IOS-Simulator wird nicht unterstützt, von ARM und statt diesen schweren ist, eine X86- und mit Strom versorgt x86_64 Simulator. Wir, die für uns bedeutet besteht darin, dass die Bibliotheken für jede Anweisung bereitgestellt werden müssen legt sie fest.

Eine Fat-Bibliothek ist `.a` Datei, die alle unterstützten Architekturen enthält.

Erstellen eine Fat binäre ist drei Schritten:

- Eine ARM-7 & ARM64 Version von der statischen Bibliothek zu kompilieren.
- Eine X86- und x84_64 Version von der statischen Bibliothek zu kompilieren.
- Verwenden der `lipo` -Befehlszeilentool zum Kombinieren der zwei statischen Bibliotheken.

Während diese drei Schritte unkompliziert sind, und es erforderlich sein kann, sie in der Zukunft wiederholen, wenn Fehlerbehebungen sind erforderlich, oder die Objective-C-Bibliothek aktualisiert wird. Wenn Sie diese Schritte automatisieren möchten, wird die künftige Wartung und Unterstützung der iOS-bindungsprojekt vereinfacht.

Es stehen zahlreiche Tools zum Automatisieren von Aufgaben – z. B. ein Shellskript verfügbar [Neigungswinkel](http://rake.rubyforge.org/), [Xbuild](http://www.mono-project.com/docs/tools+libraries/tools/xbuild/), und [stellen](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Wenn wir die Xcode-Befehlszeilentools installiert haben, installiert wir außerdem vergewissern, dass das Buildsystem, die für diese exemplarische Vorgehensweise verwendet werden. Hier ist ein **Makefile** , mit denen Sie können eine Architektur mit mehreren freigegebene Bibliothek erstellen, die auf einem iOS-Gerät und der Simulator für jede der Bibliothek funktioniert:

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

Geben Sie die **Makefile** Befehle im nur-Text-Editor Ihrer Wahl, und aktualisieren Sie in den Abschnitten mit **Ihrem PROJEKTNAMEN** mit dem Namen des Projekts. Es ist auch wichtig, sicherzustellen, dass wir Sie fügen Sie die Anweisungen oben, dass die Registerkarten in den Anweisungen beibehalten wurden.

Speichern Sie die Datei mit dem Namen **Makefile** am gleichen Speicherort wie die statische Bibliothek Xcode InfColorPicker oben erstellten:

[![](walkthrough-images/lib00.png "Speichern Sie die Datei mit dem Namen Makefile")](walkthrough-images/lib00.png#lightbox)

Öffnen Sie die Terminal-Anwendung auf Ihrem Mac, und navigieren Sie zum Speicherort der Makefile. Typ `make` drücken Sie die in den Endstatus **EINGABETASTE** und die **Makefile** wird ausgeführt:

[![](walkthrough-images/lib01.png "Makefile-Beispielausgabe")](walkthrough-images/lib01.png#lightbox)

Bei der Ausführung stellen sehen Sie viel Text Durchführen eines Bildlaufs durch. Wenn alles ordnungsgemäß funktioniert, sehen Sie die Wörter **erstellt wurde** und `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` und `libInfColorPickerSDK.a` kopiert Dateien an demselben Speicherort wie die **Makefile**:

[![](walkthrough-images/lib02.png "The libInfColorPicker-armv7.a, libInfColorPicker-i386.a and libInfColorPickerSDK.a files generated by the Makefile")](walkthrough-images/lib02.png#lightbox)

Sie können die Architekturen in die Binärdatei Fat mithilfe des folgenden Befehls überprüfen:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Es wird Folgendes angezeigt:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

An diesem Punkt haben wir den ersten Schritt von unserer iOS-Bindung abgeschlossen, durch das Erstellen einer statischen Bibliothek mit Xcode und die Xcode-Befehlszeilentools `make` und `lipo`. Wir mit dem nächsten Schritt verschieben, und verwenden Sie **Ziel Sharpie** zum Automatisieren der Erstellung der API-Bindungen für uns.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Erstellen Sie ein Projekt binden Xamarin.iOS

Bevor wir verwenden können, **Ziel Sharpie** um des Bindungsvorgangs automatisieren möchten, müssen zum Erstellen eines Projekts Xamarin.iOS Bindung, die API-Definitionen aufnehmen soll (, die wir verwenden **Ziel Sharpie** helfen uns beim Erstellen) und die C#-Bindung für uns erstellt.

Führen Sie wir Folgendes:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac.
1. Aus der **Datei** klicken Sie im Menü **neu** > **Lösung...** :

    ![](walkthrough-images/bind01.png "Starten Sie eine neue Projektmappe")

1. Wählen Sie im Dialogfeld neue Projektmappe **Bibliothek** > **iOS-Projekt Binding**:

    ![](walkthrough-images/bind02.png "Wählen Sie die iOS-Projekt binden")

1. Klicken Sie auf die **Weiter** Schaltfläche.

1. Geben Sie "InfColorPickerBinding" als die **Projektname** , und klicken Sie auf die **erstellen** Schaltfläche, um die Projektmappe zu erstellen:

    ![](walkthrough-images/bind02a.png "Geben Sie InfColorPickerBinding als den Namen des Projekts")

Die Projektmappe wird erstellt und zwei Standarddateien werden:

![](walkthrough-images/bind03.png "Die Projektmappenstruktur im Projektmappen-Explorer")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Starten Sie Visual Studio.

1. Aus der **Datei** klicken Sie im Menü **neu** > **Projekt...** :

    ![](walkthrough-images/bind01vs.png "Starten eines neuen Projekts")

1. Wählen Sie im Dialogfeld "Neues Projekt" **iOS** > **Bindungen Bibliothek**:

    ![](walkthrough-images/bind02vs.png "Wählen Sie die iOS-Bindungen-Bibliothek")

1. Geben Sie "InfColorPickerBinding" als die **Namen** , und klicken Sie auf die **OK** Schaltfläche, um die Projektmappe zu erstellen.

Die Projektmappe wird erstellt und zwei Standarddateien werden:

![](walkthrough-images/bind03vs.png "Die Projektmappenstruktur im Projektmappen-Explorer")

-----



- **ApiDefinition.cs** – diese Datei enthält die Verträge, die definieren, wie Objective-C-API in c# umbrochen wird.
- **Structs.cs** – diese Datei enthält alle Strukturen oder-Enumerationswerte fest, die die Schnittstellen und Delegaten erforderlich sind.

Wir arbeiten mit diesen beiden Dateien weiter unten in dieser exemplarischen Vorgehensweise. Zunächst muss das bindungsprojekt der InfColorPicker-Bibliothek hinzu.

### <a name="including-the-static-library-in-the-binding-project"></a>Einschließlich der statischen Bibliothek in der Bindungsprojekt

Jetzt wir unsere Bindung Basisprojekt bereit haben, müssen wir die binäre Fat-Bibliothek hinzufügen für oben erstellten der **InfColorPicker** Bibliothek.

Gehen Sie zum Hinzufügen der Bibliotheks wie folgt vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Mit der rechten Maustaste auf die **systemeigene Verweise** Ordner in der Projektmappe aufgefüllt, und wählen **systemeigene Verweise hinzufügen**:

    ![](walkthrough-images/bind04a.png "Fügen Sie systemeigene Verweise hinzu")

1. Navigieren Sie zu der Fat binäre vorher getroffen (`libInfColorPickerSDK.a`), und drücken Sie die **öffnen** Schaltfläche:

    ![](walkthrough-images/bind05.png "Wählen Sie die Datei libInfColorPickerSDK.a")
1. Die Datei wird im Projekt enthalten sein:

    ![](walkthrough-images/bind04.png "Z. B. eine Datei")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopieren der `libInfColorPickerSDK.a` aus Ihrer **Mac-Buildhost** und fügen Sie ihn in das bindungsprojekt.

1. Mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen > Vorhandenes Element...** :

    ![](walkthrough-images/bind04vs.png "Hinzufügen einer vorhandenen Datei")

1. Navigieren Sie zu der `libInfColorPickerSDK.a` , und drücken Sie die **hinzufügen** Schaltfläche:

    ![](walkthrough-images/bind05vs.png "Hinzufügen von libInfColorPickerSDK.a")

1. Die Datei wird in das Projekt aufgenommen werden.

-----


Wenn eine Datei zum Projekt hinzugefügt wird, Xamarin.iOS werden automatisch festgelegt, die **Buildvorgang** der Datei **ObjcBindingNativeLibrary**, und erstellen Sie eine spezielle Datei namens `libInfColorPickerSDK.linkwith.cs`.


Diese Datei enthält die `LinkWith` Attribut, Xamarin.iOS mitteilt, wie Handle der statischen Bibliothek, die wir gerade hinzugefügt. Der Inhalt dieser Datei wird im folgenden Codeausschnitt gezeigt:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

Die `LinkWith` Attribut identifiziert, die statische Bibliothek für das Projekt und einige wichtige Linker-Flags.


Im nächsten Schritt erforderlich ist, die API-Definitionen für das Projekt InfColorPicker erstellen. Für den Rahmen dieser exemplarischen Vorgehensweise verwenden wir Ziel Sharpie zum Generieren der Datei **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Verwenden von Dienstziel Sharpie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


Objektive Sharpie ist über die Befehlszeile Tool (bereitgestellt von Xamarin), die helfen kann, erstellen die Definitionen, die zum Binden einer 3rd Party Objective-C-Bibliothek in c# erforderlich. In diesem Abschnitt verwenden wir Ziel Sharpie zum Erstellen der anfänglichen **ApiDefinition.cs** für das Projekt InfColorPicker.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Objektive Sharpie ist über die Befehlszeile Tool (bereitgestellt von Xamarin), die helfen kann, erstellen die Definitionen, die zum Binden einer 3rd Party Objective-C-Bibliothek in c# erforderlich. In diesem Abschnitt verwenden wir Ziel Sharpie auf unserer **Mac-Buildhost** zum Erstellen der anfänglichen **ApiDefinition.cs** für das Projekt InfColorPicker.


-----

Um zu beginnen, sehen wir herunterladen Ziel Sharpie Installer-Datei, wie ausführlich in dieser [Handbuch](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Führen Sie das Installationsprogramm, und befolgen Sie alle eingabeaufforderungen auf dem Bildschirm des Installations-Assistenten Ziel Sharpie auf unserem Entwicklungscomputer zu installieren.

Nachdem wir Ziel Sharpie erfolgreich sein installiert, wir die Terminal-app starten und geben Sie den folgenden Befehl aus, um Hilfe für alle Tools, die es bereitstellt, um die Bindung zu unterstützen:

```bash
sharpie -help
```

Wenn den obigen Befehl ausgeführt werden, wird die folgende Ausgabe generiert:

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

In dieser exemplarischen Vorgehensweise wird die folgende Ziel Sharpie Tools verwenden:

- **Xcode** - dieses Tools erhalten Sie Informationen zu unseren aktuellen Xcode installieren und die Versionen von iOS und Mac-APIs, die es installiert haben. Wir werden diese Informationen später verwenden werden, wenn wir unsere Bindungen generieren.
- **Binden** -verwenden wir dieses Tool analysiert die **h** Dateien im Projekt in der ersten InfColorPicker **ApiDefinition.cs** und **StructsAndEnums.cs** Dateien.

Um Hilfe für ein bestimmtes Ziel Sharpie-Tool zu erhalten, geben Sie den Namen des Tools und die `-help` Option. Beispielsweise `sharpie xcode -help` gibt die folgende Ausgabe:

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

Bevor wir den Bindungsprozess beginnen können, müssen wir erhalten Informationen zu unseren aktuellen installierten SDKs durch Eingabe des folgenden Befehls in die Terminal `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Aus den oben genannten können wir sehen, dass wir haben die `iphoneos8.1` SDK auf unseren Machine installiert. Mit diesen Informationen vorhanden, werden wir bereit, das Projekt InfColorPicker analysieren `.h` Dateien in den ersten **ApiDefinition.cs** und `StructsAndEnums.cs` für das Projekt InfColorPicker.

Geben Sie den folgenden Befehl in der Terminal-app:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

In dem `[full-path-to-project]` ist der vollständige Pfad zum Verzeichnis, in dem die **InfColorPicker** Xcode-Projektdatei befindet sich auf unser Computer und [Iphone-Betriebssystem] ist das iOS SDK, die wir installiert haben, wie erwähnt durch die `sharpie xcode -sdks` Befehl. Beachten Sie, dass in diesem Beispiel wird übergeben haben  **\*h** als Parameter verwendet, darunter *alle* die Header-Dateien in diesem Verzeichnis - normalerweise sollten Sie dies nicht der Fall, jedoch stattdessen sorgfältig durchlesen der Header-Dateien auf der obersten Ebene suchen **h** Datei, alle anderen relevanten Dateien verweist, und übergeben, die nur zum Ziel Sharpie.

Die folgenden [Ausgabe](walkthrough-images/os05.png) wird in der abschließenden generiert:

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

Und die **InfColorPicker.enums.cs** und **InfColorPicker.cs** Dateien in unserem Verzeichnis erstellt werden:

[![](walkthrough-images/os06.png "Die InfColorPicker.enums.cs und InfColorPicker.cs-Dateien")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


Öffnen Sie beide Dateien im Projekt Bindung, dem wir zuvor erstellt haben. Kopieren Sie den Inhalt von der **InfColorPicker.cs** Datei, und fügen Sie ihn in die **ApiDefinition.cs** Ersetzen der vorhandenen Datei `namespace ...` Codeblock durch den Inhalt der der  **InfColorPicker.cs** Datei (verlassen der `using` Anweisungen intakt):

![](walkthrough-images/os07.png "Die Datei InfColorPickerControllerDelegate")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Öffnen Sie beide Dateien im Projekt Bindung, dem wir zuvor erstellt haben. Kopieren Sie den Inhalt von der **InfColorPicker.cs** Datei (aus der **Mac-Buildhost**) und fügen Sie ihn in die **ApiDefinition.cs** Ersetzen der vorhandenen Datei `namespace ...` Codeblock mit dem Inhalt der **InfColorPicker.cs** Datei (verlassen der `using` Anweisungen intakt).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>Normalisiert die API-Definitionen

Objektive Sharpie in einigen Fällen hat ein Problem übersetzen `Delegates`, daher wird die Definition der ändern muss die `InfColorPickerControllerDelegate` Schnittstelle, und Ersetzen Sie die `[Protocol, Model]` Zeile mit den folgenden:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
So, dass die Definition sieht wie folgt aus:

[![](walkthrough-images/os11.png "Die definition")](walkthrough-images/os11.png#lightbox)

Als Nächstes, führen wir die gleiche Aufgabe mit dem Inhalt der `InfColorPicker.enums.cs` Datei kopieren und Einfügen in die `StructsAndEnums.cs` Datei verlassen der `using` intakt Anweisungen:

[![](walkthrough-images/os09.png "Der Inhalt der StructsAndEnums.cs Datei ")](walkthrough-images/os09.png#lightbox)

Möglicherweise finden Sie auch, dass die Bindung mit Nachrichten Ziel Sharpie kommentiert wurde `[Verify]` Attribute. Diese Attribute geben, Sie sicherstellen sollten, dass das Ziel Sharpie wurde die richtige Eingabe durch Vergleichen der Bindung mit der ursprünglichen C#/Objective-C-Deklaration (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt werden). Nachdem Sie die Bindungen überprüft haben, sollten Sie das Attribut überprüfen entfernen. Weitere Informationen finden Sie unter der [überprüfen](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) Handbuch.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


An diesem Punkt sollte unsere bindungsprojekt nun fertig und kann zum Erstellen. Wir unsere Bindung Buildprojekt, und stellen Sie sicher, dass es ohne Fehler geliefert:

[Erstellen Sie das bindungsprojekt, und stellen Sie sicher, dass keine Fehler vorliegen](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


An diesem Punkt sollte unsere bindungsprojekt nun fertig und kann zum Erstellen. Wir unsere Bindung Buildprojekt, und stellen Sie sicher, dass es ohne Fehler geliefert.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Verwenden der Bindung

Führen Sie diese Schritte zum Erstellen einer beispielanwendung iPhone iOS verwenden, denen binden Bibliothek oben erstellt haben:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. **Erstellen Xamarin.iOS Projekt** -hinzufügen ein neues Xamarin.iOS Projekt mit der Bezeichnung **InfColorPickerSample** der Projektmappe, wie in den folgenden Screenshots dargestellt:

    ![](walkthrough-images/use01.png "Hinzufügen einer einzelnen Ansicht-App")

    ![](walkthrough-images/use01a.png "Den Bezeichner festlegen")

1. **Hinzufügen der Verweis auf das Projekt binden** -Update die **InfColorPickerSample** Projekt so, dass sie einen Verweis auf die **InfColorPickerBinding** Projekt:

    ![](walkthrough-images/use02.png "Verweis auf das Bindungsprojekt hinzufügen")

1. **Erstellen Sie die iPhone-Benutzeroberfläche** -Doppelklick auf die **MainStoryboard.storyboard** in der Datei die **InfColorPickerSample** Projekt in der iOS-Designer bearbeitet. Hinzufügen einer **Schaltfläche** zur Ansicht und Aufruf der `ChangeColorButton`, wie im folgenden gezeigt:

    ![](walkthrough-images/use03.png "Hinzufügen einer Schaltfläche in der Ansicht")
1. **Hinzufügen der InfColorPickerView.xib** -der InfColorPicker Objective-C-Bibliothek enthält eine **.xib** Datei. Xamarin.iOS umfasst dies jedoch nicht **.xib** im bindungsprojekt, wodurch Laufzeitfehler in unserem Beispiel-Anwendung. Dieses Problem zu umgehen, besteht darin, hinzuzufügen der **.xib** -Datei zu unserem Xamarin.iOS-Projekt. Wählen Sie die Xamarin.iOS-Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen > Hinzufügen von Dateien**, und fügen die **.xib** Datei wie im folgenden Screenshot gezeigt:

    ![](walkthrough-images/use04.png "Fügen Sie der InfColorPickerView.xib hinzu.")

1. Wenn Sie gefragt werden, kopieren Sie die **.xib** -Datei in das Projekt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **Erstellen Xamarin.iOS Projekt** -hinzufügen ein neues Xamarin.iOS Projekt mit der Bezeichnung **InfColorPickerSample** der Projektmappe, wie im folgenden Screenshot gezeigt:

    ![](walkthrough-images/use01vs.png "Xamarin.iOS-Projekt erstellen")

1. **Hinzufügen der Verweis auf das Projekt binden** -Update die **InfColorPickerSample** Projekt so, dass sie einen Verweis auf die **InfColorPickerBinding** Projekt:

    ![](walkthrough-images/use02vs.png "Verweis auf das Bindungsprojekt hinzufügen")

1. **Erstellen Sie die iPhone-Benutzeroberfläche** -Doppelklick auf die **MainStoryboard.storyboard** in der Datei die **InfColorPickerSample** Projekt in der iOS-Designer bearbeitet. Hinzufügen einer **Schaltfläche** zur Ansicht und Aufruf der `ChangeColorButton`, wie im folgenden gezeigt:

    ![](walkthrough-images/use03vs.png "Erstellen Sie die iPhone-Benutzeroberfläche")

1. **Hinzufügen der InfColorPickerView.xib** -der InfColorPicker Objective-C-Bibliothek enthält eine **.xib** Datei. Xamarin.iOS umfasst dies jedoch nicht **.xib** im bindungsprojekt, wodurch Laufzeitfehler in unserem Beispiel-Anwendung. Dieses Problem zu umgehen, besteht darin, hinzuzufügen der **.xib** -Datei zu unserem Xamarin.iOS-Projekt aus unserem **Mac-Buildhost**. Wählen Sie die Xamarin.iOS-Projekt, mit der rechten Maustaste und wählen Sie **hinzufügen** > **vorhandenes Element...** , und fügen die **.xib** Datei.


-----



Als Nächstes werfen wir einen Blick darauf Protokolle in Objective-C und wie wir diese Bindung und C#-Code behandeln.

### <a name="protocols-and-xamarinios"></a>Protokolle und Xamarin.iOS

In Objective-C definiert ein Protokoll Methoden (oder Nachrichten), die unter bestimmten Umständen verwendet werden kann. Im Prinzip sind sie Schnittstellen in c# sehr ähnlich. Ein Hauptunterschied zwischen einer Objective-C-Protokoll und einer C#-Schnittstelle ist, dass Protokolle optionale Methoden - Methoden können, die eine Klasse nicht implementiert. Objective-C verwendet die @optional -Schlüsselwort wird verwendet, um anzugeben, welche Methoden optional sind. Weitere Informationen zu Protokollen finden Sie unter [Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** verfügt über eine solche Protokoll, in der Codeausschnitt unten gezeigt:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Dieses Protokoll wird verwendet, indem **InfColorPickerController** um Clients zu informieren, dass der Benutzer eine neue Farbe und die übernommen hat die **InfColorPickerController** abgeschlossen ist. Objektive Sharpie dieses Protokoll zugeordnet, wie im folgenden Codeausschnitt gezeigt:

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

Wenn die Bindung Bibliothek kompiliert wird, erstellt Xamarin.iOS eine abstrakte Basisklasse aufgerufen `InfColorPickerControllerDelegate`, die diese Schnittstelle implementiert, mit virtuellen Methoden.

Es gibt zwei Möglichkeiten, dass wir diese Schnittstelle in einer Anwendung Xamarin.iOS implementieren können:

- **Delegieren der starken** -mit einem starken Delegaten umfasst das Erstellen einer C#-Klasse, Unterklassen `InfColorPickerControllerDelegate` und überschreibt die entsprechenden Methoden. **InfColorPickerController** wird eine Instanz dieser Klasse für die Kommunikation mit Clients verwenden.
- **Schwache Delegaten** -ein schwache Delegat ist ein etwas andere Verfahren, das umfasst das Erstellen einer öffentlichen Methode auf eine Klasse (z. B. `InfColorPickerSampleViewController`) und dann Verfügbarmachen dieser Methode für die `InfColorPickerDelegate` Protokoll über ein `Export` Attribut.

Starke Delegaten bieten Intellisense, typsicherheit sowie eine bessere Kapselung. Aus diesen Gründen sollten Sie starke Delegaten verwenden, können Sie, statt einen schwachen Delegaten.

In dieser exemplarischen Vorgehensweise werden beide Verfahren behandelt: zuerst einen starken Delegaten implementieren, und anschließend erläutert, wie einen schwachen Delegaten implementieren.

### <a name="implementing-a-strong-delegate"></a>Implementieren einen starken Delegaten

Fertig stellen die Xamarin.iOS-Anwendung mit einem starken Delegaten So reagieren Sie auf die `colorPickerControllerDidFinish:` Nachricht:

**Unterklasse InfColorPickerControllerDelegate** -fügen Sie eine neue Klasse, um das Projekt mit der Bezeichnung `ColorSelectedDelegate`. Bearbeiten Sie die Klasse so, dass sie den folgenden Code:

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

Xamarin.iOS wird der Delegat Objective-C binden, indem Sie erstellen eine abstrakte Basisklasse, die aufgerufen `InfColorPickerControllerDelegate`. Unterklasse diesen Typ und die Außerkraftsetzung der `ColorPickerControllerDidFinish` Methode Zugriff auf den Wert von der `ResultColor` Eigenschaft `InfColorPickerController`.

**Erstellen Sie eine Instanz des ColorSelectedDelegate** -unsere Ereignishandler benötigen eine Instanz von der `ColorSelectedDelegate` Typ, das im vorherigen Schritt erstellt wurde. Bearbeiten Sie die Klasse `InfColorPickerSampleViewController` und die Klasse die folgenden Instanzvariable hinzuzufügen:

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
**Implementieren Sie die Methode HandleTouchUpInsideWithStrongDelegate** -als Nächstes implementieren Sie den Ereignishandler für der Benutzer bei berührt **ColorChangeButton**. Bearbeiten Sie `ViewController`, und fügen Sie die folgende Methode hinzu:

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

Wir rufen Sie zunächst eine Instanz von `InfColorPickerController` über eine statische Methode, und stellen, die Instanz, beachten Sie unsere starke Delegat über die Eigenschaft `InfColorPickerController.Delegate`. Diese Eigenschaft wurde automatisch vom Ziel Sharpie für uns erstellt. Zum Schluss rufen wir `PresentModallyOverViewController` zum Anzeigen der Ansicht `InfColorPickerSampleViewController.xib` , damit der Benutzer eine Farbe auswählen kann.

**Führen Sie die Anwendung** – zu diesem Zeitpunkt sind mit allen des getesteten Codes fertig. Wenn Sie die Anwendung ausführen, Sie sollten möglicherweise so ändern Sie die Farbe des Hintergrunds der `InfColorColorPickerSampleView` wie in den folgenden Screenshots dargestellt:

[![](walkthrough-images/run01.png "Ausführen der Anwendung")](walkthrough-images/run01.png#lightbox)

Herzlichen Glückwunsch! An diesem Punkt haben Sie erfolgreich erstellt und eine Objective-C-Bibliothek zur Verwendung in einem Xamarin.iOS-Anwendung gebunden wird. Als Nächstes sehen wir die Informationen zur Verwendung von schwacher Delegaten.

### <a name="implementing-a-weak-delegate"></a>Implementieren einen schwachen Delegaten

Anstatt das Erstellen von Unterklassen für eine Klasse, die auf das Objective-C-Protokoll für einen bestimmten Delegaten gebunden, Xamarin.iOS können Sie auch die Protokollmethoden in einer Klasse zu implementieren, die abgeleitet `NSObject`, ergänzen die Methoden mit der `ExportAttribute`, und klicken Sie dann Angeben der entsprechenden Selektoren an. Wenn Sie diesen Ansatz verwenden, weisen Sie eine Instanz dieser Klasse die `WeakDelegate` Eigenschaft anstatt auf die `Delegate` Eigenschaft. Ein schwacher Delegaten bietet Ihnen die Flexibilität, um Ihre Delegate-Klasse in eine andere Hierarchie nach unten zu nutzen. Sehen Sie zum Implementieren und Verwenden von schwachen Delegaten in der vorliegenden Anwendung Xamarin.iOS an.

**Erstellen Sie Ereignishandler für TouchUpInside** -erstellen wir einen neuen Ereignishandler für das `TouchUpInside` -Ereignis der Schaltfläche Hintergrundfarbe ändern. Dieser Handler wird die gleiche Funktion wie das Ausfüllen der `HandleTouchUpInsideWithStrongDelegate` Handler, dass wir im vorherigen Abschnitt erstellt haben, verwenden jedoch einen schwachen Delegaten anstelle eines starken Delegaten. Bearbeiten Sie die Klasse `ViewController`, und fügen Sie die folgende Methode hinzu:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Aktualisieren von ViewDidLoad** -ändern wir `ViewDidLoad` , sodass den Ereignishandler verwendet, die soeben erstellt wurde. Bearbeiten Sie `ViewController` , und ändern Sie `ViewDidLoad` wie den folgende Codeausschnitt aussehen:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Behandeln der ColorPickerControllerDidFinish: Nachricht** – im Falle der `ViewController` ist nicht mehr benötigen, iOS sendet die Nachricht `colorPickerControllerDidFinish:` auf die `WeakDelegate`. Wir müssen eine C#-Methode erstellen, die diese Nachricht verarbeiten kann. Zu diesem Zweck wir erstellen Sie eine C#-Methode, und klicken Sie dann hinzugefügt werden sollen diese mit der `ExportAttribute`. Bearbeiten Sie `ViewController`, und fügen Sie die folgende Methode der Klasse:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Führen Sie die Anwendung aus. Es sollten jetzt Verhalten sich genau wie gewohnt verwendet, aber einen schwachen Delegaten anstelle der starken Delegat verwendeten. An diesem Punkt haben Sie diese exemplarische Vorgehensweise erfolgreich abgeschlossen. Sie verfügen jetzt über einen Überblick über das Erstellen und Nutzen eines Xamarin.iOS Bindung-Projekts.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden durch den Prozess des Erstellens und Verwendens eines Xamarin.iOS Bindung-Projekts. Zunächst besprochen haben wir wie eine vorhandene Objective-C-Bibliothek in einer statischen Bibliothek kompiliert. Klicken Sie dann behandelt wird wie ein Xamarin.iOS Bindung-Projekt erstellt und zum Ziel Sharpie verwenden, um die API-Definitionen für Objective-C-Bibliothek zu generieren. Zum Aktualisieren und Anpassen der generierten API-Definitionen, um sie für die öffentliche Nutzung geeignet besprochen. Nachdem das Xamarin.iOS bindungsprojekt abgeschlossen wurde, verschoben wir zu nutzen, dass die Bindung in einer Anwendung Xamarin.iOS mit dem Schwerpunkt auf starke Delegaten mit schwachen Delegaten auf.

## <a name="related-links"></a>Verwandte Links

- [Beispiel für sitebindung (Beispiel)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Weitere Informationen zu](~/cross-platform/macios/binding/overview.md)
- [Bindung Typen-Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin für Objective-C-Entwickler](~/ios/get-started/objective-c-developers/index.md)
- [Frameworkentwurfsrichtlinien](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
