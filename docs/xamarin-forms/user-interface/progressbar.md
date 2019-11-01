---
title: Xamarin. Forms ProgressBar
description: Das xamarin. Forms ProgressBar-Steuerelement ist ein Steuerelement, das den Fortschritt visuell als horizontalen Balken darstellt, der auf der Grundlage einer float-Eigenschaft ausgefüllt wird.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 40f3c9a5af29d1782249775b9f3166698eb6221a
ms.sourcegitcommit: 7acff5c5a03a0351962c05beebfc347503d83fe6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73201944"
---
# <a name="xamarinforms-progressbar"></a>Xamarin. Forms ProgressBar
[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Das xamarin. Forms- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Steuerelement stellt den Fortschritt visuell als horizontalen Balken dar, der zu einem Prozentsatz aufgefüllt wird, der durch einen `float` Wert dargestellt wird. Die `ProgressBar`-Klasse erbt von [`View`](xref:Xamarin.Forms.View).

Die folgenden Screenshots zeigen eine `ProgressBar` unter IOS und Android:

![Screenshot von ProgressBar unter IOS und Android](progressbar-images/progressbars-default.png "ProgressBar unter IOS und Android")

Das `ProgressBar`-Steuerelement definiert zwei Eigenschaften:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) ist ein `float` Wert, der den aktuellen Fortschritt als Wert zwischen 0 und 1 darstellt. `Progress` Werte, die kleiner als 0 sind, an 0 (null) gebunden werden, werden Werte, die größer als 1 sind, an 1 gebunden.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) ist ein `Color`, der sich auf die Farbe der inneren Leiste auswirkt, die den aktuellen Fortschritt darstellt.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die `ProgressBar` formatiert werden können und das Ziel von Daten Bindungen sind.

Das `ProgressBar`-Steuerelement definiert auch eine `ProgressTo` Methode, die die Leiste von Ihrem aktuellen Wert auf einen angegebenen Wert animiert. Weitere Informationen finden Sie unter [Animieren einer ProgressBar](#animate-a-progressbar).

> [!NOTE]
> Die `ProgressBar` akzeptiert keine Benutzer Bearbeitung, sodass Sie übersprungen wird, wenn Sie die Tab-Taste verwenden, um Steuerelemente auszuwählen.

## <a name="create-a-progressbar"></a>Erstellen einer ProgressBar

Eine `ProgressBar` kann in XAML instanziiert werden. Die `Progress`-Eigenschaft bestimmt den Füll Prozentsatz der inneren, farbigen Leiste. Der Standardwert `Progress`-Eigenschaft ist 0 (null). Im folgenden Beispiel wird gezeigt, wie ein `ProgressBar` in XAML mit dem optionalen `Progress` Eigenschaften Satz instanziiert wird:

```xaml
<ProgressBar Progress="0.5" />
```

Eine `ProgressBar` kann auch im Code erstellt werden:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Verwenden Sie keine eingeschränkten horizontalen Layoutoptionen, wie z. b. `Center`, `Start`oder `End` mit `ProgressBar`. Bei UWP reduziert der `ProgressBar` auf einen Balken mit der Breite 0 (null). Behalten Sie die Standard `HorizontalOptions` Wert `Fill` bei, und verwenden Sie keine Breite von `Auto`, wenn Sie eine `ProgressBar` in einem `Grid` Layout platzieren.

## <a name="progressbar-appearance-properties"></a>ProgressBar-Darstellungs Eigenschaften

Die `ProgressColor`-Eigenschaft definiert die Farbe der inneren Leiste, wenn die `Progress`-Eigenschaft größer als 0 (null) ist. Im folgenden Beispiel wird gezeigt, wie ein `ProgressBar` in XAML mit dem festgelegten `ProgressColor`-Eigenschaft instanziiert wird:

```xaml
<ProgressBar ProgressColor="Orange" />
```

Die `ProgressColor`-Eigenschaft kann auch beim Erstellen einer `ProgressBar` im Code festgelegt werden:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

Die folgenden Screenshots zeigen die `ProgressBar`, bei der die `ProgressColor`-Eigenschaft unter IOS und Android auf `Color.Orange` festgelegt ist:

![Screenshot der formatierten ProgressBar unter IOS und Android](progressbar-images/progressbars-styled.png "ProgressBar im Stil für IOS und Android")

## <a name="animate-a-progressbar"></a>Animieren einer ProgressBar

Die `ProgressTo`-Methode animiert die `ProgressBar` von Ihrem aktuellen `Progress` Wert zu einem bereitgestellten Wert im Zeitverlauf. Die-Methode akzeptiert einen `float` Fortschritts Wert, eine `uint` Dauer in Millisekunden, einen `Easing` Enumerationswert und gibt einen `Task<bool>`zurück. Der folgende Code veranschaulicht, wie Sie eine `ProgressBar`animieren:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Weitere Informationen zur `Easing`-Enumeration finden Sie unter Beschleunigungs [Funktionen in xamarin. Forms](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Verwandte Links

* [ProgressBar-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
