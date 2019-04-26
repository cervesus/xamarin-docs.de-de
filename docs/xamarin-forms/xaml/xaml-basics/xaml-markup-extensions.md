---
title: Teil 3. XAML-Markuperweiterungen
description: XAML-Markuperweiterungen bilden eine wichtige Funktion in XAML, mit denen Eigenschaften festgelegt werden, um Objekte oder Werte, die aus anderen Quellen indirekt verwiesen werden.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 03/27/2018
ms.openlocfilehash: 17ca8ec481b8af5ad0515e6544613864f0a66271
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342194"
---
# <a name="part-3-xaml-markup-extensions"></a>Teil 3. XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_XAML-Markuperweiterungen bilden eine wichtige Funktion in XAML, mit denen Eigenschaften festgelegt werden, um Objekte oder Werte, die aus anderen Quellen indirekt verwiesen werden. XAML-Markuperweiterungen sind besonders wichtig für das Freigeben von Objekten und verweisen auf Konstanten, die in der gesamten Anwendung verwendet, aber sie finden ihre größten Dienstprogramm datenbindungen._

## <a name="xaml-markup-extensions"></a>XAML-Markuperweiterungen

Im Allgemeinen verwenden Sie XAML zum Festlegen der Eigenschaften eines Objekts auf explizite Werte, z. B. eine Zeichenfolge, eine Zahl, einen Enumerationsmember oder eine Zeichenfolge, die auf einen Wert im Hintergrund konvertiert wird.

In einigen Fällen jedoch Eigenschaften müssen stattdessen auf Werte, die andernfalls an einer beliebigen Stelle definiert, oder die möglicherweise einer kleine Verarbeitung von Code zur Laufzeit. Für diese Zwecke wird XAML *Markuperweiterungen* stehen zur Verfügung.

Diese XAML-Markuperweiterungen sind nicht von XML-Erweiterungen. XAML ist vollständig gültige XML-Daten. Sind sie "Extensions" aufgerufen, da sie von Code in Klassen unterstützt werden, die implementiert `IMarkupExtension`. Sie können Ihre eigenen benutzerdefinierten Markuperweiterungen schreiben.

In vielen Fällen XAML-Markuperweiterungen sind sofort erkennbar, die in XAML-Dateien, da sie als attributeinstellungen, die geschweiften Klammern gesetzte angezeigt werden: {und}, aber manchmal Markuperweiterungen werden im Markup als herkömmliche Elemente angezeigt.

## <a name="shared-resources"></a>Gemeinsam genutzte Ressourcen

Einige XAML-Seiten enthalten mehrere Ansichten mit Eigenschaften, die auf die gleichen Werte festgelegt. Z. B. einige der eigenschaftseinstellungen für diese `Button` -Objekte identisch sind:

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

Wenn eine dieser Eigenschaften geändert werden muss, empfiehlt es sich um die Änderung nur einmal statt drei Mal zu machen. Würde dies Code, würden Sie wahrscheinlich Konstanten und statische schreibgeschützte Objekte verwenden, sorgen Sie für solche Werte konsistenten und leicht zu ändern.

In XAML, eine gängige Lösung besteht darin, diese Werte zu speichern, oder Objekte in einem *Ressourcenverzeichnis*. Die `VisualElement` -Klasse definiert eine Eigenschaft namens `Resources` des Typs `ResourceDictionary`, dies ist ein Wörterbuch mit Schlüssel vom Typ `string` und Werte des Typs `object`. Können Sie Objekte in dieses Wörterbuch einfügen und aus allen im XAML-Markup darauf verweisen.

Um ein Ressourcenverzeichnis auf einer Seite zu verwenden, schließen Sie ein Paar von `Resources` Eigenschaftenelement Tags. Es ist am einfachsten, diese am oberen Rand der Seite eingefügt:

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

Objekte und Werte verschiedener Typen können nun in das Ressourcenverzeichnis hinzugefügt werden. Diese Typen müssen instanziiert werden können. Abstrakte Klassen, z. B. nicht möglich. Diese Typen müssen auch einen öffentlichen parameterlosen Konstruktor aufweisen. Jedes Element muss einen Dictionary-Schlüssel angegeben, mit der `x:Key` Attribut. Zum Beispiel:

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

Diese beiden Elemente sind die Werte des Strukturtyps `LayoutOptions`, und jede verfügt über einen eindeutigen Schlüssel und ein oder zwei Eigenschaften festgelegt. In Code und Markup, ist es viel häufiger Verwendung die statischen Felder der `LayoutOptions`, aber hier ist es praktischer, um die Eigenschaften festzulegen.

Nun ist es erforderlich, um festzulegen der `HorizontalOptions` und `VerticalOptions` Eigenschaften dieser Schaltflächen auf diese Ressourcen und erfolgt, die mit der `StaticResource` XAML-Markuperweiterung:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

Die `StaticResource` Markuperweiterung ist immer mit geschweiften Klammern getrennt sind, sowie der Wörterbuchschlüssel.

Der Name `StaticResource` unterscheidet es sich von `DynamicResource`, die Xamarin.Forms ebenfalls unterstützt. `DynamicResource` ist für das Wörterbuchschlüssel zugeordneten Werte, die während der Laufzeit ändern können zwar `StaticResource` auf Elemente aus dem Wörterbuch zugegriffen wird, nur einmal bei der Elemente auf der Seite erstellt werden.

Für die `BorderWidth` -Eigenschaft, es ist notwendig, einen Double-Wert im Wörterbuch zu speichern. XAML definiert einfach Tags für häufig verwendete Datentypen wie `x:Double` und `x:Int32`:

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

Sie müssen sie sich in drei Zeilen eingefügt. Diesen Wörterbucheintrag für diese Drehwinkel für Bezeichnungen dauert nur eine Zeile nach oben:

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

Häufig Programme Satz einer `FontSize` Eigenschaft auf einen Member der `NamedSize` Enumeration wie z. B. `Large`. Die `FontSizeConverter` -Klasse arbeitet im Hintergrund in eine plattformabhängige Ganzzahl konvertieren die `Device.GetNamedSized` Methode. Allerdings beim Definieren einer Ressource Font-Size ist es sinnvoller, verwenden Sie einen numerischen Wert, der angezeigt wie ein `x:Double` Typ:

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

Es ist auch möglich mit `OnPlatform` im Ressourcenverzeichnis unterschiedliche Werte für die Plattformen zu definieren. So sieht wie ein `OnPlatform` Objekt kann Teil des Ressourcenverzeichnisses für verschiedene Textfarben sein:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Beachten Sie, dass `OnPlatform` ruft sowohl eine `x:Key` Attribut, da es sich um ein Objekt aus dem Wörterbuch handelt und eine `x:TypeArguments` Attribut, da es sich um eine generische Klasse handelt. Die `iOS`, `Android`, und `UWP` Attribute werden in konvertiert `Color` Werten, wenn das Objekt initialisiert wird.

So sieht die letzte vollständige XAML-Datei mit drei Schaltflächen, die Zugriff auf sechs gemeinsame Werte aus:

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

            <x:Double x:Key="fontSize">24</x:Double>
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

Die Screenshots überprüfen Sie die einheitliches Aussehen und den Stil der plattformabhängige:

[![](xaml-markup-extensions-images/sharedresources.png "Gestalteten Steuerelementen")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "gestalteten Steuerelementen")

Zwar am häufigsten verwendeten definieren die `Resources` Auflistung am oberen Rand der Seite sollten Sie bedenken, die die `Resources` Eigenschaft wird definiert, indem Sie `VisualElement`, und Sie können `Resources` Sammlungen von anderen Elementen auf der Seite. Versuchen Sie es z. B. mit dem Hinzufügen der `StackLayout` in diesem Beispiel:

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

Sie werden feststellen, dass die Textfarbe für die Schaltflächen nun blau ist. Im Grunde jedes Mal, wenn der XAML-Parser erkennt eine `StaticResource` Markuperweiterung, es die visuelle Struktur durchsucht und verwendet die ersten `ResourceDictionary` Schlüssel gefunden.

Einer der am häufigsten verwendeten Typen von Objekten, die im Ressourcenverzeichnis gespeichert ist, dass die Xamarin.Forms `Style`, das eine Auflistung von eigenschafteneinstellungen definiert. Stile werden in diesem Artikel erläuterten [Stile](~/xamarin-forms/user-interface/styles/index.md).

Entwickler, die noch nicht mit XAML zuweilen zu Fragen, wenn sie z. B. ein visuelles Element ablegen können `Label` oder `Button` in einem `ResourceDictionary`. Während es sicherlich möglich ist, macht es viel Sinn. Der Zweck der `ResourceDictionary` Objekte gemeinsam genutzt werden. Ein visuelles Element kann nicht freigegeben werden. Die gleiche Instanz kann nicht zweimal auf einer einzelnen Seite angezeigt.

## <a name="the-xstatic-markup-extension"></a>Die X: Static-Markuperweiterung

Trotz der Ähnlichkeit von ihren Namen `x:Static` und `StaticResource` sind sehr verschieden. `StaticResource` Gibt ein Objekt zurück, aus einem Ressourcenverzeichnis beim `x:Static` greift auf eine der folgenden:

- öffentliches statisches Feld
- eine öffentliche statische Eigenschaft
- Ein konstanter öffentliches Feld
- Ein Enumerationsmember.

Die `StaticResource` Markuperweiterung wird vom XAML-Implementierungen, die einem Ressourcenverzeichnis definiert unterstützt während `x:Static` ist ein integraler Bestandteil von XAML, wie die `x` Präfix anzuzeigen.

Hier sind einige Beispiele, die veranschaulichen, wie `x:Static` können explizit statische Felder und Enumerationsmembern verweisen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Dies ist bisher nicht sehr beeindruckend. Aber die `x:Static` Markuperweiterung kann auch statische Felder oder Eigenschaften aus Ihrem eigenen Code verweisen. Hier ist z. B. eine `AppConstants` -Klasse, die einige statische Felder enthält, die Sie möglicherweise auf mehreren Seiten in der gesamten Anwendung verwenden möchten:

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

Die statischen Felder dieser Klasse in der XAML-Datei verweisen möchten, benötigen Sie eine Möglichkeit, in der XAML-Datei anzugeben, wo sich diese Datei befindet. Hierzu eine XML-Namespacedeklaration.

Denken Sie daran, dass die XAML-Dateien, die als Teil der standardmäßigen Xamarin.Forms-XAML-Vorlage erstellt zwei XML-Namespacedeklaration enthalten: einen für den Zugriff auf Xamarin.Forms-Klassen und eine andere zum Verweisen auf die Tags und Attribute von XAML systeminterne:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Sie benötigen zusätzliche XML-Namespacedeklaration auf andere Klassen zugreifen zu können. Jede zusätzliche XML-Namespacedeklaration definiert ein neues Präfix. Klassen, die lokal auf dem freigegebenen Anwendung .NET Standard-Bibliothek, z. B. den Zugriff auf `AppConstants`, XAML-Programmierer verwenden häufig das Präfix `local`. Die Namespacedeklaration muss die CLR (Common Language Runtime)-Namespacenamen, auch bekannt als die .NET Namespacenamen, der den Namen, die in einer C# -Code angezeigt wird, werden angeben `namespace` Definition oder in einem `using` Richtlinie:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Sie können auch XML-Namespacedeklaration für .NET Namespaces definieren, die in einer beliebigen Assembly, die die .NET Standard-Bibliothek verweist. Hier ist z. B. eine `sys` Präfix für die .NET standard `System` -Namespace, der in der **"mscorlib"** -Assembly, die einmal für die "Microsoft Common Runtime-Objektbibliothek" erstellt, aber jetzt bedeutet "Multilanguage Standard Allgemeine Objekt Runtime Library". Da es sich um eine andere Assembly handelt, müssen Sie auch angeben den Assemblynamen an, in diesem Fall **"mscorlib"**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Beachten Sie, dass das Schlüsselwort `clr-namespace` ist, gefolgt von einem Doppelpunkt und dann den Namespacenamen .NET, gefolgt von einem Semikolon, das Schlüsselwort `assembly`, einem Gleichheitszeichen und der Name der Assembly.

Ja, ein Doppelpunkt folgt `clr-namespace` Gleichheitszeichen folgt `assembly`. Die Syntax wurde in dieser Weise absichtlich definiert: Die meisten XML-Namespacedeklaration verweisen auf ein URI, der ein URI-Schema-Name, z. B. beginnt `http`, die immer durch einen Doppelpunkt gefolgt wird. Die `clr-namespace` Teil dieser Zeichenfolge soll dieser Konvention zu imitieren.

Sowohl diese Namespacedeklarationen befinden sich der **StaticConstantsPage** Beispiel. Beachten Sie, dass die `BoxView` Dimensionen werden festgelegt, um `Math.PI` und `Math.E`, jedoch mit einem Faktor von 100 skalierte:

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

Die Größe der resultierenden `BoxView` relativ zum Bildschirm ist plattformabhängig:

 [![](xaml-markup-extensions-images/staticconstants.png "Steuerelemente mithilfe von X: statische Markuperweiterung")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "Steuerelemente mithilfe von X: statische Markuperweiterung")

## <a name="other-standard-markup-extensions"></a>Andere Standard-Markuperweiterungen

Mehrere Markuperweiterungen für XAML systemintern sind und in Xamarin.Forms XAML-Dateien unterstützt. Einige davon nicht sehr häufig verwendet werden, aber sind wichtig, wenn Sie diese benötigen:

-  Wenn eine Eigenschaft eine nicht- `null` möchten, dass Wert durch Sie jedoch den Standardwert festgelegt ist, dass `null`, legen Sie sie auf die `{x:Null}` Markuperweiterung.
-  Wenn eine Eigenschaft vom Typ `Type`, weisen Sie sie einer `Type` -Objekt unter Verwendung der Markuperweiterung `{x:Type someClass}`.
-  Sie können Arrays in XAML mit definieren die `x:Array` Markuperweiterung. Diese Markuperweiterung ist ein erforderliches Attribut, das mit dem Namen `Type` , der den Typ der Elemente im Array angibt.
- Die `Binding` Markuperweiterung ausführlicher [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Die ConstraintExpression-Markuperweiterung

Markuperweiterungen können Eigenschaften verfügen, aber sie sind nicht festgelegt, wie XML-Attribute. In einer Markuperweiterung eigenschafteneinstellungen werden durch Kommas getrennt und ohne Anführungszeichen, die innerhalb der geschweiften Klammern angezeigt werden.

Dies kann veranschaulicht werden, mit der Xamarin.Forms-Markuperweiterung, die mit dem Namen `ConstraintExpression`, das verwendet wird, mit der `RelativeLayout` Klasse. Sie können die Position und Größe der eine untergeordnete Ansicht als Konstante oder relativ zu einem übergeordneten Element oder andere benannte Ansicht angeben. Die Syntax der `ConstraintExpression` ermöglicht Ihnen das Festlegen der Position oder Größe einer Ansicht mit der eine `Factor` wie oft eine Eigenschaft einer anderen Ansicht zusammen mit einer `Constant`. Etwas komplexeren spielen als erforderlich Code.

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

Die wichtigste Lektion, die Sie, in diesem Beispiel ausführen sollten ist vielleicht die Syntax der Markuperweiterung: Keine Anführungszeichen müssen innerhalb der geschweiften Klammern einer Markuperweiterung angezeigt werden. Wenn Sie die Markuperweiterung in einer XAML-Datei eingeben, ist es natürlich, die Werte der Eigenschaften in Anführungszeichen gesetzt werden soll. Widerstehen Sie der Versuchung!

Hier wird das Programm ausgeführt wird:

[![](xaml-markup-extensions-images/relativelayout.png "Relative Layouts, die mit Einschränkungen")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "Relative Layout mithilfe von Einschränkungen")

## <a name="summary"></a>Zusammenfassung

Die hier gezeigten XAML-Markuperweiterungen bieten wichtige Unterstützung für XAML-Dateien. Jedoch ist Sie möglicherweise die wertvollste XAML-Markuperweiterung `Binding`, die finden Sie im nächsten Teil dieser Reihe [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus einer Datenbindung zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
