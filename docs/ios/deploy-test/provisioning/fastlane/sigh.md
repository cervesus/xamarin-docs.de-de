---
title: fastlane für iOS – sigh
description: In diesem Dokument wird der sigh-Befehl von fastlane beschrieben, der für das Erstellen, Erneuern und Reparieren von Bereitstellungsprofilen für alle Xamarin.iOS-Buildkonfigurationen verwendet wird.
ms.prod: xamarin
ms.assetid: CD17276F-2C8C-4A46-A54C-DD532EBD5720
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: bb2fd504cccb7aa81831288762ba623824e52554
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526513"
---
# <a name="fastlane-for-ios-sigh"></a>fastlane für iOS – sigh

> [!IMPORTANT]
> fastlane empfiehlt die Verwendung von [`match`](~/ios/deploy-test/provisioning/fastlane/match.md) zum Generieren und Verwalten von Bereitstellungsprofilen. Verwenden Sie sigh nur direkt, wenn Sie vollständige Kontrolle haben möchten und über genug Informationen zum Signieren von Code verfügen.

## <a name="overview"></a>Übersicht

Die Gerätebereitstellung wird normalerweise von jedem Mitglied eines Entwicklungsteams über Xcode oder im Apple Developer Portal ausgeführt. Sie umfasst mehrere Schritte:

- Anfordern eines Entwicklungszertifikats
- Hinzufügen eines Geräts zum Portal
- Erstellen einer App-ID
- Erstellen eines Bereitstellungsprofils
- Herunterladen von Profilen und Zertifikaten

Jeder dieser Schritte enthält Variablen, die adressiert werden müssen, und die von der von Ihnen entwickelten Anwendungsart abhängig sind. Weitere Informationen über die erforderlichen Schritte, mit denen ein Gerät für die Entwicklung eingerichtet und entweder manuell oder über Xcode bereitgestellt werden kann, finden Sie in der Anleitung zur [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md).

In dieser Anleitung werden fastlane-Tools eingeführt, die alternativ zu Xcode verwendet werden können, und es wird Folgendes erläutert:

- [Was ist sigh?](#whatissigh)
- [Erstellen einer App-ID](#appid)
- [Hinzufügen neuer Geräte](#newdevices)
- [Verwenden von sigh](#using)
- [Zusätzliche Optionen](#options)

> [!NOTE]
> Wenn eine vorhandene App-ID mit der Bündel-ID Ihrer App übereinstimmt und Ihr Gerät im Entwicklerportal vorhanden ist, können Sie die Schritte [Erstellen einer App-ID](#appid) und [Hinzufügen eines Geräts](#newdevices) überspringen. Beginnen Sie in diesem Fall direkt mit [Verwenden von sigh](#using).

## <a name="installation"></a>Installation

Weitere Informationen zum Installieren von fastlane finden Sie in der Einführung des [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)-Leitfadens.

<a name="whatissigh" />

## <a name="what-is-sigh"></a>Was ist sigh?

Sigh bietet eine Terminalschnittstelle, mit der Sie Bereitstellungsprofile für alle Konfigurationen erstellen und erneuern können: Entwicklung, App Store-Verteilung, Ad-hoc-Verteilung und Enterprise-Verteilung. Zusätzlich wird ein einfacher Weg für das Herunterladen und Reparieren von Bereitstellungsprofilen bereitgestellt.

<a name="appid" />

## <a name="creating-an-app-id"></a>Erstellen einer App-ID

Eine App-ID kann mit folgendem Befehl erstellt werden:

```
fastlane produce -u your@appleid.com -a com.company.appname --skip_itc
```

Hierbei ist `com.company.appname` die Bündel-ID Ihrer App, die wie im Folgenden dargestellt in der Datei „Info.plist“ Ihrer Xamarin.iOS-Anwendung gefunden werden kann:

[![](sigh-images/fastlane-image5.png "Die Datei „Info.plist“ der Xamarin.iOS-Anwendung")](sigh-images/fastlane-image5.png#lightbox)

Die eindeutige App-ID muss eine Zeichenfolge im Reverse-DNS-Stil sein. Notieren Sie sich diese, sobald sie erstellt wurde, da sie später in diesem Artikel für die Verwendung von sigh benötigt wird.

Wenn die App in [iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) erstellt werden muss, entfernen Sie das `--skip_itc`-Flag aus dem oben stehenden Befehl:

<a name="newdevices" />

## <a name="adding-new-devices"></a>Hinzufügen neuer Geräte

Geben Sie folgenden Befehl ein, um ein einzelnes Gerät über die Befehlszeile zum Entwicklerportal hinzuzufügen:

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

Verwenden Sie den `register_devices`-Befehl, um mehr als ein Gerät hinzuzufügen:

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>Verwenden von sigh

Geben Sie folgenden Befehl in Ihr Terminal ein, um das sigh-Hilfsprogramm zu verwenden:

```bash
fastlane sigh
```

Dadurch wird standardmäßig ein Bereitstellungsprofil für die [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) erstellt. Übergeben Sie das `--development`-Flag, um Ihr Gerät für die Entwicklung einzurichten:

```bash
fastlane sigh --development
```

Geben Sie den Benutzernamen Ihrer Apple-ID ein, wenn fastlane Sie dazu auffordert. Wenn Sie fastlane zum ersten Mal verwenden, werden Sie möglicherweise auch nach dem Kennwort gefragt. Wenn nicht,wird die Umgebungsvariable für das Kennwort aus dem Schlüsselbund abgerufen.

Wenn Ihre Apple-ID mit mehreren Teams verbunden ist, werden diese hier angezeigt. Wählen Sie die Nummer aus, die dem Team entspricht, das Sie verwenden möchten:

[![](sigh-images/fastlane-image2.png "Das Team auswählen, das Sie verwenden möchten")](sigh-images/fastlane-image2.png#lightbox)

Die Team-ID kann auch folgendermaßen an die CLI übergeben werden:

```bash
fastlane sigh -l 2TU993NY9J
```

Geben Sie die [App-ID](#appid) Ihrer App ein. Beachten Sie, dass diese mit der Bündel-ID in der Datei „Info.plist“ Ihrer App übereinstimmen sollte.

Alle Geräte, die mit Ihrem Konto verbunden sind, werden zu Ihrem Bereitstellungsprofil hinzugefügt.

Fastlane erstellt das Bereitstellungsprofil für Sie, lädt es herunter und installiert es.

Wenn Sie das Developer Center durchsuchen, können Sie das neu erstellte Bereitstellungsprofil wie im Folgenden dargestellt anzeigen lassen:

[![](sigh-images/fastlane-image10.png "Sehen Sie sich das neu erstellte Bereitstellungsprofil an")](sigh-images/fastlane-image10.png#lightbox)

sigh speichert Bereitstellungsprofile standardmäßig im aktuellen Ordner. Bearbeiten Sie `output_path`, oder gehen Sie folgendermaßen vor, um das Ausgabeverzeichnis zu ändern:

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>Zusätzliche Optionen von sigh

Die folgenden Optionen können verwendet werden, um zusätzliche Unterstützung bei der Verwendung von sigh zu erhalten:

- Verwenden Sie folgenden Befehl, um alle Bereitstellungsprofile herunterzuladen:

    ```bash
    fastlane sigh download_all
    ```

- Verwenden Sie folgenden Befehl, um eine bestimmte Signierungsidentität für Ihr Bereitstellungsprofil zu verwenden:

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    Hierbei ist `Amy cert` der Name der Codesignierungsidentität.


## <a name="related-links"></a>Verwandte Links

- [fastlane – sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
