---
title: Arbeiten mit Berechtigungen
description: Berechtigungen sind besondere App-Funktionen und Sicherheitsberechtigungen, die Anwendungen gewährt werden, die für deren Verwendung konfiguriert sind.
ms.prod: xamarin
ms.assetid: 8A3961A2-02AB-4228-A41D-06CB4108D9D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: f158ab7e51eb7610566ed052b326fecf016add8a
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="working-with-entitlements"></a>Arbeiten mit Berechtigungen

_Berechtigungen sind besondere App-Funktionen und Sicherheitsberechtigungen, die Anwendungen gewährt werden, die für deren Verwendung konfiguriert sind._

In iOS werden Apps in einer _Sandbox_ ausgeführt, die einige Regeln bereitstellt, die den Zugriff zwischen Anwendungen und bestimmten Systemressourcen oder Benutzerdaten einschränken. _Berechtigungen_ werden verwendet, um eine Erweiterung der Sandbox vom System anzufordern, um Ihrer App zusätzliche Funktionen zu geben.

Um die Funktionen Ihrer App zu erweitern, muss eine Berechtigung in der entitlements.plist-Datei Ihrer App gewährt werden. Nur bestimmte Funktionen können erweitert werden. Sie sind im Leitfaden [Working with Capabilities (Arbeit mit Funktionen)](~/ios/deploy-test/provisioning/capabilities/index.md) aufgelistet und werden [unten](#keyreference) beschrieben. Berechtigungen werden als Schlüssel-Wert-Paare an das System übergeben. Für gewöhnlich ist ein Paar pro Funktion erforderlich. Die spezifischen Schlüssel und Werte werden im Abschnitt [Referenz für Berechtigungsschlüssel](#keyreference) weiter unten in diesem Artikel beschrieben.
Visual Studio für Mac und Visual Studio stellen über den Entitlements.plist-Editor eine unkomplizierte Benutzeroberfläche zum Hinzufügen von Berechtigungen zu einer Xamarin.iOS-App zur Verfügung.
In diesem Artikel wird der entitlements.plist-Editor vorgestellt und beschrieben, wie Sie diesen verwenden können. Zudem wird eine Referenz für alle Berechtigungen zur Verfügung gestellt, die einem iOS-Projekt für jede Funktion hinzugefügt werden können.

## <a name="entitlements-and-provisioning"></a>Berechtigungen und Bereitstellung


Die entitlements.plist-Datei wird verwendet, um Berechtigungen anzugeben und das Anwendungsbündel zu signieren.

Allerdings müssen einige zusätzliche Bereitstellungen getätigt werden, um sicherzustellen, dass die App ordnungsgemäß codesigniert wird. Das verwendete Bereitstellungsprofil muss eine App-ID enthalten, für die die erforderlichen Funktionen aktiviert sind. Informationen, wie Sie dies erreichen, finden Sie unter [Working with Capabilities (Arbeit mit Funktionen)](~/ios/deploy-test/provisioning/capabilities/index.md).

> [!IMPORTANT]
> Die Entitlements.plist-Datei hilft beim Ausfüllen der korrekten Eigenschaften für eine Anwendung mit Funktionen. Allerdings kann Sie kein Bereitstellungsprofil erstellen, da Sie nicht mit einem Apple-Entwicklerkonto verknüpft ist. Sie müssen trotzdem ein Bereitstellungsprofil im Entwicklerportal erstellen, um die Anwendung bereitzustellen und zu verteilen.

## <a name="set-entitlements-in-a-xamarinios-project"></a>Festlegen von Berechtigungen im Xamarin.iOS-Projekt

Beim Definieren Ihrer App-ID müssen Sie zusätzlich zur Auswahl und Konfiguration der erforderlichen Anwendungsdienste die Berechtigungen im Xamarin.iOS-Projekt konfigurieren, indem Sie sowohl die **info.plist**- als auch die **entitlements.plist**-Datei konfigurieren.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Um die Berechtigungen in Visual Studio für Mac zu konfigurieren, gehen Sie folgendermaßen vor:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Füllen Sie im Abschnitt **Ziel für iOS-Anwendung** den Namen für Ihre Anwendung aus, und geben Sie die **Bundle-ID** ein, die Sie beim Definieren der App-ID erstellt haben:

  ![](entitlements-images/servicexs01.png "Geben Sie eine Bündel-ID ein")

3. Speichern Sie die Änderungen an der **info.plist**-Datei.
4. Doppelklicken Sie im **Projektmappen-Explorer** auf die **entitlements.plist**-Datei, um sie zur Bearbeitung zu öffnen:

    ![](entitlements-images/servicexs02.png "Bearbeiten der Berechtigungen")

5. Die Berechtigungen für Ihre Xamarin.iOS-Anwendung müssen so ausgewählt und konfiguriert werden, dass sie mit den Einstellungen übereinstimmen, die Sie beim Erstellen der App-ID festgelegt haben.
6. Speichern Sie die Änderungen an der **entitlements.plist**-Datei.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Um die Berechtigung in Visual Studio zu konfigurieren, führen Sie Folgendes aus:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die **info.plist**-Datei, klicken Sie auf **Öffnen mit...** und auf die **PLIST**-Datei, um sie für die Bearbeitung zu öffnen.
2. Füllen Sie im Abschnitt **Ziel für iOS-Anwendung** den Namen für Ihre Anwendung aus, und geben Sie die **Bundle-ID** ein, die Sie beim Definieren der App-ID erstellt haben:

  ![](entitlements-images/servicevs01.png "Festlegen der Bündel-ID")

3. Speichern Sie die Änderungen an der **info.plist**-Datei.
4. Rechtsklicken Sie im **Projektmappen-Explorer** auf die **entitlements.plist**-Datei, klicken Sie auf **Öffnen mit...** und auf die **PLIST**-Datei, um sie für die Bearbeitung zu öffnen:

    ![](entitlements-images/servicevs02.png "Bearbeiten der Berechtigungen")

    Alternativ wird nach dem Doppelklicken auf die **entitlements.plist**-Datei der XML-Quell-Editor geöffnet, in dem Sie die Eigenschaft „Entitlement“ und den Schlüsselwert festlegen können. Dies wird unten im Abschnitt [Referenz zu Berechtigungen](#keyreference) genauer beschrieben.

5. Die Berechtigungen für Ihre Xamarin.iOS-Anwendung müssen so ausgewählt und konfiguriert werden, dass sie mit den Einstellungen übereinstimmen, die Sie beim Erstellen der App-ID festgelegt haben.
6. Speichern Sie die Änderungen an der **entitlements.plist**-Datei.


-----

<a name="add-new" />

## <a name="adding-a-new-entitlementsplist-file"></a>Hinzufügen einer neuen „entitlements.plist“-Datei

Berechtigungen werden einer App mit der „entitlements.plist“-Datei hinzugefügt. Diese Datei ist in Xamarin.iOS-Projekten standardmäßig enthalten, fehlt aber möglicherweise in älteren Projekten.

Um eine „entitlements.plist“-Datei in Ihrer Xamarin.iOS-Anwendung einzufügen, führen Sie Folgendes aus:

1.  Klicken Sie mit der rechten Maustaste auf die Projektdatei, und navigieren Sie dann zu **Hinzufügen > Neue Datei...**:

    ![Kontextmenü „Dateien hinzufügen“](entitlements-images/image1.png)
2.  Klicken Sie im Dialogfeld „Neue Datei“ auf **iOS > Eigenschaftenliste**, und benennen Sie sie „Berechtigungen“:

    ![Dialogfeld „Neue Datei“](entitlements-images/image2.png)

<a name="keyreference" />

## <a name="entitlement-key-reference"></a>Referenz zu Berechtigungsschlüsseln

Berechtigungsschlüssel können über den Bereich „Quelle“ im „entitlements.plist“-Editor hinzugefügt werden. Die erforderlichen Schlüssel werden normalerweise hinzugefügt, wenn Sie den entitlements.plist-Editor verwenden. Sie sind zur Referenz auch unten aufgelistet.

### <a name="wallet"></a>Wallet

*   **Beschreibung**: Früher „Passbook“. Wallet ist eine App in der Pässe und Tickets gespeichert und verwaltet werden. Diese Pässe können Kreditkarten, Kundenkarten, Bordkarten oder Tickets sein.

    - **Passtypbezeichner**
        * **Schlüssel**: com.apple.developer.pass-type-identifiers
        * **Zeichenfolge**: `$(TeamIdentifierPrefix)*`

* **Anmerkungen**:
    - So akzeptiert Ihre App alle Passtypen. Um Ihre App einzuschränken und nur eine Teilmenge von Teampasstypen zu erlauben, legen Sie den Zeichenfolgenwert auf `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)` fest.

    Dort wo „$(CFBundleIdentifier)“ die Pass-ID ist, die [oben](~/ios/platform/passkit.md) erstellt wurde

<a name="icloud" />

### <a name="icloud"></a>iCloud

*   **Beschreibung**: iCloud stellt iOS-Benutzern einen praktischen und einfachen Weg bereit, um ihre Inhalte zu speichern und diese zwischen Geräten freizugeben. Es gibt vier Möglichkeiten, wie Entwickler iCloud verwenden können, um ihren Benutzern Speicher bereitzustellen: Schlüssel-Wert-Speicher, UIDocument-Speicher, CoreData und das direkte Verwenden von CloudKit für das Bereitstellen von Speicher für einzelne Dateien und Verzeichnisse. Weitere Informationen dazu finden Sie im Handbuch „Introduction to iCloud (Einführung in iCloud)“.

    - **iCloud-Dokumente & CloudKit**
        - **Schlüssel**: com.apple.developer.ubiquity-container-identifiers
        - **Zeichenfolge**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
    - **iCloud-Schlüsselwertspeicher**
        - **Schlüssel**: com.apple.developer.ubiquity-kvstore-identifier
        - **Zeichenfolge**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

* **Anmerkungen**:
    - Die Zeichenfolge `$(TeamIdentifierPrefix)` können Sie finden, indem Sie sich bei developer.apple.com anmelden und zu **Member Center > Your Account > Developer Account Summary** (Mitgliedcenter > Ihr Konto > Zusammenfassung Developer-Konto) navigieren, um Ihre Team-ID abzurufen (oder Ihre eigene ID, wenn Sie ein einzelner Entwickler sind). Dabei handelt es sich um eine 10-stellige Zeichenfolge (z.B. A93A5CM278).
    - Die Zeichenfolge `$(CFBundleIdentifier)` beginnt mit `iCloud` und wird festgelegt, wenn der iCloud-Container mit den in [Arbeit mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) beschriebenen Schritten erstellt wird.
    - Die Platzhalter $`(TeamIdentifierPrefix)` und `$(CFBundleIdentifier)` können verwendet werden und werden bei der Erstellung durch die entsprechenden Werte ersetzt.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

### <a name="app-groups"></a>App-Gruppen

- **Beschreibung**: Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen.

    - **Schlüssel**: com.apple.security.application-groups
    - **Zeichenfolge**: group.$(CFBundleIdentifier)

<a name="apple-pay" />

### <a name="apple-pay"></a>Apple Pay

- **Beschreibung**: Apple Pay ermöglicht Benutzern das Bezahlen physischer Produkte über Ihr iOS-Gerät.
    - **Schlüssel**: com.apple.developer.in-app-payments
    - **Zeichenfolge**: merchant.your.mechantid

### <a name="push-notifications"></a>Pushbenachrichtigungen

- **Schlüssel**: aps-environment
- **Zeichenfolge**: `production` ODER `development`

### <a name="siri"></a>Siri

- **Beschreibung**: Durch SiriKit kann eine iOS-App Dienste bereitstellen, auf die die Apps „Siri“ und „Karten“ auf einem iOS-Gerät mit App-Erweiterungen und den neuen Intents- und Intents UI-Frameworks zugreifen können. Weitere Informationen dazu finden Sie im Handbuch „Introduction to iCloud (Einführung in SiriKit)“.
    - **Schlüssel**: com.apple.developer.siri

### <a name="personal-vpn"></a>Persönliches VPN

- **Schlüssel**: com.apple.developer.networking.vpn.api
- **Zeichenfolge**: allow-vpn

### <a name="keychain-sharing"></a>Keychain-Freigabe

- **Beschreibung**: Durch die Freigabe mit Keychain können App-Entwickler Kennwörter für andere vom selben Team entwickelten Apps freigeben, die auf dem Gerät in Keychain gespeichert sind. Der Zugriff kann eingeschränkt werden, indem Sie eine Gruppen-ID für den Zugriff auf Keychain in der Zeichenfolge übergeben.
    - **Schlüssel**: keychain-access-groups
    - **Zeichenfolge**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>Inter-App-Audio

- **Beschreibung**: Mit Inter-App-Audio können Entwickler Audio zwischen Apps streamen.
    - **Schlüssel**: inter-app-audio
    - **Boolescher Wert**: YES

### <a name="associated-domains"></a>Zugehörige Domänen

- **Beschreibung**: Zugehörige Domänen, die als universelle Links behandelt werden sollen, sollten mit dieser Berechtigung übergeben werden. Universelle Links können implementiert werden, um Deep Linking zwischen Ihrer App und Ihrer Website zu ermöglichen. Sie sollten einen Eintrag für jede Domäne bereitstellen, die Ihre App unterstützt. Dabei sollte jeder Eintrag mit `applinks:` beginnen.
    - **Schlüssel**: com.apple.developer.associated-domains
    - **Zeichenfolge**: webcredentials:example.com

### <a name="data-protection"></a>Schutz von Daten

- **Beschreibung**: Beim Aktivieren des Schutzes von Daten wird die integrierte Verschlüsselungshardware verwendet, um sensible Daten zu speichern, die in Ihrer App in verschlüsseltem Format verwendet werden. Die Standardschutzebene ist auf vollständigen Schutz festgelegt. Dabei kann nur auf Dateien zugegriffen werden, wenn das Gerät entsperrt ist.
    - **Schlüssel**: com.apple.developer.default-data-protection
    - **Zeichenfolge**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **Beschreibung**: Das HomeKit-Framework bietet eine Plattform zum Einrichten, Konfigurieren und Verwalten unterstützter Heimautomatisierungsgeräte – und das alles von einem iOS-Gerät aus. Weitere Informationen zu HomeKit finden Sie im Leitfaden „Introduction to HomeKit (Einführung in HomeKit)“.
    - **Schlüssel**: com.apple.developer.homekit
    - **Boolescher Wert**: YES

### <a name="healthkit"></a>HealthKit

- **Beschreibung**: HealthKit ist ein mit iOS 8 eingeführtes Framework, das einen zentralisierten, koordinierten und sicheren Datenspeicher für Informationen bietet, die mit der Integrität der App in Verbindung stehen. Weitere Informationen zu HomeKit finden Sie im Leitfaden „Introduction to HealthKit (Einführung in HealthKit)“.
    - **Schlüssel**: com.apple.developer.healthkit
    - **Boolescher Wert**: YES

### <a name="wireless-accessory-configuration"></a>Konfiguration für drahtloses Zubehör

- **Beschreibung**: Konfiguration für drahtloses Zubehör ermöglicht Ihrer Anwendung die Konfiguration von MFi-WLAN-Zubehör.
    - **Schlüssel**: com.apple.external-accessory.wireless-configuration
    - **Boolescher Wert**: YES

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden Berechtigungen und deren Verwendung in Visual Studio für Mac und Visual Studio behandelt. Zudem enthält er eine Referenz der Schlüssel-Wert-Paare für jede Funktion.