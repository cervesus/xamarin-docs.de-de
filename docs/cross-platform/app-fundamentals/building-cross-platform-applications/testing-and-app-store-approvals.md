---
title: 'Teil 6: Testen und App Store-Genehmigungen'
description: In diesem Dokument wird beschrieben, wie Sie eine plattformübergreifende Anwendung auf dem Gerät testen, Testfälle verwalten, Tests automatisieren, Komponententests ausführen und den Prozess der APP-Übermittlung durcharbeiten.
ms.prod: xamarin
ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: a9f84192a312f9aba98817b75c058229e6c721bb
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723620"
---
# <a name="part-6---testing-and-app-store-approvals"></a>Teil 6: Testen und App Store-Genehmigungen

## <a name="testing"></a>Test

Viele apps (sogar Android-Apps in einigen Filialen) müssen einen Genehmigungsprozess durchlaufen, bevor Sie veröffentlicht werden. Daher ist das Testen wichtig, um sicherzustellen, dass Ihre APP auf den Markt kommt (lassen Sie sich allein mit ihren Kunden erfolgreich). Das Testen kann viele Formen annehmen, von Komponententests auf Entwicklerebene bis hin zur Verwaltung von Beta-Tests auf einer Vielzahl von Hardware.

### <a name="test-on-all-platforms"></a>Testen auf allen Plattformen

Es gibt geringfügige Unterschiede zwischen der Unterstützung von .net auf Windows Phone-, Tablet-und Desktop Geräten sowie Einschränkungen für IOS, die verhindern, dass dynamischer Code dynamisch generiert wird. Planen Sie, den Code auf mehreren Plattformen zu testen, während Sie ihn entwickeln, oder planen Sie das Umgestalten und Aktualisieren des Modell Teils der Anwendung am Ende des Projekts.

Es ist immer empfehlenswert, den Simulator/Emulator zu verwenden, um mehrere Versionen des Betriebssystems und auch verschiedene Gerätefunktionen/-Konfigurationen zu testen.

Außerdem sollten Sie so viele verschiedene physische Hardware Geräte wie möglich testen.

#### <a name="devices-in-cloud"></a>Geräte in der Cloud

Das Mobiltelefon-und Tablet-Ökosystem wächst ständig, sodass es nicht mehr möglich ist, die ständig zunehmende Anzahl der verfügbaren Geräte zu testen. Um dieses Problem zu beheben, bieten eine Reihe von Diensten die Möglichkeit, viele verschiedene Geräte remote zu steuern, damit Anwendungen installiert und getestet werden können, ohne dass Sie direkt in viel Hardware investieren müssen.

App Center Test bietet eine einfache Möglichkeit zum Testen von IOS-und Android-Anwendungen auf hunderten unterschiedlichen Geräten. Weitere Informationen finden Sie unter [Vorbereiten von xamarin. Android-Apps](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) und [Vorbereiten von xamarin. IOS-apps](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

### <a name="test-management"></a>Testverwaltung

Wenn Sie Anwendungen in Ihrer Organisation testen oder ein Betaprogramm mit externen Benutzern verwalten, gibt es zwei Herausforderungen:

- **Verteilung** – Verwaltung des Bereitstellungs Prozesses (insbesondere für IOS-Geräte) und erhalten aktualisierter Versionen der Software an die Tester.
- **Feedback** – Sammeln von Informationen zur Anwendungs Verwendung und ausführliche Informationen zu eventuell auftretenden Fehlern.

Es gibt eine Reihe von Diensten, mit denen Sie diese Probleme beheben können, indem Sie eine Infrastruktur bereitstellen, die in Ihre Anwendung integriert ist, um die Nutzung und die Fehler zu erfassen und zu melden. Außerdem können Sie den Bereitstellungs Prozess optimieren, um Tester und deren Geräte zu registrieren und zu verwalten. .

[Visual Studio App Center](/appcenter/) bietet eine Lösung für diese Probleme, die die Verteilung von Testversionen, Absturzberichte und ausgereifte Anwendungs Verwendungs Informationen ermöglicht.

### <a name="test-automation"></a>Test Automatisierung

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) kann verwendet werden, um automatisierte Test Skripts für Benutzeroberflächen zu erstellen, die lokal ausgeführt oder in [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/)hochgeladen werden können.

## <a name="unit-testing"></a>Unittests

### <a name="touchunit"></a>Touch.Unit

Xamarin. IOS enthält ein Komponenten Test-Framework namens "Touchscreen. Unit", das dem JUnit/nunit-Stil zum Schreiben von Tests folgt.

Ausführliche Informationen zum Schreiben von Tests und zum Ausführen von "Touchscreen. Unit" finden Sie in unserer Dokumentation zu Komponenten [Tests mit xamarin. IOS](~/ios/deploy-test/touch.unit.md) .

### <a name="andrunit"></a>Andr. Unit

Es gibt eine Open Source-Entsprechung von "Touchscreen. Unit" für Android mit dem Namen "Andr. Unit". Sie können es von [GitHub](https://github.com/spouliot/Andr.Unit) herunterladen und sich über das Tool auf [@spouliotBlog](https://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/)informieren.

## <a name="app-store-approvals"></a>App Store-Genehmigungen

Apple und Microsoft betreiben den einzigen Speicher auf ihren Plattformen: den App Store bzw. den Marketplace. Sie können Ihre Geräte Sperren und einen strengen App-Überprüfungsprozess implementieren, um die Verfügbarkeit von Anwendungen zu steuern, die heruntergeladen werden können. Die offene Natur von Android bedeutet, dass es eine Reihe von Speicheroptionen gibt, die von Google Play reichen, das allgemein verfügbar ist und keinen Überprüfungsprozess hat, in den AppStore von Amazon für Android und Hardware spezifische Maßnahmen wie Samsung-apps, die eine beschränkte Verteilung aufweisen und implementieren Sie einen Genehmigungsprozess.

Wenn Sie darauf warten, dass eine APP überprüft werden kann, kann es sehr schwierig sein, dass Anwendungen mit sehr geringem Rand für einen Fehler vor dem Startdatum für die Genehmigung übermittelt werden. Der Prozess selbst kann bis zu zwei Wochen dauern und ist nicht notwendigerweise transparent: Es gibt nur ein eingeschränktes Feedback zum Fortschritt Ihrer Anwendung, bis Sie schließlich abgelehnt oder genehmigt werden. Eine Ablehnung kann bedeuten, dass ein Marketing Fenster der Verkaufschance fehlt, insbesondere, wenn es mehr als einmal geschieht, und Wochen zwischen dem ursprünglichen Startdatum und dem Zeitpunkt, zu dem die APP schließlich genehmigt ist.

### <a name="be-prepared"></a>Vorbereitet werden

Die erste Empfehlung: Planen Sie den Start der APP im voraus, und machen Sie eine mögliche Ablehnung und erneute Übermittlung von Berechtigungen. Jeder Speicher erfordert, dass Sie ein Konto erstellen, bevor Sie Ihre APP einreichen. führen Sie dies so früh wie möglich durch.
Die Registrierung von Google Play dauert zwar nur wenige Minuten, wenn Ihre apps kostenlos sind, aber der Prozess ist weitaus komplizierter, wenn Sie Sie verkaufen und Bank-und Steuerinformationen bereitstellen müssen. Die Prozesse von Apple und Microsoft sind wesentlich mehr beteiligt als Google. es kann eine Woche oder länger dauern, bis Ihr Konto genehmigt wird, sodass Sie diese Zeit in Ihre Einführungs Pläne einbeziehen.

Nachdem Sie Ihr Konto genehmigt haben, können Sie eine APP übermitteln. Der tatsächliche Prozess zum Übermitteln von apps wird in der folgenden Dokumentation behandelt:

- [Veröffentlichen im IOS App Store von Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Vorbereiten einer APP für Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
- Windows-Entwickler sollten das [Windows dev Center](https://developer.microsoft.com/windows/windows-apps) besuchen, um sich über das Einreichen Ihrer apps zu informieren.

Im restlichen Teil dieses Abschnitts werden Aspekte erläutert, die Sie berücksichtigen sollten, um sicherzustellen, dass Ihre APP ohne hiccups genehmigt wird.

### <a name="quality"></a>Qualität

Es klingt offensichtlich, aber Anwendungen werden häufig abgelehnt, da Sie nicht eine bestimmte Qualität erfüllen: Schließlich ist dies der Grund dafür, warum die zusammengestellten Geschäfte überhaupt einen Genehmigungsprozess haben.

Abstürze sind ein gängiger Grund für die Ablehnung. Wenn es zu einfach ist, Ihre APP abstürzen zu lassen, wird Sie garantiert abgelehnt. Die meisten Entwickler senden Ihre apps nicht mit der Annahme, dass Sie abstürzen werden, aber Sie haben dies häufig getan. Testen Sie Ihre APP gründlich vor der Übermittlung, und konzentrieren Sie sich nicht nur auf die Funktionsweise, sondern auch auf häufige Mobile Fehler Szenarien wie Netzwerkprobleme und Ressourceneinschränkungen wie Arbeitsspeicher oder Speicherplatz. Verwenden Sie den Simulator und physische Geräte zum Testen. unabhängig davon, wie gut Code in einem Simulator ausgeführt wird, kann nur ein Gerät die tatsächliche Leistung einer APP veranschaulichen. Verwenden Sie so viele verschiedene Geräte, wie Sie suchen können, und tragen Sie ein Team von Beta-Testern ein, wenn Sie-Drittanbieter Dienste bei der Verwaltung der Beta-Verteilung und des Feedbacks unterstützen können.

Alle mobilen Betriebssysteme beenden eine Anwendung, die nicht schnell genug gestartet wird. Die zulässige Dauer variiert, aber in der Regel sollten Anwendungen in wenigen Sekunden reaktionsfähig sein und Hintergrundaufgaben verwenden, um die Arbeit zu erledigen, die mehr Zeit in Anspruch nehmen würde. Apps, deren Auslastung zu lange dauert oder bei normaler Verwendung nicht ausreichend reagiert, werden abgelehnt. Geben Sie immer Benutzer Feedback an, wenn im Hintergrund etwas passiert, oder wenn die APP abgestürzt ist, und wiederholen Sie dann den Vorgang.

### <a name="check-your-edge-cases"></a>Überprüfen Sie Ihre randfälle

Ein gängiges Trap für Entwickler ist nicht in der Verwendung von Edge-Fällen nicht der Fall, insbesondere solche, die eine erneute Konfiguration Ihres Simulators oder Geräts zum ordnungsgemäßen testen erfordern. Es kann leicht vergessen werden, dass nicht jeder Kunde den Zugriff auf den Speicherort Ihrer APP ermöglicht, denn nachdem der Entwickler die Anforderung einmal akzeptiert hat, wird er nie erneut aufgefordert. Die Berechtigungen und die Netzwerk Verwendung werden im Rahmen des Genehmigungs Vorgangs speziell darauf ausgerichtet. Dies bedeutet, dass eine kleine Überwachung in diesen Bereichen zu einer Ablehnung führen kann.

Die folgende Liste ist ein guter Ausgangspunkt für das Überprüfen von Edge-Fällen, die möglicherweise ausgelassen wurden:

- **Kunden können den Zugriff auf Dienste verweigern** – insbesondere in Ios, wird der Zugriff auf Daten wie z. b. Informationen zur georeportung nur dann gewährt, wenn der Benutzer die Berechtigung für Ihre Anwendung erteilt. Anwendungs Tester sollten die Anwendung häufig in Ihrem ursprünglichen Zustand neu installieren und keine Berechtigungsanforderungen zulassen, um sicherzustellen, dass die Anwendung ordnungsgemäß verhält. Schalten Sie die Berechtigung ein und aus, um das richtige Verhalten zu überprüfen, wenn Kunden ihre Meinung ändern.
- **Kunden sind überall** – nehmen Sie nicht an, dass eine app nur in der Stadt oder dem Land verwendet wird, in der Sie entwickelt wurde. Code, der mit GPS-Koordinaten, Datums-und Uhrzeitwerten und Währungen arbeitet, kann vom Standort des Kunden und den Gebiets Schema Einstellungen beeinflusst werden. Alle Plattformen bieten einen Simulator, mit dem Sie verschiedene Standorte und Gebiets Schemas angeben können, um Standorte in anderen Hemisphären und Kulturen zu testen, in denen Datumsangaben und Währungen anders formatiert werden. Werte für breiten-und Längengrade können positiv oder negativ sein, das Dezimaltrennzeichen kann ein Punkt oder ein Komma sein, und Datumsangaben können auf unzählige Arten formatiert werden.
- **Langsame Netzwerkverbindungen** – App-Entwickler arbeiten häufig in einer "idealen Welt" der schnellen, immer funktionierenden Netzwerk Konnektivität, was natürlich in der Praxis nicht der Fall ist. Tests mit langsamen Netzwerkverbindungen (z. b. eine schlechte 3G-Verbindung) und ohne Netzwerk Zugriff sind wichtig, um sicherzustellen, dass Sie keine fehlerhafte App liefern. Der Genehmigungsprozess umfasst immer einen Test mit dem Gerät im Flugzeug Modus. Stellen Sie also sicher, dass Sie dies selbst getestet haben.
- **Hardware variiert** – denken Sie daran, auf die älteste, langsamste Hardware zu testen, die Sie unterstützen möchten. Es gibt zwei Aspekte, die sich auf Ihre APP auswirken können: die Leistung, die auf einem älteren Gerät unbrauchbar sein kann, und die Unterstützung von Hardware Features wie Kamera, Mikrofon, GPS, Gyroskop oder andere optionale Komponenten. Wenn eine Komponente nicht verfügbar ist, sollten sich Anwendungen problemlos (und nicht abstürzen) verschlechtern.

### <a name="guidelines-are-more-than-just-a-guide"></a>Richtlinien sind mehr als nur ein Leitfaden.

Apple ist so bekannt, dass es strikt an der Einhaltung der Richtlinien für die Benutzeroberfläche liegt, da eine der wichtigsten Stärken ihrer Plattform die Konsistenz (und die wahrgenommene Nutzbarkeit) ist. Microsoft hat bei Windows-Anwendungen, die das [fließende Entwurfs System](https://microsoft.com/design/fluent)implementieren, einen ähnlichen Ansatz gewählt. Der Genehmigungsprozess für beide Plattformen bedeutet, dass Ihre APP für die Einhaltung der relevanten Entwurfs Philosophie ausgewertet wird.

Das heißt nicht, dass Innovationen in der Benutzeroberfläche nicht unterstützt oder empfohlen werden, aber es gibt bestimmte Dinge, die Sie einfach nicht tun müssen, oder Ihre APP wird abgelehnt.

Unter IOS umfasst dies das dealisieren integrierter Symbole oder die Verwendung von anderen bewährten Metaphern auf nicht konsistente Weise. Verwenden Sie beispielsweise das Symbol "compose" für andere Elemente als das Erstellen neuer Inhalte.

Windows-Entwickler sollten auf ähnliche Weise vorsichtig vorgehen. ein häufiger Fehler ist die fehlerhafte Unterstützung der Hardware-zurück-Taste gemäß den Richtlinien von Microsoft.

Ermutigen Sie Ihre Designer, die Entwurfs Richtlinien für jede Plattform zu lesen und zu befolgen.

### <a name="implementing-platform-specific-features"></a>Implementieren von plattformspezifischen Features

Das ist ein wenig strenger, wenn es um die Implementierung von plattformspezifischen Diensten geht, insbesondere unter IOS. Um die automatische Ablehnung durch Apple zu vermeiden, gibt es einige Regeln, die mit den folgenden IOS-Features befolgt werden müssen:

- **In-App-Käufe** – Anwendungen dürfen keine externen Zahlungsmechanismen für digitale Produkte implementieren, einschließlich in-Game-Währungen, Anwendungs Features, Magazine-Abonnements und vieles mehr. IOS-apps müssen für diese Art von Funktionalität den iTunes-basierten Dienst von Apple verwenden. Es gibt eine Lücke, bei der apps wie der Kindle Reader und einige Abonnement basierte apps Inhalte an anderer Stelle erwerben können, die an ein "Konto" angehängt werden, auf das Sie dann über die App zugreifen können. in diesem Fall darf die APP jedoch keine Verknüpfungen oder Verweise auf das Prozess der Out-of-app-Anschaffung (oder auch hier abgelehnt).
- **icloud-Sicherung** – mit der Einführung von icloud sind die Reviewer von Apple weitaus strenger in Bezug darauf, wie apps Speicher verwenden (um sicherzustellen, dass die Remote Sicherung von Kunden angenehm ist). Apps, die Sicherungs fähigen Speicherplatz verschwenden, werden möglicherweise abgelehnt. verwenden Sie daher den Cache Ordner entsprechend, und befolgen Sie die anderen speicherbezogenen Richtlinien von Apple.
- **NewsStand** – Zeitungen und Magazine-apps eignen sich hervorragend für den News-Dienst von Apple, aber apps müssen mindestens ein Abonnement für die automatische Verlängerung implementieren und das Herunterladen von hintergrundräumen genehmigen.
- **Zuordnungen – es** kommt häufiger vor, dass Mobile Maps Überlagerungen und andere Features hinzugefügt werden. Achten Sie jedoch darauf, dass die Zuordnungs Informationen (z. b. das Google-Logo in iOS5) nicht verdeckt werden, da dies zu Ablehnung führt.

### <a name="manage-your-metadata"></a>Verwalten von Metadaten

Zusätzlich zu den offensichtlichen technischen Problemen, die dazu führen können, dass eine Anwendung abgelehnt wird, gibt es einige detailliertere Aspekte ihrer Übermittlung, die zu Ablehnung führen könnten, insbesondere um die Metadaten (Beschreibung, Schlüsselwörter und Marketing Bilder), die Sie übermitteln Sie Ihre Anwendung, um Sie im App Store oder Marketplace anzuzeigen.

- **Bilder** – befolgen Sie die Richtlinien der Plattform für Anwendungs Symbole und Store-Bilder. Verwenden Sie keine mit einem Bild markierten Images, wir haben gesehen, dass apps abgelehnt werden, da ihre Symbole eine Zeichnung eines iPhones hervorbringen!
- **Marken** – verwenden Sie keine eigenen Marken, die keine eigenen Marken sind. Apps wurden zum erwähnen von Marken in der APP-Beschreibung oder sogar in den Schlüsselwörtern im App Store von Apple verweigert.
- **Description** – verwenden Sie nicht das Wort "Beta", oder geben Sie in keiner Weise an, dass die APP für die primzeit nicht bereit ist. Nennen Sie keine anderen mobilen Plattformen (selbst wenn Ihre APP plattformübergreifend ist). Vor allem sollten Sie sicherstellen, dass die APP genau das tut, was Sie tun. Wenn Sie eine Reihe von Features in ihrer Beschreibung auflisten, ist es besser ersichtlich, wie Sie diese Features verwenden können, oder Sie erhalten eine "in der Beschreibung der Anwendung erwähnte Funktion ist nicht implementiert".

Setzen Sie die Metadaten der Anwendung in die Entwicklungs-und Testzwecke. Anwendungen werden bei geringfügigen Verstößen in den Metadaten abgelehnt, sodass Sie sich die Zeit nehmen, Sie richtig zu machen.

### <a name="app-stores-not-for-everyone"></a>App Stores: nicht für alle

Der Hauptschwerpunkt der Filialen auf den einzelnen Plattformen ist die Verteilung von Kunden-die Möglichkeit, so viele Kunden wie möglich zu erreichen. Allerdings sind nicht alle Anwendungen auf Consumer ausgerichtet, sondern es gibt eine schnell wachsende Basis von internen und Extranet-ähnlichen Anwendungen, die eine eingeschränkte Verteilung an Mitarbeiter, Lieferanten oder Kunden erfordern. Diese apps sind nicht "für den Verkauf" und benötigen keine Genehmigung, da der Entwickler die Verteilung an eine geschlossene Gruppe von Benutzern steuert.
Die Unterstützung für diese Art der Bereitstellung variiert je nach Plattform.

Android bietet die größte Flexibilität in dieser Hinsicht: Anwendungen können direkt von einer URL oder einem e-Mail-Anhang installiert werden (solange die Konfiguration des Geräts dies zulässt). Dies bedeutet, dass es trivial ist, interne Unternehmensanwendungen zu erstellen und zu verteilen oder Anwendungen für bestimmte Kunden oder Lieferanten zu veröffentlichen.

Apple bietet Entwicklern eine interne Bereitstellungs Option für Entwickler, die im IOS Developer Enterprise Program registriert sind, das den App Store-Genehmigungsprozess umgeht und es Unternehmen ermöglicht, interne apps an Ihre Mitarbeiter zu verteilen.
Leider erfüllt diese Lizenz nicht die Notwendigkeit einer Extranet-ähnlichen App-Verteilung an andere geschlossene Gruppen von Kunden oder Lieferanten. [Bereitstellung von Unternehmen (und Ad-hoc)](~/ios/deploy-test/app-distribution/ipa-support.md)

### <a name="app-store-summary"></a>App Store-Zusammenfassung

Der Überprüfungsprozess kann sich als beängstigend erweisen, aber wie der Rest des Entwicklungs Lebenszyklus können Sie den Erfolg mit einigen Planungs-und Augenmerk auf Details sicherstellen. Alles ist auf einige einfache Schritte zurückzuführen: Lesen und verstehen Sie die Richtlinien für die Benutzeroberfläche, die Sie einhalten müssen, befolgen Sie die Regeln, wenn Sie plattformspezifische Features implementieren, gründlich testen (Testen Sie das Testen) und schließlich sicherstellen, dass Ihre Anwendungs Metadaten ist richtig, bevor Sie übermitteln.

Ein letztes Wort für Entwickler, die auf Google Play veröffentlichen: der Mangel an Genehmigungsprozess erscheint möglicherweise so, als ob Sie Ihren Job vereinfachen, aber ihre Kunden werden noch anspruchsvoller als ein Überprüfungsteam. Beachten Sie diese Richtlinien, als ob Ihre APP abgelehnt werden könnte. andernfalls werden die Kunden, die ablehnen, abgelehnt.
