---
title: Einführung in die Xamarin.Forms-Shell
description: Die Xamarin.Forms-Shell stellt die wichtigsten Features bereit, die von den meisten Anwendungen benötigt werden, beispielsweise eine gemeinsame Benutzeroberfläche für die Navigation, ein URI-basiertes Navigationsschema und einen integrierten Suchhandler.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 20d9fb79d03990824dd884b62138a3e29b3ee04f
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054480"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms-Shell

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

Die Xamarin.Forms-Shell reduziert die Komplexität der Entwicklung mobiler Anwendungen, indem es die grundlegenden Features bereitstellt, die die meisten mobilen Anwendungen benötigen, einschließlich:

- Einer zentralen Stelle zur Beschreibung der visuellen Hierarchie einer Anwendung.
- Eine gemeinsame Benutzeroberfläche für die Navigation.
- Ein URI-basiertes Navigationsschema, das die Navigation zu jeder Seite in der Anwendung ermöglicht.
- Ein integrierter Suchhandler.

Darüber hinaus profitieren Shell-Anwendungen von einer höheren Renderinggeschwindigkeit und einer geringeren Arbeitsspeichernutzung.

> [!IMPORTANT]
> Bestehende iOS- und Android-Anwendungen können die Shell übernehmen und sofort von Verbesserungen bei Navigation, Leistung und Erweiterbarkeit profitieren.

Shell ist derzeit experimentell und kann nur verwendet werden, indem `Forms.SetFlags("Shell_Experimental");` zu Ihrem Plattformprojekt hinzugefügt wird, bevor die `Forms.Init`-Methode aufgerufen wird.

# <a name="androidtabandroid"></a>[Android](#tab/android)

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        global::Xamarin.Forms.Forms.SetFlags("Shell_Experimental");

        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());
    }
}
```

# <a name="iostabios"></a>[iOS](#tab/ios)

```csharp
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.SetFlags("Shell_Experimental");

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
    }
}
```

----

## <a name="shell-navigation-experience"></a>Shell-Navigationsoberfläche

Die Shell bietet eine Benutzeroberfläche mit zahlreichen Optionen für die Navigation, basierend auf Flyouts und Registerkarten. Die oberste Ebene der Navigation in Shell-Anwendung ist ein Flyout:

[![Screenshot eines Shell-Flyouts, unter iOS und Android](introduction-images/flyout.png "Shell-Flyout")](introduction-images/flyout-large.png#lightbox "Shell-Flyout")

Die Auswahl eines Flyoutelements führt zu der unteren Registerkarte, die das ausgewählte Element darstellt und anzeigt:

[![Screenshot der unteren Registerkarten der Shell, unter iOS und Android](introduction-images/monkeys.png "untere Registerkarten der Shell")](introduction-images/monkeys-large.png#lightbox "untere Registerkarten der Shell")

> [!NOTE]
> Wenn der Flyout nicht geöffnet ist, wird die untere Registerkartenleiste als oberste Ebene der Navigation in der Anwendung betrachtet.

Jede Registerkarte zeigt eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) an. Wenn eine untere Registerkarte mehr als eine Seite enthält, sind die Seiten jedoch über die obere Registerkartenleiste navigierbar:

[![Screenshot der oberen Registerkarten der Shell unter iOS und Android](introduction-images/cats.png "obere Registerkarten der Shell")](introduction-images/cats-large.png#lightbox "obere Registerkarten der Shell")

Innerhalb jeder Registerkarte kann zu zusätzlichen [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten navigiert werden:

[![Screenshot der Shell-Seitennavigation unter iOS and Android](introduction-images/cat-details.png "Shell-App-Navigation")](introduction-images/cat-details-large.png#lightbox "Shell-App-Navigation")

## <a name="subclassing-the-shell-class"></a>Erstellen von Unterklassen für die Shell-Klasse

Ein `Shell`-Objekt mit Unterklassen beschreibt die visuelle Hierarchie einer Shell-Anwendung und besteht aus drei hierarchischen Hauptobjekten:

- `FlyoutItem`: Stellt ein oder mehrere Elemente im Flyout dar. Jedes `FlyoutItem`-Objekt ist ein untergeordnetes Objekt des `Shell`-Objekts.
- `Tab`: Stellt gruppierten Inhalt dar, der über die unteren Registerkarten navigierbar ist. Jedes `Tab`-Objekt ist ein untergeordnetes Objekt eines `FlyoutItem`-Objekts.
- `ShellContent`: Stellt die `ContentPage`-Objekte in Ihrer Anwendung dar. Jedes `ShellContent`-Objekt ist ein untergeordnetes Objekt eines `Tab`-Objekts. Wenn mehr als ein `ShellContent`-Objekt in einem `Tab`-Objekt vorhanden ist, sind die Objekte über obere Registerkarten navigierbar.

Keines dieser Objekte repräsentiert eine Benutzeroberfläche, sondern die Organisation der visuellen Struktur der Hierarchie. Shell wird diese Objekte übernehmen und die Benutzeroberfläche für die Navigation der Inhalte erstellen.

> [!NOTE]
> Die `FlyoutItem`-Klasse ist ein Alias für die `ShellItem`-Klasse, und die `Tab`-Klasse ist ein Alias für die `ShellSection`-Klasse. Diese Aliase dienen dazu, die-API benutzerfreundlicher zu machen.

Die folgende XAML zeigt ein Beispiel für ein `Shell`-Objekt mit Unterklassen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

Wird dieses Beispiel ausgeführt, zeigt die XAML die `CatsPage` an, da sie das erste Element des im `Shell`-Objekt mit Unterklassen deklarierten Inhalts ist:

[![Screenshot einer Shell-App unter iOS und Android](introduction-images/cats.png "Shell-App")](introduction-images/cats-large.png#lightbox "Shell-App")

Wird auf das Hamburger-Symbol gedrückt oder von links gewischt, wird das Flyout angezeigt:

[![Screenshot eines Shell-Flyouts, unter iOS und Android](introduction-images/flyout-reduced.png "Shell-Flyout")](introduction-images/flyout-reduced-large.png#lightbox "Shell-Flyout")

> [!IMPORTANT]
> In einer Shell-Anwendung wird jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das ein untergeordnetes Element eines `ShellContent`-Objekts ist, während des Anwendungsstarts erstellt. Hinzufügen weiterer `ShellContent`-Objekte mit diesem Ansatz führt dazu, dass zusätzliche Seiten während des Anwendungsstarts erstellt werden, was zu einer schlechten Starterfahrung führen kann. Mit der Shell können Seiten jedoch auch nach Bedarf als Reaktion auf die Navigation erstellt werden. Weitere Informationen finden Sie unter [Effizientes Laden von Seiten](tabs.md#efficient-page-loading) im Handbuch über die [Registerkarten der Xamarin.Forms-Shell](tabs.md).

## <a name="bootstrapping-a-shell-application"></a>Bootstrapping für eine Shell-Anwendung

Der Bootstrapvorgang wird für eine Shell-Anwendung durchgeführt, indem die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft der `App`-Klasse auf ein `Shell`-Objekt mit Unterklassen festgelegt wird:

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

In diesem Beispiel ist die `AppShell`-Klasse eine XAML-Datei, die von der `Shell`-Klasse abgeleitet ist, die die visuelle Hierarchie der Anwendung beschreibt.

## <a name="shell-appearance"></a>Shell-Darstellung

Die `Shell`-Klasse definiert die folgenden Eigenschaften, die die Darstellung einer Shell-Anwendung steuern:

- `ShellBackgroundColor` vom Typ `Color`, eine angefügte Eigenschaft, die die Hintergrundfarbe im Shell-Chrom definiert. Die Farbe wird hinter dem Shell-Inhalt nicht ausgefüllt.
- `ShellDisabledColor` vom Typ `Color`, eine angefügte Eigenschaft, die die Farbe zum Schattieren von Text und deaktivierten Symbolen definiert.
- `ShellForegroundColor` vom Typ `Color`, eine angefügte Eigenschaft, die die Farbe zum Schattieren von Text und Symbolen definiert.
- `ShellTitleColor` vom Typ `Color`, eine angefügte Eigenschaft, die die Farbe für den Titel der aktuellen Seite definiert.
- `ShellUnselectedColor` vom Typ `Color`, eine angefügte Eigenschaft, die die Farbe für nicht ausgewählten Text und Symbole im Shell-Chrom definiert.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

Darüber hinaus können diese Eigenschaften mithilfe von Cascading Stylesheets (CSS) festgelegt werden. Weitere Informationen finden Sie unter [Spezifische Eigenschaften der Xamarin.Forms-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="shell-content-layout"></a>Shell-Inhaltslayout

Die `Shell`-Klasse definiert die folgenden Eigenschaften, die das Layout einer Shell-Anwendung beeinflussen:

- `NavBarIsVisible` vom Typ`boolean`, eine angefügte Eigenschaft, die definiert, ob die Navigationsleiste eingeblendet werden soll, wenn eine Seite angezeigt wird. Diese Eigenschaft sollte auf einer Seite festgelegt werden, und der Standardwert ist `true`.
- `SetPaddingInsets` vom Typ `bool`, eine angefügte Eigenschaft, die steuert, ob Seiteninhalt unter jede Shell-Chrom fließt. Diese Eigenschaft sollte auf einer Seite festgelegt werden, und der Standardwert ist `false`.
- `TabBarIsVisible` vom Typ `bool`, eine angefügte Eigenschaft, die definiert, ob die Registerkartenleiste sichtbar sein soll, wenn eine Seite angezeigt wird. Diese Eigenschaft sollte auf einer Seite festgelegt werden, und der Standardwert ist `true`.
- `TitleView` vom Typ `View`, eine angefügte Eigenschaft, die die `TitleView` für eine Seite definiert. Diese Eigenschaft sollte auf einer Seite festgelegt werden.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
- [Spezifische Eigenschaften der Xamarin.Forms-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
