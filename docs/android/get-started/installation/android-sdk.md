---
title: Einrichten des Android SDK für Xamarin.Android
description: In Visual Studio ist der SDK-Manager enthalten, mit dem Sie Tools, Plattformen und andere Komponenten des Android SDK herunterladen können, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/09/2018
ms.openlocfilehash: da0a441be9cd07af456b1600155151e48d44162c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758062"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Einrichten des Android SDK für Xamarin.Android

_Visual Studio enthält den SDK-Manager, mit dem Sie Tools, Plattformen und andere Komponenten des Android SDK herunterladen können, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen._

## <a name="overview"></a>Übersicht

In diesem Handbuch wird erklärt, wie Sie den Xamarin Android SDK-Manager in Visual Studio und in Visual Studio für Mac verwenden.

> [!NOTE]
> Dieser Leitfaden gilt nur für Visual Studio 2019, Visual Studio 2017 und Visual Studio für Mac.  

Der Xamarin Android SDK-Manager (der als Teil der Workload **Mobile-Entwicklung mit.NET** installiert wird) unterstützt Sie beim Herunterladen der aktuellsten Komponenten, die Sie für die Entwicklung Ihrer Xamarin.Android-App benötigen. Er ersetzt den eigenständigen SDK-Manager von Google, der als veraltet markiert wurde.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="requirements"></a>Requirements (Anforderungen)

Sie benötigen Folgendes, um den Xamarin Android SDK-Manager verwenden zu können:

- Visual Studio 2019 Community, Professional oder Enterprise

- ODER Visual Studio 2017 Community, Professional oder Enterprise Visual Studio 2017, Version 15.7 oder höher.

- Visual Studio-Tools für Xamarin, Version 4.10.0 oder höher (werden im Rahmen der Workload **Mobile-Entwicklung mit .NET** installiert). 

Der Xamarin Android SDK Manager benötigt auch das Java Development Kit (das automatisch mit Xamarin.Android installiert wird). Es gibt verschiedene JDK-Alternativen, die Sie auswählen können:

- Xamarin.Android verwendet standardmäßig [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch niedrigere API-Ebenen als Ebene 24).

- Sie können weiterhin [JDK 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

- Wenn Sie Visual Studio 15.8, Vorschauversion 5 oder höher, verwenden, können Sie auch die [Mobile OpenJDK-Distribution von Microsoft ](openjdk.md) (derzeit in der Vorschauversion) anstelle von JDK 8 testen.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

## <a name="sdk-manager"></a>SDK-Manager 

Klicken Sie auf **Extras > Android > Android SDK-Manager**, um den SDK-Manager in Visual Studio zu starten:

[![Ort des Menüelements „Android SDK-Manager“](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Der Android SDK-Manager wird im Fenster **Android SDKs und Tools** geöffnet. Auf dieser Seite gibt es die zwei Registerkarten &ndash; **Plattformen** und **Tools**:

[![Screenshot vom Android SDK-Manager mit geöffneter Registerkarte „Plattformen“](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

Das Fenster **Android SDKs und Tools** wird in den folgenden Abschnitten ausführlicher beschrieben.

### <a name="android-sdk-location"></a>Speicherort von Android SDK

Der Speicherort von Android SDK ist, wie Sie im vorherigen Screenshot sehen, oben im Fenster **Android SDKs und Tools** konfigurierbar. Dieser Speicherort muss korrekt angegeben sein, damit die Registerkarten **Plattformen** und **Tools** richtig funktionieren können. Es kann sein, dass Sie den Speicherort von Android SDK aus mindestens einem der folgenden Gründe angeben müssen:

1. Der Android SDK-Manager konnte Android SDK nicht finden. 

2. Sie haben Android SDK an einem anderen (nicht dem Standard entsprechenden) Ort gespeichert. 

Klicken Sie auf die Schaltfläche mit den Auslassungszeichen (&hellip;) rechts neben dem **Android SDK-Speicherort**, um den Speicherort von Android SDK festzulegen. Daraufhin wird das Dialogfeld **Nach Ordner suchen** geöffnet. Navigieren Sie darin zum Speicherort von Android SDK. Im folgenden Screenshot wird Android SDK unter **Programme (x86)\\Android** ausgewählt:

![Screenshot, auf dem Android SDK im Dialogfeld „Nach Ordner suchen“ von Windows ausgewählt ist](android-sdk-images/win/05-browse-for-folder.png)

Wenn Sie dann auf **OK** klicken, verwaltet der SDK-Manager die von Ihnen ausgewählte Installation von Android SDK am angegebenen Speicherort.

### <a name="tools-tab"></a>Registerkarte „Extras“

Die Registerkarte **Tools** zeigt eine Liste von _Tools_ und _Extras_. Auf dieser Registerkarte können Sie Tools, Plattformtools und Buildtools von Android SDK installieren.
Darüber hinaus können Sie Android-Emulator, den LLDB (Low-Level-Debugger), das NDK, die HAXM-Beschleunigung und Google Play-Bibliotheken installieren.

Laden Sie zum Beispiel das Google Android-Emulator-Paket herunter, indem Sie auf das Kontrollkästchen neben **Android-Emulator** und dann auf **Änderungen anwenden** klicken:

[![Installieren von Android-Emulator über die Registerkarte „Tools“](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

Möglicherweise wird ein Dialogfeld mit der folgenden Meldung angezeigt: _The following package requires that you accept its license terms before installing._ (Sie müssen die Lizenzbedingungen für das folgende Paket akzeptieren, damit Sie es installieren können.):

![Bildschirm „Zustimmung zur Lizenz“](android-sdk-images/win/07-license-acceptance.png)

Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Nach Abschluss der Installation zeigt die Registerkarte **Tools** an, dass die ausgewählten Tools und Extras installiert wurden.

### <a name="platforms-tab"></a>Registerkarte „Plattformen“

Die Registerkarte **Plattformen** zeigt eine Liste der Versionen des Platform SDK und andere Ressourcen (beispielsweise Systemimages) für die einzelnen Plattformen an:

[![Screenshot des Bereichs „Plattformen“](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

In dieser Anzeige werden die Android-Version (z.B. **Android 8.0**), der Codename (z.B. **Oreo**), die API-Ebene (z.B. **26**) und die Komponentengrößen für diese Plattform (z.B. **1 GB**) aufgelistet. Verwenden Sie die Registerkarte **Plattformen**, um die Komponenten für die Android API-Ebene zu installieren, die Sie als Ziel verwenden möchten. Weitere Informationen zu Android-Versionen und API-Ebenen finden Sie unter [Understanding Android API Levels (Grundlegendes zu Android-API-Ebenen)](~/android/app-fundamentals/android-api-levels.md).

Wenn alle Komponenten einer Plattform installiert sind, wird neben dem Namen der Plattform ein Häkchen angezeigt. Wenn nicht alle Komponenten installiert sind, wird das Kontrollkästchen gefüllt. Sie können die Plattform erweitern, um deren Komponenten anzuzeigen (und welche Komponenten installiert sind), indem Sie auf die Schaltfläche **+** links neben der Plattform klicken.
Klicken Sie auf **-** , um die Auflistung der Komponenten der Plattform wieder einzuklappen.

Fügen Sie dem SDK eine weitere Plattform hinzu, indem Sie neben der Plattform auf das Kontrollkästchen klicken, bis das Häkchen angezeigt wird, und klicken Sie dann auf **Änderungen anwenden**:

[![Beispiel für das Hinzufügen von Android 7.1 Nougat-Komponenten zu Android SDK](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

Wenn Sie nur bestimmte Komponenten installieren möchten, klicken Sie einmal auf das Kontrollkästchen neben der Plattform. Dann können Sie auswählen, welche Komponenten Sie benötigen:

[![Beispiel für das Hinzufügen von Android 7.1-Komponenten](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

Beachten Sie, dass die Anzahl der zu installierenden Komponenten neben der Schaltfläche **Änderungen anwenden** angezeigt wird. Wenn Sie auf die Schaltfläche **Änderungen anwenden** klicken, wird wie oben dargestellt das Fenster **Zustimmung zur Lizenz** angezeigt.
Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Möglicherweise wird dieses Dialogfeld mehrmals angezeigt, wenn Sie mehrere Komponenten installieren. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Sobald der Download und der Installationsprozess beendet sind (das kann abhängig von der Menge der herunterzuladenden Komponenten einige Minuten beanspruchen), werden die hinzugefügten Komponenten mit einem Häkchen versehen und als **Installiert** aufgelistet.

### <a name="repository-selection"></a>Repositoryauswahl

Der Android SDK-Manager lädt standardmäßig Plattformkomponenten und Tools aus dem von Microsoft verwalteten Repository herunter. Wenn Sie auf Plattformen in der Alpha- bzw. Betaphase und noch nicht verfügbare Tools im Microsoft-Repository zugreifen möchten, können Sie den SDK-Manager aktivieren, um das Repository von Google zu verwenden. Klicken Sie dafür erst auf das Zahnradsymbol unten rechts und anschließend auf **Repository > Google (nicht unterstützt)** :

[![Google-Repository auswählen](android-sdk-images/win/11-google-repo-w157-sml.png)](android-sdk-images/win/11-google-repo-w157.png#lightbox)

Wenn das Google-Repository ausgewählt wird, werden möglicherweise weitere Pakete in der Registerkarte **Plattformen** angezeigt, die zuvor nicht verfügbar waren. (Im Screenshot oben wurde **Android SDK Platform 28** hinzugefügt, indem das Google-Repository aktiviert wurde.) Bedenken Sie, dass die Verwendung des Google-Repositorys nicht unterstützt und daher auch nicht für die Entwicklung im Alltag empfohlen wird.

Wenn Sie wieder das unterstützte Repository der Plattformen und Tools aktivieren möchten, klicken Sie auf **Microsoft (empfohlen)** . Dadurch wird die Standardliste mit den Paketen und Tools wiederhergestellt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="requirements"></a>Requirements (Anforderungen)

Sie benötigen Folgendes, um den Xamarin Android SDK-Manager verwenden zu können:

- Visual Studio für Mac 7.5 (oder höher).

Der Xamarin Android SDK Manager benötigt auch das Java Development Kit (das automatisch mit Xamarin.Android installiert wird). Es gibt verschiedene JDK-Alternativen, die Sie auswählen können:

- Xamarin.Android verwendet standardmäßig [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch niedrigere API-Ebenen als Ebene 24).

- Sie können weiterhin [JDK 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

- Wenn Sie Visual Studio für Mac 7.7 oder höher verwenden, können Sie auch die [Mobile OpenJDK-Distribution von Microsoft ](openjdk.md) (derzeit in der Vorschauversion) anstelle von JDK 8 testen.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

## <a name="sdk-manager"></a>SDK-Manager 

Klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**, um den SDK-Manager zu starten:

[![Ort des Menüelements „Android SDK-Manager“](android-sdk-images/mac/01-sdk-manager-menu-item-m75-sml.png)](android-sdk-images/mac/01-sdk-manager-menu-item-m75.png#lightbox)

Der **Android SDK-Manager** wird im **Fenster „Einstellungen“** geöffnet, das die drei Registerkarten **Plattformen**, **Tools** und **Speicherorte** enthält:

[![Screenshot vom Android SDK-Manager mit geöffneter Registerkarte „Plattformen“](android-sdk-images/mac/02-sdk-manager-platforms-m75-sml.png)](android-sdk-images/mac/02-sdk-manager-platforms-m75.png#lightbox)

Die Registerkarten im Android SDK-Manager werden in den folgenden Abschnitten beschrieben.

### <a name="locations-tab"></a>Registerkarte „Speicherorte“

Die Registerkarte **Speicherorte** hat drei Einstellung zum Konfigurieren der Speicherorte von Android SDK, Android NDK und Java SDK (JDK). Diese Speicherorte müssen korrekt angegeben sein, damit die Registerkarten **Plattformen** und **Tools** richtig funktionieren.

Wenn der SDK-Manager gestartet wird, bestimmt er automatisch den Dateipfad für jedes installierte Paket und gibt an, dass sie **Gefunden** wurden, indem er ein grünes Hakensymbol neben dem Pfad anzeigt:

[![Screenshot der Registerkarte „Speicherorte“](android-sdk-images/mac/03-locations-tab-m75-sml.png)](android-sdk-images/mac/03-locations-tab-m75.png#lightbox)

Klicken Sie auf die Schaltfläche **Auf Standards zurücksetzen**, wenn der SDK-Manager an den Standardspeicherorten nach SDK, NDK und JDK suchen soll. 

Normalerweise verwenden Sie die Registerkarte **Speicherorte**, um den Speicherort von Android SDK und Java JDK zu ändern. Sie müssen das NDK nicht installieren, um Xamarin.Android-Apps entwickeln zu können. Das NDK benötigen Sie nur, wenn Sie Teile Ihrer App mit nativen Programmiersprachen wie C und C++ entwickeln müssen.

### <a name="tools-tab"></a>Registerkarte „Extras“

Die Registerkarte **Tools** zeigt eine Liste von _Tools_ und _Extras_. Auf dieser Registerkarte können Sie Tools, Plattformtools und Buildtools von Android SDK installieren.
Darüber hinaus können Sie Android-Emulator, den LLDB (Low-Level-Debugger), das NDK, die HAXM-Beschleunigung und Google Play-Bibliotheken installieren.

Laden Sie zum Beispiel das Google Android-Emulator-Paket herunter, indem Sie auf das Kontrollkästchen neben **Android-Emulator** und dann auf **Änderungen anwenden** klicken:

[![Installieren von Android-Emulator über die Registerkarte „Tools“](android-sdk-images/mac/04-tools-tab-m75-sml.png)](android-sdk-images/mac/04-tools-tab-m75.png#lightbox)

Möglicherweise wird ein Dialogfeld mit der folgenden Meldung angezeigt: _The following package requires that you accept its license terms before installing._ (Sie müssen die Lizenzbedingungen für das folgende Paket akzeptieren, damit Sie es installieren können.):

[![Fenster „Zustimmung zur Lizenz“](android-sdk-images/mac/05-license-acceptance-m75-sml.png)](android-sdk-images/mac/05-license-acceptance-m75.png#lightbox)

Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Nach Abschluss der Installation zeigt die Registerkarte **Tools** an, dass die ausgewählten Tools und Extras installiert wurden.

### <a name="platforms-tab"></a>Registerkarte „Plattformen“

Die Registerkarte **Plattformen** zeigt eine Liste der Versionen des Platform SDK und andere Ressourcen (beispielsweise Systemimages) für die einzelnen Plattformen an:

[![Screenshot des Bereichs „Plattformen“](android-sdk-images/mac/06-platforms-tab-m75-sml.png)](android-sdk-images/mac/06-platforms-tab-m75.png#lightbox)

In dieser Anzeige werden die Android-Version (z.B. **Android 8.1**), der Codename (z.B. **Oreo**), die API-Ebene (z.B. **27**) und die Komponentengrößen für diese Plattform (z.B. **1 GB**) aufgelistet. Verwenden Sie die Registerkarte **Plattformen**, um die Komponenten für die Android API-Ebene zu installieren, die Sie als Ziel verwenden möchten. Weitere Informationen zu Android-Versionen und API-Ebenen finden Sie unter [Understanding Android API Levels (Grundlegendes zu Android-API-Ebenen)](~/android/app-fundamentals/android-api-levels.md).

Wenn alle Komponenten einer Plattform installiert sind, wird neben dem Namen der Plattform ein Häkchen angezeigt. Wenn nicht alle Komponenten installiert sind, wird das Kontrollkästchen gefüllt. Sie können die Plattform erweitern, um deren Komponenten anzuzeigen (und welche Komponenten installiert sind), indem Sie auf den **Pfeil** links neben der Plattform klicken.
Klicken Sie auf den **Dropdownpfeil**, um die Auflistung der Komponenten der Plattform wieder einzuklappen.

Fügen Sie dem SDK eine weitere Plattform hinzu, indem Sie neben der Plattform auf das Kontrollkästchen klicken, bis das Häkchen angezeigt wird, und klicken Sie dann auf **Änderungen anwenden**:

[![Beispiel für das Hinzufügen aller Komponenten einer Plattform](android-sdk-images/mac/07-install-all-m75-sml.png)](android-sdk-images/mac/07-install-all-m75.png#lightbox)

Wenn Sie nur bestimmte Komponenten installieren möchten, klicken Sie einmal auf das Kontrollkästchen neben der Plattform. Dann können Sie auswählen, welche Komponenten Sie benötigen:

[![Beispiel für das Hinzufügen bestimmter Komponenten](android-sdk-images/mac/08-individual-components-m75-sml.png)](android-sdk-images/mac/08-individual-components-m75.png#lightbox)

Beachten Sie, dass die Anzahl der zu installierenden Komponenten neben der Schaltfläche **Änderungen anwenden** angezeigt wird. Wenn Sie auf die Schaltfläche **Änderungen anwenden** klicken, wird wie oben dargestellt das Fenster **Zustimmung zur Lizenz** angezeigt.
Klicken Sie auf **Akzeptieren**, wenn Sie den Geschäftsbedingungen zustimmen. Möglicherweise wird dieses Dialogfeld mehrmals angezeigt, wenn Sie mehrere Komponenten installieren. Am unteren Rand des Fensters gibt eine Statusleiste den Fortschritt des Downloads und der Installation an. Sobald der Download und der Installationsprozess beendet sind (das kann abhängig von der Menge der herunterzuladenden Komponenten einige Minuten beanspruchen), werden die hinzugefügten Komponenten mit einem Häkchen versehen und als **Installiert** aufgelistet.

### <a name="repository-selection"></a>Repositoryauswahl

Der Android SDK-Manager lädt standardmäßig Plattformkomponenten und Tools aus dem von Microsoft verwalteten Repository herunter. Wenn Sie auf Plattformen in der Alpha- bzw. Betaphase und noch nicht verfügbare Tools im Microsoft-Repository zugreifen möchten, können Sie den SDK-Manager aktivieren, um das Repository von Google zu verwenden. Klicken Sie dafür erst auf das Zahnradsymbol unten rechts und anschließend auf **Repository > Google (nicht unterstützt)** :

[![Google-Repository auswählen](android-sdk-images/mac/09-google-repo-m75-sml.png)](android-sdk-images/mac/09-google-repo-m75.png#lightbox)

Wenn das Google-Repository ausgewählt wird, werden möglicherweise weitere Pakete in der Registerkarte **Plattformen** angezeigt, die zuvor nicht verfügbar waren. (Im Screenshot oben wurde **Android SDK Platform 28** hinzugefügt, indem das Google-Repository aktiviert wurde.) Bedenken Sie, dass die Verwendung des Google-Repositorys nicht unterstützt und daher auch nicht für die Entwicklung im Alltag empfohlen wird.

Wenn Sie wieder das unterstützte Repository der Plattformen und Tools aktivieren möchten, klicken Sie auf **Microsoft (empfohlen)** . Dadurch wird die Standardliste mit den Paketen und Tools wiederhergestellt.

-----

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurde erklärt, wie Sie den Xamarin Android SDK-Manager in Visual Studio und Visual Studio für Mac installieren und verwenden.

## <a name="related-links"></a>Verwandte Links

- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Änderungen an den Tools des Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
