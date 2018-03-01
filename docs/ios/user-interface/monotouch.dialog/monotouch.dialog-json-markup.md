---
title: MonoTouch.Dialog Json-Markup
ms.topic: article
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f784497173db6bc3ffa87617765e63fc8d904e5f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json-Markup

Diese Seite beschreibt die Json-Markup, das vom des MonoTouch.Dialog akzeptiert [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Beginnen Sie wir mit einem Beispiel. Im folgenden finden eine vollständige Json-Datei, die an JsonElement übergeben werden kann.

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

Die oben genannten Markup erzeugt die folgende Benutzeroberfläche:

 [ ![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "Die Benutzeroberfläche erstellt, indem das markup")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png)

Jedes Element in der Struktur darf die Eigenschaft `"id"`. Es ist möglich, zur Laufzeit, um einzelne Abschnitte oder Elemente mit dem Indexer JsonElement zu verweisen. Und zwar so:

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


Das Stammelement kann innerhalb eines Abschnitts als ein Element zum Erstellen eines geschachtelten Controllers verwendet werden. In diesem Fall wird die zusätzliche Eigenschaft `"type"` muss festgelegt werden `"root"`

 <a name="url" />


### <a name="url"></a>url

Wenn die `"url"` Eigenschaft wird festgelegt, wenn der Benutzer auf diese RootElement tippt, wird der Code fordert eine Datei aus der angegebenen Url und stellen den Inhalt der neue Informationen angezeigt. Sie können Hiermit können Sie erstellen die Benutzeroberfläche der abhängig davon, was der Benutzer tippt erweitern.

 <a name="group" />


### <a name="group"></a>Gruppe

Wenn festgelegt, damit die Groupname für das Stammelement wird. Gruppennamen werden verwendet, um eine Zusammenfassung zu berücksichtigen, die als Wert für das Stammelement eines geschachtelten Elemente in das Element angezeigt wird. Dies ist der Wert, der ein Kontrollkästchen oder den Wert des ein Optionsfeld.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

Identifiziert das Optionsfeld-Element, das geschachtelte Elemente ausgewählt ist

 <a name="title" />


### <a name="title"></a>Titel

Wenn vorhanden ist, kann er den Titel für die RootElement verwendet werden.

 <a name="type" />


### <a name="type"></a>Typ

Muss festgelegt werden, um `"root"` Wenn dies in einem Abschnitt angezeigt wird (Dies wird zum Schachteln von Domänencontrollern verwendet).

 <a name="sections" />


### <a name="sections"></a>Abschnitte

Dies ist ein Json-Array mit den einzelnen Abschnitten

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Abschnittssyntax

Der Abschnitt enthält:

-  `header` (optional)
-  `footer` (optional)
-  `elements`-Array


 <a name="header" />


### <a name="header"></a>header

Falls vorhanden, wird der HeaderText als Beschriftung des Abschnitts angezeigt.

 <a name="footer" />


### <a name="footer"></a>Fußzeile

Falls vorhanden, wird die Fußzeile am unteren Rand der Abschnitt angezeigt.

 <a name="elements" />


### <a name="elements"></a>Elemente

Dies ist ein Array von Elementen. Jedes Element muss mindestens einen Schlüssel enthalten die `"type"` Schlüssel, der verwendet wird, um die Art der zu erstellenden Elements zu identifizieren.
Einige Elemente gemeinsam einige gemeinsamen Eigenschaften wie `"caption"` und `"value"`. Dies sind die Liste der unterstützten Elemente:

-  `string` Elemente (beide mit und ohne Formatierung)
-  `entry` Linien ("Regular" oder "Kennwort")
-  `boolean` Werte (mit Switches oder Bilder)


Zeichenfolgenelementen dient als Schaltflächen durch die Bereitstellung einer Methode zum Aufrufen, wenn der Benutzer auf die Zelle oder die Zugriffsmethode tippt,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Rendern von Elementen

Das Rendern von Elementen basieren auf der C#-StringElement und StyledStringElement, können sie Informationen auf verschiedene Weise gerendert, und es ist möglich, Berichte auf verschiedene Weise zu rendern. Die einfachste Elemente können wie folgt erstellt werden:

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

Es wird eine einfache Zeichenfolge mit allen Standardeinstellungen angezeigt: Schriftart, Hintergrund, Textfarbe und Ergänzungen. Es ist möglich, Einbinden von Aktionen an, die diese Elemente, und stellen sie verhalten sich wie Schaltflächen durch Festlegen der `"ontap"` Eigenschaft oder die `"onaccessorytap"` Eigenschaften:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Die oben genannten wird die Methode "ShowPhotos" in der Klasse "Acme.PhotoLibrary" aufgerufen. Die `"onaccessorytap"` ist ähnlich, aber es wird nur aufgerufen werden, wenn der Benutzer auf den Accessor statt Tippen auf die Zelle tippt. Um dies zu ermöglichen, müssen Sie auch die Zugriffsmethode festlegen:

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Rendern von Elementen kann zwei Zeichenfolgen gleichzeitig angezeigt, ist die Beschriftung und ein weiterer Vorteil ist der Wert. Diese Zeichenfolgen gerendert werden richten sich nach den Stil, können Sie festlegen, mit der `"style"` Eigenschaft. Die Standardeinstellung wird die Beschriftung auf der linken Seite und den Wert auf der rechten Seite angezeigt. Stil für Weitere Informationen finden Sie im Abschnitt. Die Codierung von Farben werden mit dem '#'-Symbol, gefolgt von hexadezimalen Zahlen, die die Werte für die Werte Rot, Grün, Blau und vielleicht alpha darstellen. Der Inhalt können in Kurzform (3 oder 4 hexadezimalen Ziffern) codiert werden RGB- oder RGBA-Werte dar. Oder die Langform (6 oder 8 Ziffern) die RGB- oder RGBA-Werte darstellen. Die Kurzversion ist eine Kurzschreibweise, um das gleiche hexadezimale Ziffer zweimal zu schreiben. Daher ist die Konstante "#1bc" oder als Rot = 0x11 Grün = 0xbb und Blau = 0xcc. Wenn der alpha-Wert nicht vorhanden ist, ist die Farbe nicht transparent. Einige Beispiele:

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>Zubehör

Bestimmt die Art der Zubehör werden angezeigt im Renderingelement, die möglichen Werte sind:

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Wenn der Wert nicht vorhanden ist, wird keine Zubehör angezeigt.

 <a name="background" />


### <a name="background"></a>Hintergrund

Die Hintergrundeigenschaft legt die Hintergrundfarbe für die Zelle fest. Dieser Wert ist entweder eine URL zu einem Bild (in diesem Fall das Async-Image-Downloadprogramm wird aufgerufen und der Hintergrund wird aktualisiert, sobald das Abbild heruntergeladen wird) oder eine Farbe, mit der Farbe Syntax definiert sein.

 <a name="caption" />


### <a name="caption"></a>Beschriftung

Die wichtigsten Zeichenfolge an das Renderingelement angezeigt werden sollen. Schriftart und Farbe angepasst werden durch Festlegen der `"textcolor"` und `"font"` Eigenschaften. Das Rendering-Format richtet sich nach der `"style"` Eigenschaft.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>Farbe und detailcolor

Die Farbe für den Haupttext oder ausführliche Fehlertext verwendet werden soll.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>DetailFont und Schriftart

Die Schriftart für die Beschriftung oder den Details Text verwendet werden soll. Das Format einer Schriftart-Spezifikation ist den Schriftartnamen, optional gefolgt von einem Bindestrich und den Schriftgrad.
Im folgenden sind gültige Schriftart-Spezifikationen:

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


Beide `character-wrap` und `word-wrap` verwendet werden können, zusammen mit der `"lines"` -Eigenschaft auf 0 (null), das Renderingelement in ein Element mehrzeilige aktivieren festgelegt.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ONTAP und onaccessorytap

Diese Eigenschaften müssen auf den Namen einer statischen Methode in Ihrer Anwendung verweisen, die ein Objekt als Parameter akzeptiert. Bei der Erstellung Ihrer Hierarchie mithilfe der Methoden JsonDialog.FromFile oder JsonDialog.FromJson können Sie einen optionalen Objektwert übergeben. Dieser Objektwert wird dann an die Methoden übergeben. Sie können diese verwenden, einige Kontext auf die statische Methode übergibt. Zum Beispiel:

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

Wenn dies auf NULL gesetzt wird, wird das Element automatisch-Größe sich je nach den Inhalt der Zeichenfolgen enthalten erleichtern. Damit dies funktioniert, müssen Sie auch Festlegen der `"linebreak"` Eigenschaft `"character-wrap"` oder `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>style

Das Format bestimmt die Art der Zellenstil, die zum Rendern des Inhalts verwendet werden und sie entsprechen den Werten der UITableViewCellStyle-Enumeration.
Mögliche Werte sind:

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : Text mit einen Untertitel hinzu.


 <a name="subtitle" />


### <a name="subtitle"></a>Untertitel

Der Wert für den Untertitel verwenden. Dies ist eine Verknüpfung festzulegende den Stil zu `"subtitle"` und legen Sie die `"value"` -Eigenschaft in eine Zeichenfolge.
Dies wird sowohl mit einem einzelnen Eintrag.

 <a name="textcolor" />


### <a name="textcolor"></a>TextColor

Die Farbe für den Text verwendet werden soll.

 <a name="value" />


### <a name="value"></a>Wert

Der sekundäre Wert an das Renderingelement angezeigt werden sollen. Das Layout dieses betroffen ist die `"style"` Einstellung. Schriftart und Farbe angepasst werden durch Festlegen der `"detailfont"` und `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Boolean-Elemente

Boolesche Elemente sollte auf den Typ festgelegt `"bool"`, darf eine `"caption"` angezeigt und die `"value"` auf "true" oder "false" festgelegt ist. Wenn die `"on"` und `"off"` Eigenschaften werden festgelegt, sie werden als Bilder sein. Die Images sind relativ zum aktuellen Arbeitsverzeichnis der Anwendung behoben. Wenn Sie Relative Bundle-Dateien verweisen möchten, können Sie mithilfe der `"~"` als Verknüpfung zur Bundle Anwendungsverzeichnis darstellen. Z. B. `"~/favorite.png"` werden die favorite.png, die in der Paketdatei enthalten ist. Zum Beispiel:

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

Type kann festgelegt werden, entweder `"boolean"` oder `"checkbox"`. Wenn festgelegt, in einen booleschen Wert einer UISlider oder Bildern verwendet werden (wenn beide `"on"` und `"off"` festgelegt sind). Wenn auf das Kontrollkästchen festgelegt ist, ein Kontrollkästchen verwendet werden. Die `"group"` Eigenschaft kann verwendet werden, um ein boolesches Element der Zugehörigkeit zu einer bestimmten Gruppe zu kennzeichnen. Dies ist hilfreich, wenn die enthaltende Stamm verfügt auch über eine `"group"` Eigenschaft als Stamm werden die Ergebnisse mit einer Anzahl aller die boolesche Werte (Kontrollkästchen), die zur selben Gruppe gehören zusammengefasst.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Eintragselemente

Verwenden Sie Eintragselemente, damit der Benutzer Daten eingeben kann. Der Typ für die Eintragselemente ist entweder `"entry"` oder `"password"`. Die `"caption"` Eigenschaft wird festgelegt, auf den Text auf der rechten Seite angezeigt und die `"value"` auf den ursprünglichen Wert festgelegt ist, auf den Eintrag festgelegt werden soll. Die `"placeholder"` wird verwendet, um einen Hinweis für dem Benutzer für leere Einträge anzeigen (es wird gezeigt abgeblendeter). Hier einige Beispiele:

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

Bestimmt den Stil automatische Korrektur für den Eintrag zu verwenden. Die möglichen Werte sind "true" oder "false" (oder die Zeichenfolgen `"yes"` und `"no"`).

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

Der Hinweistext, der angezeigt wird, wenn der Eintrag für einen leeren Wert verfügt.

 <a name="return-key" />


### <a name="return-key"></a>Return-Taste

Die Bezeichnung für die EINGABETASTE. Mögliche Werte sind:

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


## <a name="radio-elements"></a>Sender-Elemente

Radio Elemente aufweisen `"radio"`. Das Element, das ausgewählt ist, wird durch entnommen die `radioselected` Eigenschaft für das enthaltende Root-Element.
Darüber hinaus, wenn ein Wert, für festgelegt wird die `"group"` Eigenschaft dieses Optionsfeld zu dieser Gruppe gehört.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Datum und Uhrzeitelemente

Die Elementtypen `"datetime"`, `"date"` und `"time"` zum Rendern von Datumsangaben mit Uhrzeiten, Datumsangaben oder Uhrzeiten verwendet werden. Diese Elemente nehmen als Parameter an eine Beschriftung und einen Wert. Der Wert kann in einem beliebigen Format, die von der .NET DateTime.Parse-Funktion unterstützt geschrieben werden. Beispiel:

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


## <a name="htmlweb-element"></a>Html/Web Element

Sie können eine Zelle, die beim Erstellen getippt bettet eine UIWebView, das den Inhalt einer angegebenen URL rendert lokal oder remote mithilfe der `"html"` Typ. Die nur zwei Eigenschaften für dieses Element sind `"caption"` und `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
