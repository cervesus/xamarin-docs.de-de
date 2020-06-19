---
title: 'title: "Xamarin.Forms Relative Bindungen" description: "In diesem Artikel wird beschrieben, wie Sie mithilfe der RelativeSource-Markuperweiterung relative Bindungen erstellen, um die Bindungsquelle relativ zur Position des Bindungsziels festzulegen."'
description: 'ms.prod: xamarin ms.assetid: CC64BB1D-8303-46B1-94B6-4EF2F20317A8 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 12/04/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: CC64BB1D-8303-46B1-94B6-4EF2F20317A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8d4e2696e6027f07b7b8e638cd1e0f1d65a5503d
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84139710"
---
# <a name="xamarinforms-relative-bindings"></a>Relative Bindungen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Mit relativen Bindungen kann die Bindungsquelle relativ zur Position des Bindungsziels festgelegt werden. Sie werden mit der Markuperweiterung `RelativeSource` erstellt und als `Source`-Eigenschaft eines Bindungsausdrucks festgelegt.

Die Markuperweiterung `RelativeSource` wird von der Klasse `RelativeSourceExtension` unterstützt, in der die folgenden Eigenschaften definiert werden:

- `Mode` (vom Typ `RelativeBindingSourceMode`): Gibt den Speicherort der Bindungsquelle relativ zur Position des Bindungsziels an.
- `AncestorLevel` (vom Typ `int`): Optionale Vorgängerebene, nach der gesucht werden soll, wenn die `Mode`-Eigenschaft `FindAncestor` lautet. Bei einer `AncestorLevel` von `n` werden `n-1` Instanzen vom `AncestorType` übersprungen.
- `AncestorType` (vom Typ `Type`): Vorgängertyp, nach dem gesucht werden soll, wenn die `Mode`-Eigenschaft `FindAncestor` lautet.

> [!NOTE]
> Im XAML-Parser kann die Klasse `RelativeSourceExtension` zu `RelativeSource` abgekürzt werden.

Die `Mode`-Eigenschaft sollte auf einen der Member der Enumeration `RelativeBindingSourceMode` festgelegt werden:

- `TemplatedParent` gibt das Element an, auf das die Vorlage (die das gebundene Element enthält) angewendet wird. Weitere Informationen finden Sie unter [An ein übergeordnetes Element mit Vorlagen binden](#bind-to-a-templated-parent).
- `Self` gibt das Element an, für das die Bindung festgelegt wird. Damit können Sie eine Eigenschaft dieses Elements an eine andere Eigenschaft desselben Elements binden. Weitere Informationen finden Sie unter [An sich selbst binden](#bind-to-self).
- `FindAncestor` gibt das Vorgängerelement in der visuellen Struktur des gebundenen Elements an. Verwenden Sie diesen Modus, um an ein Vorgängersteuerelement zu binden, das von der Eigenschaft `AncestorType` dargestellt wird. Weitere Informationen finden Sie unter [An ein Vorgängerelement binden](#bind-to-an-ancestor).
- `FindAncestorBindingContext` gibt die `BindingContext`-Eigenschaft des Vorgängerelements in der visuellen Struktur des gebundenen Elements an. Verwenden Sie diesen Modus, um an die `BindingContext`-Eigenschaft eines Vorgängersteuerelements zu binden, das von der Eigenschaft `AncestorType` dargestellt wird. Weitere Informationen finden Sie unter [An ein Vorgängerelement binden](#bind-to-an-ancestor).

Die `Mode`-Eigenschaft stellt die Inhaltseigenschaft der Klasse `RelativeSourceExtension` dar. Daher können Sie bei XAML-Markupausdrücken (die mit geschweiften Klammern ausgedrückt werden) den `Mode=`-Teil des Ausdrucks entfernen.

Weitere Informationen über Xamarin.Forms-Markuperweiterungen finden Sie unter [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md).

## <a name="bind-to-self"></a>An sich selbst binden

Bei relativen Bindungen wird der `Self`-Modus verwendet, um eine Eigenschaft eines Elements an eine andere Eigenschaft desselben Elements zu binden:

```xaml
<BoxView Color="Red"
         WidthRequest="200"
         HeightRequest="{Binding Source={RelativeSource Self}, Path=WidthRequest}"
         HorizontalOptions="Center" />
```

In diesem Beispiel wird in der [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse die Eigenschaft `WidthRequest` auf eine feste Größe festgelegt und die Eigenschaft `HeightRequest` an die Eigenschaft `WidthRequest` gebunden. Beide Eigenschaften sind demnach identisch, und es wird ein Quadrat angezeigt:

[![Screenshot einer relativen Bindung im Self-Modus unter iOS und Android](relative-bindings-images/self-relative-binding.png "Relative Bindung im Self-Modus")](relative-bindings-images/self-relative-binding-large.png#lightbox "Relative Bindung im Self-Modus")

> [!IMPORTANT]
> Soll eine Eigenschaft eines Elements an eine andere Eigenschaft desselben Elements gebunden werden, müssen die Eigenschaften vom selben Typ sein. Alternativ können Sie für die Bindung einen Konverter angeben, um den Wert zu konvertieren.

Dieser Bindungsmodus wird häufig verwendet, um die `BindingContext`-Eigenschafts eines Objekts auf eine Eigenschaft desselben Objekts festzulegen. Im folgenden Code wird ein Beispiel hierfür dargestellt:

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die `BindingContext`-Eigenschaft der Seite auf die `DefaultViewModel`-Eigenschaft derselben Seite festgelegt. Diese Eigenschaft wird in der CodeBehind-Datei für die Seite definiert und stellt eine ViewModel-Instanz bereit. Die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse wird an die `Employees`-Eigenschaft von ViewModel gebunden.

## <a name="bind-to-an-ancestor"></a>An ein Vorgängerelement binden

Die Modi `FindAncestor` und `FindAncestorBindingContext` für relative Bindungen werden verwendet, um an übergeordnete Elemente (eines bestimmten Typs) in der visuellen Struktur zu binden. Mit dem `FindAncestor`-Modus wird an ein übergeordnetes Element gebunden, das vom Typ [`Element`](xref:Xamarin.Forms.Element) abgeleitet wird. Verwenden Sie den `FindAncestorBindingContext`-Modus, um an die `BindingContext`-Eigenschaft eines übergeordneten Elements zu binden.

> [!WARNING]
> Bei Verwendung von relativen Bindungen in den Modi `FindAncestor` und `FindAncestorBindingContext` muss die Eigenschaft `AncestorType` auf `Type` festgelegt sein. Andernfalls wird eine `XamlParseException` ausgelöst.

Wird die Eigenschaft `Mode` nicht explizit festgelegt, wird durch Festlegen der Eigenschaft `AncestorType` auf einen von [`Element`](xref:Xamarin.Forms.Element) abgeleiteten Typ die `Mode`-Eigenschaft implizit auf `FindAncestor` festgelegt. Wird die `AncestorType`-Eigenschaft entsprechend hierzu auf einen Typ festgelegt, der nicht von `Element` abgeleitet ist, wird die `Mode`-Eigenschaft implizit auf `FindAncestorBindingContext` festgelegt.

> [!NOTE]
> Relative Bindungen, die den `FindAncestorBindingContext`-Modus verwenden, werden erneut angewendet, wenn sich der `BindingContext` eines Vorgängers ändert.

Im folgenden XAML-Beispiel wird die `Mode`-Eigenschaft implizit auf `FindAncestorBindingContext` festgelegt:

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Label Text="{Binding Fullname}"
                                   VerticalOptions="Center" />
                            <Button Text="Delete"
                                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeleteEmployeeCommand}"
                                    CommandParameter="{Binding}"
                                    HorizontalOptions="EndAndExpand" />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die `BindingContext`-Eigenschaft der Seite auf die `DefaultViewModel`-Eigenschaft derselben Seite festgelegt. Diese Eigenschaft wird in der CodeBehind-Datei für die Seite definiert und stellt eine ViewModel-Instanz bereit. Die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse wird an die `Employees`-Eigenschaft von ViewModel gebunden. Die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse, die die Darstellung der einzelnen Elemente in `ListView` definiert, enthält eine [`Button`](xref:Xamarin.Forms.Button)-Klasse. Die `Command`-Eigenschaft der Schaltfläche ist an die `DeleteEmployeeCommand`-Eigenschaft des übergeordneten ViewModel-Elements gebunden. Durch Tippen auf `Button` wird ein Mitarbeiter gelöscht:

[![Screenshot einer relativen Bindung im FindAncestor-Modus unter iOS und Android](relative-bindings-images/findancestor-relative-binding.png "Relative Bindung im FindAncestor-Modus")](relative-bindings-images/findancestor-relative-binding-large.png#lightbox "Relative Bindung im FindAncestor-Modus")

Zudem kann die optionale Eigenschaft `AncestorLevel` dazu beitragen, die Vorgängersuche in Szenarios eindeutig zu machen, in denen eventuell mehr als ein Vorgängerelement dieses Typs in der visuellen Struktur vorhanden ist:

```xaml
<Label Text="{Binding Source={RelativeSource AncestorType={x:Type Entry}, AncestorLevel=2}, Path=Text}" />
```

In diesem Beispiel wird die Eigenschaft `Label.Text` an die Eigenschaft `Text` der zweiten [`Entry`](xref:Xamarin.Forms.Entry)-Klasse gebunden. Diese wird im Pfad nach oben beginnend beim Zielelement der Bindung gefunden.

> [!NOTE]
> Legen Sie die Eigenschaft `AncestorLevel` auf 1 fest, um das dem Bindungszielelement am nächsten gelegene Vorgängerelement zu finden.

## <a name="bind-to-a-templated-parent"></a>An ein übergeordnetes Element mit Vorlagen binden

Der Modus `TemplatedParent` für eine relative Bindung wird verwendet, um von einer Steuerelementvorlage an die Instanz eines Laufzeitobjekts zu binden, auf die die Vorlage angewendet wird. Diese wird auch als übergeordnetes Element mit Vorlagen bezeichnet. Dieser Modus kann nur angewendet werden, wenn sich die relative Bindung in einer Steuerelementvorlage befindet. Die Vorgehensweise ähnelt dem Festlegen einer `TemplateBinding`-Eigenschaft.

Der folgende XAML-Code zeigt ein Beispiel für eine relative Bindung im `TemplatedParent`-Modus:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                   BackgroundColor="{Binding CardColor}"
                   BorderColor="{Binding BorderColor}"
                   ...>
                <Grid>
                    ...
                    <Label Text="{Binding CardTitle}"
                           ... />
                    <BoxView BackgroundColor="{Binding BorderColor}"
                             ... />
                    <Label Text="{Binding CardDescription}"
                           ... />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout>        
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel ist die `BindingContext`-Eigenschaft der [`Frame`](xref:Xamarin.Forms.Frame)-Klasse, die das Stammelement der `ControlTemplate`-Eigenschaft darstellt, auf die Instanz des Laufzeitobjekts festgelegt, auf die die Vorlage angewendet wird. Daher lösen die `Frame`-Klasse und ihre untergeordneten Elemente ihre Bindungsausdrücke anhand der Eigenschaften der einzelnen `CardView`-Objekte auf:

[![Screenshot einer relativen Bindung im TemplatedParent-Modus unter iOS und Android](relative-bindings-images/templatedparent-relative-binding.png "Relative Bindung im TemplatedParent-Modus")](relative-bindings-images/templatedparent-relative-binding-large.png#lightbox "Relative Bindung im TemplatedParent-Modus")

Weitere Informationen zu Steuerelementvorlagen finden Sie unter [Xamarin.Forms-Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms-Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md)
