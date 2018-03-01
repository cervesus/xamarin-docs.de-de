---
title: Arbeiten mit Fortschrittsanzeigen
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Statusanzeigen innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b5d3a03324e73b06bd3defe7e6610163c3d1b26d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-progress-indicators"></a>Arbeiten mit Fortschrittsanzeigen

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Statusanzeigen innerhalb einer Xamarin.tvOS-app._


Es gibt möglicherweise Zeiten, wenn Ihre app Xamarin.tvOS neuen Inhalte zu laden oder einen Vorgang langwierige Verarbeitung ausführen muss. Während dieser Zeiten sollten Sie ein Indikator für die Aktivität oder Statusanzeige angezeigt, damit die Benutzer wissen, dass die app immer noch ausgeführt wird, und geben einen Hinweis auf hinsichtlich der Länge der auszuführenden Aufgabe darstellen.

[ ![](progress-indicators-images/intro01.png "Beispiel-Statusanzeigen")](progress-indicators-images/intro01.png)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>Informationen zu Indikatoren der Aktivität

Ein Indikator für die Aktivität wird als ein sich drehendes Zahnrad visuell dargestellt und wird verwendet, um eine Aufgabe mit einer unbestimmten Länge darstellen. Der Indikator wird angezeigt, wenn der Task beginnt und verschwindet, wenn die Aufgabe abgeschlossen ist.

Apple hat die folgenden Vorschläge für den Umgang mit Indikatoren Aktivität:

- **Verwenden Sie nach Möglichkeit stattdessen Balken Fortschritt** – da ein Indikator für die Aktivität dem Benutzer keine Rückmeldung erhält über die Dauer der ausgeführte Prozess dauert, immer eine Statusanzeige verwenden, wenn die Länge (z. B., wie viele Bytes zum Herunterladen einer Datei) bekannt ist.
- **Behalten Sie den Indikator animierte** -Benutzer eine stationär Indikator für die Aktivität an eine angehaltene app beziehen, damit Sie immer haben, sollte den Indikator animiert, während er angezeigt wird, wird.
- **Beschreiben Sie den Task verarbeiteten** -lediglich zum Anzeigen der Aktivität Indikator allein reicht nicht aus, die der Benutzer muss über den Prozess informiert werden, warten auf. Schließen Sie eine aussagekräftige Bezeichnung (in der Regel einen einzelnen, vollständigen Satz), die die Aufgabe klar definiert.

<a name="Summary" />

## <a name="about-progress-bars"></a>Informationen zu Statusanzeigen

Eine Statusanzeige wird als eine Zeile, die mit der Farbe an, die die Status und Länge der sehr zeitaufwändig anzugeben, füllt dargestellt. Statusanzeigen sollte immer verwendet werden, wenn die Länge der Aufgaben bekannt ist, oder berechnet werden kann.

Apple hat die folgenden Vorschläge für die Arbeit mit Statusanzeigen:

- **Genau Fortschritt** -Statusanzeigen sollte immer eine genaue Darstellung der Zeitaufwand zum Abschließen einer Aufgabe sein. Nie sind so dargestellt, die Zeit, um die app ausgelastet angezeigt werden.
- **Verwendung für die Dauer der Well-Defined** -Statusanzeige sollte nur nicht angezeigt, dass eine langwierige Aufgabe dauert platzieren, aber der Benutzer und Überblick, wie viel des Tasks abgeschlossen ist und eine Schätzung der verbleibenden Zeit bereitstellen.

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>Statusanzeigen und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Statusanzeige in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Aktivität Indikator** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [ ![](progress-indicators-images/activity01.png "Ein Indikator für die Aktivität")](progress-indicators-images/activity01.png)
1. In der **Registerkarte "Widget"** von der **Eigenschaften Pad**, können Sie verschiedene Eigenschaften des Indikators Aktivität wie z. B. anpassen seine **Stil** und **Verhalten**: 

    [ ![](progress-indicators-images/activity02.png "Die Registerkarte "Widget" ")](progress-indicators-images/activity02.png)
1. Ziehen Sie eine **Bearbeitung** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [ ![](progress-indicators-images/activity03.png "Eine Status-Ansicht")](progress-indicators-images/activity03.png)
1. In der **Registerkarte "Widget"** von der **Property Explorer**, können Sie verschiedene Eigenschaften der Sicht ausgeführt wie z. B. Anpassen der **Stil** und **Fortschritt**(% abgeschlossen): 

    [ ![](progress-indicators-images/activity04.png "Die Registerkarte "Widget"")](progress-indicators-images/activity04.png)
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie in C#-Code auf sie reagieren können. Zum Beispiel: 

    [ ![](progress-indicators-images/activity05.png "Weisen Sie einen Namen")](progress-indicators-images/activity05.png)
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Aktivität Indikator** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [ ![](progress-indicators-images/activity01-vs.png "Ein Indikator für die Aktivität")](progress-indicators-images/activity01-vs.png)
1. In der **Registerkarte "Widget"** von der **Eigenschaften-Explorer**, können Sie verschiedene Eigenschaften des Indikators Aktivität wie z. B. Anpassen der **Stil** und **Verhalten**: 

    [ ![](progress-indicators-images/activity02-vs.png "Die Registerkarte "Widget"")](progress-indicators-images/activity02-vs.png)
1. Ziehen Sie eine **Bearbeitung** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [ ![](progress-indicators-images/activity03-vs.png "Eine Status-Ansicht")](progress-indicators-images/activity03-vs.png)
1. In der **Registerkarte "Widget"** von der **Property Explorer**, können Sie verschiedene Eigenschaften der Sicht ausgeführt wie z. B. Anpassen der **Stil** und **Fortschritt**(% abgeschlossen): 

    [ ![](progress-indicators-images/activity04-vs.png "Die Registerkarte "Widget"")](progress-indicators-images/activity04-vs.png)
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie in C#-Code auf sie reagieren können. Zum Beispiel: 

    [ ![](progress-indicators-images/activity05-vs.png "Weisen Sie einen Namen")](progress-indicators-images/activity05-vs.png)
1. Speichern Sie die Änderungen.

-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>Arbeiten mit Indikatoren der Aktivität

Wie bereits erwähnt, sollte Aktivität Indikatoren angezeigt werden, wenn Ihre app einen langen Prozess ausgeführt wird, aber Sie wissen nicht, die genaue Länge der Zeit, die die Aufgabe benötigen.

Zu jedem Zeitpunkt, Sie können sehen, wenn der Indikator für die Aktivität die Spinvorgänge Animation durch die Überprüfung ausgeführt wird, die `IsAnimating` Eigenschaft. Wenn die `HidesWhenStopped` Eigenschaft `true`, den Indikator für die Aktivität automatisch ausgeblendet wird, wenn die Animation beendet wird.

Sie können den folgenden Code zum Starten der Animation verwenden: 

```csharp
ActivityIndicator.StartAnimating();
```

Und im folgenden wird die Animation beendet:

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>Arbeiten mit Statusanzeigen

Erneut, eine Statusanzeige jederzeit vorgesehen Ihre app eine lang ausgeführte Aufgabe einer bekannten Dauer ausgeführt wird. 

Die `Progress` Eigenschaft wird verwendet, um die Menge der Aufgabe festgelegt, die zwischen 0 % und 100 % (zwischen 0,0 und 1,0) abgeschlossen wurde. Verwenden der `ProgressTintColor` Eigenschaft zum Festlegen der Farbe des Balkens Betrag abgeschlossen und die `TrackTintColor` Eigenschaft, um die Farbe des Hintergrunds (unvollständigen Zeitraum) festzulegen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Statusanzeigen innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [Beispiele für tvos. außerdem wurden](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
