---
title: Reagieren auf Systemdesign Änderungen in xamarin. Forms-Anwendungen
description: Xamarin. Forms-Anwendungen können auf Änderungen des Betriebssystem Designs reagieren, indem Sie den Typ "onapptheme" und die dynamikresource-Markup Erweiterung verwenden.
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: c524ac0809044e576a8d56561642f6c3bf2df4a4
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82533024"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>Reagieren auf Systemdesign Änderungen in xamarin. Forms-Anwendungen

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

Geräte enthalten in der Regel helle und dunkle Designs, die jeweils auf eine Vielzahl von Darstellungs Einstellungen verweisen, die auf Betriebssystemebene festgelegt werden können. Anwendungen sollten diese Systemdesigns berücksichtigen und sofort reagieren, wenn das Systemdesign geändert wird.

Abhängig von der Gerätekonfiguration kann sich das Systemdesign aus verschiedenen Gründen ändern. Dies schließt das Systemdesign ein, das explizit vom Benutzer geändert wird, da es sich aufgrund der Tageszeit geändert hat und sich aufgrund von Umgebungsfaktoren (z. b. niedriger Beleuchtung) ändert.

Xamarin. Forms-Anwendungen können auf Systemdesign Änderungen reagieren, indem Sie Ressourcen `AppThemeColor` mit der- `OnAppTheme<T>` Klasse, der- `OnAppTheme` Klasse und der-Markup Erweiterung definieren. Diese Ressourcen sollten dann mit der `DynamicResource` Markup Erweiterung genutzt werden.

> [!IMPORTANT]
> Die Reaktion auf eine Systemdesign Änderung ist derzeit experimentell und kann nur verwendet werden, indem das `AppTheme_Experimental` -Flag festgelegt wird. Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).

Die folgenden Anforderungen müssen erfüllt sein, damit xamarin. Forms auf eine Systemdesign Änderung antwortet:

- Xamarin. Forms 4,6 oder höher.
- IOS 13 oder höher.
- Android 10 (API 29) oder höher.
- UWP-Build 14393 oder höher.

Die folgenden Screenshots zeigen Design Seiten für helle und dunkle Systemdesigns unter IOS und Android:

[![Screenshot der Hauptseite einer Designs-App auf IOS-und Android](system-theme-changes-images/main-page-both-themes.png "Hauptseite der App "Thema"")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "Hauptseite der App "Thema"")
-Bildschirm Abbildung der[![Detailseite einer APP mit einem Thema unter IOS und Android](system-theme-changes-images/detail-page-both-themes.png "Detail Seite der APP mit Design")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "Detail Seite der APP mit Design")

## <a name="define-and-consume-theme-resources"></a>Definieren und Verwenden von Design Ressourcen

Ressourcen für helle und dunkle Designs können mit der `AppThemeColor` -Klasse, der `OnAppTheme<T>` -Klasse und der `OnAppTheme` -Markup Erweiterung definiert werden. Bei jedem Ansatz werden diese Ressourcen automatisch basierend auf dem Wert des aktuellen Systemdesigns angewendet. Außerdem werden Objekte, die diese Ressourcen nutzen, automatisch aktualisiert, wenn sich das Systemdesign ändert, während eine app ausgeführt wird.

### <a name="appthemecolor"></a>Appmecolor

Die `AppThemeColor` -Klasse wird verwendet, [`Color`](xref:Xamarin.Forms.Color) um Ressourcen für die hellen und dunklen Systemdesigns zu definieren. `AppThemeColor`Ressourcen sollten in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)definiert werden:

```xaml
<Application ...>
    <Application.Resources>
        <AppThemeColor x:Key="PageBackgroundColor"
                       Light="White"
                       Dark="Black" />
        <AppThemeColor x:Key="NavigationBarColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="PrimaryColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="SecondaryColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="PrimaryTextColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="SecondaryTextColor"
                       Light="White"
                       Dark="White" />
        <AppThemeColor x:Key="TertiaryTextColor"
                       Light="Gray"
                       Dark="WhiteSmoke" />
        <AppThemeColor x:Key="TransparentColor"
                       Light="Transparent"
                       Dark="Transparent" />
    </Application.Resources>
</Application>
```

Jede `AppThemeColor` Ressource muss über ein `x:Key` -Attribut verfügen, das ihr einen beschreibenden Schlüssel [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)in der übergibt. Der Wert der Eigenschaften `Light` und `Dark` sollte-Objekte [`Color`](xref:Xamarin.Forms.Color) sein. Außerdem kann eine `Default` Eigenschaft auf einen `Color` festgelegt werden, der standardmäßig vom Consumerobjekt verwendet werden soll.

`AppThemeColor`Ressourcen können Inline genutzt werden:

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

Alternativ können `AppThemeColor` Ressourcen von impliziten oder expliziten [`Style`](xref:Xamarin.Forms.Style) Objekten genutzt werden:

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor`Ressourcen sollten mit der `DynamicResource` Markup Erweiterung genutzt werden. Dadurch wird sichergestellt, dass die Darstellung des nutzenden Objekts aktualisiert wird, wenn das Systemdesign geändert wird.

### <a name="onappthemelttgt"></a>Onapptheme&lt;T&gt;

Die `OnAppTheme<T>` -Klasse wird verwendet, um Ressourcen eines beliebigen Typs für die hellen und dunklen Systemdesigns zu definieren. `OnAppTheme<T>`Ressourcen sollten in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)definiert werden, wobei das `T` Argument als Wert des- `x:TypeArguments` Attributs angegeben ist:

```xaml
<Application ...>
    <Application.Resources>
        <OnAppTheme x:Key="ImageLogo"
                    x:TypeArguments="FileImageSource"
                    Light="lightlogo.png"
                    Dark="darklogo.png" />
    </Application.Resources>
</Application>
```

Jede `OnAppTheme<T>` Ressource muss über ein `x:Key` -Attribut verfügen, das ihr einen beschreibenden Schlüssel [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)in der übergibt. Der Wert der Eigenschaften `Light` und `Dark` sollte Objekte des Typs sein, der als- `x:TypeArguments` Attribut definiert ist. Außerdem kann eine `Default` Eigenschaft auf ein Objekt vom Typ `T` festgelegt werden, das standardmäßig vom Consumerobjekt verwendet werden soll.

`OnAppTheme<T>`Ressourcen können Inline genutzt werden:

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

Alternativ können `OnAppTheme<T>` Ressourcen von impliziten oder expliziten [`Style`](xref:Xamarin.Forms.Style) Objekten genutzt werden:

```xaml
<Style x:Key="imageLogoStyle"
       TargetType="Image">
    <Setter Property="Source"
            Value="{DynamicResource ImageLogo}" />
    <Setter Property="Aspect"
            Value="AspectFit" />
</Style>
```

> [!IMPORTANT]
> `OnAppTheme<T>`Ressourcen sollten mit der `DynamicResource` Markup Erweiterung genutzt werden. Dadurch wird sichergestellt, dass die Darstellung des nutzenden Objekts aktualisiert wird, wenn das Systemdesign geändert wird.

### <a name="onapptheme-markup-extension"></a>Onapptheme-Markup Erweiterung

Die `OnAppTheme` Markup Erweiterung ermöglicht es Ihnen, eine zu verwendende Ressource anzugeben, z. b. ein Bild oder eine Farbe, basierend auf dem aktuellen Systemdesign. Sie bietet die gleiche Funktionalität wie die `OnAppTheme<T>` -Klasse, bietet aber eine präzisere Darstellung:

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die Textfarbe der ersten [`Label`](xref:Xamarin.Forms.Label) auf grün festgelegt, wenn das Gerät das helle Design verwendet, und wird auf Rot festgelegt, wenn das Gerät das dunkle Design verwendet. Entsprechend [`Image`](xref:Xamarin.Forms.Image) zeigt eine andere Bilddatei an, die auf dem aktuellen Systemdesign basiert.

Weitere Informationen zur `OnAppTheme` Markup Erweiterung finden Sie unter [onapptheme Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension).

## <a name="detect-the-current-system-theme"></a>Aktuelles Systemdesign erkennen

Das aktuelle Systemdesign kann erkannt werden, indem Sie den Wert der `Application.RequestedTheme` -Eigenschaft erhalten:

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

Die `RequestedTheme` -Eigenschaft gibt `OSAppTheme` einen Enumerationsmember zurück. Die `OSAppTheme`-Enumeration definiert die folgenden Member:

- `Unspecified`Gibt an, dass für das Gerät ein nicht bestimmtes Design verwendet wird.
- `Light`Gibt an, dass das helle Design des Geräts verwendet wird.
- `Dark`Gibt an, dass das Gerät das dunkle Design verwendet.

## <a name="react-to-theme-changes"></a>Auf Designänderungen reagieren

Das Systemdesign auf einem Gerät kann sich aus verschiedenen Gründen ändern, je nachdem, wie das Gerät konfiguriert ist. Xamarin. Forms-Apps können benachrichtigt werden, wenn sich das Systemdesign ändert `Application.RequestedThemeChanged` , indem das-Ereignis behandelt wird:

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

Das `AppThemeChangedEventArgs` -Objekt, das das `RequestedThemeChanged` -Ereignis begleitet, verfügt über eine `RequestedTheme`einzelne-Eigenschaft `OSAppTheme`mit dem Namen vom Typ. Diese Eigenschaft kann untersucht werden, um das angeforderte Systemdesign zu erkennen.

## <a name="related-links"></a>Verwandte Links

- [Systemdesigns (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [Onapptheme-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile in xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
