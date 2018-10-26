---
title: MonoTouch.Dialog-JSON-Markup
description: Dieses Dokument beschreibt die JSON-Syntax, die zum Erstellen einer Xamarin.iOS-Benutzeroberfläche mit MonoTouch.Dialog verwendet werden kann.
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: d084094ab52e317fbb42f6b8c8c553d9d6158251
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106870"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog-JSON-Markup

Diese Seite beschreibt das Json-Markup, das von der MonoTouch.Dialog akzeptiert [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Lassen Sie uns mit einem Beispiel starten. Im folgenden finden eine vollständige Json-Datei, die an JsonElement übergeben werden kann.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
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

Das obenstehende Markup erzeugt die folgende Benutzeroberfläche:

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "Die Benutzeroberfläche, die durch das Markup erstellt")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Jedes Element in der Struktur kann die Eigenschaft enthalten `"id"`. Es kann zur Laufzeit zum Verweisen auf einzelne Abschnitte oder Elemente, die unter Verwendung des Indexers JsonElement zur Verfügung. Und zwar so:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>Syntax der Root-Element

Das Stammelement enthält die folgenden Werte:

-  `title`
-  `sections` (optional)


Das Stammelement darf in einem Abschnitt als ein Element aus, um einen geschachtelten Controller zu erstellen. In diesem Fall die zusätzliche Eigenschaft `"type"` muss festgelegt werden `"root"`

 <a name="url" />


### <a name="url"></a>url

Wenn die `"url"` Eigenschaft wird festgelegt, wenn der Benutzer auf diese RootElement tippt, wird der Code wird eine Datei aus der angegebenen Url angefordert und veranlasst den Inhalt der neue Informationen angezeigt. Hiermit können Sie zum Erstellen die Benutzeroberfläche aus abhängig davon, was der Benutzer tippt erweitern.

 <a name="group" />


### <a name="group"></a>Gruppe

Wenn festgelegt, dadurch Groupname für das Stammelement wird. Gruppennamen werden verwendet, um eine Zusammenfassung auszuwählen, die als Wert für das Stammelement aus einem der geschachtelte Elemente im Element angezeigt wird. Dies ist entweder der Wert eines Kontrollkästchens oder den Wert eines Optionsfelds.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

Identifiziert das Radio-Element, das in geschachtelte Elemente ausgewählt ist

 <a name="title" />


### <a name="title"></a>Titel

Wenn vorhanden, kann er den Titel für die RootElement verwendet werden.

 <a name="type" />


### <a name="type"></a>Typ

Muss festgelegt werden, um `"root"` Wenn dies in einem Abschnitt angezeigt wird (Dies wird verwendet, Controller zu schachteln).

 <a name="sections" />


### <a name="sections"></a>Abschnitte

Dies ist ein JSON-Array mit den einzelnen Abschnitten

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Abschnittssyntax

Der Abschnitt enthält:

-  `header` (optional)
-  `footer` (optional)
-  `elements`-Array


 <a name="header" />


### <a name="header"></a>header

Falls vorhanden, wird der Text der Überschrift als Beschriftung des Abschnitts angezeigt.

 <a name="footer" />


### <a name="footer"></a>Fußzeile

Falls vorhanden, wird die Fußzeile am unteren Rand im Abschnitt angezeigt.

 <a name="elements" />


### <a name="elements"></a>Elemente

Dies ist ein Array von Elementen. Jedes Element muss mindestens ein Schlüssel, enthalten die `"type"` Schlüssel, um die Art des zu erstellenden Elements verwendet wird.
Einige Elemente Teilen gemeinsamen Eigenschaften wie `"caption"` und `"value"`. Hierbei handelt es sich um die Liste der unterstützten Elemente:

-  `string` Elemente (beide mit und ohne Formatierung)
-  `entry` Zeilen ("Regular" oder "Kennwort")
-  `boolean` Werte, die (mithilfe der Switches oder Bilder)


Zeichenfolgenelementen können verwendet werden als Schaltflächen durch die Bereitstellung einer Methode aufrufen, wenn der Benutzer auf die Zelle oder die Zugriffsmethode tippt,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Rendern von Elementen

Die Rendern von Elementen basieren auf der C# StringElement und StyledStringElement und sie können Informationen auf verschiedene Weise zu rendern, und es ist möglich, Berichte auf verschiedene Weise zu rendern. Die einfachste Elemente können wie folgt erstellt werden:

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

Dadurch sehen Sie eine einfache Zeichenfolge mit allen Standardeinstellungen: Schriftart, Hintergrund, Textfarbe und Ergänzungen. Es ist möglich, Aktionen, die diese Elemente einbinden und machen sie das Verhalten wie Schaltflächen durch Festlegen der `"ontap"` Eigenschaft oder das `"onaccessorytap"` Eigenschaften:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Die oben genannten wird die Methode "ShowPhotos" in der Klasse "Acme.PhotoLibrary" aufgerufen. Die `"onaccessorytap"` ist ähnlich, aber es werden nur aufgerufen, wenn der Benutzer auf die Zugriffsmethode anstatt zu tippen auf die Zelle tippt. Um dies zu ermöglichen, müssen Sie auch die Zugriffsmethode festlegen:

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Rendern von Elementen kann zwei Zeichenfolgen gleichzeitig angezeigt, eine ist die Beschriftung und einem anderen ist der Wert. Wie diese Zeichenfolgen gerendert werden abhängig von den Stil, können Sie dies mithilfe der `"style"` Eigenschaft. Standardmäßig wird die Beschriftung auf der linken Seite, und der Wert auf der rechten Seite angezeigt. Stil für Weitere Informationen finden Sie im Abschnitt. Farben werden codiert mit dem '#'-Symbol, gefolgt von Hexadezimalzahlen, die die Werte für die roten, grünen, blauen und vielleicht alpha-Werte darstellen. Die Inhalte können in Kurzform (3 oder 4 hexadeximale Zahlen) codiert werden, die RGB oder RGBA-Werte darstellt. Oder die Langform (6 oder 8 Ziffern) die RGB oder RGBA-Werte darstellen. Die Kurzversion ist eine Kurzform, auf das gleiche hexadezimale Zeichen doppelt schreiben. Daher ist die Konstante "#1bc"-Markierungen Rot = 0x11, Grün = 0xbb und Blau = 0xcc. Wenn der alpha-Wert nicht vorhanden ist, ist die Farbe nicht transparent. Einige Beispiele:

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>Zubehör

Bestimmt die Art der Zubehör, angezeigt in Ihrer Renderingelement, die möglichen Werte sind:

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Wenn der Wert nicht vorhanden ist, wird keine Zubehör angezeigt.

 <a name="background" />


### <a name="background"></a>Hintergrund

Die Background-Eigenschaft legt die Hintergrundfarbe der Zelle fest. Der Wert ist entweder eine URL zu einem Bild (in diesem Fall das Async-Image-Downloadprogramm wird aufgerufen, und der Hintergrund wird aktualisiert, sobald das Image heruntergeladen wurde) oder eine Farbe, die mit der Farbe-Syntax angegeben sein.

 <a name="caption" />


### <a name="caption"></a>Beschriftung

Die wichtigsten Zeichenfolge auf das Renderingelement angezeigt werden sollen. Die Schriftart und Farbe können durch Festlegen von angepasst werden die `"textcolor"` und `"font"` Eigenschaften. Das Darstellungsformat richtet sich nach der `"style"` Eigenschaft.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>Farbe und detailcolor

Die Farbe, die für den Haupttext oder der ausführliche Fehlertext verwendet werden.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>DetailFont- und Schriftart

Die Schriftart für die Beschriftung oder den Text Details verwenden. Das Format einer Schriftart-Spezifikation ist der Schriftartname, optional gefolgt von einem Bindestrich und den Schriftgrad.
Im folgenden finden gültige Schriftart-Spezifikationen:

-  "Helvetica"
-  "Helvetica-14"


 <a name="linebreak" />


### <a name="linebreak"></a>linebreak

Bestimmt, wie Zeilen unterteilt sind. Mögliche Werte sind:

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


Beide `character-wrap` und `word-wrap` kann verwendet werden, zusammen mit den `"lines"` Eigenschaft auf NULL setzen, der Renderingelement in ein Element mit mehreren Zeilen zu aktivieren.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ONTAP und onaccessorytap

Diese Eigenschaften müssen auf den Namen einer statischen Methode, in Ihrer Anwendung verweisen, die ein Objekt als Parameter akzeptiert. Bei der Erstellung Ihrer Hierarchie, die mit den Methoden JsonDialog.FromFile oder JsonDialog.FromJson können Sie einen Objekt mit optionalen Wert übergeben. Dieser Objektwert wird dann an Ihre Methoden übergeben. Sie können dies verwenden, einen Kontext an die statische Methode zu übergeben. Zum Beispiel:

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

Wenn dies 0 (null) festgelegt ist, wird sie die Auto-Elementgröße je nach Inhalt der Zeichenfolgen enthalten sein. Damit dies funktioniert, müssen Sie auch Festlegen der `"linebreak"` Eigenschaft `"character-wrap"` oder `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>style

Das Format bestimmt die Art der Zellenstil entspricht, die beim Rendern des Inhalts verwendet werden und entsprechen der UITableViewCellStyle-Enumerationswerte.
Mögliche Werte sind:

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : Text mit einem Untertitel.


 <a name="subtitle" />


### <a name="subtitle"></a>Untertitel

Der Wert, der für den Untertitel verwendet. Dies ist eine Verknüpfung zum Festlegen des Stils zu `"subtitle"` und legen Sie die `"value"` Eigenschaft in eine Zeichenfolge.
Dies wird sowohl mit einem einzelnen Eintrag.

 <a name="textcolor" />


### <a name="textcolor"></a>TextColor

Die Farbe für den Text zu verwenden.

 <a name="value" />


### <a name="value"></a>Wert

Der sekundäre Wert, der Rendering-Element angezeigt werden soll. Das Layout der betroffen ist die `"style"` festlegen. Die Schriftart und Farbe können durch Festlegen von angepasst werden die `"detailfont"` und `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Booleschen Elementen

Boolesche Elementen sollten legen Sie den Typ auf `"bool"`, darf eine `"caption"` zum Anzeigen und die `"value"` auf "true" oder "false" festgelegt ist. Wenn die `"on"` und `"off"` Eigenschaften festgelegt werden, sie wird angenommen, dass Images. Die Images sind relativ zum aktuellen Arbeitsverzeichnis in der Anwendung aufgelöst. Wenn Sie Relative Paket-Dateien verweisen möchten, können Sie mithilfe der `"~"` als Verknüpfung zu das Verzeichnis für Anwendungspaket darstellen. Z. B. `"~/favorite.png"` werden die favorite.png, die in der Paketdatei enthalten ist. Zum Beispiel:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>Typ

Typ kann festgelegt werden entweder `"boolean"` oder `"checkbox"`. Wenn festgelegt, in einen booleschen Wert ihn eine UISlider oder Images verwenden möchten (wenn beide `"on"` und `"off"` festgelegt sind). Wenn auf das Kontrollkästchen festgelegt ist, ein Kontrollkästchen verwendet werden. Die `"group"` Eigenschaft kann verwendet werden, um ein boolesches Element der Zugehörigkeit zu einer bestimmten Gruppe zu markieren. Dies ist hilfreich, wenn der enthaltende Stamm verfügt auch über eine `"group"` Eigenschaft als Stamm werden die Ergebnisse mit der Anzahl aller boolesche Werte (oder Kontrollkästchen), die zur selben Gruppe gehören zusammengefasst.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Elemente

Sie verwenden die Elemente, damit der Benutzer Daten eingeben kann. Ist der Typ Eintragselemente `"entry"` oder `"password"`. Die `"caption"` -Eigenschaftensatz auf den Text auf der rechten Seite angezeigt und die `"value"` auf den ursprünglichen Wert festgelegt ist, auf den Eintrag festgelegt werden soll. Die `"placeholder"` wird verwendet, um einen Hinweis für dem Benutzer für leere Einträge anzuzeigen (es wird dargestellt ausgegraut). Hier einige Beispiele:

```csharp
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

Bestimmt den AutoKorrektur-Stil für den Eintrag zu verwenden. Die möglichen Werte sind "true" oder "false" (oder die Zeichenfolgen `"yes"` und `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>Groß-/Kleinschreibung

Schreibweise für den Eintrag zu verwenden. Mögliche Werte sind:

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>Beschriftung

Die Beschriftung für den Eintrag zu verwenden

 <a name="keyboard" />


### <a name="keyboard"></a>Tastatur

Der Tastaturtyp für die Dateneingabe zu verwenden. Mögliche Werte sind:

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>Platzhalter

Der Hinweistext, der angezeigt wird, wenn der Eintrag für einen leeren Wert aufweist.

 <a name="return-key" />


### <a name="return-key"></a>Return-Taste

Die Bezeichnung für die return-Taste. Mögliche Werte sind:

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>Wert

Der Anfangswert für den Eintrag

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>Radio-Elemente

Radio-Elemente aufweisen `"radio"`. Die ausgewählten Elements wird ausgewählt, durch die `radioselected` Eigenschaft für das enthaltende Root-Element.
Darüber hinaus, wenn ein Wert, für festgelegt ist die `"group"` -Eigenschaft dieses Optionsfeld, gehört zu dieser Gruppe.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Datum und der Zeitelemente

Die Elementtypen `"datetime"`, `"date"` und `"time"` zum Rendern von Datumsangaben mit Uhrzeiten, Datumsangaben oder Uhrzeiten verwendet werden. Diese Elemente werden als Parameter eine Beschriftung und einem Wert. Der Wert kann in einem beliebigen von der Funktion DateTime.Parse von .NET unterstützten Format geschrieben werden. Beispiel:

```csharp
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

Sie können eine Zelle erstellen, die beim Tippen auf ein, das den Inhalt der angegebenen URL, rendert UIWebView einbetten entweder lokal oder remote mithilfe der `"html"` Typ. Nur zwei Eigenschaften für dieses Element `"caption"` und `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
