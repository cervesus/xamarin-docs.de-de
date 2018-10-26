---
title: Automation-Eigenschaften
description: In diesem Artikel wird erläutert, wie die AutomationProperties-Klasse in einer Xamarin.Forms-Anwendung verwendet, damit eine Sprachausgabe auf der Seite zu den Elementen sprechen kann.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: c720b9f38d2a34155face10b75f5f054f3313711
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131517"
---
# <a name="automation-properties-in-xamarinforms"></a>Automatisierungseigenschaften in Xamarin.Forms

_Mit Xamarin.Forms können Eingabehilfen-Werte, die auf Elemente der Benutzeroberfläche mithilfe von angefügten Eigenschaften von der AutomationProperties-Klasse, die wiederum Satz native Eingabehilfen-Werte festgelegt werden. In diesem Artikel wird erläutert, wie die Klasse AutomationProperties verwendet, damit eine Sprachausgabe auf der Seite zu den Elementen sprechen kann._

Mit Xamarin.Forms können Eigenschaften für Elemente der Benutzeroberfläche über die folgenden angefügten Eigenschaften festgelegt werden soll:

- `AutomationProperties.IsInAccessibleTree` – Gibt an, ob das Element für eine barrierefreie Anwendung verfügbar ist. Weitere Informationen finden Sie unter [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – eine kurze Beschreibung des Elements, das als ein Bezeichner für die Spracherkennung aktivieren, für das Element dient. Weitere Informationen finden Sie unter [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – eine längere Beschreibung des Elements, das als dem Element zugeordnete QuickInfo-Text verstanden werden kann. Weitere Informationen finden Sie unter [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – ermöglicht es einem anderen Element, um Informationen zur Barrierefreiheit für das aktuelle Element zu definieren. Weitere Informationen finden Sie unter [AutomationProperties.LabeledBy](#labeledby).

Diese angefügt native Eingabehilfen-Werte von Eigenschaften festgelegt, damit eine Sprachausgabe über das Element sprechen kann. Weitere Informationen zu angefügten Eigenschaften finden Sie unter [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Mithilfe der `AutomationProperties` angefügte Eigenschaften können die Ausführung des UI-Tests unter Android beeinträchtigen. Die [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` und `AutomationProperties.HelpText` Eigenschaften beide setze die systemeigene `ContentDescription` -Eigenschaft, mit der `AutomationProperties.Name` und `AutomationProperties.HelpText` Eigenschaftswerte, die Vorrang vor den `AutomationId`Wert (wenn beide `AutomationProperties.Name` und `AutomationProperties.HelpText` festgelegt sind, werden die Werte werden verkettet). Dies bedeutet, dass alle Tests nach `AutomationId` schlägt fehl, wenn `AutomationProperties.Name` oder `AutomationProperties.HelpText` auch nach dem Element festgelegt werden. In diesem Szenario UI-Tests geändert werden darf, suchen Sie nach den Wert der `AutomationProperties.Name` oder `AutomationProperties.HelpText`, oder eine Verkettung beider.

Jede Plattform gibt es verschiedene Sprachausgabe, Erzählen Sie die Eingabehilfen-Werte zu:

- iOS enthält VoiceOver. Weitere Informationen finden Sie unter [Test Zugriff auf Ihr Gerät mit VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) auf developer.apple.com.
- Android hat TalkBack. Weitere Informationen finden Sie unter [Zugriff auf Ihre App testen](https://developer.android.com/training/accessibility/testing.html#talkback) auf developer.android.com.
- Windows verfügt über die Sprachausgabe. Weitere Informationen finden Sie unter [Haupt-app-Szenarien zu überprüfen, indem Sie mithilfe von Microsoft-Sprachausgabe](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Allerdings ist das genaue Verhalten der Sprachausgabe auf dem Softwareupdatepunkt und Konfiguration des Benutzers abhängig. Die meisten Sprachausgaben lesen z. B. den Text eines Steuerelements zugeordnet wird, wenn es den Fokus erhält, Benutzern ermöglichen, sich zu orientieren, wie sie zwischen den Steuerelementen auf der Seite wechseln. Einige Bildschirmsprachausgaben werden auch die Benutzeroberfläche für die gesamte Anwendung lesen, wenn eine Seite angezeigt wird, wodurch den Benutzer alle den Inhalt der Seite verfügbaren nur zu Informationszwecken erhalten, bevor Sie versuchen, sie navigieren.

Sprachausgaben lesen Sie auch verschiedene Eingabehilfen-Werte. In der beispielanwendung:

- VoiceOver liest die `Placeholder` Wert, der die `Entry`, gefolgt von Anweisungen für die Verwendung des Steuerelements.
- TalkBack liest die `Placeholder` Wert, der die `Entry`, gefolgt von der `AutomationProperties.HelpText` Werts, gefolgt von Anweisungen für die Verwendung des Steuerelements.
- Die Sprachausgabe liest die `AutomationProperties.LabeledBy` Wert der `Entry`, indem Sie die Anweisungen zur Verwendung des Steuerelements gefolgt.

Darüber hinaus wird die Sprachausgabe bevorzugen `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, und klicken Sie dann `AutomationProperties.HelpText`. Unter Android TalkBack kombinieren, kann die `AutomationProperties.Name` und `AutomationProperties.HelpText` Werte. Aus diesem Grund empfiehlt es sich, dass der Zugriff auf umfassende Tests auf jeder Plattform aus, um sicherzustellen, dass eine optimale Leistung durchgeführt wird.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

Die `AutomationProperties.IsInAccessibleTree` angefügte Eigenschaft ist eine `boolean` , die bestimmt, ob das Element zugegriffen werden kann und somit sichtbar ist, um der Sprachausgabe. Er muss festgelegt sein, um `true` verwenden Sie den Zugriff auf andere angefügten Eigenschaften. Dies kann in XAML wie folgt erreicht werden:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternativ können sie in c# wie folgt festgelegt werden:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Beachten Sie, dass die [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.IsInAccessibleTree` angefügte Eigenschaft: `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

Die `AutomationProperties.Name` Wert der angefügten Eigenschaft muss eine kurze, beschreibende Textzeichenfolge, die eine Sprachausgabe verwendet wird, um ein Element bekanntgeben zu können. Für Elemente, die eine Bedeutung, die haben für das Verständnis des Inhalts oder der Interaktion mit der Benutzeroberfläche wichtig ist, sollte diese Eigenschaft festgelegt werden. Dies kann in XAML wie folgt erreicht werden:

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
> Beachten Sie, dass die [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.Name` angefügte Eigenschaft: `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

Die `AutomationProperties.HelpText` angefügte Eigenschaft sollte festgelegt werden, um Text, der das Benutzeroberflächenelement beschreibt und zugeordnet sein kann sich der als QuickInfo-Text des Elements. Dies kann in XAML wie folgt erreicht werden:

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
> Beachten Sie, dass die [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.HelpText` angefügte Eigenschaft: `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Auf manchen Plattformen ist für die Bearbeitung steuert, wie z. B. eine [ `Entry` ](xref:Xamarin.Forms.Entry), `HelpText` Eigenschaft kann manchmal weggelassen und mit dem Platzhaltertext ersetzt werden. "Geben Sie beispielsweise Ihren Namen ein." ist ein guter Kandidat für die [ `Entry.Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) -Eigenschaft, die den Text im Steuerelement vor der tatsächlichen Benutzereingaben platziert.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

Die `AutomationProperties.LabeledBy` angefügte Eigenschaft ermöglicht es einem anderen Element, um Informationen zur Barrierefreiheit für das aktuelle Element zu definieren. Z. B. eine [ `Label` ](xref:Xamarin.Forms.Label) neben einer [ `Entry` ](xref:Xamarin.Forms.Entry) kann verwendet werden, um was beschreiben die `Entry` darstellt. Dies kann in XAML wie folgt erreicht werden:

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
> Beachten Sie, dass die [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) Methode kann auch verwendet werden, Festlegen der `AutomationProperties.IsInAccessibleTree` angefügte Eigenschaft: `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="related-links"></a>Verwandte Links

- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
- [Barrierefreiheit (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
