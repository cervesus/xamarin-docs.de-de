---
title: Lokalisierung der Anwendungs Benutzeroberfläche
description: In diesem Dokument werden die plattformübergreifenden Konzepte der Internationalisierung und Lokalisierung beschrieben und erläutert, wie sich diese auf das Anwendungsdesign auswirken.
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: b7dfeee92020be2fb40cfdfc5eb1b97d065b97e9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758177"
---
# <a name="localization"></a>Lokalisierung

Dieses Handbuch bietet eine Einführung in die Konzepte hinter *Internationalisierung* und *Lokalisierung* und enthält Links zu Anleitungen zum Entwickeln von mobilen xamarin-Anwendungen mithilfe dieser Konzepte.

Wenn Sie die technischen Details der Lokalisierung von xamarin-apps direkt überspringen möchten, beginnen Sie mit einem der folgenden plattformspezifischen Anleitungen:

- [**Xamarin. Forms**](~/xamarin-forms/app-fundamentals/localization/index.md) plattformübergreifende Lokalisierung mithilfe von RESX-Dateien.
- Native [**xamarin. IOS**](~/ios/app-fundamentals/localization/index.md) -Platt Form Lokalisierung.
- Native [**xamarin. Android**](~/android/app-fundamentals/localization.md) -Platt Form Lokalisierung.

## <a name="i18n-and-l10n"></a>i18n und l10n

*Internationalisierung* ist der Prozess, mit dem Ihr Code verschiedene Sprachen anzeigen und die Anzeige für verschiedene Gebiets Schemas anpassen kann (z. b. die Zahlen-und Datums Formatierung). Dies wird auch als *Globalisierung*bezeichnet.

Die *Lokalisierung* ist der folgende Schritt – Erstellen von Ressourcen (z. b. Zeichen folgen und Images) für jede Sprache und bündeln der APP mit der Internationalize-app.

Internationalisierung wird häufig zu i18n – Kurzform und 18 Buchstaben zwischen "i" und "n" gekürzt. Die Lokalisierung wird auf ähnliche Weise auf l10n verkürzt – für 10 Buchstaben zwischen "L" und "n".

## <a name="overview"></a>Übersicht

Dieses Dokument enthält eine Einführung in die Konzepte der Internationalisierung und Lokalisierung und deren Anwendung auf die Entwicklung mobiler Anwendungen im Allgemeinen.
Beim Entwerfen und entwickeln einer Anwendung sind möglicherweise bereits hart codierte Elemente vorhanden, die für die Lokalisierung parametrisiert werden müssen:

- Bildschirmlayouts und Text
- Symbole, Grafiken und Farben
- Video-und Audiodateien,
- Dynamischer Text und Textformatierung (z. b. Zahlen, Währungen und Datumsangaben)
- Layoutänderungen für rechts-nach-links (RTL)-Sprachen und
- Sortieren von Daten.

Unabhängig davon, auf welchen mobilen Plattformen Ihre APP diese Tipps als Ziel hat, können Sie eine hochwertige Lokalisierte app erstellen.

## <a name="design-considerations"></a>Entwurfsüberlegungen

Das Entwerfen einer Anwendung, sodass Ihre Inhalte lokalisiert werden können, wird als Internationalisierung bezeichnet. Die ordnungsgemäße Durchführung von Internationalisierung besteht darin, dass unterschiedliche sprach Zeichenfolgen zur Laufzeit geladen werden können – eine gut entworfene app sollte es Ihnen ermöglichen, alle Ressourcen basierend auf Sprache und Gebiets Schema (einschließlich Bildern, Sounds und Videos) zu ändern. Formatierung und Layout für die Bewältigung unterschiedlicher Zeichen folgen.

In diesem Abschnitt werden einige Entwurfs Überlegungen erläutert, die beim Erstellen einer internationalisierten Anwendung berücksichtigt werden müssen.

### <a name="layouts-and-string-length"></a>Layouts und Zeichen folgen Länge

Chinesische und japanische Zeichen folgen können sehr kurz sein – manchmal kann ein oder zwei Zeichen für eine Eingabefeld Bezeichnung sinnvoll sein.

Deutsche Zeichen folgen (z. b.) können sehr lang sein. Manchmal wird ein relativ kurzes Wort in englischer Sprache in anderen Sprachen sehr lange dauern – entweder werden Sie abgeschnitten, oder Sie werden Ihr Layout unerwartet wieder fließen.

Vergleichen Sie die Zeichen folgen Längen für einige Elemente auf dem IOS-Startbildschirm auf Englisch, Deutsch und Japanisch:

[![](localization-images/language-compare-sml.png "Deutsch und Japanisch Zeichen folgen Länge")](localization-images/language-compare.png#lightbox)

Beachten Sie, dass die **Einstellungen** in Englisch (8 Zeichen) 13 Zeichen für die deutsche Übersetzung, aber nur 2 Zeichen in Japanisch erfordern.

Layouts, bei denen die Anzeige Bezeichnung und das Eingabefeld nebeneinander sind, sind schwer zu bearbeiten, wenn die Bezeichnungs Länge stark variieren kann. Häufig ist ein Layout, in dem die Bezeichnung oberhalb eines Felds angezeigt wird, einfacher zu lokalisieren, da die gesamte Breite des Bildschirms sowohl für die Bezeichnung als auch für die Eingabe verfügbar ist.

Als allgemeine Regel gilt: Wenn Sie fixierte Layouts (besonders parallele Elemente) entwickeln, lassen Sie mindestens 50% mehr Breite zu, als die englischen Zeichen folgen für Bezeichnungen und Text benötigen. Dadurch wird nicht jedes Problem gelöst, sondern es wird ein Puffer bereitgestellt, der in vielen Fällen funktioniert.

### <a name="input-validation"></a>Eingabevalidierung

Beachten Sie die Annahmen beim Schreiben von Validierungsregeln. Es ist möglicherweise gültig, dass eine Textfeld Eingabe erforderlich ist, um mindestens drei Zeichen in englischer Sprache anzufordern, da ein einzelner Buchstabe sehr selten eine Bedeutung hat. In Chinesisch und Japanisch kann ein einzelnes Zeichen jedoch eine gültige Eingabe sein, und eine Validierungs Meldung "mindestens 3 Zeichen ist erforderlich" ist für diese Sprachen nicht sinnvoll.

Andere scheinbar einfache Aufgaben wie das Überprüfen einer e-Mail-Adresse oder Website-URL werden komplizierter, da die Zeichen nicht auf die ASCII-Teilmenge beschränkt sind.

Schreiben Sie Ihre Validierungsregeln bei der Internationalisierung – wählen Sie entweder die am wenigsten einschränkenden Regeln aus, oder schreiben Sie die Logik, damit Sie für jede Sprache unterschiedlich funktioniert.

### <a name="images-and-color"></a>Bilder und Farbe

Nicht jedes Image muss basierend auf der Sprachauswahl eines Benutzers geändert werden. Viele Symbole oder Fotos sind für alle Benutzer geeignet, unabhängig von der Sprache, in der Sie gesprochen werden.
Einige Ressourcen sind jedoch für die Lokalisierung von Bedeutung, wie z. b.:

- Images, die Personen oder bestimmte Orte darstellen – Ihre APP mag für Benutzer relevanter sein, wenn Sie lokale Personen/Orte anzeigt.
- Symbole – einige Ikonographie können Kultur spezifisch sein, und Sie können die Verwendung Ihrer APP vereinfachen, indem Sie die Bilder lokalisieren, um das lokale Verständnis widerzuspiegeln.
- Farben – einige Kulturen verstehen Farben anders – Rot bedeutet möglicherweise eine Warnung in einer Region, aber ein gutes Glück in einer anderen Region. Informieren Sie sich beim Entwerfen Ihrer APP mit systemeigenen Referenten, um zu bestimmen, ob Sie einen Mechanismus zum Lokalisieren von Farben aufbauen sollten.

### <a name="videos-and-sound"></a>Videos und Sound

Videos und Sound sind besondere Herausforderungen bei der Lokalisierung einer Anwendung, denn während es relativ einfach ist, Zeichen folgen zu übersetzen, kann das Aufzeichnen mehrerer VoiceOver-Spuren oder Videoclips sowohl aufwendig als auch schwierig sein.

Mehrere Kopien von Video-und Audiodateien können auch die Größe Ihrer Anwendung erheblich erhöhen (insbesondere, wenn Sie eine Lokalisierung in eine große Anzahl von Sprachen haben oder viele Mediendateien haben). Sie können in Erwägung gezogen, dass Sie nur die erforderlichen Sprachressourcen herunterladen, nachdem der Benutzer die APP installiert hat. Dies kann jedoch auch zu einem schlechten Benutzer Verhalten in langsamen Netzwerken führen.

Es gibt oftmals mehrere Möglichkeiten, Lokalisierungs Probleme zu lösen – das wichtigste dabei ist, Sie vorab in Erwägung zu ziehen und sicherzustellen, dass Ihre Anwendung darauf ausgelegt ist.

### <a name="dates-times-numbers-and-currency"></a>Datumsangaben, Uhrzeiten, Zahlen und Währungen

Wenn Sie .net-Formatierungsfunktionen verwenden, denken Sie daran, die Kultur anzugeben, sodass dezimale Trennzeichen ordnungsgemäß analysiert werden (und vermeiden, dass Konvertierungs Ausnahmen ausgelöst werden). Beispielsweise sind 1,99 und 1, 99, abhängig von Ihrem Gebiets Schema, gültige Dezimal Darstellungen.

Wenn die Daten aus einer bekannten Quelle stammen (d. h. aus Ihrem eigenen Code oder einem von Ihnen kontrollierten Webdienst), können Sie einen Kultur Bezeichner hart codieren, der der Formatierung entspricht, z. b. der InvariantCulture, die für die Standard Formatierung in englischer Sprache funktioniert.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Wenn die Daten vom App-Benutzer eingegeben werden, analysieren Sie Sie mithilfe einer CultureInfo-Instanz, die ihr Gebiets Schema wieder gibt:

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Weitere Informationen finden Sie in den MSDN-Artikeln zum Auswerten [numerischer](https://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx) Zeichen folgen und zum Auswerten von [Datums-und Uhrzeit](https://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx) Zeichenfolgen.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Rechts-nach-links (RTL)-Sprachen

Einige Sprachen, z. b. Arabisch, Hebräisch und Urdu (z. b.), werden von rechts nach links gelesen.
Anwendungen, die diese Sprachen unterstützen, sollten Bildschirm Entwürfe verwenden, die für Leser von rechts nach links angepasst werden, z. b.:

- Der Text muss rechtsbündig ausgerichtet werden.
- Bezeichnungen sollten rechts von Eingabefeldern angezeigt werden.
- Die Standard Platzierung der Schaltfläche wird im allgemeinen umgekehrt.
- Hierarchische Navigations Streifen und Animationen (und andere Navigations Metaphern und Animationen), die die Richtung für den Kontext verwenden, sollten ebenfalls umgekehrt werden.

Sowohl IOS als auch Android unterstützen Layouts von von rechts nach links und Schriftart Rendering mit integrierten Features, mit deren Hilfe Sie die oben beschriebenen Anpassungen vornehmen können. Xamarin. Forms unterstützt derzeit nicht automatisch das RTL-Rendering.

### <a name="sorting"></a>Sortieren

In verschiedenen Sprachen wird die Sortierreihenfolge ihrer Alphabete anders definiert, auch wenn Sie denselben Zeichensatz verwenden.

Sehen Sie sich die [Details zum Vergleich von Zeichen](https://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) folgen in [bewährte Methoden für die Verwendung](https://msdn.microsoft.com/library/dd465121(v=vs.110).aspx) von Zeichen folgen im .NET Framework für ein Beispiel an, in dem Sprache (CultureInfo) die Sortierreihenfolge beeinflusst

Es ist unwahrscheinlich, dass die integrierten Datenbankfunktionen auf den mobilen Plattformen die sprachspezifische Sortierreihenfolge unterstützen, sodass Sie möglicherweise zusätzlichen Code in Ihrer Geschäftslogik implementieren müssen.

### <a name="text-search"></a>Textsuche

Stellen Sie sicher, dass Sie Ihren Suchalgorithmus mit mehreren Sprachen schreiben und testen. Zu berücksichtigende Punkte:

- Auto Vervollständigen – Wenn Sie eine Funktion für automatisches Vervollständigen erstellt haben, stellen Sie sicher, dass Sie Vorschläge für die Sprache des Benutzers eingibt.
- Abgleichen von Abfragen mit Data – Durchsuchen Sie Abfragen, die in einer bestimmten Sprache eingegeben wurden, nur für Inhalte, die in dieser Sprache geschrieben wurden, oder für alle Inhalte in der APP?
- Wort Stamm Erkennung – Wenn die Suche erstellt wurde, um nach ähnlichen Wörtern zu suchen, sind diese Optimierungen für alle Sprachen konzipiert, die Sie unterstützen?
- Sortieren – stellen Sie sicher, dass die Ergebnisse ordnungsgemäß sortiert sind (siehe oben).

### <a name="data-from-external-sources"></a>Daten aus externen Quellen

Viele Anwendungen laden Daten aus externen Quellen herunter, von Twitter-und RSS-Feeds bis hin zu Wetter-, Nachrichten-oder Aktienkursen. Wenn Sie dies für einen Benutzer anzeigen, müssen Sie in Erwägung gezogen werden, dass Sie einen Bildschirm mit nicht relevanten oder unlesbaren Informationen anzeigen.

Es gibt einige Strategien, die Sie verwenden können, um sicherzustellen, dass Ihre APP Daten anzeigt, die für den Benutzer relevant sind:

- Verschiedene Quellen – Ihre Anwendung kann die Daten abhängig von der Sprache oder dem Gebiets Schema des Benutzers aus einer anderen Quelle herunterladen. Gebiets Schema Nachrichten, Wetter-und Kurspreise sind möglicherweise sinnvoller als etwas, das aus einem North American-Feed heruntergeladen wird.
- Lokalisierte Anzeige – wenn Sie einen Twitter-oder fotofeed anzeigen, sollten Sie die Metadaten (z. b. die ergriffene Zeit) in der eigenen Sprache anzeigen, auch wenn der Inhalt selbst in der ursprünglichen Sprache verbleibt.
- Übersetzung – Sie können eine Übersetzungs Option in Ihrer APP erstellen, um eine maschinelle Übersetzung eingehender Daten durchzuführen. Dies kann automatisch oder nach dem Ermessen des Benutzers erfolgen – achten Sie darauf, dass der Benutzer benachrichtigt wird, wenn der Computer Übersetzungen nie perfekt ist.

Dies kann sich auch auf externe Links zu Audiospuren oder Videos auswirken – wenn Sie Ihre Anwendung entwerfen, planen Sie die Beschaffung von übersetzten Inhalten, oder stellen Sie sicher, dass Benutzer von der Benutzeroberfläche ausreichend informiert werden, wenn Inhalte nicht in ihren Kurse.

### <a name="dont-over-translate"></a>Nicht übersetzen

Einige Zeichen folgen in Ihrer APP müssen möglicherweise nicht übersetzt werden, oder Sie benötigen zumindest eine besondere Aufmerksamkeit durch den Übersetzer. Beispiele hierfür sind:

- URLs – Wenn Sie eine URL auflisten, muss Sie möglicherweise nach Sprache angepasst werden. Facebook.com erfordert z. b. keine automatische Übersetzung, wenn die Sprache am Hauptstandort automatisch erkannt wird. Andere Standorte verfügen über Gebiets Schema spezifischen Inhalt, und Sie möchten möglicherweise eine andere URL anbieten, z. b. yahoo.com im Vergleich zu Yahoo.fr oder Yahoo.it.
- Telefonnummern – insbesondere diejenigen mit unterschiedlichen Ländercodes oder Zahlen für Aufrufer, die eine bestimmte Sprache sprechen.
- Kontakt Details – Adressen und andere Informationen können je nach Sprache oder Gebiets Schema variieren.
- Marken & Produktnamen – einige Zeichen folgen müssen nicht übersetzt werden, da Sie immer in derselben Sprache geschrieben sind.

Stellen Sie abschließend sicher, dass Sie ausführliche Anweisungen für den Translator einschließen, wenn bestimmte Zeichen folgen eine besondere Behandlung erfordern.

### <a name="formatted-text"></a>Formatierter Text

In der Regel ein Problem mit Mobile Apps, weil Zeichen folgen in der Regel nicht umfangreich formatiert Wenn jedoch Rich-Text (z. b. Fett Formatierung oder kursiv Formatierung) in Ihrer APP erforderlich ist, stellen Sie sicher, dass der Konvertierer die Formatierung eingibt, die Zeichen folgen Dateien den Wert ordnungsgemäß speichern und ordnungsgemäß formatieren, bevor Sie dem Benutzer angezeigt werden (d. h. nicht versehentlich die Formatierungscodes selbst werden dem Benutzer angezeigt.)

## <a name="translation-tips"></a>Übersetzungstipps

Das Übersetzen der von einer Anwendung verwendeten Zeichen folgen wird als Teil des Lokalisierungsprozesses betrachtet. In der Regel wird diese Aufgabe an einen Übersetzungsdienst ausgelagert und von mehrsprachigen Mitarbeitern durchgeführt, die Ihre Anwendung oder Ihr Unternehmen nicht kennen.

Die folgenden Tipps helfen Ihnen, Zeichen folgen zu entwickeln, die einfacher zu übersetzen sind, und damit die Qualität ihrer lokalisierten apps zu verbessern.

### <a name="localize-complete-strings-not-words"></a>Lokalisieren von kompletten Zeichen folgen, nicht von Wörtern

Manchmal verwenden Entwickler den Ansatz, einzelne Wörter oder "Ausschnitte" anzugeben, damit Sie Sie in der gesamten Anwendung wieder verwenden können. Beispielsweise für den Text "Sie haben 5 Nachrichten". Sie können die folgenden Zeichen folgen für die Übersetzung angeben.

**Schlecht**:

```csharp
"You have"
"no"
"message"
"messages"
```

versuchen Sie dann, den richtigen Ausdruck im Code mithilfe der Zeichen folgen Verkettung dynamisch zu erstellen:

**Schlecht**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**Dies wird** nicht empfohlen, da es nicht notwendigerweise für alle Sprachen funktioniert und es für den Übersetzer schwierig ist, den Kontext jedes kurzen Segments zu verstehen. Dies führt auch dazu, dass übersetzte Zeichen folgen wieder verwendet werden, die später Probleme verursachen können, wenn Sie in unterschiedlichen Kontexten verwendet werden (und dann aktualisiert werden).

### <a name="allow-for-parameter-re-ordering"></a>Neuanordnung von Parametern zulassen

Einige Programmiersprachen erfordern zusätzliche Syntax, um die Reihenfolge der Parameter in einer Zeichenfolge anzugeben. .NET unterstützt jedoch bereits das Konzept nummerierter Platzhalter.

**Gut**:

```csharp
"a {0} b {1} cde {3}"
```

kann wie folgt übersetzt werden (wobei Position und Reihenfolge der Platzhalter geändert werden).

```csharp
"{2} {3} f g h {0}"
```

und die Token werden als der Konvertierer bestimmt. Stellen Sie sicher, dass die einzelnen Platzhalter beim Senden der Zeichenfolge an einen Konvertierer eine Erläuterung enthalten.

### <a name="use-multiple-strings-for-cardinality"></a>Verwenden mehrerer Zeichen folgen für die Kardinalität

Vermeiden Sie Zeichen `"You have {0} message/s."` Folgen wie die Verwendung bestimmter Zeichen folgen für jeden Zustand, um eine bessere Benutzer Leistung zu bieten:

**Gut**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Sie müssen Code in ihrer app schreiben, um die angezeigte Zahl auszuwerten und die entsprechende Zeichenfolge auszuwählen. Einige Plattformen (einschließlich IOS und Android) verfügen über integrierte Funktionen, die automatisch die beste Plural Zeichenfolge basierend auf den Voreinstellungen für die aktuelle Sprache/das aktuelle Gebiets Schema auswählen.

### <a name="allowing-for-gender"></a>Zulassen von Geschlecht

In lateinischen Sprachen werden manchmal andere Wörter verwendet, je nach Geschlecht des Subjekts. Wenn Ihre APP über das Geschlecht Bescheid weiß, sollten Sie den übersetzten Zeichen folgen diese Darstellung ermöglichen.

Außerdem gibt es auch in englischer Sprache einen offensichtlicheren Fall, bei dem Zeichen folgen auf eine bestimmte Person oder einen bestimmten Benutzer Ihrer APP verweisen. Beispielsweise zeigen einige Websites Nachrichten `"Bob commented on his post"` so an, dass Sie Zeichen folgen sowohl für ein männlich-, weiblich-als auch für ein nicht binäres oder unbekanntes Geschlecht benötigen:

**Gut**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Zeichen folgen nicht wieder verwenden

Oder es ist genauer, Zeichen folgen nicht wiederzuverwenden, da Sie ähnlich sind, wenn die Zeichenfolge selbst einen anderen Zweck oder eine andere Bedeutung hat.

Beispiel: angenommen, Sie verfügen über einen ein-/ausschalten in Ihrer APP, und für das switchsteuerelement muss der Text für ' on ' und ' off ' lokalisiert werden. Sie zeigen auch den Wert dieser Einstellung an anderer Stelle in der app in einer Text Bezeichnung an. Sie sollten verschiedene Zeichen folgen für die Anzeige des Switchs im Vergleich zum Status des Schalters verwenden (auch wenn es sich um die gleiche Zeichenfolge in der Standardsprache handelt) – z. b.:

- "On" – wird auf dem Switch selbst angezeigt.
- "Off" – wird auf dem Switch selbst angezeigt.
- "On" – in einer Bezeichnung angezeigt
- "Off" – in einer Bezeichnung angezeigt

Dies bietet maximale Flexibilität für den Translator:

- Aus Entwurfs Gründen verwendet der Switch möglicherweise den Kleinbuchstaben "on" und "Off", aber die Anzeige Bezeichnung verwendet Großbuchstaben "ein" und "aus".
- In einigen Sprachen muss der switchwert möglicherweise so abgekürzt werden, dass er in das Steuerelement der Benutzeroberfläche passt, während das gesamte (übersetzte) Wort in der Bezeichnung angezeigt werden kann.
- Alternativ kann das Rendering Ihres Schalters für einige Sprachen "I" und "O" für die Kultur Vertrautheit verwenden, aber Sie möchten möglicherweise dennoch, dass die Bezeichnung "on" oder "Off" liest.

### <a name="translation-services"></a>Übersetzungsdienste

#### <a name="machine-translation"></a>Maschinelle Übersetzung

Um Übersetzungsfunktionen in Ihre APP zu erstellen, berücksichtigen Sie die [Azure-Textübersetzungs-API](https://azure.microsoft.com/services/cognitive-services/translator-text-api/).

Zu Testzwecken können Sie eine der zahlreichen Online Übersetzungstools verwenden, um während der Entwicklung einen lokalisierten Text in Ihre APP einzubeziehen:

- [Übersetzer](https://www.bing.com/translator/)
- [Google Translation](http://translate.google.com/)

Es stehen zahlreiche weitere Möglichkeiten zur Verfügung. Die Qualität der maschinellen Übersetzung wird in der Regel nicht als ausreichend erachtet, um eine Anwendung freizugeben, ohne dass Sie zuerst von professionellen Konvertierungsprogramme oder systemeigenen Referenten geprüft

#### <a name="professional-translation"></a>Professionelle Übersetzung

Es gibt auch professionelle Übersetzungsdienste, die ihre Zeichen folgen an Ihre eigenen Konvertierungen verteilen und Ihnen die abgeschlossenen Übersetzungen für eine Gebühr bereitstellen.

Einer der bekanntesten Dienste ist [Lionbridge](http://www.lionbridge.com/). Die meisten professionellen Dienste unterstützen alle gängigen Dateitypen, einschließlich Zeichen folgen, XML, resx und Pot/po.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige der Konzepte vorgestellt, mit denen Sie sich vertraut machen sollten, bevor Sie Ihre APP internationalisieren und dann Ihre Ressourcen lokalisieren. Außerdem wird erläutert, wie Sie die Spracheinstellungen für die einzelnen Plattformen ändern.

Diese Konzepte können auf die verschiedenen plattformspezifischen und plattformübergreifenden Internationalisierungs Techniken angewendet werden, die mit xamarin möglich sind.

Lesen Sie weiterhin technische Details zu der Plattform, an der Sie interessiert sind:

- [Xamarin. Forms](~/xamarin-forms/app-fundamentals/localization/index.md) plattformübergreifende Lokalisierung mithilfe von RESX-Dateien.
- Native [xamarin. IOS](~/ios/app-fundamentals/localization/index.md) -Platt Form Lokalisierung.
- Native [xamarin. Android](~/android/app-fundamentals/localization.md) -Platt Form Lokalisierung.

## <a name="related-links"></a>Verwandte Links

- [Übersicht über die Lokalisierung von Apple](https://developer.apple.com/internationalization/)
- [Checkliste für die Lokalisierung von Android](https://developer.android.com/distribute/tools/localization-checklist.html)
- [Bewährte Methoden für die Entwicklung von weltweit einsatzfähigen Anwendungen (MSDN)](https://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
