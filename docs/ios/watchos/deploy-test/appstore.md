---
title: Bereitstellen von WatchOS-Apps für den App Store
description: Dieses Dokument beschreibt, wie Sie WatchOS-apps mit Xamarin auf dem App Store bereitstellen. Es dauert einen Blick auf das Verteilen von bereitstellungsprofilen und iTunes Connect, und es bietet auch einige Tipps zur Problembehandlung.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: bf86759832a1aba0ccc1c144981af6ea4eae8670
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61343706"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>Bereitstellen von WatchOS-Apps für den App Store

> [!IMPORTANT]
> Überprüfen Sie [Apple Watch Kit Übermittlung Guide](https://developer.apple.com/app-store/watch/), und finden Sie unter den [Problembehandlung](#troubleshooting) Abschnitt für alle Probleme, die Sie möglicherweise.

- Stellen Sie sicher, dass Sie haben:
  - [**Verteilungsprofile für Bereitstellung** ](#provisioning) für Ihre Projekte erstellt.
  - Die **Bereitstellungsziel** (`MinimumOSVersion`) für den iOS app-Gruppe, um übergeordnete **8.2** oder früher (8.3 wird nicht unterstützt).

- In [ **iTunes Connect**](#iTunes_Connect):

  - Erstellen Sie Ihre iOS-app-Eintrag (oder Hinzufügen einer **neue Version** zu einer vorhandenen app).
  - Symbol "überwachen" und Screenshots hinzufügen.

- Klicken Sie dann im [Visual Studio für Mac](#xamarin_studio) (Visual Studio wird derzeit nicht unterstützt):

  - Mit der rechten Maustaste in der iOS-app, und wählen Sie **als Startprojekt festlegen**.
  - Ändern Sie in der **App Store** Konfiguration.
  - Verwenden der **Archiv** Feature Anwendung Archiv erstellt.

- Wechseln Sie zum Schluss zu [Xcode 6.2 und höher](#xcode)

  - Wechseln Sie zu der **Fenster > Organisator** , und wählen Sie **Archive**.
  - Wählen Sie die Anwendung und das Archiv aus der Liste aus.
  - (Optional) **Überprüfen...**  das Archiv.
  - **Senden...**  "Archiv" und führen Sie die Schritte zum Hochladen in iTunes für die Überprüfung und Genehmigung Connect.

Lesen Sie die spezifische Tipps, die im Zusammenhang mit diesen nachfolgenden Elemente. Finden Sie unter den [Problembehandlung](#troubleshooting) Abschnitt, wenn Sie Probleme haben.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Bereitstellungsprofile Verteilung

Für die Bereitstellung von App-Store erstellen, Sie erstellen müssen, eine **Verteilungsbereitstellungsprofil** für jede App-ID in der Projektmappe.

Wenn Sie einen Platzhalter-App-ID, haben *nur ein Bereitstellungsprofil wird erforderlich sein,*; aber wenn Sie eine separate App-ID für jedes Projekt benötigen Sie ein Bereitstellungsprofil für jede App-ID:

![](appstore-images/provisioningprofile-distribution-sml.png "Das App Store-Verteilungsprofil")

Nachdem Sie alle drei Profile erstellt haben, werden sie in der Liste angezeigt. Denken Sie daran, herunterladen und installieren jeweils (durch Doppelklicken auf diese):

![](appstore-images/provisioningprofiles-sml.png "Die Liste der verfügbaren Profile")

Sie können überprüfen, ob das Bereitstellungsprofil in die **Projektoptionen** durch Auswählen der **erstellen > iOS Bundle-Signierung** Bildschirm- und Auswählen der **AppStore | iPhone** die Konfiguration.

Die **Bereitstellungsprofil** Liste werden alle übereinstimmenden Profile angezeigt – sollte die entsprechenden Profilen, die Sie in der Dropdown-Liste erstellt haben.

![](appstore-images/options-selectprofile-sml.png "Das Dialogfeld für die iOS-Bundle-Signierung")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Führen Sie die [Übersicht über app-Verteilung](~/ios/deploy-test/app-distribution/index.md), insbesondere:

- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Wenn die app in iTunes Connect konfigurieren zu können, vergessen Sie nicht auf das Symbol "überwachen" und die Screenshots hinzufügen:

![](appstore-images/itunesconnect-watch-sml.png "Das Symbol \"überwachen\" und die Screenshots in iTunes Connect")

Die Symboldatei muss 1024 x 1024 Pixel, und weist eine zirkuläre Maske, die darauf angewendet werden, wenn es angezeigt wird. Das Symbol sollte nicht auf einen alpha-Kanal verfügen.

Mindestens einen Screenshot ist erforderlich, bis zu fünf übermittelt werden kann.
Sie sollten 312 x 390 Pixel und die Watch-App in Aktion zeigen.
Simulator sehen Sie sich 42 mm können Screenshots dieser Größe erstellen.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Stellen Sie sicher, dass die iOS-app das Startprojekt ist. Wenn dies nicht der Fall ist, mit der rechten Maustaste, um es festzulegen:

  ![](appstore-images/xs-startup.png "Festlegen des Startprojekts")

2. Wählen Sie die **AppStore** Buildkonfiguration:

  ![](appstore-images/xs-appstore.png "Die App Store-Buildkonfiguration")

3. Wählen Sie die **erstellen > archivieren** Menüelement, um das Archivierungsprozess zu starten:

  ![](appstore-images/xs-archive.png "Im Menü erstellen")

Sie können auch die **Ansicht > Archiven...**  Menüelement Archive anzuzeigen, die zuvor erstellt wurden.

  ![](appstore-images/xs-archives-sml.png "Die Ansicht \"Archive\"")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode werden in Visual Studio für Mac erstellte Archive automatisch angezeigt werden.

1. Starten Sie Xcode, und wählen Sie **Fenster > Organisator**:

  ![](appstore-images/xc-organizer.png "Klicken Sie im Menü Fenster")

2. Wechseln Sie zu der **Archive** Registerkarte, und wählen Sie das Archiv, das mit Visual Studio für Mac erstellt wurde:

  ![](appstore-images/xc-archives.png "Die Registerkarte \"Archive\"")

3. Optional **überprüfen...**  das Archiv, wählen Sie dann **senden...**  zum Hochladen der app in iTunes Connect.

4. Wählen Sie das Entwicklungsteam (Wenn Sie mehrere angehören) und bestätigen Sie dann auf die Übermittlung:

  ![](appstore-images/xc-submit1.png "Abschnitt Team-Entwicklung")

5. Besuchen Sie iTunes Connect erneut aus, um die hochgeladenen Binärdatei finden Sie unter. Wechseln Sie zu Ihrer app-Konfigurationsseite, und wählen Sie **Vorabversion** im oberen Menü auf finden Sie unter den **erstellt** Liste:

  [![](appstore-images/itc-prerelease-sml.png "Die Seite \"Konfiguration apps\" in iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Sie können dann die app zur Genehmigung senden, auf die **Versionen** Seite. Finden Sie in der [Übersicht über die Verteilung von iOS-app](~/ios/deploy-test/app-distribution/index.md) für Weitere Informationen.


## <a name="troubleshooting"></a>Problembehandlung

Hier sind einige Fehler, die möglicherweise auftreten, bei der Übergabe an den App Store, und die Schritte, die Sie zur Problembehebung ergreifen können.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Option für das Archiv ist nicht sichtbar, in Visual Studio für Mac

Führen Sie die [oben genannten Schritte](#xamarin_studio) zum Konfigurieren der Lösung zum Archivieren. Wenn Sie das Startprojekt können nicht ordnungsgemäß festgelegt, stellen Sie sicher, dass die Buildkonfiguration zuerst festgelegt ist, zu debuggen oder freigeben, bevor Sie versuchen, das Startprojekt ändern. Legen Sie die Buildkonfiguration an **AppStore**.

### <a name="invalid-icon"></a>Symbol "ungültig"

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Führen Sie die [Anleitungen zum Entfernen des alpha-Kanals](~/ios/watchos/troubleshooting.md) aus Ihrer Symbole.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion-Konflikt

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Alle Projekte in der Projektmappe ein iOS-App, der Watch-Erweiterung, und der Watch-App - sollten die gleiche Versionsnummer verwenden. Bearbeiten Sie **"Info.plist"** Datei, sodass die Versionsnummer genau übereinstimmt.

### <a name="missing-icons"></a>Fehlende Symbole

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Befolgen Sie die Anweisungen in der [arbeiten mit Symbolen](~/ios/watchos/app-fundamentals/icons.md) der Watch-App-Projekt alle erforderlichen Bilder hinzu.

### <a name="missing-icon"></a>Fehlendes Symbol

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Stellen Sie sicher, Sie haben die neueste Version von Visual Studio für Mac, und dass Ihre **AppIcon.appiconset** enthält einen vollständigen Satz von Bildern. Wenn dieser Fehler weiterhin angezeigt werden, zeigen Sie die Quelle des der **Contents.json** , er enthält einen Eintrag für alle erforderlichen Bilder zu bestätigen. Auch nachdem Sie sichergestellt haben, verwenden Sie die neueste Version von Xamarin, löschen und Neuerstellen der **AppIcon.appiconset**.

> [!IMPORTANT]
> Besteht ein bekanntes Problem in Visual Studio für Mac sehen Sie sich Symbol-Unterstützung: es erwartet, dass ein Bild 88 x 88 Pixel für die **29x29@3x** Image (87 x 87-Pixel sein sollte).


Sie beheben dies in Visual Studio für Mac – entweder Bearbeiten der Bildobjekt im Xcode oder manuell bearbeiten, können nicht die **Contents.json** Datei (entsprechend [in diesem Beispiel](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Ungültige WatchKit-Unterstützung

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Diese Meldung kann während der Validierung und Übermittlung oder in eine automatisierte e-Mail von iTunes Connect nach dem Hochladen scheinbar erfolgreich angezeigt.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Sie müssen **Archiv** Ihrer app in Visual Studio für Mac, und wechseln Sie dann zum Xcode 6.2 und höher, um zu überprüfen und Hochladen in iTunes Connect.


Verwenden Sie die stabile Xamarin-Kanal und Xcode 6.2 und höher.



### <a name="invalid-provisioning-profile"></a>Ungültige Bereitstellungsprofil

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Verteilungsprofile für Bereitstellung** muss für alle drei Projekte in einer Watch-app-Projektmappe bereitgestellt werden: iOS-App, der Watch-Erweiterung und der Watch-App – entweder explizit (drei Profile) oder über ein einzelnes Platzhalterzeichen-Profil. Überprüfen Sie, dass die bereitstellungsprofile in der iOS Developer Center vorhanden sind, und dass Sie heruntergeladen und dem Mac hinzugefügt haben

### <a name="invalid-code-signing-entitlements"></a>Ungültiger Berechtigungen für die Codesignatur

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Stellen Sie sicher, bereitstellungsprofilen Einrichtung ordnungsgemäß auf die Apple Dev Center werden und dass Sie heruntergeladen und installiert haben. Überprüfen Sie auch, dass sie in Visual Studio für Mac Fenster "Eigenschaften" für jedes Projekt festgelegt sind.

### <a name="invalid-architecture"></a>Ungültiger-Architektur

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Sie können nur Watch-Apps hinzufügen [Unified API (64-Bit)](~/cross-platform/macios/unified/index.md) Xamarin.iOS-apps.
Mit der rechten Maustaste auf das iOS-app-Projekt finden Sie unter **Optionen > Erstellen > iOS-Build > Registerkarte "Erweitert"** und sicherstellen, dass die **unterstützte Architekturen** für das App Store-iPhone-Konfiguration umfasst **ARM64** (z. b. **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Dieses Paket ist ungültig.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Die übergeordnete iOS-Anwendung muss die MinimumOSVersion "8.2" oder ein zuvor festgelegt haben.

### <a name="non-public-api-usage"></a>Nicht öffentliche API-Nutzung

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Stellen Sie sicher, dass Sie die neueste Version von Xcode und Xamarin Tools verwenden.
Ihr Code sollten keine nicht öffentlichen APIs nicht zugreifen.

### <a name="build-error-mt5309"></a>Fehler-MT5309 erstellen

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Dieser Fehler ist vermutlich das Ergebnis Ihrer müssen umbenannt Ihr Xcode-Installation von **Xcode.app**. Dieser Fehler wird z. B. auftreten, wenn Sie die Installation auf Umbenennen **XCode 6.2.app**.



## <a name="related-links"></a>Verwandte Links

- [Apple WatchKit-Übermittlung-Handbuch](https://developer.apple.com/app-store/watch/)
