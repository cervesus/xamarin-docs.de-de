---
title: Status- und Aktivitätsindikatoren in Xamarin.iOS
description: In diesem Dokument wird erläutert, wie Status und Aktivität von Indikatoren in Xamarin.iOS verwendet wird. Es wird beschrieben, wie sie sowohl programmgesteuert als auch mit einem Storyboard verwenden.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 24ad47bd37693eccf38033159acef72a9d4942be
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242178"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Status- und Aktivitätsindikatoren in Xamarin.iOS

Ist es wahrscheinlich, dass Ihre app, zum Ausführen lang hat ausgeführte Aufgaben wie z. B. Laden oder Verarbeiten von Daten und, dass diese Verzögerung bei der Aktualisierung der Benutzeroberflächenautomatisierungs, eine Verzögerung verursachen kann. Während dieser Zeit sollten Sie immer eine Statusanzeige verwenden, dem Benutzer zu versichern, dass das System Arbeit ausgelastet ist. Dadurch wird das Benutzersteuerelement, das die app funktioniert bei ihrer Anforderung, die die Eingabe nicht gewartet wird, und bieten eine Möglichkeit, genau geschildert, genau wie lange sie warten müssen.

iOS bietet zwei Arten, um diese Fortschrittsanzeige in Ihrer app bereitzustellen: Aktivitätsindikatoren (einschließlich einen bestimmten _Netzwerk_ Aktivitätsanzeige) und Statusanzeigen.

## <a name="activity-indicator"></a>Aktivität-Indikator

Aktivitätsindikatoren sollte angezeigt werden, wenn Ihre app einen langen Prozess ausgeführt wird, aber Sie wissen nicht, die genaue Länge der Zeit, die die Aufgabe erforderlich ist.

Apple hat die folgenden Vorschläge für die Arbeit mit Aktivitätsindikatoren:

- **Verwenden Sie nach Möglichkeit stattdessen Balken Fortschritt** : Da ein Indikator für die Aktivität dem Benutzer, kein Feedback ermöglicht, wie lange der ausgeführte Prozess dauert, verwenden Sie immer eine Statusanzeige angezeigt, wenn die Länge (z. B., wie viele Bytes in eine Datei herunterladen) bekannt ist.
- **Behalten Sie den Indikator animierte** -Benutzer ein stationär Aktivität Indikator für eine angehaltene app beziehen, damit Sie immer angeben, sollte den Indikator animiert, während er angezeigt wird, wird.
- **Beschreiben Sie den Task verarbeiteten** -lediglich zum Anzeigen der Aktivität Indikator allein reicht nicht aus, die der Benutzer muss über den Prozess informiert werden, warten auf. Enthalten Sie eine aussagekräftige Bezeichnung (in der Regel eine einzelne, vollständige Satz) an, die den Task eindeutig definiert.

### <a name="implementing-an-activity-indicator"></a>Implementieren einen Indikator für die Aktivität

Ein Indikator für die Aktivität wird implementiert, über die [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) Klasse an, dass eine `UIActivity` stattfindet.

### <a name="activity-indicators-and-storyboards"></a>Aktivitätsindikatoren und Storyboards

Wenn Sie zum Erstellen der Benutzeroberflächenautomatisierungs der iOS-Designer verwenden, können den Indikator für die Aktivität auf das Layout aus der Toolbox hinzugefügt werden. Die folgenden Eigenschaften können über das Pad "Eigenschaften" angepasst werden:

![Pad "Eigenschaften"](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Verwalten von Indikator das Verhalten einer Aktivität

Verwenden der `StartAnimating()` und `StopAnimating()` Methoden zum Starten und Beenden der Aktivität Indikator Animation.

Legen Sie die `HidesWhenStopped` Eigenschaft `true` die Aktivitätsanzeige verschwinden nach vornehmen `StopAnimating()` aufgerufen wurde. Dieser Wert auf festgelegt `true` standardmäßig. Sie sehen können, ob der Indikator für die Aktivität die Animation rotierende Überprüfung ausgeführt wird, jederzeit die `IsAnimating` Eigenschaft. 


### <a name="managing-activity-indicator-appearances"></a>Verwalten von Aktivitäten Indikator Darstellungen

Die `UIActivityIndicatorViewStyle` Enumeration kann als Parameter übergeben werden, wenn der Indikator für die Aktivität zu instanziieren. Hiermit können Sie den visuellen Stil festlegen, um `Gray`, `White`, oder `WhiteLarge`, z.B.:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Sie können die Farbe von bereitgestellten überschreiben `UIActivityIndicatorViewStyle` durch Festlegen der `Color` Eigenschaft.

## <a name="progress-bar"></a>Statusanzeige

Eine Statusanzeige wird als eine Zeile, die mit der Farbe an, dass der Status und die Länge der sehr zeitaufwändig füllt dargestellt. Statusleisten sollte immer verwendet werden, wenn die Länge der Aufgaben ist, oder berechnet werden kann.

Apple hat die folgenden Vorschläge für die Arbeit mit Statusanzeigen:

- **Genau Forschritt** -Statusleisten sollte immer eine genaue Darstellung der die erforderliche Zeit zum Abschließen einer Aufgabe sein. Falsch angezeigt werden nie die Zeit für die Anwendung ausgelastet angezeigt werden.
- **Verwendung für die Dauer der Well-Defined** -Statusanzeige sollte nicht nur, dass eine langwierige Aufgabe dauert anzeigen zu platzieren, aber erhält der Benutzer und Überblick, wie viel von der Aufgabe abgeschlossen ist und eine Schätzung der verbleibenden Zeit.

### <a name="implementing-an-progress-bar"></a>Implementieren eine Statusanzeige

Eine Statusanzeige wird erstellt, durch die Instanziierung einer [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Statusanzeigen und Storyboards

Sie können auch eine Statusanzeige an die Benutzeroberfläche hinzufügen, bei Verwendung des iOS-Designers. Suchen Sie nach **Bearbeitung** in die **Toolbox** und ziehen Sie es in der Ansicht.

Die folgenden Eigenschaften können auf das Pad "Eigenschaften" angepasst werden:

![Pad "Eigenschaften"](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Verwalten von Statusanzeige-Verhalten

Der Fortschritt des Balkens anfänglich mithilfe festgelegt werden kann die `Progress` Eigenschaft:

```csharp
ProgressBar.Progress = 0f;
```

Der Fortschritt angepasst werden kann, mithilfe der `SetProgress` -Methode und übergeben einen booleschen Wert, der deklariert, wenn Sie möchten, dass die Änderung, die animiert wird, oder nicht.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Weitere Informationen zur Verwendung der Statusanzeige finden Sie unter der [Reporting ausgeführt](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) Rezept, und die [UICatalog TvOS-Beispiel](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>Verwalten die Darstellung der Statusanzeige

Ein Indikator Aktivität ähnelt der `UIProgressViewStyle` Enumeration kann als Parameter übergeben werden, wenn die Statusanzeige zu instanziieren.

Den Fortschritt und Track-Image und Tönungsfarbe kann angepasst werden, mithilfe der folgenden Eigenschaften:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



