---
title: Aktivitätsindikator inXamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a83885175a44f2174db343abf4591f8777041d39
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136512"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.FormsActivityindicator
[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Das- Xamarin.Forms [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) Steuerelement zeigt eine Animation an, die anzeigt, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist. Anders als der [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) `ActivityIndicator` gibt den Fortschritt an. Der `ActivityIndicator` erbt von [`View`](xref:Xamarin.Forms.View) .

Die folgenden Screenshots zeigen ein `ActivityIndicator` Steuerelement unter IOS und Android:

![Screenshot von activityindicator unter IOS und Android](activityindicator-images/activityindicators-default.png "Screenshot von activityindicator unter IOS und Android")

Das- `ActivityIndicator` Steuerelement definiert die folgenden Eigenschaften:

* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)ein- `Color` Wert, der die Anzeige Farbe von definiert `ActivityIndicator` .
* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)ein `bool` Wert, der angibt, ob `ActivityIndicator` sichtbar und animiert werden soll. Wenn der Wert ist, ist `false` `ActivityIndicator` nicht sichtbar.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte gestützt. Dies bedeutet, dass das formatiert `ActivityIndicator` werden kann und das Ziel von Daten Bindungen ist.

## <a name="create-an-activityindicator"></a>Erstellen eines activityindicator

Die `ActivityIndicator` Klasse kann in XAML instanziiert werden. `IsRunning`Die-Eigenschaft bestimmt, ob das Steuerelement sichtbar und animiert ist. Die-Eigenschaft hat den `IsRunning` Standardwert `false` . Im folgenden Beispiel wird gezeigt, wie ein `ActivityIndicator` in XAML mit dem optionalen Eigenschaften Satz instanziiert wird `IsRunning` :

```xaml
<ActivityIndicator IsRunning="true" />
```

Ein `ActivityIndicator` kann auch im Code erstellt werden:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Aktivitätsindikator-Darstellungs Eigenschaften

Die- `Color` Eigenschaft definiert die `ActivityIndicator` Farbe. Im folgenden Beispiel wird gezeigt, wie ein `ActivityIndicator` in XAML mit dem-Eigenschaften Satz instanziiert wird `Color` :

```xaml
<ActivityIndicator Color="Orange" />
```

Die- `Color` Eigenschaft kann auch festgelegt werden, wenn ein-Objekt `ActivityIndicator` im Code erstellt wird:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

Die folgenden Screenshots zeigen den `ActivityIndicator` , bei dem die `Color` -Eigenschaft unter `Color.Orange` IOS und Android auf festgelegt ist:

![Screenshot des formatierten activityindicator unter IOS und Android](activityindicator-images/activityindicators-styled.png "Screenshot des formatierten activityindicator unter IOS und Android")

## <a name="related-links"></a>Verwandte Links

* [Activityindicator-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
