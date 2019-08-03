---
title: Xamarin. Forms ProgressBar
description: Das xamarin. Forms ProgressBar-Steuerelement ist ein Steuerelement, das den Fortschritt visuell als horizontalen Balken darstellt, der auf der Grundlage einer float-Eigenschaft ausgefüllt wird.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 78c5f38428e20a2e0c6a15d0964f8fd505a8d082
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739412"
---
# <a name="xamarinforms-progressbar"></a>Xamarin. Forms ProgressBar
[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Xamarin. Forms [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) ist ein Steuerelement, das den Fortschritt visuell als horizontalen Balken darstellt, der zu einem durch einen `float` -Wert dargestellten Prozentsatz gefüllt wird. Die `ProgressBar` Klasse erbt von [`View`](xref:Xamarin.Forms.View).

Der folgende Screenshot zeigt ein `ProgressBar` unter IOS und Android:

![Screenshot von ProgressBar unter IOS und Android](progressbar-images/progressbars-default.png "ProgressBar unter IOS und Android")

Das `ProgressBar` -Steuerelement definiert zwei Eigenschaften:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)ein `float` -Wert, der den aktuellen Fortschritt als Wert zwischen 0 und 1 darstellt. `Progress`Werte, die kleiner als 0 sind, werden an 0 gebunden, Werte, die größer als 1 sind, werden an 1 gebunden.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)ist eine `Color` , die die Farbe der inneren Leiste beeinflusst, die den aktuellen Fortschritt darstellt.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte gestützt. Dies bedeutet, dass das `ProgressBar` formatiert werden kann und das Ziel von Daten Bindungen ist.

Das `ProgressBar` -Steuerelement definiert `ProgressTo` auch eine Methode, die die Leiste von Ihrem aktuellen Wert auf einen angegebenen Wert animiert. Weitere Informationen finden Sie unter [Animieren einer ProgressBar](#animate-a-progressbar).

> [!NOTE]
> Akzeptiert `ProgressBar` die Benutzer Manipulation nicht, sodass Sie übersprungen wird, wenn die Tab-Taste verwendet wird, um Steuerelemente auszuwählen.

## <a name="create-a-progressbar"></a>Erstellen einer ProgressBar

Eine `ProgressBar` kann in XAML instanziiert werden. Die `Progress` -Eigenschaft kann festgelegt werden, um den Füll Prozentsatz der inneren, farbigen Leiste zu bestimmen. Wenn die `Progress` Eigenschaft nicht festgelegt ist, wird der Standardwert 0 verwendet. Im folgenden Beispiel wird gezeigt, wie ein `ProgressBar` in XAML mit dem optionalen `Progress` Eigenschaften Satz instanziiert wird:

```xaml
<ProgressBar Progress="0.5" />
```

Ein `ProgressBar` kann auch im Code erstellt werden:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Verwenden Sie keine eingeschränkten horizontalen Layoutoptionen `Center`wie, `Start`oder `End` mit `ProgressBar`. Bei UWP reduziert sich `ProgressBar` der auf einen Balken der Breite 0 (null). Behalten Sie den `HorizontalOptions` Standardwert `Fill` von bei, und verwenden Sie `Auto` keine Breite von `ProgressBar` , wenn `Grid` Sie eine in einem Layout platzieren.

## <a name="progressbar-appearance-properties"></a>ProgressBar-Darstellungs Eigenschaften

Die `ProgressColor` -Eigenschaft kann so festgelegt werden, dass die Farbe der `Progress` inneren Leiste definiert wird, wenn die-Eigenschaft größer als NULL ist. Im folgenden Beispiel wird gezeigt, wie ein `ProgressBar` in XAML mit dem `ProgressColor` -Eigenschaften Satz instanziiert wird:

```xaml
<ProgressBar OnColor="Orange" />
```

Die `ProgressColor` -Eigenschaft kann auch festgelegt werden, `ProgressBar` wenn ein-Objekt im Code erstellt wird:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

Der folgende Screenshot zeigt das `ProgressBar` -Objekt `ProgressColor` , bei dem `Color.Orange` die-Eigenschaft unter IOS und Android auf festgelegt ist:

![Screenshot der formatierten ProgressBar unter IOS und Android](progressbar-images/progressbars-styled.png "ProgressBar im Stil für IOS und Android")

## <a name="animate-a-progressbar"></a>Animieren einer ProgressBar

Mit `ProgressTo` der-Methode wird `ProgressBar` die von Ihrem `Progress` aktuellen Wert zu einem bereitgestellten Wert im Laufe der Zeit animiert. Die-Methode akzeptiert `float` einen Fortschritts Wert, `uint` eine Dauer `Easing` in Millisekunden, einen Enumerationswert und `Task<bool>`gibt einen zurück. Der folgende Code veranschaulicht, wie Sie eine `ProgressBar`animieren:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Weitere Informationen `Easing` zur-Enumeration finden Sie unter Beschleunigungs [Funktionen in xamarin. Forms](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Verwandte Links

* [ProgressBar-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
