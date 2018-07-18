---
title: Einführung in den Lebenszyklus der mobilen Softwareentwicklung
description: In diesem Dokument werden unter anderem der Lebenszyklus der mobilen Softwareentwicklung, das UX-Design, das UI-Design, die Entwicklung, die Stabilisierung und die Verteilung beschrieben.
ms.prod: xamarin
ms.assetid: 420c5fdf-4610-4e71-9db5-fe894c961924
author: asb3993
ms.author: amburns
ms.date: 11/22/2016
ms.openlocfilehash: 8a95f89ad41ab793d8c26631f1a967180b4c1779
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782332"
---
# <a name="introduction-to-the-mobile-software-development-lifecycle"></a>Einführung in den Lebenszyklus der mobilen Softwareentwicklung

Das Erstellen mobiler Anwendungen kann so einfach sein: Öffnen Sie die IDE, schreiben Sie etwas, führen Sie einen kurzen Test durch, und senden Sie das Ergebnis dann an einen App Store. All dies ist an einem Nachmittag erledigt. Es kann jedoch auch ein sehr komplexer Vorgang sein, der strenge Vorabentwürfe, Nutzbarkeitstests, QA-Tests auf tausenden Geräten, einen vollständigen Beta-Lebenszyklus und anschließend die Bereitstellung einer Reihe verschiedener Methoden enthält.

Dieses Dokument enthält eine gründliche, einführende Erläuterung für das Erstellen von mobilen Anwendungen, einschließlich Folgendem:

1.   **Prozess**: Der Prozess der Softwareentwicklung wird als Lebenszyklus der Softwareentwicklung (Software Development Lifecycle, SDLC) bezeichnet. Die Phasen des SDLC werden in Bezug auf die Entwicklung mobiler Anwendungen untersucht, einschließlich der Inspiration, des Entwurfs, der Entwicklung, Stabilisierung, Bereitstellung und Wartung.
1.   **Überlegungen**: Es gibt einige Überlegungen zum Erstellen mobiler Anwendung, insbesondere im Gegensatz zu herkömmlichen Web- oder Desktopanwendungen. Diese Überlegungen und ihre Auswirkungen auf die Entwicklung mobiler Anwendungen werden untersucht.

Dieses Dokument richtet sich sowohl an neue als auch an erfahrene Anwendungsentwickler und soll grundlegende Fragen über die Entwicklung mobiler Apps beantworten. Die meisten Konzepte, denen Sie während des gesamten Lebenszyklus der Softwareentwicklung (SDLC) begegnen, werden umfassend vorgestellt. Dieses Dokument ist jedoch nicht für jeden geeignet. Wenn Sie direkt mit dem Erstellen von Anwendungen beginnen möchten, wird empfohlen, direkt mit dem Leitfaden [Introduction to Mobile Development (Einführung in die Entwicklung mobiler Anwendungen)](~/cross-platform/get-started/introduction-to-mobile-development.md) weiter zu machen und später zu diesem Dokument zurückzukehren.

## <a name="mobile-development-sdlc"></a>Mobile Entwicklung – SDLC

Der Lebenszyklus der Entwicklung mobiler Anwendungen unterscheidet sich größtenteils nicht vom SDLC für Web- und Desktopanwendungen. Wie bei diesen gibt es in der Regel fünf wichtige Teile des Prozesses:

1.   **Anfang**: Jede App beginnt mit einer Idee. Diese Idee wird üblicherweise in eine solide Grundlage für eine Anwendung weiterentwickelt.
1.   **Entwurf**: Die Entwurfsphase besteht aus der Definition der Benutzererfahrung (UX) der App, z.B. das allgemeine Layout, wie dieses funktioniert usw., sowie das Entwerfen einer richtigen Benutzeroberfläche (UI) aus der UX. Dies geschieht üblicherweise mithilfe eines Grafikdesigners.
1.   **Entwicklung**: Üblicherweise die ressourcenintensivste Phase. Hier findet das tatsächliche Erstellen der Anwendung statt.
1.   **Stabilisierung**: Wenn die Entwicklung weit genug vorangeschritten ist, beginnt die Qualitätssicherung mit dem Testen der Anwendungen und Fehler werden behoben. Oft durchläuft eine Anwendung eine eingeschränkte Betaphase, in der sie ein breiteres Publikum verwenden sowie Feedback geben und Änderungen anregen kann.
1.  **Bereitstellung**

Häufig überlappen einige dieser Teile. So ist es zum Beispiel üblich, dass die Entwicklung fortschreitet, während die Benutzeroberfläche fertiggestellt wird und sogar den UI-Entwurf informiert. Zusätzlich kann eine Anwendung in eine Stabilisierungsphase versetzt werden, während neue Funktionen zu einer neuen Version hinzugefügt werden.

Weiterhin können diese Phasen in einer beliebigen Anzahl von SDLC-Methoden verwendet werden, z.B. Agile, Spiral, Waterfall usw.

Jede dieser Phasen wird in den folgenden Abschnitten ausführlicher erläutert.

### <a name="inception"></a>Anfang

Durch die Verbreitung und den Grad an Interaktion, über die die Menschen durch mobile Geräte verfügen, hat nahezu jeder eine Idee für eine mobile App. Mobile Geräte eröffnen völlig neue Möglichkeiten, um mit dem Computing, dem Web und sogar mit Unternehmensinfrastrukturen zu interagieren.

In der Anfangsphase wird nur die Idee für eine App definiert und verfeinert.
Es ist wichtig, sich einige grundlegende Fragen zu stellen, um eine erfolgreiche App zu erstellen. Folgendes müssen Sie berücksichtigen, bevor Sie eine App in einem der öffentlichen App Stores veröffentlichen:

-   **Wettbewerbsvorteil**: Gibt es bereits ähnliche Apps? Wenn ja, wie unterscheidet sich diese Anwendung von anderen?

Für Apps, die in einem Unternehmen verteilt werden sollen:

-   **Infrastrukturintegration**: Welche vorhandene Infrastruktur wird integriert oder erweitert?

Zusätzlich sollten Apps im Kontext des mobilen Formfaktors überprüft werden:

-   **Wert**: Welchen Wert hat die App für die Benutzer? Wie werden sie sie verwenden?
-   **Form/Mobilität**: Wie arbeitet diese App in einem mobilen Formfaktor? Wie kann ich den Wert durch das Verwenden von mobilen Technologien wie z.B. die Ortserkennung, Kamera usw. erhöhen?

Für das Entwerfen der Funktionalität einer App kann es hilfreich sein, Akteure und [Anwendungsfälle](http://en.wikipedia.org/wiki/Use_case) zu definieren. Akteure sind Rollen innerhalb einer Anwendung, die häufig von Benutzern eingenommen werden. Anwendungsfälle sind in der Regel Aktionen oder Absichten.

Eine Anwendung für die Nachverfolgung von Aufgaben kann beispielsweise zwei Akteure besitzen: *Benutzer* und *Freund*. Ein Benutzer kann *eine Aufgabe erstellen* und *eine Aufgabe freigeben* für einen Freund. In diesem Fall sind das Erstellen und Freigeben einer Aufgabe zwei verschiedene Anwendungsfälle, die Sie zusammen mit den Akteuren darüber informieren, welche Bildschirme Sie erstellen sowie welche Geschäftsentitäten und Logiken entwickelt werden müssen.

Sobald eine geeignete Anzahl von Anwendungsfällen und Akteuren erfasst wurde, ist es wesentlich einfacher, mit dem Entwerfen der Anwendung zu beginnen. Die Entwicklung kann sich dann auf die Entwicklung der App konzentrieren statt darauf, was die App tut oder tun soll.

### <a name="designing-mobile-applications"></a>Entwerfen mobiler Anwendungen

Sobald die Funktionen und Funktionalitäten der App bestimmt sind, sollten Sie sich im nächsten Schritt mit der Benutzererfahrung (UX) befassen.

#### <a name="ux-design"></a>Entwurf der UX

Die Benutzererfahrung (UX) wird üblicherweise mit Drahtmodellen oder Modellen mithilfe von [einem von vielen verfügbaren Entwurfstoolkits](https://docs.microsoft.com/windows/uwp/design/downloads/) entworfen. Durch UX-Modelle kann die UX entworfen werden, ohne den tatsächlichen UI-Entwurf berücksichtigen zu müssen:

 [![](introduction-to-mobile-sdlc-images/balsamiq.png "Die Benutzererfahrung wird üblicherweise mit Drahtmodellen oder Modellen mithilfe von Tools wie Balsamiq entworfen")](introduction-to-mobile-sdlc-images/balsamiq.png#lightbox)

Beim Erstellen von UX-Modellen ist es wichtig, die Richtlinien für die Benutzeroberfläche für die verschiedenen Zielplattformen der App zu berücksichtigen. Die App sollte den Anforderungen der verschiedenen Plattformen entsprechen. Die verschiedenen offiziellen Entwurfsrichtlinien für die jeweiligen Plattformen finden Sie unter:

1.   **Apple**: -  [Human Interface Guidelines (Eingaberichtlinien)](https://developer.apple.com/ios/human-interface-guidelines/overview/themes/)
1.   **Android**:[Design Guidelines (Entwurfsrichtlinien)](http://developer.android.com/design/index.html)
1.   **UWP**: [UWP Design basics (Grundlagen des UWP-Entwurfs)](https://docs.microsoft.com/windows/uwp/design/basics/)

Beispielsweise verfügt jede App über eine Metapher, über die man zwischen den einzelnen Abschnitten einer Anwendung wechseln kann. Bei iOS ist die Registerkartenleiste im unteren Bereich des Bildschirms platziert, bei Android im oberen Bereich, während bei UWP eine Ansicht mit [Pivots und Registerkarten](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tabs-pivot) verwendet wird.

Zudem werden UX-Entscheidungen von der Hardware an sich beeinflusst. Beispielsweise haben iOS-Geräte keine physische *Zurück-* Taste, weshalb Sie die Metapher Navigation Controller (Navigationscontroller) einfügen sollten.

 ![](introduction-to-mobile-sdlc-images/01-navigation-controller.png "iOS-Geräte haben keine physische Zurück-Taste, weshalb Sie die Metapher „Navigation Controller“ (Navigationscontroller) einfügen")

Auch Formfaktoren beeinflussen UX-Entscheidungen. Tablets sind viel größer als Smartphones und können daher auch mehr Informationen anzeigen. Häufig werden mehrere Anzeigen auf einem Smartphone auf Tablets auf nur eine Anzeige komprimiert.

 [![](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png "Häufig werden mehrere Anzeigen auf einem Smartphone auf Tablets zu nur einer Anzeige komprimiert")](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png#lightbox)

Angesichts der unzähligen bestehenden Formfaktoren gibt es häufig auch mittelgroße Formfaktoren (die zwischen einem Smartphone und einem Tablet liegen), die sie auch berücksichtigen sollten.

#### <a name="user-interface-ui-design"></a>Entwurf der Benutzeroberfläche (User Interface, UI)

Sobald die UX bestimmt ist, muss als nächstes die UI entworfen werden. Für die UX werden üblicherweise nur schwarz-weiße Modelle eingesetzt. Erst in der Entwurfsphase der UI werden Farben, Grafiken, usw. eingeführt und fertiggestellt. Es ist wichtig, dass Sie sich für den Entwurf der UI viel Zeit nehmen, denn generell gilt: Die beliebtesten Apps haben einen professionellen Entwurf.

Genauso wie bei der Erstellung der UX ist es wichtig zu bedenken, dass jede Plattform eine eigene Entwurfssprache hat. D.h., eine gut entworfene Anwendung wird möglicherweise trotzdem auf jeder Plattform anders aussehen.

 [![](introduction-to-mobile-sdlc-images/multiplatform-1.png "Eine überlegt entworfene Anwendung wird möglicherweise trotzdem auf jeder Plattform anders aussehen")](introduction-to-mobile-sdlc-images/multiplatform-1.png#lightbox)

### <a name="development"></a>Entwicklung

Die Entwicklungsphase beginnt in der Regel sehr früh. Oft wird ein funktionsfähiger Prototyp entwickelt, wenn eine Idee in der Konzeptions- bzw. Inspirationsphase genügend gereift ist. Dieser Prototyp prüft die Funktionalität sowie Annahmen und soll ein Bild über den Umfang der Arbeit abgeben.

In den restlichen Tutorials konzentrieren wir uns hauptsächlich auf die Entwicklungsphase.

### <a name="stabilization"></a>Stabilisierung

Im Rahmen der Stabilisierung sollten Sie sich auf das Finden von Fehlern in Ihrer App konzentrieren. Dabei geht es nicht nur um Funktionalität (z.B. „Die App stürzt ab, wenn man auf eine bestimmte Taste drückt“), sondern auch um Nutzbarkeit und Leistung. Am besten beginnen Sie bereits früh während der Entwicklung mit der Stabilisierung, damit Sie Kurskorrekturen vornehmen können, bevor hohe Kosten entstehen. In der Regel durchlaufen Anwendungen folgende Phasen: *Prototyp*, *Alpha*, *Beta* und *Release Candidate*. Es gibt zwar verschiedene Definitionen für diese Phasen, aber sie folgen für gewöhnlich folgendem Muster:

1.   **Prototyp**: Die App befindet sich noch in der sogenannten Proof-of-Concept-Phase und nur die Kernfunktionalitäten bzw. nur bestimmte Teile der Anwendung funktionieren. Es gibt noch große Fehler.
1.   **Alpha**: Die Kernfunktionalitäten sind für gewöhnlich Code Complete, d.h. sie wurden erstellt, aber noch nicht vollständig getestet. Es gibt immer noch große Fehler, und möglicherweise sind noch keine äußeren Funktionen vorhanden.
1.   **Beta**: Die meisten Funktionen wurden nun fertiggestellt und zumindest grob getestet. Außerdem wurden einige Fehler behoben. Es kann immer noch größere bekannte Fehler geben.
1.   **Release Candidate**: Alle Funktionen wurden fertiggestellt und getestet. Abgesehen von möglichen neuen Fehlern, kann die App auf den Markt gebracht werden.

Sie können nie zu früh mit dem Testen einer Anwendung beginnen. Wenn man z.B. in der Prototypenphase auf ein größeres Problem stößt, kann die UX einer App immer noch geändert werden, um dieses Problem zu beheben. Wenn in der Alphaphase ein Problem hinsichtlich der Leistung gefunden wird, ist immer noch genügend Zeit, um die Architektur zu ändern, bevor aufgrund von falschen Annahmen zu viel Code erstellt worden ist.

In der Regel wird eine Anwendung, je weiter deren Entwicklung in den einzelnen Phasen vorangeschritten ist, immer mehr Personen zum Testen zur Verfügung gestellt, damit diese Feedback usw. geben können. Anwendungen werden in der Prototypenphase möglicherweise nur den wichtigsten Beteiligten vorgeführt. Dagegen werden Anwendungen, die sich in der Release Candidate-Phase befinden, möglicherweise schon die Kunden verteilt, die sich für einen früheren Zugriff registriert haben.

Für frühe Tests und zur Bereitstellung für verhältnismäßig wenige Geräte ist es in der Regel ausreichend, wenn die Anwendung direkt von einem Entwicklungscomputer aus zur Verfügung gestellt wird. Wenn sich aber die Zielgruppe vergrößert, wird diese Option sehr schnell sehr aufwändig. Aus diesem Grund gibt es eine Reihe von Optionen zur Testbereitstellung, die diesen Vorgang stark vereinfachen: Sie können Personen zu einem Test-Pool einladen, Builds über das Internet veröffentlichen und Tools für Benutzerfeedback zur Verfügung stellen.

Zum Testen und Bereitstellen können Sie das [App Center](https://appcenter.ms/) verwenden, um fortlaufend Apps zu erstellen, zu testen, zu veröffentlichen und zu überwachen.

### <a name="distribution"></a>Verteilung

Sobald Sie die Anwendung stabilisiert haben, können Sie sie veröffentlichen. Es gibt eine Reihe von Verteilungsoptionen, die von der jeweiligen Plattform abhängig sind.

#### <a name="ios"></a>iOS

Apps, die mit Xamarin.iOS und Objective-C erstellt wurden, werden auf dieselbe Art und Weise verteilt:

1.   **Apple App Store**: Der App Store von Apple ist ein global verfügbares Repository für Onlineanwendungen, das in Mac OS X in iTunes eingebaut wurde. Es handelt sich dabei mit Abstand um die beliebteste Verteilungsmethode für Anwendungen, denn Entwickler können darüber ihre Apps mit geringem Aufwand auf den Onlinemarkt bringen und verteilen.
1.   **Interne Bereitstellung**: Diese Methode wird zur internen Verteilung von Unternehmensanwendungen eingesetzt, die nicht im App Store Für die Öffentlichkeit zugänglich sind.
1.   **Ad-hoc-Bereitstellung**: Die Ad-hoc-Bereitstellung soll hauptsächlich für die Entwicklung und Tests verwendet werden. Mit dieser Methode können Sie Ihre Anwendung auf eine begrenzte Anzahl von bereitgestellten Geräten verteilen. Beispielsweise handelt es sich um eine Ad-hoc-Bereitstellung, wenn Sie eine Anwendung über Xcode oder Visual Studio für Mac auf einem Gerät bereitstellen.

#### <a name="android"></a>Android

Alle Android-Anwendungen müssen vor der Verteilung signiert werden. Entwickler signieren Ihre Anwendungen mit ihrem eigenen Zertifikat, das von einen privaten Schlüssel geschützt wird. Dieses Zertifikat kann das Produkt authentischer machen, da dadurch ein Anwendungsentwickler mit den Anwendungen, die er erstellt und veröffentlicht hat, in Verbindung gebracht werden kann.
Beachten Sie, dass zwar ein Entwicklungszertifikat für Android von einer anerkannten Zertifizierungsstelle signiert werden kann, jedoch die meisten Entwickler diesen Dienst nicht in Anspruch nehmen und stattdessen ihre Zertifikate selbst signieren. Mithilfe dieser Zertifikate soll vor allem zwischen den verschiedenen Entwicklern und Anwendungen unterschieden werden.
Android verwendet diese Information, um bei der Delegation von Berechtigungen zwischen Anwendungen und Komponenten, die unter Android laufen, Unterstützung zu leisten.

Im Gegensatz zu anderen beliebten mobilen Plattformen, hat Android eine offene Herangehensweise an die Appverteilung. Geräte sind nicht nur für einen einzigen genehmigten App Store freigeschaltet.
Stattdessen kann jeder einen App Store erstellen, und auf den meisten Android-Smartphones ist es möglich, Apps aus Stores von Drittanbietern zu installieren.

Dadurch haben die Entwickler Zugang zu einem Verteilungskanal für ihre Anwendungen, der zwar größer, aber auch viel komplexer ist. [Google Play](https://play.google.com/store?hl=en) ist zwar der offizielle App Store von Google, aber es gibt noch viele andere. Die Beliebtesten sind:

1.  [AppBrain](http://www.appbrain.com/)
1.  [Amazon App Store for Android (Amazon Appstore für Android)](http://www.amazon.com/mobile-apps/b?ie=UTF8&amp;node=2350149011)
1.  [Handango](http://www.handango.com/)
1.  [GetJar](http://www.getjar.com/)

#### <a name="uwp"></a>UWP 

UWP-Anwendungen werden über den Microsoft Store an die Benutzer verteilt. Entwickler senden ihre Apps zur Genehmigung an das Windows Phone Dev Center. Weitere Informationen zum Veröffentlichen von Windows-Apps finden Sie in der [UWP-Dokumentation zum Veröffentlichen](https://docs.microsoft.com/windows/uwp/publish/).

## <a name="mobile-development-considerations"></a>Überlegungen zur mobilen Entwicklung

Obwohl sich das Entwickeln von mobilen Anwendungen im Hinblick auf die Prozesse und die Architektur nicht grundlegend von der Web- bzw. Desktopentwicklung unterscheidet, sind einige Überlegungen zu beachten.

### <a name="common-considerations"></a>Allgemeine Überlegungen

#### <a name="multitasking"></a>Multitasking

Es gibt zwei erhebliche Herausforderungen beim Multitasking (d.h. wenn mehrere Anwendungen gleichzeitig ausgeführt werden) auf mobilen Geräten. Zum einen ist die Bildschirmgröße eingeschränkt, wodurch es schwierig ist, mehrere Anwendungen gleichzeitig anzuzeigen. Aus diesem Grund kann auf mobilen Geräten immer nur eine App im Vordergrund ausgeführt werden. Zum anderen kann sich der Akku schnell leeren, wenn mehrere Anwendungen gleichzeitig geöffnet sind und Aufgaben ausführen.

Multitasking wird auf jeder Plattform anders gehandhabt, worauf wir später noch eingehen wollen.

#### <a name="form-factor"></a>Formfaktor

Mobile Geräte werden in der Regel in zwei Kategorien unterteilt: Smartphones und Tablets. Einige Crossover-Geräte werden zwischen diesen beiden Kategorien angeordnet. Das Entwickeln für diese Formfaktoren ähnelt sich in der Regel, allerdings kann das Entwerfen von Anwendungen große Unterschiede aufweisen.
Der Bildschirmbereich von Smartphones ist eingeschränkt, und auch bei Tablets, die zwar größer sind, handelt es sich immer noch um mobile Geräte, die einen kleineren Bildschirmbereich haben als die meisten Laptops. Aus diesem Grund wurden UI-Steuerelemente auf mobilen Plattformen vor allem entworfen, damit sie auf kleineren Formfaktoren effektiver sind.

#### <a name="device-and-os-fragmentation"></a>Fragmentieren von Geräten und Betriebssystemen

Es ist wichtig, dass im gesamten Lebenszyklus der Softwareentwicklung verschiedene Geräte berücksichtigt werden.

1.   **Konzeptualisierung und Planung**: Beachten Sie, dass sowohl die Hardware als auch die Funktionen jedes Geräts anders sind – eine Anwendung, die auf bestimmen Funktionen basiert, läuft möglicherweise auf bestimmten Geräten nicht einwandfrei. Beispielsweise ist nicht in allen Geräten eine Kamera eingebaut. D.h., wenn Sie eine Anwendungen für Videonachrichten erstellen, können manche Geräte Videos zwar abspielen, aber nicht aufnehmen.
1.   **Entwurf**: Berücksichtigen Sie die unterschiedlichen Bildschirmverhältnisse und -größen für die verschiedenen Geräte, wenn Sie die UX einer Anwendung entwerfen. Außerdem sollten Sie die unterschiedlichen Bildschirmauflösungen beachten, wenn Sie die Benutzeroberfläche (UI) einer Anwendung entwerfen.
1.   **Entwicklung**: Wenn Sie eine Funktion aus dem Code verwenden, sollte diese Funktion zunächst getestet werden. Bevor Sie z.B. zum Beispiel eine Gerätefunktion wie die Kamera verwenden, sollten Sie immer das Betriebssystem dahingehend abfragen, ob es diese Funktion gibt. Wenn Sie anschließend die Funktion bzw. das Gerät initialisieren, sollten Sie derzeit vom Betriebssystem unterstützte Funktionen anfordern und diese Konfigurationseinstellungen anschließend verwenden.
1.   **Testen**: Es ist wichtig, dass Sie die Anwendung schon früh auf entsprechenden Geräten testen. Sogar Geräte mit denselben Hardwarespezifikationen können stark in ihrem Verhalten voneinander abweichen.

#### <a name="limited-resources"></a>Begrenzte Ressourcen

Mobile Geräte werden zwar immer leistungsstärker, sie verfügen aber im Vergleich zu Desktopcomputern oder Notebooks immer noch über eingeschränkte Funktionen. Beispielsweise müssen sich Desktopentwickler in der Regel keine Gedanken über Kapazitäten des Arbeitsspeichers machen. Sie sind daran gewöhnt, dass ihnen sowohl reichlich physischer Speicher als auch genügend virtueller Arbeitsspeicher zur Verfügung steht. Bei mobilen Geräten kann es schnell passieren, dass der gesamte zur Verfügung stehende Arbeitsspeicher aufgebraucht wird, wenn nur eine Reihe von Bildern in hoher Bildqualität geladen werden.

Außerdem können prozessorintensive Anwendungen wie Spiele oder Texterkennung die mobile CPU stark strapazieren, was sich negativ auf die Geräteleistung auswirkt.

Aufgrund der vorangegangenen Überlegungen ist es wichtig, dass Sie intelligent programmieren und Ihre Anwendung schon früh und wiederholt auf entsprechenden Geräten bereitstellen, um die Reaktionsfähigkeit zu überprüfen.

### <a name="ios-considerations"></a>Überlegungen zu iOS

#### <a name="multitasking"></a>Multitasking

Multitasking wird unter iOS streng kontrolliert, und es gibt eine hohe Anzahl von Regeln und Verhaltensweisen, mit denen Ihre Anwendung kompatibel sein muss, wenn eine andere Anwendung in den Vordergrund tritt. Ansonsten beendet iOS Ihre Anwendung.

#### <a name="device-specific-resources"></a>Gerätespezifische Ressourcen

Innerhalb eines bestimmten Formfaktors kann sich die Hardware der verschiedenen Modelle stark unterscheiden. Beispielsweise haben manche Geräte nur eine nach hinten gerichtete Kamera, andere haben sowohl eine nach hinten als auch eine nach vorne gerichtete Kamera, und wieder andere haben gar keine Kamera.

Auf einigen älteren Geräten (iPhone 3G und älter) ist gar kein Multitasking möglich.

Aufgrund dieser Unterschiede zwischen den Gerätemodellen ist es wichtig zu prüfen, ob eine Funktion vorhanden ist, bevor Sie versuchen, sie zu verwenden.

#### <a name="os-specific-constraints"></a>Vom Betriebssystem abhängige Einschränkungen

Damit sichergestellt werden kann, dass die Anwendungen reaktionsfähig und sicher sind, erzwingt iOS eine Reihe von Regeln für Anwendungen, die befolgt werden müssen. Neben den Regeln, die das Multitasking betreffen, gibt es eine Reihe von Ereignismethoden, aus denen Ihre App innerhalb einer gewissen Zeit zurückkehren muss. Ansonsten beendet iOS die App.

Außerdem sollten Sie bedenken, dass Apps in einer sogenannten Sandbox ausgeführt werden. Dabei handelt es sich um eine Umgebung, die Sicherheitseinschränkungen für die Zugriffsrechte der App erzwingt. Beispielsweise kann eine App zwar ihr eigenes Verzeichnis auslesen und darin schreiben, wenn sie aber versucht, in ein anderes Appverzeichnis zu schreiben, wird sie beendet.

### <a name="android-considerations"></a>Überlegungen zu Android

#### <a name="multitasking"></a>Multitasking

Multitasking unter Android hat zwei Komponenten. Die erste Komponente ist der Aktivitätslebenszyklus. Jede Anzeige in einer Android-Anwendung wird von einer Aktivität dargestellt, und es gibt verschiedene Ereignisse, die auftreten, wenn eine Anwendung im Hintergrund platziert wird bzw. in den Vordergrund gestellt wird. Anwendungen müssen diesen Lebenszyklus einhalten, damit sie reaktionsfähig sind und funktionieren. Weitere Informationen finden Sie im Leitfaden [Activity Lifecycle (Aktivitätslebenszyklus)](~/android/app-fundamentals/activity-lifecycle/index.md).

Die zweite Multitasking-Komponente unter Android ist die Verwendung von Diensten.
Dienste sind Prozesse mit langer Laufzeit, die unabhängig von der Anwendung bestehen und verwendet werden, um Prozesse auszuführen, während eine Anwendung im Hintergrund läuft. Weitere Informationen finden Sie im Leitfaden [Creating Services (Erstellen von Diensten)](~/android/app-fundamentals/services/index.md).

#### <a name="many-devices-and-many-form-factors"></a>Viele Geräte und viele Formfaktoren

Es bestehen keine Einschränkungen von Google, auf welchem Gerät das Android-Betriebssystem ausgeführt werden kann. Dieses offene Paradigma hat zur Folge, dass eine Produktumgebung entsteht, die aus unzähligen verschiedenen Geräten besteht, die sich alle in der Hardware, der Bildschirmauflösung und -abmessung, den Gerätefunktionen und den Funktionen unterscheiden.

Aufgrund der starken Fragmentierung von Android-Geräten, entscheiden sich die meisten Entwickler dafür, ihre Anwendungen für die beliebtesten fünf bis sechs Geräte zu entwerfen, diese Geräte zum Testen zu verwenden und sie zu priorisieren.

#### <a name="security-considerations"></a>Sicherheitsüberlegungen

Anwendungen unter Android werden alle unter einer eindeutigen und isolierten Identität mit eingeschränkten Berechtigungen ausgeführt. Standardmäßig haben Anwendungen nur sehr wenige Berechtigungen. Beispielsweise kann eine Anwendung ohne spezielle Berechtigungen keine Textnachrichten senden, nicht den Status des Telefons bestimmen oder sie haben noch nicht einmal Zugriff zum Internet. Damit die Anwendung Zugriff auf diese Funktionen erhält, muss in der Anwendungsmanifestdatei angegeben werden, welche Berechtigungen die Anwendung haben soll, und wenn sie dann installiert wird, liest das Betriebssystem diese Berechtigungen aus, benachrichtigt den Benutzer, dass die Anwendung diese Berechtigungen anfordert, und gibt ihm dann die Möglichkeit, mit der Installation entweder fortzufahren oder diese abzubrechen.
Dies ist ein entscheidender Schritt im Verteilungsmodell von Android. Es handelt sich um ein offenes App Store-Modell, da Anwendungen nicht wie beispielsweise bei iOS geprüft werden. Eine Liste mit Anwendungsberechtigungen finden Sie unter [Manifest Permissions (Bekannte Berechtigungen)](http://developer.android.com/reference/android/Manifest.permission.html) in der Android-Dokumentation.

### <a name="windows-considerations"></a>Überlegungen zu Windows

#### <a name="multitasking"></a>Multitasking

Multitasking bei UWP besteht aus zwei Teilen: dem Lebenszyklus für Seiten und Anwendungen einerseits und Hintergrundprozessen andererseits. Jede Ansicht in einer Anwendung ist eine Seitenklasseninstanz, bei der Ereignisse entweder aktiv oder inaktiv sind (dabei gibt es spezielle Regeln zur Verarbeitung eines inaktiven Zustands bzw. für den Tombstone-Zustand). 

Der zweite Teil des Multitasking besteht daraus, Hintergrund-Agents zur Verarbeitung von Aufgaben zur Verfügung zu stellen, wenn die App nicht im Vordergrund ausgeführt wird. 

#### <a name="device-capabilities"></a>Gerätefunktionen

Obwohl die UWP-Hardware recht einheitlich ist, gibt es trotzdem noch Komponenten, die optional sind, und daher bei der Entwicklung besonders berücksichtigt werden müssen. Optionale Hardwarefunktionen sind z.B. die Kamera, der Kompass und das Gyroskop. Es gibt außerdem eine spezielle Klasse mit geringem Arbeitsspeicher (256 MB), die entweder besonders berücksichtigt werden muss, oder Sie stellen die Unterstützung von Geräten mit geringem Arbeitsspeicher ein.

#### <a name="security-considerations"></a>Sicherheitsüberlegungen

Weitere Informationen zu wichtigen Sicherheitsfaktoren bei UWP finden Sie in der [Dokumentation zur Sicherheit](https://docs.microsoft.com/windows/uwp/security/).

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde der Lebenszyklus der Softwareentwicklung im Zusammenhang mit der mobilen Entwicklung vorgestellt. Allgemeine Überlegungen zum Erstellen von mobilen Anwendungen wurden erläutert, und eine Reihe von plattformspezifischen Überlegungen u.a. zum Entwerfen, Testen und zur Bereitstellung wurden untersucht.

## <a name="related-links"></a>Verwandte Links

- [Introduction to Mobile Development (Einführung in die Entwicklung mobiler Anwendungen)](~/cross-platform/get-started/introduction-to-mobile-development.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hallo, Android](http://developer.xamarin.com/get-started-droid/)
- [Application Fundamentals (Anwendungsgrundlagen)](~/cross-platform/app-fundamentals/index.md)
