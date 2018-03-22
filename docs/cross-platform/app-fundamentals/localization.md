---
title: "Lokalisierung der Anwendung Benutzeroberfläche"
ms.topic: article
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 697edde44a5d28ef24cb92a4d06a5c61609b079e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="localization"></a>Lokalisierung

Diese Anleitung erläutert die Konzepte hinter *Internationalisierung* und *Lokalisierung* und Links zu Anweisungen zum Erzeugen von Xamarin mobile Anwendungen, die mit den Konzepten.

Wenn Sie direkt auf die technische Details zum Lokalisieren von Xamarin-apps überspringen möchten, beginnen Sie mit einem der folgenden Vorgehensweisen plattformspezifischen-Artikel:

- [**Xamarin.Forms** ](~/xamarin-forms/app-fundamentals/localization.md) plattformübergreifende Lokalisierung mithilfe von RESX-Dateien.
- [**Xamarin.iOS** ](~/ios/app-fundamentals/localization/index.md) systemeigene Plattform Lokalisierung.
- [**Xamarin.Android**](~/android/app-fundamentals/localization.md) native platform localization.

## <a name="i18n-and-l10n"></a>I18N und L10n

*Internationalisierung* wird der Code kann verschiedene Sprachen anzeigen und Anpassen der Anzeige für verschiedene Gebietsschemas (z. B. Anzahl und Formatieren von Datum) gemacht. Dies wird auch als bezeichnet *Globalisierung*.

*Lokalisierung* ist der Schritt der folgende Ressourcen (z. B. Zeichenfolgen und Bilder) für jede Sprache erstellen und diese mit der app Internationalize Bündelung –.

Internationalisierung, die sie häufig auf i18n – Kurzschreibweise für 18 Buchstaben zwischen "a" und "n" gekürzt wird. Lokalisierung wird auf ähnliche Weise gekürzt, um L10n – für 10 Buchstaben zwischen "L" und "n".

## <a name="overview"></a>Übersicht

Dieses Dokument führt die Konzepten Internationalisierung und Lokalisierung und wie sie für die Entwicklung von mobile-Anwendung im Allgemeinen gelten.
Entwerfen und Erstellen einer Anwendung, die Dinge, die Sie zuvor möglicherweise hart codiert, jedoch müssen die parametrisierte für Lokalisierung enthalten:

-   Bildschirmlayouts und text
-   Symbole, Grafiken und Farben,
-   Video- und Audiodateien
-   Dynamische Text und Text-Formatierung (z. B. Zahlen, Währung und Datumsangaben)
 - Änderungen am Layout für Sprachen für rechts-nach-links (RTL), und
-   Sortieren von Daten.

Unabhängig davon, welche mobile Plattformen, die die app auf diese Tipps abzielt, erstellen eine qualitativ hochwertige lokalisierte app hilft.


## <a name="design-considerations"></a>Entwurfsüberlegungen

Architektur einer Anwendung, damit es möglich ist, den Inhalt zu lokalisieren ist Internationalisierung aufgerufen. Auf diese Weise Internationalisierung ordnungsgemäß ist mehr als nur für unterschiedliche Sprachen können Zeichenfolgen, die zur Laufzeit geladen werden – eine gut entworfene app sollte für alle Ressourcen, die zu ändernden ermöglichen basierend auf Sprache und Gebietsschema (einschließlich von Bildern, Klängen und Videos) und anpassen können Formatierung und Layout zu verschiedenen bewältigen Größe Zeichenfolgen.

Dieser Abschnitt beschreibt einige Überlegungen zum Entwurf beim Erstellen einer internationalisierten Anwendung berücksichtigt werden.

### <a name="layouts-and-string-length"></a>Layouts und der Länge der Zeichenfolge

Chinesisch oder Japanisch Zeichenfolgen können sehr kurz sein – in einigen Fällen ein oder zwei Zeichen können aussagekräftig genug für eine Bezeichnung Eingabefeld.

Deutsche Zeichenfolgen können sehr lange sein (z. B.); In einigen Fällen wird ein relativ kurzen Wort in Englisch sehr lange in anderen Sprachen – entweder als abgeschnitten oder anderen unerwartet Umfließen das Layout.

Vergleichen Sie die Zeichenfolgenlängen für einige Elemente auf dem Startbildschirm von iOS in Englisch, Deutsch und Japanisch:

[![](localization-images/language-compare-sml.png "Deutsch und Japanisch Zeichenfolgenlänge")](localization-images/language-compare.png#lightbox)

Beachten Sie, dass **Einstellungen** auf Englisch (8 Zeichen) erfordert 13 Zeichen für die deutsche Übersetzung jedoch nur 2 Zeichen in Japanisch.

Layouts, in denen die Anzeige Beschriftung als auch Eingabefeld Seite-an-Seite sind, sind schwer zu arbeiten, wenn die Bezeichnungslänge erheblich variieren kann. Häufig ist ein Layout, in dem die Bezeichnung, über ein Feld angezeigt wird, zu lokalisieren, da die gesamte Breite des Bildschirms für die Bezeichnung und die Eingabe verfügbar ist.

Als allgemeine Regel Sie erstellen, können festen Layouts (insbesondere Seite-an-Seite-Elemente) mindestens 50 % weitere Breite als englischen Zeichenfolgen für Bezeichnungen und Texte erforderlich. Dies wird nicht jedes Problem beheben, aber bietet einen Puffer, der in vielen Fällen geeignet ist.

### <a name="input-validation"></a>Validierung von Benutzereingaben

Beachten Sie beim Schreiben von Validierungsregeln Annahmen. Es mag zulässig, benötigen ein Textfeld, die Eingabe für "erfordert mindestens drei Zeichen auf Englisch", da ein einzelner Buchstabe sehr selten Bedeutung hat. Chinesisch und Japanisch ist jedoch ein einzelnes Zeichen handelt es sich möglicherweise um eine gültige Eingabe und eine Validierung message "mindestens 3 Zeichen ist ein Pflichtfeld" nicht für diese Sprachen sinnvoll sein.

Andere scheinbar einfachen Aufgaben, wie z. B. eine e-Mail-Adresse zu überprüfen oder URL der Supportwebsite komplizierter mit den Zeichen sind nicht auf die ASCII-Teilmenge beschränkt.

Schreiben Sie die Validierungsregeln mit Internationalisierung Bedenken – wählen Sie die am wenigsten restriktive Regeln oder schreiben die Logik, damit es anders als für jede Sprache funktioniert.

### <a name="images-and-color"></a>Bilder und Farben

Nicht jedes Image muss so ändern Sie basierend auf die Wahl der Sprache des Benutzers. Viele Symbole oder Fotos wird für alle Benutzer geeignet sein, keine Rolle spielt, welche Sprache sprechen.
Einige Ressourcen wenig Sinn, Lokalisieren jedoch, wie z. B.:

 - Bilder mit Personen oder bestimmte Speicherorte – Ihre app möglicherweise erweckt relevanter für Benutzer lokale Personen/-Speicherorten zeigt.
 - Symbole – kann einige Iconography kulturspezifische und Sie können vereinfachen Ihrer app, indem lokalisieren die Bilder mit der um lokalen Grundlegendes zu reflektieren.
 - Farben – einige Kulturen Farben anders – verstehen kann Rot bedeuten, dass in einer Region, aber in einer anderen Glück Warnung. Überprüfen Sie mit Muttersprachlern beim Entwerfen Ihrer Anwendung zu bestimmen, ob Sie einen Mechanismus zum Lokalisieren von Farben erstellen werden sollte.


### <a name="videos-and-sound"></a>Videos und Sound

Videos und sound vorhanden besondere Herausforderung dar, wenn eine Anwendung lokalisieren, da es zwar relativ einfach abzurufenden Zeichenfolgen übersetzt, mehrere Voiceover aufzeichnen verfolgt oder Videoclips teuer und schwierig sein können.

Mehrere Kopien von video und Audio-Dateien möglicherweise auch die Größe Ihrer Anwendung deutlich (insbesondere, wenn Sie in eine große Anzahl von Sprachen lokalisieren sind oder große Mediendateien). Sie sollten nur die gewünschte Sprachressourcen herunterladen, nachdem der Benutzer hat die app installiert, aber dies könnte auch die zu einer wenig benutzerfreundlichen Oberfläche für langsame Netzwerke.

Es gibt oftmals mehrere Möglichkeiten, um Lokalisierungsprobleme zu lösen: sehr wichtig ist, um diese im Vorfeld zu berücksichtigen, und stellen Sie sicher, dass die Anwendung für sie erledigen ausgelegt ist.


### <a name="dates-times-numbers-and-currency"></a>Datumsangaben, Uhrzeiten, Zahlen und Währung

Wenn Sie Formatierungsfunktionen .NET verwenden, müssen Sie die Kultur angeben, sodass Dezimaltrennzeichen ordnungsgemäß analysiert werden (und Konvertierung Ausnahmen vermeiden). Beispielsweise sind 1.99 und 1,99 gültige dezimaldarstellungen je nach Gebietsschema.

Wenn die Daten aus einer bekannten Quelle stammt (d. h. aus Ihrem eigenen Code oder einen Webdienst, den Sie kontrollieren) Sie können zwar eine Kulturbezeichner, die die Formatierung wie dies funktioniert für die englische Sprache Formatierung InvariantCulture entspricht.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Wenn die Eingabe der Daten von der app-Benutzer mit einem CultureInfo-Instanz, die ihre Gebietsschema wiedergibt analysiert werden:

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Finden Sie unter der [Analysieren von numerischen Zeichenfolgen](http://msdn.microsoft.com/en-us/library/xbtzcc4w(v=vs.110).aspx) und [Analysieren von Zeichenfolgen für Datum und Uhrzeit](http://msdn.microsoft.com/en-us/library/2h3syy57(v=vs.110).aspx) MSDN-Artikel für zusätzliche Informationen.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Sprachen von rechts nach links (RTL)

Einige Sprachen, z. B. Arabisch und Hebräisch Urdu werden (z. B.), von rechts nach links gelesen werden.
Anwendungen, die diese Sprachen zu unterstützen, sollten Bildschirm Entwürfe verwenden, die für rechts-nach-links-Reader an, z. B. anpassen:

 - Rechts ausgerichtete sollte angezeigt werden.
 - Bezeichnungen sollte rechts neben den Eingabefeldern angezeigt werden.
 - Befehlsschaltflächen-Platzierung Standardwert wird im Allgemeinen umgekehrt.
 - Hierarchische Navigation ein Lesegerät und Animation (und andere Navigation Metaphern und Animationen), die Richtung verwenden, für Kontext auch umgekehrt werden soll.

IOS und Android unterstützt rechts-nach-links-Layouts und Schriftart-Rendering mit integrierten Funktionen, mit denen Sie die oben genannten Korrekturen vornehmen. Xamarin.Forms unterstützt RTL-Rendering nicht derzeit automatisch.

### <a name="sorting"></a>Sortieren

Verschiedene Sprachen definieren die Sortierreihenfolge von ihrem Alphabet unterschiedlich ist, selbst wenn sie den gleichen Zeichensatz verwenden.

Finden Sie unter der [Detail Zeichenfolgenvergleich](http://msdn.microsoft.com/en-us/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) in [bewährte Methoden für die Verwendung von Zeichenfolgen in .NET Framework](http://msdn.microsoft.com/en-us/library/dd465121(v=vs.110).aspx) ein Beispiel, in dem Sprache (CultureInfo) wirkt sich auf die Sortierreihenfolge.

Es ist unwahrscheinlich, dass die integrierten Funktionen auf mobilen Plattformen unterstützen sprachspezifische Sortierreihenfolge so Sie möglicherweise zusätzlichen Code in Ihrer Geschäftslogik zu implementieren müssen.

### <a name="text-search"></a>Textsuche

Stellen Sie sicher, schreiben und Testen Ihre Suchalgorithmus mit mehreren Sprachen Bedenken. Zu berücksichtigende Aspekte umfassen:

 - AutoVervollständigen – Wenn Sie eine AutoVervollständigen-Funktion erstellt haben, stellen Sie sicher, dass sie Vorschläge für die Sprache des Benutzers relevant Datenquellen aus.
 - Übereinstimmende Abfrage Daten – sucht Abfragen, die in einer bestimmten eingegebenen Sprache nur Inhalte in dieser Sprache geschrieben wurde, oder für alle Inhalte in der app ausgeführt werden?
 - Wortstammerkennung – Wenn die Suche erstellt wird, um ähnliche Wörter suchen, Word Stämme und andere Optimierungen suchen Suchen sind diese Optimierungen erstellt für alle Sprachen, die Sie unterstützen?
 - Sortieren – stellen Sie sicher, dass die Ergebnisse sind sortiert ordnungsgemäß (siehe oben).


### <a name="data-from-external-sources"></a>Daten aus externen Quellen

Viele Anwendungen Herunterladen von Daten aus externen Quellen, Twitter und RSS-feeds, Wetter, News oder Aktienkurse. Wenn dies für einen Benutzer angezeigt, müssen Sie die Möglichkeit berücksichtigen, dass Sie einen Bildschirm irrelevante oder unlesbare Informationen Ihnen angezeigt werden.

Es gibt einige Strategien, die Sie verwenden können, um zu testen, und stellen Sie sicher, dass die app Daten, die für den Benutzer relevant anzeigt:

 - Verschiedenen Quellen – möglicherweise auf Ihre Anwendung die Daten aus einer anderen Quelle je nach Sprache oder Gebietsschema des Benutzers herunterladen. Gebietsschema News "," Weather "und" Stock Preise sinnvoll mehr als ein Element aus einem Feed Nordamerikanisches heruntergeladen.
 - Lokalisierte Anzeige – Wenn Sie einen Twitter oder Foto feed anzeigen Sie sollte die Metadaten anzeigen (z. B. die Zeit) in sein eigenes Sprache, auch wenn der Inhalt selbst in der Originalsprache verbleibt.
 - Übersetzung – könnten Sie eine Übersetzungsoption in Ihre app, eine maschinelle Übersetzung eingehender Daten erstellen. Dies kann erfolgen automatisch oder Ermessen des Benutzers – nur Sie sicher, dass den Benutzer zu benachrichtigen, wenn dies stattfindet da Computer Übersetzungen nie perfekten sind!

Dies könnte auch externe Links Audiospuren beeinträchtigen oder Videos – Wenn Sie sicher, dass Sie vorausschauend planen, für die Beschaffung werden zum Entwerfen Ihrer Anwendung übersetzt Inhalt oder indem Sie sicherstellen, dass Benutzer von der Benutzeroberfläche angemessen informiert werden, wenn der Inhalt nicht in angezeigt werden ihre Sprache.


### <a name="dont-over-translate"></a>Nicht zu übersetzen.

Einige Zeichenfolgen in Ihrer app möglicherweise nicht erforderlich, übersetzen, oder benötigen zumindest die besondere Aufmerksamkeit vom Konvertierungsprogramm. Beispiele können Folgendes umfassen:

 - URLs – Wenn Sie eine URL auflisten er möglicherweise oder eventuell durch Sprache angepasst werden. "Facebook.com" erfordert z. B. Übersetzung Auto-Sprache am zentralen Standort erkannt wird. Gebietsschemaspezifische Inhalt an andere Standorte verfügbar, und Sie eine andere URL ein, z. B. yahoo.com im Vergleich zu yahoo.fr oder yahoo.it anbieten möchten.
 - Telefonnummern – insbesondere solche, die mit verschiedenen Landeskennzahlen oder Zahlen für Aufrufer, die eine bestimmte Sprache zu sprechen.
 - Kontaktdetails – Adressen und andere Informationen variieren von Sprache oder Gebietsschema ab.
 - Marken & Produktnamen – nicht einige Zeichenfolgen übersetzen, da sie immer in derselben Sprache geschrieben sind erforderlich.

Werden Sie zum Schluss fügen Sie detaillierte Anweisungen für das Konvertierungsprogramm ein, wenn Sie bestimmte Zeichenfolgen besondere Behandlung erfordern.


### <a name="formatted-text"></a>Formatierter Text

Nicht in der Regel ein Problem mit den mobilen apps Da Zeichenfolgen in der Regel Rich-Text formatiert sind. Jedoch ggf. von rich-Text (z. B. fett bzw. kursiv formatiert) in Ihrer app sicherstellen das Konvertierungsprogramm weiß, wie von Eingaben, die Formatierung, die Zeichenfolgen-Dateien speichern es ordnungsgemäß und es vor seiner Anzeige für den Benutzer richtig formatiert ist (d. h. lassen nicht versehentlich Formatierungscodes selbst werden dem Benutzer angezeigt).



## <a name="translation-tips"></a>Translation-Tipps

Übersetzen von Zeichenfolgen, die von einer Anwendung wird als Teil des Lokalisierungsprozesses betrachtet. In der Regel wird diese Aufgabe zu einem Übersetzungsdienst ausgelagerten und durchgeführte mehrsprachige Mitarbeiter, die Ihre Anwendung oder Ihr Unternehmen möglicherweise nicht bekannt ist.

Die folgenden Tipps hilft Ihnen, Ergebniszeichenfolgen zu generieren, die einfacher zu genau zu übersetzen und daher die Qualität Ihrer lokalisierten Anwendungen zu verbessern.



### <a name="localize-complete-strings-not-words"></a>Lokalisieren Sie vollständige Zeichenfolgen nicht Wörter

In einigen Fällen sind Entwickler möchten die einzelnen Wörtern oder einen Satz "Ausschnitte" angeben, damit sie sie in der gesamten Anwendung erneut verwenden können. Z. B. haben"für den Text Sie 5 Nachrichten." Sie können die folgenden Zeichenfolgen für die Übersetzung angeben.

**Bad**:

```csharp
"You have"
"no"
"message"
"messages"
```

und dann versuchen, die richtige Satz auf dynamische im Code mithilfe von Verkettung von Zeichenfolgen erstellen:

**Bad**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**Dies wird abgeraten** daran, dass er nicht notwendigerweise für alle Sprachen funktioniert und nur des konvertierers schwer, damit Sie den Kontext der einzelnen kurze Segmente verstehen. Er führt außerdem von übersetzten Zeichenfolgen verwenden erneut die später Probleme verursachen können, wenn sie in unterschiedlichen Kontexten verwendet werden (und dann aktualisiert).


### <a name="allow-for-parameter-re-ordering"></a>Neuordnen der Parameter ermöglichen

Einige Programmiersprachen erfordern zusätzliche Syntax zum Angeben von der Reihenfolge der Parameter in einer Zeichenfolge jedoch .NET das Konzept der nummerierten Platzhaltern, sodass bereits unterstützt

**Gute**:

```csharp
"a {0} b {1} cde {3}"
```

ist möglicherweise die folgenden (die Position und Reihenfolge der Platzhalter ist geänderter) übersetzt

```csharp
"{2} {3} f g h {0}"
```

und das Token werden als das Konvertierungsprogramm vorgesehen sortiert werden. Achten Sie darauf, dass Sie eine Erläuterung, was jeder Platzhalter enthält, wenn die Zeichenfolge an einen Übersetzer senden enthalten.


### <a name="use-multiple-strings-for-cardinality"></a>Verwenden von mehreren Zeichenfolgen für die Kardinalität

Vermeiden Sie Zeichenfolgen wie `"You have {0} message/s."` gebietsschemaspezifische Zeichenfolgen für jeden Status verwenden, um eine bessere benutzererfahrung bereitzustellen:

**Gute**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Sie müssen Code schreiben, in der app, die angezeigt wird ausgewertet, und wählen die entsprechende Zeichenfolge. Einige Plattformen (einschließlich IOS- und Android) haben integrierte Funktionen, die beste plural Zeichenfolge basierend auf den benutzereinstellungen für das aktuelle Gebietsschema automatisch auszuwählen.


### <a name="allowing-for-gender"></a>Für das Geschlecht zulassen

Lateinischen Sprachen verwenden manchmal unterschiedliche Wörter abhängig von das Geschlecht des Antragstellers an. Wenn Ihre app zu Geschlecht bekannt ist, sollten Sie die übersetzten Zeichenfolgen entsprechend zulassen.

Es ist auch der offensichtlichere Fall auch in Englisch, Zeichenfolgen, in denen für eine bestimmte Person oder Benutzer Ihrer App zu verweisen. Beispielsweise einige Websites angezeigt, wie Nachrichten `"Bob commented on his post"` daher Sie Zeichenfolgen für eine Geschlecht Männlich, weiblich, und nicht-Binary oder unbekannt benötigen:

**Gute**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Zeichenfolgen nicht wiederverwenden

Oder genauer gesagt nicht Zeichenfolgen wiederverwenden, nur weil diese ähnlich sind, wenn die Zeichenfolge selbst einen anderen Zweck oder keine Bedeutung mehr hat.

Zum Beispiel: Angenommen, Sie über eine Ein-/Ausschalter in Ihrer app und der Switch-Steuerelement benötigt den Text für 'on' und 'off' lokalisiert werden soll. Zeigen Sie auch den Wert dieser Einstellung an anderer Stelle in der app in einem textbezeichnung. Verwenden Sie verschiedene Zeichenfolgen für die Switch-Anzeige im Vergleich zu den Switch-Status (selbst wenn sie die gleiche Zeichenfolge in der Standardsprache sind) – zum Beispiel:

-   "Auf" – auf dem Switch selbst angezeigt.
-   "Off" – auf dem Switch selbst angezeigt.
-   "Auf" – in eine Bezeichnung angezeigt.
-   "Off" – in eine Bezeichnung angezeigt.

Dies bietet maximale Flexibilität für das Konvertierungsprogramm:

-   Aus Gründen der Entwurf vielleicht der Schalter verwendet Kleinbuchstaben "on" und "off" aber die Anzeige Bezeichnung Großbuchstaben "On" und "Off".
-   Für einige Sprachen möglicherweise, dass den Switch-Wert, der in das Benutzeroberflächensteuerelement angepasst werden, während das vollständige (übersetzte) Wort in der Bezeichnung angezeigt werden kann abgekürzt werden.
-   Alternativ kann das Rendern des Switchs für einige Sprachen werden "I" und "O" für kulturellen vertraut sind, aber Sie sollten dennoch die Bezeichnung "On" oder "Off" lesen.

### <a name="translation-services"></a>Übersetzungsdienste

#### <a name="machine-translation"></a>Maschinellen Übersetzung

Um Übersetzungsfunktionen in Ihrer app zu erstellen, sollten Sie die [-API der Azure-Konvertierungsprogramm Text](https://azure.microsoft.com/en-au/services/cognitive-services/translator-text-api/).

Für Testzwecke können Sie eine der vielen online Übersetzungstools verwenden, während der Entwicklung Ihrer app lokalisierten Text einschließt:

- [Bing-Übersetzer](https://www.bing.com/translator/)
- [Google Translate](http://translate.google.com/)

Es gibt noch viele andere verfügbar. Die Qualität der maschinellen Übersetzung ist nicht im allgemeinen ausreichend, um eine Anwendung freigeben betrachtet, ohne zuerst überprüft und professionelle Übersetzer oder Muttersprachlern getestet.

#### <a name="professional-translation"></a>Professionelle Übersetzung

Es gibt auch professionelle Übersetzungsdienste, die die Zeichenfolgen und an ihre eigenen Konvertierer, bietet Ihnen das fertige Übersetzungen für eine Gebühr verteilen.

Einer der bekanntesten Dienste ist [LionBridge](http://www.lionbridge.com/). Die meisten professional Services unterstützen alle gängige Dateitypen, die z. B. Zeichenfolgen, XML "," RESX "und" POT/PO.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige der Konzepte erläutert, mit vertraut sein sollten, bevor die Internationalisierung Ihrer app, und klicken Sie dann zum Lokalisieren von Ressourcen und behandelt auch so ändern Sie die spracheinstellungen für jede Plattform eingeführt.

Diese Konzepte können auf die verschiedenen plattformspezifischen und plattformübergreifende Internationalisierung Techniken angewendet werden, die mit Xamarin möglich sind.

Fortsetzen Sie, lesen technische Details für die Plattform, der Sie interessiert sind:

- [Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization.md) plattformübergreifende Lokalisierung mithilfe von RESX-Dateien.
- [Xamarin.iOS](~/ios/app-fundamentals/localization/index.md) systemeigene Plattform Lokalisierung.
- [Xamarin.Android](~/android/app-fundamentals/localization.md) systemeigene Plattform Lokalisierung.



## <a name="related-links"></a>Verwandte Links

- [Übersicht über die Lokalisierung von Apple](https://developer.apple.com/internationalization/)
- [Prüfliste für Android-Lokalisierung](http://developer.android.com/distribute/tools/localization-checklist.html)
- [Bewährte Methoden für die Entwicklung weltweit einsatzfähiger Anwendungen (MSDN)](http://msdn.microsoft.com/en-us/library/w7x1y988%28v=vs.90%29.aspx)
