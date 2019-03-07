---
title: Einführung in Xamarin.iOS für Visual Studio
description: In diesem Dokument wird beschrieben, wie Sie Xamarin.iOS-Apps mit Visual Studio erstellen und testen können. Dabei werden das Erstellen eines Projekts, das Ausführen und Debuggen einer App und das Herstellen einer Verbindung zwischen Windows und einem Mac-Buildhost erläutert.
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2018
ms.openlocfilehash: 5f2617272cfdc84fa2b835ce44919d2599a1dce6
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2019
ms.locfileid: "57197198"
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Einführung in Xamarin.iOS für Visual Studio

Mit Xamarin für Windows können iOS-Anwendungen in Visual Studio geschrieben und getestet werden, wobei ein Mac, der sich in einem Netzwerk befindet, die Build- und Bereitstellungsdienste bietet.

In diesem Artikel werden die Schritte für die Installation und Konfiguration von Xamarin.iOS-Tools auf jedem Computer besprochen, sodass Sie iOS-Anwendungen mit Visual Studio erstellen können.

Wenn Sie in Visual Studio für iOS entwickeln, hat dies mehrere Vorteile:

-  Erstellen Sie plattformübergreifende Lösungen für iOS-, Android- und Windows-Anwendungen.
-  Verwenden Sie die Ihnen vertrauten Visual Studio-Tools (wie z.B. **ReSharper** und **Team Foundation Server**) für alle Ihre plattformübergreifenden Projekte, einschließlich iOS-Quellcode.
-  Arbeiten Sie mit der bekannten IDE und profitieren Sie gleichzeitig von Xamarin.iOS-Bindungen aller APIs von Apple.

<a name="Requirements_and_Installation" />

## <a name="requirements-and-installation"></a>Anforderungen und Installation

Sie müssen einige Anforderungen erfüllen, wenn Sie in Visual Studio für iOS bereitstellen. Wie bereits kurz in der Übersicht erwähnt ist ein Mac erforderlich, um IPA-Dateien zu kompilieren. Anwendungen können nicht auf einem Gerät bereitgestellt werden, das über keine Apple-Zertifikate und Codesignierungstools verfügt.

Es stehen einige Konfigurationsoptionen zur Verfügung. Sie können sich also entscheiden, welche am besten zu Ihren Entwicklungsbedürfnissen passt. Dies sind die folgenden:

-  Verwenden Sie einen Mac als Hauptcomputer für die Entwicklung, und führen Sie eine Windows-VM aus, auf der Visual Studio installiert ist. Es wird empfohlen, dass Sie eine VM-Software wie [Parallels](http://www.parallels.com/products/desktop/) oder [VMWare](http://www.vmware.com/products/fusion/) verwenden.
-  Verwenden Sie einen Mac nur als Buildhost. In diesem Szenario ist er mit demselben Netzwerk wie der Windows-Computer verbunden, und die [notwendigen Tools](~/get-started/installation/windows.md#installation) sind auf dem Mac installiert.

Führen Sie in beiden Fällen die folgenden Schritte aus:

- [Installieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation)
- [Installieren Sie Xamarin-Tools unter Windows](~/get-started/installation/windows.md)

## <a name="connecting-to-the-mac"></a>Herstellen einer Verbindung mit dem Mac

Folgen Sie den Anweisungen im Leitfaden [Durchführen einer Kopplung mit einem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md), um Visual Studio mit Ihrem Mac-Buildhost zu verbinden.

## <a name="visual-studio-toolbar-overview"></a>Übersicht: Visual Studio-Symbolleiste

Xamarin.iOS für Visual Studio fügt der Standardsymbolleiste und der neuen iOS-Symbolleiste Elemente hinzu.
Die Funktionen dieser Symbolleisten werden unten erläutert.

### <a name="standard-toolbar"></a>Standardsymbolleiste

Die für die Xamarin.iOS-Entwicklung relevanten Steuerelemente sind rot markiert:

 [![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "Die für die Xamarin.iOS-Entwicklung relevanten Steuerelemente sind rot markiert")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "The controls relevant to Xamarin iOS development are circled in red")

-  **Start**: startet das Debuggen oder Ausführen der Anwendung auf der ausgewählten Plattform. Es muss eine Verbindung zu einem Mac vorhanden sein (Statusanzeige in der iOS-Symbolleiste).
-  **Projektmappenkonfiguration**: zur Auswahl der Konfiguration (z.B. Debug, Release)
-  **Projektmappenplattform**: zur Auswahl des iPhones oder iPhone Simulators für die Bereitstellung

### <a name="ios-toolbar"></a>iOS-Symbolleiste

Die iOS-Symbolleiste in Visual Studio sieht in jeder Visual Studio-Version ähnlich aus. Sie sind alle unten dargestellt:

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "iOS-Symbolleiste")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

Jedes Element wird unten erläutert:

-  **Mac Agent/Verbindungs-Manager**: öffnet das Dialogfeld „Xamarin Mac Agent“. Dieses Symbol ist *orange*, wenn Sie eine Verbindung herstellen, und *grün*, wenn die Verbindung hergestellt wurde.
-  **iOS-Simulator anzeigen**: öffnet das Fenster des iOS-Simulators im Vordergrund
-  **IPA-Datei auf Buildserver anzeigen**: öffnet den Finder auf dem Mac am Speicherort der IPA-Ausgabedatei der Anwendung

## <a name="ios-output-options"></a>iOS-Ausgabeoptionen

### <a name="output-window"></a>Ausgabefenster

Im Bereich *Ausgabe* stehen Optionen zur Verfügung, die Sie zum Anzeigen von Meldungen und Fehlern bezüglich des Builds, der Bereitstellung und der Verbindung verwenden können.

Der unten stehende Screenshot zeigt die verfügbaren Ausgabefenster, die sich je nach Projekttyp unterscheiden können:

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "Die verfügbaren Ausgabefenster")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin**: enthält nur Informationen zu Xamarin, wie etwa die Verbindung zum Mac und der Aktivierungsstatus

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Informationen ausschließlich zu Xamarin, wie etwa die Verbindung zum Mac und der Aktivierungsstatus")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Xamarin-Diagnose**: zeigt detailliertere Angaben zu Ihrem Xamarin-Projekt an, wie etwa die Interaktionen mit und für Android

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Detaillierte Informationen zum Xamarin-Projekt")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

Andere Standardausgabebereiche von Visual Studio, wie z.B. Debug und Build, sind immer noch über die Ausgabeansicht verfügbar. Sie werden für die Debugging- und MSBuild-Ausgabe verwendet:

-  **Debuggen**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "Debugausgabe")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **Build** & **Buildreihenfolge**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "MSBuild-Ausgabe")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)

## <a name="ios-project-properties"></a>iOS-Projekteigenschaften

Sie können auf die Projekteigenschaften von Visual Studio zugreifen, indem Sie mit der rechten Maustaste auf dem Projektnamen und anschließend im Kontextmenü auf *Eigenschaften* klicken. So können Sie Ihre iOS-Anwendung wie im unten stehenden Screenshot konfigurieren:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "Konfigurieren einer iOS-Anwendung")

-  *iOS-Bundle-Signierung*: stellt eine Verbindung mit dem Mac her, um die Codesignierungsidentitäten und Bereitstellungsprofile aufzufüllen:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png "Füllen Sie die Codesignieridentitäten und Bereitstellungsprofile auf")

-  *iOS-IPA-Optionen*: Die IPA-Datei wird im Dateisystem des Macs gespeichert:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png "iOS-IPA-Optionen")

-  *iOS-Ausführungsoptionen*: Konfigurieren zusätzlicher Parameter:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png "iOS-Ausführungsoptionen")

## <a name="creating-a-new-project-for-ios-applications"></a>Erstellen eines neuen Projekts für iOS-Anwendungen

Sie erstellen ein neues iOS-Projekt wie jedes andere Projekt in Visual Studio. Wenn Sie auf **Datei > Neues Projekt** klicken, wird das unten gezeigte Dialogfeld geöffnet, das einige der verfügbaren Projekttypen zum Erstellen eines neuen iOS-Projekts enthält:

![Erstellen eines neuen Projekts](introduction-to-xamarin-ios-for-visual-studio-images/newproject.w157.png)

Indem Sie die **iOS-App (Xamarin)** auswählen, werden die folgenden Vorlagen zum Erstellen einer neuen Xamarin.iOS-Anwendung angezeigt:

![Auswählen der Vorlage für eine iOS-App](introduction-to-xamarin-ios-for-visual-studio-images/newproject-2.w157.png)

Storyboard- und XIB-Dateien können in Visual Studio mit dem iOS-Designer bearbeitet werden. Um ein Storyboard zu erstellen, wählen Sie eine der Storyboardvorlagen aus. Dadurch wird wie im unten stehenden Screenshot veranschaulicht eine **main.storyboard**-Datei im **Projektmappen-Explorer** erstellt:

![Die Datei „Main.storyboard“ in Projektmappen-Explorer](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.w157.png)

Um mit der Erstellung oder Bearbeitung des Storyboards zu beginnen, doppelklicken Sie auf `Main.storyboard`, um es im iOS-Designer zu öffnen:

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "Die Datei „Main.storyboard“ im iOS-Designer")

Um Ihrer Ansicht Objekte hinzuzufügen, fügen Sie über den Bereich **Toolbox** per Drag & Drop Elemente auf Ihrer Designoberfläche ein. Klicken Sie auf **Ansicht > Toolbox**, um die Toolbox hinzuzufügen, wenn sie noch nicht vorhanden ist. Objekteigenschaften können wie unten dargestellt über den Bereich **Eigenschaften** bearbeitet, ihre Layouts angepasst und Ereignisse erstellt werden:

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "Der Bereich „Eigenschaften“")

 Weitere Informationen zur Verwendung des iOS Designers finden Sie in den Leitfäden zum [Designer](~/ios/user-interface/designer/index.md).

## <a name="running--debugging-ios-applications"></a>Ausführen und Debuggen von iOS-Anwendungen

### <a name="device-logging"></a>Geräteprotokollierung

In Visual Studio 2017 wurden die Android- und iOS-Protokollpads vereinheitlicht.

Mit dem neuen Toolfenster „Geräteprotokoll“ für Visual Studio können Sie Protokolle für Android- und iOS-Geräte anzeigen. Dies erreichen Sie mit jedem der folgenden Befehle:

- **Ansicht > Andere Fenster > Geräteprotokoll**
- **Tools > iOS > Geräteprotokoll**
- **iOS-Symbolleiste > Geräteprotokoll**

Wenn das Toolfenster angezeigt wird, kann der Benutzer das physische Gerät aus dem Gerätedropdownmenü auswählen. Wenn ein Gerät ausgewählt wird, werden der Tabelle automatisch Protokolle hinzugefügt. Wenn Sie zwischen Geräten wechseln, wird die Geräteprotokollierung abgebrochen und erneut gestartet.

Damit die Gerät in der ComboBox angezeigt werden, muss ein iOS-Projekt geladen werden. Zusätzlich zu iOS muss Visual Studio [mit dem Mac-Server verbunden sein](~/ios/get-started/installation/windows/connecting-to-mac/index.md), damit es iOS-Geräte erkennen kann, die mit dem Mac verbunden sind.

Über dieses Toolfenster haben Sie Zugriff auf Folgendes: eine Tabelle mit Protokolleinträgen, ein Dropdownmenü zur Geräteauswahl, Löschvorgänge für Protokolleinträge, ein Suchfeld und die Schaltflächen „Wiedergabe“, „Anhalten“ und „Pause“.

### <a name="set-debugging-stops"></a>Festlegen von Haltepunkten für das Debuggen

Breakpoints können überall in Ihrer Anwendung festgelegt werden, um dem Debugger anzuzeigen, dass er dort die Ausführung des Programms vorübergehend anhalten soll. Klicken Sie zum Festlegen eines Breakpoints in Visual Studio im Randbereich Ihres Editors neben die Zeilennummer des Codes, wo Sie unterbrechen möchten:

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "Festlegen eines Debugpunkts")

Beginnen Sie mit dem Debuggen, und verwenden Sie den Simulator oder das Gerät, um Ihre Anwendung zu einem Breakpoint zu navigieren. Wenn Sie einen Breakpoint erreichen, wird die Zeile gekennzeichnet und das normale Debuggingverhalten von Visual Studio wird aktiviert: Sie können in den Code springen, Code überspringen oder aus diesem heraus springen, lokale Variablen untersuchen oder das Direktfenster verwenden.

In diesem Screenshot wird der iOS-Simulator gezeigt, der gleichzeitig mit Visual Studio mithilfe von Parallels unter OS X ausgeführt wird:

![](introduction-to-xamarin-ios-for-visual-studio-images/image19.png "In diesem Screenshot wird der iOS-Simulator gezeigt, der gleichzeitig mit Visual Studio mithilfe von Parallels unter OS X ausgeführt wird")

### <a name="examine-local-variables"></a>Untersuchen lokaler Variablen

![](introduction-to-xamarin-ios-for-visual-studio-images/image20.png "Untersuchen lokaler Variablen beim Debuggen")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie Xamarin.iOS für Visual Studio verwenden. Die verfügbaren Funktionen zum Erstellen und Testen einer iOS-App in Visual Studio wurden aufgelistet, und Sie wurden durch das Erstellen und Debuggen einer einfachen iOS-Anwendung geführt.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.iOS-Installation](~/ios/get-started/installation/windows/index.md)
- [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md)
- [Erstellen von iOS-UI in Code](~/ios/app-fundamentals/ios-code-only.md)
- [Connecting a Mac to your Visual Studio environment with XMA (Herstellen einer Verbindung zwischen einem Mac und der Visual Studio-Umgebung mit XMA) (Video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
