---
title: 'Exemplarische Vorgehensweise: Binden einer iOS Objective-C-Bibliothek'
description: Dieser Artikel enthält eine praktische Exemplarische Vorgehensweise zum Erstellen einer xamarin. IOS-Bindung für eine vorhandene Ziel-C-Bibliothek, infcolorpicker. Es behandelt Themen wie das Kompilieren einer statischen Ziel-C-Bibliothek, deren Bindung und die Verwendung der Bindung in einer xamarin. IOS-Anwendung.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/02/2017
ms.openlocfilehash: b53799f4b1c8d9299ab23191f6a702c2ec0983fb
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285766"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Exemplarische Vorgehensweise: Binden einer iOS Objective-C-Bibliothek

_Dieser Artikel enthält eine praktische Exemplarische Vorgehensweise zum Erstellen einer xamarin. IOS-Bindung für eine vorhandene Ziel-C-Bibliothek, infcolorpicker. Es behandelt Themen wie das Kompilieren einer statischen Ziel-C-Bibliothek, deren Bindung und die Verwendung der Bindung in einer xamarin. IOS-Anwendung._

Bei der Arbeit mit IOS kann es vorkommen, dass Sie eine Ziel-C-Bibliothek eines Drittanbieters verwenden möchten. In diesen Fällen können Sie ein xamarin. IOS- _Bindungs Projekt_ verwenden, um eine [ C# Bindung](~/cross-platform/macios/binding/overview.md) zu erstellen, die es Ihnen ermöglicht, die Bibliothek in ihren xamarin. IOS-Anwendungen zu nutzen.

In der Regel im IOS-Ökosystem finden Sie Bibliotheken in drei Varianten:

- Als vorkompilierte statische Bibliotheksdatei mit `.a` Erweiterung zusammen mit den zugehörigen Headern (h-Dateien). Beispiel: [die Analyse Bibliothek von Google](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
- Als vorkompiliertes Framework. Dabei handelt es sich nur um einen Ordner, der die statische Bibliothek, Header und `.framework` manchmal zusätzliche Ressourcen mit der Erweiterung enthält. Beispielsweise [die AdMob-Bibliothek von Google](https://developers.google.com/admob/ios/download).
- Nur als Quell Code Dateien. Beispielsweise eine Bibliothek, die nur `.m` und `.h` Ziel-C-Dateien enthält.

Im ersten und zweiten Szenario gibt es bereits eine vorkompilierte, statische cocoatouch-Bibliothek, daher konzentrieren wir uns in diesem Artikel auf das dritte Szenario. Bevor Sie beginnen, eine Bindung zu erstellen, sollten Sie die in der Bibliothek bereitgestellte Lizenz immer überprüfen, um sicherzustellen, dass Sie diese binden können.

Dieser Artikel enthält eine schrittweise exemplarische Vorgehensweise zum Erstellen eines Bindungs Projekts mithilfe des Open Source-Projekts " [infcolorpicker](https://github.com/InfinitApps/InfColorPicker) " für Ziel-c. alle Informationen in diesem Handbuch können jedoch für die Verwendung mit der Ziel-c-Bibliothek von Drittanbietern angepasst werden. . Die infcolorpicker-Bibliothek stellt einen wiederverwendbaren Ansichts Controller bereit, mit dem der Benutzer eine Farbe basierend auf der HSB-Darstellung auswählen kann, wodurch die Farbauswahl benutzerfreundlicher wird.

[![](walkthrough-images/run01.png "Beispiel für die infcolorpicker-Bibliothek, die unter IOS ausgeführt wird")](walkthrough-images/run01.png#lightbox)

Wir behandeln alle notwendigen Schritte, um diese spezielle Ziel-C-API in xamarin. IOS zu verwenden:

- Zuerst erstellen wir eine statische Ziel-C-Bibliothek mithilfe von Xcode.
- Dann binden wir diese statische Bibliothek mit xamarin. IOS.
- Zeigen Sie als nächstes an, wie die Arbeitsauslastung durch Ziel-Sharpie reduziert werden kann, indem automatisch einige (aber nicht alle) der für die xamarin. IOS-Bindung erforderlichen API-Definitionen erstellt werden.
- Schließlich erstellen wir eine xamarin. IOS-Anwendung, die die Bindung verwendet.

Die Beispielanwendung zeigt, wie Sie einen starken Delegaten für die Kommunikation zwischen der infcolorpicker-API C# und unserem Code verwenden. Nachdem Sie erfahren haben, wie Sie einen starken Delegaten verwenden, wird erläutert, wie Sie schwache Delegaten verwenden, um dieselben Aufgaben auszuführen.

## <a name="requirements"></a>Anforderungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Xcode und der Programmiersprache "Ziel-c" vertraut sind und dass Sie unsere [Bindungs Ziel-c-](~/cross-platform/macios/binding/index.md) Dokumentation gelesen haben. Außerdem ist Folgendes erforderlich, um die folgenden Schritte auszuführen:

- **Xcode und IOS SDK** : der Xcode von Apple und die neueste IOS-API müssen auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **[Xcode-Befehlszeilen Tools](#Installing_the_Xcode_Command_Line_Tools)** : die Xcode-Befehlszeilen Tools müssen für die aktuell installierte Version von Xcode installiert werden (Weitere Informationen zur Installation finden Sie unten).
- **Visual Studio für Mac oder Visual Studio** : die neueste Version von Visual Studio für Mac oder Visual Studio sollte auf dem Entwicklungs Computer installiert und konfiguriert werden. Zum Entwickeln einer xamarin. IOS-Anwendung ist ein Apple Mac erforderlich, und wenn Sie Visual Studio verwenden, müssen Sie mit [einem xamarin. IOS-buildhost](~/ios/get-started/installation/windows/connecting-to-mac/index.md) verbunden sein.
- **Die neueste Version von Target Sharpie** : eine aktuelle Kopie des Ziel-Sharpie-Tools, das [hier](~/cross-platform/macios/binding/objective-sharpie/get-started.md)heruntergeladen wird. Wenn Sie bereits das Ziel "Sharpie" installiert haben, können Sie es mithilfe der`sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Installieren der Xcode-Befehlszeilen Tools

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)


Wie bereits erwähnt, verwenden wir Xcode-Befehlszeilen Tools (insbesondere `make` und `lipo`) in dieser exemplarischen Vorgehensweise. Der `make` Befehl ist ein sehr häufiges UNIX-Hilfsprogramm, das die Kompilierung ausführbarer Programme und Bibliotheken mithilfe eines _Makefile_ automatisiert, das angibt, wie das Programm erstellt werden soll. Der `lipo` Befehl ist ein OS X-Befehlszeilen-Hilfsprogramm zum Erstellen von Dateien mit mehreren Architekturen. `.a` es werden mehrere Dateien zu einer Datei zusammengefasst, die von allen Hardwarearchitekturen verwendet werden kann.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


Wie bereits erwähnt, verwenden wir die Xcode-Befehlszeilen Tools auf dem **Mac** -buildhost `make` ( `lipo`insbesondere und) in dieser exemplarischen Vorgehensweise. Der `make` -Befehl ist ein sehr häufiges UNIX-Hilfsprogramm, das die Kompilierung von ausführbaren Programmen und Bibliotheken mithilfe eines _Makefile_ -para gramms automatisiert, das angibt, wie das Programm erstellt werden soll. Der `lipo` Befehl ist ein OS X-Befehlszeilen-Hilfsprogramm zum Erstellen von Dateien mit mehreren Architekturen. `.a` es werden mehrere Dateien zu einer Datei zusammengefasst, die von allen Hardwarearchitekturen verwendet werden kann.


-----

Gemäß der Apple-Dokumentation über [die Befehlszeile mit der Dokumentation zu Xcode FAQ](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) in OS X 10,9 und höher unterstützt der **Download** Bereich des Dialog Felds Xcode- **Einstellungen** nicht mehr die Download-Befehlszeilen Tools.

Sie müssen eine der folgenden Methoden verwenden, um die Tools zu installieren:

- **Installieren von Xcode** : Wenn Sie Xcode installieren, werden alle Ihre Befehlszeilen Tools gebündelt. In OS X 10,9-Shims (installiert `/usr/bin`in) kann jedes in `/usr/bin` enthaltene Tool dem entsprechenden Tool in Xcode zuordnen. Beispielsweise der `xcrun` -Befehl, mit dem Sie ein beliebiges Tool in Xcode über die Befehlszeile suchen oder ausführen können.
- **Die Terminalanwendung** : Sie können die Befehlszeilen Tools über die Terminalanwendung installieren, indem Sie den `xcode-select --install` folgenden Befehl ausführen:
  - Starten Sie die Terminal Anwendung.
  - Geben **Sie**ein, unddrückenSiedieEingabe`xcode-select --install` Taste:

  ```bash
  Europa:~ kmullins$ xcode-select --install
  ```

  - Sie werden aufgefordert, die Befehlszeilen Tools zu installieren. Klicken Sie auf die Schaltfläche **Installieren** : [![](walkthrough-images/xcode01.png "Installieren der Befehlszeilen Tools")](walkthrough-images/xcode01.png#lightbox)

  - Die Tools werden von Apple-Servern heruntergeladen und installiert: [![](walkthrough-images/xcode02.png "Herunterladen der Tools")](walkthrough-images/xcode02.png#lightbox)

- **Downloads für Apple-Entwickler** : das Paket mit den Befehlszeilen Tools ist auf der Webseite [Downloads for Apple Developers](https://developer.apple.com/downloads/index.action) verfügbar. Melden Sie sich mit Ihrer Apple-ID an, suchen Sie nach den Befehlszeilen Tools, und laden Sie Sie herunter: [![](walkthrough-images/xcode03.png "Suchen der Befehlszeilen Tools")](walkthrough-images/xcode03.png#lightbox)

Wenn die Befehlszeilen Tools installiert sind, können wir mit der exemplarischen Vorgehensweise fortfahren.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise werden die folgenden Schritte behandelt:

- **[Erstellen einer statischen Bibliothek](#Creating_A_Static_Library)** : dieser Schritt umfasst das Erstellen einer statischen Bibliothek des Ziel-C-Codes von **infcolorpicker** . Die statische Bibliothek verfügt über die `.a` Dateierweiterung und wird in die .NET-Assembly des Bibliotheks Projekts eingebettet.
- **[Erstellen eines xamarin. IOS-Bindungs Projekts](#Create_a_Xamarin.iOS_Binding_Project)** : Sobald wir über eine statische Bibliothek verfügen, verwenden wir diese, um ein xamarin. IOS-Bindungs Projekt zu erstellen. Das Bindungs Projekt besteht aus der statischen Bibliothek, die wir soeben erstellt haben, und Metadaten C# in Form von Code, in dem erläutert wird, wie die Ziel-C-API verwendet werden kann. Diese Metadaten werden im Allgemeinen als API-Definitionen bezeichnet. Wir verwenden **[Ziel-Sharpie](#Using_Objective_Sharpie)** , um uns bei der Erstellung der API-Definitionen zu unterstützen.
- **[Normalisieren der API-Definitionen](#Normalize_the_API_Definitions)** : Ziel-Sharpie führt zu einer großen Aufgabe, uns zu unterstützen, aber es kann nicht alles durchführen. Wir besprechen einige Änderungen, die wir an den API-Definitionen vornehmen müssen, bevor Sie verwendet werden können.
- **[Verwenden der Bindungs Bibliothek](#Using_the_Binding)** : Schließlich erstellen wir eine xamarin. IOS-Anwendung, um zu zeigen, wie das neu erstellte Bindungs Projekt verwendet werden kann.

Nachdem wir nun verstanden haben, welche Schritte ausgeführt werden, können wir uns mit dem Rest der exemplarischen Vorgehensweise beschäftigen.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Erstellen einer statischen Bibliothek

Wenn wir den Code für infcolorpicker in GitHub untersuchen:

[![](walkthrough-images/image02.png "Überprüfen des Codes für infcolorpicker in GitHub")](walkthrough-images/image02.png#lightbox)

Die folgenden drei Verzeichnisse können im Projekt angezeigt werden:

- **Infcolorpicker** : dieses Verzeichnis enthält den Ziel-C-Code für das Projekt.
- **Pickersamplepad** : dieses Verzeichnis enthält ein Beispiel-iPad-Projekt.
- **Pickersamplephone** : dieses Verzeichnis enthält ein Beispiel-iPhone-Projekt.

Laden wir das infcolorpicker-Projekt von [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) herunter, und entpacken Sie es in das Verzeichnis unserer Wahl. Wenn Sie das Xcode-Ziel `PickerSamplePhone` für das Projekt öffnen, wird die folgende Projektstruktur im Xcode-Navigator angezeigt:

[![](walkthrough-images/image03.png "Die Projektstruktur im Xcode-Navigator.")](walkthrough-images/image03.png#lightbox)

Dieses Projekt erreicht die Wiederverwendung von Code durch direktes Hinzufügen des infcolorpicker-Quellcodes (im roten Feld) zu jedem Beispiel Projekt. Der Code für das Beispiel Projekt befindet sich im blauen Feld. Da uns dieses bestimmte Projekt keine statische Bibliothek bereitstellt, ist es erforderlich, ein Xcode-Projekt zu erstellen, um die statische Bibliothek zu kompilieren.

Der erste Schritt besteht darin, dass wir den infocolorpicker-Quellcode der statischen Bibliothek hinzufügen. Um dies zu erreichen, gehen Sie folgendermaßen vor:

1. Starten Sie Xcode.
2. Wählen Sie im Menü **Datei** die Option **Neues** > **Projekt...** aus:

    [![](walkthrough-images/image04.png "Starten eines neuen Projekts")](walkthrough-images/image04.png#lightbox)
3. Wählen Sie **Framework & Bibliothek**aus, und klicken Sie dann **auf die Schalt** Fläche **weiter** :

    [![](walkthrough-images/image05.png "Vorlage für die statische Cocoa-Eingabe Vorlage auswählen")](walkthrough-images/image05.png#lightbox)

4. Geben `InfColorPicker` Sie als **Projektnamen** ein, und klicken Sie auf die Schaltfläche **weiter** :

    [![](walkthrough-images/image06.png "Geben Sie infcolorpicker als Projektnamen ein.")](walkthrough-images/image06.png#lightbox)
5. Wählen Sie einen Speicherort für das Projekt aus, und klicken Sie auf die Schaltfläche **OK** .
6. Nun müssen wir die Quelle aus dem Projekt infcolorpicker dem statischen Bibliotheksprojekt hinzufügen. Da die Datei " **infcolorpicker. h** " in der statischen Bibliothek bereits vorhanden ist (standardmäßig), lässt Xcode nicht zu, dass Sie überschrieben werden kann. Navigieren Sie aus dem **Finder**in das ursprüngliche Projekt, das Sie aus GitHub entzippt haben, zum infcolorpicker-Quellcode, kopieren Sie alle infcolorpicker-Dateien, und fügen Sie Sie in unser neues statisches Bibliotheksprojekt ein:

    [![](walkthrough-images/image12.png "Alle infcolorpicker-Dateien kopieren")](walkthrough-images/image12.png#lightbox)

7. Kehren Sie zu Xcode zurück, klicken Sie mit der rechten Maustaste auf den Ordner **infcolorpicker** , und wählen Sie dann **Dateien zu "infcolorpicker" hinzufügen**aus:

    [![](walkthrough-images/image08.png "Hinzufügen von Dateien")](walkthrough-images/image08.png#lightbox)

8. Navigieren Sie im Dialogfeld "Dateien hinzufügen" zu den von uns soeben kopierten Quell Code Dateien für infcolorpicker, wählen Sie alle aus, und klicken Sie auf die Schaltfläche **Hinzufügen** :

    [![](walkthrough-images/image09.png "Alle auswählen und auf die Schaltfläche \"hinzufügen\" klicken")](walkthrough-images/image09.png#lightbox)

9. Der Quellcode wird in unser Projekt kopiert:

    [![](walkthrough-images/image10.png "Der Quellcode wird in das Projekt kopiert.")](walkthrough-images/image10.png#lightbox)

10. Wählen Sie im Xcode-Projekt Navigator die Datei **infcolorpicker. m** aus, und kommentieren Sie die letzten beiden Zeilen aus (aufgrund der Art und Weise, in der diese Bibliothek geschrieben wurde, wird diese Datei nicht verwendet):

    [![](walkthrough-images/image14.png "Bearbeiten der Datei \"infcolorpicker. m\"")](walkthrough-images/image14.png#lightbox)

11. Wir müssen jetzt überprüfen, ob für die Bibliothek Frameworks erforderlich sind. Sie finden diese Informationen entweder in der Infodatei oder indem Sie eines der bereitgestellten Beispiel Projekte öffnen. In diesem Beispiel `Foundation.framework`werden `UIKit.framework`, und `CoreGraphics.framework` hinzugefügt.

12. Wählen Sie das **Ziel > buildphasen** aus, und erweitern Sie den Abschnitt **Link Binary with Libraries** :

    [![](walkthrough-images/image16b.png "Erweitern Sie den Abschnitt Link Binary with Libraries.")](walkthrough-images/image16b.png#lightbox)

13. Verwenden Sie **+** die Schaltfläche, um das Dialogfeld zu öffnen, in dem Sie die oben aufgeführten erforderlichen Frames hinzufügen können:

    [![](walkthrough-images/image16c.png "Fügen Sie die oben aufgeführten erforderlichen Frame-Frameworks hinzu.")](walkthrough-images/image16c.png#lightbox)

14. Der Abschnitt **Link Binary with Libraries** sollte nun wie in der folgenden Abbildung aussehen:

    [![](walkthrough-images/image16d.png "Der Link Binary with Libraries-Abschnitt")](walkthrough-images/image16d.png#lightbox)

An diesem Punkt sind wir geschlossen, aber wir sind noch nicht fertig. Die statische Bibliothek wurde erstellt, aber wir müssen Sie erstellen, um eine FAT-Binärdatei zu erstellen, die alle erforderlichen Architekturen für IOS-Geräte und IOS-Simulator enthält.

### <a name="creating-a-fat-binary"></a>Erstellen einer FAT-Binärdatei

Alle IOS-Geräte verfügen über Prozessoren, die von der ARM-Architektur betrieben werden und mit der Zeit entwickelt wurden Jede neue Architektur hat neue Anweisungen und andere Verbesserungen hinzugefügt, wobei die Abwärtskompatibilität weiterhin gewahrt bleibt. IOS-Geräte verfügen über ARMv6, armv7, armv7s, arm64-Anweisungs Sätze – Obwohl [ARMv6 nicht mehr verwendet wird](~/ios/deploy-test/compiling-for-different-devices.md). Der IOS-Simulator wird nicht von Arm unterversorgt und ist stattdessen ein x86-und x86_64-gestützter Simulator. Dies bedeutet, dass Bibliotheken für jeden Anweisungs Satz bereitgestellt werden müssen.

Eine FAT-Bibliothek `.a` ist eine Datei, die alle unterstützten Architekturen enthält.

Das Erstellen einer FAT-Binärdatei ist ein dreistufiger Prozess:

- Kompilieren Sie eine ARM 7-& ARM64 Version der statischen Bibliothek.
- Kompilieren Sie eine x86-und x84_64-Version der statischen Bibliothek.
- Verwenden Sie `lipo` das-Befehlszeilen Tool, um die beiden statischen Bibliotheken zu einer zu kombinieren.

Diese drei Schritte sind zwar recht unkompliziert, es kann jedoch erforderlich sein, Sie in Zukunft zu wiederholen, wenn die Ziel-C-Bibliothek Updates empfängt oder wenn wir Fehlerbehebungen benötigen. Wenn Sie diese Schritte automatisieren, wird die zukünftige Wartung und Unterstützung des IOS-Bindungs Projekts vereinfacht.

Es stehen zahlreiche Tools zur Verfügung, mit denen Sie Aufgaben wie Shellskripts, [Rake](http://rake.rubyforge.org/), [xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/)und [make](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)automatisieren können. Wenn die Xcode-Befehlszeilen Tools installiert sind `make` , wird ebenfalls installiert. Dies ist das Buildsystem, das für diese exemplarische Vorgehensweise verwendet wird. Hier ist ein **Makefile** , mit dem Sie eine freigegebene Bibliothek für mehrere Architekturen erstellen können, die auf einem IOS-Gerät und dem Simulator für jede Bibliothek funktioniert:

<!--markdownlint-disable MD010 -->
```makefile
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
<!--markdownlint-enable MD010 -->

Geben Sie die **Makefile** -Befehle im nur-Text-Editor Ihrer Wahl ein, und aktualisieren Sie die Abschnitte mit **Ihrem Projektnamen** mit dem Namen Ihres Projekts. Es ist auch wichtig, sicherzustellen, dass Sie die obigen Anweisungen genau einfügen, wobei die Registerkarten in den Anweisungen beibehalten werden.

Speichern Sie die Datei mit dem Namen **Makefile** am gleichen Speicherort wie die statische Bibliothek infcolorpicker Xcode, die wir oben erstellt haben:

[![](walkthrough-images/lib00.png "Speichern Sie die Datei mit dem Namen Makefile.")](walkthrough-images/lib00.png#lightbox)

Öffnen Sie die Terminal Anwendung auf Ihrem Mac, und navigieren Sie zum Speicherort Ihrer Makefile. Geben Sie in das Terminal ein, drücken Sie die EINGABETASTE, und das Makefile wird ausgeführt: `make`

[![](walkthrough-images/lib01.png "Beispiel für Makefile-Ausgabe")](walkthrough-images/lib01.png#lightbox)

Wenn Sie make ausführen, wird ein großer Text durch Scrollen von angezeigt. Wenn alles ordnungsgemäß funktioniert hat, sehen Sie, dass die Wörter **Build erfolgreich** `libInfColorPicker-armv7.a`sind `libInfColorPickerSDK.a` und `libInfColorPicker-i386.a` dass Dateien an denselben Speicherort wie das **Makefile**kopiert werden:

[![](walkthrough-images/lib02.png "Die vom Makefile generierten Dateien libinf ColorPicker-armv7. a, libinf ColorPicker-i386. a und libinf colorpickersdk. a")](walkthrough-images/lib02.png#lightbox)

Mit dem folgenden Befehl können Sie die Architekturen in der FAT-Binärdatei bestätigen:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Folgendes sollte angezeigt werden:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

An diesem Punkt haben wir den ersten Schritt der IOS-Bindung abgeschlossen, indem wir mithilfe von Xcode und den Xcode-Befehlszeilen Tools `make` und `lipo`eine statische Bibliothek erstellt haben. Fahren Sie mit dem nächsten Schritt fort, und verwenden Sie "Target **-Sharpie** ", um die Erstellung der API-Bindungen für uns zu automatisieren.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Erstellen eines xamarin. IOS-Bindungs Projekts

Bevor wir "Target **-Sharpie** " verwenden können, um den Bindungsprozess zu automatisieren, muss ein xamarin. IOS-Bindungs Projekt erstellt werden, um die API-Definitionen zu beherbergen (die wir zum Erstellen von "Target **-Sharpie** " verwenden, um uns zu helfen) und die C# Bindung für uns zu erstellen.

Führen Sie die folgenden Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie Visual Studio für Mac.
1. Wählen Sie im Menü Datei die Option **neue** > Projekt **Mappe** **...** aus:

    ![](walkthrough-images/bind01.png "Starten einer neuen Projekt Mappe")

1. Wählen Sie im Dialogfeld neue Projekt Mappe die Option **Bibliothek** > **IOS-Bindungs Projekt**:

    ![](walkthrough-images/bind02.png "IOS-Bindungs Projekt auswählen")

1. Klicken Sie auf die Schaltfläche **weiter** .

1. Geben Sie "infcolorpickerbinding" als **Projektnamen** ein, und klicken Sie auf die Schaltfläche **Erstellen** , um die Projekt Mappe zu erstellen:

    ![](walkthrough-images/bind02a.png "Geben Sie infcolorpickerbinding als Projektnamen ein.")

Die Projekt Mappe wird erstellt, und es werden zwei Standard Dateien eingeschlossen:

![](walkthrough-images/bind03.png "Die Lösungs Struktur im Projektmappen-Explorer")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Starten Sie Visual Studio.

1. Wählen Sie im Menü **Datei** die Option **Neues** > **Projekt...** aus:

    ![Starten eines neuen Projekts](walkthrough-images/bind01vs.png "Starten eines neuen Projekts")

1. Wählen Sie im Dialogfeld Neues Projekt die **Option C# Visual > iPhone & iPad > IOS-Bindungs Bibliothek (xamarin)** aus:

    [![IOS-Bindungs Bibliothek auswählen](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. Geben Sie "infcolorpickerbinding" als **Namen** ein, und klicken Sie auf die Schaltfläche **OK** , um die Projekt Mappe zu erstellen.

Die Projekt Mappe wird erstellt, und es werden zwei Standard Dateien eingeschlossen:

![](walkthrough-images/bind03vs.png "Die Lösungs Struktur im Projektmappen-Explorer")

-----

- **ApiDefinition.cs** : Diese Datei enthält die Verträge, die definieren, wie die Ziel-C-APIs umschließt C#werden.
- **Structs.cs** : Diese Datei enthält alle Strukturen oder Enumerationswerte, die von den Schnittstellen und Delegaten benötigt werden.

Wir arbeiten mit diesen beiden Dateien später in der exemplarischen Vorgehensweise. Zuerst müssen wir die infcolorpicker-Bibliothek zum Bindungs Projekt hinzufügen.

### <a name="including-the-static-library-in-the-binding-project"></a>Einschließen der statischen Bibliothek in das Bindungs Projekt

Wir haben jetzt das Basis Bindungs Projekt vorbereitet, und wir müssen die zuvor erstellte FAT-Datei für die-Bibliothek der **infcolorpicker** -Bibliothek hinzufügen.

Gehen Sie folgendermaßen vor, um die Bibliothek hinzuzufügen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Klicken Sie im Lösungspad mit der rechten Maustaste auf den Ordner **native References** , und wählen Sie **native Verweise hinzufügen**aus:

    ![](walkthrough-images/bind04a.png "Native Verweise hinzufügen")

1. Navigieren Sie zur FAT-Binärdatei, die`libInfColorPickerSDK.a`wir zuvor erstellt haben (), und drücken Sie die Schaltfläche **Öffnen**

    ![](walkthrough-images/bind05.png "Wählen Sie die Datei \"libinf colorpickersdk. a\" aus.")
1. Die Datei wird in das Projekt eingeschlossen:

    ![](walkthrough-images/bind04.png "Einschließen einer Datei")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Kopieren Sie `libInfColorPickerSDK.a` die von Ihrem Mac-buildhost, und fügen Sie Sie in das Bindungs Projekt ein.

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **> vorhandenes Element hinzufügen...** :

    ![](walkthrough-images/bind04vs.png "Hinzufügen einer vorhandenen Datei")

1. Navigieren Sie zum `libInfColorPickerSDK.a` , und klicken Sie auf die Schaltfläche **Hinzufügen** :

    ![](walkthrough-images/bind05vs.png "Hinzufügen von libinf colorpickersdk. a")

1. Die Datei wird in das Projekt eingefügt.

-----

Wenn die Datei " **. a** " dem Projekt hinzugefügt wird, legt xamarin. IOS automatisch den Buildvorgang der Datei auf **objcbindingnativelibrary**fest und erstellt eine spezielle Datei `libInfColorPickerSDK.linkwith.cs` **mit dem** Namen.


Diese Datei enthält das `LinkWith` Attribut, das xamarin. IOS anweist, wie die soeben hinzugefügte statische Bibliothek behandelt werden soll. Der Inhalt dieser Datei wird im folgenden Code Ausschnitt gezeigt:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

Das `LinkWith` -Attribut identifiziert die statische Bibliothek für das Projekt und einige wichtige Linker-Flags.


Als nächstes müssen wir die API-Definitionen für das infcolorpicker-Projekt erstellen. Im Rahmen dieser exemplarischen Vorgehensweise verwenden wir das Ziel "Sharpie", um die Datei " **ApiDefinition.cs**" zu generieren.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Verwenden von Ziel-Sharpie

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)


Ziel-Sharpie ist ein von xamarin bereitgestelltes Befehlszeilen Tool, das beim Erstellen der Definitionen helfen kann, die erforderlich sind, um eine Ziel-C C#-Bibliothek von Drittanbietern an zu binden. In diesem Abschnitt verwenden wir den Ziel-Sharpie, um die anfängliche **ApiDefinition.cs** für das infcolorpicker-Projekt zu erstellen.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


Ziel-Sharpie ist ein von xamarin bereitgestelltes Befehlszeilen Tool, das beim Erstellen der Definitionen helfen kann, die erforderlich sind, um eine Ziel-C C#-Bibliothek von Drittanbietern an zu binden. In diesem Abschnitt verwenden wir den Ziel-Sharpie auf dem **Mac-buildhost** , um die anfängliche **ApiDefinition.cs** für das infcolorpicker-Projekt zu erstellen.


-----

Zum Einstieg laden wir die Ziel-Sharpie-Installationsdatei herunter, wie in diesem [Handbuch](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)ausführlich beschrieben. Führen Sie das Installationsprogramm aus, und befolgen Sie alle Aufforderungen auf dem Bildschirm vom Installations-Assistenten, um Ziel-Sharpie auf unserem Entwicklungs Computer zu installieren.

Nachdem Sie das Ziel "Sharpie" erfolgreich installiert haben, starten Sie die Terminal-app, und geben Sie den folgenden Befehl ein, um Hilfe zu allen Tools zu erhalten, die zur Unterstützung der Bindung bereitstellt:

```bash
sharpie -help
```

Wenn Sie den obigen Befehl ausführen, wird die folgende Ausgabe generiert:

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

Im Rahmen dieser exemplarischen Vorgehensweise verwenden wir die folgenden Ziel-Sharpie-Tools:

- **Xcode** : Diese Tools enthalten Informationen über unsere aktuelle Xcode-Installation und die Versionen von IOS-und Mac-APIs, die installiert wurden. Diese Informationen werden später verwendet, wenn wir unsere Bindungen generieren.
- **Bind** : Wir verwenden dieses Tool, um die **h** -Dateien im Projekt infcolorpicker in die anfänglichen **ApiDefinition.cs** -und **StructsAndEnums.cs** -Dateien zu analysieren.

Um Hilfe zu einem bestimmten Ziel-Sharpie-Tool zu erhalten, geben Sie den Namen des `-help` Tools und die Option ein. Beispielsweise `sharpie xcode -help` gibt die folgende Ausgabe zurück:

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

Bevor wir den Bindungsprozess starten können, müssen wir Informationen zu unseren aktuell installierten sdden erhalten, indem wir den folgenden Befehl in das Terminal `sharpie xcode -sdks`eingeben:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Im obigen Beispiel sehen wir, dass das SDK auf dem `iphoneos9.3` Computer installiert ist. Nachdem diese Informationen vorhanden sind, können wir die infcolorpicker-Projekt `.h` Dateien in die anfängliche **ApiDefinition.cs** und das infcolorpicker- `StructsAndEnums.cs` Projekt analysieren.

Geben Sie den folgenden Befehl in der Terminal-app ein:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Dabei steht für den vollständigen Pfad zu dem Verzeichnis, in dem sich die Projektdatei " **infcolorpicker** " auf dem Computer befindet, und [iPhone-OS] ist das IOS SDK, das wir installiert haben `sharpie xcode -sdks` , wie im Befehl angegeben. `[full-path-to-project]` Beachten Sie, dass in diesem Beispiel  **\*. h** als Parameter übergeben wurde, der *alle* Header Dateien in diesem Verzeichnis enthält. Dies ist normalerweise nicht der Fall, stattdessen sollten Sie die Header Dateien sorgfältig durchlesen, um die **. h** -Datei der obersten Ebene zu finden. eine Datei, die auf alle anderen relevanten Dateien verweist und diese einfach an den Ziel-Sharpie übergibt.

Die folgende [Ausgabe](walkthrough-images/os05.png) wird im Terminal generiert:

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

Und die **InfColorPicker.enums.cs** -und **InfColorPicker.cs** -Dateien werden in unserem Verzeichnis erstellt:

[![](walkthrough-images/os06.png "Die Dateien InfColorPicker.enums.cs und InfColorPicker.cs")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)


Öffnen Sie beide Dateien im Bindungs Projekt, das wir oben erstellt haben. Kopieren Sie den Inhalt der **Datei InfColorPicker.cs** , und fügen Sie ihn in die Datei **ApiDefinition.cs** ein. `namespace ...` ersetzen Sie dabei den vorhandenen Codeblock durch den Inhalt der **InfColorPicker.cs** -Datei (indem Sie die `using` Anweisungen überschreiben. intakt):

![](walkthrough-images/os07.png "Die infcolorpickercontrollerdelegatdatei")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


Öffnen Sie beide Dateien im Bindungs Projekt, das wir oben erstellt haben. Kopieren Sie den Inhalt der Datei **InfColorPicker.cs** (auf dem **Mac-buildhost**), und fügen Sie ihn in die Datei ApiDefinition.cs `namespace ...` ein. ersetzen Sie dabei den vorhandenen Codeblock durch den Inhalt der **InfColorPicker.cs** -Datei ( die `using` Anweisungen bleiben unverändert).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>Normalisieren der API-Definitionen

Bei Ziel-Sharpie tritt mitunter ein `Delegates`Problem auf. Daher müssen wir die Definition `InfColorPickerControllerDelegate` der Schnittstelle ändern und die `[Protocol, Model]` Zeile durch Folgendes ersetzen:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```

So sieht die Definition wie folgt aus:

[![](walkthrough-images/os11.png "Die Definition")](walkthrough-images/os11.png#lightbox)

Als nächstes führen wir das gleiche mit dem Inhalt der `InfColorPicker.enums.cs` Datei aus, kopieren und Einfügen in die `StructsAndEnums.cs` Datei, sodass die `using` Anweisungen intakt bleiben:

[![](walkthrough-images/os09.png "Der Inhalt der Datei \"StructsAndEnums.cs\".")](walkthrough-images/os09.png#lightbox)

Sie können auch feststellen, dass der Ziel-Sharpie die Bindung mit `[Verify]` Attributen versehen hat. Diese Attribute geben an, dass Sie sicherstellen sollten, dass Ziel-Sharpie das richtige ist, indem Sie die Bindung mit der ursprünglichen c/Ziel-c-Deklaration vergleichen (die in einem Kommentar oberhalb der gebundenen Deklaration bereitgestellt wird). Nachdem Sie die Bindungen überprüft haben, sollten Sie das Verify-Attribut entfernen. Weitere Informationen finden Sie im Leitfaden zur [Überprüfung](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) .

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)


An diesem Punkt sollte das Bindungs Projekt abgeschlossen und bereit für die Erstellung sein. Erstellen wir nun das Bindungs Projekt, und stellen Sie sicher, dass keine Fehler aufgetreten sind:

[Erstellen Sie das Bindungs Projekt, und stellen Sie sicher, dass keine Fehler vorliegen.](walkthrough-images/os12.png)


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


An diesem Punkt sollte das Bindungs Projekt abgeschlossen und bereit für die Erstellung sein. Erstellen wir nun das Bindungs Projekt, und stellen Sie sicher, dass keine Fehler aufgetreten sind.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Verwenden der Bindung

Gehen Sie folgendermaßen vor, um eine Beispiel-iPhone-Anwendung zur Verwendung der oben erstellten IOS-Bindungs Bibliothek zu erstellen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. **Erstellen eines xamarin. IOS-Projekts** : Fügen Sie der Projekt Mappe ein neues xamarin. IOS-Projekt mit dem Namen " **infcolorpickersample** " hinzu, wie in den folgenden Screenshots gezeigt:

    ![](walkthrough-images/use01.png "Hinzufügen einer einzelnen Ansichts-App")

    ![](walkthrough-images/use01a.png "Festlegen des Bezeichners")

1. **Fügen Sie dem Bindungs Projekt einen Verweis hinzu** : Aktualisieren Sie das Projekt **infcolorpickersample** , sodass es einen Verweis auf das Projekt **infcolorpickerbinding** hat:

    ![](walkthrough-images/use02.png "Hinzufügen eines Verweises auf das Bindungs Projekt")

1. **Erstellen der iPhone-Benutzeroberfläche** : Doppelklicken Sie auf die Datei " **mainstoryboard. Storyboard** " im Projekt " **infcolorpickersample** ", um Sie im IOS-Designer zu bearbeiten. Fügen Sie der Ansicht eine **Schaltfläche** hinzu, `ChangeColorButton`und nennen Sie Sie, wie im folgenden dargestellt:

    ![](walkthrough-images/use03.png "Hinzufügen einer Schaltfläche zur Ansicht")

1. **Fügen Sie infcolorpickerview. XIb hinzu** : die infcolorpicker-Ziel-C-Bibliothek enthält eine **XIb** -Datei. Xamarin. IOS schließt this **. XIb** nicht in das Bindungs Projekt ein, was zu Laufzeitfehlern in der Beispielanwendung führt. Um dieses Problem zu umgehen, fügen Sie dem xamarin. IOS-Projekt die **XIb** -Datei hinzu. Wählen Sie das xamarin. IOS-Projekt aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Hinzufügen > Dateien**hinzufügen aus. Fügen Sie die **XIb** -Datei wie im folgenden Screenshot gezeigt hinzu:

    ![](walkthrough-images/use04.png "Fügen Sie infcolorpickerview. XIb hinzu.")

1. Wenn Sie gefragt werden, kopieren Sie die **XIb** -Datei in das Projekt.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Xamarin. IOS-Projekt erstellen** : Fügen Sie ein neues xamarin. IOS-Projekt mit dem Namen " **infcolorpickersample** " mithilfe der Vorlage für die **Einzelansicht-App** hinzu

    [![IOS-App-Projekt (xamarin)](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![Vorlage auswählen](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **Fügen Sie dem Bindungs Projekt einen Verweis hinzu** : Aktualisieren Sie das Projekt **infcolorpickersample** , sodass es einen Verweis auf das Projekt **infcolorpickerbinding** hat:

    ![](walkthrough-images/use02vs.png "Verweis zum Bindungs Projekt hinzufügen")

1. **Erstellen der iPhone-Benutzeroberfläche** : Doppelklicken Sie auf die Datei " **mainstoryboard. Storyboard** " im Projekt " **infcolorpickersample** ", um Sie im IOS-Designer zu bearbeiten. Fügen Sie der Ansicht eine **Schaltfläche** hinzu, `ChangeColorButton`und nennen Sie Sie, wie im folgenden dargestellt:

    ![](walkthrough-images/use03vs.png "Erstellen der iPhone-Benutzeroberfläche")

1. **Fügen Sie infcolorpickerview. XIb hinzu** : die infcolorpicker-Ziel-C-Bibliothek enthält eine **XIb** -Datei. Xamarin. IOS schließt this **. XIb** nicht in das Bindungs Projekt ein, was zu Laufzeitfehlern in der Beispielanwendung führt. Um dieses Problem zu umgehen, fügen Sie dem xamarin. IOS-Projekt auf dem **Mac-buildhost**die **XIb** -Datei hinzu. Wählen Sie das xamarin. IOS-Projekt aus, klicken Sie mit der rechten Maustaste, wählen Sie**Vorhandenes Element** **Hinzufügen** > aus, und fügen Sie die **XIb** -Datei hinzu.

-----

Im nächsten Schritt betrachten wir die Protokolle in Ziel-C und die Art und Weise, wie wir Sie in der C# Bindung und im Code behandeln.

### <a name="protocols-and-xamarinios"></a>Protokolle und xamarin. IOS

In Ziel-C definiert ein Protokoll Methoden (oder Meldungen), die unter bestimmten Umständen verwendet werden können. Konzeptionell ähneln Sie den Schnittstellen in C#. Ein wichtiger Unterschied zwischen einem Ziel-C-Protokoll C# und einer-Schnittstelle besteht darin, dass Protokolle optionale Methoden aufweisen können, die von einer Klasse nicht implementiert werden müssen. Ziel-C verwendet das @optional -Schlüsselwort, um anzugeben, welche Methoden optional sind. Weitere Informationen zu Protokollen finden Sie unter [Ereignisse, Protokolle und](~/ios/app-fundamentals/delegates-protocols-and-events.md)Delegaten.

**Infcolorpickercontroller** verfügt über ein solches Protokoll, wie im folgenden Code Ausschnitt gezeigt:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Dieses Protokoll wird von **infcolorpickercontroller** verwendet, um Clients darüber zu informieren, dass der Benutzer eine neue Farbe ausgewählt hat und dass **infcolorpickercontroller** fertiggestellt ist. Ziel-Sharpie hat dieses Protokoll zugeordnet, wie im folgenden Code Ausschnitt gezeigt:

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

Wenn die Bindungs Bibliothek kompiliert wird, erstellt xamarin. IOS eine abstrakte Basisklasse mit dem `InfColorPickerControllerDelegate`Namen, die diese Schnittstelle mit virtuellen Methoden implementiert.

Es gibt zwei Möglichkeiten, wie Sie diese Schnittstelle in einer xamarin. IOS-Anwendung implementieren können:

- **Starker** Delegat: die Verwendung eines starken Delegaten C# umfasst das Erstellen einer `InfColorPickerControllerDelegate` Klasse, die die entsprechenden Methoden Unterklassen und überschreibt. **Infcolorpickercontroller** verwendet eine Instanz dieser Klasse, um mit den Clients zu kommunizieren.
- **Schwacher** Delegat: bei einem schwachen Delegaten handelt es sich um eine etwas andere Technik, die das Erstellen einer öffentlichen `InfColorPickerSampleViewController`Methode für eine Klasse (z. b `InfColorPickerDelegate` .) und `Export` das anschließende verfügbar machen dieser Methode für das Protokoll über ein Attribut umfasst

Starke Delegaten bieten IntelliSense, Typsicherheit und eine bessere Kapselung. Aus diesen Gründen sollten Sie anstelle eines schwachen Delegaten starke Delegaten verwenden.

In dieser exemplarischen Vorgehensweise werden beide Verfahren erläutert: zuerst wird ein starker Delegat implementiert, und anschließend wird erläutert, wie ein schwacher Delegat implementiert wird.

### <a name="implementing-a-strong-delegate"></a>Implementieren eines starken Delegaten

Beenden Sie die xamarin. IOS-Anwendung, indem Sie einen starken Delegaten `colorPickerControllerDidFinish:` verwenden, um auf die Meldung zu reagieren:

**Subclass infcolorpickercontrollerdelegat** -fügt dem Projekt eine neue Klasse mit `ColorSelectedDelegate`dem Namen hinzu. Bearbeiten Sie die-Klasse, sodass Sie den folgenden Code enthält:

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

Xamarin. IOS bindet den Ziel-C-Delegaten, indem eine abstrakte Basisklasse `InfColorPickerControllerDelegate`mit dem Namen erstellt wird. Unterklasse dieses Typs, und über `ColorPickerControllerDidFinish` schreiben Sie die-Methode, um `ResultColor` auf den `InfColorPickerController`Wert der-Eigenschaft von zuzugreifen.

**Erstellen einer Instanz von colorselecteddelegat** : für den Ereignishandler wird eine Instanz des `ColorSelectedDelegate` Typs benötigt, den wir im vorherigen Schritt erstellt haben. Bearbeiten Sie die `InfColorPickerSampleViewController` -Klasse, und fügen Sie der-Klasse die folgende Instanzvariable hinzu:

```csharp
ColorSelectedDelegate selector;
```

**Initialisieren Sie die colorselecteddelegatvariable** . um `selector` sicherzustellen, dass eine gültige-Instanz `ViewDidLoad` ist `ViewController` , aktualisieren Sie die-Methode in, um dem folgenden Code Ausschnitt zu entsprechen:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();
  ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
  selector = new ColorSelectedDelegate (this);
}
```

**Implementieren Sie die Methode "shandtouchupinsidewithstrongdelegat** -Next implementieren Sie den Ereignishandler für", wenn der Benutzer **colorchangebutton**berührt. Bearbeiten `ViewController`Sie, und fügen Sie die folgende Methode hinzu:

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

Zuerst wird eine Instanz von `InfColorPickerController` über eine statische Methode abgerufen, und diese Instanz wird über die-Eigenschaft `InfColorPickerController.Delegate`auf unseren starken Delegaten aufmerksam gemacht. Diese Eigenschaft wurde von Ziel-Sharpie automatisch für uns generiert. Schließlich wird aufgerufen `PresentModallyOverViewController` , um die Ansicht `InfColorPickerSampleViewController.xib` anzuzeigen, sodass der Benutzer eine Farbe auswählen kann.

**Ausführen der Anwendung** : an dieser Stelle wird der gesamte Code ausgeführt. Wenn Sie die Anwendung ausführen, sollten Sie in der Lage sein, die Hintergrundfarbe `InfColorColorPickerSampleView` der zu ändern, wie in den folgenden Screenshots gezeigt:

[![](walkthrough-images/run01.png "Ausführen der Anwendung")](walkthrough-images/run01.png#lightbox)

Herzlichen Glückwunsch! An diesem Punkt haben Sie eine Ziel-C-Bibliothek erfolgreich erstellt und für die Verwendung in einer xamarin. IOS-Anwendung gebunden. Als Nächstes erfahren Sie mehr über die Verwendung von schwachen Delegaten.

### <a name="implementing-a-weak-delegate"></a>Implementieren eines schwachen Delegaten

Anstatt eine Klasse zu Unterklassen, die an das Ziel-C-Protokoll für einen bestimmten Delegaten gebunden ist, können Sie mit xamarin. IOS auch die Protokoll Methoden in jeder `NSObject`Klasse implementieren, die von abgeleitet `ExportAttribute`ist, und die Methoden mit dem versehen. Bereitstellen der entsprechenden Selektoren. Wenn Sie diesen Ansatz verwenden, weisen Sie der `WeakDelegate` -Eigenschaft anstelle `Delegate` der-Eigenschaft eine Instanz der-Klasse zu. Ein schwacher Delegat bietet Ihnen die Flexibilität, ihre Delegatklasse in eine andere Vererbungs Hierarchie zu bringen. Sehen wir uns an, wie Sie einen schwachen Delegaten in unserer xamarin. IOS-Anwendung implementieren und verwenden.

**Create Event Handler for touchupinside** : Erstellen Sie einen neuen Ereignishandler für das `TouchUpInside` -Ereignis der Schaltfläche Hintergrundfarbe ändern. Dieser Handler nimmt dieselbe Rolle wie der `HandleTouchUpInsideWithStrongDelegate` Handler, den wir im vorherigen Abschnitt erstellt haben, verwendet aber einen schwachen Delegaten anstelle eines starken Delegaten. Bearbeiten Sie die `ViewController`-Klasse, und fügen Sie die folgende Methode hinzu:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```

**ViewDidLoad aktualisieren** : Wir müssen ändern `ViewDidLoad` , damit der soeben erstellte Ereignishandler verwendet wird. Bearbeiten `ViewController` und ändern `ViewDidLoad` Sie den folgenden Code Ausschnitt:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Behandeln Sie colorpickercontrollerdidfinish: Meldung** : Wenn der `ViewController` abgeschlossen ist, sendet IOS die Nachricht `colorPickerControllerDidFinish:` an den `WeakDelegate`. Wir müssen eine C# Methode erstellen, die diese Nachricht verarbeiten kann. Zu diesem Zweck erstellen wir eine C# -Methode und schmücken Sie dann mit dem `ExportAttribute`. Bearbeiten `ViewController`Sie, und fügen Sie der-Klasse die folgende Methode hinzu:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Führen Sie die Anwendung aus. Es sollte sich nun genau wie zuvor Verhalten, aber es verwendet einen schwachen Delegaten anstelle des starken Delegaten. An diesem Punkt haben Sie diese exemplarische Vorgehensweise erfolgreich abgeschlossen. Sie sollten nun wissen, wie ein xamarin. IOS-Bindungs Projekt erstellt und genutzt werden kann.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Erstellen und Verwenden eines xamarin. IOS-Bindungs Projekts erläutert. Zuerst wurde erläutert, wie eine vorhandene Ziel-C-Bibliothek in eine statische Bibliothek kompiliert wird. Anschließend haben wir erläutert, wie Sie ein xamarin. IOS-Bindungs Projekt erstellen und die API-Definitionen für die Ziel-C-Bibliothek mit dem Ziel-Sharpie generieren. Wir haben erläutert, wie Sie die generierten API-Definitionen aktualisieren und optimieren, damit Sie für den öffentlichen Gebrauch geeignet sind. Nachdem das xamarin. IOS-Bindungs Projekt fertiggestellt wurde, haben wir mit der Verwendung dieser Bindung in einer xamarin. IOS-Anwendung weiterentwickelt, wobei der Schwerpunkt auf der Verwendung starker Delegaten und schwacher Delegaten liegt.

## <a name="related-links"></a>Verwandte Links

- [Bindungs Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/infcolorpicker)
- [Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Bindungs Details](~/cross-platform/macios/binding/overview.md)
- [Bindungs Typen-Referenzhandbuch](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin für Objective-C-Entwickler](~/ios/get-started/objective-c-developers/index.md)
- [Frameworkentwurfsrichtlinien](https://msdn.microsoft.com/library/ms229042.aspx)
