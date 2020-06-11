---
Title: "DatePicker Item Selection on IOS" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, wann die Elementauswahl in einem DatePicker auftritt.
ms. Prod: xamarin ms. assetid: 847e69d1-6ae0-4e82-b9c8-919e009c2014 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 01/15/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="datepicker-item-selection-on-ios"></a>DatePicker-Elementauswahl unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifischen Steuerelemente, wenn die Elementauswahl in einem Auftritt [`DatePicker`](xref:Xamarin.Forms.DatePicker) , sodass der Benutzer angeben kann, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement oder erst nach dem Drücken der Schaltfläche **done** erfolgt. Sie wird in XAML verwendet, indem die `DatePicker.UpdateMode` angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird `UpdateMode` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <DatePicker MinimumDate="01/01/2020"
                   MaximumDate="12/31/2020"
                   ios:DatePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die- `DatePicker.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `DatePicker.SetUpdateMode` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, wann die Elementauswahl stattfindet, wobei die- `UpdateMode` Enumeration zwei mögliche Werte bereitstellt:

- `Immediately`die – Item-Auswahl tritt auf, wenn der Benutzer Elemente in der durchsucht [`DatePicker`](xref:Xamarin.Forms.DatePicker) . Dies ist das Standardverhalten in Xamarin.Forms .
- `WhenFinished`die Auswahl von – Items erfolgt nur, wenn der Benutzer die Schaltfläche **done** in der gedrückt hat [`DatePicker`](xref:Xamarin.Forms.DatePicker) .

Außerdem kann die- `SetUpdateMode` Methode zum Umschalten der Enumerationswerte verwendet werden, indem die- `UpdateMode` Methode aufgerufen wird, die den aktuellen zurückgibt `UpdateMode` :

```csharp
switch (datePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Das Ergebnis ist, dass eine angegebene `UpdateMode` auf den angewendet wird [`DatePicker`](xref:Xamarin.Forms.DatePicker) , der steuert, wann die Elementauswahl erfolgt:

[![Screenshot der DatePicker-Aktualisierungs Modi](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode plattformspezifisch")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
