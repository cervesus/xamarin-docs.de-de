---
title: Eingabemethoden-Editor-Optionen für den Eintrag unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Methode verwenden, die die Optionen für den Eingabemethoden-Editor für die weiche Tastatur für einen Eintrag festlegt.
ms.prod: xamarin
ms.assetid: 7909C738-04B2-4476-9A3B-A6D79BC3B9B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 3711e85bd30deb06f351f4539c5ffc7e4236efb6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653602"
---
# <a name="entry-input-method-editor-options-on-android"></a>Eingabemethoden-Editor-Optionen für den Eintrag unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem Android-plattformspezifischen werden die Optionen für den Eingabemethoden-Editor (IME) für [`Entry`](xref:Xamarin.Forms.Entry)die weiche Tastatur für ein festgelegt. Dies beinhaltet das Festlegen der Schaltfläche "Benutzer-Aktion" in der unteren Ecke die Bildschirmtastatur und die Interaktionen mit der `Entry`. Es ist in XAML verwendet, durch Festlegen der [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) angefügte Eigenschaft auf den Wert der [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

Die `Entry.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Eingabemethode Action-Option für die Bildschirmtastatur für Festlegen der [ `Entry` ](xref:Xamarin.Forms.Entry), mit der [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration, die die folgenden Werte:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – Gibt an, dass keine bestimmte Aktion-Schlüssel ist erforderlich, dass das zugrunde liegende Steuerelement selbst, wenn erzeugt werden können. Entweder `Next` oder `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – Gibt an, dass kein aktionsschlüssel zur Verfügung gestellt wird.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Gibt an, dass der aktionsschlüssel führt einen Vorgang "go", um den Benutzer an das Ziel des Texts Eingabe.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Gibt an, dass der aktionsschlüssel wird eine Operation mit "Suche", um den Benutzer auf die Ergebnisse der Suche nach dem sie eingegeben haben.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Gibt an, dass der aktionsschlüssel einen "Senden"-Vorgang, auf der Bereitstellung von den Text an das Ziel ausgeführt wird.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Gibt an, dass der aktionsschlüssel wird einen "Weiter" Vorgang ausführen, um den Benutzer zum nächsten Feld, das Text akzeptiert.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Gibt an, dass der aktionsschlüssel einen "erledigt"-Vorgang schließen die Bildschirmtastatur ausgeführt werden.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Gibt an, dass der aktionsschlüssel wird einen "früheren" Vorgang ausführen, um den Benutzer zum vorherigen Feld, das Text akzeptiert.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – der Maske zur Aktionsoptionen auswählen.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Gibt an, dass die Rechtschreibprüfung wird weder lernen Sie von der Benutzer noch basierte auf was der Benutzer zuvor eingegeben hat Korrekturen vorzuschlagen.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Gibt an, dass die Benutzeroberfläche nicht im Vollbildmodus gespeichert werden sollen.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – Gibt an, dass keine Benutzeroberfläche für den extrahierten Text angezeigt wird.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Gibt an, dass keine Benutzeroberfläche für benutzerdefinierte Aktionen angezeigt wird.

Das Ergebnis ist, die einem angegebenen [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Wert angewendet wird, um die Bildschirmtastatur für die [ `Entry` ](xref:Xamarin.Forms.Entry), die den Eingabemethoden-Editor-Optionen festlegt:

[![Eintrag input Methode Editor plattformspezifische](entry-ime-options-images/entry-imeoptions.png "Eintrag input Methode Editor plattformspezifische")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "Eintrag input Methode Editor plattformspezifische")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
