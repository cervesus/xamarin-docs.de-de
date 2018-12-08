---
title: Xamarin.Forms-Visual
description: Dieser Artikel enthält die Ansichten identisch oder weitgehend identisch, unter iOS und Android rendert Xamarin.Forms Visualisierung.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
ms.openlocfilehash: df2351e35817a6468fed236752eea8e5ad11024f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061885"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms-Visual

![Vorschau](~/media/shared/preview.png)

_Dieser Artikel enthält die Ansichten identisch oder weitgehend identisch, unter iOS und Android rendert Xamarin.Forms Visualisierung._

Viele Entwickler möchten erstellen Xamarin.Forms-Anwendungen, die Aussehen identische oder weitgehend identisch, in iOS und Android. Xamarin.Forms-4.0-pre1 umfasst einen Mechanismus zum Einschließen von zusätzlicher Renderers, die eine visuelle Darstellung, mit Anwendungen, die Entscheidung für die Darstellung durch Implementieren einer `Visual` Eigenschaft:

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>    
```

Renderer, die die visuelle Darstellung implementieren werden auf Renderer Ansichten statt der Standard-Renderer verwendet. Zum Zeitpunkt der Renderer Auswahl der `Visual` -Eigenschaft der Ansicht werden untersucht und in den Renderer Prozess enthalten. Darüber hinaus, wenn die `Visual` Eigenschaft, die zur Laufzeit ändert, wird der angegebene Renderer wird zusammen mit ihren untergeordneten Elementen neu erstellt.

> [!IMPORTANT]
> Die `Visual` Eigenschaft wird definiert, der `VisualElement` Klasse mit Vererbung Ansichten der `Visual` Eigenschaftswert von ihren übergeordneten Elementen. Daher ist das Festlegen der `Visual` Eigenschaft für eine `ContentPage` wird sichergestellt, dass alle unterstützten Ansichten auf der Seite, visuelle Darstellung verwenden. Darüber hinaus die `Visual` Eigenschaft kann für eine Sicht überschrieben werden.

Xamarin.Forms-4.0-pre1 enthält eine experimentelle visuelle Darstellung, die basierend auf Material Design, mit der Renderer, der als der Material-Renderer bezeichnet wird. Material Renderer sind derzeit enthalten, für die folgenden Ansichten unter iOS und Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)

Funktionell unterscheiden sich die Material-Renderer nicht auf die Standard-Renderer. Allerdings sie derzeit experimentell und kann nur verwendet werden, durch die folgende Zeile des Codes zum Hinzufügen Ihrer `AppDelegate` Klasse unter iOS, oder um Ihre `MainActivity` Klasse, die unter Android werden vor dem Aufruf `Forms.Init`:

```csharp
Forms.SetFlags("Visual_Experimental");
```

Darüber hinaus unter iOS, die das plattformprojekt müssen die [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents/) NuGet-Paket installiert. Unter Android Visual funktioniert nur mit API-29, Ihre plattformprojekt v28 der Unterstützungsbibliotheken verwenden muss, und legen Sie das Design von Komponenten von Material Design zu erben, oder ein Design AppCompat erben, während einige neue Design-Attribute hinzugefügt, um das Design weiterhin. Weitere Informationen finden Sie unter [erste Schritte mit der Material-Komponenten für Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Die folgenden Screenshots zeigen eine Benutzeroberfläche, die die vier Ansichten enthält, für welches, die, die Material Renderer vorhanden, aber mit dem Standard-Renderer gerendert:

[![Standard-Renderer](visual-images/default-renderers.png "Ansichten mit Standard-Renderer")](visual-images/default-renderers-large.png#lightbox)

Die folgenden Screenshots zeigen die gleiche Benutzeroberfläche, die mit der Material-Renderer gerendert:

[![Material Renderer](visual-images/material-renderers.png "Ansichten mit der Material-Renderer")](visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> Klicken Sie mit Material Renderer die gerenderten Steuerelemente bleiben nativen Steuerelementen, und aus diesem Grund wird immer noch vorhanden sein Unterschiede bei der Benutzeroberfläche zwischen den Plattformen für Bereiche wie Schriftarten, Schatten, Farben und Erhöhung der Rechte.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
