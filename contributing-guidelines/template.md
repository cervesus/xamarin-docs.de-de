---
title: Der Titel dieses Artikels
description: Eine gute, solide Beschreibung dieses Artikels. Der hier gespeicherte Inhalt wird vom Microsoft-Dokumentationssystem mit den Suchergebnissen zurückgegeben. Es sollte sich also um eine vollständige Zweck- und Inhaltsbeschreibung für diese Dokumentdatei handeln.
keywords: Schlüsselwort, Liste, für, interne, Suche
author: conceptdev
ms.author: crdun
ms.date: 02/26/2018
ms.topic: conceptual
ms.assetid: 11111111-2222-3333-4444-555555555555
ms.prod: xamarin
ms.openlocfilehash: 50bcdcc23d1291071a6045544faad9388755116b
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70225463"
---
# <a name="metadata-and-markdown-template"></a>Metadaten und Markdownvorlage

Diese Vorlage enthält Beispiele der Markdownsyntax sowie einen Leitfaden zum Festlegen der Metadaten. Um die Vorlage bestmöglich zu nutzen, müssen Sie sowohl das **unformatierte Markdown** als auch die **gerenderte Ansicht** anzeigen (beispielsweise zeigt das unformatierte Markdown, anders als die gerenderte Ansicht, den Metadatenblock an).

Beim Erstellen einer Markdowndatei sollten Sie diese Vorlage in eine neue Datei kopieren, die Metadaten wie nachstehend angegeben ausfüllen, den H1-Header oben als Titel des Artikels festlegen und den Inhalt löschen.

## <a name="metadata"></a>Metadaten

Der vollständige Metadatenblock mit einer Reihe von Anmerkungen ist nachfolgend gezeigt:

```
---
title: This Article's Title
description: A good, solid description of this article. The content kept here is what the Microsoft Docs system returns with search results, so this had better be a complete description of the purpose and content for this document file
keywords: a, comma, separated, list, of keywords, to, use, for, this, doc, however, this, might, not, be, used, for, anything, much, anymore, but, still, good, to, have, if, you, want
author: conceptdev (your Github user name)
ms.author: crdun (your Microsoft Alias)
ms.date: 02/26/2018 (creation date, in US format: mm/dd/yyyy)
ms.topic: conceptual (one only of the following: conceptual, quickstart, tutorial, overview)
ms.assetid: b39854a6-c523-4a66-bef6-9b5da03bafff (a unique number representing the asset - just use a GUID, you can generate one at https://www.guidgenerator.com/)
ms.prod: xamarin (no reason to change this)
ms.custom: Analytics data, a field that gets imported into SkyEye so you can use it in custom reports
---
```

Wichtige Hinweise:

- Bei einem Metadatenelement **muss** ein Leerzeichen zwischen dem Doppelpunkt (:) und dem Wert stehen.
- Wenn ein optionales Metadatenelement über keinen Wert verfügt, löschen Sie es (lassen Sie es nicht leer, und verwenden Sie nicht „n/v“).
- Doppelpunkte in einem Wert (z.B. einem Titel) unterbrechen den Metadatenparser. In diesem Fall setzen Sie den Titel in doppelte Anführungszeichen (z.B. `title: "Writing .NET Core console apps: An advanced step-by-step guide"`).
- **title**: Dieser Titel wird in Suchergebnissen angezeigt. Der Titel muss nicht mit dem Titel in Ihrem H1-Header übereinstimmen, und er sollte höchstens 60 Zeichen umfassen.
- **author**, **manager**, **ms.reviewer**: Das Feld „author“ sollte den **GitHub-Benutzernamen** des Autors enthalten, nicht dessen Alias.  Andererseits sollten die Felder „manager“ und „ms.reviewer“ Microsoft-Aliase enthalten. „ms.reviewer“ gibt den Namen des PM/dev an, der dem Artikel oder der Funktion zugeordnet ist.
- **ms.assetid**: Dies ist die GUID des Artikels, die für interne Nachverfolgungszwecke wie z.B. Business Intelligence (BI) verwendet wird. Wenn Sie eine neue Markdowndatei erstellen, rufen Sie eine GUID von [https://www.guidgenerator.com](https://www.guidgenerator.com) ab.

## <a name="basic-markdown-gfm-and-special-characters"></a>Grundlegendes Markdown, GFM und Sonderzeichen

Alles grundlegende Markdown sowie GitHub Flavored Markdown (GFM) wird unterstützt. Weitere Informationen dazu finden Sie auf den folgenden Seiten:

- [Grundlegende Markdownsyntax](https://daringfireball.net/projects/markdown/syntax)
- [DocFX Flavored Markdown](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html)
- [GFM-Dokumentation](https://guides.github.com/features/mastering-markdown)

Markdown verwendet Sonderzeichen wie z.B. \*, \`, und \# für die Formatierung. Wenn Sie eines dieser Zeichen in Ihren Inhalt einschließen möchten, müssen Sie einen der folgenden Schritte ausführen:

- Setzen Sie einen umgekehrten Schrägstrich vor das Sonderzeichen, um es mit einem Escapezeichen zu versehen (z.B. `\*` für \*)
- Verwenden Sie den [HTML-Entitätscode](http://www.ascii.cl/htmlcodes.htm) für das Zeichen (z.B. `&#42;` für ein &#42;).

## <a name="file-name"></a>Dateiname

Dateinamen verwenden die folgenden Regeln:
- Es sollten nur Kleinbuchstaben, Zahlen und Bindestriche enthalten sein.
- Keine Leer- oder Interpunktionszeichen. Verwenden Sie die Bindestriche zum Trennen von Wörtern und Zahlen im Dateinamen.
- Verwenden Sie genaue Aktionsverben wie „entwickeln“, „kaufen“, „erstellen“ oder „beheben“. Keine auf „-ing“ endenden Worte dürfen verwendet werden.
- Keine kurzen Worte wie a, and, the, in, or usw. sind erlaubt.
- Muss in Markdown geschrieben werden und die Dateierweiterung .md verwenden.
- Halten Sie Dateinamen einigermaßen kurz. Sie sind Teil der URL für Ihre Artikel.



## <a name="headings"></a>Kopfzeilen

Verwenden Sie die übliche Groß-/Kleinschreibung. Folgendes sollte immer groß geschrieben werden:
- Das erste Wort eines Headers
- Das erste Wort nach einem Doppelpunkt in einem Titel oder einer Überschrift (z.B. „Vorgehensweise: Sortieren eines Arrays“).

Überschriften sollten mithilfe des atx-Stils erstellt werden, d.h. es sollten 1-6 Hash-Zeichen (#) am Anfang der Zeile verwendet werden, um eine Überschrift anzugeben, entsprechend der HTML-Überschriftenebenen H1 bis H6. Beispiele für Header der ersten und zweiten Ebene werden oben verwendet.

In Ihrem Thema **darf** nur eine Überschrift der ersten Ebene (H1) enthalten sein, die als Titel auf der Seite angezeigt wird.

Wenn Ihre Überschrift mit dem Zeichen `#` endet, müssen Sie ein zusätzliches `#`-Zeichen am Ende einfügen, damit der Titel richtig gerendert wird. Beispielsweise `# Async Programming in F# #`.

Überschriften der zweiten Ebene generieren das Inhaltsverzeichnis auf der Seite, das im Abschnitt „In diesem Artikel“ unterhalb des Titels auf der Seite angezeigt wird.

### <a name="third-level-heading"></a>Überschrift der dritten Ebene
#### <a name="fourth-level-heading"></a>Überschrift der vierten Ebene
##### <a name="fifth-level-heading"></a>Überschrift der fünften Ebene
###### <a name="sixth-level-heading"></a>Überschrift der sechsten Ebene

## <a name="text-styling"></a>Textstil

*Kursiv*: wird für Dateien, Ordner, Pfade (lange Pfade sollten in einer eigenen Zeile stehen), neue Begriffe und URLs (sofern diese nicht entsprechend dem Standard als Links gerendert werden) verwendet.

**Fett** wird für Benutzeroberflächenelemente verwendet.

## <a name="links"></a>Links

### <a name="internal-links"></a>Interne Links

Um auf einen Header in der gleichen Markdowndatei zu verlinken (auch als Ankerlinks bekannt), müssen Sie die ID des Headers ermitteln, auf den Sie verlinken möchten. Um die ID zu bestätigen, zeigen Sie die Quelle des gerenderten Artikels an, suchen Sie nach der ID des Headers (z.B. `id="blockquote"`), und erstellen Sie den Link mithilfe von # + ID (z.B. `#blockquote`).
Die ID wird basierend auf dem Headertext automatisch generiert. Wenn also beispielsweise ein eindeutiger Abschnitt `## Step 2` genannt wird, sieht die ID folgendermaßen aus: `id="step-2"`.

- Ein Beispiel: `[Chapter 1](#chapter-1)`

Um auf eine Markdowndatei im gleichen Repository zu verlinken, verwenden Sie [relative Links](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), einschließlich „.md“ am Ende des Dateinamens.

- Ein Beispiel: `[Readme file](../readme.md)`
- Ein Beispiel: `[Welcome to .NET](../docs/welcome.md)`

Um auf einen Header in einer Markdowndatei im gleichen Repository zu verlinken, verwenden Sie die relative Verlinkung + die Hashtag-Verlinkung.

- Ein Beispiel: `[.NET Community](../docs/welcome.md#community)`

### <a name="external-links"></a>Externe Links

Um auf eine externe Datei zu verlinken, verwenden Sie als Link die vollständige URL.

- Ein Beispiel: `[GitHub](http://www.github.com)`

Wenn in einer Markdowndatei eine URL erscheint, wird diese in einen klickbaren Link verwandelt.

- Ein Beispiel: `http://www.github.com`

### <a name="links-to-apis"></a>Links zu APIs

Das Buildsystem verfügt über einige Erweiterungen, die es ermöglichen, auf .NET-APIs zu verlinken, ohne externe Links verwenden zu müssen.
Wenn auf eine API verlinkt wird, können Sie deren eindeutigen Bezeichner (UID) verwenden, der automatisch aus dem Quellcode generiert wird.

Sie können eine der folgenden Syntaxen verwenden:
1. Markdownlink: `[link_text](xref:UID)`
2. Automatischer Link: `<xref:UID>`
3. Kurzform: `@UID`

- Ein Beispiel: `@System.String`
- Ein Beispiel: `[String class](xref:System.String)`

Weitere Informationen zur Verwendung dieser Notation finden Sie unter [Using cross reference (Verwenden von Querverweisen)](https://dotnet.github.io/docfx/tutorial/links_and_cross_references.html#using-cross-reference).

> Im Moment gibt es keine einfache Möglichkeit, nach den UIDs zu suchen. Die beste Möglichkeit, nach der UID für eine API zu suchen, ist, im folgenden Repository zu suchen: [docascode/coreapi](https://github.com/docascode/coreapi). Wir arbeiten daran, in Zukunft über ein besseres System zu verfügen.

Wenn die UID die Sonderzeichen \` und \# enthält, muss der UID-Wert als %60 und %23 entsprechend HTML-codiert werden, wie in den folgenden Beispielen:
- Beispiel: @System.Threading.Tasks.Task\`1 wird zu `@System.Threading.Tasks.Task%601`
- Beispiel: @System.Exception\#ctor wird zu `@System.Exception.%23ctor`

## <a name="lists"></a>Listen

### <a name="ordered-lists"></a>Geordnete Liste

1. Dies
1. Ist
1. Eine
1. Geordnete
1. Liste


#### <a name="ordered-list-with-an-embedded-list"></a>Geordnete Liste mit einer eingebetteten Liste

1. Hier
1. kommt
1. eine
1. eingebettete
    1. Frau Gloria
    1. Professor Bloom
1. geordnete
1. Liste


### <a name="unordered-lists"></a>Ungeordnete Listen

- Dies
- ist
- eine
- Aufzählungs-
- Liste


##### <a name="unordered-list-with-an-embedded-list"></a>Ungeordnete Liste mit einer eingebetteten Liste

- Diese
- Aufzählungs-
- Liste
  - Baronin von Porz
  - Reverend Grün
- enthält
- andere
    1. Oberst von Gatow
    1. Frau Weiß
- Listen


## <a name="horizontal-rule"></a>Horizontale Regel

---

## <a name="tables"></a>Tabellen

| Tabellen        | Sind           | Toll  |
| ------------- |:-------------:| -----:|
| col 3 ist      | rechtsbündig | $1600 |
| col 2 ist      | zentriert      |   $12 |
| col 1 ist der Standard | linksbündig     |    $1 |

Sie können ein [Markdowntool zum Generieren von Tabellen](http://www.tablesgenerator.com/markdown_tables) verwenden, um diese einfacher zu erstellen.


### <a name="inline-code-blocks-with-language-identifier"></a>Inlinecodeblöcke mit Sprachen-ID

Verwenden Sie drei Graviszeichen (\`\`\`) + eine Sprachen-ID, um die sprachenspezifische Farbcodierung für einen Codeblock anzuwenden. Hier finden Sie die gesamte Liste der [GFM-Sprachen-IDs](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C&#9839;

```c#
using System;
namespace HelloWorld
{
    class Hello
    {
        static void Main()
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```

#### <a name="xml"></a>xml

```xml
<dict>
    <key>CFBundleURLSchemes</key>
    <array>
        <string>evolveCountdown</string>
    </array>
```

#### <a name="xaml"></a>XAML

```xaml
<Label Text="Hello, XAML!"
        FontSize="Large"
        FontAttributes="Bold"
        TextColor="Blue" />
```

#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="generic-code-block"></a>Generischer Codeblock

Verwenden Sie drei Graviszeichen (&#96;&#96;&#96;) für die Codierung mit generischem Codeblock.

> Die empfohlene Vorgehensweise ist, Codeblöcke mit Sprachen-IDs wie im vorherigen Abschnitt beschrieben zu verwenden, um sicherzustellen, dass auf der Dokumentationsseite die richtige Syntax hervorgehoben wird. Verwenden Sie generische Codeblöcke nur bei Bedarf.

```
function fancyAlert(arg) {
    if(arg) {
        $.docs({div:'#foo'})
    }
}
```

### <a name="inline-code"></a>Inlinecode

Verwenden Sie Graviszeichen (&#96;) für `inline code`. Verwenden Sie Inlinecode für Befehlszeilenbefehle, Datenbanktabellennamen und -spaltennamen sowie Sprachschlüsselwörter.

## <a name="blockquotes"></a>Blockquotes

> Die Dürre hatte schon zehn Millionen Jahre angehalten, und die Herrschaft der Schrecklichen Saurier war lange vorbei. Hier am Äquator, auf dem Kontinent, der eines Tages Afrika heißen würde, hatte der Existenzkampf ein neues Stadium von Grausamkeit erreicht und ein Gewinner war noch nicht abzusehen. In diesem ausgetrockneten, ausgedörrten Land konnte nur der Kleinste oder der Schnellste oder der Zäheste gedeihen oder zu überleben hoffen.

## <a name="images"></a>Bilder

### <a name="static-image-or-animated-gif"></a>Statisches Bild oder animiertes GIF

```
![this is the alt text](images/dotnet.png)
```

![Reaktionsfähiger Entwurf](images/responsivedesign.gif)

### <a name="linked-image"></a>Verlinktes Bild

```
[![alt text for linked image](images/dotnet.png)](https://dot.net)
```

## <a name="videos"></a>Videos

### <a name="channel-9"></a>Channel 9

Verwenden Sie diese spezielle Blockquote-Syntax:

```
> [!Video https://channel9.msdn.com/Shows/On-NET/Shipping-NET-Core-RC2--Tools-Preview-1/player]
```

Die Channel 9-Video-URL sollte mit `https` beginnen und mit `/player` enden, da andernfalls die gesamte Seite und nicht nur das Video eingebettet wird.

### <a name="youtube"></a>YouTube

Verwenden Sie diese spezielle Blockquote-Syntax:

```
> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]
```

Das YouTube-Video sollte `/embed/` enthalten (entsprechend dem oben angegebenen Format), da andernfalls versucht wird, die gesamte Seite zu rendern, und dies möglicherweise nicht funktioniert.

## <a name="docsmicrosoft-extensions"></a>docs.microsoft-Erweiterungen

docs.microsoft bietet einige zusätzliche Erweiterungen zu GitHub Flavored Markdown.

### <a name="alerts"></a>Benachrichtigungen

Es ist wichtig, die folgenden Warnstile zu verwenden, damit diese im richtigen Stil auf der Dokumentationsseite gerendert werden. Die Rendering-Engine auf GitHub unterscheidet diese jedoch nicht.

#### <a name="note"></a>Hinweis

```
> [!NOTE]
> This is a NOTE
```

#### <a name="warning"></a>Warnung

```
> [!WARNING]
> This is a WARNING
```

#### <a name="tip"></a>Tipp

```
> [!TIP]
> This is a TIP
```

#### <a name="important"></a>Wichtig

```
> [!IMPORTANT]
> This is IMPORTANT
```

Und sie werden wie folgt gerendert:

![Warnstile](images/alerts.png)

### <a name="buttons"></a>Schaltflächen

```
> [!div class="button"]
[button links](../docs/core/index.md)
```

Ein Beispiel für Schaltflächen in Aktion finden Sie in der [Intune-Dokumentation](https://docs.microsoft.com/intune/get-started/choose-how-to-enroll-devices).

### <a name="selectors"></a>Selektoren

```
> [!div class="op_single_selector"]
- [macOS](../docs/core/tutorials/using-on-macos.md)
- [Windows](../docs/core/tutorials/using-on-windows.md)
```

Ein Beispiel für Selektoren in Aktion finden Sie in der [Intune-Dokumentation](https://docs.microsoft.com/intune/deploy-use/what-to-tell-your-end-users-about-using-microsoft-intune#how-your-end-users-get-their-apps).

### <a name="step-by-steps"></a>Schritt-für-Schritt-Funktionen

```
> [!div class="step-by-step"]
[Pre](../docs/csharp/expression-trees-interpreting.md)
[Next](../docs/csharp/expression-trees-translating.md)
```

Ein Beispiel für Schritt-für-Schritt-Funktionen in Aktion finden Sie in der [Dokumentation für Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/deploy-use/install-ata-step2).
