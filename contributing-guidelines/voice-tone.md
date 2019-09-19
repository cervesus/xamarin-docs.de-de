---
ms.openlocfilehash: 4d045c0025effae4f5e4cd30e03f8776be0c1270
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105752"
---
# <a name="voice-and-tone-guidelines"></a>Richtlinien zu Sprache und Schreibstil

Entwickler lesen Ihre Dokumentation, um Xamarin kennenzulernen und bei der täglichen Arbeit zu verwenden.
Ziel ist es, eine sinnvolle Dokumentation zu erstellen, die den Leser bei seiner Arbeit unterstützt. Hierbei helfen unsere Richtlinien. Unsere Stilvorgaben enthalten vier Empfehlungen:

- [Verwenden Sie einen umgangssprachlichen Ton](#use-a-conversational-tone)
- [Schreiben Sie in zweiter Person](#write-in-2nd-person)
- [Verwenden Sie Aktivkonstruktionen](#use-active-voice)
- [Setzen Sie das Leseniveau der 5. Klasse voraus](#target-a-fifth-grade-reading-level)

Beispiele hierfür finden Sie in diesen Stilvorgaben. Wir haben diese Stilvorgaben gemäß unseren Richtlinien geschrieben, um Ihnen auf diese Weise Beispiele zum Erstellen einer Dokumentation für Xamarin bereitzustellen. Darüber hinaus enthalten diese Stilvorgaben auch Gegenbeispiele, sodass Sie sehen, wie Artikel aussehen, wenn die Richtlinien nicht eingehalten werden.

## <a name="details-on-each-guideline"></a>Details zu den einzelnen Richtlinien

### <a name="use-a-conversational-tone"></a>Verwenden Sie einen umgangssprachlichen Ton

#### <a name="appropriate-style"></a>Angemessener Stil

Wir möchten, dass unsere Dokumentation in einem umgangssprachlichen Ton verfasst wird. Beim Lesen der Tutorials und Erläuterungen muss für Sie als Leser der Eindruck entstehen, Sie würden sich mit dem Autor unterhalten.
Der Text muss locker und informativ geschrieben sein. Für Sie als Leser muss der Eindruck entstehen, Sie würden zuhören, wie der Autor die Konzepte erläutert.

#### <a name="inappropriate-style"></a>Unangemessener Stil

Der Gegensatz zwischen einem umgangssprachlichen Ton und dem Ton, in dem wissenschaftliche Abhandlungen über technische Themen verfasst sind, ist leicht zu erkennen. Keine Frage, diese Ressourcen sind sehr hilfreich. Jedoch haben die Autoren diese Artikel im Vergleich zu unserer Dokumentation in einem völlig anderen Stil geschrieben. Wissenschaftliche Zeitschriften werden in einem völlig anderen Ton und Schreibstil verfasst.
Dem Leser werden trockene Beschreibungen trockener Themen präsentiert.

Im ersten Abschnitt wurden die Empfehlungen für einen umgangssprachlichen Ton befolgt. Im zweiten ein eher wissenschaftlicher Ton. Der Unterschied ist sofort zu erkennen.

### <a name="write-in-second-person"></a>Schreiben Sie in der zweiten Person

#### <a name="appropriate-style"></a>Angemessener Stil

Schreiben Sie Ihre Artikel so, als würden Sie direkt mit dem Leser sprechen. Verwenden Sie so oft wie möglich die zweite Person (so wie in diesen beiden Sätzen). Das Schreiben in der zweiten Person bedeutet nicht immer, dass Sie das Wort „Sie“ verwenden. Sprechen Sie den Leser direkt an. Schreiben Sie Aufforderungssätze.
Teilen Sie Ihren Lesern mit, was Sie ihnen beibringen möchten.

#### <a name="inappropriate-style"></a>Unangemessener Stil

Ein Autor kann auch in der dritten Person schreiben. Dabei muss er ein Pronomen oder Nomen suchen, mit dem er sich auf den Leser bezieht. Für den Leser ist dieser Schreibstil in der dritten Person meist weniger ansprechend und weniger unterhaltsam zu lesen.

Im ersten Abschnitt wurden unsere Stilempfehlungen befolgt. Im zweiten wurde die dritte Person verwendet. Bitte schreiben Sie in der zweiten Person. Sie fanden diesen Abschnitt bestimmt viel einfacher zu lesen.

### <a name="use-active-voice"></a>Verwenden Sie Aktivkonstruktionen

Schreiben Sie Ihre Artikel im Aktiv. Aktiv bedeutet, dass das Subjekt des Satzes die Aktion (Verb) dieses Satzes durchführt. Im Gegensatz dazu steht das Passiv. Hier wird der Satz so angeordnet, dass mit dem Subjekt im Satz etwas geschieht. Vergleichen Sie diese beiden Beispiele:

> Der Compiler hat den Quellcode in eine ausführbare Datei transformiert.

> Der Quellcode wurde vom Compiler in eine ausführbare Datei transformiert.

Der erste Satz ist im Aktiv geschrieben. Im zweiten Satz wurde eine Passivkonstruktion verwendet.
(Diese beiden Sätze sind ein weiteres Beispiel für die beiden Stilrichtungen.)

Wir empfehlen Aktivkonstruktionen, weil sie sich leichter lesen lassen. Passivkonstruktionen können schwieriger zu lesen sein.

### <a name="target-a-fifth-grade-reading-level"></a>Setzen Sie das Leseniveau der 5. Klasse voraus

Diese letzte Richtlinie haben wir aufgestellt, weil viele unserer Leser keine Muttersprachler sind.
Mit Ihren Artikeln erreichen Sie eine international Zielgruppe. Denken Sie daran, dass diese möglicherweise nicht über denselben Wortschatz verfügt wie Sie.

Dennoch schreiben Sie für technische Experten. Sie können davon ausgehen, dass Ihre Leser über Programmierkenntnisse verfügen und spezifische Programmierterminologie beherrschen. Objektorientierte Programmierung, Klasse und Objekt, Funktion und Methode sind bekannte Begriffe. Aber nicht jeder Leser Ihrer Artikel verfügt über einen formalen Abschluss in Informatik. Ausdrücke wie „idempotent“ sollten vor der Verwendung besser erläutert werden:

> Die `Close()`-Methode ist idempotent, d.h. dass Sie sie zwar mehrmals aufrufen können, der Effekt jedoch derselbe ist, als wenn Sie sie nur einmal aufrufen würden.
