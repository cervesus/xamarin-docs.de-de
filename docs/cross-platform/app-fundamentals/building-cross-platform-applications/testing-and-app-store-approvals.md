---
title: 'Teil 6: Testen und App Store-Genehmigungen'
description: In diesem Dokument wird beschrieben, wie Sie eine plattformübergreifende Anwendung im Gerät zu testen, Verwalten von Testfällen, automatisieren Sie Tests, Ausführen von Komponententests und mehr über die app senden.
ms.prod: xamarin
ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0faf7c9e4ff7c96cdfd25ab6d6658726ef247b32
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275406"
---
# <a name="part-6---testing-and-app-store-approvals"></a>Teil 6: Testen und App Store-Genehmigungen

## <a name="testing"></a>Test

Viele Anwendungen (auch Android-apps in einigen Stores) müssen Sie einen Genehmigungsprozess zu übergeben, bevor sie veröffentlicht werden; Daher testen, um sicherzustellen, dass kritische ist Ihrer app erreicht den Markt (ganz zu schweigen von erfolgreich ausgeführt wird, mit Ihren Kunden). Tests kann viele Formen, vom Entwickler auf Komponententests, die für die Verwaltung von beta-Tests auf einer Vielzahl verschiedener Hardware annehmen.

### <a name="test-on-all-platforms"></a>Testen auf allen Plattformen

Es gibt jedoch geringfügige Unterschiede zwischen was .NET unterstützt, auf Windows Phone, Tablet und desktop-Geräte, sowie Einschränkungen für iOS, die verhindern, dass dynamische Code dynamisch generiert werden soll. Entweder auf den Code auf mehreren Plattformen testen, entwickeln, oder planen Zeit für das Umgestalten und aktualisieren den Modellteil Ihrer Anwendung am Ende des Projekts planen.

Es ist immer empfiehlt sich, den Simulator-Emulator verwenden, um mehrere Versionen des Betriebssystems und auch verschiedene Gerätekonfigurationen/Funktionen zu testen.

Sie sollten auch auf so viele unterschiedliche physische Hardwaregeräte können Sie testen.

#### <a name="devices-in-cloud"></a>Geräte in der cloud

Das mobile Ökosystem für Smartphone und Tablet wächst immer wieder kann zum Testen der stetig wachsende Anzahl von Geräten, die zur Verfügung. Zur Lösung dieses Problems eine Reihe von Diensten die Möglichkeit bieten, die vielen verschiedene Geräten Remote zu steuern, sodass Anwendungen installiert und ohne dass Sie direkt in viele Hardware investieren müssen getestet werden können.

[App Center Test](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) bietet ein einfaches Verfahren, um IOS- und Android-Anwendungen auf Hunderten von verschiedenen Geräten zu testen.

### <a name="test-management"></a>Testverwaltung

Beim Testen von Anwendungen innerhalb Ihrer Organisation oder ein Beta-Programm für externe Benutzer verwalten, gibt es zwei Herausforderungen:

- **Verteilung** – Verwalten des Bereitstellungsprozesses (insbesondere bei iOS-Geräte) und aktualisierte Versionen der Software an die Tester.
- **Feedback** – Sammeln von Informationen zu Verwendung und ausführliche Informationen zu Fehlern, die auftreten können.


Es gibt eine Reihe von Services – Hilfe zum Beheben dieser Probleme durch Bereitstellen der Infrastruktur, die in Ihrer Anwendung zu sammeln und Berichte auf Nutzung und Fehlern basiert, und auch Optimierung im Rahmen des Bereitstellungsprozesses, um zu registrieren und zu verwalten, Tester und deren Geräte .

[Visual Studio App Center](/appcenter/) bietet eine Lösung für diese Probleme, die Bereitstellung von Test-Version Verteilung, Absturzberichte und Nutzungsinformationen von komplexen Anwendungen.

### <a name="test-automation"></a>Automatisierung von Tests

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) können verwendet werden, um die Benutzeroberfläche mit automatisierten Testskripts erstellen, die lokal oder in hochgeladen werden können [App Center Testen](https://docs.microsoft.com/appcenter/test-cloud/).

## <a name="unit-testing"></a>Unittests

### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS enthält ein Komponententestframework Touch.Unit an den JUnit/NUnit-Stil, der zum Schreiben von Tests aufgerufen.

Finden Sie in unserem [Komponententests mit Xamarin.iOS](~/ios/deploy-test/touch.unit.md) Dokumentation zum Schreiben von Tests und Touch.Unit ausgeführt.

### <a name="andrunit"></a>Andr.Unit

Es ist eine Open-Source-Entsprechung des Touch.Unit für Android Andr.Unit aufgerufen. Sie können es von [Github](https://github.com/spouliot/Andr.Unit) und erfahren Sie, das Tool auf [ @spouliotBlog](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).

## <a name="app-store-approvals"></a>App-Store-Genehmigungen

Betrieb von Apple und Microsoft den einzigen Speicher auf ihren Plattformen: die App Store und den Marketplace bzw. Beide Sperren Sie ihre Geräte und Implementieren einer strengen app-Prüfung, um zu steuern, die Qualität der Anwendungen, die zum download zur Verfügung. Öffnen von Android-Art bedeutet, dass es eine Reihe von Store-Optionen, die im Bereich von Google Play, die allgemein verfügbar und verfügt über keine Reviewprozess, auf der Amazon Appstore für Android und hardwarespezifischen bemühungen wie Samsung-Apps, die weitere Verteilung eingeschränkter und implementieren Sie einen Genehmigungsprozess.

Warten auf eine app, die überprüft werden kann sehr anstrengend sein - Business-Druck, häufig bedeutet, dass Anwendungen mit sehr geringem Rand für Fehler, die vor einem Datum "Ziel" Launch zur Genehmigung übermittelt werden. Der Prozess selbst kann bis zu zwei Wochen dauern, und nicht unbedingt transparent: beschränkt Feedback auf den Status der Anwendung vorhanden ist, bis er schließlich abgelehnt oder genehmigt wird. Ablehnung kann bedeuten eine marketing einzigartige Gelegenheit, fehlen, insbesondere dann, wenn es mehr als einmal passiert, und übergeben Sie Wochen, zwischen Ihrer ursprünglichen Start Date, und wenn die app schließlich genehmigt wurde.

### <a name="be-prepared"></a>Seien Sie darauf vorbereitet

Der erste Teil Rat: Planen Ihrer app-Start auch im voraus und Beschränkungen für eine mögliche Ablehnung und eine erneute Übermittlung. Jedes Geschäft müssen Sie zum Erstellen eines Kontos vor dem übermitteln der app – dazu so früh wie möglich.
Während der Anmeldung bei Google Play nur ein paar Minuten dauert, wenn Ihre apps kostenlos sind, ruft den Prozess sehr viel komplizierter ab, wenn Sie sie verkaufen und geben Sie eine Banking und Steuerinformationen müssen. Apple und Microsoft Prozesse sind beide viel komplizierter als das von Google, kann es dauern, eine Woche oder mehr auf Ihr Konto genehmigt dieses Mal also in Ihre Planung starten berücksichtigen.

Nachdem Ihr Konto genehmigt wurde, können Sie eine app zu senden. Der tatsächliche Prozess zum Übermitteln von apps wird die folgende Dokumentation behandelt:

- [Apple iOS App Store veröffentlichen](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Vorbereiten einer app für Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
- Windows-Entwickler sollten finden Sie auf die [Windows Dev Center](https://developer.microsoft.com/windows/windows-apps) , erfahren Sie mehr über ihre apps übermitteln.

Der übrige Teil dieses Abschnitts wird erläutert, Dinge, die Sie berücksichtigt werden sollten, um sicherzustellen, dass Ihre app ohne Unterbrechungen genehmigt wird.

### <a name="quality"></a>Qualität

Hört sich offensichtlich, aber Anwendungen werden häufig abgelehnt, da sie nicht über ein gewisses Maß an Qualität treffen: Dies ist der Grund, warum die geordneten einen Genehmigungsprozess im vornherein haben!

Abstürze sind ein häufiger Grund für Ablehnung. Wenn sie auch ganz einfach Ihre app abgestürzt ist, wird er sind garantiert abgelehnt werden. Die meisten Entwickler senden nicht ihre apps unter der Annahme, die sie abstürzen werden, aber häufig der Fall. Testen Sie Ihre app gründlich, vor der Übermittlung und konzentriert Sie sich, nicht nur auf und stellen Sie sicher, dass alles funktioniert, sondern auch, dass mobile-Fehler wie z. B. Netzwerkprobleme und ressourcenbeschränkungen wie Arbeitsspeicher oder ein Speicherplatz Standardszenarien. Verwenden Sie den Simulator und physische Geräte um zu testen – unabhängig davon, wie gut Code in einem Simulator ausgeführt wird, kann nur ein Gerät tatsächliche Leistung der app darstellen. Verwenden Sie so viele verschiedene Geräte können Sie suchen, und ein Team von Beta-Tester eintragen, wenn Sie - Diensten von Drittanbietern die Beta-Verteilung und Feedback zu verwalten können.

Alle mobilen Betriebssystemen wird eine Anwendung beenden, die nicht schnell genug beginnt. Die Länge der zulässigen Zeit variiert, aber im Allgemeinen sollten apps versuchen, seine Reaktionsfähigkeit in wenigen Sekunden, und verwenden Aufgaben im Hintergrund Schritte erforderlich, die länger dauern würde. Apps, die beim Laden zu lange dauert oder reagiert nicht genug regulären verwendet werden, werden abgelehnt. Geben Sie immer Feedback der Benutzer auf, wenn etwas, im Hintergrund passiert ist oder die app angezeigt wird, ist ein Prozess abgestürzt und wieder abrufen abgelehnt.

### <a name="check-your-edge-cases"></a>Überprüfen Sie Ihre Grenzfälle

Eine allgemeine Trap für Entwickler, tritt ein Fehler auf Edge-Fälle, insbesondere über diejenigen, die erfordern, erneut zu konfigurieren, ihre Simulator oder ein Gerät, um ordnungsgemäß zu testen. Es kann sein leicht vergessen, dass nicht jeder Kunde auf "Zulassen" wird Ihre app auf ihrem Speicherort zugreifen, da nachdem der Entwickler die Anforderung nach akzeptiert, sie nicht erneut aufgefordert werden. Berechtigungen und Netzwerkauslastung sind speziell auf während des Genehmigungsprozesses, lag Dies bedeutet, dass die Ablehnung einer kleinen Aufsicht in diesen Bereichen führen kann.

In der folgende Liste ist ein guter Ausgangspunkt für die Überprüfung der Grenzfälle, die möglicherweise ausgelassene:

- **Kunden können den Zugriff auf Dienste "Ablehnen"** – insbesondere in iOS, den Zugriff auf Daten, z. B. GeoLocation-Informationen nur bereitgestellt wird, wenn der Benutzer die Berechtigung für Ihre Anwendung erteilt. -Anwendungstester sollte häufig die Anwendung in seinen ursprünglichen Zustand erneut installieren und verweigert alle berechtigungsanforderungen, um sicherzustellen, dass die Anwendung richtig verhält. Aktivieren Sie die Berechtigung ein, und deaktivieren Sie, um das richtige Verhalten zu überprüfen, wie Kunden ihre Meinung ändern.
- **Kunden sind überall verfügbar** – nicht davon ausgehen, dass eine app nur verwendet werden wird, in der Stadt oder Land, in denen es entwickelt wurde. Code, der mit GPS-Koordinaten, Datums-und Uhrzeitwerte und Währungen arbeitet kann alle von Speicherort und den gebietsschemaeinstellungen des Kunden betroffen. Alle Plattformen bieten einen Simulator, mit denen Sie die verschiedenen Speicherorten und Gebietsschemas - angeben, die damit teststandorte, andere hemisphären und Kulturen, die Datumsangaben und Währungen unterschiedlich zu formatieren können. Breitengrad-und längengradwerte können positiv oder negativ sein, ein Punkt oder ein Komma als Dezimaltrennzeichen verwendet möglicherweise und können formatierte Datumsangaben zahllose Möglichkeiten – Umgang mit!
- **Langsame Netzwerkverbindungen** – app-Entwickler häufig zu arbeiten, in einer "idealen Welt" für die schnelle, immer mit der Arbeit Netzwerkverbindung getrennt wird, die offensichtlich nicht der Fall, in der Praxis ist. Testen mit langsamen Netzwerkverbindungen (z. B. einer schlechten Verbindung mit 3 G) sowie kein Netzwerkzugriff ist entscheidend, um sicherzustellen, dass Sie eine fehlerhafte app nicht zusammen. Der Genehmigungsprozess immer einen Test mit dem Gerät in den flugzeugmodus umfasst, müssen Sie sicherstellen, dass Sie selbst, die getestet haben.
- **Hardware variiert** : Denken Sie daran, die auf der ältesten, langsamste Hardware zu testen, die Sie unterstützen möchten. Es gibt zwei Aspekte, die Ihre app auswirken können: Leistung, die möglicherweise auf einem älteren Gerät sowie Unterstützung für Hardwarefeatures wie z. B. eine Kamera, Mikrofon, GPS, Gyroskop oder anderen optionalen Komponenten unbrauchbar. Anwendungen sollten beeinträchtigt ordnungsgemäß (und nicht Absturz) Wenn eine Komponente ist nicht verfügbar.

### <a name="guidelines-are-more-than-just-a-guide"></a>Richtlinien sind mehr als nur eine "Anleitung"

Apple ist berühmt für wird zur Einhaltung ihrer Human Interface Guidelines strict, da eine der Stärken von ihrer Plattform Konsistenz (und die wahrgenommene Erhöhung der benutzerfreundlichkeit) ist. Microsoft verfügt über einen ähnlichen Ansatz mit Windows-Anwendungen, die Implementierung der Benutzeroberflächenautomatisierungs im Metro-Stil verwendet. Der Genehmigungsprozess für beide Plattformen muss Ihre app für die Einhaltung der relevanten Designphilosophie ausgewertet wird.

Die nicht zu sagen, dass Innovationen für Benutzer-Schnittstelle nicht unterstützt oder empfohlen, aber es bestimmte Dinge, die Sie gibt "nur tun sollte nicht" oder Ihre app abgelehnt.

Unter iOS schließt dies integrierte Symbole verwendet oder mithilfe von anderen gut etablierten Metaphern nicht konsistent; verwenden das Symbol "compose" z. B. für etwas anderes als das Erstellen neuer Inhalte.

Windows-Entwickler sollten ebenso vorsichtig sein; Ein häufiger Fehler ist nicht ordnungsgemäß die Hardware wieder Schaltfläche gemäß Microsoft Richtlinien unterstützen.

Empfehlen Sie Ihren Entwicklern zu lesen und befolgen die Entwurfsrichtlinien für jede Plattform.

### <a name="implementing-platform-specific-features"></a>Implementieren von plattformspezifischen Features

Dinge sind ein wenig strengere, wenn es darum geht, implementieren plattformspezifische Dienste, insbesondere unter iOS. Um automatische-Ablehnung von Apple zu vermeiden, gibt es einige Regeln mit den folgenden iOS-Features zu befolgen:

- **In-App-Käufe** – Anwendungen müssen externe Zahlung-Mechanismen zum digitalen Produkten wie z.B. in-Game-Währung Anwendungsfeatures, magazine-Abonnements und viel mehr nicht implementiert. iOS-apps müssen von Apple iTunes-basierten Dienst für diese Art von Funktionalität verwenden. Es gibt eine Sicherheitslücke - apps, wie der Reader Kindle und einige Abonnement-basierte apps können Sie die Inhalte an anderer Stelle zu erwerben, die zugeordnet, ein "Konto" die Sie dann über die app zugreifen können, jedoch in diesem Fall die app nicht enthalten darf, links oder Verweise auf die Out-of-app-Prozess kaufen (oder wieder werden abgelehnt werden).
- **iCloud-Sicherung** – durch die Einführung von iCloud, Apple Prüfer sind wesentlich strenger in Bezug auf die apps wie Speicher verwenden (um sicherzustellen, dass der remote-Sicherung kundenerfahrung angenehme). Apps, die Sicherung kann Speicherplatz verschwendet abgelehnt abrufen kann, also verwenden den Cacheordner entsprechend ein, und führen Sie die Apple der anderen speicherbezogenen Richtlinien.
- **Zeitungskiosk** – Zeitungs- und magazine apps eignen sich hervorragend für den Apple Zeitungskiosk, jedoch mindestens eine automatische Erneuerung das Abonnement und Hintergrund um zu genehmigenden Herunterladen von apps implementiert werden müssen.
- **Ordnet** – es ist immer häufiger, dass das Hinzufügen von Überlagerungen und andere Funktionen zu mobilen Zuordnungen können jedoch eine sorgfältige nicht verdecken die Zuordnung "-Gutschrift in Höhe" Informationen (z. B. das Google-Logo in iOS5) wie auf diese Weise Ablehnung geführt haben.

### <a name="manage-your-metadata"></a>Verwalten von Metadaten

Zusätzlich zu den offensichtlichen technischen Problemen, die in einer Anwendung, die zurückgewiesen führen können, sind einige Subtiler Aspekte Ihrer Übermittlung an, die in der Ablehnung, insbesondere um die Metadaten (Beschreibung, Schlüsselwörter und marketing-Images), die Sie führen können übermitteln Sie mit Ihrer Anwendung für die Anzeige in der App Store oder den Marketplace.

- **Bilder** – befolgen Sie die Richtlinien von der Plattform für Symbole und Bilder zu speichern. Verwenden Sie keine markenrechtlich geschützten Bilder, wir haben gesehen, dass apps mein Beitrag abgelehnt, da die Symbole mit eine Zeichnung eines iPhones wichtige!
- **Marken** – verwenden Sie Marken als Ihren eigenen. Apps wurde verweigert, Marken, die in die app-Beschreibung oder sogar in den Schlüsselwörtern auf Apple App Store erwähnen.
- **Beschreibung** – verwenden Sie das Wort "Beta" oder in keiner Weise anzugeben, dass die app nicht bereit für eine. Erwähnen Sie nicht andere mobilen Plattformen (auch wenn Ihre app plattformübergreifend ist). Stellen Sie wichtiger ist, sicher, dass die app leistet genau was Sie sagen, dass dies der Fall ist. Wenn Sie eine Reihe von Funktionen in der Beschreibung auflisten, es wäre besser offensichtlich wie für jede dieser Funktionen verwendet werden, oder Sie erhalten eine Ablehnung "die Funktion, die in der Anwendung Beschreibung erwähnt ist nicht implementiert".

Fügen Sie so viel Aufwand in der Anwendung Metadaten wie in der Entwicklung und Tests. Anwendungen für kleinere Verstöße in den Metadaten abgelehnt zu erhalten, daher ist es sinnvoll, befassen Sie sich für eine Korrektur.

### <a name="app-stores-not-for-everyone"></a>App-Stores: Nicht für alle Benutzer

Der primäre Fokus von der Speicher auf jeder Plattform ist die Consumer-Distribution - die Möglichkeit, wie viele Kunden wie möglich zu erreichen. Gelten jedoch nicht alle Anwendungen im Consumer ist ein schnell wachsender Basis intern und extranet-ähnliche Anwendungen, die eingeschränkten Verteilung Mitarbeiter, Lieferanten oder Kunden zu erfordern. Diese apps sind nicht "für den Verkauf" und nicht genehmigt haben, da die Verteilung des Entwickler-Steuerelemente in einer geschlossenen Benutzergruppe erforderlich.
Unterstützung für diese Art der Bereitstellung variieren je nach Plattform.

Android bietet die größte Flexibilität in dieser Hinsicht: Anwendungen können direkt aus einer URL oder e-Mail-Anlage (sofern die Gerätekonfiguration in der es erlaubt) installiert werden. Dies bedeutet, dass es sehr einfach zu erstellen und interne unternehmensanwendungen zu verteilen oder Veröffentlichen von Anwendungen für bestimmte Kunden oder Lieferanten.

Apple bietet eine Option für die interne Bereitstellung für Entwickler, die in der iOS Developer Enterprise Program, die den App Store-Genehmigungsprozess umgeht und ermöglicht es Unternehmen, interne apps an ihre Mitarbeiter verteilen registriert.
Diese Lizenz befasst sich leider nicht auf die Notwendigkeit der extranet-ähnliche app-Verteilung auf andere geschlossenen Gruppen von Kunden oder Lieferanten aus. [Enterprise (und Ad-hoc-) Bereitstellung](~/ios/deploy-test/app-distribution/ipa-support.md)

### <a name="app-store-summary"></a>App-Store-Zusammenfassung

Der Überprüfungsprozess kann sinnvoll sein, allerdings wie der Rest des Entwicklungslebenszyklus können Sie sicherstellen Erfolg mit etwas planungs- und Aufmerksamkeit für Details. Kommt es, einige einfache Schritte: Lesen und Verstehen der Richtlinien zur Benutzeroberfläche müssen Sie einhalten, den Regeln entsprechen, wenn Sie plattformspezifischen Features implementieren, sehr gründlich zu testen (dann testen noch mehr) und schließlich Ihre Anwendungsmetadaten stellen Sie sicher ist richtig, vor dem übermitteln.

Eine Zusammenfassung der Ratschläge, die Entwickler, die auf Google Play veröffentlichen: Ihres Auftrags - vereinfacht, ist der Mangel an den Prozess mag Ihren Kunden werden jedoch als ein reviewteam sogar noch mehr anfordern. Beachten Sie Folgendes ein, wie wenn Ihre app abgelehnt abrufen konnte, andernfalls Ihre Kunden die Ablehnung.
