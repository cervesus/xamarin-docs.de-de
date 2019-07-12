---
title: Xamarin.Forms "ProgressBar".
description: Die Xamarin.Forms "ProgressBar" handelt es sich um ein Steuerelement, das visuelle Bearbeitung als horizontalen Balken, der gefüllt ist, und basierend auf einer "float"-Eigenschaft darstellt.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 2e2917077575ebc78dbdda8ae55aa230a39a7ba1
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837010"
---
# <a name="xamarinforms-progressbar"></a>Xamarin.Forms "ProgressBar".
[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)

Die Xamarin.Forms [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar) ist ein Steuerelement, das visuell Fortschritt als horizontalen Balken darstellt, das auf eine Prozentangabe, ausgedrückt durch gefüllt wird eine `float` Wert. Die `ProgressBar` Klasse erbt von [ `View` ](xref:Xamarin.Forms.View).

Der folgende Screenshot zeigt ein `ProgressBar` unter iOS und Android:

![Screenshot der "ProgressBar" unter iOS und Android](progressbar-images/progressbars-default.png "\"ProgressBar\" unter iOS und Android")

Die `ProgressBar` -Steuerelement definiert zwei Eigenschaften:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) ist eine `float` Wert, der den aktuellen Status als Wert von 0 auf 1 darstellt. `Progress` Werte kleiner als 0 wird auf 0 (null) Werte größer als 1 wird auf 1 bindende geklammert.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) ist eine `Color` , beeinflusst das innere Leiste Farbe, die den aktuellen Status darstellt.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekte, die das bedeutet, dass die `ProgressBar` formatiert werden können und das Ziel von datenbindungen verwendet werden.

Die `ProgressBar` Steuerelement definiert auch eine `ProgressTo` -Methode, die die Leiste der aktuelle Wert auf einen angegebenen Wert animiert. Weitere Informationen finden Sie unter [animieren Sie eine Statusanzeige](#animate-a-progressbar).

> [!NOTE]
> Die `ProgressBar` akzeptiert keine Bearbeitung durch Benutzer aus, damit er wird übersprungen, wenn die Tab-Taste verwenden, zum Auswählen von Steuerelementen.

## <a name="create-a-progressbar"></a>Erstellen Sie eine Statusanzeige

Ein `ProgressBar` in XAML instanziiert werden. Die `Progress` Eigenschaft kann festgelegt werden, um zu bestimmen, den Fill-Prozentsatz, der die innere, farbige Leiste. Wenn die `Progress` Eigenschaft nicht festgelegt ist, wird standardmäßig auf 0. Das folgende Beispiel zeigt, wie Sie instanziieren ein `ProgressBar` in XAML mit dem optionalen `Progress` Eigenschaftensatz:

```xaml
<ProgressBar Progress="0.5" />
```

Ein `ProgressBar` können auch im Code erstellt werden:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Verwenden Sie keine uneingeschränkte horizontales Layout-Optionen wie z. B. `Center`, `Start`, oder `End` mit `ProgressBar`. Auf UWP die `ProgressBar` reduziert auf einen Balken, der größer als NULL sein. Behalten Sie den Standardwert `HorizontalOptions` Wert `Fill` und keine Breite von `Auto` beim Festlegen einer `ProgressBar` in einer `Grid` Layout.

## <a name="progressbar-appearance-properties"></a>ProgressBar-Darstellungseigenschaften

Die `ProgressColor` Eigenschaft kann festgelegt werden, definieren Sie die innere Leiste Farbe, wenn die `Progress` -Eigenschaft ist größer als 0 (null). Das folgende Beispiel zeigt, wie Sie instanziieren ein `ProgressBar` in XAML mit dem `ProgressColor` Eigenschaftensatz:

```xaml
<ProgressBar OnColor="Orange" />
```

Die `ProgressColor` Eigenschaft kann auch festgelegt werden, beim Erstellen einer `ProgressBar` im Code:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

Der folgende Screenshot zeigt die `ProgressBar` mit der `ProgressColor` -Eigenschaftensatz auf `Color.Orange` unter iOS und Android:

![Screenshot des formatierten "ProgressBar" unter iOS und Android](progressbar-images/progressbars-styled.png "im Stil von \"ProgressBar\" unter iOS und Android")

## <a name="animate-a-progressbar"></a>Animieren Sie eine Statusanzeige

Die `ProgressTo` Methode erstellt eine Animation die `ProgressBar` von seinem aktuellen `Progress` Wert mit einem angegebenen Wert im Laufe der Zeit. Die Methode akzeptiert eine `float` Status-Wert, ein `uint` Dauer in Millisekunden, ein `Easing` Enum-Wert und gibt eine `Task<bool>`. Der folgende Code zeigt, wie zum Animieren einer `ProgressBar`:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Weitere Informationen zu den `Easing` -Enumeration finden Sie unter [Beschleunigungsfunktionen in Xamarin.Forms](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Verwandte Links

* [ProgressBar-Demos](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)
