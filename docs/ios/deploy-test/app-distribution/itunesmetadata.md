---
title: Die Datei „iTunesMetadata.plist“ in Xamarin.iOS-Apps
description: In diesem Artikel wird die „iTunesMetadata.plist“-Datei vorgestellt, die verwendet wird, um iTunes Informationen zu einer iOS-Anwendung zu liefern, die die Ad-hoc-Verteilung entweder zu Testzwecken oder für die Unternehmensbereitstellung verwendet.
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: c03815776921a61c1f54136e3f09c0996dff71d3
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528415"
---
# <a name="the-itunesmetadataplist-file-in-xamarinios-apps"></a>Die Datei „iTunesMetadata.plist“ in Xamarin.iOS-Apps

_In diesem Artikel wird die iTunesMetadata.plist-Datei vorgestellt, die verwendet wird, um iTunes Informationen zu einer iOS-Anwendung zu liefern, die die Ad-hoc-Verteilung entweder zu Testzwecken oder für die Unternehmensbereitstellung verwendet._

Beim Erstellen einer iOS-Anwendung in iTunes Connect (entweder zum Verkauf oder für die kostenlose Veröffentlichung im iTunes App Store) kann der Entwickler die folgenden Angaben machen: Genre, Subgenre, Urheberrechtshinweis, unterstützte iOS-Geräte und Geräteanforderungen. iOS-Anwendungen, die entweder Testbenutzern oder Unternehmensbenutzern über die Ad-hoc-Verteilung bereitgestellt werden, fehlen diese Informationen.

Um die fehlenden Informationen einer Ad-hoc-Verteilung bereitzustellen, kann eine optionale `iTunesMetadata.plist`-Datei erstellt und in die IPA-Datei der Anwendung eingefügt werden. Diese PLIST-Datei ist eine speziell formatierte XML-Datei, die Schlüssel-Wert-Paare enthält, die Information zu einer angegebenen iOS-Anwendung enthalten (weitere Informationen finden Sie im [Property List Programming Guide (Programmierleitfaden zu Eigenschaftenlisten)](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html) von Apple).

<a name="iTunesMetadata_contents" />

## <a name="the-itunesmetadataplist-contents"></a>Inhalt der „iTunesMetadata.plist“-Datei

Hier sehen Sie ein Beispiel für eine typische `iTunesMetadata.plist`-Datei, die verwendet wird, um die iTunes-Informationen für eine Ad-hoc-Verteilung zu definieren:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

Die Werte der einzelnen Schlüssel werden weiter unten ausführlich behandelt.

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

Der Schlüssel `UIRequiredDeviceCapabilities` informiert iTunes darüber, welche gerätespezifischen Funktionen für eine iOS-Anwendung vor der Installation auf einem Gerät erforderlich sind. Er wird als Wörterbuch (`<dict>...</dict>`) aus Funktionen (`<key>...</key>`) mit jeweils einem booleschen Wert für jede Funktion bereitgestellt. Wenn der Wert einer Funktion `true` ist, muss die Funktion vorhanden sein. Wenn er `false` ist, muss die Funktion auf dem Gerät nicht vorhanden sein. Zum Beispiel:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```
Gibt an, dass das iOS-Gerät den ARM7-Befehlssatz unterstützen und über eine Frontkamera verfügen muss, damit die Anwendung auf dem Gerät installiert werden kann. Eine vollständige Liste der zulässigen Werte finden Sie in der Dokumentation zu [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) von Apple.

### <a name="artistname-and-playlistartistname"></a>artistName und playlistArtistName

Verwenden Sie die Schlüssel `artistName` und `playlistArtistName`, um den Namen des Unternehmens anzugeben, das die iOS-Anwendung entwickelt hat, der in iTunes angezeigt werden soll. Beispiel:

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName, itemName und playlistName

Verwenden Sie die Schlüssel `bundleDisplayName`, `itemName` und `playlistName`, um den Namen der iOS-Anwendung anzugeben, der in iTunes angezeigt werden soll. Beispiel:

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString und bundleVersion

Verwenden Sie die Schlüssel `bundleShortVersionString` und `bundleVersion`, um die Versionsnummer der iOS-Anwendung anzugeben, die in iTunes angezeigt werden soll. Beispiel:

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

Verwenden Sie den Schlüssel `softwareVersionBundleId`, um die Bundle-ID der iOS-Anwendung anzugeben. Beispiel:

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>Copyright

Verwenden Sie den Schlüssel `copyright`, um den Urheberrechtshinweis anzugeben, der in iTunes angezeigt werden soll. Beispiel:

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>releaseDate

Verwenden Sie den Schlüssel `releaseDate`, um das Veröffentlichungsdatum der iOS-Anwendung anzugeben, das in iTunes angezeigt werden soll. Beispiel:

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

Verwenden Sie den Schlüssel `softwareIconNeedsShine`, um iTunes darüber zu informieren, ob für das Symbol der iOS-Anwendung für iOS 6 (und früher) ein _glänzende Kennzeichnung_ erforderlich ist. Beispiel:

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled und gameCenterEverEnabled

Verwenden Sie die Schlüssel `gameCenterEnabled` und `gameCenterEverEnabled`, um iTunes darüber zu informieren, ob diese iOS-Anwendung das Game Center von Apple unterstützt. Beispiel:

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre, genreId und subgenres

Verwenden Sie die Schlüssel `genre` und `genreId`, um iTunes darüber zu informieren, welchem Genre die iOS-Anwendung zugeordnet werden kann. Beispiel:

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

Der Schlüssel `subgenres` kann optional auch verwendet werden, um zwei Subgenres für die iOS-Anwendung zu definieren. Beispiel:

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

Für iOS-Anwendungen stellt Apple zur Zeit folgende Genres und Genre-IDs zur Verfügung:

[!include[](~/ios/includes/table-appstore.md)]

Weitere Informationen finden Sie in der Dokumentation von Apple zu [Genre IDs Appendix (Genre-ID-Anhang)](http://www.apple.com/itunes/affiliates/resources/documentation/genre-mapping.html).

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

Verwenden Sie den Schlüssel `softwareSupportedDeviceIds`, um iTunes darüber zu informieren, welche iOS-Geräte von dieser iOS-Anwendung unterstützt werden. Beispiel:

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

Folgende Werte sind gültig:

- 1: Klassische iPhones
- 2: iPod Touch
- 4: iPad
- 9: Moderne iPhones

### <a name="standard-keys"></a>Standard Keys

Die folgenden Schlüssel werden in allen `iTunesMetadata.plist`-Dateien für iOS-Anwendungen mit einbezogen, und weisen immer die gleichen Werte auf:

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating" />

## <a name="creating-an-itunesmetadataplist-file"></a>Erstellen einer „iTunesMetadata.plist“-Datei

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 Wenn Sie in Visual Studio für Mac mit einer `iTunesMetadata.plist`-Datei arbeiten, haben Sie zwei Optionen:

- Sie können die Datei mit dem visuellen PLIST-Editor in Visual Studio für Mac erstellen und verwalten.
- Sie können die Datei in einem Nur-Text-Editor erstellen und verwalten.

 Beide Optionen werden unten ausführlich besprochen.

### <a name="using-the-visual-plist-editor"></a>Verwenden des visuellen PLIST-Editors

Führen Sie folgende Schritte aus:

1. Rechtsklicken Sie im **Projektmappen-Explorer** auf die Xamarin.iOS-Projektdatei, und klicken Sie dann auf **Hinzufügen** > **Neue Datei...**.
2. Klicken Sie im Dialogfeld „Neue Datei“ auf **iOS** > **Eigenschaftenliste**:

    ![](itunesmetadata-images/image01.png "Auswählen der iOS-Eigenschaftenliste")
3. Geben Sie für den **Namen** `iTunesMetadata` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie im `iTunesMetadata.plist`Projektmappen-Explorer**auf die Datei**, um sie zur Bearbeitung zu öffnen:

    ![](itunesmetadata-images/image02.png "Der iTunesMetadata.plist-Editor")
5. Klicken Sie auf das grüne **+**, um einen neuen Eintrag hinzuzufügen, oder geben Sie `UIRequiredDeviceCapabilities` als Schlüsselnamen ein:

    ![](itunesmetadata-images/image03.png "Erstellen eines neuen Eintrags und Eingeben des Schlüsselnamens „UIRequiredDeviceCapabilities“")
6. Klicken Sie auf den Werttyp **String**, und klicken Sie anschließend in der Popupliste auf **Wörterbuch**:

    ![](itunesmetadata-images/image04.png "Auswählen von „Wörterbuch“ aus der Popupliste")
7. Klicken Sie auf den Pfeil links vom Namen der Eigenschaft, um die Einträge des Wörterbuchs anzuzeigen:

    ![](itunesmetadata-images/image05.png "Anzeigen der Wörterbucheinträge")
8. Klicken Sie auf **Neuen Eintrag hinzufügen**, und klicken Sie dann auf das grüne **+**, um einen Eintrag im Wörterbuch hinzuzufügen:

    ![](itunesmetadata-images/image06.png "Hinzufügen eines Eintrags zum Wörterbuch")
9. Geben Sie `armv7` für den Schlüsselnamen ein, wählen Sie einen **booleschen Wert** aus, und geben Sie **YES** als Wert ein:

    ![](itunesmetadata-images/image07.png "Eingeben von „armv7“ für den Schlüsselnamen, auswählen eines booleschen Typs und auswählen von YES als Wert")
10. Wiederholen Sie die oben stehenden Schritte, bis Sie die `iTunesMetadata.plist`-Datei mit allen erforderlichen Schlüssel-Wert-Paaren ausgefüllt haben. Ausführlichere Informationen finden Sie im Abschnitt [Inhalt der „iTunesMetadata.plist“-Datei](#iTunesMetadata_contents) weiter oben.

11. Speichern Sie die Änderungen an der PLIST-Datei.

### <a name="using-a-plain-text-editor"></a>Verwenden eines Nur-Text-Editors

Führen Sie folgende Schritte aus:

1. Erstellen Sie in einem Nur-Text-Editor eine neue Textdatei mit dem Namen `iTunesMetadata.plist`.
2. Kopieren Sie den Beispielinhalt aus dem oben stehenden Abschnitt [Inhalt der „iTunesMetadata.plist“-Datei](#iTunesMetadata_contents).
3. Fügen Sie den Inhalt in die Datei ein, und bearbeiten Sie ihn wo nötig.
4. Speichern Sie die Datei, und kehren Sie zu Visual Studio für Mac zurück.
5. Rechtsklicken Sie im **Projektmappen-Explorer** auf die Xamarin.iOS-Projektdatei, und klicken Sie dann auf **Hinzufügen** > **Vorhandene Dateien...**.
6. Klicken Sie im Dialogfeld „Datei öffnen“ auf die `iTunesMetadata.plist`-Datei, die Sie oben erstellt haben, und dann auf **OK**.
7. Verändern Sie die Einstellung von **Buildaktion** nicht von **None**.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Das Xamarin-Plug-In für Visual Studio unterstützt nur einen visuellen Editor für `Info.plist`- und `Entitlement.plist`-Dateien. Sie müssen also Ihre `iTunesMetadata.plist`-Datei in einem Standard-Text-Editor erstellen und ihn manuell in Ihr Xamarin.iOS-Projekt einfügen.

Führen Sie folgende Schritte aus:

1. Erstellen Sie in einem Nur-Text-Editor eine neue Textdatei mit dem Namen `iTunesMetadata.plist`.
2. Kopieren Sie den Beispielinhalt aus dem oben stehenden Abschnitt [Inhalt der „iTunesMetadata.plist“-Datei](#iTunesMetadata_contents).
3. Fügen Sie den Inhalt in die Datei ein, und bearbeiten Sie ihn wo nötig.
4. Speichern Sie die Datei, und kehren Sie zu Visual Studio zurück.
5. Rechtsklicken Sie im **Projektmappen-Explorer** auf die Xamarin.iOS-Projektdatei, und klicken Sie dann auf **Hinzufügen** > **Vorhandene Dateien...**.
6. Klicken Sie im Dialogfeld „Datei öffnen“ auf die `iTunesMetadata.plist`-Datei, die Sie oben erstellt haben, und dann auf **Öffnen**.
7. Verändern Sie die Einstellung von **Buildaktion** nicht von **None**.

-----

Wählen Sie später diese `iTunesMetadata.plist`-Datei aus, wenn Sie Ihre IPA-Datei in der IDE erstellen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die `iTunesMetadata.plist`-Datei besprochen, die Sie verwenden können, um iTunes über eine iOS-Anwendung zu informieren, die per Ad-hoc-Verteilung bereitgestellt wurde. Zudem wurden der Standardschlüssel in der PLIST-Datei sowie das Erstellen und Verwalten der Datei in Visual Studio und Visual Studio für Mac erläutert.

## <a name="related-links"></a>Verwandte Links

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interne Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [IPA-Unterstützung](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
