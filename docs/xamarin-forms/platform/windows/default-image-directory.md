---
Title: "Standard Image Verzeichnis on Windows" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie das Windows-plattformspezifische verwenden, das das Verzeichnis in dem Projekt definiert, aus dem Image-Assets geladen werden.
ms. Prod: xamarin ms. assetid: 537a032b-74dd-4D43-864e-7d7113286d0d ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 01/16/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="default-image-directory-on-windows"></a>Standard Image Verzeichnis unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch definiert das Verzeichnis im Projekt, aus dem Image Assets geladen werden. Sie wird in XAML verwendet, indem auf ein festgelegt wird `Application.ImageDirectory` `string` , das das Projektverzeichnis darstellt, das Bild Ressourcen enthält:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
             ...
             windows:Application.ImageDirectory="Assets">
    ...
</Application>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
Application.Current.On<Windows>().SetImageDirectory("Assets");
```

Die- `Application.On<Windows>` Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. Die- `Application.SetImageDirectory` Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird verwendet, um das Projektverzeichnis anzugeben, aus dem Bilder geladen werden. Außerdem kann die- `GetImageDirectory` Methode verwendet werden, um ein-Objekt zurückzugeben `string` , das das Projektverzeichnis darstellt, das die Anwendungs Image Ressourcen enthält.

Das Ergebnis ist, dass alle in einer Anwendung verwendeten Bilder aus dem angegebenen Projektverzeichnis geladen werden.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
