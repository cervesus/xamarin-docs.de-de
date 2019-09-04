---
title: Grundlegendes zu SiriKit-Konzepten
description: In diesem Dokument werden die wichtigsten Konzepte beschrieben, die für die Arbeit mit dem Sirikit in einer xamarin. IOS-App erforderlich sind. Beispielsweise werden Intents und Intents UI Extensions, Sirikit-Berechtigungen, das Entwerfen einer hervorragend Benutzeroberfläche und vieles mehr erläutert.
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: bce2c1e543084ea80908946b1e37e43cf53c1676
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227347"
---
# <a name="understanding-sirikit-concepts"></a>Grundlegendes zu SiriKit-Konzepten

_In diesem Artikel werden die wichtigsten Konzepte behandelt, die für die Arbeit mit dem Sirikit in einer xamarin. IOS-App erforderlich sind._


Mit dem neuen IOS 10 ermöglicht das Sirikit einer xamarin. IOS-APP das Bereitstellen von Diensten, auf die der Benutzer mithilfe von Siri und der Maps-APP auf einem IOS-Gerät zugreifen kann. Diese Funktionalität wird in einer oder mehreren App-Erweiterungen mithilfe der neuen **Intents** und **Intents-Benutzer** Oberflächen-Frameworks bereitgestellt.

Mit "Sirikit" kann eine IOS-App Dienste bereitstellen, auf die der Benutzer mit Siri und der Maps-APP auf einem IOS-Gerät mithilfe von App-Erweiterungen und den neuen **Intents** und **Intents-Benutzer** Oberflächen-Frameworks zugreifen kann

Siri arbeitet mit dem Konzept von **Domänen**, Gruppen von bekannten Aktionen für Verwandte Aufgaben. Jede Interaktion zwischen der APP und Siri muss wie folgt in eine der bekannten Dienst Domänen fallen:

- Aufrufen von Audiodaten oder Videos.
- Die Reservierung einer Fahrt.
- Verwalten von-Workouts.
- Übermittlung.
- Fotos werden gesucht.
- Senden oder empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri an einen der Dienste der APP-Erweiterung sendet, sendet Sirikit der Erweiterung ein **Intent** -Objekt, das die Anforderung des Benutzers zusammen mit allen unterstützenden Daten beschreibt. Die APP-Erweiterung generiert dann das entsprechende **Antwort** Objekt für die angegebene **Absicht**und erläutert, wie die Erweiterung die Anforderung verarbeiten kann.

## <a name="the-intents-and-intents-ui-extensions"></a>Intents und Intents UI Extensions

Sowohl Siri als auch die Maps-APP interagieren mit den Diensten der APP über zwei unterschiedliche Arten von App-Erweiterungen:

- **Intents-Erweiterung** : gibt Siri und Maps mit dem Inhalt der APP an und führt die Aufgaben aus, die erforderlich sind, um alle unterstützten Intents zu erfüllen.
- **Intents-Benutzeroberflächen Erweiterung** : stellt eine benutzerdefinierte Benutzeroberfläche bereit, die für den Inhalt der app in Siri oder Maps angezeigt wird.

Die APP muss eine Intents-Erweiterung zur Unterstützung von Sirikit bereitstellen, und Sie ist verantwortlich für die Bereitstellung von Informationen, die Siri und Maps für den Benutzer bereitstellen können.

Das Erstellen einer Intents-Benutzeroberflächen Erweiterung ist optional, da Siri in der Regel die gesamte Benutzerinteraktion verarbeitet und über eine standardmäßige, integrierte Benutzeroberfläche zum Darstellen von Informationen in jeder der unterstützten Domänen verfügt. Durch die Bereitstellung einer Intents-Benutzeroberflächen Erweiterung kann die APP das **Intent UI** -Framework verwenden, um eine umfangreiche, benutzerdefinierte Benutzeroberfläche mit dem Branding der APP und zusätzlichen Informationen zu präsentieren.

## <a name="siri-and-the-maps-app-role"></a>Siri und die APP-Rolle Maps

Die gesprochenen Anforderungen des Benutzers werden von Siri verarbeitet und semantisch analysiert, wodurch diese Anforderungen in umsetzbare Intents umgewandelt werden, die von den beabsichtigten Erweiterungen verarbeitet werden können.

In Maps werden die Intent-Erweiterungen der APP verwendet, um Informationen in der Kartenschnittstelle als Reaktion auf Benutzeraktionen anzuzeigen. Z. b. das Anfordern von Restaurants in der Nähe oder das Abrufen der Restaurant Reviews der APP

Sowohl Siri als auch Maps verwalten alle Interaktionen des Benutzers und zeigen die Ergebnisse mithilfe der Standardsystem Schnittstelle an. Die Rolle "App-Erweiterungen" besteht hauptsächlich darin, die angezeigten Daten bereitzustellen. Optional kann die APP eine Intents-Benutzeroberflächen Erweiterung bereitstellen und eine benutzerdefinierte Benutzeroberfläche zur Verbesserung der Standardsystem Schnittstelle darstellen.

## <a name="interacting-with-siri-via-sirikit"></a>Interaktion mit Siri über Sirikit

In diesem Abschnitt erhalten Sie einen Überblick darüber, wie mit dem Benutzer von Sirikit die Interaktion mit der App mithilfe von Siri ermöglicht wird. Für dieses Beispiel verwenden wir die gefälschte monkeychat-App:

[![](understanding-sirikit-images/monkeychat01.png "Das monkeychat-Symbol")](understanding-sirikit-images/monkeychat01.png#lightbox)

Monkeychat hält ein eigenes Kontaktbuch der Freunde des Benutzers, die jeweils einem Bildschirmnamen (z. b. "Bobo") zugeordnet sind, und ermöglicht es dem Benutzer, Text Chats über den Bildschirmnamen an jeden Freund zu senden.

Es gibt viele Möglichkeiten, wie der Benutzer eine Interaktion mit der APP initiieren kann, da verschiedene Personen dieselbe Anforderung in vielen verschiedenen Formularen vornehmen können.

Wenn der Benutzer z. b. eine Nachricht an den Friend-Bobo senden möchte, kann er über die folgende Konversation mit Siri verfügen:

_Bedienungs Hey Siri, sendet eine monkeychat-Nachricht._<br />
_Siri Zu wem?_<br />
_Bedienungs Bos._<br />
_Siri Was möchten Sie für "Bobo" sagen?_<br />
_Bedienungs Senden Sie weitere Bananen._<br />

Eine andere Person kann dieselbe Anforderung mit einer anderen Konversation treffen:

_Bedienungs Senden Sie eine Nachricht an Bobo auf monkeychat._<br />
_Siri Was möchten Sie für "Bobo" sagen?_<br />
_Bedienungs Senden Sie weitere Bananen._<br />

Und ein anderer Benutzer kann eine noch kürzere Anforderung stellen:

_Bedienungs Monkeychat-Bobo senden Sie weitere Bananen._<br />
_Siri OK, senden Sie eine Nachricht, und senden Sie weitere Bananen an Bobo auf monkeychat._<br />

Sie können auch dieselbe Anforderung in einer anderen Sprache ausführen:

_Bedienungs Monkeychat-"......._<br />
_Siri Oui, envoi-Nachricht, ' il vous plaît Envoyer ' plus de Bananes à Bobo sur monkeychat._<br />

Ein anderer Benutzer ist in der Konversation jedoch möglicherweise sehr ausführlich:

_Bedienungs Wenn Sie Siri haben, können Sie mich bevorzugen und die monkeychat-app starten, um einen Text mit der Nachricht zu senden. senden Sie weitere Bananen._<br />
_Siri Zu wem?_<br />
_Bedienungs Mein bester PAL-Bobo._<br />

Außerdem gibt es viele Möglichkeiten, wie Siri auf eine Anforderung reagieren kann, und zwar basierend auf der Art der Anforderung:

- **Durch die Verwendung der Schaltfläche "Home** " (Start Schaltfläche) werden mehr visuelle Antworten mit eingeschränktem verbalen Feedback
- **Von "Hey Siri"** -Siri ist eher verbal und bietet weniger visuelle Antworten.

Siri ist auch darauf abgestimmt, den Barrierefreiheits Anforderungen des Benutzers gerecht zu werden und auf der Grundlage dieser Anforderungen zu interagieren.

Unabhängig davon, wie eine Anforderung erfolgt oder wie Siri auf die Anforderung antwortet, verarbeitet Siri die Konversation mit dem Benutzer, und die APP (über deren Erweiterungen) stellt die Funktionalität bereit.

Wenn der Benutzer eine verbale Anforderung von Siri sendet, sind dies die Schritte, die von Siri befolgt werden:

[![](understanding-sirikit-images/monkeychat02.png "Die Schritte, die von Siri befolgt werden.")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Zuerst übernimmt Siri das audiozeichen der **Sprache** des Benutzers und konvertiert es in Text.
2. Als nächstes wird der Text in eine **Absicht**konvertiert, eine strukturierte Darstellung der Anforderung des Benutzers.
3. Basierend auf der Absicht führt Siri eine **Aktion** aus, um die Anforderung des Benutzers auszuführen.
4. Zum Schluss stellt Siri dem Benutzer auf der Grundlage der ausgeführten Aktion **Antworten** (sowohl visuell als auch verbal) dar.

Es gibt drei Hauptmethoden, mit denen die APP bei Siri an der Konversation des Benutzers teilnehmen kann:

[![](understanding-sirikit-images/monkeychat03.png "Die drei wichtigsten Möglichkeiten, dass die APP an der Benutzer Konversation mit Siri teilnehmen kann.")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Vokabular** : auf diese Weise teilt die APP Siri die Wörter, die Sie kennen müssen, um mit der Anwendung zu interagieren.
2. **App-Logik** : Hierbei handelt es sich um die Aktionen und Antworten, die von der App basierend auf den jeweiligen Intents ausgeführt werden.
3. **Benutzeroberfläche** : Dies ist die optionale benutzerdefinierte Benutzeroberfläche, in der die APP Antworten senden kann.

### <a name="example"></a>Beispiel

Überprüfen Sie anhand der obigen Informationen, wie die folgende Konversation mit der monkeychat-App interagieren würde:

_Bedienungs Hey Siri, sendet eine Nachricht an Bobo auf monkeychat._<br />
_Siri Was möchten Sie für "Bobo" sagen?_<br />
_Bedienungs Senden Sie weitere Bananen._<br />

Die erste Rolle, die die app in der Konversation übernimmt, ist die Unterstützung von Siri beim Verständnis der Sprache des Benutzers:

[![](understanding-sirikit-images/monkeychat04.png "Unterstützung von Siri bei der sprach sprechungssprache")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri hat nicht den Namen "Bobo" in der Datenbank, aber die APP hat diese Informationen über das Vokabular mit Siri gemeinsam genutzt. Mit der APP kann Siri auch erkennen, dass Bobo ein Empfänger ist, da Sie als *Kontakt*für Siri angegeben wurde.

Siri weiß, dass mehr erforderlich ist, um eine Nachricht zu senden, als nur ein Empfänger, sodass Sie schnell mit der APP-Erweiterung überprüfen kann, ob eine Nachricht Inhalte erfordert. Da monkeychat dies tut, antwortet Siri an den Benutzer mit der folgenden Frage: *"Was möchten Sie an Bobo sagen?"*

Im obigen Beispiel hat der Benutzer geantwortet: *"Bitte senden Sie weitere Bananen"* , die Siri in eine strukturierte **Absicht**bündeln wird:

[![](understanding-sirikit-images/monkeychat05.png "Siri bündelt die Antwort des Benutzers in eine strukturierte Absicht")](understanding-sirikit-images/monkeychat05.png#lightbox)

Die strukturierte Absicht enthält die folgenden Informationen:

- **-** Meldungen
- **Absicht:** SendMessage
- **Erhält** Bos
- **Inhaltliche** Senden Sie weitere Bananen

Jede Domäne verfügt über eine Reihe von bekannten *Aktionen* , die innerhalb der Domäne ausgeführt werden können. basierend auf der Domäne und der Aktion können null bis viele Parameter in die Absicht eingeschlossen werden, die an die APP gesendet wird.

Die Absicht wird dann zur Verarbeitung an die APP-Erweiterung gesendet. Aufgrund der Verarbeitung der Absicht generiert die APP ein **intentresponse** -Paket, das mit der Absicht gebündelt wird und Parameter enthält, die beschreiben, was die APP mit der Absicht getan hat.

Jede intentresponse enthält auch einen **Antwort Code** , der Siri anweist, ob die APP die Anforderung erfüllen konnte oder nicht. Einige Domänen verfügen über sehr spezifische Fehler Antwort Codes, die ebenfalls gesendet werden können.

Schließlich enthält die intentresponse eine `NSUserActivity` (z. b. die, die zur Unterstützung von Hand Eingaben verwendet werden). `NSUserActivity` Wird verwendet, um die APP zu starten, wenn die Antwort erfordert, dass Sie die SIRI-Umgebung verlassen und die app eingeben, um Sie abzuschließen.

Siri erstellt automatisch einen geeigneten `NSUserActivity` , um die APP zu starten und zu Abholung, wo der Benutzer in der SIRI-Umgebung aufgehört hat. Die APP kann jedoch eine eigene `NSUserActivity` mit benutzerdefinierten Informationen bereitstellen, wenn dies erforderlich ist.

Nachdem die APP die Absicht verarbeitet und eine Antwort an Siri zurückgegeben hat, zeigt Sie die Ergebnisse dem Benutzer (sowohl verbal als auch visuell) an:

[![](understanding-sirikit-images/monkeychat06.png "Die Ergebnisse werden dem Benutzer sowohl verbal als auch visuell angezeigt.")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri verfügt über mehrere integrierte Benutzeroberflächen für die Reaktionsfähigkeit jeder Domäne, die für die app verfügbar ist. Da monkeychat jedoch eine optionale Benutzeroberflächen Erweiterung der Absicht bereitgestellt hat, wird es verwendet, um dem Benutzer die Ergebnisse der Konversation im obigen Beispiel darzustellen.

## <a name="the-intent-lifecycle"></a>Der Intent-Lebenszyklus

Es gibt drei Hauptaufgaben, die die APP-Erweiterung beim Umgang mit Intents ausführen muss:

[![](understanding-sirikit-images/monkeychat07.png "Der Intent-Lebenszyklus")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. Die APP muss alle Parameter für ein Ereignis **Auflösen** . Folglich ruft die APP mehrmals auflösen (einmal pro Parameter) und manchmal mehrmals für denselben Parameter auf, bis die APP und der Benutzer den angeforderten Anforderungen zustimmen.
2. Die APP muss **bestätigen** , dass Sie die angeforderte Absicht verarbeiten kann, und Siri über das erwartete Ergebnis informiert.
3. Schließlich muss die APP die Absicht **verarbeiten** und die Schritte ausführen, um das gewünschte Ergebnis zu erzielen.

### <a name="the-resolve-stage"></a>Die Auflösungsphase

Mit der Auflösungsphase können Siri die vom Benutzer bereitgestellten Werte verstehen und sicherstellen, dass der tatsächliche Benutzer tatsächlich beabsichtigt ist, wenn die Absicht von der APP verarbeitet wird.

Diese Phase bietet auch eine Möglichkeit für die APP, das Verhalten von Siri während der Konversation mit dem Benutzer zu beeinflussen. Zu diesem Zweck stellt die APP eine **Auflösungs Antwort**bereit. Es gibt eine Reihe vordefinierter Antworten auf die unterschiedlichen Typen von Daten, die Siri versteht.

Die häufigste Lösungs Antwort von der APP ist **erfolgreich**, was bedeutet, dass die APP mit den spezifischen Daten aus einem Parameter (z. b. dem Namen des Benutzer Bildschirms) mit einer Information übereinstimmt, die er kennt.

Es kann vorkommen, dass die APP sicherstellen muss, dass eine bestimmte Anforderung mit der richtigen Information übereinstimmt, die Sie kennt. In diesen Fällen sendet Sie eine **confirmationrequired** -Antwort, um dem Benutzer eine Ja-oder keine Frage zu stellen, z. b. *"Nachricht an Bobo das großartige senden"* .

Möglicherweise gibt es in anderen Fällen, in denen die APP den Benutzer benötigt, eine kurze Liste von Optionen auszuwählen. In diesem Fall stellt die APP eine mehrdeutigkeits Antwort mit einer Liste von zwei bis zehn Optionen bereit, aus denen der Benutzer auswählen kann:

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri verarbeitet den Benutzer, der die Auswahl treffen soll, entweder verbal oder durch Interaktion mit der Siri-Benutzeroberfläche, und das Ergebnis wird zurück an die APP gesendet.

In anderen Fällen sind möglicherweise nicht genügend Informationen vorhanden, um die APP zum Auflösen des Parameters zu verwenden, oder es können zu viele Übereinstimmungen vorhanden sein, die mithilfe von Mehrdeutigkeit aufgelöst werden sollen (z. b. 80 Benutzer mit Bobo in Ihrem Namen). In diesem Fall sendet die APP eine **needsmoredetails** -Antwort, und Siri fordert den Benutzer auf, spezifischer zu sein.

Wenn der Benutzer keinen Wert bereitgestellt hat, der für die Verarbeitung der Absicht erforderlich ist, kann er eine **needsvalue** -Antwort senden, damit Siri den Benutzer zur Eingabe des Werts auffordert.

Wenn die APP einen Wert nicht unterstützt, den der Benutzer für einen bestimmten Parameter angegeben hat, kann Sie die **unsupportedwithreason** -Antwort senden, um einen Grund anzugeben, warum der Wert nicht unterstützt wurde. Siri fordert dann den Benutzer zur Eingabe eines vollständig neuen Werts auf und gibt ihm den Grund, warum er erforderlich ist.

Verwenden Sie schließlich die **notrequired** -Antwort, um Siri mitzuteilen, dass die APP keinen Wert für einen bestimmten Parameter benötigt. Wenn der Benutzer dies trotzdem bereitstellt, wird er von Siri einfach ignoriert.

### <a name="the-confirm-stage"></a>Die Bestätigungsphase

Die Confirm-Phase besteht aus zwei Gründen:

- Um Siri das erwartete Ergebnis der Handhabung einer Absicht mitzuteilen, damit Siri dem Benutzer mitteilen kann, was passiert.
- Bietet eine Gelegenheit zum Überprüfen aller erforderlichen Zustände, die die APP möglicherweise benötigt, um die vom Benutzer bereitgestellte Anforderung abzuschließen, z. b. Wenn Sie über ausreichend Geld in der Bank zum Anfordern der angeforderten Zahlung verfügen.

Die APP stellt eine beabsichtigte **Antwort** aus dem Schritt "bestätigen" bereit, die mit so vielen Informationen aufgefüllt werden soll, wie die app verfügbar ist, sodass Siri Sie effektiv mit dem Benutzer kommunizieren kann.

Basierend auf dem Domänen-und Aktionstyp kann Siri den Benutzer zur Bestätigung auffordern, z. b. vor dem Senden einer Zahlung oder dem Buchen einer Fahrt.

### <a name="the-handle-stage"></a>Die Handle-Phase

Die Handle-Phase ist der wichtigste Teil der Arbeit mit einer Absicht, da die APP die Anforderung des Benutzers erfüllt, indem Sie die Aufgabe ausführt, die Sie angefordert hat.

Ebenso wie in der Confirm-Phase muss die APP so viele Informationen zum Ergebnis wie möglich bereitstellen, damit Siri dies dem Benutzer zuordnen kann. Manchmal werden diese Informationen visuell oder in anderen Fällen angezeigt, in denen Siri Sie einfach an den Benutzer zurückgibt.

Manchmal kann es vorkommen, dass die APP zusätzliche Zeit benötigt, um eine bestimmte Anforderung zu verarbeiten, z. b. Verzögerungen bei Netzwerk anrufen oder wenn eine LivePerson die Anforderung erfüllen muss (z. b. das abschließen und Versenden einer Bestellung oder das Fahren eines Fahrzeugs zum Speicherort des Benutzers). Wenn Siri auf eine Antwort von der APP wartet, wird dem Benutzer eine wartende Benutzeroberfläche angezeigt, die Sie darüber informiert, dass die APP die Anforderung verarbeitet.

Im Idealfall sollte die APP innerhalb von zwei bis drei Sekunden höchstens eine Antwort auf Siri bereitstellen. Wenn die APP weiß, dass die Verarbeitung einer bestimmten Antwort länger dauert, muss Sie einen **InProgress** -Antwort Code an Siri senden. Siri informiert den Benutzer dann, dass die APP die Anforderung im Hintergrund verarbeitet, und auch dann, wenn Sie die SIRI-Umgebung verlassen.

## <a name="adding-sirikit-to-the-app"></a>Hinzufügen von Sirikit zur APP

Mit dem Sirikit in ios 10 hat Apple zwei neue Erweiterungs Punkte erstellt:

- **Intents-Erweiterung** : gibt Siri den Inhalt der APP an und führt die Aufgaben aus, die erforderlich sind, um alle unterstützten Intents zu erfüllen.
- **Intents-Benutzeroberflächen Erweiterung** : stellt eine benutzerdefinierte Benutzeroberfläche bereit, die für den Inhalt der apps in Siri angezeigt wird.

Es gibt auch eine API zum Bereitstellen von Wörtern und Ausdrücken für Siri, um die Erkennung in Form von zu unterstützen:

- **App-Vokabulare** Wörter und Ausdrücke, die allen Benutzern der APP gemeinsam sind.
- **Benutzer Vokabular** : Wörter und Ausdrücke, die für einen bestimmten App-Benutzer eindeutig sind.

## <a name="the-intents-extension"></a>Intents-Erweiterung

Die Intents-Erweiterung ist für die Verarbeitung der Haupt Interaktionen zwischen der APP und Siri wie folgt verantwortlich:

[![](understanding-sirikit-images/intents01.png "Intents-Erweiterung")](understanding-sirikit-images/intents01.png#lightbox)

Die Intent-Erweiterung kann eine oder mehrere Intents unterstützen, es ist dem Entwickler zu entscheiden, wie Sie Sirikit in der APP implementieren möchten. Der Entwickler kann auch eine separate Intent-Erweiterung für jede Absicht hinzufügen, die behandelt werden muss.  Das heißt, Apple fordert an, dass der Entwickler die Anzahl der Intent-Erweiterungen einschränkt, damit Siri nicht über mehrere Prozesse verfügt, die für die APP geöffnet sind, die mehr Arbeitsspeicher und Zeit für die Handhabung benötigen.

Der Entwickler sollte außerdem beachten, dass die Intent-Erweiterung im Hintergrund ausgeführt wird, während Siri aktiv ist. Dadurch kann Siri aktiv eine Konversation mit dem Benutzer durchführen und gleichzeitig mit der Erweiterung kommunizieren, um Informationen über die Anforderung zu verarbeiten.

## <a name="privacy-and-security-considerations"></a>Überlegungen zu Datenschutz und Sicherheit

Apple hat hervorragend Maßnahmen ergriffen, um sicherzustellen, dass die privaten Informationen eines Benutzers bei der Arbeit mit Siri sicher sind. Daher gibt es mehrere Interaktionen, die erfordern, dass der Benutzer auf dem IOS-Gerät angemeldet ist. Dies ist beispielsweise der Fall, wenn Sie eine Fahrt anfordern oder eine Zahlung tätigen.

Außerdem gibt es bestimmte Verhalten, die die APP möglicherweise auf den Benutzer beschränken möchte, der beim Gerät angemeldet ist. In diesen Fällen kann die APP die **Einschränkung beim gesperrten** Verhalten anfordern. Dies erfolgt über eine Einstellung in der `Info.plist` Datei.

Das lokale Authentifizierungs Framework ist für die beabsichtigte Erweiterung verfügbar, sodass die APP den Benutzer zur weiteren Authentifizierung auffordern kann, auch wenn das Gerät bereits entsperrt ist.

Zum Schluss ist Apple Pay für die beabsichtigte Erweiterung verfügbar, sodass die APP eine Transaktion mithilfe Apple Pay ausführen kann, und das integrierte Apple Pay Blatt wird über der Siri-Schnittstelle angezeigt.

Außerdem möchte Apple sicherstellen, dass die Benutzer wissen, wann Sie Informationen an eine Drittanbieter-App senden, und dass der Benutzer den spezifischen Namen der APP angeben **muss** (wie im Paket anzeigen amen der APP angegeben), wenn er eine Anforderung sendet.

Apple hat SIRI entworfen, um natürliche, fließende Konversationen mit dem Benutzer durchzuführen, und aus diesem Grund kann der Paketname der app in vielen Teilen der Sprache verwendet werden, und zwar unabhängig davon, wo Sie sich auf natürliche Weise in der Anforderung des Benutzers einfügt.

Eine der häufigsten Aufgaben besteht darin, den Namen der APP zu "verbify", d. h., den APP-Namen zu übernehmen und ihn als Verb in einer Anforderung zu verwenden. Beispiel: *"monkeychat Bobo, das war großartig Bananen."*

## <a name="the-intents-ui-extension"></a>Die Intents-Benutzeroberflächen Erweiterung

Die Intents-Benutzeroberflächen Erweiterung bietet die Möglichkeit, die Benutzeroberfläche und das Branding der app in die Siri-Erfahrung zu bringen und die Benutzer mit der App verbunden zu gestalten. Mit dieser Erweiterung kann die APP die Marke sowie visuelle und andere Informationen in das Transkript einbringen.

[![](understanding-sirikit-images/intents02.png "Beispiel Intents-Benutzeroberflächen-Erweiterungs Ausgabe")](understanding-sirikit-images/intents02.png#lightbox)

Die Intents-Benutzeroberflächen Erweiterung gibt immer `UIViewController` eine zurück, und die APP kann alle Elemente hinzufügen, die Sie im Ansichts Controller anweist, z. b. zusätzliche Informationen, die über die erste Antwort hinausgehen. Die Intents-Benutzeroberfläche kann den Benutzer auch mit dem Status eines Ereignisses mit langer Laufzeit aktualisieren, z. b. wie viel länger eine Fahrt Freigabe Wagen hat, um Ihren Standort zu erreichen.

Die Intents-Benutzeroberflächen Erweiterung wird immer zusammen mit anderen Siri-Inhalten angezeigt, z. b. dem App-Symbol und dem Namen oben auf der Benutzeroberfläche, oder auf der Grundlage der Absicht werden möglicherweise Schaltflächen (z. b. "Senden" oder "Abbrechen") unten angezeigt.

Es gibt einige Fälle, in denen die APP die Informationen ersetzen kann, die von Siri standardmäßig für den Benutzer angezeigt werden, z. b. Nachrichten oder Zuordnungen, bei denen die APP die Standardbenutzer Anwendung ersetzen kann, die auf die APP zugeschnitten ist.

> [!IMPORTANT]
> Obwohl es möglich ist, interaktive Elemente wie `UIButtons` oder `UITextFields` zu den Intent UI- `UIViewController`Erweiterungen hinzuzufügen, sind diese als beabsichtigte Benutzeroberfläche in nicht interaktiv unzulässig, und der Benutzer ist nicht in der Lage, mit Ihnen zu interagieren.

Es ist völlig optional, dass die APP eine beabsichtigte Benutzeroberflächen Erweiterung bereitstellt, da Siri einen Standardsatz von Benutzeroberflächen für jeden Intent-Typ enthält. Außerdem sind die Intents UI-Schnittstellen nur für bestimmte Intents verfügbar, die von Apple als hilfreich für den Benutzer angesehen werden.

## <a name="adding-sirikit-vocabulary"></a>Hinzufügen von Sirikit-Vokabular

Der letzte Teil der Implementierung von "Sirikit" liegt in der app durch Bereitstellen des erforderlichen Vokabulars. Viele apps verfügen über einzigartige Möglichkeiten, Informationen für den Benutzer zu beschreiben, und es gibt eindeutige Möglichkeiten, wie der Benutzer der APP Informationen bereitstellt.

Aus diesem Grund benötigt Siri die Unterstützung der APP, um die Wörter und Ausdrücke zu verstehen, die für die APP eindeutig sind. Einige dieser Ausdrücke sind Teil der APP, sodass jeder Benutzer Sie kennt und versteht. Andere werden jedoch für einen bestimmten Benutzer der APP eindeutig sein.

### <a name="app-specific-vocabulary"></a>App-spezifisches Vokabular

Das App-spezifische Vokabular definiert die Wörter und Ausdrücke, die allen Benutzern der APP, z. b. Fahrzeugtypen oder Trainings Namen, bekannt sein werden. Da diese Teil der Anwendung sind, werden Sie als Teil des Haupt `AppIntentVocabulary.plist` App Bundle in einer Datei definiert. Außerdem sollten diese Wörter und Ausdrücke lokalisiert werden.

Eine Vokabulardatei `AppIntentVocabulary.plist` besteht aus mehreren Teilen:

- **Beispiel-App-Verwendung** : Diese stellen eine Reihe allgemeiner Anwendungsfälle für die Anforderungen bereit, die der Benutzer für die APP erstellen kann. Beispiel: *"Starten Sie ein Training mit monkeyfit".*
- **Parameter** : Diese stellen eine Reihe von nicht standardmäßigen Parametertypen bereit, die für die APP spezifisch sind. Beispielsweise Trainings Namen für die monkeyfit-app. Diese umfassen Folgendes:
  - **Ausdruck** : ermöglicht der APP, eindeutige Begriffe für die APP zu definieren. Beispiel: der "bananarific"-traintyp für die monkeyfit-app.
  - **Aussprache** : gibt die Ausrufe Hinweise an Siri als einfache phonetische Schreibweise für einen gegebenen Ausdruck an. Beispiel: "BA Nana RI FIC".
  - **Beispiel** : gibt ein Beispiel für die Verwendung des angegebenen Ausdrucks in der APP an. Beispiel: *"Start a bananarific in monkeyfit"* .

Weitere Informationen finden Sie in der APP- [vokabulardateiformatreferenz](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)von Apple.

### <a name="user-specific-vocabulary"></a>Benutzerspezifisches Vokabular

Das benutzerspezifische Vokabular dient zum Bereitstellen von Wörtern oder Ausdrücken, die für einzelne Benutzer der APP eindeutig sind. Diese werden zur Laufzeit von der Haupt-app (nicht von den App-Erweiterungen) als geordneter Satz von Begriffen bereitgestellt, die mit einer signifikantesten Nutzungs Priorität für die Benutzer geordnet sind, mit den wichtigsten Begriffen am Anfang der Liste.

Sehen Sie sich das Beispiel der oben dargestellten monkeychat-APP an. Monkeychat speichert eine Liste aller Kontakte des Benutzers, die über das benutzerspezifische Vokabular an Siri gesendet werden. Außerdem wird eine Liste der 10 neuesten Kontakte angezeigt, die für den Benutzer angezeigt werden, und er verfügt über eine Reihe von bevorzugten Kontakten für jeden Benutzer. In diesem Beispiel sollten sich die bevorzugten Kontakte am Anfang des benutzerspezifischen Vokabulars befinden, gefolgt von den aktuellen Kontakten und den restlichen Kontakten des Benutzers.

Die folgenden Informationstypen werden von einem benutzerspezifischen Vokabular unterstützt:

- Kontaktnamen.
- Trainings Namen.
- Foto-Album-Namen.
- Foto Schlüsselwörter.

Wenn die APP auf dem IOS-Adressbuch basiert, muss die APP keine Maßnahmen ergreifen, da diese Informationen bereits für Siri verfügbar sind. Die APP muss nur Kontaktnamen bereitstellen, wenn die APP über eine eigene, eindeutige Kontaktdatenbank verfügt.

Stellen Sie beim Entwerfen des Vokabulars nur die erforderlichen Werte bereit, die den Benutzern bekannt und wichtig sind. Vermeiden Sie die Angabe von Informationen wie Telefonnummern oder e-Mail-Adressen.

Die APP muss Siri auch umgehend aktualisieren, wenn das benutzerspezifische Vokabular geändert wird. Benutzer sind daran gewöhnt, Informationen von Siri anzufordern, sobald Sie zu Ihrem IOS-Gerät hinzugefügt wurden. Wenn der Benutzer beispielsweise einen neuen Kontakt in der APP hinzufügt, senden Sie diese Informationen an Siri, sobald Sie vom Benutzer gespeichert werden.

Noch wichtiger ist, dass die APP Informationen aus dem Siri-Vokabular umgehend löschen _muss_ , da ein Benutzer verärgert sein könnte, wenn er eine Information gelöscht hat, aber Siri noch Stunden oder Tage später erkannt hat.

> [!IMPORTANT]
> Die APP sollte alle benutzerspezifischen Vokabulare aus Siri entfernen, wenn der Benutzer sich für das Zurücksetzen der APP entscheidet oder wenn Sie sich abmelden.

## <a name="sirikit-permissions"></a>Sirikit-Berechtigungen

Der letzte Teil von Sirikit ist um Berechtigungen herum zentriert. Wie bei anderen Features von IOS (z. b. Fotos, Kameras oder Kontakten) müssen Benutzer der APP explizit die Berechtigung erteilen, mit Siri zu kommunizieren.

Die APP kann eine Zeichenfolge bereitstellen, die festlegt, welche Informationen für Siri bereitgestellt werden, und gibt einen Grund dafür an, warum der Benutzer diesen Zugriff gewähren sollte.

Apple schlägt vor, dass die APP die Berechtigung des Benutzers für die Verwendung von Siri anfordern soll, wenn der Benutzer die APP zum ersten Mal öffnet, nachdem er auf IOS 10 aktualisiert wurde. So wissen die Benutzer über die Siri-Integration und können die Nutzung vorab genehmigen, bevor Sie Ihre erste Anforderung stellen.

## <a name="sirikit-and-maps"></a>Sirikit und Maps

Sirikit ist ein integraler Bestandteil von IOS und nutzt das größere Intents-Framework, das IOS 10 hinzugefügt wird. Das Intents-Framework wurde entwickelt, um gemeinsame und gemeinsam genutzte Aktionen und Intents mit anderen Teilen des Systems gemeinsam zu nutzen.

Das Intents-Framework geht über die Siri-Integration hinaus und bietet weitere Features, wie z. b. die Integration von Kontakten, bei denen die APP als Standard telefonieapp oder Messaging-App für bestimmte Kontakte Intents bieten außerdem eine umfassende Integration in callkit, um Benutzern die beste VoIP-Darstellung zu bieten.

Die Maps-app in ios 10 hat Features wie die Fahrt Freigabe hinzugefügt, mit denen der Benutzer direkt in der kartenbenutzer Oberfläche eine Fahrt buchen kann. Das Sirikit bietet einen gemeinsamen Erweiterungs Punkt mit Maps, sodass Fahrt Freigabe (und andere) Intents von Siri und Maps gemeinsam genutzt werden können.

Dies bedeutet, dass, wenn die APP die Sirikit-Erweiterungen übernommen hat, auch die Maps-Integration kostenlos ist.

## <a name="designing-a-great-siri-experience"></a>Entwerfen einer hervor artigen Siri-Darstellung

Das Entwerfen einer hervorragend Benutzeroberfläche bei der Integration einer APP in Siri unterscheidet sich vom Entwerfen einer großartigen App-Benutzeroberfläche. Im Gegensatz zu normalen Situationen, in denen der Benutzer direkt auf dem Bildschirm mit der APP interagiert, gibt es bei Verwendung von Siri viele Male, wenn überhaupt keine visuelle Oberfläche sichtbar ist. Beispielsweise, wenn der Benutzer die Konversation mit *"Hey Siri"* gestartet hat.

### <a name="how-siri-helps-the-developer"></a>Wie Siri dem Entwickler hilft

Beim Entwerfen der Interaktionen einer APP mit Siri erstellt die APP eine *Konversation-Schnittstelle*. Dies bedeutet, dass der Kontext von der Konversation abgeleitet ist, die Siri mit dem Benutzer im Auftrag der APP aufweist.

Wenn kein visueller Verweis vorhanden ist, muss der Benutzer die Informationen nachverfolgen, die im Kopf angezeigt werden. Aus diesem Grund stellt Siri die minimalen Informationen dar, die erforderlich sind, um die Aufgabe zu erfüllen, die der Benutzer ausführen möchte.

Die Konversation-Schnittstelle wird durch die Fragen und Antworten von Benutzern und Siri während der Konversation strukturiert. Daher ist es wichtig zu berücksichtigen, wie Siri Fragen stellt und antwortet, wenn diese Schnittstelle entworfen wird.

Nehmen Sie im folgenden Beispiel an, dass der Benutzer, der eine Nachricht erstellt, mit der Frage *"bereit zum Senden"* antwortet. Der Benutzer kann auf viele verschiedene Arten Antworten, z. b. *"senden*, *Abbrechen* " oder sogar etwas, das mit dieser Frage nicht verknüpft ist. Unabhängig davon, wie die Konversation abgespielt wird, verarbeitet Siri Sie für die APP und sendet Ihnen nur die relevanten Informationen, sobald Sie verfügbar wird.

Es gibt verschiedene Möglichkeiten, wie ein Benutzer eine Konversation mit Siri initiieren kann:

- Wenn Sie das Gerät auswählen, klicken Sie auf die Start Schaltfläche. In dieser Situation stellt Siri mehr visuelle Schnittstellen und weniger verbale Antworten dar.
- Indem Sie *"Hey Siri"* sagen und eine Hand freie Konversation beginnen. In dieser Situation ist Siri weniger visuell und eher verbal.
- Verwendung von Barrierefreiheits Funktionen wie Bluetooth-aktivierten Hörhilfen, bei denen die Benutzeroberfläche für einen Benutzer mit besonderen Anforderungen angepasst wird.
- Die Verwendung von Auto spielen, bei denen der Benutzer ihre Aufmerksamkeit behalten muss, konzentriert sich auf die Verwendung von Ablenkungen.

### <a name="how-the-developer-helps-siri"></a>Unterstützung von Siri durch den Entwickler

Wenn Sie eine app in Siri integrieren, muss der Entwickler diese Integration häufig testen und sicherstellen, dass Sie viele verschiedene Anforderungen ausführen, indem er die gleichen Informationen oder Aufgaben auf so viele verschiedene Arten wie möglich anfordert.

Da keine zwei Personen gleich sind, ist es wichtig, dass der Entwickler so viele verschiedene Betatester wie möglich erhält, um die Siri-Integration zu optimieren. Benutzer Fragen sich möglicherweise nach Informationen oder stellen Anforderungen auf eine Weise ein, die der Entwickler niemals hat. Diese Feinabstimmung kann dabei helfen, sicherzustellen, dass die breiteste Gruppe von Benutzern eine gute benutzerfreundliche Benutzeranwendung mit Siri verwendet.

Testen Sie in unterschiedlichen Situationen und Umgebungen. Initiieren Sie die Konversationen mit Siri auf alle möglichen Möglichkeiten, um sicherzustellen, dass diese Konversationen fließend und natürlich bleiben. Testen Sie an Standorten, an denen der Benutzer die APP eher wie in einem überfüllten Fitnessgerät verwenden würde.

Stellen Sie sicher, dass die APP alle Informationen bereitstellt, die Siri benötigt, damit die Anforderung und das Ergebnis für den Benutzer ordnungsgemäß dargestellt werden. Dies trifft vor allem dann zu, wenn SIRI in einer praktischen Situation genutzt wird.

### <a name="siri-design-guidelines"></a>Siri-Entwurfs Richtlinien

Denken Sie immer daran, dass Siri über eine Konversation mit dem Benutzer im Auftrag der App verfügt. Der Entwickler möchte sicher sein, dass diese Konversation so reibungslos und natürlich wie möglich bleibt.

Wie bei jeder wichtigen Konversation muss der Entwickler Folgendes sicherstellen:

- , Dass die APP für die Konversation vorbereitet ist.
- Die APP lauscht genau darauf, was der Benutzer zu erreichen versucht.
- Die APP fragt die entsprechenden Fragen zu den entsprechenden Zeiten ab.
- , Dass die APP auf die Anforderung mit den vom Benutzer gewünschten Informationen antwortet.

#### <a name="preparing-for-the-conversation"></a>Vorbereiten der Konversation

Als erstes sollten Sie daran denken, dass die Benutzer der APP nicht genau wie der Entwickler arbeiten. Sie können aus unterschiedlichen Hintergründen stammen, verschiedene Sprachen sprechen oder besondere Anforderungen haben, wenn Sie mit der APP arbeiten.

Seit dem Entwickler, der die APP entworfen und erstellt hat, haben Sie außerdem Tiefe, intime Kenntnisse der APP und ihrer inneren Abläufe und Funktionen, die ein typischer Benutzer nicht hat. Der Entwickler kann die Anforderung von Siri anders als ein normaler Benutzer anfordern.

Aus diesem Grund ist es wichtig, dass so viele verschiedene Personen wie möglich über Siri mit der APP interagieren. Benutzer können mithilfe von Siri Anforderungen an die APP stellen, die der Entwickler nie in Erwägung gezogen hat.

#### <a name="ensure-the-app-is-a-good-listener"></a>Stellen Sie sicher, dass die APP ein guter Listener ist.

Der Entwickler muss sicherstellen, dass die APP ein guter Listener ist und die Besonderheiten der Konversation erhält, die die Erwartungen der Benutzer erfüllen. Es ist jedoch auch möglich, dass Sie möglicherweise nicht alle Informationen bereitgestellt haben, die die APP zum Erreichen der angeforderten Aufgabe benötigt.

Es gibt mehrere Möglichkeiten, wie die APP diese Situation verarbeiten kann:

- **Wählen Sie einen guten Standardwert für den fehlenden Wert** aus, z. b. kann eine Fahrt Freigabe-App standardmäßig auf den aktuellen Speicherort des Benutzers festgelegt werden, wenn Sie nicht angeben, von wo Sie abgerufen werden sollen.
- **Erstellen Sie einen fundierten** Schätzwert, der bestimmte Informationen verwendet, die die APP für den Benutzer gesammelt hat. die APP ist möglicherweise in der Lage, die fehlenden Informationen zu erraten, z. b. das Ausfüllen einer fehlenden mobilen Nummer aus den Kontaktinformationen des Benutzers. Sie sollten jedoch darauf achten, dass Sie unerwartete Überraschungen vermeiden, wie z. b. das Auswählen der teuersten Option usw.
- **Aufforderung zur Eingabe von weiteren Informationen** : die APP kann den Benutzer von Siri auffordern, den fehlenden Wert einzugeben. Der Schlüssel hier besteht jedoch darin, die Konversationen einfach und an den Punkt zu halten. Benutzer werden schnell frustriert, wenn Sie mehrere Fragen beantworten müssen, um Ihre Anforderung zu erfüllen.
- Ordnungsgemäßes **Behandeln von fehl** Informationen: der Benutzer kann einen Wert angeben, der von der APP nicht erwartet wurde oder der nicht im angegebenen Kontext verarbeitet werden kann. Stellen Sie sicher, dass die APP diese Situation für den Benutzer auf eine Weise verknüpft, mit der es klar und leicht zu korrigieren ist.

Wenn die APP mit einem einzelnen fraglichen Wert angezeigt wird, ist die bevorzugte Methode zur Behandlung dieses Vorgangs, dass Siri den Benutzer zur Bestätigung auffordert. Beispiel: *"Meinten Sie das toll?"* , mit dem Sie Antworten können, indem Sie eine einfache Ja-oder Nein-Antwort erhalten.

Wenn es eine Situation gibt, in der mehrere mögliche Optionen für einen einzelnen Wert korrekt sein könnten, ist die Eindeutigkeit die bevorzugte Behandlungsmethode. In dieser Situation kann Siri den Benutzer zur Auswahl von bis zu zehn möglichen Optionen auffordern. Beispiel:

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Wenn Sie noch immer dabei sind, muss Siri den Benutzer auffordern, eine völlig neue, spezifischere Antwort für einen bestimmten Wert bereitzustellen.

#### <a name="request-final-confirmation"></a>Endgültige Bestätigung anfordern

Bevor die APP tatsächlich die Aufgabe ausführt, um die Anforderung des Benutzers zu erfüllen, prüft Siri die APP-Erweiterung, um sicherzustellen, dass alles vorhanden ist. Verfügt der Benutzer z. b. über ausreichende Kosten in seinem Konto, um die angeforderte Zahlung zu tätigen?

Darüber hinaus muss die APP sicherstellen, dass Sie alle möglichen Informationen für Siri bereitstellt, damit Sie dem Benutzer präsentiert und bestätigt werden kann, dass die durchzuführende Aufgabe Ihren Erwartungen entspricht.

Nachdem der Benutzer die Anforderung bestätigt hat und die APP ihn ausgeführt hat, muss die APP erneut sicherstellen, dass Sie alle Ergebnisse an Siri zurückgegeben hat, damit Sie Sie mit dem Benutzer verknüpfen können.

#### <a name="responding-to-the-request"></a>Antworten auf die Anforderung

Siri verfügt über mehrere integrierte Benutzeroberflächen für jede der Domänen und Aktionen, die Sie kennen. Allerdings kann die APP ggf. eine benutzerdefinierte Intent-Benutzeroberflächen Erweiterung bereitstellen, um die Benutzeroberfläche zu erweitern, indem Sie das Branding und die Benutzeroberfläche der APP oder mehr Informationen bereitstellen, als in der Anforderung vorhanden waren.

Dies bedeutet, dass die Unterhaltung beim Entwerfen von benutzerdefinierten Schnittstellen für Siri verwendet werden sollte. Normalerweise möchte der Benutzer eine bestimmte Aufgabe so schnell wie möglich ausführen und sollte nicht mit unnötigen Informationen überladen werden.

Außerdem sollten Sie sicherstellen, dass die benutzerdefinierte Benutzeroberfläche auf allen IOS-Geräten und-Ausrichtungen, die der Benutzer möglicherweise hat oder das Gerät verwendet, korrekt aussieht und antwortet.

Verwenden Sie bei Bedarf die Sirikit-API, um redundante Informationen auszublenden, die bereits in der standardmäßigen Siri-Benutzeroberfläche vorhanden sind Stellen Sie jedoch sicher, dass die APP immer noch die Informationen für Siri bereitstellt, damit Sie Sie in einer Hand freien Situation in Hand haben können.

Es kann vorkommen, dass Siri die APP startet, um die Anforderung des Benutzers zu erfüllen, z. b. das darstellen der Fotos, die der Benutzer angefordert hat. In diesen Situationen überraschen Sie den Benutzer nicht. Zeigen Sie die erwarteten Informationen an, ohne dass Zwischenschritte oder eine weitere Interaktion erforderlich ist. Niemals Informationen anzeigen oder eine Aufgabe ausführen, die der Benutzer nicht erwartet.

### <a name="polishing-the-design"></a>Entwerfen des Entwurfs

Es gibt mehrere Schritte, die Apple vorschlagen soll, den Entwurf der konverterenden Schnittstellen zu polieren. Zuerst wird durch die Bereitstellung eines eindeutigen, präzisen vokabularvokabulars und von Anwendungsfällen für Siri.

Eine der Methoden, mit denen ein Benutzer die APP ermittelt, besteht darin, eine Konversation mit Siri zu initiieren und die Frage zu *stellen, was Sie tun können* . Siri zeigt mehrere verschiedene Dinge, die Sie ausführen können, einschließlich der APP des Entwicklers und der von ihr `plist` bereitgestellten Beispiel-Anwendungsfälle.

So schreiben Sie gute Anwendungsfälle:

- Stellen Sie sicher, dass die Beispiele den APP-Namen enthalten.
- Halten Sie das Beispiel kurz und auf den Punkt.
- Geben Sie mehrere Beispiele für die einzelnen Intents an, die von der App unterstützt werden.
- Priorisieren Sie die Intents und die darin enthaltenen Beispiele basierend auf den gängigsten Anwendungsfällen für die app.
- Stellen Sie sicher, dass die APP lokalisierte Beispiele enthält.
- Stellen Sie sicher, dass jedes angegebene Beispiel in der APP erwartungsgemäß funktioniert.
- Vermeiden Sie die Adressierung von Siri in den Beispielen. Fügen Sie daher keinen Text wie *"Hey Siri..." ein.*
- Vermeiden Sie unnötige Genüsse, wie z. b. *"bitte"* oder *"vielen Dank*".

Nehmen Sie sich die geeignete Zeit, um zu untersuchen und zu experimentieren, wie die APP die Konversation, die Siri hat, mit dem Benutzer in Ihrem Namen gestalten kann. Stellen Sie sicher, dass Sie im gesamten Prozess mit typischen Benutzern kommunizieren, da sich ihre Interaktionen mit und den Erwartungen der APP im Laufe der Zeit ändern können.

Denken Sie immer daran, die app in unterschiedlichen Situationen und allen unterschiedlichen Methoden zum Aufrufen einer Konversation mit Siri zu testen. Testen Sie an realen Orten, wo die Benutzer die APP verwenden, und nicht im Büro und Desk.

Möchten Sie, dass die Konversationen mit Siri (im Auftrag der APP) fließend, natürlich und "gut wahr" sind.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die wesentlichen Konzepte erläutert, die für die Verwendung von Sirikit erforderlich sind, und es wird gezeigt, dass es mit den xamarin. IOS-apps interagieren kann, um Dienste bereitzustellen, auf die der Benutzer mithilfe von Siri und der Maps-APP auf einem IOS




## <a name="related-links"></a>Verwandte Links

- [Elizachat-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [Sirikit-Programmier Handbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Intents Framework-Referenz](https://developer.apple.com/reference/intents)
- [Intents UI-frameworkreferenz](https://developer.apple.com/reference/intentsui)
