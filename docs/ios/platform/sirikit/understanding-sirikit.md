---
title: Grundlegendes zu Konzepten SiriKit
description: "Dieser Artikel behandelt die grundlegenden Konzepte, die für das Arbeiten mit SiriKit in einer app Xamarin.iOS benötigt werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 202df615f1b35504f1fe5c9fd64c9c4b4db77a2d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="understanding-sirikit-concepts"></a>Grundlegendes zu Konzepten SiriKit

_Dieser Artikel behandelt die grundlegenden Konzepte, die für das Arbeiten mit SiriKit in einer app Xamarin.iOS benötigt werden._


Neue iOS 10, SiriKit ermöglicht eine Xamarin.iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Maps-app auf einem iOS-Gerät zugänglich sind. Diese Funktion finden Sie im App-Erweiterung für eine oder mehrere mit dem neuen **Intents** und **Intents UI** Frameworks.

SiriKit ermöglicht eine iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Maps-app auf einem iOS-Gerät mithilfe von App-Erweiterungen und die neue zugänglich sind **Intents** und **Intents UI** Frameworks.

Siri funktioniert mit dem Konzept der **Domänen**, Gruppen von wissen Aktionen für verwandte Aufgaben. Jeder Interaktion mit die app mit Siri muss wie folgt in einen bekannten Dienst Domäne fallen:

- Audio oder video aufrufen.
- Buchung eine fuhr an.
- Verwalten von Training.
- Messaging.
- Suchen Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung Siri im Zusammenhang mit einer der Dienste für die App-Erweiterung stellt, sendet SiriKit die Erweiterung ein **Absicht** -Objekt, das der Benutzer-Anforderung sowie alle unterstützungsdaten beschreiben. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Details wie die Erweiterung für die Anforderung verarbeiten kann.

## <a name="the-intents-and-intents-ui-extensions"></a>Die Intents und Intents UI-Erweiterungen

Siri und die Zuordnungen app interagieren mit der app-Dienste über zwei verschiedene Arten von App-Erweiterungen:

- **Intents Erweiterung** -Siri bietet und Karten mit der app den Inhalt und führt die erforderlichen Aufgaben zum erfüllen alle unterstützten Intents.
- **Intents Benutzeroberflächenerweiterung** -bietet eine benutzerdefinierte Benutzeroberfläche, die angezeigt werden, für die app-Inhalte innerhalb der Siri oder Zuordnungen.

Die app muss eine Intents-Erweiterung zur Unterstützung von SiriKit angeben, und er ist verantwortlich für das Bereitstellen von Informationen, die Siri und Zuordnungen für dem Benutzer angezeigt werden können und für die Behandlung von Intents.

Erstellen einer Intents Benutzeroberflächenerweiterung ist optional, da Siri in der Regel alle Benutzerinteraktionen behandelt, und eine standardmäßige, integrierte Benutzeroberfläche zum Darstellen von Informationen in den einzelnen unterstützten Domänen verfügt. Durch die Bereitstellung einer Benutzeroberflächenerweiterung Intents, können die app die **Absicht UI** Framework ein umfassendes, benutzerdefinierte Benutzeroberfläche mit die app branding präsentieren und zusätzliche Informationen.

## <a name="siri-and-the-maps-app-role"></a>Siri und die Maps-App-Rolle

Der gesprochenen benutzeranforderungen sind Sprache verarbeitet und analysiert semantisch von Siri auf die Anforderungen in aussagefähige Intents deaktiviert wird, der die Absicht Erweiterungen verarbeiten kann.

Maps verwendet der app Absicht Erweiterungen, um Informationen in der Map-Schnittstelle als Antwort auf Aktionen des Benutzers anzuzeigen. Prüft z. B. in der Nähe Restaurants anfordern oder für das Abrufen der app Restaurant.

Siri und Karten alle Interaktionen des Benutzers zu verwalten und mithilfe der Benutzeroberfläche Standardbetriebssystem Ergebnisse anzeigen. Die app-Erweiterungen-Rolle ist in erster Linie um die Daten bereitzustellen, die angezeigt werden. Optional kann die app bieten eine Benutzeroberflächenerweiterung Intents und eine benutzerdefinierte Benutzeroberfläche zur Verbesserung der Benutzeroberfläche von System.

## <a name="interacting-with-siri-via-sirikit"></a>Siri über SiriKit interagieren

Dieser Abschnitt bietet einen Überblick darüber, wie SiriKit dem Benutzer die Interaktion mit der app mithilfe von Siri ermöglicht. Für dieses Beispiel müssen die gefälschte MonkeyChat-app verwendet werden:

[![](understanding-sirikit-images/monkeychat01.png "Das Symbol "MonkeyChat"")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat behält eine eigene Adressbuch des Benutzers Freunde, jeweils eines Anzeigenamens (z. B. Bobo z. B.) zugeordnet ist, und ermöglicht es dem Benutzer nach dem Bildschirmnamen jeder Freund Chats Text an.

Es gibt viele Möglichkeiten, dass der Benutzer möglicherweise eine Interaktion mit der app initiieren, da verschiedene Personen dieselbe Anforderung in vielen verschiedenen Formen vorgenommen werden können.

Beispielsweise, wenn der Benutzer eine Nachricht an ihre "Friend" Bobo senden möchten, haben sie möglicherweise die folgenden Konversation mit Siri:

<table width="100%" border="1px">
<tr>
    <td width="50%"><b>Siri</b></td>
    <td width="50%"><b>User</b></td>
</tr>
<tr>
    <td></td>
    <td>"Hey Siri Senden einer Nachricht MonkeyChat"</td>
</tr>
<tr>
    <td>", Denen?"</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>"Bobo"</td>
</tr>
<tr>
    <td>"Was möchten Sie reden, Bobo?"</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>"Stellen senden Sie weitere Bananen"</td>
</tr>
</table>

Eine andere Person möglicherweise dieselbe Anforderung mit einer anderen Konversation stellen:

<table width="100%" border="1px">
<tr>
    <td width="50%"><b>Siri</b></td>
    <td width="50%"><b>User</b></td>
</tr>
<tr>
    <td></td>
    <td>"Senden Sie eine Nachricht an Bobo auf MonkeyChat"</td>
</tr>
<tr>
    <td>"Was möchten Sie reden, Bobo?"</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>"Stellen senden Sie weitere Bananen"</td>
</tr>
</table>

Und ein anderer Benutzer kann eine noch kürzere anfordern:

<table width="100%" border="1px">
<tr>
    <td width="50%"><b>Siri</b></td>
    <td width="50%"><b>User</b></td>
</tr>
<tr>
    <td></td>
    <td>"MonkeyChat Bobo senden Sie weitere Bananen"</td>
</tr>
<tr>
    <td>"Nun senden Nachricht schicken Sie weitere Bananen Bobo auf Monkeychat"</td>
    <td></td>
</tr>
</table>

Oder sogar dieselbe Anforderung in einer anderen Sprache:

<table width="100%" border="1px">
<tr>
    <td width="50%"><b>Siri</b></td>
    <td width="50%"><b>User</b></td>
</tr>
<tr>
    <td></td>
    <td>"MonkeyChat Bobo s'il Vous Plaît Envoyer plus de Bananes"</td>
</tr>
<tr>
    <td>"Oui, Envoi Nachricht s'il Vous Plaît Envoyer plus de Bananes À Bobo Sur Monkeychat"</td>
    <td></td>
</tr>
</table>

Ein anderer Benutzer möglicherweise noch sehr ausführliche in ihrer Konversation werden:

<table width="100%" border="1px">
<tr>
    <td width="50%"><b>Siri</b></td>
    <td width="50%"><b>User</b></td>
</tr>
<tr>
    <td></td>
    <td>"Hey Siri, können Sie bitte mir einer kompakten und starten Sie die app MonkeyChat senden ein Text mit der Nachricht senden Sie weitere Bananen"</td>
</tr>
<tr>
    <td>", Denen?"</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>"Bobo über Mein bewährte Pal"</td>
</tr>
</table>

Darüber hinaus sind viele Methoden, die Siri reagieren kann, auf eine Anforderung, einige basierend auf wie die Anforderung gestellt wurde:

- **Halten Sie die Schaltfläche "Start"** -Siri stellt weitere visual Antworten mit begrenzten verbale Feedback bereit.
- **Von "Hey Siri"** -Siri werden mehr verbale und weniger visual Antworten.

Siri ist auch optimiert, um die Bedürfnisse des Benutzers zu erfüllen und interagiert, und basierend auf diesen Anforderungen zu reagieren.

Unabhängig davon, wie eine Anforderung gestellt wird, oder wie Siri auf die Anforderung reagiert Siri behandelt die Konversation mit dem Benutzer, und die Anwendung (über ihre Erweiterungen) stellt die Funktionalität bereit.

Wenn der Benutzer eine verbale Anforderung der Siri sendet, sind dies die Schritte, die Siri folgen wird:

[![](understanding-sirikit-images/monkeychat02.png "Die folgenden Schritte Siri wird")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Siri nimmt zunächst die Audiodaten des Benutzers **Spracherkennung** und in Text konvertiert.
2. Als Nächstes wird der Text in konvertiert ein **Absicht**, wird eine strukturierte Darstellung der Anforderung des Benutzers.
3. Basierend auf der Absicht, ist Siri dauert **Aktion** die Anforderung des Benutzers ausführen.
4. Schließlich bietet Siri **Antworten** (visual und verbale) für den Benutzer basierend auf der die ausgeführte Aktion.

Es gibt drei Hauptmethoden, die die app des Benutzers Konversation mit Siri teilnehmen kann:

[![](understanding-sirikit-images/monkeychat03.png "Die drei Hauptmethoden, dass die app in der Konversation Benutzer mit Siri teilnehmen kann")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Vokabular** -Dies ist die app Siri die Wörter mitteilen, wie sie wissen, um damit zu interagieren muss.
2. **App-Logik** – Hierbei handelt es sich um die Aktionen und Antworten, mit denen die app gelangen auf Basis der angegebenen Intents.
3. **Benutzeroberfläche** -Dies ist die optionalen, benutzerdefinierten Benutzeroberfläche, mit denen die app ihre Antworten in erhalten kann.

### <a name="example"></a>Beispiel

Überprüfen Sie angesichts der oben angegebenen Informationen, wie die folgenden Konversation mit der app MonkeyChat interagieren würden:

<table width="100%" border="1px">
<tr>
    <td width="50%"><b>Siri</b></td>
    <td width="50%"><b>User</b></td>
</tr>
<tr>
    <td></td>
    <td>"Hey Siri Senden einer Nachricht an Bobo auf MonkeyChat"</td>
</tr>
<tr>
    <td>"Was möchten Sie reden, Bobo?"</td>
    <td></td>
</tr>
<tr>
    <td></td>
    <td>"Stellen senden Sie weitere Bananen"</td>
</tr>
</table>

Die erste Rolle, die die app in der Konversation akzeptiert wird Siri Sprache des Benutzers verstehen zu helfen:

[![](understanding-sirikit-images/monkeychat04.png "Helfen Siri verstehen die Benutzer-Sprache")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri verfügt nicht über den Namen "Bobo" in der Datenbank, aber die app ist und hat diese Informationen mit Siri über des Vokabulars freigegeben. Die app hilft außerdem beim erkennen, dass Bobo ein Empfänger ist, da sie zu Siri als angegeben Siri eine *Kontakt*.

Siri weiß, dass mehrere ist erforderlich, um eine Nachricht als nur einen Empfänger zu senden, damit sie schnell mit der App-Erweiterung überprüfen wird, um festzustellen, ob eine Nachricht Inhalt erforderlich ist. Da MonkeyChat der Fall ist, werden Siri für den Benutzer mit der Frage Antworten: *"Was möchten Sie reden, Bobo?"*

Im obigen Beispiel hat der Benutzer geantwortet, *"Senden Sie weitere Bananen"*, die Siri in eine strukturierte bündeln wird **Absicht**:

[![](understanding-sirikit-images/monkeychat05.png "Siri wird die Antwort des Benutzers in einem strukturierten Absicht bündeln.")](understanding-sirikit-images/monkeychat05.png#lightbox)

Die strukturierte Absicht enthält die folgende Informationen:

- **Domäne:** Nachrichten
- **Zweck:** SendMessage
- **Empfänger:** Bobo
- **Inhalt:** Weitere Bananen senden

Jede Domäne verfügt, als Satz von wissen *Aktionen* , kann darin ausgeführt und basierend auf der Domäne und die Aktion, wird 0 (null), die möglicherweise zu viele Parameter, in die Absicht enthalten sein an die Anwendung gesendet.

Die Absicht ist an die App-Erweiterung für Verarbeitung gesendet. Als Ergebnis der Verarbeitung der Absicht, die app generiert eine **IntentResponse** der wird mit der Absicht gebündelt werden, und schließen Sie Parameter, die beschreiben, was die app-app mit der Absicht geschehen ist.

Jede IntentResponse gehören auch eine **Antwortcode** wodurch Siri angewiesen wird, wenn die app die Anforderung nicht oder nicht abschließen konnte. Einige Domänen wurden sehr spezifischen Fehlercodes, die gleichzeitig gesendet werden können.

Schließlich enthält der IntentResponse eine `NSUserActivity` (z. B. solche zur Unterstützung von Handoff verwendet). Die `NSUserActivity` wird verwendet, um die app zu starten, wenn die Antwort werden benötigt, lassen die Umgebung Siri und geben die app aus, um sie abzuschließen. 

Siri wird automatisch ein geeignetes erstellen `NSUserActivity` zum Starten der app, und Pickup unterbrochen, in denen der Benutzer wurde in der Umgebung Siri. Die app kann jedoch bereitstellen, eine eigene `NSUserActivity` mit angepasst Informationen, wenn es erforderlich ist.

Nachdem die app verarbeitet die Absicht und eine Antwort auf Siri zurückgegeben hat, stellt er dann die Ergebnisse dem Benutzer (Alternativtext und visuell):

[![](understanding-sirikit-images/monkeychat06.png "Die Ergebnisse, die dem Benutzer sowohl Alternativtext und visuell angezeigt")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri enthält mehrere integrierte Antwort von Benutzeroberflächen für jede der Domänen für die app verfügbar. Da MonkeyChat eine optionale Erweiterung des Intent-Benutzeroberfläche bereitgestellt hat, ist es jedoch verwendet die Ergebnisse der Konversation für den Benutzer im obigen Beispiel anzuzeigen.

## <a name="the-intent-lifecycle"></a>Der beabsichtigte Lebenszyklus

Es gibt drei Hauptaufgaben, die die App-Erweiterung beim Umgang mit Intents ausführen müssen:

[![](understanding-sirikit-images/monkeychat07.png "Der beabsichtigte Lebenszyklus")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. Die app muss **beheben** jeder Parameter für ein Ereignis. Die app aufrufen folglich Resolve mehrmals (einmal pro jeden Parameter), und manchmal mehrere Male auf den gleichen Parameter erst die app und der Benutzer den gleichen was angefordert wird.
2. Die app muss **bestätigen** , die angeforderte Absicht behandeln und das erwartete Ergebnis Siri erzählen werden können.
3. Die app schließlich muss **behandeln** die Absicht, und führen Sie die Schritte, um das gewünschte Ergebnis zu erzielen.

### <a name="the-resolve-stage"></a>Die Resolve-Phase

Die Resolve-Phase wird Siri die Werte zu verstehen, die der Benutzer bereitgestellt hat, und stellt sicher, dass das, was der Benutzer tatsächlich vorgesehen ist, was passiert, wenn der Zweck von der Anwendung verarbeitet wird. 

Diese Stufe bietet auch die Möglichkeit für die app, die während der Konversation mit dem Benutzer Siris Verhalten beeinflussen. Zu diesem Zweck die app bietet ein **Auflösung Antwort**. Es gibt eine Anzahl vordefinierter als Antwort auf die verschiedenen Datentypen, die Siri versteht.

Die am häufigsten verwendeten Auflösung Antwort von der app werden **Erfolg**, d. h. der app zugeordnet die bestimmten Daten aus einem Parameter (z. B. Benutzer Bildschirmname) eine Information über bekannt ist.

Es gibt möglicherweise Zeiten, wenn die app benötigt, um sicherzustellen, dass eine bestimmte Anforderung die richtigen Information übereinstimmt, die bekannten. In diesen Fällen Traffic Manager sendet einen **ConfirmationRequired** als Antwort auf "Ja" oder "no" an den Benutzer z. B. Fragen *"Send message zu Bobo einzigartigen?"*

Es gibt möglicherweise andere Fälle, in denen die app den Benutzer eine kurze Liste der Optionen ausgewählt benötigen. In diesem Fall wird die app Bereitstellen einer **Mehrdeutigkeitsvermeidung** Antwort mit einer Liste von zwei bis zehn Optionen für den Benutzer z. B. auswählen: 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri werden verarbeitet, die des Benutzers die Auswahl vornehmen, Alternativtext oder durch Interaktion mit der Siri UI und das Ergebnis wird an die app gesendet werden.

In anderen Fällen gibt es möglicherweise nicht genügend Informationen für die app zum Auflösen des Parameters ab oder gibt es möglicherweise zu viele Übereinstimmungen mit mehrdeutigkeitsvermeidung (z. B. 80 Benutzern in deren Namen mit Bobo) aufgelöst. In diesen Fällen die app sendet eine **NeedsMoreDetails** Antwort und Siri fordert den Benutzer spezifischere sein.

Wenn der Benutzer einen Wert, die dafür erforderlich, den Prozess der Absicht ist bereitgestellt haben, senden sie eine **NeedsValue** als Antwort auf Siri auffordern den Benutzer für den Wert haben.

Wenn die app einen Wert nicht unterstützen, die der Benutzer für einen bestimmten Parameter akzeptiert hat, senden sie die **UnsupportedWithReason** als Antwort auf einen Grund angeben, warum der Wert wurde nicht unterstützt. Siri fordert dann den Benutzer für einen vollständig neuen Wert und der angegebene sie den Grund ist erforderlich.

Verwenden Sie schließlich die **NotRequired** Antwort Siri informieren, dass die app einen Wert für einen bestimmten Parameter erfordert. Wenn der Benutzer eine trotzdem bereitstellt, wird sie vom Siri einfach ignoriert werden.

### <a name="the-confirm-stage"></a>Die Confirm-Phase

Die Phase bestätigen verfügt über zwei Zwecken:

- Siri mitteilen, ist das erwartete Ergebnis der Behandlung von Priorität, sodass Siri den Benutzer erkennen kann, was passiert durchgeführt werden soll.
- Bietet einer Verkaufschance Überprüfung erforderliche Status die app möglicherweise die Anfrage durch den Benutzer, z. B. mit genügend Geld in der Bank für die die angeforderte Zahlung erforderlich.

Geben Sie die app wird ein **Absicht Antwort** aus dem Schritt bestätigen, die mit aufgefüllt werden soll, wie viele Informationen der app zur Verfügung steht, damit Siri mit der Benutzer effektiv kommunizieren kann.

Auf Grundlage des Domain "und" Aktion ", möglicherweise Siri den Benutzer zur Bestätigung, z. B. Eingabe vor dem Senden der Zahlung oder eine fuhr Buchung.

### <a name="the-handle-stage"></a>Der Handle-Phase

Die Stufe zu behandeln ist der wichtigste Teil, arbeiten mit der Priorität, da es der Punkt ist die app erfüllt, in dem die Anforderung des Benutzers ist, durch Ausführen des Tasks an, die er dazu aufgefordert wurde.

Wie es in der Phase bestätigen hat, muss die app so viele Informationen über das Ergebnis wie möglich bereitzustellen, damit dies Siri auf Benutzer beziehen kann. In einigen Fällen diese Informationen werden erhält visuell oder anderen Fällen Siri wird es einfach sprechen, zurück an den Benutzer. 

Wenn die app möglicherweise zusätzliche Zeit zum Verarbeiten einer bestimmten Anforderung, z. B. Aufruf netzwerkverzögerungen erfordern oder wenn eine live Person, die zur Erfüllung der Anforderung (z. B. abschließen und Versand einer Bestellung oder ein Auto beitragen, um den Standort des Benutzers) muss möglicherweise Zeiten vor. Wenn Siri auf eine Antwort von der app wartet, zeigt er eine warten-Benutzeroberfläche für den Benutzer informiert wird, dass die Anwendung die Anforderung verarbeitet wird.

Im Idealfall sollte die app eine Antwort auf Siri innerhalb von zwei bis drei Sekunden darf maximal bereitstellen. Wenn die app bekannt ist, dass eine bestimmte Antwort abgelaufen ist, den Prozess länger dauert, muss Sie zum Senden einer **InProgress** Antwortcode auf Siri. Siri informiert dann die Benutzer, dass die app die Anforderung im Hintergrund verarbeitet wird und weiterhin möglich, selbst wenn sie die Umgebung Siri lassen.

## <a name="adding-sirikit-to-the-app"></a>SiriKit der App hinzugefügt

Apple SiriKit in iOS-10 verfügt über zwei neue Erweiterungspunkte erstellt:

- **Intents Erweiterung** - Siri mit der app-Inhalte enthält, und führt die erforderlichen Aufgaben zum erfüllen alle unterstützten Intents.
- **Intents Benutzeroberflächenerweiterung** -bietet eine benutzerdefinierte Benutzeroberfläche, die angezeigt werden für die apps Inhalte innerhalb Siri. 

Es gibt auch eine API zur Siri bei Deaktivierung in Form von Wörtern und Ausdrücken zu gewähren:

- **App-Vokabular** -Wörter und Ausdrücke, die jedem Benutzer der app gemeinsam verwendet werden.
- **Benutzer-Vokabular** -Wörter und Ausdrücke, die für einen bestimmten app Benutzer eindeutig sind.

## <a name="the-intents-extension"></a>Die Erweiterung Intents

Die Erweiterung Intents ist verantwortlich für die wichtigsten Interaktionen zwischen der Anwendung und Siri wie folgt behandeln:

[![](understanding-sirikit-images/intents01.png "Die Erweiterung Intents")](understanding-sirikit-images/intents01.png#lightbox)

Die Absicht-Erweiterung kann eine oder mehrere Intents zu unterstützen, es obliegt dem Entwickler, zu entscheiden, wie sie in der app SiriKit implementieren möchten. Der Entwickler hinzufügen für jeden Zweck behandelt werden müssen auch eine separate Absicht-Erweiterung.  Dies bedeutet, dass Apple fordert an, dass der Entwickler die Anzahl der Absicht Erweiterungen beschränken, sodass Siri besitzt mehrere Prozesse, die für die app öffnen, die eine erfordern, mehr Arbeitsspeicher und die Uhrzeit, zu behandeln.

Der Entwickler sollte auch bewusst sein, dass die Absicht-Erweiterung im Hintergrund ausgeführt wird, während Siri aktiv ist. Dadurch wird Siri zur Übertragung von aktiv in einer Konversation mit dem Benutzer während der Kommunikation weiterhin mit der Erweiterung, um Informationen über die Anforderung zu verarbeiten.

## <a name="privacy-and-security-considerations"></a>Datenschutz und Überlegungen zur Sicherheit

Apple hat hervorragende Maßnahmen, um sicherzustellen, dass einen Benutzer, die private Informationen sicher ist, bei der Verwendung von Siri und folglich stehen mehrere Aktivitäten, die der Benutzer auf iOS-Gerät angemeldet werden übernommen. Beispielsweise, wenn eine fuhr anfordern oder Zahlungsoptionen.

Darüber hinaus sind bestimmte Verhaltensweisen, die möglicherweise die app möchten, um einzuschränken, die dem Benutzer das Gerät angemeldet wird. In diesen Situationen kann die app Anfordern der **einschränken gesperrten** Verhalten. Dies erfolgt über eine Einstellung in der `Info.plist` Datei.

Die lokale Authentifizierung Framework ist für die Absicht-Erweiterung verfügbar, daher die app für weitere Authentifizierungsinformationen, die Benutzer auffordern, kann auch, wenn das Gerät bereits aufgehoben wurde.

Apple Pay ist schließlich für die Absicht-Erweiterung verfügbar, damit die app eine Transaktion mit Apple Pay abzuschließen und das integrierte Apple Pay Blatt angezeigt, über die Siri-Schnittstelle kann wird.

Darüber hinaus Apple möchte sicherstellen, dass der Benutzer weiß, wenn sie Informationen zu einer 3rd Party-app und als solche Benutzer senden **müssen** sagen Sie den spezifischen Namen der app (wie in der app-Bündel-Anzeigename angegeben) Wenn eine Anforderung.

Apple Siri ausführen natürliche, flüssigen Konversationen mit dem Benutzer und aus diesem Grund wurde entwickelt, um der app-Bundle-Namen können in vielen Wortarten verwendet werden, wo er natürliche Weise in die Anforderung des Benutzers stimmt mit;

Eine der allgemeinen Aufgaben, die Benutzer ist "den Anwendungsnamen verbify" heißt, wobei der Name der app, und verwenden ihn als Verb in einer Anforderung. Beispielsweise *"MonkeyChat Bobo diese hervorragende Bananen wurden."*

## <a name="the-intents-ui-extension"></a>Die Benutzeroberflächenerweiterung Intents

Die Benutzeroberflächenerweiterung Intents bietet die Möglichkeit, schalten Sie der app Benutzeroberfläche und in die-Oberfläche Siri branding und stellen die Benutzer können mit der app verbunden. Mit dieser Erweiterung kann die app Brand als auch visual und andere Informationen in die Aufzeichnung bringen.

[![](understanding-sirikit-images/intents02.png "Beispielausgabe für Intents Benutzeroberflächenerweiterung")](understanding-sirikit-images/intents02.png#lightbox)

Die Benutzeroberflächenerweiterung Intents gibt stets eine `UIViewController` und die app hinzufügen nichts es gerne innerhalb der Ansicht-Controller, z. B. mit zusätzlichen Informationen, die die erste Antwort überschritten wird. Die Benutzeroberfläche Intents können auch Benutzer mit dem Status einer lang ausgeführten Ereignisses, z. B., wie viel länger dauert es, eine Freigabe Auto zum Erreichen von ihrem Speicherort fuhr aktualisieren.

Die Benutzeroberflächenerweiterung Intents immer zusammen mit anderem Inhalt Siri, beispielsweise das Symbol "app" und der Name am oberen Rand der Benutzeroberfläche angezeigt oder wird, basierend auf der Absicht, Schaltflächen (z. B. "Senden" oder "Abbrechen" ") können im unteren Bereich angezeigt werden.

Es gibt einige Instanzen, in dem die app die Informationen ersetzen können, die Siri für den Benutzer angezeigt wird, z. B. messaging standardmäßig oder zugeordnet, in dem die app mit einem zugeschnitten, um die app die standarderfahrung ersetzen können.

> [!IMPORTANT]
> **Hinweis:** während es möglich ist, interaktive Elemente hinzufügen, z. B. `UIButtons` oder `UITextFields` auf der Absicht Benutzeroberflächenerweiterung `UIViewController`, diesen sind streng unzulässig, als die Absicht-Benutzeroberfläche in nicht interaktiven und der Benutzer ist nicht in der Lage, zu interagieren mit ihnen.

Es ist völlig optional für die app eine beabsichtigte Benutzeroberflächenerweiterung angeben, da es sich bei Siri einen Standardsatz von Benutzeroberflächen für die einzelnen beabsichtigte enthält. Darüber hinaus sind die Schnittstellen Intents Benutzeroberfläche nur verfügbar für bestimmte Intents, dass Apple bewertet wurde für den Benutzer hilfreich sein würde.

## <a name="adding-sirikit-vocabulary"></a>Hinzufügen von SiriKit Vokabular

Der letzte Schritt implementieren SiriKit liegt innerhalb der app durch die Bereitstellung der erforderlichen Vokabular. Viele Apps haben eindeutige Möglichkeiten zur Beschreibung von Informationen für den Benutzer und eine eindeutige Weise, dass der Benutzer Informationen an die app bereitstellt.

Aus diesem Grund erfordert Siri der app um Unterstützung zu erhalten, die Wörter und Ausdrücke, die nur für die app zu verstehen. Einige der folgenden Meldungen werden Teil der app, damit jeder Benutzer, kennen und verstehen. Andere werden noch für einen angegebenen Benutzer der app eindeutig sein.

### <a name="app-specific-vocabulary"></a>App-spezifische Vokabular

Das bestimmte App-Vokabular definiert die bestimmte Wörter und Ausdrücke, die bekannt sein muss, werden für alle Benutzer mit der app, z. B. Fahrzeugtypen oder Namen des Trainings an. Da diese Teil der Anwendung sind, werden in definiert eine `AppIntentVocabulary.plist` -Datei als Teil des Haupt-app-Pakets. Darüber hinaus sollten diese Wörter und Ausdrücke lokalisiert werden.

Es gibt mehrere Komponenten für ein Vokabular `AppIntentVocabulary.plist` Datei:

- **Beispiel-App verwendet** -dazu stehen eine Reihe von gängigen Anwendungsfällen für die Anforderungen, die der Benutzer der Anwendung vornehmen können. Zum Beispiel: *"Eine Trainings mit MonkeyFit starten".*
- **Parameter** – diese stellen einen Satz von nicht standardmäßigen Parametertypen, die speziell auf die app bereit. Beispielsweise Trainings Namen für die app MonkeyFit. Diese bestehen aus:
    - **Ausdruck** -ermöglicht der app, eindeutige Begriffe für die app zu definieren. Zum Beispiel: "Bananarific"-Trainings-Typ für die app MonkeyFit. 
    - **Aussprache** -Siri Aussprache Hinweise zuweist, als eine einfache phonetische Schreibweise nach einem bestimmten Ausdruck. Beispiel: "Ba Nana ri einen bestimmten".
    - **Beispiel** -enthält ein Beispiel für die Verwendung des angegebenen Ausdrucks in der app. Beispielsweise *"Starten eines Bananarific in MonkeyFit"*.

Weitere Informationen finden Sie in der Apple- [App Vokabular Format Dateiverweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Bestimmte Benutzer-Vokabular

Der Benutzer bestimmte Vokabular geht zum Bereitstellen von Wörtern oder Ausdrücken, die an einzelne Benutzer der app eindeutig sind. Diese werden als eine geordnete Menge von Begriffen in einem signifikantesten Priorität für die Benutzer, mit der die wichtigsten Begriffe am Anfang der Liste sortiert zur Laufzeit aus dem Haupt-app (nicht die App-Erweiterungen) angegeben werden.

Betrachten Sie das Beispiel für die oben genannten MonkeyChat-app. MonkeyChat hält eine Liste aller Kontakte des Benutzers, die zum Siri über das Benutzer bestimmte Vokabular gesendet wird. Darin sind auch eine Liste der letzten 10 Kontakte, die der Benutzer Nachrichten hat und er verfügt über einen Satz von Favoritenkontakte für jeden Benutzer. In diesem Beispiel sollte die bevorzugten Kontakte zu Beginn der unsere Benutzer bestimmte Vokabular, gefolgt von der Kontakte klicken Sie dann den Rest der Kontakte eines Benutzers sein.

Die folgenden Arten von Informationen werden vom Benutzer bestimmte Vokabular unterstützt:

- Wenden Sie sich an Namen.
- Trainings-Namen.
- Fotoalbum Namen.
- Foto-Schlüsselwörter.

Wenn die app im IOS-Adressbuch – abhängig ist, wird die app keine keine Maßnahmen ergreifen wie diese Informationen bereits zu Siri verfügbar ist. Die app muss nur Kontaktnamen zu sorgen, wenn die app einen eigenen eindeutigen Datenbanknamen von Kontakten hat.

Geben Sie beim Entwerfen des Vokabulars, nur die notwendigen Werte, die der Benutzer kennen und von Interesse. Vermeiden Sie die Bereitstellung von Informationen wie z. B. Telefonnummern oder e-Mail-Adressen.

Die app muss auch Siri sofort zu aktualisieren, wenn Benutzer bestimmte Vokabular ändert. Benutzer sind zum Anfordern von Informationen aus Siri den Zeitpunkt daran gewöhnt, die auf seinem iOS-Gerät hinzugefügt wurde. Z. B. wenn der Benutzer ein neues Kontakts der app hinzugefügt wird, senden Sie diese Informationen an Siri, sobald der Benutzer es speichert.

Vor allem die app _müssen_ Löschen von Informationen aus dem Vokabular Siri umgehend, da ein Benutzer stören werden konnte, wenn sie eine Information gelöscht, aber Siri noch Stunden oder Tage später erkannt wurde.

> [!IMPORTANT]
> **Hinweis:** die app sollte Entfernen aller Benutzer bestimmte Vokabular aus Siri, wenn der Benutzer entscheidet, die app zurücksetzen oder sie abmelden.

## <a name="sirikit-permissions"></a>SiriKit-Berechtigungen

Berechtigungen wird das letzte Stück SiriKit erübrigt. Genau wie mit anderen Funktionen von iOS (z. B. Fotos, Kamera oder Kontakte), müssen Benutzer die explizite Berechtigung für die app mit Siri sprechen zu gewähren.

Die app ist eine Zeichenfolge, welche Informationen er Siri ermöglichen und geben Sie einen Grund, warum der Benutzer diesen Zugriff gewähren, sollten angeben zu können. 

Apple schlägt vor, dass die app sollte eine Berechtigung anfordern, vom Benutzer Siri das erste Mal verwenden, der Benutzer die Anwendung öffnet, nachdem sie ein Upgrade auf iOS 10 durchgeführt haben. Dies ist, damit Benutzer, über die Integration Siri wissen und vorab genehmigter Nutzung können, bevor sie ihre erste Anforderung vornehmen können.

## <a name="sirikit-and-maps"></a>SiriKit und Karten

SiriKit ist ein integraler Bestandteil von iOS und nutzt die größeren Intents-Framework für iOS-10 hinzugefügt. Die Intents-Framework wurde entworfen, um Aktionen common "und" freigegebene und Intents zusammen mit anderen Teilen des Systems nutzen.

Das Framework Intents nur Siri Integration hinausgeht und stellt weitere Funktionen, wie die Kontakte-Integration, in dem die app die Telefonie Standardwert oder die messaging-app für bestimmte Kontakte werden kann. Intents bieten auch die enge Integration mit CallKit Benutzern mit VOIP nutzungserlebnis zu bieten.

Die Maps-app im iOS 10 verfügt über Funktionen, z. B. bei Stromausfällen freigeben, in denen der Benutzer eine fuhr direkter Verweis innerhalb der Maps-Benutzeroberfläche book kann hinzugefügt. SiriKit bietet eine allgemeine Erweiterungspunkt mit Maps aus, damit bei Stromausfällen Dateifreigabe (und andere) Intents Siri und Karten gemeinsam genutzt werden können. 

Dies bedeutet, dass die app die SiriKit Erweiterungen beschlossen, er auch die Integration von Maps kostenlos erhält. 

## <a name="designing-a-great-siri-experience"></a>Entwerfen einer Siri Problembehandlungserlebnis

Entwerfen eine hervorragende Benutzeroberfläche beim Integrieren von einer app in Siri unterscheidet sich das Entwerfen einer tollen app-Benutzeroberfläche zur Verfügung. Im Gegensatz zu normalen Situationen, in denen der Benutzer mit der app, die direkt auf interagiert, Bildschirm, wenn es mit Siri sind häufig, wenn keine visuelle Oberfläche sichtbar überhaupt ist. Z. B. wenn der Benutzer die Konversation mit gestartet hat *"Hey Siri"*.

### <a name="how-siri-helps-the-developer"></a>Wie den Entwickler kann Siri

Beim Entwerfen einer app Interaktionen mit Siri wird die app erstellen eine *natürlichsprachliches Schnittstelle*, d. h. den Kontext stammt aus der Konversation, die mit dem Benutzer in der app-Namen Siri aufgetreten sind.

Eine visuelle Referenz vorhanden ist der Benutzer muss die Informationen in ihren Head von verfolgt. Aus diesem Grund bietet Siri den bare minimale Informationen erforderlich, um den Vorgang zu erreichen, dass der Benutzer, erreichen möchte, ist.

Die Netzwerkebene Schnittstelle wird von Fragen und Antworten für den des Benutzers und die Siri während der Konversation strukturiert. Daher ist es wichtig zu bedenken, wie Siri Fragen stellt und reagiert, wenn diese Schnittstelle zu entwerfen.

Nehmen Sie im folgende Beispiel für den Benutzer erstellt eine Nachricht Siri reagieren möglicherweise mit der Frage *"Bereit zum Senden?"* . Der Benutzer kann auf viele verschiedene Arten reagieren, wie z. B. *"Senden"*, *"Abbrechen"* oder sogar etwas völlig unabhängig vom stagingstatus diese Frage. Unabhängig davon, wie die Konversation spielt wird Siri für die app zu behandeln, und nur senden sie die relevante Informationen sobald sie verfügbar sind.

Es gibt mehrere Möglichkeiten, dass ein Benutzer eine Konversation mit Siri initiieren kann:

- Durch übernehmen das Gerät aus, drücken die Schaltfläche "Start". In diesem Fall wird Siri Weitere visuellen Schnittstellen und weniger verbalen Antworten vorgelegt.
- Mit den Worten *"Hey Siri"* und Starten einer Konversations Hände frei. In diesem Fall werden Siri weniger visual und mehr verbale.
- Mithilfe von Funktionen zur Barrierefreiheit, z. B. Bluetooth aktiviert hörgeschädigt sind Hilfsmittel, in dem die Benutzeroberfläche für einen Benutzer mit besonderen Anforderungen zugeschnitten ist.
- Mit Auto wiedergeben, in denen der Benutzer ihre Aufmerksamkeit behalten muss, konzentriert sich driving abgelenkt auf ein Minimum beschränkt bleiben.

### <a name="how-the-developer-helps-siri"></a>Wie kann Entwickler Siri

Wenn Siri eine app integrieren, muss der Entwickler häufig durch diese Integration testen, und stellen Sie sicher, dass sie viele unterschiedliche Anforderungen von gefragt werden, wie möglich für den gleichen Teil Informationen oder der Task auf wie viele verschiedene Arten vornehmen.

Da keine zwei Reaktionszeiten gleichermaßen Personen, ist es wichtig, dass der Entwickler so viele verschiedene Betatester wie möglich zu Feinabstimmungen Siri Integration erhält. Benutzer möglicherweise aufgefordert, Informationen oder stellen anfordert Möglichkeiten des Entwicklers nie jedoch der und diese Optimierung können Sie sicherstellen, dass die umfangreichste Gruppe von Benutzern eine überzeugenden Umgebung, die mit ihrer app mit Siri.

In anderen Situationen und Umgebungen zu testen. Die Konversationen mit Siri in allen Arten möglich ist, stellen Sie sicher, dass diese Konversationen Fluid bleiben und natürliche zu initiieren. Testen Sie in die Speicherorte, in dem der Benutzer mehr als wahrscheinlich die app wie in einem Rechenzentrum weiter aufzurüsten Fitnesscenter verwendet werden würde.

Stellen Sie sicher, dass die app alle Informationen bereitstellt, Siri benötigt wird, um die Anforderung und das Ergebnis ordnungsgemäß für den Benutzer darstellen. Dies gilt vor allem bei Verwendung von Siri in einer Situation Hände frei.

### <a name="siri-design-guidelines"></a>Siri Entwurfsrichtlinien

Erinnerung: immer Siri eine Konversation mit dem Benutzer im Namen der app aufgetreten sind. Der Entwickler möchte nicht sicher, dass diese Konversation bleibt als flüssig und natürliche wie möglich.

Als auch mit wichtigen Konversationen der Fall ist, muss der Entwickler Folgendes beachtet werden:

- Die app für die Konversation vorbereitet ist.
- Die app Port überwacht, genau wie der Benutzer zu erreichen versucht.
- Dass die app die entsprechenden Fragen zu den entsprechenden Zeitpunkten aufgefordert.
- Dass die app mit den Informationen auf die Anforderung, die der Benutzer gesucht wird reagiert.

#### <a name="preparing-for-the-conversation"></a>Vorbereiten für die Konversation

Erstes sollten Sie denken Sie daran ist, dass Benutzer der app ist nicht exakt so wie der Entwickler sein. Sie können aus verschiedenen Hintergründen unterschiedliche Sprachen sprechen, oder verfügen über besondere Anforderungen, bei der Arbeit mit der app stammen.

Da der Entwickler entworfen und erstellt die app, haben sie darüber hinaus tief; fundierte Kenntnisse der hinsichtlich der app und der internen Funktionsweise und Funktionen, die ein Benutzer normalerweise nicht verbunden ist. Daher kann der Entwickler Anforderung der Siri anders als normaler Benutzer bitten.

Deshalb ist es ist wichtig, wie viele verschiedene Personen wie möglich mit der app über Siri interagieren. Benutzer können Anforderungen an die Anwendung über Siri vornehmen, die der Entwickler nie verstanden hat, oder in Methoden, mit denen, die Entwickler berücksichtigen haben nicht.

#### <a name="ensure-the-app-is-a-good-listener"></a>Stellen Sie sicher, dass die App einen guten Listener ist

Der Entwickler muss sicherstellen, dass die app eine gute Listener ist und nur noch die Einzelheiten der Konversation, die der Benutzer den Erwartungen. Aber es ist auch möglich, dass sie nicht alle Informationen eingegeben haben möglicherweise, dass die app erfordert, um die angeforderte Aufgabe zu erzielen.

Es gibt mehrere Möglichkeiten, dass die Anwendung diese Situation verarbeiten konnte:

- **Wählen Sie ein Standardwert für den fehlenden Wert** – z. B. eine Freigabe-app fuhr möglicherweise zum aktuellen Ort des Benutzers standardmäßig, wenn sie angegeben haben, in dem sie verwenden wollten, aus übernommen werden.
- **Fachkenntnisse** -verwenden spezifische Informationen, die der Benutzer die app gesammelt hat, die app möglicherweise Marke oder ein bestimmtes Fachkenntnisse auf die fehlenden Informationen, wie z. B. das Ausfüllen der fehlenden Mobiltelefonnummer von Kontaktinformationen des Benutzers. Allerdings sollte geachtet werden, ungültige überraschungen, z. B. auswählen, die teuerste Option usw. zu vermeiden.
- **Weitere Informationen auffordern** -Siri auffordern den Benutzer für den fehlenden Wert kann über die app verfügen. Allerdings wird hier der Schlüssel der Konversationen einfach und zum Zeitpunkt beibehalten. Benutzer werden schnell frustriert besäßen sie verschiedene Fragen beantworten, die ihre Anforderung zu erreichen.
- **Handhaben irreführende Informationen** -der Benutzer kann einen Wert, der die app wurde nicht erwartet oder, die es im angegebenen Kontext verarbeiten kann nicht, bereitstellen. Stellen Sie sicher, dass die app bezieht sich diese Situation für den Benutzer auf eine Weise, in dem deutlich und leicht für sie beheben können.

Wenn die app mit einem einzigen Wert, die zweifelhaft ist dargestellt wird, ist die bevorzugte Methode zum Behandeln dieses Siri den Benutzer zur Bestätigung auffordern, aufweisen. Beispielsweise *"Meinten Sie Bobo einzigartigen?"* , die sie mit einer einfachen Ja oder Nein-Antwort Antworten können.

Wird eine Situation, in denen mehrere mögliche Optionen für einen einzelnen Wert korrekt werden konnte, ist zur Klärung der bevorzugte Behandlungsmethode. In diesem Fall kann Siri den Benutzer mit bis zu zehn möglichen Optionen zur Auswahl aufgefordert. Zum Beispiel:

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Wenn Sie sich noch in Frage, haben Sie Siri fordert den Benutzer auf eine völlig neue, finden Sie spezifischere-Antwort für einen bestimmten Wert angeben.

#### <a name="request-final-confirmation"></a>Abschließende Bestätigung anfordern

Bevor die app tatsächlich die Aufgabe für die Erfüllung der Anforderung des Benutzers ausführt, prüft Siri mit der App-Erweiterung, um sicherzustellen, dass alles vorhanden ist. Ist der Benutzer besitzt z. B. genug Geld in ihrem Konto für die die angeforderte Zahlung?

Darüber hinaus muss die app, um sicherzustellen, dass er alle möglichen Informationen zu Siri bereit ist, damit es dem Benutzer präsentieren und sicher, dass die Task ausgeführt wird, werden ihre Erwartungen erfüllt werden kann.

Sobald der Benutzer die Anforderung und der app bestätigt hat durchgeführten es der app muss erneut stellen Sie sicher, dass er alle Ergebnisse zurück an Siri bereitgestellt hat, damit es dem Benutzer verknüpft sein kann.

#### <a name="responding-to-the-request"></a>Auf die Anforderung reagiert

Siri enthält mehrere integrierte Benutzeroberflächen für jede der Domänen und der Aktionen, die über bekannt ist. Allerdings kann gegebenenfalls die app einer benutzerdefinierten Absicht Benutzeroberflächenerweiterung, um die benutzerfreundlichkeit zu ergänzen, durch die Bereitstellung der app branding und Benutzeroberfläche oder Weitere Informationen benötigen, als in der Anforderung vorhanden war vermittelt.

Dies bedeutet, dass beim Entwerfen der benutzerdefinierter Schnittstellen Siri Führung verwendet werden soll. In der Regel wird der Benutzer wird eine bestimmte Aufgabe ausgeführt wird, so schnell wie möglich abrufen möchte, und nicht mit Informationen überladen werden soll.

Außerdem sollte vorsichtig vorgenommen werden, um sicherzustellen, dass die benutzerdefinierte Benutzeroberfläche sucht und in der anderen iOS-Geräte und die Ausrichtungen an, dass der Benutzer möglicherweise, oder Sie das Gerät verwenden ordnungsgemäß reagiert.

Wenn dies angebracht ist, verwenden Sie die SiriKit-API So blenden Sie alle redundanten Informationen, die bereits in der standardmäßigen Siri UI aus. Noch stellen Sie sicher, dass die app die Informationen noch um Siri bereitstellt, damit es Alternativtext in eine praktische freien Situationen darstellen kann.

Es gibt möglicherweise Situationen, in denen Siri starten die app zum Erfüllen der Anforderung des Benutzers, z. B. präsentieren die Fotos, die der Benutzer angefordert hat. In diesen Situationen sollte überraschen Sie nicht den Benutzer werden. Zeigen Sie die erwarteten Informationen ohne Zwischenschritte oder weitere Interaktion erforderlich. Nie Anzeigeinformationen oder eine Aufgabe, die der Benutzer erwartet wird nicht ausgeführt.

### <a name="polishing-the-design"></a>Verfeinerung nach dem Entwurf

Es sind mehrere Schritte, die von Apple vorgeschlagen, um den Entwurf der natürlichsprachliches Schnittstellen Polnisch. Erstens sind von Beispielen für klare und prägnante Vokabulars und Verwendung Groß-/Kleinschreibung zu Siri.

Einer der Methoden, die ein Benutzer die app ermittelt wurden wird durch das Initiieren einer Konversations mit Siri und anfordern, *"Was können Sie tun?"* Siri zeigt mehrere verschiedene Dinge, es möglich, einschließlich des Entwicklers-app und die Beispiel-Hero Anwendungsfälle, die darauf über seine `plist` Datei.

Wie Sie zu den Einsatzgebieten gutes Beispiel schreiben:

- Stellen Sie sicher, dass im Beispiel ist der Name der app enthalten.
- Behalten Sie das Beispiel kurz- und zu Punkt.
- Geben Sie mehrere Beispiele für jede der Intents, die die app unterstützt.
- Priorisieren Sie die Absichten und in den Beispielen in basierend auf den am häufigsten verwendeten Fällen für die app zu verwenden.
- Stellen Sie sicher, dass die app lokalisierte Beispiele zur Verfügung.
- Stellen Sie sicher, dass jedes Beispiel funktioniert wie erwartet innerhalb der app.
- Vermeiden Sie Adressierung Siri in den Beispielen, damit keine Text wie enthalten *"Hey Siri..."*
- Vermeiden Sie z. B. unnötigen Pleasantries *"Bitte"* oder *"Danke"*.

Die entsprechende Zeit für das untersuchen und zu experimentieren, auf wie die app die Konversation strukturieren kann, die mit dem Benutzer in seinem Auftrag Siri aufgetreten sind. Achten Sie darauf, sprechen Sie mit normalen Benutzern während des Prozesses als ihre Interaktionen mit und Erwartungen der app können mit der Zeit ändern.

Stets auf die app in verschiedenen Situationen und alle anderen Methoden zum Aufrufen einer Konversations mit Siri zu testen. Test in der realen Welt Speicherorte des Benutzers möglicherweise verwenden Sie die app Weg von der Office und das Helpdesk.

Bemühen Sie, die Konversationen mit Siri (im Auftrag der app) Fluid, natürlichen Erscheinungsbild besitzen und "einfach" sein. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die grundlegenden Konzepte erforderlich, um SiriKit verwenden und gezeigt, dass es mit den apps Xamarin.iOS, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Maps-app auf einem iOS-Gerät zugänglich sind interagieren kann behandelt.




## <a name="related-links"></a>Verwandte Links

- [ElizaChat-Beispiel](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Programmierhandbuch SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Intents Frameworkverweis](https://developer.apple.com/reference/intents)
- [Referenz zur Benutzeroberfläche des Framework Intents](https://developer.apple.com/reference/intentsui)
