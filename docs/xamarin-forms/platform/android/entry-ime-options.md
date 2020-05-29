---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bb77e9fafe39bf76a7d4290dba0bc658cd15094f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140035"
---
# <a name="entry-input-method-editor-options-on-android"></a>Eingabemethoden-Editor-Optionen für den Eintrag unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem Android-plattformspezifischen werden die Optionen für den Eingabemethoden-Editor (IME) für die weiche Tastatur für ein festgelegt [`Entry`](xref:Xamarin.Forms.Entry) . Dies umfasst das Festlegen der Benutzer Aktions Schaltfläche in der unteren Ecke der Bildschirmtastatur und der Interaktionen mit dem `Entry` . Sie wird in XAML verwendet, indem die [`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

Die- `Entry.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `Entry.SetImeOptions` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. entry. * timeoptions ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Eintrag}, Xamarin.Forms . Platformconfiguration. androidspecific. imeflags))-Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um die Aktions Option der Eingabemethode für die weiche Tastatur für das festzulegen [`Entry`](xref:Xamarin.Forms.Entry) , wobei die- [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration die folgenden Werte bereitstellt:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default)– Gibt an, dass kein spezifischer Aktions Schlüssel erforderlich ist, und dass das zugrunde liegende Steuerelement ggf. seine eigene erzeugt. Dabei handelt es sich entweder um `Next` oder `Done` .
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None)– Gibt an, dass kein Aktions Schlüssel verfügbar gemacht wird.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go)– Gibt an, dass der Aktions Schlüssel einen "Go"-Vorgang ausführt und den Benutzer zum Ziel des eingegebenen Texts führt.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search)– Gibt an, dass der Aktions Schlüssel einen Suchvorgang ausführt, bei dem der Benutzer die Ergebnisse der Suche nach dem eingegebenen Text annimmt.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send)– Gibt an, dass der Aktions Schlüssel einen Send-Vorgang ausführt und den Text an das Ziel übergibt.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next)– Gibt an, dass der Aktions Schlüssel einen "Next"-Vorgang ausführt, wobei der Benutzer zum nächsten Feld wird, das Text annimmt.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done)– Gibt an, dass der Aktions Schlüssel einen "Done"-Vorgang ausführt und die weiche Tastatur schließt.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous)– Gibt an, dass der Aktions Schlüssel einen "Previous"-Vorgang ausführt, wobei der Benutzer zum vorherigen Feld wird, das Text annimmt.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction)– die Maske, um Aktions Optionen auszuwählen.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning)– Gibt an, dass die Rechtschreibprüfung weder vom Benutzer erfährt noch Korrekturen basierend auf dem, was der Benutzer zuvor eingegeben hat.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen)– Gibt an, dass die Benutzeroberfläche nicht in den Vollbild gelangen soll.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi)– Gibt an, dass für den extrahierten Text keine Benutzeroberfläche angezeigt wird.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction)– Gibt an, dass für benutzerdefinierte Aktionen keine Benutzeroberfläche angezeigt wird.

Das Ergebnis ist, dass ein [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) angegebener Wert auf die weiche Tastatur für die angewendet wird [`Entry`](xref:Xamarin.Forms.Entry) , die die Optionen für den Eingabemethoden-Editor festlegt:

[![Eingabemethoden-Editor für den Eintrag plattformspezifisch](entry-ime-options-images/entry-imeoptions.png "Eingabemethoden-Editor für den Eintrag plattformspezifisch")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "Eingabemethoden-Editor für den Eintrag plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
