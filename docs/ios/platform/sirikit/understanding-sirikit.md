---
title: Grundlegendes zu SiriKit-Konzepten
description: Dieses Dokument beschreibt die wichtigsten Konzepte für die Arbeit mit SiriKit in einer Xamarin.iOS-app erforderlich sind. Z. B. erläutert Intents und Intents UI-Erweiterungen, SiriKit Berechtigungen Entwerfen einer überzeugenden Umgebung und vieles mehr.
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: b72246968c9b321329e56fd51eaaa98a1625922e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123075"
---
# <a name="understanding-sirikit-concepts"></a>Grundlegendes zu SiriKit-Konzepten

_Dieser Artikel behandelt die grundlegenden Konzepte, die für die Arbeit mit SiriKit in einer Xamarin.iOS-app benötigt werden._


Neue iOS 10, SiriKit kann eine Xamarin.iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät zugänglich sind. Diese Funktion finden Sie im App-Erweiterung für eine oder mehrere mit dem neuen **Intents** und **Intents UI** Frameworks.

SiriKit ermöglicht eine iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät mithilfe von App-Erweiterungen und neuen zugänglich sind **Intents** und **Intents UI** Frameworks.

Siri arbeitet mit dem Konzept der **Domänen**, wissen, Gruppen von Aktionen für verwandte Aufgaben. Jede Interaktion mit die app mit Siri muss wie folgt in eine Domäne bekannten Diensts liegen:

- Audio- oder Videoanruf.
- Buchung einer Tour an.
- Verwalten von fitnessaktivitäten aus.
- Messaging.
- Durchsuchen von Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri im Zusammenhang mit einer der Dienste für die App-Erweiterung stellt, sendet SiriKit die Erweiterung einer **Absicht** Objekt, das die Anforderung des Benutzers sowie alle unterstützungsdaten beschreibt. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Informationen, wie die Erweiterung für die Anforderung verarbeiten kann.

## <a name="the-intents-and-intents-ui-extensions"></a>Die Intents und Intents UI-Erweiterungen

Interagieren mit der app-Diensten über zwei verschiedene Arten von App-Erweiterungen, sowohl Siri und die Karten-app:

- **Intents-Erweiterung** -bietet Siri und Karten mit der app, den Inhalt, und führt die Aufgaben, die erforderlich sind, um alle unterstützten Absichten zu erfüllen.
- **Intents-Benutzeroberflächenerweiterung** -bietet eine benutzerdefinierte Benutzeroberfläche, die angezeigt wird, für die app Inhalte in entweder Siri- oder Maps.

Die app muss eine Intents-Erweiterung zur Unterstützung von SiriKit bereitstellen, und er ist verantwortlich für die Bereitstellung von Informationen, die für dem Benutzer Siri und Zuordnungen vorweisen kann und für die Behandlung von Intents.

Erstellt eine Intents-Benutzeroberflächenerweiterung ist optional, da Siri in der Regel alle Benutzerinteraktionen behandelt, und verfügt über einen integrierten Benutzeroberfläche zum Darstellen von Informationen in jeder der unterstützten Domänen. Die app kann durch die Bereitstellung eine Intents-Benutzeroberflächenerweiterung, verwenden die **beabsichtigte Benutzeroberfläche** Framework, um eine umfassende, benutzerdefinierte Benutzeroberfläche mit dem branding die app zu präsentieren und zusätzliche Informationen.

## <a name="siri-and-the-maps-app-role"></a>Siri und die Karten-App-Rolle

Die Anforderungen der Benutzer gesprochen werden Sprache verarbeitet und semantisch analysiert von Siri auf die Anforderungen in handlungsrelevante Intents deaktiviert wird, der die Absicht Erweiterungen verarbeiten kann.

Maps verwendet die app Absichten Erweiterungen zum Anzeigen von Informationen in der Mapping-Interface als Reaktion auf Benutzeraktionen. Werden z. B. nahe gelegene Restaurants anfordern oder für das Abrufen der app-Restaurant.

Sowohl Siri und Zuordnungen Verwalten aller die Interaktionen des Benutzers, und Ergebnisse, die mithilfe der Standardsystem-Benutzeroberfläche anzeigen. Die app-Erweiterungen-Rolle ist in erster Linie um die Daten bereitzustellen, die angezeigt wird. Die app kann optional eine Intents-Benutzeroberflächenerweiterung bereitzustellen und eine benutzerdefinierte Benutzeroberfläche um die Standardschnittstelle für das System zu verbessern.

## <a name="interacting-with-siri-via-sirikit"></a>Interaktion mit Siri über SiriKit

Dieser Abschnitt bietet einen Überblick darüber, wie Benutzer für die Interaktion mit der app mithilfe von Siri von SiriKit kann. Dieses Beispiel wird die gefälschte MonkeyChat-app verwendet werden:

[![](understanding-sirikit-images/monkeychat01.png "Das Symbol \"MonkeyChat\"")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat behält eine eigene wenden Sie sich an Buchs für die Freunde des Benutzers, die jeweils einem Bildschirmnamen (z. B. Bobo z. B.) zugeordnet, und ermöglicht dem Benutzer jeder Freund von seinem Bildschirmnamen Chats von Text an.

Es gibt viele Möglichkeiten, dass der Benutzer möglicherweise eine Interaktion mit der app initiieren können, da verschiedene Personen möglicherweise dieselbe Anforderung in vielen verschiedenen Formen getroffen.

Z. B. wenn der Benutzer zum Senden einer Nachricht an ihre Freund Bobo, muss er die folgende Konversation mit Siri möglicherweise:

_Benutzer: Hallo Siri, senden Sie eine Nachricht MonkeyChat._<br />
_Siri: um die?_<br />
_Benutzer: Bobo._<br />
_Siri: Was möchten Sie für Bobo ein?_<br />
_Benutzer: Bitte senden Sie weitere Bananen._<br />

Eine andere Person erstellt möglicherweise die gleiche Anforderung mit einem anderen Gespräch:

_Benutzer: Senden einer Nachricht an Bobo auf MonkeyChat._<br />
_Siri: Was möchten Sie für Bobo ein?_<br />
_Benutzer: Bitte senden Sie weitere Bananen._<br />

Und ein anderer Benutzer kann eine noch kürzere anfordern:

_Benutzer: MonkeyChat Bobo senden Sie weitere Bananen._<br />
_Siri: Ok, senden Nachrichten senden Sie weitere Bananen an Bobo auf Monkeychat._<br />

Oder stellen Sie die gleiche Anforderung auch in einer anderen Sprache:

_Benutzer: MonkeyChat Bobo s'il Vous Plaît Envoyer plus de Bananes._<br />
_Siri: Oui, Envoi Nachricht s'il Vous Plaît Envoyer plus de Bananes Kenntnis Bobo Sur Monkeychat._<br />

Noch sehr ausführlich in die Konversation möglicherweise ein anderer Benutzer:

_Benutzer: Hallo Siri, können Sie Sie tun Sie mir einen gefallen und starten Sie die app MonkeyChat senden eine SMS mit der Nachricht senden Weitere Bananen._<br />
_Siri: um die?_<br />
_Benutzer: Meine bewährte Pal Bobo._<br />

Darüber hinaus stehen viele Möglichkeiten, die Siri reagieren kann, auf eine Anforderung, einige basierend auf wie die Anforderung gestellt wurde:

- **Durch Drücken der Schaltfläche "Start"** -Siri bieten mehr visual Reaktionen beschränkt mündliche Feedback zu senden.
- **Von "Hey Siri"** -Siri werden mehr mündliche und geben Sie weniger visual Antworten.

Siri wurde auch optimiert, um den Zugriff auf Anforderungen des Benutzers und interagiert, und basierend auf diesen Anforderungen zu reagieren.

Unabhängig davon, wie eine Anforderung gestellt wird, oder wie Siri auf die Anforderung reagiert Siri behandelt die Konversation mit dem Benutzer, und die app (über ihre Erweiterungen) stellt die Funktionalität bereit.

Wenn der Benutzer eine mündliche von Siri anfordert, sind dies die folgenden Schritte Siri wird:

[![](understanding-sirikit-images/monkeychat02.png "Die folgenden Schritte Siri wird")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Siri wird zuerst das Audio des des Benutzers **Spracherkennung** und in Text konvertiert wird.
2. Als Nächstes wird der Text in konvertiert eine **Absicht**, eine strukturierte Darstellung der Anforderung des Benutzers.
3. Basierend auf der Absicht, ist Siri dauert **Aktion** zum Ausführen der Anforderung des Benutzers.
4. Schließlich bietet Siri **Antworten** (visual und verbalen) für den Benutzer basierend auf die durchzuführende Aktion.

Es gibt drei Hauptmethoden, die die app des Benutzers-Konversation mit Siri teilnehmen kann:

[![](understanding-sirikit-images/monkeychat03.png "Die drei Hauptmethoden, dass die app an die Benutzer-Konversation mit Siri teilnehmen kann")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Vokabular** – Dies ist die app Siri die Wörter mitteilen, wie sie wissen Sie mit ihm interagieren muss.
2. **App-Logik** : Hierbei handelt es sich um die Aktionen und Antworten, mit denen die app gelangen auf Basis der angegebenen Intent-Elemente.
3. **Benutzeroberfläche** – Dies ist die optionalen, benutzerdefinierten Benutzeroberfläche, die Antworten in die app übergeben werden kann.

### <a name="example"></a>Beispiel

Wenn die obige Informationen, untersuchen Sie, wie die folgende Konversation mit der app MonkeyChat interagieren würde:

_Benutzer: Hallo Siri, Senden einer Nachricht an Bobo auf MonkeyChat._<br />
_Siri: Was möchten Sie für Bobo ein?_<br />
_Benutzer: Bitte senden Sie weitere Bananen._<br />

Die erste Rolle, die die app in der Konversation verwendet, besteht darin, können Siri, die der Benutzer die Spracheingabe zu verstehen:

[![](understanding-sirikit-images/monkeychat04.png "Helfen Siri, die die Sprache der Benutzer zu verstehen.")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri verfügt nicht über den Namen "Bobo" in der Datenbank, aber die app ist und für Sie freigegeben hat diese Informationen Siri über des Vokabulars. Die app hilft auch Siri erkennen, dass Bobo einen Empfänger an, ist, da sie siri wie angegeben ein *wenden Sie sich an*.

Siri weiß, dass weitere ist erforderlich, um eine Nachricht als nur einen Empfänger an, zu senden, damit wird schnell überprüft, mit der App-Erweiterung, um festzustellen, ob eine Nachricht Inhalt erforderlich ist. Da MonkeyChat der Fall ist, antwortet Siri für den Benutzer mit der Frage: *"Was möchten Sie sagen zu Bobo?"*

Im obigen Beispiel hat der Benutzer geantwortet, *"Senden Sie weitere Bananen"*, die Siri in eine strukturierte bündeln wird **Absicht**:

[![](understanding-sirikit-images/monkeychat05.png "Siri wird die Antwort des Benutzers in eine strukturierte Absicht bündeln.")](understanding-sirikit-images/monkeychat05.png#lightbox)

Die strukturierte Absicht enthält die folgende Informationen:

- **Domäne:** Nachrichten
- **Zweck:** SendMessage
- **Empfänger:** Bobo
- **Inhalt:** senden Sie eine weitere Bananen

Jede Domäne verfügt, als Satz von wissen *Aktionen* , kann darin ausgeführt werden und basierend auf der Domäne und die Aktion, 0 (null), die zu viele Parameter in der Absicht eingeschlossen werden können, an die app gesendet.

Die Absicht wird an die App-Erweiterung für die Verarbeitung, klicken Sie dann gesendet. Als Ergebnis der Verarbeitung der Absicht an, die app generiert ein **IntentResponse** der wird mit der Absicht gebündelt werden, und schließen Sie Parameter, die beschreiben, was die app mit der Absicht hat.

Jede IntentResponse umfasst auch eine **Antwortcode** wodurch Siri angewiesen wird, wenn die app die Anforderung oder nicht abgeschlossen werden konnte. Einige Domänen haben sehr spezifische Fehlerantwort geführt haben, die auch gesendet werden können.

Zum Schluss die IntentResponse umfasst eine `NSUserActivity` (z. B. die zur Unterstützung von Hand aus verwendet werden). Die `NSUserActivity` wird verwendet, um die app zu starten, wenn die Antwort erforderlich ist, lassen die Siri-Umgebung, und geben die app, um es abzuschließen. 

Siri erstellt automatisch eine entsprechende `NSUserActivity` zum Starten der app "und" Pickup der Benutzer, wo in der Umgebung Siri unterbrochen. Die app kann jedoch Bereitstellen eigener `NSUserActivity` mit benutzerdefinierten Informationen, wenn dies erforderlich ist.

Nachdem die app verarbeitet die Absicht und hat eine Antwort siri zurückgegeben hat, wird Sie dann die Ergebnisse an dem Benutzer (verbal und visuell):

[![](understanding-sirikit-images/monkeychat06.png "Die Ergebnisse, die dem Benutzer angezeigten verbal sowohl visuell")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri enthält mehrere integrierte Antwort von Benutzeroberflächen für jede der Domänen für die app zur Verfügung. Allerdings da MonkeyChat eine optionale Erweiterung des Intent-Benutzeroberfläche zur Verfügung gestellt hat, wird es verwendet die Ergebnisse der Konversation, die dem Benutzer im obigen Beispiel angezeigt.

## <a name="the-intent-lifecycle"></a>Der beabsichtigte Lebenszyklus

Es gibt drei Hauptaufgaben, die die App-Erweiterung zum Ausführen beim Umgang mit Intent-Elemente müssen:

[![](understanding-sirikit-images/monkeychat07.png "Der beabsichtigte Lebenszyklus")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. Die app muss **beheben** jeden Parameter auf ein Ereignis. Daher wird die app auflösen, die mehrere Male (einmal pro jeden Parameter), und manchmal mehrmals auf denselben Parameter aufrufen, bis die app und der Benutzer zustimmen, auf was angefordert wird.
2. Die app muss **bestätigen** , behandeln das angeforderte Ziel und das erwartete Ergebnis Siri erzählen können.
3. Abschließend muss von die app **behandeln** die Absicht, und führen Sie die Schritte, um das gewünschte Ergebnis zu erzielen.

### <a name="the-resolve-stage"></a>Der Resolve-Phase

Die Resolve-Stufe können Siri, die die Werte zu verstehen, die der Benutzer wurde bereitgestellt und stellt sicher, dass der Benutzer tatsächlich vorgesehen ist, was passiert, wenn das Ziel von der Anwendung verarbeitet wird. 

Diese Phase ermöglicht auch für die app aus, um während der Konversation mit dem Benutzer Siris-Verhalten zu beeinflussen. Zu diesem Zweck die app bietet eine **Auflösung Antwort**. Es gibt eine Anzahl von vordefinierten Antwort auf die verschiedenen Typen von Daten, die Siri versteht.

Die am häufigsten verwendeten Auflösung Antwort der app werden **Erfolg**, d. h. die app das bestimmte Datenelement aus einem Parameter (z. B. Bildschirmname des Benutzers) zugeordnet, einem Teil der Informationen, die Informationen zu.

Es gibt möglicherweise Zeiten, wenn die app benötigt, um sicherzustellen, dass eine bestimmte Anforderung die richtigen Information übereinstimmt, die er kennt. In diesen Fällen sendet eine **ConfirmationRequired** als Antwort auf eine Ja oder Nein, die dem Benutzer z. B. Frage *"Send message, Bobo großartigen?"*

Möglicherweise gibt es andere Fälle, in denen muss die app den Benutzer, eine kurze Liste der Optionen auswählen. In diesem Fall die app bietet eine **zur Mehrdeutigkeitsvermeidung** Antwort mit einer Liste von zwei bis zehn-Optionen für die Benutzer z. B. auswählen: 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri den Benutzer die Auswahl, verbal oder durch Interaktion mit der Siri UI behandelt wird, und das Ergebnis wird an die app gesendet werden.

In anderen Fällen ist es möglicherweise nicht genügend Informationen für die app aus, um den Parameter zu beheben, oder gibt es möglicherweise zu viele Übereinstimmungen zum Auflösen von using zur mehrdeutigkeitsvermeidung (z. B. 80 Benutzern mit Bobo in ihrem Namen). In diesen Fällen die app sendet eine **NeedsMoreDetails** Antwort und Siri fordert den Benutzer genauer.

Wenn der Benutzer einen Wert, die den Prozess der Absicht erforderlich ist angeben nicht haben, kann er senden eine **NeedsValue** Reaktion, damit Siri, die den Benutzer auf den Wert anzugeben.

Wenn die app einen-Wert unterstützt, die der Benutzer für einen bestimmten Parameter erteilt hat, kann er senden die **UnsupportedWithReason** als Antwort auf einen Grund angeben, warum der Wert war nicht unterstützt. Siri fordert dann den Benutzer für einen vollständig neuen Wert ein, und bei ihnen der Grund dafür ist erforderlich.

Verwenden Sie schließlich die **NotRequired** Antwort Siri mitteilen, dass die app einen Wert für einen bestimmten Parameter erfordert. Wenn der Benutzer eine trotzdem bereitstellt, wird es von Siri einfach ignoriert.

### <a name="the-confirm-stage"></a>Die Confirm-Phase

Die Phase bestätigen hat zwei Aufgaben:

- Siri mitteilen, das erwartete Ergebnis ein Intent-Objekt zu verarbeiten, damit Siri den Benutzer überprüfen kann, was geschehen soll.
- Bietet einer Möglichkeit Überprüfung erforderlichen Zustände, die die Anwendung benötigen könnte, um die Anforderung vom Benutzer, z. B., dass ausreichende Geldmittel der Bank für die Zahlung für die angeforderte bereitgestellten abzuschließen.

Die app bietet ein **Absicht Antwort** aus dem Schritt bestätigen, die aufgefüllt werden soll mit, wie viele Informationen der app zur Verfügung hat, sodass Siri mit dem Benutzer effektiv kommunizieren kann.

Basierend auf der Domäne und zu den Aktion-Typ, möglicherweise Siri den Benutzer zur Bestätigung, Eingabe wie z. B. vor dem Senden einer Zahlungsausgangs oder Buchung einer Tour.

### <a name="the-handle-stage"></a>Der Handle-Phase

Der Phase verarbeitet, ist der wichtigste Teil der Arbeit mit Intent, da es der Punkt ist, in dem die app die Anforderung des Benutzers durch Ausführen der Aufgabe, die es Sie gepostet wurde erfüllt.

Wie sie in der Phase zu bestätigen, muss die app so viele Informationen über das Ergebnis wie möglich bereitstellen, damit diese Siri auf Benutzer beziehen kann. Auch diese Informationen werden erhält visuell oder anderen Zeiten Siri einfach zurück an den Benutzer sprechen werden. 

Möglicherweise Zeiten, wenn die app möglicherweise zusätzliche Zeit zum Verarbeiten einer angegebenen Anforderung, z. B. der Aufruf von netzwerkverzögerungen erfordern oder wenn eine live-Person zur Erfüllung der Anforderung (z. B. abschließen und Lieferung eines Auftrags oder ein Auto einbringen, um den Standort des Benutzers muss). Wenn Siri eine Antwort von der app wartet, werden angezeigt eine warten-Benutzeroberfläche für den Benutzer, der angibt, dass die app die Anforderung verarbeitet.

Im Idealfall sollte die app eine Antwort auf Siri innerhalb von zwei bis drei Sekunden höchstens bereitstellen. Wenn die app bekannt ist, dass eine bestimmte Antwort Prozess länger dauern wird, muss zum Senden einer **InProgress** Antwortcode siri. Siri informiert dem Benutzer dann, dass die app die Anforderung im Hintergrund verarbeitet und fortgesetzt wird, auch wenn sie Siri Umgebung verlassen.

## <a name="adding-sirikit-to-the-app"></a>Hinzufügen von SiriKit zur App

Mit SiriKit unter iOS 10 hat Apple zwei neue Erweiterungspunkte erstellt:

- **Intents-Erweiterung** : der Inhalt der app Siri bietet und führt die Aufgaben, die erforderlich sind, um alle unterstützten Absichten zu erfüllen.
- **Intents-Benutzeroberflächenerweiterung** -bietet eine benutzerdefinierte Benutzeroberfläche, die angezeigt wird, für den Inhalt der apps innerhalb von Siri. 

Es gibt auch eine API zum Bereitstellen von Wörtern und Ausdrücken siri zur Unterstützung bei der Erkennung in Form von:

- **App-Vokabular** -Wörter und Ausdrücke, die für jeden Benutzer der app gelten.
- **Benutzer-Vokabular** -Wörter und Ausdrücke, die für einen bestimmten app-Benutzer eindeutig sind.

## <a name="the-intents-extension"></a>Die Intents-Erweiterung

Die Intents-Erweiterung ist verantwortlich für die wichtigsten Interaktionen zwischen der app und Siri wie folgt behandeln:

[![](understanding-sirikit-images/intents01.png "Die Intents-Erweiterung")](understanding-sirikit-images/intents01.png#lightbox)

Die Absicht-Erweiterung kann eine oder mehrere Intents unterstützen, es ist Aufgabe des Entwicklers, entscheiden, wie sie möchten SiriKit in der app zu implementieren. Der Entwickler könnten auch eine separate Intent-Erweiterung für jeden Zweck, die verarbeitet werden müssen hinzufügen.  Dies bedeutet, dass Apple fordert an, dass der Entwickler die Anzahl der Absicht Erweiterungen beschränkt, sodass Siri nicht mehrere Prozesse, die erfordern, mehr Arbeitsspeicher und Zeit verarbeiten, die für die app geöffnet.

Der Entwickler sollte auch bewusst sein, dass die Absicht-Erweiterung im Hintergrund ausgeführt werden, wird während Siri aktiv ist. Dies ermöglicht Siri aktiv für eine Konversation mit dem Benutzer während der Kommunikation weiterhin mit der Erweiterung zum Verarbeiten der Informationen über die Anforderung auszuführen.

## <a name="privacy-and-security-considerations"></a>Datenschutz und Überlegungen zur Sicherheit

Apple hat großartige Maßnahmen, um sicherzustellen, dass einen Benutzer, die private Informationen sicher ist, bei der Verwendung von Siri und es sind mehrere Aktivitäten, die der Benutzer auf iOS-Geräten angemeldet sein, übernommen. Beispielsweise wird beim Anfordern einer Tour oder Zahlungsoptionen.

Darüber hinaus stehen bestimmte Verhaltensweisen, die die app dem wird auf diesem Gerät angemeldeten Benutzer beschränken möchten. In diesen Situationen kann die app Anfordern der **einschränken gesperrten** Verhalten. Dies erfolgt über eine Einstellung in der `Info.plist` Datei.

Die lokalen Authentifizierungs-Frameworks ist verfügbar, für die Ziel-Erweiterung, damit die app den Benutzer Informationen für die zusätzliche Authentifizierung anfordern kann, auch wenn das Gerät bereits entsperrt wird.

Schließlich ist die Apple Pay für die Ziel-Erweiterung zur Verfügung, damit die app eine Transaktion, die Verwendung von Apple Pay abzuschließen und den integrierten Apple Pay-Blatt angezeigt, über die Siri-Schnittstelle kann wird.

Apple darüber hinaus möchte, um sicherzustellen, dass der Benutzer weiß, wenn sie Informationen zu einer 3rd Party-app und daher den Benutzer senden **müssen** angenommen, bei den angegebenen Namen der app (wie in der app-Bundle-Anzeigename angegeben) einer Anforderung.

Wurde entwickelt, Apple Siri um natürliches und Fließendes Konversationen mit dem Benutzer und aus diesem Grund durchzuführen, den Namen der app-Bundle kann in vielen Wortarten, verwendet werden, wo sie sich auf natürliche Weise in die Anforderung des Benutzers passen.

Eine der Probleme, die Benutzer ausführen, werden ist "den Namen der app, verbify" heißt, wobei der Name der app und verwenden ihn als Verb in einer Anforderung. Z. B. *"MonkeyChat Bobo war eine großartige Bananen."*

## <a name="the-intents-ui-extension"></a>Die Intents-Benutzeroberflächenerweiterung

Die Intents-Benutzeroberflächenerweiterung die Möglichkeit, app Benutzeroberfläche und in die Oberfläche Siri branding anzuzeigen, und stellen die Verbindung mit der app können Benutzer. Mit dieser Erweiterung kann die app sowohl die Marke als auch visual und andere Informationen in die Aufzeichnung erhöhen.

[![](understanding-sirikit-images/intents02.png "Beispielausgabe für die Intents-Benutzeroberflächenerweiterung")](understanding-sirikit-images/intents02.png#lightbox)

Die Intents-Benutzeroberflächenerweiterung gibt stets eine `UIViewController` und die app kann alles innerhalb der View-Controller, z. B. zeigt zusätzliche Informationen, die die erste Antwort überschreitet gewünscht hinzufügen. Die Intents-Benutzeroberfläche können Benutzer auch aktualisieren, mit dem Status eines lang andauernden-Ereignisses, z. B. wie lange es, eine Tour, die gemeinsame Nutzung von Autos dauert, um ihren Speicherort zu erreichen.

Die Intents-Benutzeroberflächenerweiterung immer zusammen mit anderem Inhalt Siri, z. B. die app-Symbol und Name am oberen Rand der Benutzeroberfläche angezeigt oder wird, basierend auf der Absicht, Schaltflächen (z. B. Sende- oder "Abbrechen") am unteren Rand angezeigt werden können.

Es gibt einige Fälle, in dem die app die Informationen ersetzen können, die Siri anzeigt, die dem Benutzer standardmäßig wie z. B. messaging oder zugeordnet, in dem die app mit einem maßgeschneidert an die app die standarderfahrung ersetzen können.

> [!IMPORTANT]
> Es ist zwar möglich, interaktive Elemente hinzufügen, wie z. B. `UIButtons` oder `UITextFields` auf der Absicht-Benutzeroberflächenerweiterung `UIViewController`, diesen sind streng verboten, als die beabsichtigte Benutzeroberfläche im nicht interaktiven und der Benutzer ist nicht in der Lage, Sie mit ihnen interagieren.

Es ist völlig optional für die app eine Benutzeroberflächenerweiterung für beabsichtigte Lesevorgänge angeben, da Siri einen Standardsatz der Benutzeroberfläche für jeden beabsichtigten Typ enthält. Darüber hinaus stehen die Intents UI-Schnittstellen nur für bestimmte, dass Intents, dass Apple angesehen wird für den Benutzer hilfreich wäre.

## <a name="adding-sirikit-vocabulary"></a>Hinzufügen von SiriKit-Vokabular

Der letzte Schritt beim Implementieren von SiriKit liegt innerhalb der app durch die Bereitstellung der erforderlichen Vokabular. Viele Apps besitzen eindeutige Möglichkeiten zur Beschreibung der Informationen für den Benutzer und dem eindeutigen Möglichkeiten, dass der Benutzer Informationen an die app bereitstellt.

Aus diesem Grund erfordert Siri der app Unterstützung zu erhalten, die Wörter und Ausdrücke, die eindeutig für die app zu verstehen. Einige der folgenden Meldungen wird Teil der app, sodass alle Benutzer kennen und sie verstehen. Andere werden noch für einen bestimmten Benutzer der app eindeutig sein.

### <a name="app-specific-vocabulary"></a>App-spezifische Vokabular

Der App-spezifische Programmierterminologie definiert die bestimmte Wörter und Ausdrücke an, die bekannt sind, an alle Benutzer der app, wie z. B. Fahrzeugtypen oder Trainings-Namen. Da diese Teil der Anwendung sind, werden sie in definiert eine `AppIntentVocabulary.plist` -Datei als Teil des Haupt-app-Pakets. Darüber hinaus sollte diese Wörter und Ausdrücke lokalisiert werden.

Es gibt mehrere Teile eines Vokabulars `AppIntentVocabulary.plist` Datei:

- **Beispiel-App verwendet** – diese bieten eine Reihe von gängigen Anwendungsfällen, für die Anforderungen, die der Benutzer der app vornehmen können. Zum Beispiel: *"Thema mit MonkeyFit starten".*
- **Parameter** – diese bieten eine Reihe von nicht standardmäßigen Parametertypen, die speziell auf die app. Z. B. Trainings Namen für die MonkeyFit-app. Diese bestehen aus:
    - **Ausdruck** -ermöglicht der app, die eindeutigen Begriffe für die app zu definieren. Zum Beispiel: der Typ "Bananarific" Trainings, für die MonkeyFit-app. 
    - **Aussprache** -Aussprache Hinweise siri als eine einfache lautrechtschreibung nach einem bestimmten Ausdruck zu erhalten. Beispiel: "Ba Nana ri Einzelschritt".
    - **Beispiel** -enthält ein Beispiel für die Verwendung des angegebenen Ausdrucks in der app. Z. B. *"Starten Sie ein Bananarific in MonkeyFit"*.

Weitere Informationen finden Sie unter Apple [App Vokabular Datei Formatreferenz für Blobüberwachungsprotokolle](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Bestimmte Benutzer-Vokabular

Benutzer spezifische Programmierterminologie wird Geben Sie Wörter oder Ausdrücke, die für einzelne Benutzer der app eindeutig sind. Diese werden zur Laufzeit aus der Haupt-app (nicht der App-Erweiterungen) als einen geordneten Satz von Bedingungen, die in einer wichtigsten Nutzung Priorität für die Benutzer mit den wichtigsten Begriffen am Anfang der Liste sortiert angegeben werden.

Sehen Sie sich das Beispiel für die oben aufgeführten MonkeyChat-app. MonkeyChat behält es sich um eine Liste aller Kontakte des Benutzers, die sie siri über spezifische Programmierterminologie Benutzer gesendet wird. Es führt auch eine Liste von 10 letzten Kontakten, die der Benutzer Nachrichten hat, und eine Reihe von Favoriten-Kontakte für jeden Benutzer ist. In diesem Beispiel sollte die bevorzugten Kontakte zu Beginn unserer Benutzer spezifische Programmierterminologie, gefolgt von der letzten Kontakten klicken Sie dann die restlichen Kontakte des Benutzers sein.

Die folgenden Arten von Informationen werden vom Benutzer spezifische Programmierterminologie unterstützt:

- Wenden Sie sich an Namen.
- Trainings-Namen.
- Fotoalbum Namen.
- Foto-Schlüsselwörter.

Wenn die app auf dem iOS-Adressbuch basiert, wird die app keine Aktion ausführen, da diese Informationen bereits siri verfügbar sind. Die app benötigt nur Kontaktnamen bereitstellen, verfügt die app einen eigenen eindeutigen Datenbanknamen von Kontakten.

Geben Sie beim Entwerfen des Vokabulars nur die erforderlichen Werte, die die Benutzer kennen und kümmern. Vermeiden Sie die Bereitstellung von Informationen wie z.B. Telefonnummern oder e-Mail-Adressen.

Die app muss außerdem Siri sofort zu aktualisieren, wenn Benutzer spezifische programmierterminologie ändert. Benutzer zum Anfordern von Informationen von Siri den Zeitpunkt daran gewöhnt sind, die sie für ihre iOS-Gerät hinzugefügt wurde. Z. B. wenn der Benutzer einen neuen Kontakt in der app hinzufügt, sendet diese Informationen siri, sobald der Benutzer speichert.

Noch wichtiger ist, dass die app _müssen_ Löschen von Daten aus dem Vokabular Siri sofort, da ein Benutzer verärgern werden konnte, wenn sie eine Information gelöscht, aber Siri noch es Stunden oder Tage später erkennt wurde.

> [!IMPORTANT]
> Die app sollte entfernt sämtliche Benutzer spezifische Programmierterminologie Siri, wenn der Benutzer zum Zurücksetzen der app auswählt oder wenn sie abmelden.

## <a name="sirikit-permissions"></a>SiriKit-Berechtigungen

Das letzte Stück von SiriKit basiert auf Berechtigungen. Vergleichbar mit dem andere Funktionen der e/a (z. B. Fotos, Kamera oder Kontakte) verwenden, müssen Benutzer die explizite Berechtigung für die Kommunikation mit Siri-app zu gewähren.

Die app ist eine Zeichenfolge, welche Informationen sie siri bieten und geben Sie einen Grund, warum der Benutzer diesen Zugriff zu gewähren sollten bereitstellen. 

Apple empfiehlt, dass die app vom Benutzer Siri das erste Mal verwenden, der Benutzer die app wird geöffnet, nachdem sie auf iOS 10 aktualisiert haben, Berechtigung anfordern muss. Dies ist, damit Benutzer, über die Integration von Siri wissen und vorab genehmigte Verwendung können, bevor sie mit die erste Anforderung vornehmen.

## <a name="sirikit-and-maps"></a>SiriKit und Zuordnungen

SiriKit ist ein integraler Bestandteil der e/a und nutzt die größeren Intents-Framework, das auf iOS 10 hinzugefügt. Die Intents-Framework soll allgemeiner und gemeinsam genutzter Aktionen und Intents zusammen mit anderen Teilen des Systems nutzen.

Die Intents-Framework nur Siri Integration hinausgeht und bietet andere Funktionen wie z. B. Kontakte-Integration, in dem die app die Standard-Telefonie oder die messaging-app für bestimmte Kontakte werden kann. Intent-Elemente bieten auch nahtlose Integration in CallKit VOIP am besten mögliche Benutzern bereitstellen.

Die Karten-app unter iOS 10 wurde hinzugefügt, Funktionen, z. B. der Fahrt Freigabe, in denen der Benutzer auf eine Tour direkt in der Maps-Benutzeroberfläche buchen können. SiriKit bietet einen allgemeinen Erweiterungspunkt mit Karten aus, sodass Fahrt freigeben (und andere) Intents Siri und Zuordnungen gemeinsam genutzt werden können. 

Dies bedeutet, dass die app die SiriKit Erweiterungen beschlossen, auch die Integration von Maps kostenlos abgerufen werden sollen. 

## <a name="designing-a-great-siri-experience"></a>Entwerfen einer überzeugenden Umgebung für Siri

Ein optimales Benutzererlebnis entwerfen, wenn Sie eine app in Siri integrieren unterscheidet sich das Entwerfen einer tollen app-Benutzeroberfläche zur Verfügung. Im Gegensatz zu normalen Situationen, in denen der Benutzer interagiert mit der app, die direkt auf, Bildschirm, wenn es mithilfe von Siri sind oft Wenn keine Benutzeroberfläche sichtbar ist. Z. B. wenn der Benutzer gestarteten die Konversation mit *"Hey Siri"*.

### <a name="how-siri-helps-the-developer"></a>Wie Siri dabei hilft, den Entwickler

Beim Entwerfen einer app-Interaktionen mit Siri wird die app erstellen eine *Gesprächsschnittstelle*, d. h. des Kontexts wird abgeleitet von der Konversation, die von Siri mit dem Benutzer in der app-Namen hat.

In Ermangelung als visuelle Referenz der Benutzer muss die Informationen in ihren Köpfen Anzeige von verfolgt. Aus diesem Grund stellt Siri den bare mindestens erforderlichen Informationen für die Aufgabe, dass der Benutzer wird zum ausführen möchte.

Die Gesprächsschnittstelle wird während der Konversation aus der Benutzer- und Siri von Fragen und Antworten für den strukturiert. Daher ist es wichtig, zu überlegen, wie Siri Fragen stellt und reagiert, wenn diese Schnittstelle zu entwerfen.

Im folgenden Beispiel für den Benutzer erstellt eine Nachricht Siri reagieren möglicherweise mit der Frage, *"Zum Senden bereit?"* . Der Benutzer kann auf viele verschiedene Arten reagieren, wie z. B. *"Senden"*, *"Abbrechen"* oder sogar etwas völlig auf diese Frage nicht verbunden. Unabhängig davon, wie die Konversation funktioniert wird Siri für die app behandeln, und nur senden sie die relevante Informationen sobald diese verfügbar werden.

Es gibt verschiedene Möglichkeiten, ein Benutzer eine Konversation mit Siri initiieren kann:

- Durch Auswählen des Geräts ein, drücken die Schaltfläche "Start". In diesem Fall werden Siri Weitere visuellen Schnittstellen und weniger mündlichen Antworten angezeigt.
- Mit den Worten *"Hey Siri"* und Starten einer Konversation freie Hand. In diesem Fall werden Siri weniger visual und mehr mündliche.
- Verwenden von Barrierefreiheitsfunktionen z. B. Bluetooth aktiviert hören Hilfsmittel, in denen die Benutzeroberfläche für einen Benutzer mit speziellen Anforderungen zugeschnitten ist.
- Mithilfe von Autos wiedergeben, in denen der Benutzer benötigt ihre Aufmerksamkeit zu, konzentriert sich einbringen ablenkungen auf ein Minimum zu halten.

### <a name="how-the-developer-helps-siri"></a>Wie kann Entwickler für Siri

Wenn Sie eine app mit Siri integrieren zu können, muss der Entwickler oft diese Integration zu testen und stellen Sie sicher, dass sie viele verschiedene Anforderungen werden aufgefordert, für denselben Speicherbereich Informationen "oder" Task in so viele unterschiedliche Weisen wie möglich vornehmen.

Da nicht zwei Personen, die gleichermaßen denken, ist es wichtig, dass so viele verschiedene Betatester möglichst optimieren können die Integration von Siri erhält. Benutzer können Informationen anfordern oder Anforderungen in die Möglichkeiten des Entwicklers nie jedoch der und diese Feinabstimmung helfen sicherzustellen, dass die umfangreichste Gruppe von Benutzern eine überzeugenden Umgebung, die mit ihrer app mit Siri.

In verschiedenen Situationen und Umgebungen testen. Die Konversationen mit Siri in allen wie möglich, stellen Sie sicher, dass diese Konversationen fließend bleiben und natürliche zu initiieren. Testen Sie in die Speicherorte, in denen der Benutzer sehr wahrscheinlich die app, wie in einem Rechenzentrum weiter aufzurüsten Fitnessstudios verwenden würde.

Stellen Sie sicher, dass die app alle Informationen bereitstellt, dass Siri ordnungsgemäß die Anforderung und das Ergebnis für den Benutzer darstellt muss. Dies gilt insbesondere bei Verwendung von Siri in eine praktische kostenlose Situation.

### <a name="siri-design-guidelines"></a>Siri-Entwurfsrichtlinien

Denken Sie immer daran, dass Siri im Auftrag von der app eine Konversation mit der Benutzer hat. Der Entwickler kann nicht sicher, dass diese Konversation bleibt, wie fließende und natürlich wie möglich ist.

Wie bei jeder wichtige Konversation auch, muss der Entwickler, um Folgendes sicherzustellen:

- Die app für die Konversation vorbereitet ist.
- Dass die app lauscht, genau wie der Benutzer zu erreichen versucht.
- Dass die app die entsprechenden Fragen zu den entsprechenden Zeitpunkten fordert.
- Dass die app auf die Anforderung mit den Informationen, die der Benutzer gesucht wird reagiert.

#### <a name="preparing-for-the-conversation"></a>Vorbereiten für die Konversation

Die erste, was zu beachten ist, dass die Benutzer der app genau wie der Entwickler sind nicht. Sie können die von verschiedenen Hintergründen, unterschiedliche Sprachen sprechen oder besondere Anforderungen haben, bei der Arbeit mit der app stammen.

Darüber hinaus, da der Entwickler entworfen und die app, verfügen sie tiefgreifende, sehr gute Kenntnisse über die app und die internen Abläufe und Funktionen, die ein typischer Benutzer keinen hat. Daher kann der Entwickler Anforderung von Siri anders als ein normaler Benutzer bitten.

Daher ist es ist wichtig, wie viele verschiedene Personen wie möglich mit der app über Siri interagieren. Benutzer können Anforderungen an die app über Siri vornehmen, die der Entwickler nie mehr Gedanken hat, oder in Methoden, die der Entwickler nicht berücksichtigt hat.

#### <a name="ensure-the-app-is-a-good-listener"></a>Stellen Sie sicher, dass die App befindet sich eine gute Listener

Der Entwickler muss sicherstellen, dass die app ein guter Listener ist und erhält die Einzelheiten der Konversation, die der Benutzer die Erwartungen zu erfüllen. Aber es ist auch möglich, dass sie nicht alle Informationen eingegeben haben könnte, dass der app erforderlich sind, um die angeforderte Aufgabe zu erzielen.

Es gibt mehrere Möglichkeiten, die app diese Situation behandeln kann:

- **Wählen Sie ein geeigneter Standard für den Missing Value** – beispielsweise eine Fahrt Freigabe-app kann auf aktuellen Standort des Benutzers standardmäßig, wenn sie nicht angegeben haben, in dem sie von abgeholt werden wollten.
- **Stellen Sie eine wohl begründete Vermutung** -verwenden spezielle Informationen, die für den Benutzer die app gesammelt wurden, die app möglicherweise Hersteller und das wohl begründete Vermutung anzustellen die fehlenden Informationen, z. B. eine fehlende Mobiltelefonnummer von Kontaktinformationen des Benutzers eingeben. Allerdings sollte darauf geachtet werden, z. B. auswählen, die teuerste Option usw., bösen überraschungen zu vermeiden.
- **Weitere Informationen auffordern** -die app kann Siri benutzeraufforderung für den fehlenden Wert verfügen. Der Schlüssel ist jedoch die Konversationen einfach und präzise beibehalten. Benutzer werden schnell frustriert, wenn sie mehrere Fragen zum Erreichen ihrer Anforderung haben.
- **Missverständnisse ordnungsgemäß behandeln** – der Benutzer kann einen Wert, der die app wurde nicht erwartet oder, die es im angegebenen Kontext verarbeiten kann nicht, angeben. Stellen Sie sicher, dass die app diese Situation für den Benutzer auf eine Weise verknüpft, die es einfach zu verstehen und zu korrigieren ist.

Wenn die app mit einem Wert, die zweifelhaft ist angezeigt wird, ist die bevorzugte Methode, dies zu verarbeiten, haben Sie Siri, die der Benutzer zur Bestätigung aufgefordert. Z. B. *"Meinten Sie Bobo großartigen?"* , die sie mit der eine einfache Ja oder Nein-Antwort Antworten können.

Wenn es eine Situation ist, in denen mehrere mögliche Optionen für einen einzelnen Wert handeln, ist zur mehrdeutigkeitsvermeidung die bevorzugte Behandlungsmethode. In diesem Fall kann Siri den Benutzer mit bis zu zehn möglichen Optionen zur Auswahl aufgefordert. Zum Beispiel:

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Falls Sie sich noch in Frage, haben Sie Siri, die den Benutzer auffordern, eine völlig neue und spezifischere-Antwort für einen bestimmten Wert angeben.

#### <a name="request-final-confirmation"></a>Abschließende Bestätigung anfordern

Bevor die app tatsächlich die Aufgabe zum Erfüllen der Anforderung des Benutzers ausführt, prüft Siri mit der App-Erweiterung, um sicherzustellen, dass alles vorhanden ist. Beispielsweise besitzt der Benutzer ausreichende Geldmittel in ihrem Konto für die Zahlung für die angeforderte?

Darüber hinaus muss die app, um sicherzustellen, dass er alle möglichen Informationen siri bereit ist, damit sie für den Benutzer stellt und bestätigen Sie, dass die Aufgabe ausgeführt werden, ihre Erwartungen erfüllt werden können.

Sobald der Benutzer, die Anforderung und die app bestätigt hat wurde ausgeführt, muss der app erneut stellen Sie sicher, dass er alle Ergebnisse zurück an Siri bereitgestellt hat, damit es dem Benutzer verknüpft sein kann.

#### <a name="responding-to-the-request"></a>Reagieren auf die Anforderung

Siri enthält mehrere integrierte Benutzeroberflächen, für jede der Domänen und der Aktionen, die er kennt. Allerdings kann gegebenenfalls die app um die benutzerfreundlichkeit zu bereichern, durch die Bereitstellung der app branding und die Benutzeroberfläche oder die mehr Informationen als in der Anforderung wurde eine benutzerdefinierte Erweiterung auf der Absicht-Benutzeroberfläche bereitstellen.

Allerdings zeigen beim Entwerfen von benutzerdefinierter Schnittstellen für Siri verwendet werden soll. In der Regel wird der Benutzer wird eine bestimmte Aufgabe muss so schnell wie möglich erhalten möchten, und nicht mit unnötigen Informationen überladen werden soll.

Außerdem sollte vorsichtig vorgenommen werden, um sicherzustellen, dass die benutzerdefinierte Benutzeroberfläche das Aussehen und Verhalten sich ordnungsgemäß in alle anderen iOS-Geräte und Ausrichtungen an, dass der Benutzer möglicherweise oder das Gerät verwenden.

Falls angebracht, verwenden Sie die SiriKit-API, alle redundanten Informationen, die bereits in der standardmäßigen Siri UI ausgeblendet. Noch stellen Sie sicher, dass die app die Informationen noch siri bereitstellt, damit er es verbal in eine praktische kostenlose Situationen darstellen kann.

Es gibt möglicherweise Situationen, in denen Siri starten wird der app zum Erfüllen der Anforderung des Benutzers, z. B. präsentieren die Fotos, die der Benutzer angefordert hat. In diesen Situationen sollte überraschen Sie nicht den Benutzer werden. Zeigen Sie die erwarteten Informationen ohne Zwischenschritte oder weitere Interaktion erforderlich. Nie zeigen Sie Informationen an, oder führen Sie eine Aufgabe, die nicht der Benutzer erwartet, dass.

### <a name="polishing-the-design"></a>Optimieren des Entwurfs

Es gibt mehrere Schritte, die von Apple vorgeschlagen, um den Entwurf der Gesprächsschnittstellen polish. Erstens ist durch die Bereitstellung der klare und kompakte Vokabulars und Verwendung Beispiele für Anwendungsfälle für siri.

Eine der Möglichkeiten, dass ein Benutzer die app ermittelt wird durch Initiieren einer Konversations mit Siri und aufzufordern, *"Was können Sie tun?"* Siri zeigt verschiedene Dinge können erfolgen, einschließlich des Entwicklers-app und die Beispiel-Hero-Anwendungsfälle, die sie über bereitgestellt hat seine `plist` Datei.

So schreiben Sie gute Beispiele für Anwendungsfälle:

- Stellen Sie sicher, dass die Beispiele umfassen die Namen der app.
- Lassen Sie das Beispiel kurz und auf den Punkt an.
- Geben Sie mehrere Beispiele für jede der Intent-Elemente, die die app unterstützt.
- Priorisieren Sie sowohl die Beispiele in basierend auf der die häufigsten Anwendungsfälle für die app als auch der Intent-Elemente.
- Stellen Sie sicher, dass die app lokalisierte Beispiele bietet.
- Stellen Sie sicher, dass jedes Beispiel funktioniert wie erwartet in der app.
- Vermeiden Sie die Adressierung von Siri in den Beispielen wird daher Text wie enthalten nicht *"Hey Siri..."*
- Vermeiden Sie z. B. alle unnötigen Pleasantries *"Sie"* oder *"Danke"*.

Nehmen Sie die entsprechende Zeit, ausprobieren und experimentieren auf wie die app die Konversation strukturieren, die von Siri mit dem Benutzer in ihrem Namen hat. Achten Sie darauf, sprechen Sie mit typischen Benutzern während des Prozesses, als deren Interaktionen mit und Erwartungen der app können sich im Laufe der Zeit ändern.

Denken Sie immer daran, zum Testen der app in verschiedenen Situationen und all die verschiedenen Methoden, um eine Konversation mit Siri aufzurufen. Test in der realen Welt Speicherorte des Benutzers möglicherweise verwenden Sie die app von der Office und den Helpdesk.

Bemühen uns um die Konversationen mit Siri (im Auftrag einer app) Fluid, natürlichen und "genau das richtige fühlen" sein. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die grundlegenden Konzepte für die Verwendung von SiriKit erforderlich und gezeigt, dass sie interagieren kann, mit dem Xamarin.iOS-apps, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät zugänglich sind, behandelt.




## <a name="related-links"></a>Verwandte Links

- [ElizaChat-Beispiel](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Frameworkverweis Intents](https://developer.apple.com/reference/intents)
- [Referenz für Intents UI-Framework](https://developer.apple.com/reference/intentsui)
