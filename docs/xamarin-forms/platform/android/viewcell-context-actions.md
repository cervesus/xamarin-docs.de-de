---
title: Viewcell-Kontext Aktionen unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Modell verwenden, das viewcell-Kontext Aktionen im Legacy Modus ermöglicht.
ms.prod: xamarin
ms.assetid: 8BD71B10-5037-443F-9975-B941CE393E5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2019
ms.openlocfilehash: b040c7c5b7f132b0832469efba91dd89d6b2ddbd
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697746"
---
# <a name="viewcell-context-actions-on-android"></a>Viewcell-Kontext Aktionen unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Wenn eine [`ViewCell`](xref:Xamarin.Forms.ViewCell) in einer Android-Anwendung Kontext Aktionen für jedes Element in einem [`ListView`](xref:Xamarin.Forms.ListView)definiert, wird Standard 4,3 mäßig das Menü Kontext Aktionen aktualisiert, wenn sich das ausgewählte Element im `ListView` ändert. In früheren Versionen von xamarin. Forms wurde das Kontext Aktionsmenü jedoch nicht aktualisiert, und dieses Verhalten wird als `ViewCell` Legacy Modus bezeichnet. Dieser Legacy Modus kann zu einem falschen Verhalten führen, wenn ein `ListView` ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) verwendet, um seine `ItemTemplate` von [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekten festzulegen, die unterschiedliche Kontext Aktionen definieren.

Diese Android-plattformspezifische Aktivierung ermöglicht den [`ViewCell`](xref:Xamarin.Forms.ViewCell) Kontext Aktionen-Menü Legacy Modus, aus Gründen der Abwärtskompatibilität, sodass das Kontext Aktionsmenü nicht aktualisiert wird, wenn sich das ausgewählte Element in einer [`ListView`](xref:Xamarin.Forms.ListView) ändert. Sie wird in XAML verwendet, indem die `ViewCell.IsContextActionsLegacyModeEnabled` bindbare Eigenschaft auf `true` festgelegt wird:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding Items}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell android:ViewCell.IsContextActionsLegacyModeEnabled="true">
                        <ViewCell.ContextActions>
                            <MenuItem Text="{Binding Item1Text}" />
                            <MenuItem Text="{Binding Item2Text}" />
                        </ViewCell.ContextActions>
                        <Label Text="{Binding Text}" />
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie von C# der Verwendung der flüssigen API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

Die `ViewCell.On<Android>`-Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die `ViewCell.SetIsContextActionsLegacyModeEnabled`-Methode im [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um den [`ViewCell`](xref:Xamarin.Forms.ViewCell) Kontext Aktionen-Menü Legacy Modus zu aktivieren, sodass das Kontextmenü Menü nicht aktualisiert wird, wenn das ausgewählte Element in einem [`ListView`](xref:Xamarin.Forms.ListView) geändert wird. Darüber hinaus kann die `ViewCell.GetIsContextActionsLegacyModeEnabled`-Methode verwendet werden, um zurückzugeben, ob der Legacy Modus für Kontext Aktionen aktiviert ist.

Die folgenden Screenshots zeigen [`ViewCell`](xref:Xamarin.Forms.ViewCell) Kontext Aktionen im Legacy Modus aktiviert:

![Screenshot des Legacy Modus "viewcell" unter Android](viewcell-context-actions-images/legacy-mode-enabled.png "Viewcell-Legacy Modus aktiviert")

In diesem Modus sind die angezeigten Kontext Aktionsmenü Elemente für Zelle 1 und Zelle 2 identisch, obwohl verschiedene Kontextmenü Elemente für Zelle 2 definiert sind.

Die folgenden Screenshots zeigen [`ViewCell`](xref:Xamarin.Forms.ViewCell) Kontext Aktionen, die im Legacy Modus deaktiviert sind. Dies ist das Standardverhalten von xamarin. Forms:

![Screenshot des Legacy Modus "viewcell" deaktiviert unter Android](viewcell-context-actions-images/legacy-mode-disabled.png "Viewcell-Legacy Modus deaktiviert")

In diesem Modus werden die richtigen Kontext Aktionsmenü Elemente für Zelle 1 und Zelle 2 angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
