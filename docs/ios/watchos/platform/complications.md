---
title: WatchOS Komplikationen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit WatchOS Komplikationen in Xamarin. Es wird erläutert, wie eine Komplikation, schreiben eine Komplikation, Vorlagen, hinzufügen und enthält Beispielcode.
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/03/2017
ms.openlocfilehash: 85b0c9b0688e9fb310a8f427018a02fe629404bb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61225542"
---
# <a name="watchos-complications-in-xamarin"></a>WatchOS Komplikationen in Xamarin

_WatchOS ermöglicht Entwicklern das Schreiben von benutzerdefinierter Komplikationen für watchfaces_

Auf dieser Seite wird erläutert, die verschiedenen Typen von Komplikationen, die verfügbar sind und wie Sie eine Komplikation WatchOS 3-app hinzufügen.

Beachten Sie, dass jede WatchOS-Anwendung eine Schwierigkeit besteht nur aufweisen kann.

Lesen Sie zunächst [Apple Dokumentation](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) zu bestimmen, ob Ihre app für eine Komplikation geeignet ist. Es gibt 5 `CLKComplicationFamily` Typen von Anzeigen zur Auswahl:

[![](complications-images/all-complications-sml.png "Die 5 CLKComplicationFamily-Typen, die verfügbar ist: Zirkulär Small Modular Small, modulare große, zweckmäßigen kleine, große zweckmäßigen")](complications-images/all-complications.png#lightbox)

Apps können nur eine Art oder alle fünf, je nach den angezeigten Daten implementieren.
Sie können auch Zeitreise, geben Sie Werte für vergangenen oder zukünftigen Zeiten an, wie der Benutzer die digitale Crown unterstützen.

<a name="adding" />

## <a name="adding-a-complication"></a>Eine Komplikation hinzufügen

### <a name="configuration"></a>Konfiguration

Schwierigkeiten können eine Watch-app hinzugefügt, während der Erstellung oder manuell zu einer vorhandenen Projektmappe hinzugefügt werden.

### <a name="add-new-project"></a>Neues Projekt hinzufügen...

Die **neues Projekt hinzufügen...**  -Assistent umfasst ein Kontrollkästchen, das automatisch eine Komplikation-Controller-Klasse erstellen und konfigurieren Sie die **"Info.plist"** Datei:

![](complications-images/file-new-project-sml.png "Das Kontrollkästchen Komplikation einschließen")

### <a name="existing-projects"></a>Vorhandene Projekte

So fügen Sie eine Komplikation zu einem vorhandenen Projekt hinzu:

1. Erstellen Sie ein neues **ComplicationController.cs** Klassendatei, und implementieren `CLKComplicationDataSource`.
2. Konfigurieren der app **"Info.plist"** verfügbar machen, der Komplikation und Identität der Komplikation sortierungsfamilien unterstützt.

Diese Schritte werden im folgenden ausführlicher beschrieben.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource-Klasse

Die folgenden C# Vorlage enthält, die mindestens erforderlichen Methoden zum Implementieren einer `CLKComplicationDataSource`.

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

Führen Sie die [schreiben eine Komplikation](#writing) Anweisungen zum Hinzufügen von Code zu dieser Klasse.

### <a name="infoplist"></a>Info.plist

Der Watch-Erweiterungs **"Info.plist"** Datei geben Sie den Namen des der `CLKComplicationDataSource` und welche Komplikation-Familien, die Sie unterstützen möchten:

[![](complications-images/complications-config-sml.png "Die Typen der Komplikation-Familien")](complications-images/complications-config.png#lightbox)

Die **Datenquellenklasse** Eingabeliste zeigt Klassennamen diese Unterklasse `CLKComplicationDataSource` Unterklasse, die Ihre Komplikation Logik enthält.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Alle Komplikation-Funktionalität ist in eine einzelne Klasse, überschreiben Methoden implementiert die `CLKComplicationDataSource` abstrakte Klasse (implementiert die `ICLKComplicationDataSource` Schnittstelle).

### <a name="required-methods"></a>/ Zwingend erforderliche Methoden

Sie müssen die folgenden Methoden für die Komplikation auszuführende implementieren:

- `GetPlaceholderTemplate` -Zurückgegeben Sie die statische Anzeige verwendet wird, während der Konfiguration, oder wenn die app einen Wert angeben, kann nicht.
- `GetCurrentTimelineEntry` -Berechnen Sie das richtige anzeigen aus, wenn der Komplikation ausgeführt wird.
- `GetSupportedTimeTravelDirections` -Gibt die Optionen aus `CLKComplicationTimeTravelDirections` wie z. B. `None`, `Forward`, `Backward`, oder `Forward | Backward`.

### <a name="privacy"></a>Datenschutz

Komplikationen, die persönlichen Daten anzeigen

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` oder `HideOnLockScreen`

Wenn diese Methode zurückgibt `HideOnLockScreen` entweder ein Symbol oder den Namen der Anwendung (und keine Daten) zeigt die Schwierigkeit bei die Apple Watch gesperrt ist.

### <a name="updates"></a>Updates

- `GetNextRequestedUpdateDate` -Zurückgeben einer Uhrzeit an, wenn das Betriebssystem als Nächstes die app für aktualisierte Komplikation Anzeigen von Daten abgefragt werden sollen

Sie können auch ein Update von Ihrer iOS-app erzwingen.

### <a name="supporting-time-travel"></a>Unterstützung von Zeitreise

Zeit Travel-Unterstützung ist optional und kontrollierten durch die `GetSupportedTimeTravelDirections` Methode. Wenn sie zurückgibt `Forward`, `Backward`, oder `Forward | Backward` und dann müssen Sie die folgenden Methoden implementieren

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Schreiben Sie eine Komplikation

Komplikationen-reichen von einfachen Daten anzeigen zu kompliziert-Image und Rendern von Daten mit Zeitreise-Unterstützung. Der folgende Code zeigt, wie Sie eine einfache, einzelne-Template-Komplikation zu erstellen.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Beispielcode

In diesem Beispiel unterstützt nur die `UtilitarianLarge` Vorlage, damit nur auf bestimmten watchfaces ausgewählt werden können, die diese Art von Komplikation zu unterstützen. Wenn *auswählen* Komplikationen zeigt auf eine Überwachung, **Meine KOMPLIKATION** und wann *ausführen* es zeigt den Text **MINUTE _Stunde_**   (mit einem Teil der Zeit).

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

## <a name="complication-templates"></a>Komplikation-Vorlagen

Eine Reihe von verschiedenen Vorlagen stehen zur Verfügung, für die einzelnen Stile Komplikation.
Die **Ring** -Vorlagen können Sie einen Fortschritt-Style-Ring um die Schwierigkeit, anzuzeigen, mit dem Fortschritt oder einen anderen Wert grafisch angezeigt werden kann.

[Apple CLKComplicationTemplate-Dokumentation](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Zirkulär Small

Diese Vorlage Klassennamen werden alle mit dem Präfix `CLKComplicationTemplateCircularSmall`:

- **RingImage** – ein einzelnes Bild, mit der ein statuskreis, um es herum anzuzeigen.
- **RingText** -anzeigen eine einzelne Textzeile an, mit der ein statuskreis, um es herum.
- **SimpleImage** – nur ein kleines Bild enthält einzelnes angezeigt.
- **SimpleText** – nur ein kleines Snippet von Text angezeigt.
- **StackImage** – ein Abbild und eine Textzeile, übereinander anzeigen
- **StackText** -zwei Textzeilen anzeigen.

### <a name="modular-small"></a>Modular Small

Diese Vorlage Klassennamen werden alle mit dem Präfix `CLKComplicationTemplateModularSmall`:

- **ColumnsText** – Anzeigen von einem kleinen Raster von Textwerten (2 Zeilen und 2 Spalten).
- **RingImage** – ein einzelnes Bild, mit der ein statuskreis, um es herum anzuzeigen.
- **RingText** -anzeigen eine einzelne Textzeile an, mit der ein statuskreis, um es herum.
- **SimpleImage** – nur ein kleines Bild enthält einzelnes angezeigt.
- **SimpleText** – nur ein kleines Snippet von Text angezeigt.
- **StackImage** – ein Abbild und eine Textzeile, übereinander anzeigen
- **StackText** -zwei Textzeilen anzeigen.

### <a name="modular-large"></a>Modular Large

Diese Vorlage Klassennamen werden alle mit dem Präfix `CLKComplicationTemplateModularLarge`:

- **Spalten** – zeigt ein Raster mit 3 Zeilen mit 2 Spalten, optional einschließlich ein Bild auf der linken Seite der einzelnen Zeilen.
- **StandardBody** -eine Kopfzeile in fettformatierung-Zeichenfolge, mit zwei Zeilen mit nur-Text angezeigt. Der Header kann optional ein Bild auf der linken Seite anzeigen.
- **Tabelle** -eine Kopfzeile in fettformatierung-Zeichenfolge, mit einem Raster aus 2 x 2 der Text unter ihm angezeigt. Der Header kann optional ein Bild auf der linken Seite anzeigen.
- **TallBody** -eine Kopfzeile in fettformatierung-Zeichenfolge, mit einer größeren Schrift Textzeile unter angezeigt.

### <a name="utilitarian-small"></a>Zweckmäßigen klein

Diese Vorlage Klassennamen werden alle mit dem Präfix `CLKComplicationTemplateUtilitarianSmall`:

- **Flache** – zeigt ein Bild und Text in einer einzelnen Zeile (der Text sollte kurz sein).
- **RingImage** – ein einzelnes Bild, mit der ein statuskreis, um es herum anzuzeigen.
- **RingText** -anzeigen eine einzelne Textzeile an, mit der ein statuskreis, um es herum.
- **Quadrat** – ein quadratisches Bild (40px oder 44px Square, für die 38 mm "oder" 42 mm Apple Watch) angezeigt.

### <a name="utilitarian-large"></a>Große zweckmäßigen

Es gibt nur eine Vorlage für diese Art der Komplikation: `CLKComplicationTemplateUtilitarianLargeFlat`.
Ein einzelnes Bild und Text, alle in einer einzelnen Zeile angezeigt.



## <a name="related-links"></a>Verwandte Links

- [Apple Dokumentation](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC video](https://developer.apple.com/videos/play/wwdc2015-209/)
