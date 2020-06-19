---
title: Xamarin.FormsProgressBar
description: Das Xamarin.Forms ProgressBar-Steuerelement ist ein Steuerelement, das den Fortschritt visuell als horizontalen Balken darstellt, der basierend auf einer float-Eigenschaft ausgefüllt wird.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b4ac6231c0483c0c44755c2ac9539f237dd64251
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136278"
---
# <a name="xamarinforms-progressbar"></a>Xamarin.FormsProgressBar
[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Das- Xamarin.Forms [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Steuerelement stellt den Fortschritt visuell als horizontalen Balken dar, der zu einem durch einen-Wert dargestellten Prozentsatz gefüllt wird `float` . Die `ProgressBar` Klasse erbt von [`View`](xref:Xamarin.Forms.View) .

Die folgenden Screenshots zeigen eine `ProgressBar` unter iOS und Android:

![Screenshot von ProgressBar unter IOS und Android](progressbar-images/progressbars-default.png "ProgressBar unter IOS und Android")

Das- `ProgressBar` Steuerelement definiert zwei Eigenschaften:

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)ein- `float` Wert, der den aktuellen Fortschritt als Wert zwischen 0 und 1 darstellt. `Progress`Werte, die kleiner als 0 sind, werden an 0 gebunden, Werte, die größer als 1 sind, werden an 1 gebunden.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)ist eine `Color` , die die Farbe der inneren Leiste beeinflusst, die den aktuellen Fortschritt darstellt.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte gestützt. Dies bedeutet, dass das formatiert `ProgressBar` werden kann und das Ziel von Daten Bindungen ist.

Das- `ProgressBar` Steuerelement definiert auch eine `ProgressTo` Methode, die die Leiste von Ihrem aktuellen Wert auf einen angegebenen Wert animiert. Weitere Informationen finden Sie unter [Animieren einer ProgressBar](#animate-a-progressbar).

> [!NOTE]
> `ProgressBar`Akzeptiert die Benutzer Manipulation nicht, sodass Sie übersprungen wird, wenn die Tab-Taste verwendet wird, um Steuerelemente auszuwählen.

## <a name="create-a-progressbar"></a>Erstellen einer ProgressBar

Eine `ProgressBar` kann in XAML instanziiert werden. `Progress`Die-Eigenschaft bestimmt den Füll Prozentsatz der inneren, farbigen Leiste. Der Standard `Progress` Eigenschafts Wert ist 0 (null). Im folgenden Beispiel wird gezeigt, wie ein `ProgressBar` in XAML mit dem optionalen Eigenschaften Satz instanziiert wird `Progress` :

```xaml
<ProgressBar Progress="0.5" />
```

Ein `ProgressBar` kann auch im Code erstellt werden:

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> Verwenden Sie keine eingeschränkten horizontalen Layoutoptionen wie `Center` , `Start` oder `End` mit `ProgressBar` . Bei UWP reduziert sich der `ProgressBar` auf einen Balken der Breite 0 (null). Behalten Sie den Standard `HorizontalOptions` Wert von `Fill` bei, und verwenden Sie keine Breite von, wenn Sie `Auto` eine `ProgressBar` in einem `Grid` Layout platzieren.

## <a name="progressbar-appearance-properties"></a>ProgressBar-Darstellungs Eigenschaften

Die- `ProgressColor` Eigenschaft definiert die Farbe der inneren Leiste, wenn die- `Progress` Eigenschaft größer als 0 (null) ist. Im folgenden Beispiel wird gezeigt, wie ein `ProgressBar` in XAML mit dem-Eigenschaften Satz instanziiert wird `ProgressColor` :

```xaml
<ProgressBar ProgressColor="Orange" />
```

Die- `ProgressColor` Eigenschaft kann auch festgelegt werden, wenn ein-Objekt `ProgressBar` im Code erstellt wird:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

Die folgenden Screenshots zeigen den `ProgressBar` , bei dem die `ProgressColor` -Eigenschaft unter `Color.Orange` IOS und Android auf festgelegt ist:

![Screenshot der formatierten ProgressBar unter IOS und Android](progressbar-images/progressbars-styled.png "ProgressBar im Stil für IOS und Android")

## <a name="animate-a-progressbar"></a>Animieren einer ProgressBar

Mit der- `ProgressTo` Methode wird die `ProgressBar` von Ihrem aktuellen `Progress` Wert zu einem bereitgestellten Wert im Laufe der Zeit animiert. Die-Methode akzeptiert einen `float` Fortschritts Wert, eine `uint` Dauer in Millisekunden, einen Enumerationswert `Easing` und gibt einen zurück `Task<bool>` . Der folgende Code veranschaulicht, wie Sie eine animieren `ProgressBar` :

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

Weitere Informationen zur- `Easing` Enumeration finden Sie unter Beschleunigungs [Funktionen in Xamarin.Forms ](~/xamarin-forms/user-interface/animation/easing.md).

## <a name="related-links"></a>Verwandte Links

* [ProgressBar-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
