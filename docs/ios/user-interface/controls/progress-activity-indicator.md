---
title: Status-und Aktivitätsindikatoren in xamarin. IOS
description: In diesem Dokument wird erläutert, wie Status-und Aktivitätsindikatoren in xamarin. IOS verwendet werden. Es wird beschrieben, wie Sie sowohl Programm gesteuert als auch mit einem Storyboard verwendet werden.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2017
ms.openlocfilehash: 76e1ee54a5e1b729fdcb0b0a2c1f278703b2b4d6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021969"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Status-und Aktivitätsindikatoren in xamarin. IOS

Es ist wahrscheinlich, dass Ihre APP Aufgaben mit langer Ausführungsdauer ausführen muss, z. b. das Laden oder Verarbeiten von Daten, und dass diese Verzögerung zu einer Verzögerung bei der Aktualisierung der Benutzeroberfläche führen kann. Während dieser Zeit sollten Sie immer eine Statusanzeige verwenden, um dem Benutzer zu versichern, dass das System ausgelastet ist. Dadurch wird dem Benutzer Steuerelement ermöglicht, dass die APP an Ihrer Anforderung arbeitet, dass Sie nicht auf Ihre Eingabe wartet und eine Möglichkeit bereitstellen kann, um genau zu bestimmen, wie lange Sie warten müssen.

IOS bietet zwei Hauptmöglichkeiten, diese Fortschrittsanzeige in Ihrer APP bereitzustellen: Aktivitätsindikatoren (einschließlich eines bestimmten _Netzwerk_ Aktivitäts Indikators) und Status leisten.

## <a name="activity-indicator"></a>Aktivitätsindikator

Aktivitätsindikatoren sollten angezeigt werden, wenn Ihre APP einen langen Prozess ausgeführt hat, aber Sie wissen nicht genau, wie lange die Aufgabe benötigt.

Apple hat die folgenden Vorschläge zum Arbeiten mit Aktivitätsindikatoren:

- **Verwenden Sie nach Möglichkeit stattdessen** Status anzeigen, da der Benutzer in einem Aktivitätsindikator kein Feedback darüber gibt, wie lange der Prozess ausgeführt wird. verwenden Sie immer eine Statusanzeige, wenn die Länge bekannt ist (z. b., wie viele Bytes in einer Datei heruntergeladen werden müssen).
- **Halten Sie den Indikator animiert** : Benutzer verknüpfen einen Indikator für eine stationäre Aktivität mit einer angehaltenen APP, sodass der Indikator immer animiert werden muss, während er angezeigt wird.
- **Beschreiben der verarbeiteten Aufgabe** : nur das Anzeigen des Aktivitäts Indikators allein ist nicht ausreichend, der Benutzer muss über den Prozess informiert werden, auf den Sie warten. Fügen Sie eine aussagekräftige Bezeichnung (in der Regel einen einzelnen, vollständigen Satz) ein, die die Aufgabe eindeutig definiert.

### <a name="implementing-an-activity-indicator"></a>Implementieren eines Aktivitäts Indikators

Ein Aktivitätsindikator wird durch die [`UIActivityIndictorView`](xref:UIKit.UIActivityIndicatorView) -Klasse implementiert, um anzugeben, dass ein `UIActivity` stattfindet.

### <a name="activity-indicators-and-storyboards"></a>Aktivitätsindikatoren und Storyboards

Wenn Sie den IOS-Designer verwenden, um die Benutzeroberfläche zu erstellen, kann der Aktivitätsindikator dem Layout aus der Toolbox hinzugefügt werden. Die folgenden Eigenschaften können von der Eigenschaftenpad angepasst werden:

![Eigenschaftenpad](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Verwalten des Aktivitätsindikator Verhaltens

Verwenden Sie die Methoden `StartAnimating()` und `StopAnimating()`, um die Aktivitätsindikator Animation zu starten und zu unterbinden.

Legen Sie die `HidesWhenStopped`-Eigenschaft auf `true` fest, damit der Aktivitätsindikator nach dem Aufrufen `StopAnimating()` verschwindet. Diese Einstellung ist standardmäßig auf `true` festgelegt. An jedem Punkt können Sie festzustellen, ob der Aktivitätsindikator seine drehende Animation durch Überprüfen der `IsAnimating`-Eigenschaft ausgeführt wird. 

### <a name="managing-activity-indicator-appearances"></a>Verwalten von Aktivitätsindikator Auftritten

Die `UIActivityIndicatorViewStyle` Enumeration kann als Parameter übergeben werden, wenn der Aktivitätsindikator instanziiert wird. Sie können dies verwenden, um den visuellen Stil auf `Gray`, `White`oder `WhiteLarge`festzulegen, z. b.:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Sie können die von `UIActivityIndicatorViewStyle` bereitgestellte Farbe überschreiben, indem Sie die `Color`-Eigenschaft festlegen.

## <a name="progress-bar"></a>Statusanzeige

Eine Statusanzeige wird als Linie dargestellt, die die Farbe füllt, um den Zustand und die Länge einer zeitaufwändigen Aufgabe anzugeben. Status leisten sollten immer verwendet werden, wenn die Länge der Aufgaben bekannt ist oder berechnet werden kann.

Apple hat die folgenden Vorschläge zum Arbeiten mit Status leisten:

- **Genaueres melden** von Status leisten sollten immer eine genaue Darstellung der Zeit sein, die zum Ausführen einer Aufgabe erforderlich ist. Geben Sie niemals die Zeit an, die die APP als ausgelastet angezeigt werden soll.
- **Für klar definierte Dauer verwenden** : die Statusanzeige sollte nicht nur zeigen, dass eine lange Aufgabe stattfindet, sondern den Benutzer und die Angabe, wie viel der Aufgabe abgeschlossen ist, und eine Schätzung der verbleibenden Zeit anzeigen.

### <a name="implementing-an-progress-bar"></a>Implementieren einer Statusanzeige

Eine Statusanzeige wird erstellt, indem ein [`UIProgressView`](xref:UIKit.UIProgressView) instanziiert wird.

### <a name="progress-bars-and-storyboards"></a>Status anzeigen und Storyboards

Wenn Sie den IOS-Designer verwenden, können Sie der Benutzeroberfläche auch eine Statusanzeige hinzufügen. Suchen Sie in der **Toolbox** nach der **Fortschritts Ansicht** , und ziehen Sie Sie in die Ansicht.

Die folgenden Eigenschaften können im eigenschaftenpad angepasst werden:

![Eigenschaftenpad](progress-activity-indicator-images/progress-indicator3.png)

### <a name="managing-progress-bar-behavior"></a>Verwalten des Statusanzeige Verhaltens

Der Status der Leiste kann anfänglich mit der `Progress`-Eigenschaft festgelegt werden:

```csharp
ProgressBar.Progress = 0f;
```

Der Fortschritt kann mit der `SetProgress`-Methode angepasst und eine boolesche Deklaration übergeben werden, wenn die Änderung animiert werden soll oder nicht.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Weitere Informationen zur Verwendung der Statusanzeige finden Sie in der Anleitung zum [melden](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) des Status und im [Beispiel uicatalog tvos](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-uicatalog).

### <a name="managing-progress-bar-appearance"></a>Verwalten der Statusanzeige

Ähnlich wie bei einem Aktivitätsindikator kann die `UIProgressViewStyle` Enumeration beim Instanziieren der Statusanzeige als Parameter übergeben werden.

Der Fortschritt und die Bild-und Bildfarben können mithilfe der folgenden Eigenschaften angepasst werden:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```
