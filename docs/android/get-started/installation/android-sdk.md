---
title: Android SDK-Setup
description: "Visual Studio enthält den Android SDK-Manager, der den eigenständigen SDK-Manager von Google ersetzt. In diesem Handbuch wird erklärt, wie Sie den SDK-Manager verwenden, um Tools , Plattformen und andere Komponenten von Android SDK herunterzuladen, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 585bcac193d6824bc7c96092c14e40fd7971b0e2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="android-sdk-setup"></a>Android SDK-Setup

_Visual Studio enthält den Android SDK-Manager, der den eigenständigen SDK-Manager von Google ersetzt. In diesem Handbuch wird erklärt, wie Sie den SDK-Manager verwenden, um Tools, Plattformen und andere Komponenten des Android SDK herunterzuladen, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen._


## <a name="overview"></a>Übersicht

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Handbuch wird erklärt, wie Sie den Xamarin Android SDK-Manager für Visual Studio unter Windows (oder [Mac](?tabs=vsmac)) installieren und verwenden.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Handbuch wird erklärt, wie Sie den Xamarin Android SDK-Manager für Visual Studio unter Mac (oder [Windows](?tabs=vswin)) installieren und verwenden.

> [!NOTE]
> Dieser Leitfaden gilt nur für Visual Studio 2017 und Visual Studio für Mac.  

-----

Der Xamarin Android SDK-Manager hilft Ihnen beim Download der neuesten Android-Komponenten, die Sie zum Entwickeln Ihrer Xamarin.Android-App benötigen.
Er ersetzt den eigenständigen SDK-Manager von Google, der als veraltet markiert wurde.

Warum sollten Sie den Xamarin Android SDK-Manager anstelle des SDK-Managers verwenden, der in Android SDK enthalten ist? Google hat mit Version 25.2.3 des Android SDK Tools-Pakets ein neues Tool zum Verwalten von Android SDK eingeführt. Dieses neue Tool, **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)**, ist ein Befehlszeilen-Hilfsprogramm, das den eigenständigen UI-Manager für Android SDK ersetzt. Deshalb müssen Sie nach einem Update auf SDK Tools Version 26.0.1 oder höher (notwendig für Android 8.0) den Xamarin Android SDK-Manager verwenden, wenn Sie Android SDK weiterhin über die Benutzeroberfläche verwalten möchten.

## <a name="requirements"></a>Anforderungen

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie benötigen Folgendes, um den Xamarin Android SDK-Manager verwenden zu können:

- Visual Studio 2017 Community Edition oder höher. Visual Studio 2017 Version 15.5 oder höher.

- Xamarin für Visual Studio Version 4.5.0 oder höher. 

Der Xamarin Android SDK-Manager ist nicht mit Visual Studio kompatibel
2015. Benutzer von Visual Studio 2015 sollten die SDK-Managertools verwenden, die von Google in Android SDK bereitgestellt werden.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

-   Visual Studio für Mac 7.0.0.3146 (oder höher).

-----

Der Xamarin Android SDK Manager benötigt auch das Java Development Kit (das automatisch mit Xamarin.Android installiert wird).
Xamarin.Android verwendet [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch API-Ebenen älter als 24). Sie können weiterhin [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>Installation

Der Xamarin SDK-Manager kann bei der Installation von Visual Studio 2017 hinzugefügt werden. Wenn Sie Visual Studio installieren, klicken Sie auf **Einzelne Komponenten**, und scrollen Sie zum Abschnitt **Entwicklungsaktivitäten**.
Aktivieren Sie den **Xamarin SDK-Manager**, wenn er noch nicht aktiviert ist:

![Aktivieren des Xamarin SDK-Managers aus einzelnen Komponenten](android-sdk-images/win/01-sdk-manager-install.png)

Anweisungen zur Modifikation von Visual Studio, wenn Sie Visual Studio 2017 bereits installiert haben, finden Sie unter [Ändern von Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio), folgen Sie anschließend dem oben genannten Verfahren, um Xamarin SDK-Manager zu aktivieren. Wenn Sie dazu aufgefordert werden, den SDK-Manager zu aktualisieren, können Sie dieses Verfahren für die Installation des Xamarin SDK-Managers verwenden.

Wenn Sie auf **Extras > Android > Android SDK-Manager** (wie im Folgenden erläutert) klicken, wird der Xamarin Android SDK-Manager anstelle des Google Android SDK-Managers gestartet. Wenn Sie eine ältere Version von Android SDK verwenden, die den eigenständigen Google Android SDK-Manager unterstützt, wird durch das Installieren des Xamarin Android SDK-Managers kein Konflikt ausgelöst. Sie können nach wie vor den eigenständigen Google SDK-Manager außerhalb von Visual Studio starten, um Android SDK zu verwalten.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
 
-----

 
## <a name="sdk-manager"></a>SDK-Manager 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Klicken Sie auf **Extras > Android > Android SDK-Manager**, um den SDK-Manager in Visual Studio zu starten:

[![Ort des Menüelements „Android SDK-Manager“](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Der **Xamarin Android SDK-Manager** wird auf der Seite **Android SDKs und Tools** geöffnet. Auf dieser Seite gibt es die zwei Registerkarten &ndash; **Plattformen** und **Tools**:

[![Screenshot vom Android SDK-Manager mit geöffneter Registerkarte „Plattformen“](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

Das Fenster **Android SDKs und Tools** wird in den folgenden Abschnitten ausführlicher beschrieben.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**, um den SDK-Manager zu starten:
 
![Ort des Menüelements „Android SDK-Manager“](android-sdk-images/mac/sdkmanager-01.png )

Der **Android SDK-Manager** wird im **Fenster „Einstellungen“** geöffnet, das die drei Registerkarten **Plattformen**, **Tools** und **Speicherorte** enthält:

![Screenshot vom Android SDK-Manager mit geöffneter Registerkarte „Plattformen“](android-sdk-images/mac/sdkmanager-02.png)

Die Registerkarten im Xamarin Android SDK-Manager werden in den folgenden Abschnitten beschrieben.

-----



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Android SDK-Speicherort

Der Speicherort von Android SDK ist, wie Sie im vorherigen Bild sehen, oben im Fenster **Android SDKs und Tools** konfigurierbar. Dieser Speicherort muss korrekt angegeben sein, damit die Registerkarten **Plattformen** und **Tools** richtig funktionieren können. Es kann sein, dass Sie den Speicherort von Android SDK aus mindestens einem der folgenden Gründe angeben müssen:

1. Der Xamarin SDK-Manager konnte Android SDK nicht finden. 

2. Sie haben Android SDK an einem anderen (nicht dem Standard entsprechenden) Ort gespeichert. 

Klicken Sie auf die Schaltfläche &hellip; rechts neben **Android SDK-Speicherort**, um den Speicherort von Android SDK festzulegen. Daraufhin wird das Dialogfeld **Nach Ordner suchen** geöffnet. Navigieren Sie darin zum Speicherort von Android SDK. Im folgenden Screenshot wird Android SDK unter **Programme (x86)\\Android** ausgewählt:

![Screenshot, auf dem Android SDK im Dialogfeld „Nach Ordner suchen“ von Windows ausgewählt ist](android-sdk-images/win/05-browse-for-folder.png)

Wenn Sie dann auf **OK** klicken, verwaltet der Xamarin Android SDK-Manager die von Ihnen ausgewählte Installation von Android SDK am angegebenen Speicherort.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

### <a name="locations-tab"></a>Registerkarte „Speicherorte“

Die Registerkarte **Speicherorte** hat drei Einstellung zum Konfigurieren der Speicherorte von Android SDK, Android NDK und Java SDK (JDK). Diese Speicherorte müssen korrekt angegeben sein, damit die Registerkarten **Plattformen** und **Tools** richtig funktionieren.

Wenn der SDK-Manager gestartet wird, bestimmt er automatisch den Dateipfad für jedes installierte Paket und gibt an, dass sie **Gefunden** wurden, indem er ein grünes Hakensymbol neben dem Pfad anzeigt:

![Screenshot von der Registerkarte „Speicherorte“](android-sdk-images/mac/sdkmanager-03.png)

Klicken Sie auf die Schaltfläche **Auf Standards zurücksetzen**, wenn der SDK-Manager an den Standardspeicherorten nach SDK, NDK und JDK suchen soll. 

Normalerweise verwenden Sie die Registerkarte **Speicherorte**, um den Speicherort von Android SDK und Java JDK zu ändern. Sie müssen das NDK nicht installieren, um Xamarin.Android-Apps entwickeln zu können. Das NDK benötigen Sie nur, wenn Sie Teile Ihrer App mit nativen Programmiersprachen wie C und C++ entwickeln müssen.

-----


### <a name="tools-tab"></a>Registerkarte „Tools“

Die Registerkarte **Tools** zeigt eine Liste von _Tools_ und _Extras_. Auf dieser Registerkarte können Sie Tools, Plattformtools und Buildtools von Android SDK installieren.
Darüber hinaus können Sie Android-Emulator, den LLDB (Low-Level-Debugger), das NDK, die HAXM-Beschleunigung und Google Play-Bibliotheken installieren.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Laden Sie zum Beispiel das Google Android-Emulator-Paket herunter, indem Sie auf das Kontrollkästchen neben **Android-Emulator** und dann auf **Änderungen anwenden** klicken:

[![Installieren von Android-Emulator über die Registerkarte „Tools“](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Laden Sie zum Beispiel das Google Android-Emulator-Paket herunter, indem Sie auf das Kontrollkästchen neben **Android-Emulator** und dann auf **Updates installieren** klicken:

![Installieren von Android-Emulator über die Registerkarte „Tools“](android-sdk-images/mac/sdkmanager-08.png)

-----


Möglicherweise wird ein Dialogfeld mit folgender Nachricht angezeigt: _Einige Komponenten können aktualisiert werden. Möchten Sie sie jetzt aktualisieren?_ Klicken Sie auf **Ja**. Als Nächstes wird das Dialogfeld „Zustimmung zur Lizenz“ angezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Bildschirm „Zustimmung zur Lizenz“](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Bildschirm „Zustimmung zur Lizenz“](android-sdk-images/mac/sdkmanager-09.png)

-----

Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Nach Abschluss der Installation zeigt die Registerkarte **Tools** an, dass die ausgewählten Tools und Extras installiert wurden.



### <a name="platforms-tab"></a>Registerkarte „Plattformen“

Die Registerkarte **Plattformen** zeigt eine Liste der Versionen des Plattform SDK und andere Ressourcen (beispielsweise Systemimages) für jede Plattform.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Screenshot des Bereichs „Plattformen“](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Screenshot des Bereichs „Plattformen“](android-sdk-images/mac/sdkmanager-11.png)

-----

In dieser Anzeige wird die Android Version (z.B. **Android 7.0**), die Programmiersprache (z.B. **Nougat**), die API-Ebene (z.B. **24**) und der Status (z.B. **Installiert** aufgelistet, wenn die Plattform installiert wird). Mit der Registerkarte **Plattformen** können Sie Komponenten für die Android API-Ebene, die Sie verwenden wollen, installieren (Weitere Informationen zu Android Versionen und API-Ebenen finden Sie unter [Grundlegendes zu Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)).

Wenn alle Komponenten einer Plattform installiert sind, wird neben dem Namen der Plattform ein Häkchen angezeigt. Wenn nicht alle Komponenten installiert sind, wird das Kontrollkästchen gefüllt. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie können die Plattform erweitern, um deren Komponenten anzuzeigen (und welche Komponenten installiert sind), indem Sie auf die Schaltfläche **+** links neben der Plattform klicken.
Klicken Sie auf **-**, um die Auflistung der Komponenten der Plattform wieder einzuklappen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Sie können die Plattform erweitern, um deren Komponenten anzuzeigen (und welche Komponenten installiert sind), indem Sie auf den **Pfeil** links neben der Plattform klicken.
Klicken Sie auf den **Dropdownpfeil**, um die Auflistung der Komponenten der Plattform wieder einzuklappen.

-----

Fügen Sie dem SDK eine weitere Plattform hinzu, indem Sie neben der Plattform auf das Kontrollkästchen klicken, bis das Häkchen angezeigt wird, und klicken Sie dann auf **Änderungen anwenden**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Beispiel für das Hinzufügen von Android 7.1 Nougat-Komponenten zu Android SDK](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Beispiel für das Hinzufügen von Android 4.4-Komponenten zu Android SDK](android-sdk-images/mac/sdkmanager-12.png)

-----

Wenn Sie nur das SDK installieren wollen, klicken Sie einmal auf das Kontrollkästchen neben der Plattform. Dann können Sie auswählen, welche Komponenten Sie benötigen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Beispiel für das Hinzufügen von Android 7.1-Komponenten](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Beispiel für das Hinzufügen von Android 4.4-Komponenten](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Beachten Sie, dass die Anzahl der zu installierenden Komponenten neben der Schaltfläche **Änderungen anwenden** angezeigt wird. Im oberen Beispiel sind sechs Komponenten bereit, installiert zu werden. Wenn Sie auf **Änderungen anwenden** klicken, wird das Fenster **Zustimmung zur Lizenz** angezeigt:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Beachten Sie, dass die Anzahl der zu installierenden Komponenten neben der Schaltfläche **Änderungen anwenden** angezeigt wird. Wenn Sie auf **Änderungen anwenden** klicken, wird das Fenster **Zustimmung zur Lizenz** angezeigt:

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogfeld „Zustimmung zur Lizenz“ mit Registerkarte „Plattformen“](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Dialogfeld „Zustimmung zur Lizenz“ mit Registerkarte „Plattformen“](android-sdk-images/mac/sdkmanager-14.png)

-----

Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Möglicherweise wird dieses Dialogfeld mehrmals angezeigt, wenn Sie mehrere Komponenten installieren. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Sobald der Download und der Installationsprozess beendet sind (das kann abhängig von der Menge der herunterzuladenden Komponenten einige Minuten beanspruchen), werden die hinzugefügten Komponenten mit einem Häkchen versehen und als **Installiert** aufgelistet.

Jetzt sind Sie dafür vorbereitet, Ihre Apps für die neueste und beste API-Ebene zu entwickeln.


 
## <a name="summary"></a>Zusammenfassung

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In diesem Handbuch wurde erklärt, wie Sie den Xamarin Android SDK-Manager in Visual Studio installieren und verwenden.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Handbuch wurde erklärt, wie Sie den Xamarin Android SDK-Manager in Visual Studio für Mac verwenden.

-----


## <a name="related-links"></a>Verwandte Links

- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
