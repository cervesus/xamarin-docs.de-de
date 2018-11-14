---
title: Rechts-nach-links-Lokalisierung
description: Rechts-nach-links-Lokalisierung bietet Unterstützung für flussrichtung von rechts-nach-Links zu Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 67b0d90290b18c7a5b55c5e3496b54970a8cfc38
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617604"
---
# <a name="right-to-left-localization"></a>Rechts-nach-links-Lokalisierung

_Rechts-nach-links-Lokalisierung bietet Unterstützung für flussrichtung von rechts-nach-Links zu Xamarin.Forms-Anwendungen._

> [!NOTE]
> Rechts-nach-links-Lokalisierung ist die Verwendung der API 17 oder höher unter Android und iOS 9 oder höher erforderlich.

Flussrichtung ist die Richtung, in der die Elemente der Benutzeroberfläche auf der Seite vom Auge gescannt werden. Bei einigen Sprachen wie Arabisch und Hebräisch, erfordern, dass UI-Elemente in einer flussrichtung von rechts-nach-links angeordnet werden. Dies kann erreicht werden, durch Festlegen der [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft. Diese Eigenschaft ruft ab oder legt die Richtung fest, in der UI-Elemente-Fluss innerhalb von übergeordneten Elementen, die steuert, der das Layouts und sollte auf eine der festgelegt werden die [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) -Enumerationswerte fest:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Festlegen der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) für ein Element in der Regel legt die Ausrichtung fest, das Recht, die rechts-nach-Links-Lesefolge und das Layout des Steuerelements aus rechts-nach-links:

[![TodoItemPage in Arabisch mit einer flussrichtung von rechts-nach-links-](rtl-images/TodoItemPage-Arabic.png "TodoItemPage in Arabisch mit einer flussrichtung von rechts-nach-links-")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage in Arabisch mit einer flussrichtung von rechts-nach-links")

> [!TIP]
> Sie sollten nur festlegen, die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft für das anfängliche Layout. Ändern diesen Wert zur Laufzeit bewirkt, dass eine teure Layoutvorgang, wird die Leistung auswirken.

Der Standardwert [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaftswert für ein Element ohne übergeordnetes Element ist [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), zwar standardmäßig `FlowDirection` für ein mit einem übergeordneten Element Element [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Aus diesem Grund ein Element erbt die `FlowDirection` Eigenschaftswert von seinem übergeordneten Element in der visuellen Struktur und ein Element kann den Wert von seinem übergeordneten Element wird überschreiben.

> [!TIP]
> Wenn eine app für rechts-nach-links-Sprachen lokalisieren möchten, legen die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft auf eine Seite oder Root-Layout. Dies bewirkt, dass alle Elemente innerhalb der Seite oder Root-Layout, um angemessen auf die flussrichtung reagieren.

## <a name="respecting-device-flow-direction"></a>Unter Beachtung der flussrichtung des Geräts

Unter Beachtung der flussrichtung des Geräts basierend auf der ausgewählten Sprache und Region ist eine explizite Developer-Option und wird nicht automatisch ausgeführt. Er erzielt werden, indem Sie die Einstellung der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) -Eigenschaft in einer Seite oder Root-Layout, auf die `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) Wert:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Alle untergeordneten Elemente auf der Seite oder Root-Layout werden dann standardmäßig erben die [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) Wert.

## <a name="platform-setup"></a>Plattform-setup

Bestimmte Plattform-Setup ist erforderlich, um rechts-nach-links-Gebietsschemas zu aktivieren.

### <a name="ios"></a>iOS

Das erforderliche rechts-nach-links-Gebietsschema sollten als einer unterstützten Sprache hinzugefügt werden, auf die Arrayelemente für die `CFBundleLocalizations` -Schlüssels im **"Info.plist"**. Das folgende Beispiel zeigt, dass wurde hinzugefügt, in das Array für Arabisch der `CFBundleLocalizations` Schlüssel:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Datei "Info.plist" unterstützt Sprachen](rtl-images/ios-locales.png "\"Info.plist\" unterstützte Sprachen")

Weitere Informationen finden Sie unter [Lokalisierung-Grundlagen in iOS](https://docs.microsoft.com/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Rechts-nach-links-Lokalisierung kann dann getestet werden, durch ändern die Sprache und Region auf der Gerätesimulator in eine rechts-nach-links-Gebietsschema, das im angegebenen **"Info.plist"**.

> [!WARNING]
> Beachten Sie, dass wenn Sie die Sprache und Region in eine rechts-nach-links-Gebietsschema unter iOS, alle [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) Ansichten werden eine Ausnahme ausgelöst, wenn Sie nicht die erforderlichen Ressourcen für das Gebietsschema verwenden. Z. B., wenn Sie eine app in Arabisch zu testen, die eine `DatePicker`, sicher, dass **Naher Osten** ausgewählt ist, der **Internationalisierung** im Abschnitt der **iOS-Build** Bereich.

### <a name="android"></a>Android

Der app **"androidmanifest.xml"** Datei aktualisiert werden soll, damit die `<uses-sdk>` Knotengruppen der `android:minSdkVersion` -Attribut auf 17, und die `<application>` Knotengruppen der `android:supportsRtl` Attribut `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Lokalisierung von rechts-nach-links klicken Sie dann getestet werden kann, indem Sie den Geräteemulator zum Verwenden der rechts-nach-links-Sprache ändern, oder durch aktivieren **Force RTL layoutausrichtung** in **Einstellungen > Entwickleroptionen**.

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Die Ressourcen für die erforderliche Sprache sollte angegeben werden, der `<Resources>` Knoten die **"Package.appxmanifest"** Datei. Das folgende Beispiel zeigt, Arabisch hinzugefügt, um die `<Resources>` Knoten:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Darüber hinaus erfordert UWP an, dass die Standardkultur der Anwendung explizit in der .NET Standard-Bibliothek definiert ist. Dies kann erreicht werden, durch Festlegen der `NeutralResourcesLanguage` -Attribut im `AssemblyInfo.cs`, oder in einer anderen Klasse, um die Standardkultur:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Rechts-nach-links-Lokalisierung kann dann getestet werden, ändern Sie die Sprache und Region auf dem Gerät zur entsprechenden Gebietsschema-rechts-nach-links.

## <a name="limitations"></a>Einschränkungen

Xamarin.Forms-rechts-nach-links-Lokalisierung verfügt derzeit über einige Einschränkungen:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) Speicherort der Schaltfläche, Symbolleiste Element Speicherort aus, und übergangsanimation wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) streichen Sie nach Richtung wird nicht gekippt.
- [`Image`](xref:Xamarin.Forms.Image) Visueller Inhalt wird nicht gekippt.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) und [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) Ausrichtung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`WebView`](xref:Xamarin.Forms.WebView) Inhalt berücksichtigt nicht die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- Ein `TextDirection` -Eigenschaft muss hinzugefügt werden, um die textausrichtung zu steuern.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) Ausrichtung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) textausrichtung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Gesten und Ausrichtung werden nicht rückgängig gemacht.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Ausrichtung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Platzierung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) textausrichtung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft wird nicht von geerbt [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) untergeordneten Elemente.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) textausrichtung wird gesteuert, durch das Gebietsschema des Geräts, anstatt die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Unterstützung von rechts nach links Sprache mit Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms-3.0-rechts-nach-Links zu unterstützen, indem [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [TodoLocalizedRTL-Beispiel-App](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
