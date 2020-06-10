---
Title: "Aktivitätsindikator in Xamarin.Forms " Beschreibung: "das Steuerelement" activityindicator "gibt Benutzern an, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist, ohne dass ein Status angegeben wird. In diesem Artikel wird erläutert, wie ein activityindicator in XAML und Code verwendet wird. "
ms. Prod: xamarin ms. assetid: 4ceed02d-5ca3-4 C3a-b7ed-3193fc272261 ms. Technology: xamarin-Forms Author: profexorgeek ms. Author: jusjohns ms. Date: 07/10/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
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
