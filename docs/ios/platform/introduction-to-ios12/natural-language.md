---
title: Mithilfe von natürlicher sprachframework mit Xamarin.iOS
description: Dieses Dokument beschreibt die natürlichen Sprachen-Framework. In iOS-12 eingeführt wurde, ist das natürliche Sprache-Framework die bevorzugte iOS-API, die für die Sprache verwendet, Wortarten-ID und Erkennung benannter Entitäten verwendet.
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/20/2018
ms.openlocfilehash: 0b3fb7d467ae64e2cbfdb61644b1537bc5ae1161
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233067"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>Mithilfe von natürlicher sprachframework mit Xamarin.iOS

In iOS-12 eingeführt wurde, ermöglicht die natürlichen Sprachen-Framework Verarbeitung auf dem Gerät natürlicher Sprache. Sprache erkennen, Tokenisierung und tagging unterstützt. Tokenisierung teilt Text in der Komponente Wörter, Sätze oder Absätze; Tags identifiziert Wortarten, Personen, Orten und Organisationen.

Natürlicher Sprachen-Framework kann auch benutzerdefinierte Core ML-Modelle verwenden, zu klassifizieren und Markieren von Text in speziellen Kontexten.

Die [NSLinguisticTagger](xref:Foundation.NSLinguisticTagger) -Klasse ist weiterhin verfügbar. Das Framework natürlicher Sprache ist jedoch der bevorzugte Mechanismus für die Verarbeitung natürlicher Sprache verwenden.

## <a name="sample-app-xamarinnl"></a>Beispiel-app: XamarinNL

Um zu erfahren, wie Sie das Framework natürlicher Sprache mit Xamarin.iOS verwenden, sehen Sie sich die [XamarinNL-Beispiel-app](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL).
Diese Beispiel-app veranschaulicht, wie das Framework natürlicher Sprache:

- [Erkennen von Sprachen](#recognizing-languages).
- [Mit einem Token versehen Text in Wörter und Sätze](#tokenizing-text-into-words-sentences-and-paragraphs).
- [Tag mit dem Namen Entitäten und Wortarten](#tagging-named-entities-and-parts-of-speech).

## <a name="recognizing-languages"></a>Erkennen von Sprachen

Die **Erkennung** Registerkarte der Beispiel-app veranschaulicht, wie ein [`NLLanguageRecognizer`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguageRecognizer/)
die Sprache für einen Textblock zu ermitteln.

> [!NOTE]
> Sprache ist eine bestimmte Art von textklassifizierung. Die natürlichen Sprachen-Framework unterstützt auch benutzerdefinierte textklassifizierung, über das Entwickler bereitgestellte Core ML-Modellen. Weitere Informationen sehen Sie sich die [Einführung in natürlicher Sprachframework](https://developer.apple.com/videos/play/wwdc2018/713/) -Sitzung von WWDC 2018.

### <a name="dominant-language"></a>Vorherrschende Sprache

Tippen Sie auf die **Sprache** Schaltfläche, um die vorherrschende Sprache in die Benutzereingabe zu identifizieren.

Die `HandleDetermineLanguageButtonTap` Methode der `LanguageRecognizerViewController` verwendet das [`GetDominantLanguage`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage/)
Methode eine `NLLanguageRecognizer` zum Abrufen der [`NLLanguage`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguage/)
für die primäre Sprache, die im Text gefunden wird:

```csharp
partial void HandleDetermineLanguageButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        NLLanguage lang = NLLanguageRecognizer.GetDominantLanguage(UserInput.Text);
        DominantLanguageLabel.Text = lang.ToString();
    }
}
```

### <a name="language-probabilities"></a>Language-Wahrscheinlichkeiten

Tippen Sie auf die **Sprache Wahrscheinlichkeiten** Schaltfläche, um eine Liste der Sprache Hypothesen für den in der Benutzereingabe abzurufen.

Die `HandleLanguageProbabilitiesButtonTap` Methode der `LanguageRecognizerViewController` Klasse instanziiert ein `NLLanguageRecognizer` und fordert ihn auf [`Process`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.Process/)
der Benutzer Text. Es ruft dann der Language-Erkennung [`GetNativeLanguageHypotheses`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses)
Methode, die ein Wörterbuch mit den Sprachen und der zugehörigen Wahrscheinlichkeiten abruft. Die `LanguageRecognizerTableViewController` Klasse rendert dann diesen Sprachen und Wahrscheinlichkeiten.

```csharp
partial void HandleLanguageProbabilitiesButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var recognizer = new NLLanguageRecognizer();
        recognizer.Process(UserInput.Text);
        NSDictionary<NSString, NSNumber> probabilities = recognizer.GetNativeLanguageHypotheses(10);
        PerformSegue(ShowLanguageProbabilitiesSegue, this);
    }
}
```

Potenzielle `NLLanguage` Werte sind:

- `Amharic`
- `Arabic`
- `Armenian`
- `Bengali`
- `Bulgarian`
- `Burmese`
- `Catalan`
- `Cherokee`
- `Croatian`
- `Czech`
- `Danish`
- `Dutch`
- `English`
- `Finnish`
- `French`
- `Georgian`
- `German`
- `Greek`
- `Gujarati`
- `Hebrew`
- `Hindi`
- `Hungarian`
- `Icelandic`
- `Indonesian`
- `Italian`
- `Japanese`
- `Kannada`
- `Khmer`
- `Korean`
- `Lao`
- `Malay`
- `Malayalam`
- `Marathi`
- `Mongolian`
- `Norwegian`
- `Oriya`
- `Persian`
- `Polish`
- `Portuguese`
- `Punjabi`
- `Romanian`
- `Russian`
- `SimplifiedChinese`
- `Sinhalese`
- `Slovak`
- `Spanish`
- `Swedish`
- `Tamil`
- `Telugu`
- `Thai`
- `Tibetan`
- `TraditionalChinese`
- `Turkish`
- `Ukrainian`
- `Undetermined`
- `Urdu`
- `Vietnamese`

Eine vollständige Liste der unterstützten Sprachen steht als Teil der [`NLLanguage`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguage/)
Enum-API-Dokumentation.

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>Zerlegen von Text in Wörter, Sätze und Absätzen

Die **Tokenizer** der Beispiel-app veranschaulicht, wie trennen Sie einen Textblock in der Komponente Wörter oder Sätze mit einem [ `NLTokenizer` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTokenizer/).

Tippen Sie auf die **Wörter** oder **Sätze** Schaltfläche, um eine Liste der Token abzurufen. Jedes Token ist ein Wort oder Satz im ursprünglichen Text zugeordnet.

`ShowTokens` teilt die Eingabe des Benutzers in Token durch Aufrufen der [`GetTokens`](https://developer.xamarin.com/api/member/NaturalLanguage.NLTokenizer.GetTokens/)
die Methode von einer `NLTokenizer`. Diese Methode gibt ein Array zurück. [`NSValue`](xref:Foundation.NSValue)
Objekten, die jede umschließen einer `NSRange` Wert, der ein Token im Originaltext entspricht.

```csharp
void ShowTokens(NLTokenUnit unit)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tokenizer = new NLTokenizer(unit);
        tokenizer.String = UserInput.Text;
        var range = new NSRange(0, UserInput.Text.Length);
        NSValue[] tokens = tokenizer.GetTokens(range);
        PerformSegue(ShowTokensSegue, this);
    }
}
```

`LanguageTokenizerTableViewController` Rendert ein einzelnes Token in jeder Zelle der Tabelle an. Extrahiert eine `NSRange` aus einem Token `NSValue`, sucht die entsprechende Zeichenfolge im ursprünglichen Text und eine Bezeichnung auf die tabellenansichtszelle festlegt:

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## <a name="tagging-named-entities-and-parts-of-speech"></a>Tagging benannten Entitäten und Wortarten

Die **Tagger** Registerkarte der XamarinNL Beispiel-app veranschaulicht, wie die [`NLTagger`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTagger/)
die Klasse, die Token einer Eingabezeichenfolge Kategorien zugeordnet werden soll.
Die natürlichen Sprachen-Framework umfasst integrierten Unterstützung für die Erkennung von Personen, Orte, Organisationen und Wortarten.

> [!NOTE]
> Die natürlichen Sprachen-Framework unterstützt auch benutzerdefinierte Tags Schemas über das Entwickler bereitgestellte Core ML-Modellen. Weitere Informationen sehen Sie sich die [Einführung in natürlicher Sprachframework](https://developer.apple.com/videos/play/wwdc2018/713/) -Sitzung von WWDC 2018.

Tippen Sie auf die **benannte Entitäten** oder **Wortarten** Schaltfläche abrufen:

- Ein Array von `NSValue` Objekten, die jede umschließen einer `NSRange` für ein Token im Originaltext.
- Ein Array von [ `NLTag` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTag/) Werte – Kategorien für die `NSValue` Token in der gleichen Arrayindex.

In `LanguageTaggerViewController`, `HandlePartsOfSpeechButtonTap` und `HandleNamedEntitiesButtonTap` jeder Aufruf `ShowTags`, und übergeben Sie zusammen eine [ `NLTagScheme` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTagScheme/) – entweder `NLTagScheme.LexicalClass` (für Wortarten) oder `NLTagScheme.NameType` (für eine benannte Entitäten).

`ShowTags` erstellt eine `NLTagger`, instanziieren es mit einem Array von `NLTagScheme` -Typen für die sie abgefragt werden (in diesem Fall nur der übergegebenen `NLTagScheme` Wert). Anschließend wird mithilfe der [`GetTags`](https://developer.xamarin.com/api/member/NaturalLanguage.NLTagger.GetTags/)
Methode für die `NLTagger` um zu bestimmen, die Tags, die relevant für den Text in der Benutzereingabe.

```csharp
void ShowTags(NLTagScheme tagScheme)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tagger = new NLTagger(new NLTagScheme[] { tagScheme });
        var range = new NSRange(0, UserInput.Text.Length);
        tagger.String = UserInput.Text;

        NLTag[] tags = tagger.GetTags(range, NLTokenUnit.Word, tagScheme, NLTaggerOptions.OmitWhitespace, out NSValue[] ranges);
        NSValue[] tokenRanges = ranges;
        detailViewTitle = tagScheme == NLTagScheme.NameType ? "Named Entities" : "Parts of Speech";

        PerformSegue(ShowEntitiesSegue, this);
    }
}
```

Die Tags werden dann angezeigt, in einer Tabelle durch die `LanguageTaggerTableViewController`.

Potenzielle `NLTag` Werte sind:

- `Adjective`
- `Adverb`
- `Classifier`
- `CloseParenthesis`
- `CloseQuote`
- `Conjunction`
- `Dash`
- `Determiner`
- `Idiom`
- `Interjection`
- `Noun`
- `Number`
- `OpenParenthesis`
- `OpenQuote`
- `OrganizationName`
- `Other`
- `OtherPunctuation`
- `OtherWhitespace`
- `OtherWord`
- `ParagraphBreak`
- `Particle`
- `PersonalName`
- `PlaceName`
- `Preposition`
- `Pronoun`
- `Punctuation`
- `SentenceTerminator`
- `Verb`
- `Whitespace`
- `Word`
- `WordJoiner`

Eine vollständige Liste der unterstützten Tags steht als Teil der [`NLTag`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTag/)
Enum-API-Dokumentation.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-app – XamarinNL](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL)
- [Einführung in natürlicher Sprache-Framework](https://developer.apple.com/videos/play/wwdc2018/713/)
- [Natürlicher Sprache (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)
