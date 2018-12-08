---
title: Windows-Plattform-Besonderheiten
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert sind.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 39922ffaa144892c87d271e9b24524dc0a0a941a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061919"
---
# <a name="windows-platform-specifics"></a>Windows-Plattform-Besonderheiten

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

_Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert sind._

## <a name="visualelements"></a>VisualElements

Für die universelle Windows-Plattform die folgende plattformspezifischen Funktionen zur Xamarin.Forms-Ansichten, Seiten und Layouts angezeigt werden:

- Festlegen einer Zugriffstaste für eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [Einstellung VisualElement Zugriffsschlüssel](#visualelement-accesskeys).
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [deaktivieren Farbe Legacymodus](#legacy-color-mode).

<a name="visualelement-accesskeys" />

### <a name="setting-visualelement-access-keys"></a>Festlegen von VisualElement Zugriffstasten

Zugriffsschlüssel werden Tastenkombinationen aufgeführt, die Verbesserung der gebrauchstauglichkeit sowie der Barrierefreiheit von apps für die universelle Windows-Plattform durch die Bereitstellung einer intuitiven Methode für Benutzer schnell navigieren und interagieren mit der Benutzeroberfläche der Anwendung sichtbar über eine Tastatur anstelle von per Touch oder ein Maus. Sie sind Kombinationen aus die Alt-Taste und einen oder mehrere alphanumerische Schlüssel, die in der Regel nacheinander aufgerufen. Tastenkombinationen in Visual Studio werden automatisch für Zugriffsschlüssel unterstützt, die ein einzelnes alphanumerisches Zeichen verwenden.

Wichtige Tipps für den Zugriff schwimmen Badges, die neben der Steuerelemente, die Zugriffsschlüssel enthalten. Jeder Zugriff Zugriffstasteninfos enthält die alphanumerische Schlüssel, die das zugeordnete Steuerelement zu aktivieren. Wenn ein Benutzer die Alt-Taste drückt, werden die Zugriffstasteninfos Zugriff angezeigt.

Diese plattformspezifischen wird verwendet, um Geben Sie eine Zugriffstaste für eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) angefügten Eigenschaft, um einen alphanumerischen Wert, und legen optional die [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) angefügte Eigenschaft auf den Wert der [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) Enumeration, die [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) angefügten Eigenschaft, um eine `double`, und die [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) angefügte Eigenschaft auf einen `double`:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

Die `VisualElement.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um den Wert des Zugriffsschlüssels für Festlegen der `VisualElement`. Die [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) -Methode, gibt optional die Position zum Anzeigen der Zugriffstasteninfos Zugriff, mit der [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) Enumeration, die Bereitstellung von der folgenden möglichen Werten:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – Gibt an, dass die Platzierung der Zugriffstasteninfos vom Betriebssystem bestimmt wird.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – Gibt an, dass die Zugriffstasteninfos für den Zugriff über dem oberen Rand des angezeigt werden, wird die `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – Gibt an, dass der Zugriff Zugriffstasteninfos unter am unteren Rand des angezeigt wird der `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – Gibt an, dass der Zugriff Zugriffstasteninfos rechts neben dem rechten Rand angezeigt wird der `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – Gibt an, dass die Zugriffstasteninfos für den Zugriff auf der linken Seite des linken Rands des erscheint die `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – Gibt an, dass die Zugriffstasteninfos für den Zugriff auf den Mittelpunkt der überlagerten angezeigt wird der `VisualElement`.

> [!NOTE]
> In der Regel die [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) Zugriffstasteninfos Platzierung ist ausreichend, wozu die Unterstützung für adaptive Benutzeroberflächen.

Die [ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) und [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) Methoden können für eine präzisere Steuerung des Speicherorts Zugriffstasteninfos Zugriff verwendet werden. Das Argument für die `SetAccessKeyHorizontalOffset` -Methode gibt an, wie weit nach links Zugriffstasteninfos Zugriff zu verschieben oder rechts, und das Argument für die `SetAccessKeyVerticalOffset` Methode gibt an, wie weit die Zugriffstasteninfos für den Zugriff nach oben oder unten verschieben.

>[!NOTE]
> Zugriff Zugriffstasteninfos Offsets können nicht festgelegt werden, wenn die Platzierung der Key festgelegt ist `Auto`.

Darüber hinaus die [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), und [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) Methoden können verwendet werden zum Abrufen eines Wert und den Speicherort des Schlüssels.

Das Ergebnis ist, wichtige Tipps für den Zugriff können, neben einer beliebigen angezeigt werden [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Instanzen, die definieren, Zugriffsschlüssel, durch Drücken der Alt-Taste:

![VisualElement-Zugriffsschlüsseln plattformspezifische](windows-images/visualelement-accesskeys.png "VisualElement-Zugriffsschlüsseln plattformspezifische")

Wenn ein Benutzer einen Zugriffsschlüssel aktiviert, durch Drücken der Alt-Taste gefolgt von den Zugriff Schlüssel ist, wird die Standardaktion für die `VisualElement` ausgeführt wird. Z. B. wenn ein Benutzer aktiviert den Zugriffsschlüssel für ein [ `Switch` ](xref:Xamarin.Forms.Switch), `Switch` umgeschaltet wird. Wenn ein Benutzer die Zugriffstaste aktiviert, auf eine [ `Entry` ](xref:Xamarin.Forms.Entry), `Entry` den Fokus erhält. Wenn ein Benutzer die Zugriffstaste aktiviert, auf eine [ `Button` ](xref:Xamarin.Forms.Button), den Ereignishandler für die [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) -Ereignis immer ausgeführt wird.

Weitere Informationen zu Zugriffsschlüsseln finden Sie unter [Zugriffsschlüssel](/windows/uwp/design/input/access-keys#key-tip-positioning).

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Deaktivieren die Farbe der Legacymodus

Einige der Xamarin.Forms-Ansichten bieten eines ältere Farbmodus. In diesem Modus bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht nastaven NA hodnotu `false`, die Ansicht überschreibt die Farben, die vom Benutzer mit den systemeigenen Standardfarben für den deaktivierten Zustand festgelegt wird. Für die Abwärtskompatibilität dieser älteren Farbmodus-Kompatibilität für das Standardverhalten für die unterstützten Ansichten bleibt.

Diese plattformspezifischen deaktiviert dieser Modus ältere Farbe, damit Farben, die für eine Sicht vom Benutzer festgelegt bleiben, selbst wenn die Sicht deaktiviert ist. Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) angefügte Eigenschaft zu `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<Windows>` Methode angibt, dass diese plattformspezifischen nur auf Windows ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Kontrolle, ob die älteren Farbmodus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob es sich bei der älteren Farbmodus deaktiviert ist.

Das Ergebnis ist, dass die ältere Farbmodus deaktiviert werden kann, Farben, die für eine Sicht vom Benutzer festgelegt auch bleiben, wenn die Sicht deaktiviert ist:

![](windows-images/legacy-color-mode-disabled.png "Ältere Farbmodus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Farbmodus vollständig ignoriert wird. Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="views"></a>Ansichten

Auf der universellen Windows-Plattform (UWP), wird die folgende plattformspezifische Funktionalität für Xamarin.Forms-Ansichten bereitgestellt werden:

- Erkennen von leserichtung von Textinhalt in [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Weitere Informationen finden Sie unter [erkennen der Leserichtung von Inhalten](#inputview-readingorder).
- Aktivieren von Tap-gestenunterstützung im eine [ `ListView` ](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [aktivieren Tippen Sie auf Unterstützung für Gesten in einem ListView](#listview-selectionmode).
- Aktivieren einer [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) für die Interaktion mit der Rechtschreibprüfungs-Check-Engine. Weitere Informationen finden Sie unter [SearchBar Rechtschreibprüfung aktivieren](#searchbar-spellcheck).
- Aktivieren einer [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem Meldungsdialogfeld UWP. Weitere Informationen finden Sie unter [JavaScript-Warnungen anzeigen von](#webview-javascript-alert).

<a name="inputview-readingorder" />

### <a name="detecting-reading-order-from-content"></a>Erkennen von Leserichtung von Inhalten

Diese plattformspezifischen ermöglicht die leserichtung (links-nach-rechts oder rechts-nach-links) der bidirektionalen Text in [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) -Instanzen, die nicht dynamisch erkannt werden. Es ist in XAML verwendet, durch Festlegen der [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (für `Entry` und `Editor` Instanzen) oder [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

Die `Editor.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet um zu steuern, ob die leserichtung aus dem Inhalt erkannt wird die [ `InputView` ](xref:Xamarin.Forms.InputView). Darüber hinaus die `InputView.SetDetectReadingOrderFromContent` Methode kann verwendet werden, um umzuschalten, ob die leserichtung aus dem Inhalt, durch den Aufruf erkannt wird der [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) Methode, um den aktuellen Wert zurückzugeben:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Das Ergebnis ist, [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen können die leserichtung ihrer Inhalte, die dynamisch ermittelt haben:

[![InputView erkennen die Lesefolge von Inhalten plattformspezifische](windows-images/editor-readingorder.png "wird, die Lesefolge von Inhalten plattformspezifische erkennen")](windows-images/editor-readingorder-large.png#lightbox "InputView erkennen die Lesefolge von Inhalt plattformspezifische")

> [!NOTE]
> Im Gegensatz zur Einstellung der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) -Eigenschaft, die Logik für Ansichten, die erkennen, die leserichtung von deren Textinhalt wirkt sich nicht auf die Ausrichtung des Texts in der Ansicht. Stattdessen wird die Reihenfolge, in der bidirektionalen Textblöcke angeordnet werden, angepasst.

<a name="listview-selectionmode" />

### <a name="enabling-tap-gesture-support-in-a-listview"></a>Tippen Sie auf der Unterstützung für Gesten in einem ListView aktivieren

Für die universelle Windows-Plattform, standardmäßig die Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die systemeigenen `ItemClick` Ereignis, um auf die Interaktion, anstatt das systemeigene reagieren `Tapped` Ereignis. Dadurch wird die Barrierefreiheitsfunktionen, sodass die Windows-Sprachausgabe und Tastatur interagieren können die `ListView`. Allerdings rendert es auch alle Bewegungen Tippen Sie in der `ListView` nicht mehr funktionsfähig.

Diese plattformspezifischen steuert, ob Elemente in eine [ `ListView` ](xref:Xamarin.Forms.ListView) Gesten, tippen Sie auf reagieren können und somit, ob die systemeigene `ListView` ausgelöst wird der `ItemClick` oder `Tapped` Ereignis. Es ist in XAML verwendet, durch Festlegen der [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) angefügte Eigenschaft auf den Wert der [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) Enumeration:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

Die `ListView.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Steuerelement gibt an, ob Elemente in einer [ `ListView` ](xref:Xamarin.Forms.ListView) tippen von Bewegungen mit reagieren können die [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) Enumeration, die Bereitstellung von zwei möglicher Werten:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Gibt an, dass die `ListView` wird ausgelöst, die Native `ItemClick` Ereignis behandeln Interaktion, und geben Sie daher die Barrierefreiheitsfunktionen. Aus diesem Grund die Windows-Sprachausgabe und Tastatur interagieren die `ListView`. Jedoch Elemente in der `ListView` Gesten tippen kann nicht reagieren. Dies ist das Standardverhalten für `ListView` Instanzen für die universelle Windows-Plattform.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Gibt an, dass die `ListView` wird ausgelöst, die Native `Tapped` zu Interaktion zu behandelnden Ereignisses. Aus diesem Grund Elemente in der `ListView` Gesten tippen reagieren können. Allerdings sind keine Barrierefreiheitsfunktionen vorhanden und somit die Windows-Sprachausgabe und die Tastatur können nicht interagieren mit der `ListView`.

> [!NOTE]
> Die `Accessible` und `Inaccessible` die Modi schließen sich gegenseitig aus, und Sie müssen eine zugängliche Wahlmöglichkeiten [ `ListView` ](xref:Xamarin.Forms.ListView) oder `ListView` , die auf Gesten tippen reagieren können.

Darüber hinaus die [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) Methode kann verwendet werden, um das aktuelle [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Das Ergebnis ist, die einem angegebenen [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) gilt, an die [ `ListView` ](xref:Xamarin.Forms.ListView), steuert, ob Elemente in der `ListView` Gesten, tippen Sie auf reagieren können und somit, ob das systemeigene `ListView` löst die `ItemClick` oder `Tapped` Ereignis.

<a name="searchbar-spellcheck" />

### <a name="enabling-searchbar-spell-check"></a>Aktivieren der Rechtschreibprüfung SearchBar

Diese plattformspezifischen ermöglicht eine [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) für die Interaktion mit der Rechtschreibprüfungs-Check-Engine. Es ist in XAML verwendet, durch Festlegen der [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

Die `SearchBar.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird die Rechtschreibprüfung aktiviert und deaktiviert. Darüber hinaus die `SearchBar.SetIsSpellCheckEnabled` Methode kann verwendet werden, um die Rechtschreibprüfung zu wechseln, durch den Aufruf der [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) Methode zurückgeben soll, ob die Rechtschreibprüfung aktiviert ist:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Das Ergebnis ist, den in eingegebenen Text die [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) kann Rechtschreibung überprüft werden, mit der falsche Schreibweisen für den Benutzer angegeben wird:

![SearchBar Rechtschreibung prüfen plattformspezifische](windows-images/searchbar-spellcheck.png "SearchBar Rechtschreibung prüfen plattformspezifische")

> [!NOTE]
> Die `SearchBar` -Klasse in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace verfügt auch über [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) und [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) Methoden, die zum Aktivieren und deaktivieren verwendet werden können die Rechtschreibprüfung für die `SearchBar`bzw.

<a name="webview-javascript-alert" />

### <a name="displaying-javascript-alerts"></a>Anzeigen von JavaScript-Warnungen

Diese plattformspezifischen ermöglicht eine [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem Meldungsdialogfeld UWP. Es ist in XAML verwendet, durch Festlegen der [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

Die `WebView.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Kontrolle, ob JavaScript-Warnungen aktiviert sind. Darüber hinaus die `WebView.SetIsJavaScriptAlertEnabled` Methode kann verwendet werden, zum Umschalten der JavaScript-Warnungen durch Aufrufen der [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) Methode zurückgeben soll, ob sie aktiviert sind:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Das Ergebnis ist, dass JavaScript-Warnungen in einem UWP-Meldungsdialogfeld angezeigt werden können:

![WebView-JavaScript-Warnung plattformspezifische](windows-images/webview-javascript-alert.png "WebView-JavaScript-Warnung plattformspezifische")

## <a name="pages"></a>Seiten

Für die universelle Windows-Plattform die folgende plattformspezifischen Funktionen zur Xamarin.Forms-Seiten angezeigt werden:

- Reduzieren der [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Navigationsleiste. Weitere Informationen finden Sie unter [reduzieren eine Navigationsleiste MasterDetailPage](#collapsable_navigation_bar).
- Festlegen von Platzierungsoptionen für die Symbolleiste. Weitere Informationen finden Sie unter [ändern die Platzierung auf der Seite Symbolleiste](#toolbar_placement).
- Aktivieren die Seitensymbole anzuzeigendes eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Symbolleiste. Weitere Informationen finden Sie unter [Symbole aktivieren, klicken Sie auf einer "tabbedpage"](#tabbedpage-icons).

<a name="collapsable_navigation_bar" />

### <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Eine MasterDetailPage Navigationsleiste reduzieren

Diese plattformspezifischen wird zum Reduzieren der Navigationsleiste auf einen [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage), und in XAML verwendet wird, durch Festlegen der [ `MasterDetailPage.CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) und [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty)angefügte Eigenschaften:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

Die `MasterDetailPage.On<Windows>` Methode angibt, dass diese plattformspezifischen nur auf Windows ausgeführt wird. Die [ `Page.SetCollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um mit der reduzieren-Stil, Angeben der [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) Enumeration, die Bereitstellung von zwei Werte: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) und [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). Die [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},System.Double)) Methode wird verwendet, um die Breite einer teilweise reduzierten Navigationsleiste angeben.

Das Ergebnis ist, die einem angegebenen [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) gilt, an die [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) -Instanz, mit die Breite ebenfalls angegeben wird:

[![](windows-images/collapsed-navigation-bar.png "Reduziert die Navigationsleiste plattformspezifische")](windows-images/collapsed-navigation-bar-large.png#lightbox "reduzierten Navigationsleiste plattformspezifische")

<a name="toolbar_placement" />

### <a name="changing-the-page-toolbar-placement"></a>Ändern die Platzierung der Symbolleiste der Seite

Diese plattformspezifischen wird verwendet, um die Position einer Symbolleiste zu ändern, auf eine [ `Page` ](xref:Xamarin.Forms.Page), und in XAML verwendet wird, durch Festlegen der [ `Page.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) angefügte Eigenschaft auf den Wert der [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) Enumeration:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

Die `Page.On<Windows>` Methode angibt, dass diese plattformspezifischen nur auf Windows ausgeführt wird. Die [ `Page.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um die Symbolleiste-Position festgelegt werden, mit der [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) Enumeration bereitstellen drei Werte: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), und [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Das Ergebnis ist, dass die angegebene Symbolleisten-Platzierung, um angewendet wird die [ `Page` ](xref:Xamarin.Forms.Page) Instanz:

[![](windows-images/toolbar-placement.png "Symbolleiste Platzierung plattformspezifische")](windows-images/toolbar-placement-large.png#lightbox "Symbolleiste Platzierung plattformspezifische")

<a name="tabbedpage-icons" />

### <a name="enabling-icons-on-a-tabbedpage"></a>Aktivieren die Symbole auf einer "tabbedpage"

Diese plattformspezifischen ermöglicht Seitensymbole auf anzuzeigende eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Symbolleiste, und bietet die Möglichkeit, geben Sie optional auf die Größe der Symbole. Er wird genutzt, in XAML durch Festlegen der [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) angefügten Eigenschaft, um `true`, optional festlegen und die [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) angefügten Eigenschaft, um eine [ `Size` ](xref:Xamarin.Forms.Size) Wert:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" Icon="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" Icon="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" Icon="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
    {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", Icon = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", Icon = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", Icon = "contacts.png" });
  }
}
```

Die `TabbedPage.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um das Aktivieren oder deaktivieren Sie die Header-Symbole. Die [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) Methode gibt optional die Headergröße-Symbol mit einem [ `Size` ](xref:Xamarin.Forms.Size) Wert.

Darüber hinaus die `TabbedPage` -Klasse in der `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` Namespace verfügt auch über eine [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) Methode, die Header-Symbole, ermöglicht eine [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) -Methode, die Header-Symbole, deaktiviert und eine [ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) -Methode, die gibt eine `boolean` Wert, der angibt, ob Header Symbole aktiviert sind.

Das Ergebnis ist diese Seite, die Symbole angezeigt werden können, auf eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Symbolleiste mit der Größe der Symbole wird optional auf eine gewünschte Größe festgelegt werden:

!["Tabbedpage" Symbole aktiviert plattformspezifische](windows-images/tabbedpage-icons.png "\"tabbedpage\" Symbole aktiviert plattformspezifische")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert sind. Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
