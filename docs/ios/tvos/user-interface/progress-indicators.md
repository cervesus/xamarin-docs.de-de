---
title: Arbeiten mit tvos. außerdem wurden Statusanzeigen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Statusanzeigen in einer app für tvos. außerdem wurden mit Xamarin erstellten. Es wird erläutert, Statusanzeigen und Aktivität Indikatoren.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/25/2018
ms.openlocfilehash: f8812f6b3f8a461487dcaf548637c84b16631d6b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789215"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>Arbeiten mit tvos. außerdem wurden Statusanzeigen in Xamarin

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Statusanzeigen innerhalb einer Xamarin.tvOS-app._

Es gibt möglicherweise Zeiten, wenn Ihre app Xamarin.tvOS neuen Inhalte zu laden oder einen Vorgang langwierige Verarbeitung ausführen muss. Während dieser Zeiten sollten Sie einen Indikator für die Aktivität oder eine Statusanzeige angezeigt, damit die Benutzer wissen, dass die app immer noch ausgeführt wird, und geben einen Hinweis auf hinsichtlich der Länge der auszuführenden Aufgabe darstellen.

![Beispiel-Statusanzeigen](progress-indicators-images/intro01.png "Statusanzeigen-Beispiel")

## <a name="about-activity-indicators"></a>Informationen zu Indikatoren der Aktivität

Ein Indikator für die Aktivität wird als ein sich drehendes Zahnrad dargestellt und wird verwendet, um eine Aufgabe mit einer unbestimmten Länge darstellen. Der Indikator wird angezeigt, wenn der Task beginnt und verschwindet, wenn die Aufgabe abgeschlossen ist.

Apple hat die folgenden Vorschläge für den Umgang mit Indikatoren Aktivität:

- **Verwenden Sie Statusanzeigen nach Möglichkeit stattdessen** : Da eine Aktivität Indikator ermöglicht der Benutzer kein Feedback, wie lange den ausgeführte Prozess dauert, wird immer eine Statusanzeige verwenden, wenn die Länge (z. B., wie viele Bytes zum Herunterladen einer Datei) bekannt ist.
- **Behalten Sie den Indikator animiert** -Benutzer einen Indikator stationär Aktivität an eine angehaltene app beziehen, damit Sie immer den Indikator animiert sollten, während er angezeigt wird, wird.
- **Beschreiben Sie den Task verarbeiteten** -lediglich zum Anzeigen des Aktivität Indikators allein reicht nicht aus; der Benutzer über den Prozess informiert werden, auf dem sie darauf warten, muss. Schließen Sie eine aussagekräftige Bezeichnung (in der Regel einen einzelnen, vollständigen Satz), die die Aufgabe klar definiert.

## <a name="about-progress-bars"></a>Informationen zu Statusanzeigen

Eine Statusanzeige wird als eine Zeile, die mit der Farbe an, die die Status und Länge der sehr zeitaufwändig anzugeben, füllt dargestellt. Statusanzeigen sollte immer verwendet werden, wenn die Länge der Aufgaben bekannt ist, oder berechnet werden kann.

Apple hat die folgenden Vorschläge für die Arbeit mit Statusanzeigen:

- **Genau melden kann, Fortschritt** -Statusanzeigen sollte immer eine genaue Darstellung der Zeitaufwand zum Abschließen einer Aufgabe vorhanden. Nie sind so dargestellt, die Zeit, um die app ausgelastet angezeigt werden.
- **Verwenden Sie für eine klar definierte Dauer** -Fortschritt Balken sollte nur nicht angezeigt, dass eine langwierige Aufgabe dauert platzieren, aber weisen Sie dem Benutzer und Überblick, wie viel des Tasks abgeschlossen ist und eine Schätzung der verbleibenden Zeit.

## <a name="progress-indicators-and-storyboards"></a>Statusanzeigen und storyboards

Die einfachste Möglichkeit, eine Statusanzeige eingeblendet, in einer app Xamarin.tvOS arbeiten, ist es zur Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
1. In der **Lösung Pad**, doppelklicken Sie auf die **Main.storyboard** Datei und öffnet ihn zur Bearbeitung.

2. Ziehen Sie ein **Aktivität Indikator** aus der **Toolbox** und legen Sie sie in der Sicht: 

    ![Ein Indikator für die Aktivität](progress-indicators-images/activity01.png "ein Indikator für die Aktivität")

3. In der **Widget** auf der Registerkarte die **Eigenschaften Pad**, können Sie verschiedene Eigenschaften des Indikators Aktivität wie z. B. Anpassen der **Stil**, **Verhalten**, und **Namen**: 

    ![Die Registerkarte "Widget" für einen Indikator für die Aktivität](progress-indicators-images/activity02.png "das Widget-Registerkarte für einen Indikator für die Aktivität")
    
    Die **Namen** bestimmt den Namen der Eigenschaft, die den Indikator Aktivität in C#-Code darstellt.

4. Ziehen Sie eine **Bearbeitung** aus der **Toolbox** und legen Sie sie in der Sicht: 

    ![Eine Sicht Fortschritt](progress-indicators-images/activity03.png "eine Status-Ansicht")

5. In der **Widget** auf der Registerkarte die **Property Explorer**, können Sie verschiedene Eigenschaften der Sicht ausgeführt wie z. B. Anpassen der **Stil**, **Status**(% abgeschlossen), und **Namen**: 

    ![Die Registerkarte "Widget" für eine Sicht ausgeführt](progress-indicators-images/activity04.png "das Widget-Registerkarte für eine Status-Ansicht")
    
    Die **Namen** bestimmt den Namen der Eigenschaft, die die Status-Ansicht in C#-Code darstellt.

6. Speichern Sie die Änderungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die **Main.storyboard** Datei und öffnet ihn zur Bearbeitung.

2. Ziehen Sie ein **Aktivität Indikator** aus der **Toolbox** und legen Sie sie in der Sicht: 

    ![Ein Indikator für die Aktivität](progress-indicators-images/activity01-vs.png
    "ein Indikator für die Aktivität")

3. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, können Sie verschiedene Eigenschaften des Indikators Aktivität wie z. B. Anpassen der **Stil**, **Verhalten**, und **Namen**: 

    ![Die Registerkarte "Widget" für einen Indikator für die Aktivität](progress-indicators-images/activity02-vs.png "das Widget-Registerkarte für einen Indikator für die Aktivität")

    Die **Namen** bestimmt den Namen der Eigenschaft, die den Indikator Aktivität in C#-Code darstellt.

4. Ziehen Sie eine **Bearbeitung** aus der **Toolbox** und legen Sie sie in der Sicht: 

   ![Eine Sicht Fortschritt](progress-indicators-images/activity03-vs.png "eine Status-Ansicht")

5. In der **Widget** auf der Registerkarte die **Property Explorer**, können Sie verschiedene Eigenschaften der Sicht ausgeführt wie z. B. Anpassen der **Stil**, **Status**(% abgeschlossen), und **Namen**: 

    ![Die Registerkarte "Widget" für eine Sicht ausgeführt](progress-indicators-images/activity04-vs.png "das Widget-Registerkarte für eine Status-Ansicht")
    
    Die **Namen** bestimmt den Namen der Eigenschaft, die die Status-Ansicht in C#-Code darstellt.

6. Speichern Sie die Änderungen.

-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Arbeiten mit Indikatoren der Aktivität

Wie bereits erwähnt, sollte Aktivität Indikatoren angezeigt werden, wenn Ihre app einen langen Prozess unbestimmt Länge ausgeführt wird.

Zu einem beliebigen Zeitpunkt können Sie sehen, wenn ein Indikator für die Aktivität durch Überprüfen animieren wird seine `IsAnimating` Eigenschaft. Wenn die `HidesWhenStopped` Eigenschaft `true`, der Indikator für die Aktivität automatisch ausgeblendet wird, wenn die Animation beendet wird.

Sie können den folgenden Code zum Starten der Animation verwenden: 

```csharp
ActivityIndicator.StartAnimating();
```

Und im folgenden wird die Animation beendet:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Diese Codeausschnitte davon ausgehen, dass die Aktivität des Indikators **Namen** wurde festgelegt, um **ActivityIndicator** in der **Widget** iOS-Designer auf der Registerkarte.

## <a name="working-with-progress-bars"></a>Arbeiten mit Statusanzeigen

Erneut, eine Statusanzeige jederzeit vorgesehen Ihre app eine lang ausgeführte Aufgabe einer bekannten Dauer ausgeführt wird. 

Die `Progress` Eigenschaft wird verwendet, um die Menge der Aufgabe festgelegt, die zwischen 0 % und 100 % (zwischen 0,0 und 1,0) abgeschlossen wurde. Verwenden der `ProgressTintColor` Eigenschaft zum Festlegen der Farbe des Balkens Betrag abgeschlossen und die `TrackTintColor` Eigenschaft, um die Farbe des Hintergrunds (unvollständigen Zeitraum) festzulegen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Statusanzeigen innerhalb einer Xamarin.tvOS-app.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
