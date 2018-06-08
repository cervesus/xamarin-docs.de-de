---
title: Lokalisierung von rechts nach links
description: Lokalisierung von rechts nach links bietet Unterstützung für rechts-nach-links-flussrichtung Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 94cf937a810e84ead0bdc1f69933968b42090a6f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846431"
---
# <a name="right-to-left-localization"></a>Lokalisierung von rechts nach links

_Lokalisierung von rechts nach links bietet Unterstützung für rechts-nach-links-flussrichtung Xamarin.Forms-Anwendungen._

> [!NOTE]
> Rechts-nach-links-Lokalisierung erfordert die Verwendung von iOS 9 oder höher und API 17 oder höher auf Android-Geräten.

Flussrichtung ist die Richtung, in der die Elemente der Benutzeroberfläche auf der Seite vom Auge gescannt werden. Einige Sprachen wie Arabisch und Hebräisch, erfordern, dass Elemente der Benutzeroberfläche in eine flussrichtung von rechts nach links angeordnet sind. Dies kann durch Festlegen der [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft. Diese Eigenschaft ruft ab oder legt die Richtung, in der UI-Elemente-Fluss innerhalb von übergeordneten Elementen, die ihr Layout steuert und sollte auf eine der festgelegt werden die [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) Enumerationswerte:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Festlegen der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) für ein Element in der Regel legt die Ausrichtung fest, das Recht, die rechts-nach-Links-Lesefolge und das Layout des Steuerelements von übertragen werden rechts-nach-links:

[![Arabisch, mit einem rechts-nach-links-flussrichtung TodoItemPage](rtl-images/TodoItemPage-Arabic.png "TodoItemPage in Arabisch mit einem rechts-nach-links-flussrichtung")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage in Arabisch mit einem rechts-nach-links-flussrichtung")

> [!TIP]
> Sie sollten nur Festlegen der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) -Eigenschaft für das anfängliche Layout. Durch Ändern dieses Werts zur Laufzeit führt eine teure Layout, die Leistung auswirkt.

Die Standardeinstellung [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaftswert für ein Element ohne übergeordnetes Element ist [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), dagegen die Standardeinstellung `FlowDirection` für ein mit einem übergeordneten Element [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Daher erbt er ein Element der `FlowDirection` Eigenschaftswert aus dem übergeordneten Element in der visuellen Struktur und jedes Element kann den Wert Überschreiben von seinem übergeordneten Element abgerufen.

> [!TIP]
> Wenn eine app für rechts-nach-links-Sprachen lokalisieren möchten, legen die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft auf eine Seite oder Root-Layout. Dies bewirkt, dass alle Elemente innerhalb der Seite oder die Stammlayout So reagieren Sie auf die flussrichtung entsprechend.

## <a name="respecting-device-flow-direction"></a>Ressourcenbezogene Gerät flussrichtung

Das Gerät flussrichtung ressourcenbezogene basierend auf der ausgewählten Sprache und Region ist eine explizite Developer-Auswahl und erfolgt nicht automatisch. Kann erreicht werden, durch Festlegen der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft auf einer Seite oder die Stammlayout auf die `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) Wert:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Alle untergeordneten Elemente auf der Seite oder Root-Layout werden dann standardmäßig erben die [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) Wert.

## <a name="platform-setup"></a>Platform-setup

Bestimmte Plattform-Setup ist erforderlich, um die rechts-nach-links-Gebietsschemas zu aktivieren.

### <a name="ios"></a>iOS

Das erforderliche rechts-nach-links-Gebietsschema sollte als einer unterstützten Sprache hinzugefügt werden, auf die Arrayelemente für die `CFBundleLocalizations` -Schlüssel in **"Info.plist"**. Das folgende Beispiel zeigt Arabisch hinzugefügt, um das Array nach den `CFBundleLocalizations` Schlüssel:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Datei "Info.plist" unterstützten Sprachen](rtl-images/ios-locales.png "\"Info.plist\" unterstützte Sprachen")

Weitere Informationen finden Sie unter [Lokalisierung-Grundlagen in iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Rechts-nach-links-Lokalisierung kann dann durch Ändern von Sprache und Region auf der Gerätesimulator in einem rechts-nach-links-Gebietsschema, das im angegebenen getestet werden **"Info.plist"**.

> [!WARNING]
> Bitte beachten Sie, dass bei der alle Ändern von Sprache und Region in einem rechts-nach-links-Gebietsschema auf iOS, [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) Ansichten löst eine Ausnahme aus, wenn Sie nicht die erforderlichen Ressourcen für das Gebietsschema verwenden. Beispielsweise, wenn eine app in Arabisch zu testen, die hat eine `DatePicker`, sicher, dass **Naher Osten** ausgewählt ist, der **Internationalisierung** im Abschnitt der **iOS-Build** Bereich.

### <a name="android"></a>Android

Der app **AndroidManifest.xml** Datei aktualisiert werden soll, damit die `<uses-sdk>` Knotengruppen der `android:minSdkVersion` -Attribut auf 17, und die `<application>` Knotengruppen der `android:supportsRtl` -Attribut auf `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Rechts-nach-links-Lokalisierung kann dann getestet werden, durch Ändern der Geräteemulator um die rechts-nach-links-Sprache verwenden, oder aktivieren **Force RTL layoutausrichtung** in **Einstellungen > Entwickleroptionen**.

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Die erforderlichen Sprachressourcen sollte angegeben werden, der `<Resources>` Knoten der **"Package.appxmanifest"** Datei. Das folgende Beispiel zeigt Arabisch hinzugefügt, um die `<Resources>` Knoten:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Darüber hinaus erfordert universelle Windows-Plattform an, dass die Standardkultur der app in der Standardbibliothek .NET explizit definiert ist. Dies geschieht durch Festlegen der `NeutralResourcesLanguage` -Attribut im `AssemblyInfo.cs`, oder in einer anderen Klasse, um die Standardkultur:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Rechts-nach-links-Lokalisierung kann dann durch Ändern von Sprache und Region auf dem Gerät zur entsprechenden Gebietsschema-rechts-nach-links-getestet werden.

## <a name="limitations"></a>Einschränkungen

Xamarin.Forms rechts-nach-links-Lokalisierung weist derzeit einige Einschränkungen:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) Schaltfläche-Speicherort Symbolleiste-Element Speicherort aus, und Übergangsanimationen wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Wischen Richtung wird nicht gekippt.
- [`Image`](xref:Xamarin.Forms.Image) visuellen Inhalt wird nicht gekippt.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) und [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) Ausrichtung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`WebView`](xref:Xamarin.Forms.WebView) Inhalt berücksichtigt nicht die [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- Ein `TextDirection` -Eigenschaft muss hinzugefügt werden, um die Ausrichtung steuern.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) Ausrichtung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) textausrichtung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Gesten und Ausrichtung sind nicht umgekehrt.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Ausrichtung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Platzierung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) textausrichtung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft wird nicht von geerbt [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) untergeordneten Elemente.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) textausrichtung wird gesteuert, indem das Gebietsschema Gerät statt über das [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Unterstützung von rechts nach links Sprache mit Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0-rechts-nach-links-Unterstützung von [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [TodoLocalizedRTL-Beispiel-App](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
