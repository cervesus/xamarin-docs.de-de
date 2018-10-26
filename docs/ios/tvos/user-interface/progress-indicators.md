---
title: Arbeiten mit TvOS-Statusanzeige in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Statusanzeige in einer TvOS-app mit Xamarin erstellt wurde. Es wird erläutert, Statusleisten und aktivitätsindikatoren.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/25/2018
ms.openlocfilehash: cbd2b2de237a5bb22d1dc0242569b96b12bca070
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106675"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>Arbeiten mit TvOS-Statusanzeige in Xamarin

_Dieser Artikel behandelt das Entwerfen von und Arbeiten mit der Statusanzeige in ein Xamarin.tvOS-app._

Es gibt möglicherweise Zeiten, wenn Ihre app Xamarin.tvOS neuen Inhalte zu laden, oder führen Sie einen Vorgang für die langwierige Verarbeitung muss. Während dieser Zeiten sollte Sie entweder ein Indikator für die Aktivität oder eine Statusanzeige angezeigt, damit der Benutzer wissen, dass die app immer noch ausgeführt wird, und geben ein Hinweis auf hinsichtlich der Länge der ausgeführten Aufgabe darstellen.

![Beispiel Statusanzeigen](progress-indicators-images/intro01.png "Beispiel Statusanzeigen")

## <a name="about-activity-indicators"></a>Informationen zu aktivitätsindikatoren

Ein Indikator für die Aktivität wird als eine sich drehende Zahnrad dargestellt und wird verwendet, um eine Aufgabe mit einer unbestimmten Länge darstellen. Der Indikator wird angezeigt, wenn die Aufgabe gestartet wird und verschwindet, wenn die Aufgabe abgeschlossen ist.

Apple hat die folgenden Vorschläge für die Arbeit mit aktivitätsindikatoren:

- **Verwenden Sie Statusanzeigen nach Möglichkeit stattdessen** : Da eine Aktivität Indikator bietet, die der Benutzer kein Feedback, wie lange den ausgeführte Prozess dauert, wird immer eine Statusanzeige verwenden, wenn die Länge (z. B., wie viele Bytes in eine Datei herunterladen) bekannt ist.
- **Behalten Sie den Indikator animiert** -Benutzer beziehen sich einen Indikator stationär Aktivität auf eine angehaltene app, damit Sie immer den Indikator animieren sollten, während er angezeigt wird, wird.
- **Beschreiben Sie den Task verarbeiteten** -lediglich zum Anzeigen des Aktivität Indikators allein reicht nicht aus; der Benutzer muss über den Prozess informiert werden, auf dem warten. Enthalten Sie eine aussagekräftige Bezeichnung (in der Regel eine einzelne, vollständige Satz) an, die den Task eindeutig definiert.

## <a name="about-progress-bars"></a>Über Statusleisten

Eine Statusanzeige wird als eine Zeile, die mit der Farbe an, dass der Status und die Länge der sehr zeitaufwändig füllt dargestellt. Statusleisten sollte immer verwendet werden, wenn die Länge der Aufgaben ist bekannt, oder berechnet werden kann.

Apple hat die folgenden Vorschläge für die Arbeit mit Statusanzeigen:

- **Genau melden Fortschritt** -Statusleisten sollte immer eine genaue Darstellung der die erforderliche Zeit zum Abschließen einer Aufgabe darstellen. Falsch angezeigt werden nie die Zeit für die Anwendung ausgelastet angezeigt werden.
- **Verwenden Sie für eine klar definierte Dauer** -Status, die leisten soll nicht nur, dass eine langwierige Aufgabe dauert anzuzeigen, platzieren, jedoch erhält der Benutzer und Überblick, wie viel von der Aufgabe abgeschlossen ist und eine Schätzung der verbleibenden Zeit.

## <a name="progress-indicators-and-storyboards"></a>Statusanzeigen und storyboards

Die einfachste Möglichkeit zum Arbeiten mit einer Statusanzeige in einer Xamarin.tvOS-app ist es der app Benutzeroberfläche, die Verwendung des iOS Designers hinzufügen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
1. In der **Lösungspad**, doppelklicken Sie auf die **"Main.Storyboard"** und öffnen Sie sie für die Bearbeitung.

2. Ziehen Sie ein **Aktivitätsanzeige** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    ![Ein Indikator für die Aktivität](progress-indicators-images/activity01.png "ein Indikator für die Aktivität")

3. In der **Widget** Registerkarte die **Pad "Eigenschaften"**, können Sie verschiedene Eigenschaften des Indikators Aktivität wie z. B. Anpassen der **Stil**, **Verhalten**, und **Namen**: 

    ![Die Registerkarte "Widget" für einen Indikator für die Aktivität](progress-indicators-images/activity02.png "die Widget-Registerkarte für einen Indikator für die Aktivität")
    
    Die **Namen** bestimmt den Namen der Eigenschaft, die den Aktivität Indikator in darstellt C# Code.

4. Ziehen Sie eine **Bearbeitung** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    ![Eine Ansicht für die Bearbeitung](progress-indicators-images/activity03.png "eine Status-Ansicht")

5. In der **Widget** auf der Registerkarte die **Property Explorer**, können Sie mehrere Eigenschaften der Sicht ausgeführt wie z. B. Anpassen der **Stil**, **Fortschritt**(Prozent abgeschlossen), und **Namen**: 

    ![Der Registerkarte "Widget" für eine Status-Ansicht](progress-indicators-images/activity04.png "die Widget-Registerkarte für eine Sicht wird ausgeführt")
    
    Die **Namen** bestimmt den Namen der Eigenschaft, die die Status-Ansicht in darstellt C# Code.

6. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die **"Main.Storyboard"** und öffnen Sie sie für die Bearbeitung.

2. Ziehen Sie ein **Aktivitätsanzeige** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    ![Ein Indikator für die Aktivität](progress-indicators-images/activity01-vs.png
    "ein Indikator für die Aktivität")

3. In der **Widget** Registerkarte die **Eigenschaften-Explorer**, können Sie verschiedene Eigenschaften des Indikators Aktivität wie z. B. Anpassen der **Stil**, **Verhalten**, und **Namen**: 

    ![Die Registerkarte "Widget" für einen Indikator für die Aktivität](progress-indicators-images/activity02-vs.png "die Widget-Registerkarte für einen Indikator für die Aktivität")

    Die **Namen** bestimmt den Namen der Eigenschaft, die den Aktivität Indikator in darstellt C# Code.

4. Ziehen Sie eine **Bearbeitung** aus der **Toolbox** und legen Sie sie in der Ansicht: 

   ![Eine Ansicht für die Bearbeitung](progress-indicators-images/activity03-vs.png "eine Status-Ansicht")

5. In der **Widget** auf der Registerkarte die **Property Explorer**, können Sie mehrere Eigenschaften der Sicht ausgeführt wie z. B. Anpassen der **Stil**, **Fortschritt**(Prozent abgeschlossen), und **Namen**: 

    ![Der Registerkarte "Widget" für eine Status-Ansicht](progress-indicators-images/activity04-vs.png "die Widget-Registerkarte für eine Sicht wird ausgeführt")
    
    Die **Namen** bestimmt den Namen der Eigenschaft, die die Status-Ansicht in darstellt C# Code.

6. Speichern Sie die Änderungen.

-----

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Arbeiten mit aktivitätsindikatoren

Wie bereits erwähnt, sollte aktivitätsindikatoren angezeigt werden, wenn Ihre app einen langen Prozess unbestimmten Länge ausgeführt wird.

Zu jedem Zeitpunkt können Sie sehen, ob ein Indikator für die Aktivität, indem Sie überprüfen animiert wird die `IsAnimating` Eigenschaft. Wenn die `HidesWhenStopped` Eigenschaft `true`, der Indikator für die Aktivität automatisch ausgeblendet wird, wenn die Animation beendet wurde.

Sie können den folgenden Code zum Starten der Animation verwenden: 

```csharp
ActivityIndicator.StartAnimating();
```

Und im folgenden wird die Animation beendet:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Diese Codeausschnitte davon ausgehen, dass der Aktivitätsanzeige **Namen** wurde **ActivityIndicator** in die **Widget** der iOS-Designer auf der Registerkarte.

## <a name="working-with-progress-bars"></a>Arbeiten mit den Statusleisten

In diesem Fall sollte eine Statusanzeige jederzeit verwendet werden Ihre app eine langwierige Aufgabe einer bekannten Dauer ausgeführt wird. 

Die `Progress` Eigenschaft wird verwendet, um die Menge der Aufgabe festgelegt, der auf 100 % (0.0, 1.0) von 0 % abgeschlossen wurde. Verwenden der `ProgressTintColor` Eigenschaft zum Festlegen der Farbe des Balkens Menge, die abgeschlossen und die `TrackTintColor` Eigenschaft, um die Farbe des Hintergrunds (nicht abgeschlossenen Zeitraum) festzulegen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit der Statusanzeige in ein Xamarin.tvOS-app.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
