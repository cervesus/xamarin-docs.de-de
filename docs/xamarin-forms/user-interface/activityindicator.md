---
title: Aktivitätsindikator in xamarin. Forms
description: Das Steuerelement activityindicator gibt Benutzern an, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist, ohne dass der Fortschritt angegeben wird. In diesem Artikel wird erläutert, wie ein activityindicator in XAML und Code verwendet wird.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: e13a46e1022f4e33ace6f9f19bb5cea5d1ac784b
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739168"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin. Forms-activityindicator
[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin. Forms[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) ist ein Steuerelement, das eine Animation anzeigt, um anzuzeigen, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist. Anders als [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)der gibt `ActivityIndicator` den Fortschritt an. Der `ActivityIndicator` erbt von [`View`](xref:Xamarin.Forms.View).

Der folgende Screenshot zeigt ein `ActivityIndicator` -Steuerelement unter IOS und Android:

![Screenshot von activityindicator unter IOS und Android](activityindicator-images/activityindicators-default.png "Screenshot von activityindicator unter IOS und Android")

Das `ActivityIndicator` -Steuerelement definiert die folgenden Eigenschaften:

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)ein `bool` Wert, der `ActivityIndicator` angibt, ob sichtbar und animiert werden soll. Wenn der Wert ist `false` , `ActivityIndicator` ist nicht sichtbar.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)ein `Color` -Wert, der die Anzeige Farbe `ActivityIndicator`von definiert.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte gestützt. Dies bedeutet, dass das `ActivityIndicator` formatiert werden kann und das Ziel von Daten Bindungen ist.

## <a name="create-an-activityindicator"></a>Erstellen eines activityindicator

Eine `ActivityIndicator` kann in XAML instanziiert werden. Die `IsRunning` -Eigenschaft kann festgelegt werden, um zu bestimmen, ob das Steuerelement sichtbar und animiert ist. Wenn die `IsRunning` -Eigenschaft nicht festgelegt ist, `false` wird Standard `ActivityIndicator` mäßig auf festgelegt, und der ist nicht sichtbar. Im folgenden Beispiel wird gezeigt, wie ein `ActivityIndicator` in XAML mit dem optionalen `IsRunning` Eigenschaften Satz instanziiert wird:

```xaml
<ActivityIndicator IsRunning="true" />
```

Ein `ActivityIndicator` kann auch im Code erstellt werden:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>Aktivitätsindikator-Darstellungs Eigenschaften

Die `Color` -Eigenschaft kann festgelegt werden, `ActivityIndicator` um die Farbe zu definieren. Im folgenden Beispiel wird gezeigt, wie ein `ActivityIndicator` in XAML mit dem `Color` -Eigenschaften Satz instanziiert wird:

```xaml
<ActivityIndicator Color="Orange" />
```

Die `Color` -Eigenschaft kann auch festgelegt werden, `ActivityIndicator` wenn ein-Objekt im Code erstellt wird:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

Der folgende Screenshot zeigt das `ActivityIndicator` -Objekt `Color` , bei dem `Color.Orange` die-Eigenschaft unter IOS und Android auf festgelegt ist:

![Screenshot des formatierten activityindicator unter IOS und Android](activityindicator-images/activityindicators-styled.png "Screenshot des formatierten activityindicator unter IOS und Android")

## <a name="related-links"></a>Verwandte Links

* [Activityindicator-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
