---
title: Lokalisierung der Anwendung Benutzeroberfläche
description: Dieses Dokument beschreibt die Konzepte plattformübergreifende Internationalisierung und Lokalisierung und untersucht, wie sie den Entwurf auswirken.
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: a1218d836aad827390d9f5e70de189a869b7c6b8
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830999"
---
# <a name="localization"></a>Lokalisierung

Dieser Leitfaden beschreibt die Konzepte hinter der *Internationalisierung* und *Lokalisierung* und Links zu Anweisungen zum Erstellen von mobiler Anwendungen diese Konzepte mit Xamarin.

Wenn Sie direkt zum Lokalisieren von Xamarin-apps die technischen Details überspringen möchten, beginnen Sie mit einem der folgenden plattformspezifischen Gewusst-wie-Artikel:

- [**Xamarin.Forms** ](~/xamarin-forms/app-fundamentals/localization/index.md) plattformübergreifende Lokalisierung mit RESX-Dateien.
- [**Xamarin.iOS** ](~/ios/app-fundamentals/localization/index.md) Lokalisierung der nativen Plattform.
- [**Xamarin.Android** ](~/android/app-fundamentals/localization.md) Lokalisierung der nativen Plattform.

## <a name="i18n-and-l10n"></a>I18N und L10n

*Internationalisierung* ist der Prozess der macht Ihren Code in verschiedenen Sprachen anzeigen und die Anzeige für verschiedene Gebietsschemas (z.B. Anzahl und die Formatierung des Datums) anpassen kann. Dies wird auch als bezeichnet *Globalisierung*.

*Lokalisierung* ist der Schritt die folgende Ressourcen (z. B. Zeichenfolgen und Bilder) für jede Sprache erstellen und Bündeln dieser mit der app Internationalize –.

Internationalisierung, die sie häufig auf i18n: Kurzform für 18 Buchstaben zwischen "i" und "n" gekürzt wird. Lokalisierung wird auf ähnliche Weise verkürzt, um L10n – für 10 Buchstaben zwischen "L" und "n".

## <a name="overview"></a>Übersicht

In diesem Dokument werden, die Konzepte, die Internationalisierung und Lokalisierung und deren Entwicklung mobiler Anwendungen im Allgemeinen Anwendung zugeordnet wird.
Beim Entwerfen und Erstellen einer Anwendung, Schritte, die Sie zuvor möglicherweise hart codiert, aber die müssen parametrisiert werden, für die Lokalisierung enthalten:

- Layouts für Startbildschirm und dem Fehlertext
- Symbole, Grafiken und Farben
- Video- und Audiodateien,
- Dynamischer Text und Text-Formatierung (z. B. Zahlen, Währung und Datumsangaben)
- Änderungen am Layout für Sprachen für rechts-nach-links (RTL), und
- Sortieren von Daten.

Unabhängig davon, welche hilft mobile Plattformen, die Ihre app diese Tipps ausgerichtet ist Ihnen eine qualitativ hochwertige lokalisierte app erstellen.


## <a name="design-considerations"></a>Entwurfsüberlegungen

Entwerfen einer Anwendung, sodass es möglich ist, dessen Inhalte zu lokalisieren, heißt Internationalisierung. Internationalisierung richtig ist, dass mehr als für andere Sprache ermöglichen Zeichenfolgen zur Laufzeit geladen werden: eine gut entworfene app sollte für alle Ressourcen, die geändert werden, können auf Sprache und Gebietsschema (einschließlich Bildern, Sound und Videos) basieren und anpassen können formatierungs- und Layoutinformationen zu verschiedenen bewältigen Größe Zeichenfolgen.

Dieser Abschnitt beschreibt einige Überlegungen zum Entwurf beim Erstellen einer internationalisierten Anwendung berücksichtigt werden.

### <a name="layouts-and-string-length"></a>Layouts und die Länge der Zeichenfolge

Chinesische und japanische Zeichensätze Zeichenfolgen können sehr kurz sein – manchmal ein oder zwei Zeichen vorhanden sind, die für eine Bezeichnung Eingabefeld aussagekräftig genug.

Deutsche Zeichenfolgen können sehr lange sein (z. B.); Manchmal wird ein relativ kurze Wort in englischer Sprache in anderen Sprachen – entweder zu abgeschnittenen oder else unerwartet Umfließen Ihr Layout sehr lange.

Vergleichen Sie die Länge der Zeichenfolge für einige Elemente auf der Startseite für iOS in Englisch, Deutsch und Japanisch:

[![](localization-images/language-compare-sml.png "Deutsch und Japanisch Zeichenfolgenlänge")](localization-images/language-compare.png#lightbox)

Beachten Sie, dass **Einstellungen** in englischer Sprache (8 Zeichen) ist 13 Zeichen für die deutsche Übersetzung jedoch nur 2 Zeichen in japanischer Sprache erforderlich.

Layouts, in denen die Beschriftung anzeigen und das Eingabefeld Seite-an-Seite sind, sind schwierig zu zusammenarbeiten, wenn die Bezeichnungslänge stark variieren kann. Häufig ist ein Layout, in dem die Bezeichnung, über ein Feld angezeigt wird, zu lokalisieren, da die gesamte Breite des Bildschirms für die Beschriftung und die Eingabe verfügbar ist.

Bei der Erstellung werden, können festen Layouts (insbesondere Seite-an-Seite-Elemente) als in der Regel mindestens 50 % mehr die Breite als die englischsprachigen Zeichenfolgen für Bezeichnungen und Texte erfordern. Dies wird nicht jedes Problem lösen, jedoch bietet einen Puffer, der in vielen Fällen funktioniert.

### <a name="input-validation"></a>Eingabeüberprüfung

Beachten Sie, dass der Annahmen beim Schreiben von Validierungsregeln. Es mag zulässig, benötigen ein Textfeld, die Eingabe für die "mindestens drei Zeichen im englischen erforderlich", da ein einzelner Buchstabe verwendet nur sehr selten keine Bedeutung hat. In der chinesische und japanische Zeichensätze ist jedoch ein einzelnes Zeichen handelt es sich möglicherweise um eine gültige Eingabe und eine Überprüfung Nachricht "mindestens 3 Zeichen ist erforderlich" nicht für diese Sprachen sinnvoll.

Andere scheinbar einfachen Aufgaben wie Überprüfung eine e-Mail-Adresse oder Website-URL, die komplizierter mit den Zeichen sind nicht auf die Teilmenge von ASCII-beschränkt.

Schreiben Sie die Validierungsregeln mit Internationalisierung Denken Sie daran: Wählen Sie die am wenigsten restriktive Regeln oder schreiben die Logik, sodass es sich anders als für jede Sprache funktioniert.

### <a name="images-and-color"></a>Bilder und Farben

Nicht jedes Bild muss so ändern Sie basierend auf Ihrer Wahl eines Benutzers. Viele Symbole oder Fotos wird für alle Benutzer geeignet ist, spielt keine Rolle, welche Sprache sprechen.
Sinnvoll, einige Ressourcen So lokalisieren Sie jedoch, wie z. B.:

- Bilder mit Personen oder bestimmte Speicherorte – Ihre app fühlen sich möglicherweise für Benutzer relevantere lokalen Personen/Standorte angezeigt.
- Symbole – kann einige ikonographie kulturspezifische und Sie können die app einfacher zu verwenden von der Lokalisierung der Bilder entsprechend der lokalen Grundlegendes zu machen.
- Farben – einige Kulturen Farben anders – verstehen kann Rot bedeuten, Warnung, die in einer Region, aber viel Glück in einem anderen. Wenden Sie Muttersprache beim Entwerfen Ihrer app zu bestimmen, ob Sie einen Mechanismus zum Lokalisieren von Farben erstellen soll.


### <a name="videos-and-sound"></a>Videos und Sound

Videos und sound vorhanden besondere Herausforderungen beim Lokalisieren einer Anwendung, da es zwar relativ einfach, die übersetzte Zeichenfolgen abgerufen, mehrere Voiceover aufzeichnen überwacht oder Videoclips teuer und schwierig sein können.

Mehrere Kopien von Video- und Audio-Dateien können auch die Größe Ihrer Anwendung deutlich (insbesondere, wenn Sie in eine große Anzahl von Sprachen lokalisieren werden oder verfügen über zahlreiche Mediendateien). Sie sollten nur die erforderliche Sprachressourcen herunterladen, nachdem der Benutzer hat Ihre app installiert, jedoch auch eine schlechte benutzererfahrung in langsamen Netzwerken Dies könnte.

Es gibt oftmals mehrere Möglichkeiten, um Lokalisierungsprobleme zu lösen – sehr wichtig, dass diese Investitionen zu berücksichtigen, und stellen Sie sicher, dass Ihre Anwendung konzipiert ist, um diese abzudecken.


### <a name="dates-times-numbers-and-currency"></a>Datumsangaben, Uhrzeiten, Zahlen und Währung

Wenn Sie die Formatierungsfunktionen von .NET nutzen, denken Sie daran, um die Kultur anzugeben, damit Dezimaltrennzeichen korrekt analysiert werden (und Konvertierung Ausnahmen vermeiden). Beispielsweise sind sowohl für 1,99 1,99 gültige dezimaldarstellungen je nach Ihrem Gebietsschema.

Wenn die Daten aus einer bekannten Quelle stammt (d. h. aus Ihren eigenen Code oder einen Webdienst, den Sie steuern) Sie können zwar ein, das dieselbe Formatierung wie das funktioniert für die englische sprachformatierung InvariantCulture Kulturbezeichner.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Wenn die Daten von der app-Benutzer eingegeben werden, analysieren sie mit der ein CultureInfo-Instanz, die ihrem Gebietsschema wiedergibt:

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Finden Sie unter den [analysieren numerischer Zeichenfolgen](https://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx) und [Analysieren von Zeichenfolgen für Datum und Uhrzeit](https://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx) MSDN-Artikel Weitere Informationen.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Rechts-nach-links (RTL)-Sprachen

Einige Sprachen wie Arabisch und Hebräisch Urdu werden (z. B.), von rechts nach links gelesen werden.
Anwendungen, die diese Sprachen zu unterstützen, sollten Bildschirm Entwürfe verwenden, die für rechts-nach-links-Reader an, z. B. anpassen:

- Rechts ausgerichtete sollte angezeigt werden.
- Bezeichnungen sollten rechts neben den Eingabefeldern angezeigt werden.
- Befehlsschaltflächen-Platzierung der Standard ist in der Regel rückgängig gemacht werden.
- Wischen hierarchische Navigation und Animation (und andere Navigation Metaphern und Animationen) verwenden, die Richtung, für Kontext auch umgekehrt werden soll.

IOS und Android unterstützt rechts-nach-links-Layouts und Schriftart-Rendering mit integrierten Features, mit denen Sie die oben aufgeführten Anpassungen vornehmen. Xamarin.Forms unterstützt keine derzeit automatisch RTL-Rendering.

### <a name="sorting"></a>Sortieren

Verschiedene Sprachen definieren die Sortierreihenfolge von ihrem Alphabet unterschiedlich, auch wenn sie den gleichen Zeichensatz verwenden.

Finden Sie unter den [Details der Zeichenfolgenvergleich](https://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) in [bewährte Methoden für die Verwendung von Zeichenfolgen in .NET Framework](https://msdn.microsoft.com/library/dd465121(v=vs.110).aspx) ein Beispiel, in denen Sprache (CultureInfo) wirkt sich auf die Sortierreihenfolge.

Es ist unwahrscheinlich, dass die integrierte Datenbank-Funktionen auf den mobilen Plattformen unterstützen sprachspezifische sortieren, die Reihenfolge, also Sie möglicherweise zusätzlichen Code in der Geschäftslogik zu implementieren müssen.

### <a name="text-search"></a>Textsuche

Stellen Sie sicher, schreiben und Testen Sie Ihre Suchalgorithmus mit mehreren Sprachen, denken Sie daran. Zu berücksichtigende Aspekte umfassen:

- Automatische Vervollständigung – Wenn Sie eine AutoVervollständigen-Funktion erstellt haben, stellen Sie sicher, dass sie Vorschläge für die Sprache des Benutzers relevanten Datenquellen.
- Übereinstimmende Abfrage zu Daten – werden Suchabfragen eingegeben haben, in einer bestimmten Sprache nur Inhalte, die in dieser Sprache geschrieben wurde, oder für alle Inhalte in Ihrer app ausgeführt werden?
- Wortstammerkennung – Wenn die Suche erstellt wird, um ähnliche Wörter suchen, Word Stämme und andere Optimierungen suchen, suchen, sind diese Optimierungen, die für alle Sprachen, die Sie unterstützen erstellt?
- Sortierung – stellen Sie sicher, dass die Sortierung der Ergebnisse ordnungsgemäß (siehe oben).


### <a name="data-from-external-sources"></a>Daten aus externen Quellen

Viele Anwendungen Herunterladen von Daten aus externen Quellen, von Twitter und RSS-feeds, zum Wetter, Nachrichten oder Aktienkurse. Wenn dies zu einem Benutzer angezeigt, müssen Sie die Möglichkeit berücksichtigen, dass Sie einen Bildschirm von irrelevanten oder unlesbar werden angezeigt werden.

Es gibt einige Strategien, die Sie verwenden können, versuchen Sie es aus, und stellen Sie sicher, dass Ihre app relevanten Daten für den Benutzer anzeigt:

- Unterschiedliche Quellen – kann Ihre Anwendung die Daten aus einer anderen Quelle abhängig von Sprache oder Gebietsschema des Benutzers herunterladen. Gebietsschema News, Wetter und Stock Preise möglicherweise sinnvoller als etwas von einem nordamerikanischen Feed heruntergeladen.
- Lokalisierte anzeigen – wenn Sie eine Twitter oder Foto-feed angezeigt werden Sie sollte anzeigen, die Metadaten (z.B. die angefallene Zeit) in sein eigenes Sprache, selbst wenn der Inhalt selbst in der ursprünglichen Sprache bleibt.
- Übersetzung: könnten Sie eine Übersetzungsoption in Ihre app eine maschinelle Übersetzung der eingehenden Daten erstellen. Dies kann Automatic oder Ermessen des Benutzers – Achten Sie nur auf den Benutzer zu benachrichtigen, wenn dies stattfindet, da Computer Übersetzungen nie perfekt sind!

Videos – beim Entwerfen Ihrer Anwendung im Voraus planen Sie für die Ermittlung werden übersetzt, Inhalt oder sicherstellen, dass Benutzer von der Benutzeroberfläche angemessen informiert werden, wenn der Inhalt nicht in angezeigt wird bzw. externe Links zu den Audiospuren auch Auswirkungen auf ihre die Sprache.


### <a name="dont-over-translate"></a>Nicht zu übersetzen.

Einige Zeichenfolgen in Ihrer app möglicherweise nicht benötigen, übersetzen oder benötigen zumindest die besondere Aufmerksamkeit vom Übersetzer. Beispiele:

- URLs – Wenn Sie eine URL auflisten möglicherweise oder kann nicht nach Sprache angepasst werden müssen. Z. B. erforderlich keine facebook.com Übersetzung, die es erkennt automatisch-Sprache am zentralen Standort. Gebietsschemaspezifische-Inhalte für andere Standorte haben, und Sie eine andere URL ein, z. B. yahoo.com im Vergleich zu yahoo.fr oder yahoo.it anbieten möchten.
- Telefonnummern – vor allem solche mit verschiedenen Ländercodes oder Zahlen für Aufrufer, die eine bestimmte Sprache zu sprechen.
- Details für sicherheitskontakt – Adressen und andere Informationen unterscheiden sich von Sprache oder Gebietsschema.
- Marken & aus Produktnamen besteht – nicht einige Zeichenfolgen erforderlich, übersetzen, weil sie immer in derselben Sprache geschrieben werden.

Schließlich müssen Sie detaillierte Anweisungen für das Konvertierungsprogramm einschließen, wenn bestimmte Zeichenfolgen besondere Behandlung erfordern.


### <a name="formatted-text"></a>Formatierter Text

Nicht in der Regel ein Problem mit den mobilen apps, da Zeichenfolgen in der Regel Rich-Text formatiert sind. Jedoch sicherstellen der Übersetzer weiß, wie für die Eingabe, die Formatierung, die Zeichenfolgen-Dateien speichern richtig und sie ist ordnungsgemäß formatiert, bevor Sie dem Benutzer angezeigt wird, wenn rich-Text (z. B. fett bzw. kursiv formatiert) in Ihrer app erforderlich ist (d. h. lassen nicht versehentlich die Formatierungscodes selbst werden dem Benutzer angezeigt).



## <a name="translation-tips"></a>Translation-Tipps

Übersetzen die Zeichenfolgen, die von einer Anwendung verwendeten gilt als Teil der Lokalisierungsprozess sein. In der Regel wird diese Aufgabe, ein Übersetzungsdienst ausgelagert und ausgeführt, die mehrsprachige Personal, die Ihre Anwendung oder Ihr Unternehmen möglicherweise nicht bekannt ist.

Die folgenden Tipps hilft Ihnen das Erstellen von Zeichenfolgen, die einfacher zu übersetzen, genau und verbessert die Qualität der lokalisierten apps sind.



### <a name="localize-complete-strings-not-words"></a>Lokalisieren Sie vollständige Zeichenfolgen, die keine Wörter

Manchmal sind Entwickler versuchen, um einzelne Wörter oder einen Satz "Snippets" anzugeben, damit sie sie in der gesamten Anwendung erneut verwenden können. Z. B. müssen"für den Text 5 Nachrichten." Sie können die folgenden Zeichenfolgen für die Übersetzung angeben.

**Ungültige**:

```csharp
"You have"
"no"
"message"
"messages"
```

und dann versuchen, die richtige Ausdruck auf dynamisch im Code mithilfe von zeichenfolgenverkettung zu erstellen:

**Ungültige**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**Dies wird davon abgeraten,** daran, dass es nicht unbedingt für alle Sprachen funktioniert und nur des konvertierers schwer, damit Sie den Kontext der einzelnen kurze Segmente zu verstehen. Er führt auch zu einer übersetzten Zeichenfolgen, erneut verwenden, die später Probleme verursachen können, wenn sie in unterschiedlichen Kontexten verwendet werden (und anschließend aktualisiert).


### <a name="allow-for-parameter-re-ordering"></a>Die Reihenfolge von Parameter ermöglichen

Einige Programmiersprachen erfordern zusätzliche Syntax zum Angeben der Reihenfolge von Parametern in einer Zeichenfolge, aber .NET bereits also das Konzept der nummerierte Platzhalter unterstützt

**Good**:

```csharp
"a {0} b {1} cde {3}"
```

ist möglicherweise die folgenden (die Position und die Reihenfolge der Platzhalter ist geänderter) übersetzt

```csharp
"{2} {3} f g h {0}"
```

und die Token werden als das Konvertierungsprogramm soll sortiert werden. Achten Sie darauf, dass Sie erläutert, was jeder Platzhalter enthält, wenn die Zeichenfolge an einen Übersetzer zu senden.


### <a name="use-multiple-strings-for-cardinality"></a>Verwenden Sie mehrere Zeichenfolgen für die Kardinalität

Vermeiden Sie Zeichenfolgen wie `"You have {0} message/s."` Zeichenfolgen für die einzelnen Zustände verwenden, um eine bessere benutzererfahrung bereitzustellen:

**Good**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Sie müssen zum Schreiben von Code in Ihrer app, die angezeigt wird ausgewertet, und wählen die entsprechende Zeichenfolge. Einige Plattformen (einschließlich iOS und Android) verfügen über integrierte automatisch die beste Zeichenfolge im plural basierend auf den Einstellungen für das aktuelle Sprache/Gebietsschema auswählen.


### <a name="allowing-for-gender"></a>Für das Geschlecht zulassen

Lateinische Sprachen verwenden gelegentlich andere Wörtern abhängig von das Geschlecht des Antragstellers. Wenn Ihre app zu Geschlecht bekannt ist, sollten Sie die übersetzten Zeichenfolgen entsprechend dies zulassen.

Es gibt auch weitere offensichtliche Fall auch in Englisch, Zeichenfolgen, in dem für einen bestimmten Benutzer oder die Benutzer Ihrer App zu verweisen. Beispielsweise einige Websites angezeigt wie `"Bob commented on his post"` benötigen Sie Zeichenfolgen für eine Geschlecht "Male", "female", und nicht-Binärdaten oder unbekannt:

**Good**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Nicht erneut Zeichenfolgen

Oder genauer gesagt keine Zeichenfolgen wiederverwenden, nur weil sie ähnlich sind, wenn die Zeichenfolge eine anderen Zweck oder die Bedeutung hat.

Zum Beispiel: Angenommen, Sie haben eine Ein-/Ausschalter in Ihrer app und das Switch-Steuerelement ist des Texts für 'on' und 'off' lokalisiert werden soll. Sie zeigt auch den Wert dieser Einstellung an anderer Stelle in der app in einer textbezeichnung. Verwenden Sie verschiedene Zeichenfolgen für die Switch-Anzeige und des Switches Status (auch wenn sie die gleiche Zeichenfolge in die Standardsprache sind) – zum Beispiel:

- "On" – auf dem Switch selbst angezeigt.
- "Off": auf dem Switch selbst angezeigt.
- "On" – in einer Bezeichnung angezeigt.
- "Off" – in einer Bezeichnung angezeigt.

Dies bietet maximale Flexibilität für das Konvertierungsprogramm:

- Aus Gründen der Entwurf vielleicht der Switch auch Kleinbuchstaben "on" und "off" verwendet die Bezeichnung für die Anzeige verwendet jedoch Großbuchstaben "On" und "Off".
- Einige Sprachen möglicherweise den Schalterwert abgekürzt werden soll, in das Benutzeroberflächensteuerelement, angepasst werden, während die wortvervollständigung (übersetzte) in der Bezeichnung angezeigt werden kann.
- Alternativ kann das Rendering des Switches für einige Sprachen sein "I" und "O" kulturellen vertraut zu machen, aber Sie sollten immer noch die Bezeichnung "On" oder "Off" zu lesen.

### <a name="translation-services"></a>Übersetzungsdienste

#### <a name="machine-translation"></a>Maschinelle Übersetzungen

Um Übersetzungsfunktionen in Ihrer app zu erstellen, sollten Sie die [Textübersetzungs-API von Azure](https://azure.microsoft.com/services/cognitive-services/translator-text-api/).

Zu Testzwecken können eines der vielen online-Übersetzungstools Sie lokalisierten Text in Ihrer app während der Entwicklung umfassen:

- [Bing-Übersetzer](https://www.bing.com/translator/)
- [Google Translate](http://translate.google.com/)

Es gibt noch viele andere verfügbar. Die Qualität der maschinellen Übersetzung ist nicht in der Regel gut genug, um eine Anwendung freigeben betrachtet, ohne zuerst überprüft und von professionellen Übersetzer oder Muttersprache getestet.

#### <a name="professional-translation"></a>Professionelle Übersetzung

Es gibt auch professionelle Übersetzung-Dienste, die Ihre Zeichenfolgen und verteilen Sie sie an ihre eigenen Übersetzer, bietet Ihnen das fertige Übersetzungen gegen eine Gebühr.

Einer der bekanntesten Dienste ist [LionBridge](http://www.lionbridge.com/). Die meisten professional-Dienste unterstützen allen gängigen Dateiformaten, die z. B. Zeichenfolgen "," XML "," RESX "und" TEAPOT/PO.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, einige der Konzepte sollten, vor dem Ihrer app über die Internationalisierung und Lokalisierung klicken Sie dann auf Ihre Ressourcen mit vertraut sein und auch erläutert, wie Sie die spracheinstellungen für jede Plattform zu ändern.

Unterschiedliche plattformspezifischen und plattformübergreifende Internationalisierung Verfahren beschrieben, die mit Xamarin möglich sind, können diese Konzepte angewendet werden.

Fahren Sie mit technischen Details für die Plattform, die, der Sie interessiert sind:

- [Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md) plattformübergreifende Lokalisierung mit RESX-Dateien.
- [Xamarin.iOS](~/ios/app-fundamentals/localization/index.md) Lokalisierung der nativen Plattform.
- [Xamarin.Android](~/android/app-fundamentals/localization.md) Lokalisierung der nativen Plattform.



## <a name="related-links"></a>Verwandte Links

- [Übersicht über die Apple-Lokalisierung](https://developer.apple.com/internationalization/)
- [Checkliste für die Lokalisierung von Android](https://developer.android.com/distribute/tools/localization-checklist.html)
- [Bewährte Methoden für die Entwicklung weltweit einsatzfähiger Anwendungen (MSDN)](https://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
