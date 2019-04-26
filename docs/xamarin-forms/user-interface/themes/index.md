---
title: Xamarin.Forms-Designs
description: Dieser Artikel enthält die Xamarin.Forms-Designs, die bestimmte visuelle Darstellungen für Standardansichten definieren.
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1c5b2635dca6aa74fd0dfb92d7e62e6da3140538
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60902839"
---
# <a name="xamarinforms-themes"></a>Xamarin.Forms-Designs

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschauversion")

Xamarin.Forms-Designs wurden bei der Evolve 2016 angekündigt und stehen als Vorschau für Kunden, ausprobieren und Feedback geben.

Ein Design wird zu einer Xamarin.Forms-Anwendung hinzugefügt, durch Einschließen der **Xamarin.Forms.Theme.Base** Nuget-Paket, sowie ein zusätzliches Paket, das definiert ein bestimmtes Design (z. b. Xamarin.Forms.Theme.Light) oder anderen lokalen Design kann für die Anwendung definiert werden.

Finden Sie in der [Design "hell"](light.md) und [Design "dunkel"](dark.md) Seiten für Anweisungen zum Hinzufügen einer app, oder sehen Sie sich die [Beispiel benutzerdefiniertes Design](custom.md).

**WICHTIG:** Sie sollten auch die Schritte zum [Design-Assemblys (siehe unten) laden](#loadtheme) durch Hinzufügen von Bausteincode für den iOS- `AppDelegate` und Android `MainActivity`. Dies wird in einer kommenden Vorschau-Version verbessert werden.


## <a name="control-appearance"></a>Steuerelementdarstellung

Die [Licht](light.md) und [dunkel](dark.md) Designs, die beide definieren eine bestimme visuelle darstellen, für die standard-Steuerelemente. Nachdem Sie ein Design zu Ressourcenverzeichnis der Anwendung hinzugefügt haben, ändert sich die Darstellung der Standardsteuerelemente.

Das folgende XAML-Markup zeigt einige allgemeine Steuerelemente:

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Diese Screenshots zeigen die Steuerelemente mit:

* Kein Design angewendet
* Design "hell" (nur geringfügige Unterschiede, dass kein Design)
* Dunkles Design

![](images/standard-none-sml.png "Steuerelemente ohne Designumgebung") ![](images/standard-light-sml.png "Steuerelemente mit Design \"hell\"") ![](images/standard-dark-sml.png "Steuerelemente mit Design \"dunkel\"")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

Die `StyleClass` Eigenschaft können Sie eine Ansicht des Darstellung entsprechend einer Definition bereitgestellt werden, indem Sie ein Design geändert werden.

Die [Licht](light.md) und [dunkel](dark.md) Designs, die beide definieren drei unterschiedliche Darstellungen für einen `BoxView`: `HorizontalRule`, `Circle`, und `Rounded`. Dieses Markup zeigt drei verschiedene `BoxView`mit anderen Stil Klassen angewendet:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Dieses Licht und dunkel wird wie folgt gerendert:

![](images/boxview-light-sml.png "Mit einem Design \"hell\" StyleClass BoxView") ![](images/boxview-dark-sml.png "BoxView mit einem StyleClass Design \"dunkel\"")

<a name="builtin" />

## <a name="built-in-classes"></a>Integrierte Klassen

Zusätzlich zu automatisch formatieren der Standardsteuerelemente des Lichts und dunklen Designs unterstützen derzeit die folgenden Klassen, die durch die Einstellung angewendet werden können die `StyleClass` für diese Steuerelemente:

**BoxView**

* HorizontalRule
* Kreis
* Gerundet

**Image**

* Kreis
* Gerundet
* Miniaturansicht

**Button** (Schaltfläche)

* Standard
* Primär
* Erfolgreich
* Info
* Warnung
* Gefahr
* Link
* Kleine
* Große

**Bezeichnung**

* Header
* Untertitel
* Text
* Link
* Inverse


## <a name="troubleshooting"></a>Problembehandlung

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Datei oder Assembly 'Xamarin.Forms.Theme.Light' oder eine ihrer Abhängigkeiten konnte nicht geladen werden

In der Vorschauversion von Designs zur Laufzeit laden können möglicherweise nicht. Fügen Sie den folgenden Code in den relevanten Projekten, die diesen Fehler zu beheben.

**iOS**

In der **Datei "appdelegate.cs"** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

In der **"mainactivity.cs"** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```


## <a name="related-links"></a>Verwandte Links

- [ThemesDemo-Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
