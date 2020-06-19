---
title: Benutzerdefinierte XAML-Namespace Schemas inXamarin.Forms
description: Ein benutzerdefiniertes XAML-Namespace Schema kann mit der XmlnsDefinitionAttribute-Klasse definiert werden, die eine Zuordnung zwischen einer benutzerdefinierten URL und einem oder mehreren CLR-Namespaces angibt. Das benutzerdefinierte Namespace Schema kann dann in XAML-Namespace Deklarationen verwendet werden.
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 41a95b1a82ab8aa1f6938e5a2bcdebcef368e72d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138163"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Benutzerdefinierte XAML-Namespace Schemas inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)

Auf Typen in einer Bibliothek kann in XAML verwiesen werden, indem ein XAML-Namespace für die Bibliothek deklariert wird, wobei die Namespace Deklaration den CLR-Namespace Namen (Common Language Runtime) und einen Assemblynamen angibt:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly=MyCompany.Controls">
    ...
</ContentPage>
```

Das Angeben eines CLR-Namespace und eines Assemblynamens in einer `xmlns` Definition kann jedoch umständlich und fehleranfällig sein. Außerdem sind möglicherweise mehrere XAML-Namespace Deklarationen erforderlich, wenn die Bibliothek Typen in mehreren Namespaces enthält.

Eine alternative Vorgehensweise besteht darin, ein benutzerdefiniertes Namespace Schema (z. b.) zu definieren, `http://mycompany.com/schemas/controls` das einem oder mehreren CLR-Namespaces zugeordnet ist. Dies ermöglicht es einer einzelnen XAML-Namespace Deklaration, auf alle Typen in einer Assembly zu verweisen, auch wenn Sie sich in unterschiedlichen Namespaces befinden. Außerdem ermöglicht es einer einzelnen XAML-Namespace Deklaration, auf Typen in mehreren Assemblys zu verweisen.

Weitere Informationen zu XAML-Namespaces finden Sie [unter XAML-Namespaces in Xamarin.Forms ](namespaces.md).

## <a name="defining-a-custom-namespace-schema"></a>Definieren eines benutzerdefinierten Namespace Schemas

Die Beispielanwendung enthält eine Bibliothek, die einige einfache Steuerelemente verfügbar macht, z. b. `CircleButton` :

```csharp
using Xamarin.Forms;

namespace MyCompany.Controls
{
    public class CircleButton : Button
    {
        ...
    }
}
```

Alle-Steuerelemente in der Bibliothek befinden sich im- `MyCompany.Controls` Namespace. Diese Steuerelemente können über ein benutzerdefiniertes Namespace Schema für eine aufrufende Assembly verfügbar gemacht werden.

Ein benutzerdefiniertes Namespace Schema wird mit der- `XmlnsDefinitionAttribute` Klasse definiert, die die Zuordnung zwischen einem XAML-Namespace und einem oder mehreren CLR-Namespaces angibt. `XmlnsDefinitionAttribute`Erfordert zwei Argumente: den Namen des XAML-Namespace und den Namen des CLR-Namespace. Der Name des XAML-Namespace wird in der `XmlnsDefinitionAttribute.XmlNamespace` -Eigenschaft gespeichert, und der CLR-Namespace Name wird in der- `XmlnsDefinitionAttribute.ClrNamespace` Eigenschaft gespeichert.

> [!NOTE]
> Die- `XmlnsDefinitionAttribute` Klasse verfügt auch über eine-Eigenschaft mit dem Namen `AssemblyName` , die optional auf den Namen der Assembly festgelegt werden kann. Dies ist nur erforderlich, wenn sich ein CLR-Namespace, auf den von verwiesen `XmlnsDefinitionAttribute` wird, in einer externen Assembly befindet.

`XmlnsDefinitionAttribute`Muss auf Assemblyebene in dem Projekt definiert werden, das die CLR-Namespaces enthält, die im benutzerdefinierten Namespace Schema zugeordnet werden. Das folgende Beispiel zeigt die **AssemblyInfo.cs** -Datei aus der Beispielanwendung:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

Dieser Code erstellt ein benutzerdefiniertes Namespace Schema, das die `http://mycompany.com/schemas/controls` URL dem `MyCompany.Controls` CLR-Namespace zuordnet. Außerdem wird das- `Preserve` Attribut für die Assembly angegeben, um sicherzustellen, dass der Linker alle Typen in der Assembly beibehält.

> [!IMPORTANT]
> Das- `Preserve` Attribut sollte auf Klassen in der Assembly angewendet werden, die über das benutzerdefinierte Namespace Schema zugeordnet werden oder auf die gesamte Assembly angewendet werden.

Das benutzerdefinierte Namespace Schema kann dann für die Typauflösung in XAML-Dateien verwendet werden.

## <a name="consuming-a-custom-namespace-schema"></a>Verwenden eines benutzerdefinierten Namespace Schemas

Um Typen aus dem benutzerdefinierten Namespace Schema zu verwenden, erfordert der XAML-Compiler, dass ein Code Verweis aus der Assembly, die die Typen verwendet, auf die Assembly vorhanden ist, die die Typen definiert. Dies kann durch Hinzufügen einer Klasse, die eine `Init` Methode enthält, zu der Assembly erfolgen, die die Typen definiert, die über XAML verwendet werden:

```csharp
namespace MyCompany.Controls
{
    public static class Controls
    {
        public static void Init()
        {
        }
    }
}
```

Die- `Init` Methode kann dann von der Assembly aufgerufen werden, die Typen aus dem benutzerdefinierten Namespace Schema verwendet:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

namespace CustomNamespaceSchemaDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            Controls.Init();
            InitializeComponent();
        }
    }
}
```

> [!WARNING]
> Wenn ein solcher Code Verweis nicht einbezogen wird, kann der XAML-Compiler die Assembly, die die benutzerdefinierten Namespace Schema Typen enthält, nicht finden.

Um das- `CircleButton` Steuerelement zu verwenden, wird ein XAML-Namespace deklariert, wobei die Namespace Deklaration die Schema-URL des benutzerdefinierten Namespaces angibt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="http://mycompany.com/schemas/controls"
             x:Class="CustomNamespaceSchemaDemo.MainPage">
    <StackLayout Margin="20,35,20,20">
        ...
        <controls:CircleButton Text="+"
                               BackgroundColor="Fuchsia"
                               BorderColor="Black"
                               CircleDiameter="100" />
        <controls:CircleButton Text="-"
                               BackgroundColor="Teal"
                               BorderColor="Silver"
                               CircleDiameter="70" />
        ...
    </StackLayout>
</ContentPage>
```

`CircleButton`-Instanzen können dann hinzugefügt werden, [`ContentPage`](xref:Xamarin.Forms.ContentPage) indem Sie mit dem- `controls` Namespace Präfix deklariert werden.

Um die benutzerdefinierten Namespace-Schema Typen zu finden, sucht referenzierte Assemblys nach- Xamarin.Forms `XmlnsDefinitionAttribute` Instanzen. Wenn das- `xmlns` Attribut für ein Element in einer XAML-Datei mit dem `XmlNamespace` Eigenschafts Wert in einem übereinstimmt `XmlnsDefinitionAttribute` , Xamarin.Forms versucht, den `XmlnsDefinitionAttribute.ClrNamespace` Eigenschafts Wert für die Auflösung des Typs zu verwenden. Wenn die Typauflösung fehlschlägt, Xamarin.Forms versucht weiterhin, die Typauflösung basierend auf allen zusätzlichen übereinstimmenden Instanzen durchzuführen `XmlnsDefinitionAttribute` .

Das Ergebnis ist, dass zwei `CircleButton` Instanzen angezeigt werden:

![Kreis Schaltflächen](custom-namespace-schemas-images/circle-buttons.png "Kreis Schaltflächen")

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Namespace Schemas (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)
- [Empfohlene Präfixe für den XAML-Namespace](custom-prefix.md)
- [XAML-Namespaces inXamarin.Forms](namespaces.md)
