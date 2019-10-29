---
title: MonoTouch.Dialog-JSON-Markup
description: In diesem Dokument wird die JSON-Syntax beschrieben, die zum Erstellen einer xamarin. IOS-Benutzeroberfläche mithilfe von MonoTouch. Dialog verwendet werden kann.
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 84698ab769156726982c4d5a38d5f284bdc30328
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002227"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog-JSON-Markup

Diese Seite beschreibt das JSON-Markup, das von MonoTouch. Dialog Feld- [jsonelement](xref:MonoTouch.Dialog.JsonElement) akzeptiert wird.

Beginnen wir mit einem Beispiel. Im folgenden finden Sie eine umfassende JSON-Datei, die an jsonelement übermittelt werden kann.

```json
{     
    "title": "Json Sample",
    "sections": [ 
        {
            "header": "Booleans",
            "footer": "Slider or image-based",
            "id": "first-section",
            "elements": [
                { 
                    "type": "boolean",
                    "caption": "Demo of a Boolean",
                    "value": true
                }, {
                    "type": "boolean",
                    "caption": "Boolean using images",
                    "value": false,
                    "on": "favorite.png",
                    "off": "~/favorited.png"
                }, {
                    "type": "root",
                    "title": "Tap for nested controller",
                    "sections": [
                        {
                            "header": "Nested view!",
                            "elements": [
                                {
                                    "type": "boolean",
                                    "caption": "Just a boolean",
                                    "id": "the-boolean",
                                    "value": false
                                }, {
                                    "type": "string",
                                    "caption": "Welcome to the nested controller"
                                }
                            ]
                        }
                    ]
                }
            ]
        }, {
            "header": "Entries",
            "elements" : [
                {
                    "type": "entry",
                    "caption": "Username",
                    "value": "",
                    "placeholder": "Your account username"
                }
            ]
        }
    ]
}
```

Mit dem obigen Markup wird die folgende Benutzeroberfläche erzeugt:

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "The UI created by the given markup")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Jedes Element in der Struktur kann die Eigenschaften `"id"`enthalten. Es ist möglich, zur Laufzeit auf einzelne Abschnitte oder Elemente mit dem jsonelement-Indexer zu verweisen. Und zwar so:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement;
```

 <a name="Root_Element_Syntax" />

## <a name="root-element-syntax"></a>Root-Element Syntax

Das root-Element enthält die folgenden Werte:

- `title`
- `sections` (optional)

Das root-Element kann in einem Abschnitt als Element zum Erstellen eines geschachtelten Controllers angezeigt werden. In diesem Fall muss die zusätzliche Eigenschaft `"type"` auf festgelegt werden `"root"`

 <a name="url" />

### <a name="url"></a>url

Wenn die `"url"`-Eigenschaft festgelegt ist, fordert der Code, wenn der Benutzer auf dieses rootelements tippt, eine Datei von der angegebenen URL an und macht die Inhalte der neuen Informationen angezeigt. Sie können dies verwenden, um die Benutzeroberfläche vom Server zu erweitern, je nachdem, was der Benutzer tippt.

 <a name="group" />

### <a name="group"></a>Gruppe

Wenn festgelegt, wird der GroupName für das Stamm Element festgelegt. Gruppennamen werden verwendet, um eine Zusammenfassung auszuwählen, die als Wert des Stamm Elements aus einem der im-Element gruppierten Elemente angezeigt wird. Dies ist entweder der Wert eines Kontrollkästchens oder der Wert eines Options Felds.

 <a name="radioselected" />

### <a name="radioselected"></a>Radio ausgewählt

Identifiziert das Optionsfeld, das in den in der Liste enthaltenen Elementen ausgewählt ist.

 <a name="title" />

### <a name="title"></a>Titel

Wenn vorhanden, ist dies der Titel, der für das rootelor verwendet wird.

 <a name="type" />

### <a name="type"></a>Typ

Muss auf `"root"` festgelegt werden, wenn dies in einem Abschnitt angezeigt wird (dient zum Schachteln von Controllern).

 <a name="sections" />

### <a name="sections"></a>Abschnitte

Dabei handelt es sich um ein JSON-Array mit einzelnen Abschnitten.

 <a name="Section_Syntax" />

## <a name="section-syntax"></a>Abschnitts Syntax

Der-Abschnitt enthält:

- `header` (optional)
- `footer` (optional)
- `elements`-Array

 <a name="header" />

### <a name="header"></a>header

Falls vorhanden, wird der Header Text als Beschriftung des Abschnitts angezeigt.

 <a name="footer" />

### <a name="footer"></a>Fußzeile

Falls vorhanden, wird der Fußzeile unten im Abschnitt angezeigt.

 <a name="elements" />

### <a name="elements"></a>Elemente

Dies ist ein Array von-Elementen. Jedes Element muss mindestens einen Schlüssel enthalten, den `"type"` Schlüssel, der zum Identifizieren der Art des zu erstellenden Elements verwendet wird.
Einige der Elemente haben einige gemeinsame Eigenschaften wie `"caption"` und `"value"`. Dies ist die Liste der unterstützten Elemente:

- `string` Elemente (sowohl mit als auch ohne Format)
- `entry` Zeilen (regulär oder Kennwort)
- `boolean` Werte (mithilfe von Switches oder Bildern)

Zeichen folgen Elemente können als Schaltflächen verwendet werden, indem eine Methode bereitgestellt wird, die aufgerufen wird, wenn der Benutzer entweder auf die Zelle oder das Zubehör tippt,

 <a name="Rendering_Elements" />

## <a name="rendering-elements"></a>Rendern von Elementen

Die renderingelemente basieren auf C# dem stringelement und styledstringelement und können Informationen auf verschiedene Weise rendern, und es ist möglich, Sie auf verschiedene Weise zu rendern. Die einfachsten Elemente können wie folgt erstellt werden:

```json
{
    "type": "string",
    "caption": "Json Serializer"
}
```

Dadurch wird eine einfache Zeichenfolge mit allen Standardwerten angezeigt: Schriftart, Hintergrund, Textfarbe und Dekorationen. Es ist möglich, Aktionen in diese Elemente einzuschließen und Sie wie Schaltflächen zu Verhalten, indem Sie die `"ontap"`-Eigenschaft oder die `"onaccessorytap"` Eigenschaften festlegen:

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos"
}
```

Im obigen Beispiel wird die Methode "showphotos" in der Klasse "acme. PhotoLibrary" aufgerufen. Der `"onaccessorytap"` ist ähnlich, wird jedoch nur aufgerufen, wenn der Benutzer auf das Zubehör tippt, anstatt auf die Zelle zu tippen. Um dies zu aktivieren, müssen Sie auch das-Zubehör festlegen:

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos",
    "accessory": "detail-disclosure",
    "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Renderingelemente können zwei Zeichen folgen gleichzeitig anzeigen, eine ist die Beschriftung und eine andere den Wert. Wie diese Zeichen folgen gerendert werden, hängt vom Format ab, Sie können dies mit der `"style"`-Eigenschaft festlegen. In der Standardeinstellung wird die Beschriftung auf der linken Seite und der Wert auf der rechten Seite angezeigt. Weitere Informationen finden Sie im Abschnitt zum Stil. Farben werden mit dem Symbol "#" gefolgt von hexadezimalen Zahlen codiert, die die Werte für die Werte für Rot, grün, blau und möglicherweise Alpha darstellen. Der Inhalt kann in Kurzform (3 oder 4 hexadezimal Ziffern) codiert werden, die entweder RGB-oder RGBA-Werte darstellt. Oder die lange Form (6 oder 8 Ziffern), die entweder RGB-oder RGBA-Werte darstellen. Die kurze Version ist eine Kurzform für das doppelte Schreiben derselben hexadezimal Ziffer. Daher wird die Konstante "#1bc" als "Red = 0x11", "Green = 0xBB" und "Blue = 0xcc" als "Red" bezeichnet. Wenn der Alpha-Wert nicht vorhanden ist, ist die Farbe nicht transparent. Einige Beispiele:

```json
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />

### <a name="accessory"></a>Zubehör

Bestimmt die Art von Zubehör, das in Ihrem Rendering-Element angezeigt werden soll. mögliche Werte:

- `checkmark`
- `detail-disclosure`
- `disclosure-indicator`

Wenn der Wert nicht vorhanden ist, wird kein Zubehör angezeigt.

 <a name="background" />

### <a name="background"></a>Hintergrund

Die Background-Eigenschaft legt die Hintergrundfarbe für die Zelle fest. Der Wert ist entweder eine URL zu einem Bild (in diesem Fall wird das asynchrone Bild-Download Programm aufgerufen, und der Hintergrund wird aktualisiert, sobald das Bild heruntergeladen wurde), oder es kann eine mit der Farb Syntax angegebene Farbe sein.

 <a name="caption" />

### <a name="caption"></a>unter

Die Haupt Zeichenfolge, die im Rendering-Element angezeigt werden soll. Die Schriftart und Farbe können durch Festlegen der Eigenschaften `"textcolor"` und `"font"` angepasst werden. Der Renderingstil wird durch die `"style"`-Eigenschaft bestimmt.

 <a name="color_and_detailcolor" />

### <a name="color-and-detailcolor"></a>Farbe und detailcolor

Die Farbe, die für den Haupttext oder den detaillierten Text verwendet werden soll.

 <a name="detailfont_and_font" />

### <a name="detailfont-and-font"></a>detailschriftart und Schriftart

Die Schriftart, die für die Beschriftung oder den Detail Text verwendet werden soll. Das Format einer Schriftart Spezifikation ist der Schriftart Name, gefolgt von einem Bindestrich und der Punktgröße.
Die folgenden Schriftart Spezifikationen sind gültig:

- Helvetica
- "Helvetica-14"

 <a name="linebreak" />

### <a name="linebreak"></a>linebreak

Bestimmt, wie Linien untergliedert werden. Mögliche Werte sind:

- `character-wrap`
- `clip`
- `head-truncation`
- `middle-truncation`
- `tail-truncation`
- `word-wrap`

Sowohl `character-wrap` als auch `word-wrap` können verwendet werden, wobei die Eigenschaft `"lines"` auf NULL festgelegt ist, um das renderingelement in ein mehrzeilige Element umzuwandeln.

 <a name="ontap_and_onaccessorytap" />

### <a name="ontap-and-onaccessorytap"></a>ONTAP und onaccessorytap

Diese Eigenschaften müssen auf einen statischen Methodennamen in der Anwendung verweisen, der ein Objekt als Parameter annimmt. Wenn Sie die-Hierarchie mithilfe der Methoden "jsondialog. FromFile" oder "jsondialog. fromjson" erstellen, können Sie einen optionalen Objektwert übergeben. Dieser Objektwert wird dann an ihre Methoden weitergegeben. Sie können damit einen Kontext an die statische Methode übergeben. Beispiel:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />

### <a name="lines"></a>Linien

Wenn dieser Wert auf 0 (null) festgelegt ist, wird die automatische Größe des Elements abhängig vom Inhalt der enthaltenen Zeichen folgen festgelegt. Damit dies funktioniert, müssen Sie auch die `"linebreak"`-Eigenschaft auf `"character-wrap"` oder `"word-wrap"`festlegen.

 <a name="style" />

### <a name="style"></a>style

Der Stil bestimmt die Art des Zellstils, der zum Rendering des Inhalts verwendet wird, und entspricht den uitableviewcellstyle-Enumerationswerten.
Mögliche Werte sind:

- `"default"`
- `"value1"`
- `"value2"`
- `"subtitle"`: Text mit einem Untertitel.

 <a name="subtitle" />

### <a name="subtitle"></a>Titel

Der Wert, der für den Untertitel verwendet werden soll. Dies ist eine Verknüpfung, um den Stil auf `"subtitle"` festzulegen und die `"value"`-Eigenschaft auf eine Zeichenfolge festzulegen.
Dies erfolgt mit einem einzelnen Eintrag.

 <a name="textcolor" />

### <a name="textcolor"></a>TextColor

Die Farbe, die für den Text verwendet werden soll.

 <a name="value" />

### <a name="value"></a>Wert

Der sekundäre Wert, der im Rendering-Element angezeigt werden soll. Das Layout dieses ist von der `"style"` Einstellung betroffen. Die Schriftart und Farbe können durch Festlegen der `"detailfont"` und `"detailcolor"`angepasst werden.

 <a name="Boolean_Elements" />

## <a name="boolean-elements"></a>Boolesche Elemente

Boolesche Elemente müssen den Typ auf `"bool"`festlegen, können eine anzuzeigende `"caption"` enthalten, und die `"value"` ist entweder auf "true" oder "false" festgelegt. Wenn die Eigenschaften `"on"` und `"off"` festgelegt sind, werden Sie als Bilder angenommen. Die Bilder werden relativ zum aktuellen Arbeitsverzeichnis in der Anwendung aufgelöst. Wenn Sie auf Paket relative Dateien verweisen möchten, können Sie die `"~"` als Verknüpfung verwenden, um das Anwendungs Bündel Verzeichnis darzustellen. `"~/favorite.png"` ist beispielsweise der in der Paketdatei enthaltene Favorit. png. Beispiel:

```json
{ 
    "type": "boolean",
    "caption": "Demo of a Boolean",
    "value": true
},

{
    "type": "boolean",
    "caption": "Boolean using images",
    "value": false,
    "on": "favorite.png",
    "off": "~/favorited.png"
}
```

 <a name="type" />

### <a name="type"></a>Typ

Der Typ kann entweder auf `"boolean"` oder `"checkbox"`festgelegt werden. Wenn er auf einen booleschen Wert festgelegt ist, wird ein UISlider oder Bilder verwendet (wenn sowohl `"on"` als auch `"off"` festgelegt sind). Wenn dieses Kontrollkästchen aktiviert ist, wird ein Kontrollkästchen verwendet. Die `"group"`-Eigenschaft kann verwendet werden, um ein boolesches Element so zu kennzeichnen, dass es zu einer bestimmten Gruppe gehört. Dies ist nützlich, wenn der enthaltende Stamm auch über eine `"group"`-Eigenschaft verfügt, da der Stamm die Ergebnisse mit der Anzahl aller booleschen Werte (oder Kontrollkästchen) zusammenfasst, die zur selben Gruppe gehören.

 <a name="Entry_Elements" />

## <a name="entry-elements"></a>Einstiegs Elemente

Sie verwenden Eintrags Elemente, um dem Benutzer die Eingabe von Daten zu ermöglichen. Der Typ für Eintrags Elemente ist entweder `"entry"` oder `"password"`. Die `"caption"`-Eigenschaft wird auf den Text festgelegt, der auf der rechten Seite angezeigt wird, und die `"value"` wird auf den Anfangswert festgelegt, um den Eintrag auf festzulegen. Der `"placeholder"` wird verwendet, um dem Benutzer einen Hinweis für leere Einträge anzuzeigen (er ist abgeblendet dargestellt). Hier einige Beispiele:

```json
{
    "type": "entry",
    "caption": "Username",
    "value": "",
    "placeholder": "Your account username"
}, {
    "type": "password",
    "caption": "Password",
    "value": "",
    "placeholder": "You password"
}, {
    "type": "entry",
    "caption": "Zip Code",
    "value": "01010",
    "placeholder": "your zip code",
    "keyboard": "numbers"
}, {
    "type": "entry",
    "return-key": "route",
    "caption": "Entry with 'route'",
    "placeholder": "captialization all + no corrections",
    "capitalization": "all",
    "autocorrect": "no"
}
```

 <a name="autocorrect" />

### <a name="autocorrect"></a>AutoKorrektur

Bestimmt den für den Eintrag zu verwendenden automatischen Korrektur Stil. Mögliche Werte sind "true" oder "false" (oder die `"yes"` "String" und "`"no"`").

 <a name="capitalization" />

### <a name="capitalization"></a>Groß-/Kleinschreibung

Der für den Eintrag zu verwendende groß Schriftstil. Mögliche Werte sind:

- `all`
- `none`
- `sentences`
- `words`

 <a name="caption" />

### <a name="caption"></a>unter

Die Beschriftung, die für den Eintrag verwendet werden soll.

 <a name="keyboard" />

### <a name="keyboard"></a>Tastatur

Der für die Dateneingabe zu verwendende Tastatur-Typ. Mögliche Werte sind:

- `ascii`
- `decimal`
- `default`
- `email`
- `name`
- `numbers`
- `numbers-and-punctuation`
- `twitter`
- `url`

 <a name="placeholder" />

### <a name="placeholder"></a>Platzhalter

Der Hinweis Text, der angezeigt wird, wenn der Eintrag einen leeren Wert aufweist.

 <a name="return-key" />

### <a name="return-key"></a>Return-Key

Die Bezeichnung, die für die Rückgabetaste verwendet wird. Mögliche Werte sind:

- `default`
- `done`
- `emergencycall`
- `go`
- `google`
- `join`
- `next`
- `route`
- `search`
- `send`
- `yahoo`

 <a name="value" />

### <a name="value"></a>Wert

Der Anfangswert für den Eintrag.

 <a name="Radio_Elements" />

## <a name="radio-elements"></a>Radio-Elemente

Radio-Elemente haben den Typ `"radio"`. Das Element, das ausgewählt wird, wird von der `radioselected`-Eigenschaft auf dem enthaltenden root-Element ausgewählt.
Wenn außerdem ein Wert für die `"group"`-Eigenschaft festgelegt ist, gehört dieses Optionsfeld zu dieser Gruppe.

 <a name="Date_and_Time_Elements" />

## <a name="date-and-time-elements"></a>Datums-und Uhrzeit Elemente

Die Elementtypen `"datetime"`, `"date"` und `"time"` werden verwendet, um Datumsangaben mit Uhrzeiten, Datumsangaben oder Uhrzeiten zu erzeugen. Diese Elemente akzeptieren als Parameter eine Beschriftung und einen Wert. Der Wert kann in einem beliebigen Format geschrieben werden, das von der .NET DateTime. Analyse-Funktion unterstützt wird. Beispiel:

```json
"header": "Dates and Times",
"elements": [
    {
        "type": "datetime",
        "caption": "Date and Time",
        "value": "Sat, 01 Nov 2008 19:35:00 GMT"
    }, {
        "type": "date",
        "caption": "Date",
        "value": "10/10"
    }, {
        "type": "time",
        "caption": "Time",
        "value": "11:23"
    }                       
]
```

 <a name="Html/Web_Element" />

## <a name="htmlweb-element"></a>HTML/Web-Element

Sie können eine Zelle erstellen, die beim Tippen eine UIWebView einbettet, die den Inhalt einer angegebenen URL, entweder lokal oder Remote, mithilfe des `"html"` Typs rendert. Die einzigen beiden Eigenschaften für dieses Element sind `"caption"` und `"url"`:

```json
{
    "type": "html",
    "caption": "Miguel's blog",
    "url": "https://tirania.org/blog" 
}
```
