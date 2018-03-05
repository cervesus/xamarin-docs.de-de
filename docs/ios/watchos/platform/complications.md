---
title: Bereichsregeln
description: "WatchOS kann Entwickler benutzerdefinierte Komplikationen für Überwachung gesichtsabbildungen schreiben"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: a13de7fbb4b6e1f9fa2853ce599f3a038a5e4040
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="complications"></a>Bereichsregeln

_WatchOS kann Entwickler benutzerdefinierte Komplikationen für Überwachung gesichtsabbildungen schreiben_

Auf dieser Seite wird erläutert, die verschiedenen Typen von Komplikationen verfügbar, und wie Ihre app WatchOS 3 eine Komplikation hinzugefügt wird.

Beachten Sie, dass jede Anwendung WatchOS nur eine Komplikation haben kann.

Lesen Sie zuerst [Apple Docs](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) zu bestimmen, ob Ihre app für eine Komplikation geeignet ist. Es gibt 5 `CLKComplicationFamily` Typen der Anzeige zur Auswahl:

[ ![](complications-images/all-complications-sml.png "Die 5 CLKComplicationFamily-Typen: zirkuläre kleine, modulare kleine, modulare große, kleine aus, aus großen")](complications-images/all-complications.png)

Apps können nur einen Stil oder alle fünf, abhängig von den angezeigten Daten implementieren.
Sie können auch unterstützen Zeitreise, geben Sie Werte für die vergangenen und/oder zukünftige Zeiten, wie der Benutzer die digitale Crown aktiviert.

<a name="adding" />

## <a name="adding-a-complication"></a>Eine Schwierigkeit hinzufügen

### <a name="configuration"></a>Konfiguration

Komplikationen können während der Erstellung einer Watch-app hinzugefügt oder manuell zu einer vorhandenen Projektmappe hinzugefügt werden.

### <a name="add-new-project"></a>Neues Projekt hinzufügen...

Die **neues Projekt hinzufügen...**  -Assistent umfasst ein Kontrollkästchen, das automatisch eine Controllerklasse Komplikation erstellen und konfigurieren Sie die **"Info.plist"** Datei:

![](complications-images/file-new-project-sml.png "Das Kontrollkästchen Komplikation einschließen")

### <a name="existing-projects"></a>Vorhandene Projekte

So fügen Sie eine Komplikation ein vorhandenes Projekt hinzu

1. Erstellen Sie ein neues **ComplicationController.cs** Klassendatei und implementieren `CLKComplicationDataSource`.
2. Konfigurieren der app **"Info.plist"** verfügbar machen, der Komplikation und die Identität der Komplikation Familien werden unterstützt.

Diese Schritte werden im folgenden ausführlicher beschrieben.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource-Klasse

Die folgende C#-Vorlage enthält, die mindestens erforderlichen Methoden implementiert eine `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

Führen Sie die [Schreiben einer Komplikation](#writing) Anweisungen zum Hinzufügen von Code zu dieser Klasse.

### <a name="infoplist"></a>Info.plist

Der Watch-Erweiterungs **"Info.plist"** Datei geben Sie den Namen des der `CLKComplicationDataSource` und welche Komplikation Familien, die Sie unterstützen möchten:

[ ![](complications-images/complications-config-sml.png "Die Schwierigkeit-Familie-Typen")](complications-images/complications-config.png)

Die **Datenquellenklasse** anzeigen Eingabeliste Klassennamen auf diese Unterklasse `CLKComplicationDataSource` Unterklasse, die die Schwierigkeit Logik enthält.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Alle Komplikation-Funktionalität ist in eine einzelne Klasse, Methoden überschreiben implementiert die `CLKComplicationDataSource` abstrakten Klasse (implementiert die `ICLKComplicationDataSource` Schnittstelle).

### <a name="required-methods"></a>Erforderlichen Methoden

Sie müssen die folgenden Methoden für die Schwierigkeit auszuführende implementieren:

- `GetPlaceholderTemplate` -Die statische Anzeige verwendet, während der Konfiguration oder die app nicht bereitstellen kann, einen Wert zurück.
- `GetCurrentTimelineEntry` -Berechnen Sie das richtige anzeigen, wenn der Komplikation ausgeführt wird.
- `GetSupportedTimeTravelDirections` -Gibt die Optionen aus `CLKComplicationTimeTravelDirections` wie z. B. `None`, `Forward`, `Backward`, oder `Forward | Backward`.

### <a name="privacy"></a>Datenschutz

Komplikationen, die persönliche Daten anzeigen

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` Oder `HideOnLockScreen`

Wenn diese Methode zurückgibt `HideOnLockScreen` entweder ein Symbol oder den Namen der Anwendung (und keine Daten) zeigt die Schwierigkeit bei der Apple Watch gesperrt ist.

### <a name="updates"></a>Updates

- `GetNextRequestedUpdateDate` -Return aktualisiert einen Zeitraum aus, wenn die app für das Betriebssystem neben Abfrage sollte, Komplikation Anzeigen von Daten.

Sie können auch ein Update von Ihrer iOS-app erzwingen.

### <a name="supporting-time-travel"></a>Unterstützende Zeitreise

Ausführungszeit Reisen Support ist optional, und durch gesteuert die `GetSupportedTimeTravelDirections` Methode. Wenn zurückgegeben `Forward`, `Backward`, oder `Forward | Backward` dann implementieren Sie die folgenden Methoden müssen

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Eine Schwierigkeit schreiben

Komplikationen reichen von einfachen Daten anzeigen zu kompliziert-Image und Rendern von Daten mit Zeitreise-Unterstützung. Der folgende Code zeigt, wie eine einfache, einzelne Vorlage Komplikation erstellen.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Beispielcode

In diesem Beispiel unterstützt nur die `UtilitarianLarge` Vorlage, damit nur auf bestimmten Überwachungsfenster Flächen ausgewählt werden können, die diese Art von Problemen zu unterstützen. Wenn *auswählen* Komplikationen auf einer Überwachung angezeigt **Meine KOMPLIKATION** und wann *ausführen* es zeigt den Text **MINUTE _Stunde_**   (zu einem Teil der Zeit).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Komplikation Vorlagen

Eine Reihe von verschiedenen Vorlagen stehen zur Verfügung, für jede Komplikation-Format.
Die **Ring** Richtlinienvorlagen können Sie einen Status-Stil-Ring, um die Schwierigkeit angezeigt werden, die mit der Bearbeitung oder ein anderer Wert grafisch angezeigt werden kann.

[Apple CLKComplicationTemplate docs](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Zirkuläre klein

Diese Vorlage Klassennamen werden mit Präfix `CLKComplicationTemplateCircularSmall`:

- **RingImage** -ein einzelnes Image, mit der ein Status Ring um es anzuzeigen.
- **RingText** -eine einzelne Textzeile mit Status Ring herum anzuzeigen.
- **SimpleImage** -nur ein kleines Bild für die einzelnes anzeigen.
- **SimpleText** -nur einen kleinen Textausschnitt anzeigen.
- **StackImage** -anzuzeigen, ein Bild sowie eine Zeile des Texts, übereinander
- **StackText** -zwei Textzeilen anzeigen.

### <a name="modular-small"></a>Modulare klein

Diese Vorlage Klassennamen werden mit Präfix `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -ein kleines Raster mit Textwerten (2 Zeilen und 2 Spalten).
- **RingImage** -ein einzelnes Image, mit der ein Status Ring um es anzuzeigen.
- **RingText** -eine einzelne Textzeile mit Status Ring herum anzuzeigen.
- **SimpleImage** -nur ein kleines Bild für die einzelnes anzeigen.
- **SimpleText** -nur einen kleinen Textausschnitt anzeigen.
- **StackImage** -anzuzeigen, ein Bild sowie eine Zeile des Texts, übereinander
- **StackText** -zwei Textzeilen anzeigen.

### <a name="modular-large"></a>Große testumgebungsanleitungen

Diese Vorlage Klassennamen werden mit Präfix `CLKComplicationTemplateModularLarge`:

- **Spalten** -ein Raster mit 3 Zeilen mit 2 Spalten, optional einschließlich ein Bild auf der linken Seite jeder Zeile anzuzeigen.
- **StandardBody** -eine fett Headerzeichenfolge mit zwei Zeilen mit nur-Text angezeigt. Der Header kann optional ein Bild auf der linken Seite anzuzeigen.
- **Tabelle** -eine fett Headerzeichenfolge mit einem 2 x 2-Raster des Texts, darunter angezeigt. Der Header kann optional ein Bild auf der linken Seite anzuzeigen.
- **TallBody** -eine fett Headerzeichenfolge mit einer größeren Schrift Textzeile unter angezeigt.

### <a name="utilitarian-small"></a>Aus kleinen

Diese Vorlage Klassennamen werden mit Präfix `CLKComplicationTemplateUtilitarianSmall`:

- **Flache** -zeigt ein Bild und Text in einer einzelnen Zeile (der Text sollte kurz sein).
- **RingImage** -ein einzelnes Image, mit der ein Status Ring um es anzuzeigen.
- **RingText** -eine einzelne Textzeile mit Status Ring herum anzuzeigen.
- **Quadrat** -ein Quadrat Bild (40px oder 44px quadratische für die 38 mm oder 42 mm Apple Watch) angezeigt.

### <a name="utilitarian-large"></a>Große aus

Es ist nur eine Vorlage für diese Art der Komplikation: `CLKComplicationTemplateUtilitarianLargeFlat`.
Ein einzelnes Bild und Text, alle in einer einzelnen Zeile angezeigt.



## <a name="related-links"></a>Verwandte Links

- [Apple Dokumentation](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC video](https://developer.apple.com/videos/play/wwdc2015-209/)
