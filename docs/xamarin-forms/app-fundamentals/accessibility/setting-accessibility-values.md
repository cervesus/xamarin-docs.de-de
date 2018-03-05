---
title: "Festlegen von Eingabehilfen-Werte für Elemente der Benutzeroberfläche"
description: "Xamarin.Forms ermöglicht Zugriff auf Werte auf die Elemente der Benutzeroberfläche mithilfe von angefügten Eigenschaften aus der AutomationProperties-Klasse, die wiederum von einheitlichen Zugriff auf die Werte festgelegt werden. In diesem Artikel wird erläutert, wie die Klasse AutomationProperties verwendet, damit eine Bildschirmsprachausgabe zu den Elementen auf der Seite äußern kann."
ms.topic: article
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 4aeeea7f946a121b12741d2da217daf531935849
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Festlegen von Eingabehilfen-Werte für Elemente der Benutzeroberfläche

_Xamarin.Forms ermöglicht Zugriff auf Werte auf die Elemente der Benutzeroberfläche mithilfe von angefügten Eigenschaften aus der AutomationProperties-Klasse, die wiederum von einheitlichen Zugriff auf die Werte festgelegt werden. In diesem Artikel wird erläutert, wie die Klasse AutomationProperties verwendet, damit eine Bildschirmsprachausgabe zu den Elementen auf der Seite äußern kann._

## <a name="overview"></a>Übersicht

Xamarin.Forms ermöglicht Zugriff auf Werte für die Elemente der Benutzeroberfläche über die folgenden angefügten Eigenschaften festgelegt werden:

- `AutomationProperties.IsInAccessibleTree` – Gibt an, ob das Element für eine barrierefreie Anwendung verfügbar ist. Weitere Informationen finden Sie unter [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – eine kurze Beschreibung des Elements, das als Bezeichner für die Spracherkennung aktivieren für das Element dient. Weitere Informationen finden Sie unter [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – eine längere Beschreibung für das Element, das als QuickInfo-Text, die dem Element zugeordneten betrachtet werden kann. Weitere Informationen finden Sie unter [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – können Sie ein anderes Element aus, um Informationen über Eingabehilfen für das aktuelle Element zu definieren. Weitere Informationen finden Sie unter [AutomationProperties.LabeledBy](#labeledby).

Diese angefügte Eigenschaften Satz native Eingabehilfen-Werte, damit über das Element eine Sprachausgabe äußern kann. Weitere Informationen über angefügte Eigenschaften finden Sie unter [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Mithilfe der `AutomationProperties` angefügte Eigenschaften Ausführung des UI-Tests unter Android beeinträchtigt werden können. Die [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` und `AutomationProperties.HelpText` Eigenschaften werden beide festlegen den systemeigenen `ContentDescription` -Eigenschaft, mit der `AutomationProperties.Name` und `AutomationProperties.HelpText` Eigenschaftswerte Vorrang vor den `AutomationId`Wert (wenn beide `AutomationProperties.Name` und `AutomationProperties.HelpText` festgelegt ist, werden die Werte verkettet werden). Dies bedeutet, dass alle Tests, die Suche nach `AutomationId` schlägt fehl, wenn `AutomationProperties.Name` oder `AutomationProperties.HelpText` sind auch für das Element festlegen. In diesem Szenario Benutzeroberflächentests geändert werden darf, suchen Sie den Wert der `AutomationProperties.Name` oder `AutomationProperties.HelpText`, oder eine Verkettung der beiden.

Jede Plattform weist unterschiedliche Bildschirmsprachausgaben zu erzählen Eingabehilfen-Werte:

- iOS hat VoiceOver. Weitere Informationen finden Sie unter [Test Zugriff auf Ihr Gerät mit VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) auf developer.apple.com.
- Android bietet TalkBack. Weitere Informationen finden Sie unter [Zugriff auf Ihre App testen](https://developer.android.com/training/accessibility/testing.html#talkback) auf developer.android.com.
- Windows verfügt über die Sprachausgabe. Weitere Informationen finden Sie unter [überprüfen Haupt-app-Szenarien mit Sprachausgabe](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Allerdings ist das genaue Verhalten von einer Sprachausgabe auf dem Softwareupdatepunkt und Benutzerkonfiguration davon abhängig. Die meisten Bildschirmsprachausgaben gelesen z. B. bei Erhalt des Fokus, die einem Steuerelement zugeordneten Text, sodass Benutzer sich selbst zu orientieren, wie sie die Steuerelemente auf der Seite navigieren. Einige Bildschirmsprachausgaben können auch der Benutzeroberfläche für die gesamte Anwendung, wenn eine Seite angezeigt wird, wodurch den Benutzer von aller verfügbaren nur zu Informationszwecken Inhalt der Seite erhalten, bevor Sie versuchen, navigieren.

Sprachausgaben lesen Sie auch unterschiedliche zugriffsmöglichkeiten-Werte. In der beispielanwendung:

- VoiceOver liest die `Placeholder` Wert, der die `Entry`, gefolgt von Anweisungen für die Verwendung des Steuerelements.
- TalkBack liest die `Placeholder` Wert, der die `Entry`, gefolgt von der `AutomationProperties.HelpText` Werts, gefolgt vom Anweisungen für die Verwendung des Steuerelements.
- Sprachausgabe liest die `AutomationProperties.LabeledBy` Wert, der die `Entry`, gefolgt von Anweisungen zur Verwendung des Steuerelements.

Darüber hinaus Sprachausgabe priorisieren wird `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, und klicken Sie dann `AutomationProperties.HelpText`. Auf Android-Geräten kann TalkBack Kombinieren der `AutomationProperties.Name` und `AutomationProperties.HelpText` Werte. Aus diesem Grund wird empfohlen, dass Eingabehilfen gründliche Tests für jede Plattform, um eine optimale Leistung sicherzustellen, dass durchgeführt wird.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

Die `AutomationProperties.IsInAccessibleTree` angefügte Eigenschaft ist eine `boolean` , die bestimmt, ob das Element zugegriffen werden kann und daher sichtbar ist, um die Sprachausgabe ist. Er muss festgelegt werden, um `true` zu verwenden, den Zugriff auf andere angefügte Eigenschaften. Dies kann in XAML wie folgt erfolgen:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternativ können sie in c# wie folgt festgelegt werden:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Beachten Sie, dass die [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.IsInAccessibleTree` angefügte Eigenschaft – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

Die `AutomationProperties.Name` Wert der angefügten Eigenschaft sollte eine kurze, beschreibende Textzeichenfolge, die von eine Sprachausgabe verwendet wird, um ein Element bekannt sein. Für Elemente, die eine Bedeutung haben, die zum Verständnis des Inhalts oder mit der Benutzeroberfläche interagieren wichtig ist, sollte diese Eigenschaft festgelegt werden. Dies kann in XAML wie folgt erfolgen:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Alternativ können sie in c# wie folgt festgelegt werden:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Beachten Sie, dass die [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.Name` angefügte Eigenschaft – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

Die `AutomationProperties.HelpText` angefügte Eigenschaft sollte auf Text zur Beschreibung der Benutzeroberflächen-Elements festgelegt werden, und kann sein, um herauszufinden, der als QuickInfo-Text zu dem Element gehört. Dies kann in XAML wie folgt erfolgen:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Alternativ können sie in c# wie folgt festgelegt werden:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Beachten Sie, dass die [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.HelpText` angefügte Eigenschaft – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Auf manchen Plattformen zur Bearbeitung steuert, wie z. B. ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)die `HelpText` Eigenschaft kann in einigen Fällen nicht angegeben und mit Platzhaltertext ersetzt. "Geben Sie beispielsweise Ihr Name" ist ein guter Kandidat für die [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) -Eigenschaft, die den Text im Steuerelement vor dem tatsächlichen Benutzereingaben platziert.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

Die `AutomationProperties.LabeledBy` angefügte Eigenschaft ermöglicht es, ein anderes Element aus, um Informationen über Eingabehilfen für das aktuelle Element zu definieren. Z. B. eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) neben einer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kann verwendet werden, um was beschreiben die `Entry` darstellt. Dies kann in XAML wie folgt erfolgen:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Alternativ können sie in c# wie folgt festgelegt werden:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Beachten Sie, dass die [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.IsInAccessibleTree` angefügte Eigenschaft – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie festzulegende Eingabehilfen-Werte für den Benutzer Elemente der Benutzeroberfläche in einer Xamarin.Forms-Anwendung mithilfe von angefügten Eigenschaften aus der `AutomationProperties` Klasse. Diese Eigenschaften Satz native Eingabehilfen-Werte angefügt, sodass Bildschirmsprachausgaben zu den Elementen auf der Seite äußern kann.


## <a name="related-links"></a>Verwandte Links

- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
- [Eingabehilfen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
