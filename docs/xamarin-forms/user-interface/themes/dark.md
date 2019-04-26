---
title: Xamarin.Forms Dunkles Design
description: In diesem Artikel wird erläutert, wie das Xamarin.Forms-dunkle Design in einer app genutzt werden.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61186337"
---
# <a name="xamarinforms-dark-theme"></a>Xamarin.Forms Dunkles Design

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschauversion")

> [!NOTE]
> Designs erfordern die Preview-Version von Xamarin.Forms 2.3. Überprüfen Sie die [Tipps zur Problembehandlung](~/xamarin-forms/user-interface/themes/index.md) , wenn Fehler auftreten.

So verwenden Sie das Design "dunkel"

## <a name="1-add-nuget-packages"></a>1. Nuget-Pakete hinzufügen

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Das Ressourcenverzeichnis hinzufügen

In der **"App.xaml"** Datei hinzufügen, einen neuen benutzerdefinierter `xmlns` für das Design, und vergewissern Sie sich das Design Ressourcen mit Ressourcenverzeichnis der Anwendung zusammengeführt werden.
Eine Beispiel-XAML-Datei ist unten dargestellt:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Laden von designklassen

Gehen Sie folgendermaßen vor [Problembehandlung – Schritt](~/xamarin-forms/user-interface/themes/index.md) und fügen Sie den erforderlichen Code in den IOS- und Android-Anwendungsprojekten.

## <a name="4-use-styleclass"></a>4. Verwenden Sie StyleClass

Hier ist ein Beispiel für Schaltflächen und Bezeichnungen im dunklen Design, zusammen mit das Markup, das sie erstellt.

[![](dark-images/dark-theme-sml.png "Schaltflächen und Bezeichnungen im dunklen Design")](dark-images/dark-theme.png#lightbox "Schaltflächen und Bezeichnungen in das Design \"dunkel\"")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

Die [vollständige Liste der integrierten Klassen](~/xamarin-forms/user-interface/themes/index.md) zeigt, welche Stile für einige häufig verwendete Steuerelemente verfügbar sind.
