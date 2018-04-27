---
title: Hinzufügen einer Windows-App
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0d2ef44896c9352776443c2fec318d40d27d7539
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="adding-a-windows-app"></a>Hinzufügen einer Windows-App


1. **mit der rechten Maustaste auf die Projektmappe > Hinzufügen > Neues Projekt...**  und Hinzufügen einer **leere App (Windows)**

 ![](tablet-images/add-wu.png "Dialogfeld "Neues Projekt" hinzufügen")

2. **mit der rechten Maustaste auf das Projekt > NuGet-Pakete verwalten...**  und Hinzufügen der **Xamarin.Forms** Paket.

3. **mit der rechten Maustaste auf das Projekt > Hinzufügen > Verweis** , und erstellen Sie einen Projektverweis auf die freigegebene Xamarin.Forms-Anwendungsprojekt.

  ![](tablet-images/addref.png "Dialogfeld "Verweis-Manager"")

4. Bearbeiten **App.xaml.cs** enthalten die `Init()` Methodenaufruf innerhalb der `OnLaunched` Methode, um die Linie 65

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5. Bearbeiten Sie **"MainPage.xaml"** -ändern Sie das Stammelement `<Page` zu `<forms:WindowsPage` *und* definieren die `xmlns:forms` , die verwendet werden:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6. Bearbeiten Sie **"MainPage.Xaml.cs"** So entfernen Sie die `: Page` Vererbung Spezifizierer für Name der Klasse.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7. Noch im **"MainPage.Xaml.cs"**, Hinzufügen der `LoadApplication` rufen Sie in der `MainPage` Konstruktor (in der Nähe der Zeile 28) um die Xamarin.Forms-app zu starten:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8. Doppelklicken Sie auf **"Package.appxmanifest"** diese Funktionen festlegen, die häufig erforderlich sind:

  Legen Sie die Funktionen:

  * Internet (Client)
  * Speicherort

9. Fügen Sie schließlich alle lokalen Ressourcen (z. b. Bilddateien) aus den vorhandenen plattformprojekten, die erforderlich sind.

