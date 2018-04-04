---
title: Status und Aktivität-Indikatoren
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 58a492bed81a1d96a482c1396718da1c5e4af589
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="progress-and-activity-indicators"></a>Status und Aktivität-Indikatoren

Ist es wahrscheinlicher, dass Ihre app hat beziehenden lang ausgeführter Aufgaben, z. B. beim Laden oder Verarbeiten von Daten und, dass diese Verzögerung eine Verzögerung bewirken kann, aktualisieren Sie die Benutzeroberfläche. Während dieser Zeit sollten Sie immer eine Statusanzeige verwenden, um dem Benutzer versichern, dass das System ausgelastet Arbeit ist. Dadurch werden das Benutzersteuerelement, an der die app ihre Anforderung arbeitet, die sie nicht für ihre Eingabe wartet und können bieten eine Möglichkeit der ausführlichen genau wie lange gewartet haben.

iOS bietet zwei Hauptmethoden, um diese Angabe Fortschritt in Ihrer app bereitzustellen: Aktivität Indikatoren (einschließlich einen bestimmten _Netzwerk_ Aktivität Indikator) und Statusanzeigen.

## <a name="activity-indicator"></a>Indikator der Aktivität

Aktivität Indikatoren sollte angezeigt werden, wenn Ihre app einen langen Prozess ausgeführt wird, aber Sie wissen nicht, die genaue Länge der Zeit, die die Aufgabe benötigen.

Apple hat die folgenden Vorschläge für den Umgang mit Indikatoren Aktivität:

- **Verwenden Sie nach Möglichkeit stattdessen Balken Fortschritt** – da ein Indikator für die Aktivität dem Benutzer keine Rückmeldung erhält über die Dauer der ausgeführte Prozess dauert, immer eine Statusanzeige verwenden, wenn die Länge (z. B., wie viele Bytes zum Herunterladen einer Datei) bekannt ist.
- **Behalten Sie den Indikator animierte** -Benutzer eine stationär Indikator für die Aktivität an eine angehaltene app beziehen, damit Sie immer haben, sollte den Indikator animiert, während er angezeigt wird, wird.
- **Beschreiben Sie den Task verarbeiteten** -lediglich zum Anzeigen der Aktivität Indikator allein reicht nicht aus, die der Benutzer muss über den Prozess informiert werden, warten auf. Schließen Sie eine aussagekräftige Bezeichnung (in der Regel einen einzelnen, vollständigen Satz), die die Aufgabe klar definiert.

### <a name="implementing-an-activity-indicator"></a>Implementieren einen Indikator für die Aktivität

Ein Indikator für die Aktivität wird implementiert, über die [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) Klasse an, dass eine `UIActivity` ist, die stattfinden.

### <a name="activity-indicators-and-storyboards"></a>Aktivität Indikatoren und Storyboards

Wenn Sie den iOS-Designer verwenden, um die Benutzeroberfläche zu erstellen, können der Indikator für die Aktivität auf das Layout aus der Toolbox hinzugefügt werden. Die folgenden Eigenschaften können über das Auffüllzeichen Eigenschaften angepasst werden:

![Eigenschaften mit Leerstellen auffüllen](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Verwalten von Indikator das Verhalten einer Aktivität

Verwenden der `StartAnimating()` und `StopAnimating()` Methoden zum Starten und Beenden der Aktivität Indikator Animation.

Festlegen der `HidesWhenStopped` Eigenschaft `true` zu der Aktivität Indikator verschwinden nach `StopAnimating()` aufgerufen wurde. Dieser Wert ist festgelegt, um `true` standardmäßig. Zu jedem Zeitpunkt, Sie können sehen, wenn der Indikator für die Aktivität die Spinvorgänge Animation durch die Überprüfung ausgeführt wird, die `IsAnimating` Eigenschaft. 


### <a name="managing-activity-indicator-appearances"></a>Verwalten von Aktivität Indikator Eindeutigkeitsmetrik

Die `UIActivityIndicatorViewStyle` Enumeration kann als Parameter übergeben werden, wenn der Indikator für die Aktivität instanziiert. Sie können Hiermit können Sie den visuellen Stil festgelegt `Gray`, `White`, oder `WhiteLarge`, beispielsweise:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Sie können die Farbe von bereitgestellten überschreiben `UIActivityIndicatorViewStyle` durch Festlegen der `Color` Eigenschaft.

## <a name="progress-bar"></a>Statusanzeige

Eine Statusanzeige wird als eine Zeile, die mit der Farbe an, die die Status und Länge der sehr zeitaufwändig anzugeben, füllt dargestellt. Statusanzeigen sollte immer verwendet werden, wenn die Länge der Aufgaben bekannt ist, oder berechnet werden kann.

Apple hat die folgenden Vorschläge für die Arbeit mit Statusanzeigen:

- **Genau Fortschritt** -Statusanzeigen sollte immer eine genaue Darstellung der Zeitaufwand zum Abschließen einer Aufgabe sein. Nie sind so dargestellt, die Zeit, um die app ausgelastet angezeigt werden.
- **Verwendung für die Dauer der Well-Defined** -Statusanzeige sollte nur nicht angezeigt, dass eine langwierige Aufgabe dauert platzieren, aber der Benutzer und Überblick, wie viel des Tasks abgeschlossen ist und eine Schätzung der verbleibenden Zeit bereitstellen.

### <a name="implementing-an-progress-bar"></a>Implementieren eine Statusanzeige

Eine Statusanzeige wird erstellt, durch die Instanziierung einer [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Statusanzeigen und Storyboards

Sie können auch eine Statusanzeige an die Benutzeroberfläche hinzufügen, wenn die iOS-Designer verwenden. Suchen Sie nach **Bearbeitung** in der **Toolbox** und ziehen Sie es in der Ansicht.

Die folgenden Eigenschaften können auf das Auffüllzeichen Eigenschaften angepasst werden:

![Eigenschaften mit Leerstellen auffüllen](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Verwalten von Statusanzeige-Verhalten

Der Fortschritt des Balkens kann anfänglich festgelegt werden, mithilfe der `Progress` Eigenschaft:

```csharp
ProgressBar.Progress = 0f;
```

Der Fortschritt angepasst werden kann, mithilfe der `SetProgress` -Methode und übergeben einen booleschen Wert ab, der deklariert wird, wenn die Änderung oder nicht animiert werden soll.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Weitere Informationen finden Sie unter der Statusanzeige, finden Sie in der [Reporting ausgeführt](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS) Rezept, und die [UICatalog tvos. außerdem wurden Beispiel](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>Verwalten von Statusanzeige-Darstellung

Ein Indikator Aktivität ähnelt der `UIProgressViewStyle` Enumeration kann als Parameter übergeben werden, wenn die Statusanzeige zu instanziieren.

Den Fortschritt und Track-Image und Farbton kann angepasst werden, mithilfe der folgenden Eigenschaften:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



