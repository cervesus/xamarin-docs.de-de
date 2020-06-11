---
Title: "Teil 3. XAML-Markup Erweiterungen "Description:" XAML-Markup Erweiterungen bilden ein wichtiges Feature in XAML, das es ermöglicht, Eigenschaften auf Objekte oder Werte festzulegen, auf die indirekt aus anderen Quellen verwiesen wird. "
ms. Prod: xamarin ms. Technology: xamarin-Forms ms. assetid: F4A37564-B18B-42FF-B841-9A1949895AB6 Author: davidbritch ms. Author: dabritch ms. Date: 03/27/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="part-3-xaml-markup-extensions"></a>Teil 3. XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML-Markup Erweiterungen bilden ein wichtiges Feature in XAML, das es ermöglicht, Eigenschaften auf Objekte oder Werte festzulegen, auf die indirekt aus anderen Quellen verwiesen wird. XAML-Markup Erweiterungen sind besonders wichtig für die Freigabe von Objekten und verweisen auf Konstanten, die in einer Anwendung verwendet werden, aber Sie finden Ihr größtes Hilfsprogramm in Daten Bindungen._

## <a name="xaml-markup-extensions"></a>XAML-Markuperweiterungen

Im Allgemeinen verwenden Sie XAML, um die Eigenschaften eines Objekts auf explizite Werte festzulegen, z. b. eine Zeichenfolge, eine Zahl, ein Enumerationsmember oder eine Zeichenfolge, die in einen Wert hinter den Kulissen konvertiert wird.

Manchmal müssen Eigenschaften jedoch auf Werte verweisen, die an anderer Stelle definiert sind, oder für die möglicherweise ein wenig Code zur Laufzeit verarbeitet werden. Zu diesem Zweck sind XAML- *Markup Erweiterungen* verfügbar.

Diese XAML-Markup Erweiterungen sind keine XML-Erweiterungen. XAML ist vollständig gültiger XML-Code. Sie werden als "Erweiterungen" bezeichnet, da Sie durch Code in Klassen unterstützt werden, die implementieren `IMarkupExtension` . Sie können eigene benutzerdefinierte Markup Erweiterungen schreiben.

In vielen Fällen sind XAML-Markup Erweiterungen in XAML-Dateien sofort erkennbar, da Sie als durch geschweifte Klammern getrennte Attribut Einstellungen angezeigt werden: {und}, aber manchmal werden Markup Erweiterungen im Markup als konventionelle Elemente angezeigt.

## <a name="shared-resources"></a>Freigegebene Ressourcen

Einige XAML-Seiten enthalten mehrere Ansichten mit Eigenschaften, die auf dieselben Werte festgelegt sind. Viele der Eigenschafts Einstellungen für diese Objekte sind z. b. `Button` identisch:

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

Wenn eine dieser Eigenschaften geändert werden muss, empfiehlt es sich, die Änderung nur einmal statt dreimal vorzunehmen. Wenn dies Code wäre, würden Sie wahrscheinlich Konstanten und statische schreibgeschützte Objekte verwenden, um solche Werte konsistent und leicht zu ändern.

In XAML besteht eine beliebte Lösung darin, solche Werte oder Objekte in einem *Ressourcen Wörterbuch*zu speichern. Die- `VisualElement` Klasse definiert eine Eigenschaft `Resources` mit dem Namen vom Typ `ResourceDictionary` , bei der es sich um ein Wörterbuch mit Schlüsseln vom Typ `string` und Werten vom Typ handelt `object` . Sie können Objekte in dieses Wörterbuch einfügen und dann in XAML von Markup aus darauf verweisen.

Um ein Ressourcen Wörterbuch auf einer Seite zu verwenden, schließen Sie ein paar von `Resources` Eigenschaften-Element-Tags ein. Es ist am einfachsten, diese oben auf der Seite zu platzieren:

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

Es ist auch erforderlich, Tags explizit einzuschließen `ResourceDictionary` :

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

Nun können Objekte und Werte verschiedener Typen dem Ressourcen Wörterbuch hinzugefügt werden. Diese Typen müssen instanziiert werden können. Sie können z. b. keine abstrakten Klassen sein. Diese Typen müssen auch über einen öffentlichen Parameter losen Konstruktor verfügen. Jedes Element erfordert einen Wörterbuch Schlüssel, der mit dem-Attribut angegeben wird `x:Key` . Beispiel:

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

Diese beiden Elemente sind Werte vom Strukturtyp `LayoutOptions` , und jede verfügt über einen eindeutigen Schlüssel und eine oder zwei Eigenschaften. Im Code und im Markup ist es viel häufiger üblich, die statischen Felder von zu verwenden `LayoutOptions` . hier ist es jedoch bequemer, die Eigenschaften festzulegen.

Nun müssen die-Eigenschaft und die-Eigenschaft dieser `HorizontalOptions` `VerticalOptions` Schaltflächen auf diese Ressourcen festgelegt werden, und das geschieht mit der `StaticResource` XAML-Markup Erweiterung:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

Die `StaticResource` Markup Erweiterung ist immer mit geschweiften Klammern begrenzt und enthält den Wörterbuch Schlüssel.

Der Name `StaticResource` unterscheidet ihn von `DynamicResource` , der Xamarin.Forms auch unterstützt. `DynamicResource`ist für Wörterbuch Schlüssel, die Werten zugeordnet sind, die sich während der Laufzeit ändern können, während gleichzeitig `StaticResource` auf Elemente aus dem Wörterbuch zugreift, wenn die Elemente auf der Seite erstellt werden.

Bei der `BorderWidth` -Eigenschaft ist es erforderlich, einen Double-Wert im Wörterbuch zu speichern. XAML definiert Tags für allgemeine Datentypen wie `x:Double` und `x:Int32` :

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

Sie müssen Sie nicht in drei Zeilen einfügen. Dieser Wörterbucheintrag für diesen Drehwinkel nimmt nur eine Zeile an:

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

Auf diese beiden Ressourcen kann auf die gleiche Weise wie die- `LayoutOptions` Werte verwiesen werden:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Für Ressourcen vom Typ `Color` können Sie dieselben Zeichen folgen Darstellungen verwenden, die Sie verwenden, wenn Sie Attribute dieser Typen direkt zuweisen. Die Typkonverter werden aufgerufen, wenn die Ressource erstellt wird. Im folgenden finden Sie eine Ressource des Typs `Color` :

```xaml
<Color x:Key="textColor">Red</Color>
```

Häufig legen Programme eine `FontSize` Eigenschaft auf einen Member der- `NamedSize` Enumeration fest, z `Large` . b.. Die- `FontSizeConverter` Klasse arbeitet im Hintergrund, um Sie mithilfe der-Methode in einen Platt Form abhängigen Wert zu konvertieren `Device.GetNamedSized` . Wenn Sie jedoch eine Ressource mit Schriftart Größe definieren, ist es sinnvoller, einen numerischen Wert zu verwenden, der hier als `x:Double` Typ dargestellt wird:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Nun werden alle Eigenschaften außer `Text` durch Ressourcen Einstellungen definiert:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Es ist auch möglich, `OnPlatform` innerhalb des Ressourcen Wörterbuchs zu verwenden, um unterschiedliche Werte für die Plattformen zu definieren. Im folgenden wird erläutert, wie ein `OnPlatform` Objekt für verschiedene Textfarben Teil des Ressourcen Wörterbuchs sein kann:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Beachten Sie, dass ein `OnPlatform` `x:Key` Attribut abruft, da es ein Objekt im Wörterbuch und ein Attribut ist, `x:TypeArguments` weil es sich um eine generische Klasse handelt. Die `iOS` `Android` Attribute, und `UWP` werden in-Werte konvertiert, `Color` Wenn das Objekt initialisiert wird.

Hier ist die abschließende vollständige XAML-Datei mit drei Schaltflächen, die auf sechs freigegebene Werte zugreifen:

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

Die Screenshots überprüfen das konsistente formatieren und das Platt Form abhängige Format:

[![Formatierte Steuerelemente](xaml-markup-extensions-images/sharedresources.png)](xaml-markup-extensions-images/sharedresources-large.png#lightbox)

Obwohl es am häufigsten ist, die Auflistung `Resources` am oberen Rand der Seite zu definieren, beachten Sie, dass die `Resources` -Eigenschaft von definiert wird `VisualElement` , und Sie können Auflistungen `Resources` für andere Elemente auf der Seite haben. Fügen Sie z. b. `StackLayout` in diesem Beispiel einen hinzu:

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

Sie werden feststellen, dass die Textfarbe der Schaltflächen jetzt blau ist. Wenn der XAML `StaticResource` -Parser auf eine Markup Erweiterung stößt, wird im Grunde die visuelle Struktur durchsucht, und die erste `ResourceDictionary` stößt mit diesem Schlüssel wird verwendet.

Einer der gängigsten Typen von Objekten, der in Ressourcen Wörterbüchern gespeichert ist, ist die Xamarin.Forms `Style` , die eine Auflistung von Eigenschafts Einstellungen definiert. Stile werden in den Artikel [Stilen](~/xamarin-forms/user-interface/styles/index.md)erläutert.

Manchmal fragen sich Entwickler, wenn Sie ein visuelles Element wie `Label` oder `Button` in ein einfügen können `ResourceDictionary` . Obwohl es sicherlich möglich ist, ist es nicht sinnvoll. Der Zweck der `ResourceDictionary` besteht darin,-Objekte gemeinsam zu verwenden. Ein visuelles Element kann nicht freigegeben werden. Dieselbe Instanz kann nicht zweimal auf einer einzelnen Seite angezeigt werden.

## <a name="the-xstatic-markup-extension"></a>Die x:statische Markup Erweiterung

Trotz der Ähnlichkeit ihrer Namen `x:Static` `StaticResource` sind und sehr verschieden. `StaticResource`Gibt ein-Objekt aus einem Ressourcen Wörterbuch zurück, während `x:Static` auf eine der folgenden zugreift:

- ein öffentliches statisches Feld
- eine öffentliche statische Eigenschaft
- ein öffentliches Konstantenfeld
- Ein Enumerationsmember.

Die `StaticResource` Markup Erweiterung wird von XAML-Implementierungen unterstützt, die ein Ressourcen Wörterbuch definieren, während ein System interner `x:Static` Teil von XAML ist, wie das `x` Präfix zeigt.

Im folgenden finden Sie einige Beispiele, die veranschaulichen, wie auf `x:Static` statische Felder und Enumerationsmember explizit verwiesen werden kann:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Bisher ist das nicht sehr beeindruckend. Die `x:Static` Markup Erweiterung kann jedoch auch auf statische Felder oder Eigenschaften aus Ihrem eigenen Code verweisen. Hier ist beispielsweise eine- `AppConstants` Klasse, die einige statische Felder enthält, die Sie möglicherweise auf mehreren Seiten in einer Anwendung verwenden möchten:

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

Um auf die statischen Felder dieser Klasse in der XAML-Datei zu verweisen, benötigen Sie eine Möglichkeit, in der XAML-Datei anzugeben, in der sich diese Datei befindet. Dies wird mit einer XML-Namespace Deklaration durchzuführen.

Beachten Sie, dass die XAML-Dateien, die als Teil der standardmäßigen Xamarin.Forms XAML-Vorlage erstellt wurden, zwei XML-Namespace Deklarationen enthalten: eine für den Zugriff auf Xamarin.Forms Klassen und eine andere für das verweisen auf die in XAML intrinsischen

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Für den Zugriff auf andere Klassen benötigen Sie zusätzliche XML-Namespace Deklarationen. Jede zusätzliche XML-Namespace Deklaration definiert ein neues Präfix. Für den Zugriff auf Klassen, die für die freigegebene Anwendung .NET Standard Bibliothek wie z. b. `AppConstants` verwendet werden, verwenden XAML-Programmierer häufig das Präfix `local` Die Namespace Deklaration muss den CLR-Namespace Namen (Common Language Runtime) angeben, der auch als .NET-Namespace Name bezeichnet wird. Dies ist der Name, der in einer c#- `namespace` Definition oder in einer- `using` Direktive angezeigt wird:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Sie können auch XML-Namespace Deklarationen für .NET-Namespaces in jeder Assembly definieren, auf die die .NET Standard-Bibliothek verweist. Hier ist beispielsweise ein `sys` Präfix für den .NET-Standard `System` Namespace, der sich in der **netstandard** -Assembly befindet. Da es sich um eine andere Assembly handelt, müssen Sie auch den Assemblynamen angeben, in diesem Fall **netstandard**:

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

Beachten Sie, dass auf das Schlüsselwort `clr-namespace` ein Doppelpunkt und dann der .NET-Namespace Name, gefolgt von einem Semikolon, dem Schlüsselwort `assembly` , einem Gleichheitszeichen und dem Assemblynamen folgt.

Ja, ein Doppelpunkt folgt, `clr-namespace` aber das Gleichheitszeichen folgt `assembly` . Die Syntax wurde auf diese Weise absichtlich definiert: die meisten XML-Namespace Deklarationen verweisen auf einen URI, der einen URI-Schema Namen startet `http` , z. b., dem immer ein Doppelpunkt folgt. Der `clr-namespace` Teil dieser Zeichenfolge soll diese Konvention imitieren.

Beide Namespace Deklarationen sind im **staticconstantspage** -Beispiel enthalten. Beachten Sie, dass die `BoxView` Dimensionen auf und festgelegt sind `Math.PI` `Math.E` , aber mit dem Faktor 100 skaliert werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
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

Die Größe des resultierenden `BoxView` relativ zum Bildschirm ist plattformabhängig:

[![Steuerelemente mit x:Static-Markup Erweiterung](xaml-markup-extensions-images/staticconstants.png)](xaml-markup-extensions-images/staticconstants-large.png#lightbox)

## <a name="other-standard-markup-extensions"></a>Andere Standard Markup Erweiterungen

Mehrere Markup Erweiterungen sind in XAML intrinsisch und werden in Xamarin.Forms XAML-Dateien unterstützt. Einige dieser Elemente werden nicht sehr häufig verwendet, sind jedoch für Sie erforderlich:

- Wenn eine Eigenschaft standardmäßig einen nicht-- `null` Wert aufweist, Sie jedoch auf festlegen möchten `null` , legen Sie Sie auf die `{x:Null}` Markup Erweiterung fest.
- Wenn eine Eigenschaft vom Typ ist `Type` , können Sie Sie `Type` mithilfe der Markup Erweiterung einem-Objekt zuweisen `{x:Type someClass}` .
- Sie können Arrays in XAML mithilfe der `x:Array` Markup Erweiterung definieren. Diese Markup Erweiterung verfügt über ein erforderliches Attribut mit `Type` dem Namen, das den Typ der Elemente im Array angibt.
- Die `Binding` Markup Erweiterung wird in [Teil 4 erläutert. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).
- Die `RelativeSource` Markup Erweiterung wird unter [relative Bindungen](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)erläutert.

## <a name="the-constraintexpression-markup-extension"></a>Die Markup Erweiterung "einschränintexpression"

Markup Erweiterungen können Eigenschaften haben, Sie sind jedoch nicht wie XML-Attribute festgelegt. In einer Markup Erweiterung werden Eigenschafts Einstellungen durch Kommas getrennt, und in den geschweiften Klammern werden keine Anführungszeichen angezeigt.

Dies kann mit der Xamarin.Forms Markup Erweiterung namens veranschaulicht werden `ConstraintExpression` , die mit der-Klasse verwendet wird `RelativeLayout` . Sie können den Speicherort oder die Größe einer untergeordneten Sicht als Konstante oder relativ zu einer übergeordneten oder anderen benannten Ansicht angeben. Die Syntax von `ConstraintExpression` ermöglicht es Ihnen, die Position oder Größe einer Ansicht mit einem `Factor` Uhrzeitwert einer Eigenschaft einer anderen Ansicht und einem festzulegen `Constant` . Alles komplexere als das, was Code erfordert.

Hier ist ein Beispiel angegeben:

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

Die wichtigste Lektion, die Sie in diesem Beispiel berücksichtigen sollten, ist die Syntax der Markup Erweiterung: Es müssen keine Anführungszeichen innerhalb der geschweiften Klammern einer Markup Erweiterung angezeigt werden. Wenn Sie die Markup Erweiterung in eine XAML-Datei eingeben, empfiehlt es sich, die Werte der Eigenschaften in Anführungszeichen einzuschließen. Widerstehen Sie der Versuchung!

Hier ist das Programm, das ausgeführt wird:

[![Relativer Layout mithilfe von Einschränkungen](xaml-markup-extensions-images/relativelayout.png)](xaml-markup-extensions-images/relativelayout-large.png#lightbox)

## <a name="summary"></a>Zusammenfassung

Die hier gezeigten XAML-Markup Erweiterungen bieten wichtige Unterstützung für XAML-Dateien. Aber vielleicht ist die nützlichste XAML-Markup Erweiterung `Binding` , die im nächsten Teil dieser Reihe behandelt wird, [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 1: Einstieg in XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Wichtige XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Von der Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
