---
title: Hinzufügen einer Windows Phone-App
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 55bd4bdcfde4c91ad5c9b94bef486207466e135d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-phone-app"></a>Hinzufügen einer Windows Phone-App


Erstens, wenn Sie die Vorlage Xamarin.Forms PCL verwendet [das Profil aktualisieren](~/xamarin-forms/platform/windows/installation/index.md), klicken Sie dann die Anweisungen unten befolgen:

1. **mit der rechten Maustaste auf die Projektmappe > Hinzufügen > Neues Projekt...**  und Hinzufügen einer **leere App (Windows Phone)**

  ![](phone-images/add-wp81.png "Dialogfeld "Neues Projekt" hinzufügen")

2. **mit der rechten Maustaste auf das neu erstellte Projekt > NuGet-Pakete verwalten...**  und Hinzufügen der **Xamarin.Forms** Paket.

3. **mit der rechten Maustaste auf das Projekt > Hinzufügen > Verweis** , und erstellen Sie einen Projektverweis auf die freigegebene Xamarin.Forms-Anwendungsprojekt.

  ![](phone-images/addref.png "Dialogfeld "Verweis-Manager"")

4. Bearbeiten **App.xaml.cs** enthalten die `Init()` Methode aufzurufen, in der `OnLaunched` Methode, um die Linie 67:

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Bearbeiten Sie **"MainPage.xaml"** -ändern Sie das Stammelement `<Page` zu `<forms:WindowsPhonePage` *und* definieren die `xmlns:forms` , die verwendet werden:

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . Bearbeiten Sie **"MainPage.Xaml.cs"** So entfernen Sie die `: PhonePage` Vererbung Spezifizierer für Name der Klasse.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . Noch im **"MainPage.Xaml.cs"**, Hinzufügen der `LoadApplication` rufen Sie in der `MainPage` Konstruktor (in der Nähe der Zeile 28) um die Xamarin.Forms-app zu starten:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Doppelklicken Sie auf **"Package.appxmanifest"** diese Funktionen festlegen, die häufig erforderlich sind:

  * Internet (Client & Server)

9 . Fügen Sie schließlich alle lokalen Ressourcen (z. b. Bilddateien) aus den vorhandenen plattformprojekten, die erforderlich sind.

