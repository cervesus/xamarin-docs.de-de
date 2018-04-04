#<a name="---"></a>---
Title: "Teil 6 – testen und App Store Genehmigungen" ms.prod: Xamarin ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B ms.technology: Xamarin plattformübergreifende Autor: asb3993 ms.author: Amburns ms.date: 03/23/2017
---

# <a name="part-6---testing-and-app-store-approvals"></a>Teil 6: Testen und App-Store Genehmigungen


## <a name="testing"></a>Test

Viele apps (auch Android-apps für einige Niederlassungen) müssen Sie einen Genehmigungsprozess übergeben werden, bevor sie veröffentlicht werden; Daher testen unerlässlich, um sicherzustellen Ihrer app den Markt erreicht (ganz zu schweigen von erfolgreich ausgeführt wird, mit Ihren Kunden). Testen kann viele Formen annehmen, Developer-Ebene Komponententests für die Verwaltung über eine Vielzahl von Hardware Betatests dauern.


### <a name="test-on-all-platforms"></a>Testen auf allen Plattformen

Es gibt geringfügige Unterschiede zwischen der Art der Unterstützung durch .NET für Windows Phone, Tablet und Desktopgeräte sowie Einschränkungen für iOS, die verhindern, dass dynamische Code dynamisch generiert werden soll. Entweder planen, testen den Code auf mehreren Plattformen entwickeln, oder planen Zeit für die Umgestaltung und aktualisieren die Modellteil der Anwendung am Ende des Projekts.

Es ist immer empfiehlt sich, den Simulator-Emulator verwenden, um mehrere Versionen des Betriebssystems und auch andere Gerätekonfigurationen/Funktionen zu testen.

Sie sollten außerdem Testen auf so viele verschiedene physische Hardwaregeräten wie möglich.


#### <a name="devices-in-cloud"></a>Geräte in der cloud

Die mobile Smartphone- und Tablet-Ökosystem wächst ständig, machen es unmöglich, die ständig zunehmenden Anzahl der verfügbaren Geräte zu testen. Um dieses Problem zu lösen, das eine Reihe von Diensten die Möglichkeit bieten, vielen verschiedene Geräten Remote zu steuern, sodass Anwendungen installiert und ohne direkt in viele Hardware investieren müssen getestet werden können.

[Testen der App Mitte](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) bietet eine einfache Möglichkeit zum Testen von IOS- und Android-Apps auf Hunderten von verschiedenen Geräten.


### <a name="test-management"></a>Testverwaltung

Beim Testen von Anwendungen in Ihrer Organisation und Verwaltung von einem Betaprogramm mit externen Benutzern stehen zwei Herausforderungen:

-   **Verteilung** – Verwaltung im Rahmen des Bereitstellungsprozesses (insbesondere für iOS-Geräte) und Abrufen von aktualisierten Versionen der Software an die Tester.
-   **Feedback** – Erfassen von Informationen zur Anwendungsverwendung und ausführliche Informationen zu Fehlern, die auftreten können.


Es gibt eine Reihe von Diensten Hilfe ', um diese Probleme zu beheben, durch Bereitstellen der Infrastruktur, die in der Anwendung zu sammeln und Berichte zur Verwendung und Fehler integriert ist, und auch Optimierung im Rahmen des Bereitstellungsprozesses Hilfe registrieren und Verwalten von Testern und ihre Geräte .

[Visual Studio-App Center](/appcenter/) bietet eine Lösung für diese Probleme, die Version testverteilung, Absturzberichte und Informationen zur Anwendungsverwendung anspruchsvolle bereitstellen.



### <a name="test-automation"></a>Testautomatisierung

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) können verwendet werden, um die Benutzeroberfläche mit automatisierten Testskripts erstellen, die lokal ausgeführt werden oder in hochgeladen werden können [App Center Testen](https://docs.microsoft.com/appcenter/test-cloud/).




## <a name="unit-testing"></a>Unittests



#### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS umfasst eine Komponententestframework Touch.Unit an das JUnit/NUnit-Format, das Schreiben von Tests aufgerufen.

Finden Sie in unserem [Komponententests mit Xamarin.iOS](~/ios/deploy-test/touch.unit.md) Dokumentation ausführliche Tests schreiben und Ausführen von Touch.Unit.



#### <a name="andrunit"></a>Andr.Unit

Es ist ein Open Source-Äquivalent Touch.Unit für Android Andr.Unit aufgerufen. Sie können es von herunterladen [Github](https://github.com/spouliot/Andr.Unit) und erfahren Sie über das Tool auf [ @spouliotBlog](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).




## <a name="app-store-approvals"></a>App-Store Genehmigungen

Apple und Microsoft Betrieb den einzigen Speicher für ihre Plattformen: die App Store "und" Marketplace bzw.. Sowohl ihre Geräte gesperrt, und implementieren Sie einen strenge app Review-Prozess, um zu steuern, die Qualität der Anwendungen, die für den download verfügbaren. Öffnen des Android-Art bedeutet, dass es eine Reihe von Store-Optionen im Bereich von Google Play, weit verbreitet und keine Review-Prozess, der Amazon-App Store für Android und Hardware-spezifischen bemühungen wie Samsung Apps, die Verteilung auf stärker eingeschränkt haben Darüber hinaus einen Genehmigungsprozess implementiert.

Warten auf eine app, die geprüft werden kann sehr verkraftet - Business-Druck bedeuten, dass Anwendungen mit wenig Rand für Fehler, vor einem Datum "Ziel" Launch zur Genehmigung gesendet werden. Der Prozess selbst kann bis zu zwei Wochen dauern und ist nicht notwendigerweise transparent: beschränkt Feedback zum Status der Anwendung vorhanden ist, bis er schließlich abgelehnt oder genehmigt wird. Ablehnung kann bedeuten fehlende ein marketing Fenster Verkaufschancen, besonders, wenn es mehr als einmal auftritt und Wochen zwischen des ursprünglichen Start Datums übergeben, und wenn die app schließlich genehmigt wird.



### <a name="be-prepared"></a>Darauf vorbereitet sein

Der erste Teil der Empfehlung: Planen Sie Ihre app-Start auch im voraus, und stellen Sie Zertifikate für einen möglichen Ablehnung und erneute Übermittlung. Jedes Geschäft erfordert, dass Sie ein Konto erstellen, bevor Sie übermitteln der app – dazu so früh wie möglich.
Während der Anmeldung mit Google-Play nur ein paar Minuten dauert, wenn Ihre apps zur Verfügung stehen, ruft der Prozess viel komplizierter, wenn sie verkaufen und Bankgeschäfte angeben und Steuerinformationen müssen. Apple und im Microsoft Prozesse sind sowohl deutlich komplizierter als das Google, könnte es dauern, einer Woche oder mehr, um Ihr Konto genehmigt verlagern also diesmal in Ihre Pläne starten.

Sobald Ihr Konto genehmigt wurde, können Sie keine app übermitteln. Der tatsächliche Prozess zum Übermitteln von apps fällt in der folgenden Dokumentation:

-   [Veröffentlichen in Apple iOS App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
-   [Vorbereiten einer app auf Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
-  Windows-Entwickler sollten besuchen Sie die [Windows Dev Center](https://developer.microsoft.com/en-us/windows/windows-apps) senden ihre apps informieren.


Der übrige Teil dieses Abschnitts wird erläutert, Dinge, die Sie berücksichtigt werden sollten, um sicherzustellen, dass die app ohne Unterbrechungen genehmigt wird.



### <a name="quality"></a>Qualität

Klingt offensichtlich, aber Anwendungen werden häufig zurückgewiesen, weil sie nicht über ein gewisses Maß an Qualität erfüllen: Nachdem alle Dies ist der Grund, warum die umfassendes Speicher über einen Genehmigungsprozess ursprünglich!

Abstürze (crashes) sind eine häufige Ursache für die Ablehnung. Stellen Ihre app-Abstürze zu einfach ist, ist sichergestellt, dass abgelehnt werden. Die meisten Entwickler senden nicht ihre apps unter der Annahme, die sie werde stürzt ab, aber häufig der Fall. Testen Sie Ihre app gründlich, vor dem Senden des Updategrams, Schwerpunkt nicht nur sicherstellen, dass alles funktioniert, sondern auch, die Sie allgemeine mobile Fehlerszenarien wie Netzwerkprobleme und ressourceneinschränkungen z. B. Arbeitsspeicher oder Speicherplatz behandeln. Sowohl die physische Geräte der Simulator zum Testen verwenden – unabhängig davon, wie gut Code in einem Simulator ausgeführt wird, kann nur auf einem Gerät eine echte appleistung veranschaulichen. Verwenden von wie vielen verschiedenen Geräten, die beim Suchen und ein Team von Testern Beta eintragen, wenn Sie können - Dienste von Drittanbietern Beta-Verteilung und Feedback zu verwalten können.

Alle mobilen Betriebssystemen wird eine Anwendung beenden, die nicht schnell genug gestartet. Die Länge der zulässigen Zeit variiert, aber im Allgemeinen apps sollte darauf abzielen, werden in ein paar Sekunden reagiert und Verwenden von Hintergrundaufgaben, die keine Aktionen ausführt, die länger dauern würde. Apps, die zu lange Ladezeiten oder antwortet nicht genug reguläre verwendet werden, werden zurückgewiesen. Immer bereitstellen Sie Benutzerfeedback, wenn etwas im Hintergrund geschieht oder die app angezeigt wird, abgestürzt und noch einmal: Abrufen von abgelehnt.


### <a name="check-your-edge-cases"></a>Überprüfen Sie Ihre Grenzfälle

Eine allgemeine Trap für Entwickler Fehler bei der Adresse Grenzfälle, insbesondere solche, die erfordern, Neukonfigurieren ihre Simulator oder ein Gerät, um ordnungsgemäß zu testen. Es kann leicht vergessen, dass nicht jeder Kunde auf "Zulassen" wird die app auf ihren Speicherort zugreifen, da nach der Entwickler einmal akzeptiert hat, er nie erneut aufgefordert werden. Berechtigungen und die Auslastung der sind speziell auf während der Genehmigungsprozess Projekte Dies bedeutet, dass eine kleine Aufsicht in den folgenden Bereichen Ablehnung führen kann.

In der folgenden Liste ist ein guter Ausgangspunkt für die Überprüfung der Grenzfälle, die ggf. übersehen wurden:

-   **Kunden können möglicherweise den Zugriff auf Dienste "Ablehnen"** – insbesondere in iOS, den Zugriff auf Daten, z. B. GeoLocation-Informationen nur bereitgestellt werden, wenn der Benutzer die Berechtigung für die Anwendung erteilt. -Anwendungstester sollte häufig die Anwendung in ihren ursprünglichen Zustand erneut installieren und unterbinden, um sicherzustellen, dass die Anwendung ordnungsgemäß verhält sich alle Anforderungen für die Berechtigungen. Schalten Sie Berechtigung auf, und deaktivieren Sie, um das richtige Verhalten zu überprüfen, wie Kunden ihre Meinung ändern.
-   **Kunden werden überall** – nicht davon ausgehen, dass eine app nur verwendet werden wird, in dem Ort oder das Land, in dem entwickelt wurde. Code, der mit GPS-Koordinaten, Datums-und Uhrzeitwerte und Währungen arbeitet, kann alle nach Position und den gebietsschemaeinstellungen des Kunden betroffen. Alle Plattformen bieten einen Emulator, mit denen Sie unterschiedliche Speicherorte und Gebietsschemas - angeben kann, verwenden Sie diese Speicherorte in anderen Halbkugel und mit Kulturen, die unterschiedlich Formatieren von Datumsangaben und Währungen getestet. Breiten- und Längengrad Werte können positiv oder negativ sein, ein Punkt oder ein Komma als Dezimaltrennzeichen verwendet möglicherweise und können formatierte Datumsangaben zahllose Arten - Umgang mit!
-   **Langsame Netzwerkverbindungen** – app-Entwickler arbeiten häufig in einer "idealen Welt" schnelle, immer arbeiten, die nicht der Fall, in der realen Welt offensichtlich ist Netzwerkkonnektivität. Testen mit langsamen Netzwerkverbindung (z. B. eine schlechte 3 G-Verbindung) sowie kein Zugriff auf das Netzwerk ist entscheidend, um sicherzustellen, dass Sie eine fehlerhafte app liefern nicht. Der Genehmigungsprozess wird immer einen Test mit dem Gerät in flugzeugmodus einschließen, müssen Sie sicherstellen, dass Sie, die für sich selbst getestet haben.
-   **Hardware variiert** – Denken Sie daran, auf die älteste langsamste Hardware zu testen, die Sie unterstützen möchten. Es gibt zwei Aspekte, die Ihre app betreffen können: Leistung zu erzielen, die möglicherweise auf eine ältere Geräte und Unterstützung für Hardware-Features, z. B. einer Kamera, Mikrofon, GPS, Gyroskop oder andere optionale Komponente unbrauchbar. Anwendungen sollten beeinträchtigen ordnungsgemäß (und nicht eines Absturzes) Wenn eine Komponente ist nicht verfügbar.




### <a name="guidelines-are-more-than-just-a-guide"></a>Richtlinien sind mehr als nur eine "Anleitung"

Apple ist für die strenge zur Einhaltung ihrer menschlichen Richtlinien zur Benutzeroberfläche wird, wie eine der wichtigsten Vorteile von ihrer Plattform Konsistenz (und die wahrgenommene Erhöhung der benutzerfreundlichkeit) ist. Microsoft hat einen ähnlichen Ansatz mit Windows-Anwendungen implementieren die Benutzeroberfläche im Metro-Stil übernommen. Der Genehmigungsprozess für beide Plattformen wird Ihre app, die für die Einhaltung der relevanten Entwurfsphilosophie ausgewertet wird umfassen.

Die nicht zu sagen, dass Benutzer Schnittstelle Innovation wird nicht unterstützt oder empfohlen, jedoch sind bestimmte Punkte, die Sie "nur tun darf nicht" oder die app abgelehnt.

IOS schließt dies missbraucht integrierte Symbole oder mithilfe von anderen Bereich etablierten Metaphern nicht konsistent; verwenden z. B. das Symbol "erstellen" für etwas anderes als das Erstellen von neuen Inhalt.

Windows-Entwickler sollten ebenso vorsichtig sein; Ein häufiger Fehler ist nicht ordnungsgemäß Hardware wieder Schaltfläche gemäß Microsoft Richtlinien unterstützen.

Fordern Sie die Designern zu lesen und befolgen die Entwurfsrichtlinien für jede Plattform.



### <a name="implementing-platform-specific-features"></a>Implementieren von Clientplattform-spezifische Funktionen

Folgende Dinge sind etwas strengere, wenn es darum geht, Implementieren von plattformspezifischen-Diensten, insbesondere für iOS. Um automatische-Ablehnung von Apple zu vermeiden, gibt es einige Regeln mit den folgenden iOS-Funktionen zu befolgen:

-   **In App-Käufe** – Anwendungen müssen nicht implementieren externer Zahlung Mechanismen für die digitale Produkte, einschließlich Währung Spiels, Anwendungsfunktionen, Magazin Abonnements und vieles mehr. iOS-apps müssen von Apple iTunes-basierten Dienst für diese Art von Funktion verwenden. Es ist eine Lücke - apps, wie der Reader Kindle und einige Abonnement-basierte apps können Sie die Inhalte an anderer Stelle zu erwerben, die ruft an ein "Konto" klicken Sie dann über die app zugreifen können, jedoch in diesem Fall die app nicht enthalten darf, links oder Verweise auf die Out-des-app-Prozess kaufen (oder müssen noch einmal: abgelehnt werden).
-   **iCloud-Sicherung** – durch die Einführung der iCloud, Apple Bearbeiter stehen erheblich strikter wie apps Speicher (um sicherzustellen, dass Kunden remote backup Erfahrung angenehmeren) verwenden. Apps, die Sicherung kann Speicherplatz verschwendet zurückgewiesen abrufen kann, daher sollten Sie Cacheordner für die entsprechend, und führen Sie Apple des anderen speicherbezogene Richtlinien.
-   **Newsstand** – "Zeitung" und magazine apps sind hervorragend für Apple Newsstand, jedoch mindestens eine automatische Erneuerung Abonnement- und Support im Hintergrund um zu genehmigenden Herunterladen von apps implementiert werden müssen.
-   **Ordnet** – es ist immer häufiger, dass mobile Maps Overlays und andere Funktionen hinzugefügt haben, jedoch Achten Sie nicht die Zuordnung verdecken "Guthaben" Informationen (z. B. das Google-Logo in iOS5) wie auf diese Weise Ablehnung führt.




### <a name="manage-your-metadata"></a>Verwalten von Metadaten

Zusätzlich zu den offensichtlichen technischen Problemen, die in einer Anwendung, die zurückgewiesen auftreten können, sind einige subtilere Aspekte Ihrer Einsendung, die in der Ablehnung, vor allem um die Metadaten (Beschreibung, Schlüsselwörter und marketing Bilder), die Sie führen können Senden Sie mit der Anwendung für die Anzeige in der App Store oder Marketplace.

-   **Bilder** – Richtlinien für die Plattform für Anwendungssymbolen und Bilder zu speichern. Verwenden Sie keine markenrechtlich Bilder, kennen gelernt haben, dass apps zurückgewiesen abrufen, weil die Symbole Zeichnung iPhone Funktionsumfang!
-   **Marken** – Marken als Ihren eigenen verwenden. Apps haben erwähnen Marken in der app-Beschreibung oder sogar in die Schlüsselwörter Apple App Store verweigert wurde.
-   **Beschreibung** – nicht verwenden Sie das Wort "Beta" oder in irgendeiner Weise anzugeben, dass die app nicht für Primzahlen bereit. Erwähnen Sie nicht anderen mobilen Plattformen (selbst wenn Ihre app plattformübergreifend ist). Stellen Sie am wichtigsten ist, sicher, dass die app wird genau wie Sie sagen, dass dies der Fall ist. Wenn Sie eine Reihe von Funktionen in der Beschreibung aufzulisten, es wäre besser offensichtlich wie diese Funktionen verwenden oder Sie erhalten eine Ablehnung "Feature gemäß der Beschreibung der Anwendung ist nicht implementiert".


Fügen Sie so viel Aufwand in die Anwendung Metadaten wie in der Entwicklung und Tests. Anwendungen daher entsprechend Zeit, um sie richtig machen ist für kleinere Verstöße in den Metadaten abgelehnt.



### <a name="app-stores-not-for-everyone"></a>App-Stores: Nicht für "Jeder"

Consumer Verteilung - die Möglichkeit, wie viele Kunden möglichst erreichen wird schwerpunktmäßig der Speicher auf jeder Plattform. Gelten jedoch nicht alle Anwendungen zu Consumer, besteht eine schnell wachsende Basis des internen und extranet-ähnliche Anwendungen, die Mitarbeitern, Lieferanten oder Kunden eingeschränkten Verteilung erforderlich ist. Diese apps "für den Verkauf" nicht und Genehmigung, seit der Verteilung des Entwickler-Steuerelemente auf einer geschlossenen Benutzergruppe nicht erforderlich.
Unterstützung für diese Art der Bereitstellung variieren je nach Plattform.

Android bietet die größte Flexibilität in dieser Hinsicht: Anwendungen können direkt aus einer URL oder e-Mail-Anlage (solange das Gerät Konfiguration hinauszögern) installiert werden. Dies bedeutet, dass es sehr einfach zu erstellen und Verteilen von internen unternehmensanwendungen oder Anwendungen veröffentlichen, um bestimmte Kunden und Lieferanten.

Apple bietet eine interne Bereitstellungsoption für Entwickler, die registriert wird, in der iOS Developer Enterprise Program, umgeht die Store-App-Genehmigungsprozess und ermöglicht es Unternehmen, interne apps an Mitarbeiter zu verteilen.
Diese Lizenz befasst sich leider nicht auf die Notwendigkeit der extranet-ähnliche app-Verteilung für andere geschlossenen Gruppen von Kunden oder Lieferanten aus. [Enterprise (und Ad-hoc-) Bereitstellung](~/ios/deploy-test/app-distribution/ipa-support.md)



### <a name="app-store-summary"></a>App-Store-Zusammenfassung

Review-Prozess kann schwierig sein, aber wie der Rest des Entwicklungszyklus können Sie sicherstellen einige Planung und Details genau beachtet werden erfolgreich ausgeführt. Kommt es zu wenigen einfachen Schritten: gelesen und verstanden haben die Richtlinien zur Benutzeroberfläche müssen Sie befolgen, den Regeln entsprechen, wenn Sie Clientplattform-spezifische Funktionen implementieren, gründlich testen, (anschließend testen einen weiteren) und schließlich die Metadaten für die Anwendung stellen Sie sicher richtig ist, bevor Sie senden.

Eine letzte Wort Ratschläge für Entwickler, die auf Google Play veröffentlichen: Ihre Arbeit leichter - kann ist das Fehlen der Genehmigungsprozess mag, aber Ihre Kunden werden noch mehr anspruchsvollen als ein Team überprüfen. Befolgen Sie diese Richtlinien, wie wenn Ihre app abgelehnt abrufen konnte, andernfalls er Ihre Kunden, die auf diese Weise die ablehnen kann.

