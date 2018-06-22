---
title: Visual Studio-Emulator für Android
description: Dieser Leitfaden erläutert die Methoden zum Konfigurieren und Verwenden des Android-Emulators von Visual Studio zum Entwickeln von Xamarin.Android-Apps in Visual Studio 2015.
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: 29e35d0dee614d28eed08fbe8799fc74c5ad1eba
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436988"
---
# <a name="visual-studio-android-emulator"></a>Visual Studio-Emulator für Android

_Dieser Leitfaden erläutert die Methoden zum Konfigurieren und Verwenden des Android-Emulators von Visual Studio zum Entwickeln von Xamarin.Android-Apps in Visual Studio 2015._

## <a name="visual-studio-android-emulator-overview"></a>Überblick: Android-Emulator für Visual Studio

Microsoft Visual Studio 2015 enthält einen Android-Emulator, der als Debugziel für eine Xamarin.Android-App verwendet werden kann: *Visual Studio-Emulator für Android*. Dieser Emulator verwendet die Hyper-V-Funktionen Ihres Entwicklungscomputers, wodurch schnellere Start- und Ausführungszeiten garantiert werden können, als beim Standardemulator, der mit dem Android SDK geliefert wird. Der Visual Studio-Emulator für Android kann als eine Alternative zum Android SDK-Standardemulator verwendet werden, wenn Sie eine Xamarin.Android-Anwendung verwenden.

> [!NOTE]
> Der Visual Studio-Emulator für Android ist nur mit Visual Studio 2015 kompatibel und nicht mit Visual Studio 2017.

Dieser Leitfaden erläutert, wie der Microsoft Android-Emulator von Visual Studio zum Testen Ihrer App gestartet wird. Ebenso werden die unterschiedlichen verfügbaren Funktionen des Emulators beschrieben. Sie erfahren, wie Sie *Geräteprofile* (ähnlich wie Gerätedefinitionen im Android SDK-Standardemulator) auswählen können, um unterschiedliche Arten von Android-Geräten zu simulieren. Zum Schluss werden im Abschnitt zur Problembehandlung häufige Fehlerquellen und Problemumgehung erklärt.

## <a name="requirements"></a>Anforderungen

Um den Emulator ausführen, muss der Computer die Voraussetzungen zum Ausführen von Hyper-V erfüllen. Hyper-V erfordert eine 64-Bit-Version der Pro-Edition von Windows 8, Windows 8.1, Windows 10 oder höher. Weitere Informationen zu den Anforderungen finden Sie unter [Systemanforderungen für den Visual Studio-Emulator für Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx)

> [!NOTE]
> Sie können HAXM (vom Android SDK-Emulator verwendet) nicht verwenden, während Hyper-V aktiviert ist. Weitere Informationen zu den Einschränkungen und potentielle Probleme mit HAXM finden Sie unter [HAXM Virtualization Conflicts (HAXM-Virtualisierungskonflikte)](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts).


## <a name="running-the-emulator"></a>Ausführen des Emulators

Visual Studio macht mehrere vorkonfigurierte Zielgerätprofile im Dropdownmenü **Debugziel** verfügbar (dies ist im folgenden Screenshot dargestellt). Den Microsoft Android-Emulatorzielen wird der **VS-Emulator** vorangestellt:

[![Vorkonfigurierte Zielgerätprofile](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Wenn Visual Studio eine Xamarin.Android-Anwendung startet, wird der Emulator mit dem ausgewählten Geräteziel gestartet, und die App wird auf dem Emulator bereitgestellt. In der unteren linken Ecke von Visual Studio wird eine Meldung angezeigt, die angibt, dass der Emulator gestartet wird:

[![VS-Emulator starten](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

Nach einer Verzögerung beim Systemstart wird der Emulatorbildschirm wie unten links angezeigt. Ziehen Sie das Sperrsymbol auf dem Bildschirm nach oben, um das Gerät zu entsperren.
Die Xamarin.Android-App sollte dann, wie auf dem rechten Telefon dargestellt, im Emulator ausgeführt werden:

[![Emulatorscreenshots](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

So wie bei beim Android SDK-Standardemulator ist es auch möglich, Breakpoints im Code festzulegen, Variablen zu prüfen und die Aufrufliste anzuzeigen. Die vertikale Symbolleiste rechts neben dem Emulator stellt den Zugriff auf die Emulatorfunktionen bereit:

[![Schaltflächen auf der vertikalen Symbolleiste](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

In der folgenden Liste ist die Funktion jeder Schaltfläche auf der vertikalen Symbolleiste zusammengefasst:

-   **Close** (Schließen): Fährt die Emulatoranwendung herunter. Diese Schaltfläche wird nicht oft verwendet &ndash; normalerweise wird der Emulator nach dem ersten Start dauerhaft ausgeführt (um die Verzögerung beim Neustart des Emulators zu vermeiden) und nur geschlossen, wenn er nicht länger benötigt wird.

-   **Minimize** (Minimieren): Der Emulator wird weiter ausgeführt, jedoch auf der Taskleiste minimiert.

-   **Power** (Startschaltfläche): Simuliert das An- und Ausschalten des Geräts. (Der Emulator wird weiter ausgeführt.)

-   **Multi-Touch**: Überlagert mehrere Punkt auf dem Gerätebildschirm, die als Berührpunkte für das Zusammenführen und Zoomen fungieren.
    Das Bewegen eines Punkts führt dazu, dass der andere Punkt in die entgegengesetzte Richtung verschoben wird, wodurch die Bewegung durch zwei Finger simuliert wird.

-   **Einzelpunkt-Mauseingabe**: Setzt das Gerät wieder auf die Einzelpunkteingabe (nach Verwendung der Multi-Touch-Eingabe) zurück.

-   **Nach links drehen/Nach rechts drehen**: Hilft dabei, zu testen, wie die App auf Richtungswechsel reagiert. Das erste Mal, wenn auf die Schaltfläche *Nach links drehen* gedrückt wird, wechselt der Emulator z.B. in das Querformat. Wenn auf die Schaltfläche *Nach rechts gehen* gedrückt wird, kehrt der Emulator in das Hochformat zurück.

-   **An Bildschirmgröße anpassen**: Die Größe des Emulatorbildschirms wird vergrößert, um sie an den Desktopbildschirm anzupassen.

-   **Zoom**: Skaliert den Emulatorbildschirm auf 33 %, 50 %, 66 %, 100 % oder auf eine benutzerdefiniert Prozentangabe.


Die Schaltfläche *Additional Tools* (Zusätzliche Tools) zeigt an, dass ein Dialogfeld geöffnet wird, auf dem die zusätzlichen Funktionen des Emulators angezeigt werden.

[![Dialogfeld „Zusätzliche Tools“](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


Jede zusätzliche Funktion ist über unterschiedliche Schaltflächen oben im Dialogfeld verfügbar:

-   **Accelerometer** (Beschleunigungsmesser): Simuliert die Bewegung des Geräts in einem dreidimensionalen Raum.

-   **Location** (Standort): Präsentiert eine Karte, die zum Suchen und Simulieren eines GPS-Standorts verwendet werden kann. Auf dieser Karte können *Kartenpunkte* zum Simulieren von Bewegung zwischen Standorten simuliert werden.

-   **Battery** (Akku): Stellt einen Schieberegler bereit, mit dem der verfügbare Akkustand simuliert wird.

-   **Screenshot**: Auf dieser Registerkarte ist die Schaltfläche **Capture** (Erfassen) dargestellt, die einen Screenshot aufnimmt und eine sofortige Vorschau anzeigt. Durch die Schaltfläche *Save* (Speichern) wird der Screenshot gespeichert.

-   **Camera** (Kamera): Simuliert das Aufnehmen eines Fotos über ein festgelegtes animiertes Bild, ein Bild aus einer Datei oder von einer an Ihrem Hostcomputer angefügte Webcam. Es ist mögliche, entweder die Frontkamera oder die rückseitige Kamera auszuwählen.

-   **SD Card** (SD-Karte): Der Emulator kann einen Ordner auf Ihrem Computer auf dem Gerät als SD-Karte verfügbar machen.
    Wenn die App Dateien auf die simulierte SD-Karte liest und schreibt, kann auf diese direkt vom Desktop aus zugegriffen werden, ohne den `adb`-Befehl zu verwenden.

-   **Network** (Netzwerk): Stellt eine Zusammenfassungen der Netzwerkeinstellungen des Emulators dar (dieser verwendet die Netzwerkverbindung des Hostcomputers wieder).

Weitere Informationen darüber, wie diese Funktionen verwendet werden, finden Sie unter [Introducing Visual Studio's Emulator for Android (Einführung in den Visual Studio-Emulator für Android)](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/).



## <a name="configuring-device-profiles"></a>Konfigurieren von Geräteprofilen

Der Microsoft Android-Emulator enthält einen Satz von Geräteprofilen, die die beliebtesten Android-Versionen, Bildschirmgrößen und Hardwareeigenschaften von Android-Geräten auf dem Markt darstellen. Dazu sind diese Geräteprofile bereits für eine Reihe von Android-Versionen konfiguriert, z.B. für KitKat, Lollipop und Marshmallow.

Der *Emulator-Manager* wird zum Installieren, Deinstallieren und Starten von Geräteprofilen verwendet. Wählen Sie im Menü **Tools** **Visual Studio-Emulator für Android...**, so wie im Screenshot unten gezeigt, aus:

[![Emulators aus dem Menü „Tools“ starten](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

Dadurch wird das Dialogfeld **Geräteprofile** geöffnet. Die installierten Profile werden oben in der Geräteprofilliste gekennzeichnet. Profile, die nicht installiert werden (jedoch zur Installation verfügbar sind), sind ausgegraut:

[![Symbole für Geräteprofile](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

Um ein neues Profil zu erstellen, klicken Sie auf das Symbol für die Profilinstallation (ein Pfeil, der nach unten zeigt, so wie im Screenshot oben dargestellt). Wenn Sie z.B. auf das Symbol für die Profilinstallation für das **5.7" Marshmallow (6.0.0) XHDPI Phone** klicken, lädt der Emulator-Manager das Profil, wie unten dargestellt, herunter:

[![Beispiel für das Herunterladen von Profilen](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

Nachdem das Geräteprofil heruntergeladen wurde, wird es gekennzeichnet, um anzugeben, dass das Profil erfolgreich installiert wurde. Durch Klicken auf das Symbol *Details anzeigen* wird der Plattformtyp, die CPU-Architektur, die Bildschirmgröße sowie -auflösung sowie der verfügbare Speicher für das Gerät angezeigt.

[![Geräteprofildetails anzeigen](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Wenn das Dropdownmenü **Ziel debuggen** in Visual Studio geöffnet wird, ist das neu installierte Geräteprofil nun als Ziel verfügbar:

[![Neues Profil im Ziel-Dropdownmenü](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

Diese Liste kann gekürzt werden. Klicken Sie hierzu im *Emulator-Manager* auf **Uninstall this profile** (Diese Profil deinstallieren), um nicht verwendete Geräteprofile zu entfernen. Beachten Sie, dass Sie derzeit keine benutzerdefinierten Geräteprofile in diesem Emulator erstellen können.


## <a name="troubleshooting"></a>Problembehandlung

In diesem Abschnitt werden einige häufige Fehler und Problemumgehungen bei Verwendung des *Visual Studio-Emulators für Android* mit Xamarin.Android beschrieben.

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>Der Emulator wird nicht gestartet

In einigen Fällen wird der Emulator nicht gestartet, wenn es Inkompatibilitäten zwischen dem Hostprozessor und dem virtuellen Hyper-V-Computer gibt. Um diese Problem zu umgehen, konfigurieren Sie Hyper-V so, dass die Prozessorfunktionen, über die ein virtueller Computer verfügen kann, eingeschränkt werden &ndash; so wird die Kompatibilität des virtuellen Computers mit unterschiedlichen Hostprozessorversionen verbessert.
Durchlaufen Sie folgende Schritte, um diese Änderung wirksam zu machen:

1.  Klicken Sie auf die Schaltfläche **Start**, geben Sie **MMC** ein, und drücken Sie auf die **EINGABETASTE**. Klicken Sie, wie unten dargestellt, auf **Hyper-V Manager**:

    [![Hyper-V-Manager](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  Klicken Sie im Bereich **Virtual Machines** des Hyper-V Managers mit der rechten Maustaste auf den Emulator, der zum Bearbeiten verwendet werden soll, und klicken Sie auf **Einstellungen...**:

    [![Menüelement „Einstellungen“ von Virtual Machines](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  Navigieren Sie im Fenster „Einstellungen“ zum Bereich **Compatibility** (Kompatibilität) (unter **Hardware > Prozessor**), und aktivieren Sie **Migrate to a physical computer with a different processor version** (Zu einem physischen Computer mit einer anderen Prozessorversion migrieren):

    [![Option „Migrieren“ aktiviert](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  Klicken Sie auf **OK**, und schließen Sie das Fenster des Hyper-V Managers.



### <a name="app-deploys-and-starts-but-fails-immediately"></a>Die App wird bereitgestellt und gestartet, schlägt jedoch sofort fehl

In dieser Situation wird der Emulator gestartet, die App erfolgreich im Emulator bereitgestellt und gestartet. Die App schlägt jedoch sofort fehl.
In vielen Fällen liegt der Grund bei Inkompatibilitäten zwischen dem Hostprozessor und dem virtuellen Hyper V-Computer. Befolgen Sie die Anweisungen unter [Der Emulator wird nicht gestartet](#cant_connect) (oben), um diesen Fehler zu beheben.


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>Der Emulator wird mit der Diagnosemeldung **libaot-mscorlib.dll.so nicht gefunden** angehalten

Befolgen Sie die folgenden Schritte zur Deaktivierung der Bereitstellung, um diesen Fehler zu beheben:

1.  Befolgen Sie die Anweisungen unter [Der Emulator wird nicht gestartet](#cant_connect) (oben).

2.  Führen Sie einen Doppelklick auf **Properties** (Eigenschaften) aus.

3.  Klicken Sie auf **Android-Optionen**, und heben Sie die Auswahl von **Fast Deployment verwenden (nur Debugmodus)** auf:

    [![Option „Use Fast Deployment“ (Fast Deployment verwenden) aufgehoben](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>Drag & Drop funktioniert nicht

Wenn der *Visual Studio-Emulator für Android* als Administrator gestartet wird (oder falls sie ihn über Visual Studio starten, während Visual Studio mit Administratorberechtigung ausgeführt wird), funktioniert Drag & Drop von APK- oder ZIP-Dateien möglicherweise nicht. Um diese Problem zu umgehen, führen Sie den *Visual Studio-Emulator für Android* ohne erweiterte Berechtigungen (d.h. nicht als Administrator) aus.


### <a name="other-errors"></a>Andere Fehler

Die oben genannten Tipps zur Problembehandlung decken die häufigsten Probleme bei der Verwendung des Visual Studio-Emulators für Android mit Xamarin.Android ab. Eine ausführlichere Anleitung für die Problembehandlung im Visual Studio-Emulator für Android finden Sie unter [Troubleshooting the Visual Studio Emulator for Android (Problembehandlung für den Visual Studio-Emulator für Android)](https://msdn.microsoft.com/en-us/library/mt228282.aspx).



## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde der *Visual Studio-Emulator für Android* vorgestellt. Es wurde erklärt, wie der Emulator zum Debuggen von Xamarin.Android-Apps in Visual Studio verwendet wird. Zudem wurden die Funktionen der Schaltflächen auf der vertikalen Symbolleiste beschrieben und eine kurze Übersicht der verfügbaren Funktionen im Dialogfeld **Zusätzliche Tools** bereitgestellt. Die Verwendung des *Emulator-Managers* zum Installieren, Deinstallieren und Starten von Geräteprofilen wurde ebenfalls behandelt. Im Abschnitt *Problembehandlung* wurden häufige Probleme und Problembehandlungen bei der Verwendung des Emulators beschrieben.


## <a name="related-links"></a>Verwandte Links

- [Visual Studio-Emulator für Android](https://www.visualstudio.com/vs/msft-android-emulator/)
- [Einführung in den Visual Studio-Emulator für Android](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
