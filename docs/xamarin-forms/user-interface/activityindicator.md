---
title: Aktivität-Indikators in Xamarin.Forms
description: ActivityIndicator-Steuerelements zeigt Benutzer, dass die Anwendung in einer langen Aktivität beteiligt ist, ohne dass darauf ausgeführt. In diesem Artikel wird erläutert, wie ein ActivityIndicator in XAML und Code verwendet wird.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: abd8150e3aa4ec347c8d956004993340630208bf
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837060"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.Forms ActivityIndicator
[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)

Die Xamarin.Forms[ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) ist ein Steuerelement, das zeigt eine Animation, um anzugeben, dass die Anwendung in einer langen Aktivität beteiligt ist. Im Gegensatz zu den [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar), `ActivityIndicator` können Sie nicht erkennen des Status. Die `ActivityIndicator` erbt [ `View` ](xref:Xamarin.Forms.View).

Der folgende Screenshot zeigt ein `ActivityIndicator` Steuerelement unter iOS und Android:

![Screenshot des ActivityIndicator unter iOS und Android](activityindicator-images/activityindicators-default.png "Screenshot des ActivityIndicator unter iOS und Android")

Die `ActivityIndicator` Steuerelement definiert die folgenden Eigenschaften:

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) ist eine `bool` Wert, der angibt, ob die `ActivityIndicator` sichtbar und animieren oder ausgeblendet werden soll. Wenn der Wert ist `false` der `ActivityIndicator` nicht sichtbar.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color) ist eine `Color` Wert, der die Anzeigefarbe des definiert die `ActivityIndicator`.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekte, die das bedeutet, dass die `ActivityIndicator` formatiert werden können und das Ziel von datenbindungen verwendet werden.

## <a name="create-an-activityindicator"></a>Erstellen Sie ein ActivityIndicator

Ein `ActivityIndicator` in XAML instanziiert werden. Die `IsRunning` Eigenschaft kann festgelegt werden, um festzustellen, ob das Steuerelement sichtbar und Animation ist. Wenn die `IsRunning` Eigenschaft nicht festgelegt, wird standardmäßig `false` und `ActivityIndicator` nicht sichtbar. Das folgende Beispiel zeigt, wie Sie instanziieren ein `ActivityIndicator` in XAML mit dem optionalen `IsRunning` Eigenschaftensatz:

```xaml
<ActivityIndicator IsRunning="true" />
```

Ein `ActivityIndicator` können auch im Code erstellt werden:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Eigenschaften des ActivityIndicator-Darstellung

Die `Color` Eigenschaft kann festgelegt werden, zum Definieren der `ActivityIndicator` Farbe. Das folgende Beispiel zeigt, wie Sie instanziieren ein `ActivityIndicator` in XAML mit dem `Color` Eigenschaftensatz:

```xaml
<ActivityIndicator Color="Orange" />
```

Die `Color` Eigenschaft kann auch festgelegt werden, beim Erstellen einer `ActivityIndicator` im Code:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

Der folgende Screenshot zeigt die `ActivityIndicator` mit der `Color` -Eigenschaftensatz auf `Color.Orange` unter iOS und Android:

![Screenshot des formatierten ActivityIndicator unter iOS und Android](activityindicator-images/activityindicators-styled.png "Screenshot des formatierten ActivityIndicator unter iOS und Android")

## <a name="related-links"></a>Verwandte Links

* [ActivityIndicator-Demos](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
