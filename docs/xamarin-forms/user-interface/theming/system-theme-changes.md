---
title: Reagieren auf Änderungen des Systemdesigns in Xamarin.Forms Anwendungen
description: Xamarin.FormsAnwendungen können auf Änderungen des Betriebssystem Designs reagieren, indem Sie den Typ "onapptheme" und die dynamikresource-Markup Erweiterung verwenden.
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 28bcbed3a03a2abbec42a619062579419a3063a4
ms.sourcegitcommit: 8a18471b3d96f3f726b66f9bc50a829f1c122f29
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988207"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>Reagieren auf Änderungen des Systemdesigns in Xamarin.Forms Anwendungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

Geräte enthalten in der Regel helle und dunkle Designs, die jeweils auf eine Vielzahl von Darstellungs Einstellungen verweisen, die auf Betriebssystemebene festgelegt werden können. Anwendungen sollten diese Systemdesigns berücksichtigen und sofort reagieren, wenn das Systemdesign geändert wird.

Abhängig von der Gerätekonfiguration kann sich das Systemdesign aus verschiedenen Gründen ändern. Dies schließt das Systemdesign ein, das explizit vom Benutzer geändert wird, da es sich aufgrund der Tageszeit geändert hat und sich aufgrund von Umgebungsfaktoren (z. b. niedriger Beleuchtung) ändert.

Xamarin.FormsAnwendungen können auf Systemdesign Änderungen reagieren, indem Sie Ressourcen mit der `AppThemeBinding` Markup Erweiterung und die `SetAppThemeColor` -und- `SetOnAppTheme<T>` Erweiterungs Methoden nutzen.

> [!IMPORTANT]
> Die Reaktion auf eine Systemdesign Änderung ist derzeit experimentell und kann nur verwendet werden, indem das-Flag festgelegt wird `AppTheme_Experimental` . Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).

Die folgenden Anforderungen müssen erfüllt sein, damit auf Xamarin.Forms eine Systemdesign Änderung reagiert:

- Xamarin.Forms4.6.0.967 oder höher.
- IOS 13 oder höher.
- Android 10 (API 29) oder höher.
- UWP-Build 14393 oder höher.

Die folgenden Screenshots zeigen Design Seiten für helle und dunkle Systemdesigns unter IOS und Android:

[![Screenshot der Hauptseite einer APP mit einem Thema unter IOS und Android](system-theme-changes-images/main-page-both-themes.png "Hauptseite der App "Thema"")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "Hauptseite der App "Thema"") 
 [ ![Screenshot der Detailseite einer Designs-app unter IOS und Android](system-theme-changes-images/detail-page-both-themes.png "Detail Seite der APP mit Design")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "Detail Seite der APP mit Design")

## <a name="define-and-consume-theme-resources"></a>Definieren und Verwenden von Design Ressourcen

Ressourcen für helle und dunkle Designs können mit der `AppThemeBinding` Markup Erweiterung und den `SetAppThemeColor` Erweiterungs Methoden und verwendet werden `SetOnAppTheme<T>` . Mit diesen Ansätzen werden Ressourcen automatisch basierend auf dem Wert des aktuellen Systemdesigns angewendet. Außerdem werden Objekte, die diese Ressourcen nutzen, automatisch aktualisiert, wenn sich das Systemdesign ändert, während eine app ausgeführt wird.

### <a name="appthemebinding-markup-extension"></a>Apptomebinding-Markup Erweiterung

Die `AppThemeBinding` Markup Erweiterung ermöglicht es Ihnen, basierend auf dem aktuellen Systemdesign eine Ressource, z. b. ein Bild oder eine Farbe, zu verwenden:

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{AppThemeBinding Light=Green, Dark=Red}" />
        <Image Source="{AppThemeBinding Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die Textfarbe der ersten [`Label`](xref:Xamarin.Forms.Label) auf grün festgelegt, wenn das Gerät das helle Design verwendet, und wird auf Rot festgelegt, wenn das Gerät das dunkle Design verwendet. Entsprechend [`Image`](xref:Xamarin.Forms.Image) zeigt eine andere Bilddatei an, die auf dem aktuellen Systemdesign basiert.

Außerdem können in einem definierte Ressourcen [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) mit der Markup Erweiterung genutzt werden `StaticResource` :

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Light colors -->
        <Color x:Key="LightPrimaryColor">WhiteSmoke</Color>
        <Color x:Key="LightSecondaryColor">Black</Color>

        <!-- Dark colors -->
        <Color x:Key="DarkPrimaryColor">Teal</Color>
        <Color x:Key="DarkSecondaryColor">White</Color>

        <Style x:Key="ButtonStyle"
               TargetType="Button">
            <Setter Property="BackgroundColor"
                    Value="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}" />
            <Setter Property="TextColor"
                    Value="{AppThemeBinding Light={StaticResource LightSecondaryColor}, Dark={StaticResource DarkSecondaryColor}}" />
        </Style>

    </ContentPage.Resources>

    <Grid BackgroundColor="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}">
      <Button Text="MORE INFO"
              Style="{StaticResource ButtonStyle}" />
    </Grid>    
</ContentPage>    
```

In diesem Beispiel ändert sich die Hintergrundfarbe der [`Grid`](xref:Xamarin.Forms.Grid) und der Stil, je nachdem, [`Button`](xref:Xamarin.Forms.Button) ob das Gerät das helle Design oder das dunkle Design verwendet.

Weitere Informationen zur `AppThemeBinding` Markup Erweiterung finden Sie unter [appdermebinding-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension).

### <a name="extension-methods"></a>Erweiterungsmethoden

Xamarin.Formsschließt `SetAppThemeColor` -und- `SetOnAppTheme<T>` Erweiterungs Methoden ein, mit denen [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekte auf Systemdesign Änderungen reagieren können.

Die- `SetAppThemeColor` Methode ermöglicht das [`Color`](xref:Xamarin.Forms.Color) Angeben von-Objekten, die basierend auf dem aktuellen Systemdesign für eine Ziel Eigenschaft festgelegt werden:

```csharp
Label label = new Label();
label.SetAppThemeColor(Label.TextColorProperty, Color.Green, Color.Red);
```

In diesem Beispiel wird die Textfarbe der [`Label`](xref:Xamarin.Forms.Label) auf grün festgelegt, wenn das Gerät das helle Design verwendet, und wird auf Rot festgelegt, wenn das Gerät das dunkle Design verwendet.

Die- `SetOnAppTheme<T>` Methode ermöglicht die Angabe von Objekten des Typs `T` , die basierend auf dem aktuellen Systemdesign für eine Ziel Eigenschaft festgelegt werden:

```csharp
Image image = new Image();
image.SetOnAppTheme<FileImageSource>(Image.SourceProperty, "lightlogo.png", "darklogo.png");
```

In diesem Beispiel wird [`Image`](xref:Xamarin.Forms.Image) angezeigt, `lightlogo.png` Wenn das Gerät das helle Design verwendet und `darklogo.png` Wenn das Gerät das dunkle Design verwendet.

## <a name="detect-the-current-system-theme"></a>Aktuelles Systemdesign erkennen

Das aktuelle Systemdesign kann erkannt werden, indem Sie den Wert der- `Application.RequestedTheme` Eigenschaft erhalten:

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

Die- `RequestedTheme` Eigenschaft gibt einen `OSAppTheme` Enumerationsmember zurück. Die `OSAppTheme`-Enumeration definiert die folgenden Member:

- `Unspecified`Gibt an, dass für das Gerät ein nicht bestimmtes Design verwendet wird.
- `Light`Gibt an, dass das helle Design des Geräts verwendet wird.
- `Dark`Gibt an, dass das Gerät das dunkle Design verwendet.

## <a name="set-the-current-user-theme"></a>Aktuelles Benutzer Design festlegen

Das Design, das von der Anwendung verwendet wird, kann mit der-Eigenschaft festgelegt werden `Application.UserAppTheme` , die vom Typ ist `OSAppTheme` , unabhängig davon, welches Systemdesign zurzeit betriebsbereit ist:

```csharp
Application.Current.UserAppTheme = OSAppTheme.Dark;
```

In diesem Beispiel wird die Anwendung so festgelegt, dass Sie das Design verwendet, das für den dunklen System Modus definiert ist, und zwar unabhängig davon, welches Systemdesign derzeit betriebsbereit ist.

> [!NOTE]
> Legen Sie die-Eigenschaft auf fest, um `UserAppTheme` `OSAppTheme.Unspecified` das Betriebssystem standardmäßig zu übersetzen.

## <a name="react-to-theme-changes"></a>Auf Designänderungen reagieren

Das Systemdesign auf einem Gerät kann sich aus verschiedenen Gründen ändern, je nachdem, wie das Gerät konfiguriert ist. Xamarin.FormsApps können benachrichtigt werden, wenn sich das Systemdesign ändert, indem das-Ereignis behandelt wird `Application.RequestedThemeChanged` :

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

Das- `AppThemeChangedEventArgs` Objekt, das das- `RequestedThemeChanged` Ereignis begleitet, verfügt über eine einzelne-Eigenschaft mit dem Namen `RequestedTheme` vom Typ `OSAppTheme` . Diese Eigenschaft kann untersucht werden, um das angeforderte Systemdesign zu erkennen.

## <a name="related-links"></a>Verwandte Links

- [Systemdesigns (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [Apptomebinding-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension)
- [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
