---
title: Einrichten des Android SDK für Xamarin.Android
description: In Visual Studio ist der SDK-Manager enthalten, mit dem Sie Tools, Plattformen und andere Komponenten des Android SDK herunterladen können, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/10/2018
ms.openlocfilehash: 895496f6a198f679ce08322ae48fe88e03b85629
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947269"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Einrichten des Android SDK für Xamarin.Android

_Visual Studio enthält den SDK-Manager, mit dem Sie Tools, Plattformen und andere Komponenten des Android SDK herunterladen können, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen._


## <a name="overview"></a>Übersicht

In diesem Handbuch wird erklärt, wie Sie den Xamarin Android SDK-Manager in Visual Studio und in Visual Studio für Mac verwenden.

> [!NOTE]
> Dieser Leitfaden gilt nur für Visual Studio 2017 und Visual Studio für Mac.  

Der Xamarin Android SDK-Manager (der als Teil der Workload **Mobile-Entwicklung mit.NET** installiert wird) unterstützt Sie beim Herunterladen der aktuellsten Komponenten, die Sie für die Entwicklung Ihrer Xamarin.Android-App benötigen. Er ersetzt den eigenständigen SDK-Manager von Google, der als veraltet markiert wurde.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um den Xamarin Android SDK-Manager verwenden zu können:

- Visual Studio 2017 (Community, Professional oder Enterprise Edition). Visual Studio 2017, Version 15.7 oder höher.

- Visual Studio-Tools für Xamarin Version 4.10.0 oder höher. 

Der Xamarin Android SDK-Manager ist nicht mit Visual Studio kompatibel
2015. Benutzer von Visual Studio 2015 sollten die SDK-Managertools verwenden, die von Google in Android SDK bereitgestellt werden.


Der Xamarin Android SDK Manager benötigt auch das Java Development Kit (das automatisch mit Xamarin.Android installiert wird).
Xamarin.Android verwendet [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch API-Ebenen älter als 24). Sie können weiterhin [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

 
## <a name="sdk-manager"></a>SDK-Manager 

Klicken Sie auf **Extras > Android > Android SDK-Manager**, um den SDK-Manager in Visual Studio zu starten:

[![Ort des Menüelements „Android SDK-Manager“](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Der **Xamarin Android SDK-Manager** wird auf der Seite **Android SDKs und Tools** geöffnet. Auf dieser Seite gibt es die zwei Registerkarten &ndash; **Plattformen** und **Tools**:

[![Screenshot vom Android SDK-Manager mit geöffneter Registerkarte „Plattformen“](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

Das Fenster **Android SDKs und Tools** wird in den folgenden Abschnitten ausführlicher beschrieben.


### <a name="android-sdk-location"></a>Android SDK-Speicherort

Der Speicherort von Android SDK ist, wie Sie im vorherigen Bild sehen, oben im Fenster **Android SDKs und Tools** konfigurierbar. Dieser Speicherort muss korrekt angegeben sein, damit die Registerkarten **Plattformen** und **Tools** richtig funktionieren können. Es kann sein, dass Sie den Speicherort von Android SDK aus mindestens einem der folgenden Gründe angeben müssen:

1. Der Xamarin SDK-Manager konnte Android SDK nicht finden. 

2. Sie haben Android SDK an einem anderen (nicht dem Standard entsprechenden) Ort gespeichert. 

Klicken Sie auf die Schaltfläche &hellip; rechts neben **Android SDK-Speicherort**, um den Speicherort von Android SDK festzulegen. Daraufhin wird das Dialogfeld **Nach Ordner suchen** geöffnet. Navigieren Sie darin zum Speicherort von Android SDK. Im folgenden Screenshot wird Android SDK unter **Programme (x86)\\Android** ausgewählt:

![Screenshot, auf dem Android SDK im Dialogfeld „Nach Ordner suchen“ von Windows ausgewählt ist](android-sdk-images/win/05-browse-for-folder.png)

Wenn Sie dann auf **OK** klicken, verwaltet der Xamarin Android SDK-Manager die von Ihnen ausgewählte Installation von Android SDK am angegebenen Speicherort.



### <a name="tools-tab"></a>Registerkarte „Tools“

Die Registerkarte **Tools** zeigt eine Liste von _Tools_ und _Extras_. Auf dieser Registerkarte können Sie Tools, Plattformtools und Buildtools von Android SDK installieren.
Darüber hinaus können Sie Android-Emulator, den LLDB (Low-Level-Debugger), das NDK, die HAXM-Beschleunigung und Google Play-Bibliotheken installieren.


Laden Sie zum Beispiel das Google Android-Emulator-Paket herunter, indem Sie auf das Kontrollkästchen neben **Android-Emulator** und dann auf **Änderungen anwenden** klicken:

[![Installieren von Android-Emulator über die Registerkarte „Tools“](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)



Möglicherweise wird ein Dialogfeld mit folgender Nachricht angezeigt: _Einige Komponenten können aktualisiert werden. Möchten Sie sie jetzt aktualisieren?_ Klicken Sie auf **Ja**. Als Nächstes wird das Dialogfeld „Zustimmung zur Lizenz“ angezeigt:


![Bildschirm „Zustimmung zur Lizenz“](android-sdk-images/win/07-license-acceptance.png)


Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Nach Abschluss der Installation zeigt die Registerkarte **Tools** an, dass die ausgewählten Tools und Extras installiert wurden.



### <a name="platforms-tab"></a>Registerkarte „Plattformen“

Die Registerkarte **Plattformen** zeigt eine Liste der Versionen des Plattform SDK und andere Ressourcen (beispielsweise Systemimages) für jede Plattform.


[![Screenshot des Bereichs „Plattformen“](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)


In dieser Anzeige wird die Android Version (z.B. **Android 7.0**), die Programmiersprache (z.B. **Nougat**), die API-Ebene (z.B. **24**) und der Status (z.B. **Installiert** aufgelistet, wenn die Plattform installiert wird). Mit der Registerkarte **Plattformen** können Sie Komponenten für die Android API-Ebene, die Sie verwenden wollen, installieren (Weitere Informationen zu Android Versionen und API-Ebenen finden Sie unter [Grundlegendes zu Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)).

Wenn alle Komponenten einer Plattform installiert sind, wird neben dem Namen der Plattform ein Häkchen angezeigt. Wenn nicht alle Komponenten installiert sind, wird das Kontrollkästchen gefüllt. 


Sie können die Plattform erweitern, um deren Komponenten anzuzeigen (und welche Komponenten installiert sind), indem Sie auf die Schaltfläche **+** links neben der Plattform klicken.
Klicken Sie auf **-**, um die Auflistung der Komponenten der Plattform wieder einzuklappen.


Fügen Sie dem SDK eine weitere Plattform hinzu, indem Sie neben der Plattform auf das Kontrollkästchen klicken, bis das Häkchen angezeigt wird, und klicken Sie dann auf **Änderungen anwenden**:


[![Beispiel für das Hinzufügen von Android 7.1 Nougat-Komponenten zu Android SDK](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)


Wenn Sie nur das SDK installieren wollen, klicken Sie einmal auf das Kontrollkästchen neben der Plattform. Dann können Sie auswählen, welche Komponenten Sie benötigen:


[![Beispiel für das Hinzufügen von Android 7.1-Komponenten](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)




Beachten Sie, dass die Anzahl der zu installierenden Komponenten neben der Schaltfläche **Änderungen anwenden** angezeigt wird. Im oberen Beispiel sind sechs Komponenten bereit, installiert zu werden. Wenn Sie auf **Änderungen anwenden** klicken, wird das Fenster **Zustimmung zur Lizenz** angezeigt:



![Dialogfeld „Zustimmung zur Lizenz“ mit Registerkarte „Plattformen“](android-sdk-images/win/11-license-screen.png)


Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Möglicherweise wird dieses Dialogfeld mehrmals angezeigt, wenn Sie mehrere Komponenten installieren. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Sobald der Download und der Installationsprozess beendet sind (das kann abhängig von der Menge der herunterzuladenden Komponenten einige Minuten beanspruchen), werden die hinzugefügten Komponenten mit einem Häkchen versehen und als **Installiert** aufgelistet.



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


## <a name="requirements"></a>Anforderungen

Sie benötigen Folgendes, um den Xamarin Android SDK-Manager verwenden zu können:

-   Visual Studio für Mac 7.5 (oder höher).

Der Xamarin Android SDK Manager benötigt auch das Java Development Kit (das automatisch mit Xamarin.Android installiert wird).
Xamarin.Android verwendet [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch API-Ebenen älter als 24). Sie können weiterhin [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

 
## <a name="sdk-manager"></a>SDK-Manager 

Klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**, um den SDK-Manager zu starten:
 
![Ort des Menüelements „Android SDK-Manager“](android-sdk-images/mac/sdkmanager-01.png )

Der **Android SDK-Manager** wird im **Fenster „Einstellungen“** geöffnet, das die drei Registerkarten **Plattformen**, **Tools** und **Speicherorte** enthält:

![Screenshot vom Android SDK-Manager mit geöffneter Registerkarte „Plattformen“](android-sdk-images/mac/sdkmanager-02.png)

Die Registerkarten im Xamarin Android SDK-Manager werden in den folgenden Abschnitten beschrieben.


### <a name="locations-tab"></a>Registerkarte „Speicherorte“

Die Registerkarte **Speicherorte** hat drei Einstellung zum Konfigurieren der Speicherorte von Android SDK, Android NDK und Java SDK (JDK). Diese Speicherorte müssen korrekt angegeben sein, damit die Registerkarten **Plattformen** und **Tools** richtig funktionieren.

Wenn der SDK-Manager gestartet wird, bestimmt er automatisch den Dateipfad für jedes installierte Paket und gibt an, dass sie **Gefunden** wurden, indem er ein grünes Hakensymbol neben dem Pfad anzeigt:

![Screenshot von der Registerkarte „Speicherorte“](android-sdk-images/mac/sdkmanager-03.png)

Klicken Sie auf die Schaltfläche **Auf Standards zurücksetzen**, wenn der SDK-Manager an den Standardspeicherorten nach SDK, NDK und JDK suchen soll. 

Normalerweise verwenden Sie die Registerkarte **Speicherorte**, um den Speicherort von Android SDK und Java JDK zu ändern. Sie müssen das NDK nicht installieren, um Xamarin.Android-Apps entwickeln zu können. Das NDK benötigen Sie nur, wenn Sie Teile Ihrer App mit nativen Programmiersprachen wie C und C++ entwickeln müssen.

### <a name="tools-tab"></a>Registerkarte „Tools“

Die Registerkarte **Tools** zeigt eine Liste von _Tools_ und _Extras_. Auf dieser Registerkarte können Sie Tools, Plattformtools und Buildtools von Android SDK installieren.
Darüber hinaus können Sie Android-Emulator, den LLDB (Low-Level-Debugger), das NDK, die HAXM-Beschleunigung und Google Play-Bibliotheken installieren.


Laden Sie zum Beispiel das Google Android-Emulator-Paket herunter, indem Sie auf das Kontrollkästchen neben **Android-Emulator** und dann auf **Updates installieren** klicken:

![Installieren von Android-Emulator über die Registerkarte „Tools“](android-sdk-images/mac/sdkmanager-08.png)


Möglicherweise wird ein Dialogfeld mit folgender Nachricht angezeigt: _Einige Komponenten können aktualisiert werden. Möchten Sie sie jetzt aktualisieren?_ Klicken Sie auf **Ja**. Als Nächstes wird das Dialogfeld „Zustimmung zur Lizenz“ angezeigt:


![Bildschirm „Zustimmung zur Lizenz“](android-sdk-images/mac/sdkmanager-09.png)


Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Nach Abschluss der Installation zeigt die Registerkarte **Tools** an, dass die ausgewählten Tools und Extras installiert wurden.



### <a name="platforms-tab"></a>Registerkarte „Plattformen“

Die Registerkarte **Plattformen** zeigt eine Liste der Versionen des Plattform SDK und andere Ressourcen (beispielsweise Systemimages) für jede Plattform.


![Screenshot des Bereichs „Plattformen“](android-sdk-images/mac/sdkmanager-11.png)


In dieser Anzeige wird die Android Version (z.B. **Android 7.0**), die Programmiersprache (z.B. **Nougat**), die API-Ebene (z.B. **24**) und der Status (z.B. **Installiert** aufgelistet, wenn die Plattform installiert wird). Mit der Registerkarte **Plattformen** können Sie Komponenten für die Android API-Ebene, die Sie verwenden wollen, installieren (Weitere Informationen zu Android Versionen und API-Ebenen finden Sie unter [Grundlegendes zu Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)).

Wenn alle Komponenten einer Plattform installiert sind, wird neben dem Namen der Plattform ein Häkchen angezeigt. Wenn nicht alle Komponenten installiert sind, wird das Kontrollkästchen gefüllt. 


Sie können die Plattform erweitern, um deren Komponenten anzuzeigen (und welche Komponenten installiert sind), indem Sie auf den **Pfeil** links neben der Plattform klicken.
Klicken Sie auf den **Dropdownpfeil**, um die Auflistung der Komponenten der Plattform wieder einzuklappen.


Fügen Sie dem SDK eine weitere Plattform hinzu, indem Sie neben der Plattform auf das Kontrollkästchen klicken, bis das Häkchen angezeigt wird, und klicken Sie dann auf **Änderungen anwenden**:


![Beispiel für das Hinzufügen von Android 4.4-Komponenten zu Android SDK](android-sdk-images/mac/sdkmanager-12.png)


Wenn Sie nur das SDK installieren wollen, klicken Sie einmal auf das Kontrollkästchen neben der Plattform. Dann können Sie auswählen, welche Komponenten Sie benötigen:


![Beispiel für das Hinzufügen von Android 4.4-Komponenten](android-sdk-images/mac/sdkmanager-13.png)




Beachten Sie, dass die Anzahl der zu installierenden Komponenten neben der Schaltfläche **Änderungen anwenden** angezeigt wird. Wenn Sie auf **Änderungen anwenden** klicken, wird das Fenster **Zustimmung zur Lizenz** angezeigt:



![Dialogfeld „Zustimmung zur Lizenz“ mit Registerkarte „Plattformen“](android-sdk-images/mac/sdkmanager-14.png)


Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Möglicherweise wird dieses Dialogfeld mehrmals angezeigt, wenn Sie mehrere Komponenten installieren. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Sobald der Download und der Installationsprozess beendet sind (das kann abhängig von der Menge der herunterzuladenden Komponenten einige Minuten beanspruchen), werden die hinzugefügten Komponenten mit einem Häkchen versehen und als **Installiert** aufgelistet.

-----

 
## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde erklärt, wie Sie den Xamarin Android SDK-Manager in Visual Studio und Visual Studio für Mac installieren und verwenden.


## <a name="related-links"></a>Verwandte Links

- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
