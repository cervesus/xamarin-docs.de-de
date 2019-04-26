---
title: Benutzerdefinierte Namespace-Schemas in Xamarin.Forms XAML
description: Mit der XmlnsDefinitionAttribute-Klasse, die eine Zuordnung zwischen einer benutzerdefinierten URL und einen oder mehrere CLR-Namespaces angibt, kann ein XAML-Schema benutzerdefinierten Namespace definiert werden. Das Schema des benutzerdefinierten Namespace können Sie dann in XAML-Namespace-Deklarationen verwendet werden.
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
ms.openlocfilehash: 2e09e89fe17956efaef910638e827b69a5795bc0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60857480"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Benutzerdefinierte Namespace-Schemas in Xamarin.Forms XAML

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)

Typen in einer Bibliothek können in XAML deklarieren einen XAML-Namespace für die Bibliothek, mit der Namespacedeklaration angeben den Namespacenamen der Common Language Runtime (CLR)-und Namen einer Assembly verwiesen werden:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

Allerdings angeben einen CLR-Namespace und Assembly die Namen in einem `xmlns` Definition kann umständlich sein und fehleranfällig. Darüber hinaus können mehrere XAML-Namespacedeklarationen erforderlich sein, wenn die Bibliothek Typen in mehreren Namespaces enthält.

Ein alternativer Ansatz ist ein benutzerdefinierter Namespace-Schema, z. B. definieren `http://mycompany.com/schemas/controls`, der einem oder mehreren CLR-Namespaces zugeordnet. Dies ermöglicht eine einzelne XAML-Namespacedeklaration auf alle Typen in einer Assembly zu verweisen, selbst wenn sie sich in unterschiedlichen Namespaces befinden. Darüber hinaus können eine einzelne XAML-Namespacedeklaration auf Verweistypen in mehreren Assemblys.

Weitere Informationen zu XAML-Namespaces finden Sie unter [XAML-Namespaces in Xamarin.Forms](namespaces.md).

## <a name="defining-a-custom-namespace-schema"></a>Definieren eines benutzerdefinierten Namespaces-Schemas

Die beispielanwendung enthält eine Bibliothek, die einige einfache Steuerelemente, z. B. macht `CircleButton`:

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

Alle Steuerelemente in der Bibliothek befinden sich in der `MyCompany.Controls` Namespace. Diese Steuerelemente können auf eine aufrufende Assembly über einen benutzerdefinierten Namespace-Schema verfügbar gemacht werden.

Mit einem benutzerdefinierten Namespace-Schema definiert die `XmlnsDefinitionAttribute` -Klasse, die die Zuordnung zwischen einem XAML-Namespace und einem oder mehreren CLR-Namespaces angibt. Die `XmlnsDefinitionAttribute` akzeptiert zwei Argumente: den Namen der XAML-Namespace und Name des CLR-Namespaces. Der Name der XAML-Namespace befindet sich in der `XmlnsDefinitionAttribute.XmlNamespace` -Eigenschaft und den Namen der CLR-Namespace befindet sich in der `XmlnsDefinitionAttribute.ClrNamespace` Eigenschaft.

> [!NOTE]
> Die `XmlnsDefinitionAttribute` -Klasse verfügt auch über eine Eigenschaft namens `AssemblyName`, dem kann optional auf den Namen der Assembly festgelegt werden. Dies ist nur erforderlich, wenn ein CLR-Namespace, der aus auf die verwiesen wird. eine `XmlnsDefinitionAttribute` in einer externen Assembly ist.

Die `XmlnsDefinitionAttribute` sollte definiert werden, auf der Assemblyebene in das Projekt, das den CLR-Namespaces enthält, die im benutzerdefinierten Namespace-Schema zugeordnet werden. Das folgende Beispiel zeigt die **"AssemblyInfo.cs"** Datei aus der beispielanwendung:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

Dieser Code erstellt einen benutzerdefinierten Namespace-Schema, zugeordnet wird, die `http://mycompany.com/schemas/controls` URL für die `MyCompany.Controls` CLR-Namespace. Darüber hinaus die `Preserve` Attribut wird angegeben, für die Assembly, um sicherzustellen, dass der Linker alle Typen in der Assembly wird beibehalten.

> [!IMPORTANT]
> Die `Preserve` Attribut auf Klassen in der Assembly, die über das Schema des benutzerdefinierten Namespace zugeordnet sind, oder angewendet, für die gesamte Assembly angewendet werden soll.

Die benutzerdefinierten Namespace-Schema kann dann für die typauflösung in XAML-Dateien verwendet werden.

## <a name="consuming-a-custom-namespace-schema"></a>Verwenden eines benutzerdefinierten Namespaces-Schemas

Um Typen aus dem Schema des benutzerdefinierten Namespace nutzen zu können, muss der XAML-Compiler an, dass ein Codeverweis aus der Assembly, die die Typen verwendet, auf die Assembly, die die Typen definiert. Dies kann erreicht werden, durch das Hinzufügen einer Klasse mit einer `Init` Methode, um die Assembly, die die Typen definiert, die über XAML verwendet werden:

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

Die `Init` Methode kann dann aufgerufen werden, aus der Assembly, die Typen aus dem benutzerdefinierten Namespace-Schema verwendet:

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
> Solch einen Codeverweis enthalten führt in der XAML-Compiler nicht auf die Assembly mit der benutzerdefinierten Namespace-Schema-Typen zu suchen.

Nutzen der `CircleButton` -Steuerelement ein XAML-Namespace wird deklariert, mit der Namespacedeklaration, die die URL der benutzerdefinierten Namespace-Schema angeben:

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

`CircleButton` Instanzen können dann hinzugefügt werden, um die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) durch Deklarieren sie mit der `controls` Namespacepräfix.

Um den benutzerdefinierten Namespace Schematypen finden, durchsucht Xamarin.Forms referenzierten Assemblys für `XmlnsDefinitionAttribute` Instanzen. Wenn die `xmlns` -Attributs für ein Element in einer XAML-Datei entspricht der `XmlNamespace` Eigenschaftswert in einer `XmlnsDefinitionAttribute`, Xamarin.Forms versucht, verwenden die `XmlnsDefinitionAttribute.ClrNamespace` Eigenschaftswert für die Auflösung des Typs. Wenn typauflösung fehlschlägt, Xamarin.Forms wird weiterhin typauflösung basierend auf alle weiteren entsprechenden versucht `XmlnsDefinitionAttribute` Instanzen.

Das Ergebnis ist, dass zwei `CircleButton` Instanzen werden angezeigt:

![Kreis, Schaltflächen](custom-namespace-schemas-images/circle-buttons.png "Kreis von Schaltflächen")

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Namespace-Schemas (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)
- [Empfohlene Präfixe für XAML-Namespaces](custom-prefix.md)
- [XAML-Namespaces in Xamarin.Forms](namespaces.md)
