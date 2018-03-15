---
title: Designs
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 12741b4756d26f4090613127d143380e1c4fb55a
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="themes"></a>Designs

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschau verfügbar")

Xamarin.Forms Designs auf Evolve 2016 angekündigt wurden und als Vorschau für Kunden, um zu testen und Bereitstellen von Feedback verfügbar sind.

Ein Design wird zu einer Xamarin.Forms-Anwendung hinzugefügt, dazu den **Xamarin.Forms.Theme.Base** Nuget-Paket sowie ein zusätzliches Paket, das definiert ein bestimmtes Design (z. b. Xamarin.Forms.Theme.Light) oder andere kann ein lokaler Design definiert werden, für die Anwendung.

Finden Sie in der [Design "hell"](light.md) und [Design "dunkel"](dark.md) Seiten für Anweisungen zum Hinzufügen einer app, oder sehen Sie sich die [Beispiel benutzerdefiniertes Design](custom.md).

**Wichtig:** befolgen Sie die Schritte zum auch [Design-Assemblys (siehe unten) laden](#loadtheme) durch Hinzufügen von einigen Standardcode für den iOS- `AppDelegate` und Android `MainActivity`. Dies wird in zukünftigen Preview-Version verbessert werden.


## <a name="control-appearance"></a>Steuerelementdarstellung

Die [Licht](light.md) und [dunkel](dark.md) Designs beide definieren ein bestimmtes Erscheinungsbild für Standardsteuerelemente. Nachdem Sie ein Design Ressourcenwörterbuch für die Anwendung hinzugefügt haben, ändert sich die Darstellung der Standardsteuerelemente.

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

Diese Screenshots zeigen diese Steuerelemente mit:

* Kein Design angewendet
* Design "hell" (nur geringfügige Unterschiede, sodass kein Design)
* Dunkles Design

![](images/standard-none-sml.png "Steuerelemente ohne Designumgebung") ![ ] (images/standard-light-sml.png "Steuerelemente mit Design "hell"") ![ ] (images/standard-dark-sml.png "Steuerelemente mit Design "dunkel"")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

Die `StyleClass` -Eigenschaft kann eine Sicht Darstellung entsprechend einer Definition bereitgestellt werden, indem Sie ein Design geändert werden.

Die [Licht](light.md) und [dunkel](dark.md) Designs beide definieren drei verschiedenen eindeutigkeitsmetrik für eine `BoxView`: `HorizontalRule`, `Circle`, und `Rounded`. Dieses Markup zeigt drei verschiedene `BoxView`mit verschiedenen Formatklassen angewendet:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Dies rendert mit helle und dunkle wie folgt:

![](images/boxview-light-sml.png "Mit einem Design "hell" StyleClass BoxView") ![ ] (images/boxview-dark-sml.png "BoxView mit einem StyleClass Design "dunkel"")

<a name="builtin" />

## <a name="built-in-classes"></a>Integrierte Klassen

Zusätzlich zu den automatisch formatieren die Standardsteuerelemente des Lichts und dunkle Designs unterstützt derzeit die folgenden Klassen, die von der Einstellung angewendet werden können die `StyleClass` auf diese Steuerelemente:

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

In der Vorschauversion Designs zur Laufzeit laden möglicherweise nicht. Fügen Sie Code unten in der relevanten Projekte, um diesen Fehler zu beheben.

**iOS**

In der **AppDelegate.cs** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

In der **MainActivity.cs** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```


## <a name="related-links"></a>Verwandte Links

- [ThemesDemo-Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
