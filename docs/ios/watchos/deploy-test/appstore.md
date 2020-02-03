---
title: Bereitstellen von watchos-apps im App Store
description: In diesem Dokument wird beschrieben, wie Sie watchos-apps, die mit xamarin erstellt wurden, im App Store bereitstellen. Hier finden Sie einen Einblick in die Verteilungs Bereitstellungs Profile und iTunes Connect. Außerdem finden Sie hier einige Tipps zur Problembehandlung.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: a622684461bfe2e4a57b910288ee1f9afb54c694
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725122"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>Bereitstellen von watchos-apps im App Store

> [!IMPORTANT]
> Informieren Sie sich über das [Apple Watch Kit-](https://developer.apple.com/app-store/watch/)Übermittlungs Handbuch, und lesen Sie den Abschnitt zur [Problem](#troubleshooting) Behandlung für Probleme, die Sie möglicherweise haben.

- Stellen Sie Folgendes sicher:
  - [**Verteilungs Bereitstellungs profile**](#provisioning) , die für Ihre Projekte erstellt wurden.
  - Das **Bereitstellungs Ziel** (`MinimumOSVersion`) für die übergeordnete IOS-APP, die auf **8,2** oder früher festgelegt ist (8,3 wird nicht unterstützt).

- In [**iTunes Connect**](#iTunes_Connect):

  - Erstellen Ihres IOS-App-Eintrags (oder Hinzufügen einer **neuen Version** zu einer vorhandenen APP).
  - Fügen Sie das Überwachungs Symbol und Screenshots hinzu.

- In [Visual Studio für Mac](#xamarin_studio) (Visual Studio wird derzeit nicht unterstützt):

  - Klicken Sie mit der rechten Maustaste auf die IOS-APP und wählen Sie **als Startprojekt festlegen**.
  - Wechseln Sie zur **App Store** -Konfiguration.
  - Verwenden Sie die Funktion **Archive** , und erstellen Sie ein Anwendungs Archiv.

- Wechseln Sie zum Schluss zu [Xcode 6.2](#xcode) und höher.

  - Wechseln Sie zum **Fenster > Planer** , und wählen Sie **Archive**aus.
  - Wählen Sie die Anwendung und das Archiv aus der Liste aus.
  - Optional Überprüfen **...** das Archiv.
  - Über **Mitteln...** Führen Sie das Archiv aus, und befolgen Sie die Schritte zum Hochladen in iTunes Connect zur Überprüfung und Genehmigung.

Informieren Sie sich über spezifische Tipps im Zusammenhang mit diesen Elementen. Wenn Probleme auftreten, finden Sie weitere Informationen im Abschnitt zur [Problem](#troubleshooting) Behandlung.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Verteilungs Bereitstellungs profile

Zum Erstellen einer App Store-Bereitstellung müssen Sie ein **Verteilungs Bereitstellungs Profil** für jede APP-ID in ihrer Projekt Mappe erstellen.

Wenn Sie über eine Platzhalter-APP-ID verfügen, *wird nur ein Bereitstellungs Profil benötigt*. Wenn Sie jedoch über eine separate App-ID für jedes Projekt verfügen, benötigen Sie ein Bereitstellungs Profil für jede APP-ID:

![](appstore-images/provisioningprofile-distribution-sml.png "The App Store Distribution profile")

Nachdem Sie alle drei Profile erstellt haben, werden Sie in der Liste angezeigt. Denken Sie daran, die einzelnen Anwendungen herunterzuladen und zu installieren (indem Sie darauf doppelklicken):

![](appstore-images/provisioningprofiles-sml.png "The list of available profiles")

Sie können das Bereitstellungs Profil in den **Projektoptionen** überprüfen, indem Sie den Bildschirm **Build > IOS Bundle Signing** auswählen und die Konfiguration **AppStore | iPhone** auswählen.

In der Liste der **Bereitstellungs profile** werden alle übereinstimmenden Profile angezeigt. Sie sollten die entsprechenden Profile sehen, die Sie in dieser Dropdown Liste erstellt haben.

![](appstore-images/options-selectprofile-sml.png "The iOS Bundle Signing dialog")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Befolgen Sie die [Übersicht über die APP-Verteilung](~/ios/deploy-test/app-distribution/index.md), insbesondere:

- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Wenn Sie die app in iTunes Connect konfigurieren, vergessen Sie nicht, das Überwachungs Symbol und die Screenshots hinzuzufügen:

![](appstore-images/itunesconnect-watch-sml.png "The Watch icon and screenshots in iTunes Connect")

Die Symbol Datei muss 1024 x 1024 Pixel enthalten, und es wird eine zirkuläre Maske darauf angewendet, wenn Sie angezeigt wird. Das Symbol darf keinen Alphakanal aufweisen.

Mindestens ein Screenshot ist erforderlich, bis zu fünf können übermittelt werden.
Sie sollten eine Auflösung von eine Auflösung von einer Auflösung von mehr als 4 Pixel aufweisen
Sie können den 42mm-Überwachungs Simulator verwenden, um Screenshots mit dieser Größe zu erstellen.

<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Stellen Sie sicher, dass die IOS-APP das Startprojekt ist. Wenn dies nicht der Fall ist, klicken Sie mit der rechten Maustaste darauf:

   ![](appstore-images/xs-startup.png "Setting the startup project")

2. Wählen Sie die **AppStore** -Buildkonfiguration aus:

   ![](appstore-images/xs-appstore.png "The AppStore build configuration")

3. Wählen Sie das Menü Element **Build > Archive** aus, um den archivprozess zu starten:

   ![](appstore-images/xs-archive.png "The Build menu")

Sie können auch das Menü Element **Ansicht > Archive...** auswählen, um die zuvor erstellten Archive anzuzeigen.

  ![](appstore-images/xs-archives-sml.png "The Archives view")

<a name="xcode" />

## <a name="xcode"></a>Xcode

In Xcode werden Archive automatisch angezeigt, die in Visual Studio für Mac erstellt wurden.

1. Starten Sie Xcode, und wählen Sie **Fenster > Planer**aus:

   ![](appstore-images/xc-organizer.png "The Window menu")

2. Wechseln Sie zur Registerkarte **Archive** , und wählen Sie das Archiv aus, das mit Visual Studio für Mac erstellt wurde:

   ![](appstore-images/xc-archives.png "The Archives tab")

3. Optional können Sie das Archiv überprüfen **...** , und wählen Sie dann **Submit...** aus, um die app in iTunes Connect hochzuladen.

4. Wählen Sie das Entwicklungsteam aus (wenn Sie zu mehr als einem gehören), und bestätigen Sie dann die Übermittlung:

   ![](appstore-images/xc-submit1.png "The development team section")

5. Besuchen Sie iTunes Connect erneut, um die hochgeladene Binärdatei anzuzeigen. Wechseln Sie zur Konfigurationsseite Ihrer APP, und wählen Sie im oberen Menü **vorab** Version aus, um die Liste der **Builds** anzuzeigen:

   [![](appstore-images/itc-prerelease-sml.png "The apps configuration page in iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Anschließend können Sie die APP zur Genehmigung auf der Seite **Versionen** übermitteln. Weitere Informationen finden Sie in der [Übersicht über die IOS-App-Verteilung](~/ios/deploy-test/app-distribution/index.md) .

## <a name="troubleshooting"></a>Problembehandlung

Im folgenden finden Sie einige Fehler, die bei der Übermittlung an den App Store auftreten können, sowie die Schritte, die Sie ausführen können, um Sie zu beheben.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Die Menüoption "Archive" ist in Visual Studio für Mac nicht sichtbar.

Führen Sie die [obigen Schritte](#xamarin_studio) aus, um die Lösung für die Archivierung zu konfigurieren. Wenn Sie das Startprojekt nicht korrekt festlegen können, stellen Sie sicher, dass die Buildkonfiguration zuerst auf Debug oder Release festgelegt wird, bevor Sie versuchen, das Startprojekt zu ändern. Legen Sie dann die Buildkonfiguration auf **AppStore**zurück.

### <a name="invalid-icon"></a>Symbol „Ungültig“

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Befolgen Sie die [Anweisungen zum Entfernen des Alphakanals](~/ios/watchos/troubleshooting.md) aus ihren Symbolen.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion stimmt nicht überein.

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Alle Projekte in der Projekt Mappe: die IOS-APP, die Watch-Erweiterung und die Watch-App sollten dieselbe Versionsnummer verwenden. Bearbeiten Sie jede **Info. plist** -Datei so, dass die Versionsnummer genau übereinstimmt.

### <a name="missing-icons"></a>Fehlende Symbole

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Befolgen Sie die Anweisungen im Abschnitt [Arbeiten mit Symbolen](~/ios/watchos/app-fundamentals/icons.md) , um alle erforderlichen Images dem App-Überwachungsprojekt hinzuzufügen.

### <a name="missing-icon"></a>Fehlendes Symbol

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Stellen Sie sicher, dass Sie über die neueste Version von Visual Studio für Mac verfügen und dass Ihr **AppIcon. appifiset** einen vollständigen Satz von Bildern enthält. Wenn dieser Fehler weiterhin angezeigt wird, sehen Sie sich die Quelle der **Datei "Content. JSON** " an, um zu bestätigen, dass Sie einen Eintrag für alle erforderlichen Images enthält. Wenn Sie sichergestellt haben, dass Sie die neueste Version von xamarin verwenden, löschen Sie die **AppIcon. appianset-Datei**, und erstellen Sie Sie neu.

> [!IMPORTANT]
> Es gibt einen bekannten Fehler in Visual Studio für Mac Unterstützung für das Überwachen von Symbolen: Sie erwartet ein 88x88-Pixel Bild für das **29x29@3x** Bild (in der Größe 87x 87 Pixel).

Dies kann in Visual Studio für Mac nicht behoben werden. Bearbeiten Sie entweder das Image-Asset in Xcode, oder bearbeiten Sie die Datei " **Content. JSON** " manuell.

### <a name="invalid-watchkit-support"></a>Ungültige watchkit-Unterstützung

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Diese Meldung wird möglicherweise während der Validierung und Übermittlung oder in einer automatisierten e-Mail von iTunes Connect nach einem anscheinend erfolgreichen Upload angezeigt.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Sie müssen Ihre APP in Visual Studio für Mac **archivieren** und dann zu Xcode 6.2 + wechseln, um iTunes Connect zu validieren und hochzuladen.

Verwenden Sie den stabilen xamarin-Kanal und Xcode 6.2 und höher.

### <a name="invalid-provisioning-profile"></a>Ungültiges Bereitstellungs Profil

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Verteilungs Bereitstellungs profile** müssen für alle drei Projekte in einer Watch-App-Lösung bereitgestellt werden: die IOS-APP, die Watch-Erweiterung und die Watch-APP, entweder explizit (drei Profile) oder über ein einzelnes Platzhalter Profil. Überprüfen Sie, ob die Bereitstellungs Profile im IOS dev Center vorhanden sind und Sie Sie heruntergeladen und Ihrem Mac hinzugefügt haben.

### <a name="invalid-code-signing-entitlements"></a>Ungültige Code Signatur Berechtigungen.

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Stellen Sie sicher, dass Ihre Bereitstellungs profile ordnungsgemäß im Apple dev Center eingerichtet sind und dass Sie Sie heruntergeladen und installiert haben. Überprüfen Sie auch, ob Sie für jedes Projekt im Eigenschaften Fenster Visual Studio für Mac festgelegt sind.

### <a name="invalid-architecture"></a>Ungültige Architektur

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Sie können nur Watch-apps [Unified API (64-Bit)](~/cross-platform/macios/unified/index.md) xamarin. IOS-apps hinzufügen.
Klicken Sie mit der rechten Maustaste auf das IOS-App-Projekt, und navigieren Sie zu **Optionen > > IOS-Build > Registerkarte "erweitert** ". Stellen Sie sicher, dass die **unterstützten Architekturen** für die Konfiguration "AppStore-iPhone" **ARM64** **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Dieses Bündel ist ungültig.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Für ihre übergeordnete IOS-Anwendung muss die minimumosversion auf "8,2" oder früher festgelegt sein.

### <a name="non-public-api-usage"></a>Nicht öffentliche API-Verwendung

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Stellen Sie sicher, dass Sie die neueste Version von Xcode und xamarin-Tools verwenden.
Ihr Code sollte nicht auf nicht öffentliche APIs zugreifen.

### <a name="build-error-mt5309"></a>Buildfehler MT5309

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Dieser Fehler ist wahrscheinlich darauf zurückzuführen, dass Ihre Xcode-Installation von **Xcode. app**umbenannt wurde. Dieser Fehler tritt beispielsweise auf, wenn Sie die-Installation in **Xcode 6.2. app**umbenennen.

## <a name="related-links"></a>Verwandte Links

- [Leitfaden für die Apple watchkit-Übermittlung](https://developer.apple.com/app-store/watch/)
