---
title: Teil 3. Verwendung von XAML-Markuperweiterungen
description: Verwendung von XAML-Markuperweiterungen bilden eine wichtige Funktion in XAML, die ermöglichen, Eigenschaften festgelegt werden, um Objekte oder Werte, die aus anderen Quellen indirekt verwiesen werden. Verwendung von XAML-Markuperweiterungen sind besonders wichtig für das Freigeben von Objekten, und verweisen auf die Konstanten, die in einer Anwendung verwendet, aber sie finden ihre größte Hilfsprogramm im datenbindungen.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: c110223eae2bb06f64adf3e09977d97cc7b5d71b
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="part-3-xaml-markup-extensions"></a>Teil 3. Verwendung von XAML-Markuperweiterungen

_Verwendung von XAML-Markuperweiterungen bilden eine wichtige Funktion in XAML, die ermöglichen, Eigenschaften festgelegt werden, um Objekte oder Werte, die aus anderen Quellen indirekt verwiesen werden. Verwendung von XAML-Markuperweiterungen sind besonders wichtig für das Freigeben von Objekten, und verweisen auf die Konstanten, die in einer Anwendung verwendet, aber sie finden ihre größte Hilfsprogramm im datenbindungen._

## <a name="xaml-markup-extensions"></a>Verwendung von XAML-Markuperweiterungen

Im Allgemeinen verwenden Sie XAML-Code zum Festlegen der Eigenschaften eines Objekts auf explizite Werte, z. B. eine Zeichenfolge, eine Zahl, einen Enumerationsmember oder eine Zeichenfolge, die auf einen Wert im Hintergrund konvertiert wird.

In einigen Fällen jedoch Eigenschaften müssen stattdessen else an einer beliebigen Stelle definierten Werte verweisen, oder das möglicherweise eine wenig Verarbeitung erforderlich ist, durch Code zur Laufzeit. Für diese Zwecke wird XAML *Markuperweiterungen* verfügbar sind.

Diese Verwendung von XAML-Markuperweiterungen sind nicht von XML-Erweiterungen. XAML ist vollständig gültige XML-Daten. Sie sind "Extensions" aufgerufen, da sie von Code in Klassen unterstützt werden, die implementieren `IMarkupExtension`. Sie können Ihre eigenen benutzerdefinierten Markuperweiterungen schreiben.

In vielen Fällen XAML-Markuperweiterungen sind sofort erkennbar, die in XAML-Dateien, da sie als attributeinstellungen, die als Trennzeichen in geschweiften Klammern angezeigt werden: {und}, aber manchmal Markuperweiterungen werden in Markup als herkömmliche Elemente angezeigt.

## <a name="shared-resources"></a>Freigegebene Ressourcen

Einige XAML-Seiten enthalten mehrere Ansichten mit Eigenschaften, die auf die gleichen Werte festgelegt sind. Z. B. viele der eigenschafteneinstellungen für diese `Button` -Objekte identisch sind:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

Wenn eine dieser Eigenschaften geändert werden muss, empfiehlt es sich, die Änderung nur einmal und nicht als dreimal stellen. Wenn dies Code wäre, würde wahrscheinlich Konstanten und statische schreibgeschützte Objekte verwenden Sie sorgen Sie solche Werte konsistent und leicht zu ändern.

In XAML wird eine beliebte Lösung besteht darin, diese Werte zu speichern oder Objekte in einer *Ressourcenverzeichnis*. Die `VisualElement` Klasse definiert eine Eigenschaft namens `Resources` des Typs `ResourceDictionary`, also ein Wörterbuch mit Schlüssel des Typs `string` und Werten des Typs `object`. Sie können Objekte in dieses Wörterbuch aufweist und versetzt und verweisen dann alle im XAML-Markup.

Um ein Ressourcenverzeichnis auf einer Seite verwenden, schließen Sie ein Paar von `Resources` Property-Element Tags. Es ist am einfachsten, diese am oberen Rand der Seite eingefügt werden soll:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

Es ist auch erforderlich, explizit `ResourceDictionary` Tags:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Objekte und Werte verschiedener Typen können jetzt auf das Ressourcenwörterbuch hinzugefügt werden. Diese Typen müssen instanziierbaren sein. Abstrakte Klassen enthalten, z. B. nicht möglich. Diese Typen müssen auch einen öffentlichen parameterlosen Konstruktor aufweisen. Jedes Element erfordert eine Wörterbuchschlüssel angegeben, mit der `x:Key` Attribut. Zum Beispiel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Diese beiden Elemente sind die Werte des Strukturtyps `LayoutOptions`, und jede verfügt über einen eindeutigen Schlüssel und ein oder zwei Eigenschaften festzulegen. In Code und Markup, ist es sehr viel häufiger, verwenden Sie die statischen Felder der `LayoutOptions`, jedoch hier ist es sinnvoller sein, um die Eigenschaften festzulegen.

Jetzt festgelegt werden muss die `HorizontalOptions` und `VerticalOptions` Eigenschaften dieser fünf Schaltflächen auf diese Ressourcen und erfolgt, die mit der `StaticResource` XAML-Markuperweiterung:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

Die `StaticResource` Markuperweiterung immer mit geschweiften Klammern begrenzt wird, und der Wörterbuchschlüssel enthält.

Der Name `StaticResource` unterscheidet es sich von `DynamicResource`, die Xamarin.Forms ebenfalls unterstützt. `DynamicResource` ist für die Wörterbuchschlüssel zugeordneten Werte, die während der Laufzeit ändern können während `StaticResource` Elemente aus dem Wörterbuch zugreift, nur einmal bei die Elemente auf der Seite "erstellt werden.

Für die `BorderWidth` -Eigenschaft, es ist notwendig, einen Double-Wert im Wörterbuch zu speichern. XAML-Code definiert bequem Tags für häufig verwendete Datentypen wie `x:Double` und `x:Int32`:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Sie müssen es auf drei Zeilen zu speichern. Diesen Wörterbucheintrag für diese Drehwinkel für Bezeichnungen akzeptiert nur eine Zeile nach oben:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Diese beiden Ressourcen verwiesen werden können, auf die gleiche Weise wie die `LayoutOptions` Werte:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Für Ressourcen vom Typ `Color`, können Sie die gleichen zeichenfolgendarstellungen, die Sie verwenden, wenn Sie Attribute dieser Typen direkt zuweisen. Der Typkonverter werden aufgerufen, wenn die Ressource erstellt wird. Hier ist eine Ressource vom Typ `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Häufig Programme Satz ein `FontSize` Eigenschaft an ein Mitglied der `NamedSize` Enumeration wie z. B. `Large`. Die `FontSizeConverter` -Klasse arbeitet im Hintergrund in einer mithilfe von plattformabhängigen Wert konvertieren die `Device.GetNamedSized` Methode. Jedoch wenn Sie eine Schriftgröße Ressource definieren, es ist sinnvoller mit einem numerischen Wert dargestellt hier als ein `x:Double` Typ:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Jetzt alle Eigenschaften außer `Text` werden durch die Einstellungen definiert:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Es ist auch möglich, verwenden Sie `OnPlatform` in das Ressourcenwörterbuch, um unterschiedliche Werte für die Plattformen zu definieren. So sieht wie ein `OnPlatform` Objekt kann Teil der Ressourcenverzeichnis für verschiedene Textfarben sein:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Beachten Sie, dass `OnPlatform` ruft sowohl eine `x:Key` Attribut, da es sich um ein Objekt im Wörterbuch vorhanden ist und ein `x:TypeArguments` Attribut, da es sich um eine generische Klasse handelt. Die `iOS`, `Android`, und `UWP` Attribute werden in konvertiert `Color` Werten, wenn das Objekt initialisiert wird.

So sieht die letzte vollständige XAML-Datei mit drei Schaltflächen, die Zugriff auf sechs freigegebenen Werte aus:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:String x:Key="fontSize">Large</x:String>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

Die Screenshots überprüfen, einheitliches Aussehen und die Formatvorlage plattformabhängige:

[![](xaml-markup-extensions-images/sharedresources.png "Gestalteten Steuerelementen")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "gestalteten Steuerelementen")

Zwar am häufigsten verwendeten definieren die `Resources` Auflistung am oberen Rand der Seite ", sollten Sie bedenken, die `Resources` Eigenschaft wird definiert, indem `VisualElement`, und Sie können `Resources` Auflistungen in andere Elemente auf der Seite. Versuchen Sie es z. B. mit dem Hinzufügen der `StackLayout` in diesem Beispiel:

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

Sie werden feststellen, dass die Textfarbe der Schaltflächen jetzt Blau ist. Im Grunde bei jedem der Verwendung von XAML-Parser erkennt eine `StaticResource` Markuperweiterung, er durchsucht die visuelle Struktur und verwendet die erste `ResourceDictionary` gefunden wird, die diesen Schlüssel enthält.

Eine der am häufigsten verwendeten Arten von Objekten im Ressourcenverzeichnis gespeichert ist, dass die Xamarin.Forms `Style`, die eine Auflistung von Eigenschaften definiert. Stile werden im Artikel erläutert [Stile](~/xamarin-forms/user-interface/styles/index.md).

Entwickler mit Erfahrung mit XAML in einigen Fällen zu Fragen, wenn sie z. B. ein visuelles Element einfügen können `Label` oder `Button` in einem `ResourceDictionary`. Während es sicherlich möglich ist, ist nicht sinnvoll erleichtern. Der Zweck der `ResourceDictionary` besteht darin, Objekte freizugeben. Ein visuelles Element kann nicht freigegeben werden. Die gleiche Instanz kann nicht zweimal auf einer einzelnen Seite angezeigt werden.

## <a name="the-xstatic-markup-extension"></a>X: Static-Markuperweiterung

Trotz der Ähnlichkeit der entsprechenden Namen `x:Static` und `StaticResource` sind sehr unterschiedlich. `StaticResource` Gibt ein Objekt aus einem Ressourcenwörterbuch beim `x:Static` greift auf einen der folgenden:

- eine öffentliche statische Feld
- eine öffentliche statische Eigenschaft
- eine öffentliche konstantes Feld 
- einen Enumerationsmember. 

Die `StaticResource` Markuperweiterung wird vom XAML-Implementierungen, die ein Ressourcenverzeichnis definieren unterstützt während `x:Static` ist ein integraler Bestandteil von XAML-Code als das `x` Präfix ergibt.

Hier sind einige Beispiele, die veranschaulichen, wie `x:Static` können explizit statische Felder und Enumerationsmember verweisen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Dies ist bisher nicht sehr beeindruckend. Aber die `x:Static` Markuperweiterung kann auch statische Felder oder Eigenschaften aus Ihren eigenen Code verweisen. Hier ist z. B. eine `AppConstants` -Klasse, die einige statische Felder enthält, die Sie möglicherweise auf mehreren Seiten in einer Anwendung verwenden möchten:

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;
                    
                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

Um die statischen Felder dieser Klasse in der XAML-Datei verweisen zu können, benötigen Sie in der XAML-Datei anzugeben, wo sich diese Datei befindet. Dazu eine XML-Namespacedeklaration.

Beachten Sie, dass die XAML-Dateien, die als Teil der Verwendung von XAML-Xamarin.Forms-Standardvorlage erstellt zwei XML-Namespacedeklaration enthalten: einen für den Zugriff auf Xamarin.Forms-Klassen und die andere zum Verweisen auf Tags und Attribute, die für XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Sie benötigen zusätzliche XML-Namespacedeklaration auf andere Klassen zugreifen. Jede zusätzliche XML-Namespacedeklaration definiert ein neues Präfix. Klassen, die lokal auf die freigegebene Anwendung .NET Standardbibliothek, z. B. den Zugriff auf `AppConstants`, XAML-Programmierer verwenden häufig das Präfix `local`. Die Namespacedeklaration muss die CLR (Common Language Runtime)-Namespacenamen, auch bekannt als die .NET Namespacenamen, der der Name ist, die in einem C#-angezeigt angeben `namespace` Definition oder in einem `using` Richtlinie:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Sie können auch XML-Namespacedeklarationen für .NET-Namespaces definieren, die in jeder beliebigen Assembly, die den standardmäßigen .NET Bibliothek verweist. Hier ist z. B. eine `sys` Präfix für den standardmäßigen .NET `System` -Namespace, der in der **"mscorlib"** -Assembly, die "Microsoft Common Runtime Objektbibliothek" einmal hatten, aber jetzt bedeutet "mehrsprachigen Standard Common Runtime Objektbibliothek." Da dies eine andere Assembly ist, müssen Sie auch angeben der Name der Assembly, in diesem Fall **"mscorlib"**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Beachten Sie, dass das Schlüsselwort `clr-namespace` wird gefolgt von einem Doppelpunkt und dann den Namespacenamen .NET, gefolgt von einem Semikolon, das Schlüsselwort `assembly`, einem Gleichheitszeichen und der Name der Assembly.

Ja, ein Doppelpunkt folgt `clr-namespace` Gleichheitszeichen folgt aber `assembly`. Die Syntax wurde in dieser Weise absichtlich definiert: die meisten XML-Namespacedeklaration verweisen auf ein URI, der einen URI-Schema-Namen wie z. B. beginnt `http`, die immer durch einen Doppelpunkt gefolgt wird. Die `clr-namespace` Teil dieser Zeichenfolge zu imitieren, die diese Konvention dient.

Sowohl diese Namespacedeklarationen befinden sich die **StaticConstantsPage** Beispiel. Beachten Sie, dass die `BoxView` Dimensionen werden festgelegt, um `Math.PI` und `Math.E`, jedoch mit dem Faktor 100 skalierte:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

Die Größe der resultierenden `BoxView` relativ zu dem Bildschirm ist plattformabhängig:

 [![](xaml-markup-extensions-images/staticconstants.png "Steuerelemente, die mithilfe von X: statische Markuperweiterung")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "Steuerelementen mithilfe von X: Static-Markuperweiterung")

## <a name="other-standard-markup-extensions"></a>Andere Standard Markuperweiterungen

Verschiedene Markuperweiterungen werden XAML-systeminterne und Xamarin.Forms XAML-Dateien unterstützt. Einige davon sind nicht sehr häufig verwendet, jedoch sind erforderlich, wenn Sie Sie benötigen:

-  Wenn eine Eigenschaft verfügt über einen nicht- `null` möchten, dass der Wert von Default, aber Sie festgelegt ist, dass `null`, legen Sie sie auf die `{x:Null}` Markuperweiterung.
-  Wenn eine Eigenschaft vom Typ `Type`, weisen Sie sie einer `Type` -Objekt unter Verwendung der Markuperweiterung `{x:Type someClass}`.
-  Definieren Sie Arrays in XAML mit dem `x:Array` Markuperweiterung. Dieser Markuperweiterung hat ein erforderliches Attribut mit dem Namen `Type` , der den Typ der Elemente im Array angibt.
- Die `Binding` Markuperweiterung finden [Teil 4. Grundlagen für die Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Die ConstraintExpression-Markuperweiterung

Markuperweiterungen können verfügen über Eigenschaften, aber sie sind nicht festgelegt, wie XML-Attribute. In einer Markuperweiterung eigenschafteneinstellungen werden durch Kommas getrennt, und keine Anführungszeichen in der geschweiften Klammern angezeigt werden.

Dies kann mit der Xamarin.Forms-Markuperweiterung, die mit dem Namen dargestellt werden `ConstraintExpression`, der verwendet wird, mit der `RelativeLayout` Klasse. Sie können die Position und Größe der eine untergeordnete Ansicht als Konstante oder relativ zu einem übergeordneten Element oder andere benannte Sicht angeben. Die Syntax der `ConstraintExpression` ermöglicht Ihnen das Festlegen der Position oder die Größe einer Ansicht mit einer `Factor` wie oft eine Eigenschaft von einer anderen Ansicht zusammen mit einer `Constant`. Etwas komplizierter als das erfordert Code.

Im Folgenden ein Beispiel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

Die wichtigste Lektion, Sie von diesem Beispiel ergreifen, ist vielleicht die Syntax der Markuperweiterung: keine Anführungszeichen müssen in der geschweiften Klammern eine Markuperweiterung angezeigt werden. Bei der Eingabe der Markuperweiterung in XAML-Datei ist es üblich, um die Werte der Eigenschaften in Anführungszeichen gesetzt werden soll. Widerstehen der Versuchung!

Hier wird das Programm ausgeführt wird:

[![](xaml-markup-extensions-images/relativelayout.png "Relative Layouts mit den Einschränkungen")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "Relative Layout mithilfe von Einschränkungen")

## <a name="summary"></a>Zusammenfassung

Die hier gezeigten XAML-Markuperweiterungen bieten wichtige Unterstützung für XAML-Dateien. Sie jedoch vielleicht die wertvollste XAML-Markuperweiterung `Binding`, wird die in den nächsten Teil dieser Serie behandelt [Teil 4. Grundlagen für die Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus dem Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
