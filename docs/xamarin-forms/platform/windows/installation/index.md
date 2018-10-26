---
title: Windows-Setup-Projekten
description: Ältere Xamarin.Forms-Projektmappen (oder unter MacOS erstellt) keine Projekte für die universelle Windows-Plattform, und daher wird in diesem Artikel erläutert, wie einer vorhandenen Xamarin.Forms-Projektmappe ein neues UWP-Projekt hinzugefügt.
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: fb166b69c76ca4c87746358258d97f1cb81cb301
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123225"
---
# <a name="setup-windows-projects"></a>Windows-Setup-Projekten

_Hinzufügen von neuen Windows-Projekten zu einer vorhandenen Xamarin.Forms-Projektmappe_

Ältere Xamarin.Forms-Projektmappen (oder unter MacOS erstellt) müssen sich nicht auf app-Projekte (Universelle Windows Plattform) aus. Aus diesem Grund müssen Sie manuell ein UWP-Projekt zum Erstellen einer app für Windows 10 (UWP) hinzufügen.

## <a name="add-a-universal-windows-platform-app"></a>Fügen Sie eine universelle Windows Plattform-app

Sie müssen ausgeführt werden **Visual Studio 2017** auf **Windows 10** zum Erstellen von UWP-apps. Weitere Informationen zu universellen Windows-Plattform, finden Sie unter [Einführung in die universelle Windows-Plattform](/windows/uwp/get-started/universal-application-platform-guide/).

UWP ist in Xamarin.Forms 2.1 und höher verfügbar und Xamarin.Forms.Maps wird in Xamarin.Forms 2.2 und höher unterstützt.

Überprüfen Sie die <a href="#troubleshooting">Problembehandlung</a> Abschnitt nützliche Tipps.

Um eine UWP-app hinzufügen, die auf Windows 10-Smartphones, Tablets und Desktops ausgeführt wird, gehen Sie wie folgt vor:

 1. Mit der rechten Maustaste auf die Projektmappe, und wählen Sie **hinzufügen > Neues Projekt...**  und Hinzufügen einer **leere App (Universelles Windows)** Projekt:

  ![](universal-images/add-wu.png "Dialogfeld \"Neues Projekt\" hinzufügen")

 2. In der **neues universelles Windows-Plattform-Projekt** Dialogfeld Wählen Sie die Minimum und Ziel-Versionen von Windows 10, die die Anwendung ausgeführt wird:

  ![](universal-images/target-version.png "Dialogfeld \"Neues universelles Windows-Plattform-Projekt\"")

 3. Mit der rechten Maustaste auf das UWP-Projekt, und wählen Sie **NuGet-Pakete verwalten...**  und Hinzufügen der **Xamarin.Forms** Paket. Stellen Sie sicher, dass die anderen Projekte in der Lösung auch auf die gleiche Version des Pakets Xamarin.Forms aktualisiert werden.

 4. Stellen Sie sicher, dass das neue UWP-Projekt erstellt werden der **erstellen > Configuration Manager** Fenster (wahrscheinlich wird nicht Ursache hierfür sein, in der Standardeinstellung). Tick der **erstellen** und **bereitstellen** Felder für die Universal-Projekt:

  [![](universal-images/configuration-sml.png "Fenster \"Konfigurations-Manager\"")](universal-images/configuration.png#lightbox "Konfigurations-Manager-Fenster")

 5. Mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen > Verweis** und einen Verweis auf das Projekt der Xamarin.Forms-Anwendung (.NET Standard oder freigegebenes Projekt) erstellen.

  ![](universal-images/addref-sml.png "Dialogfeld \"Verweis-Manager\"")

 6. Bearbeiten Sie in der UWP-Projekt, **"App.Xaml.cs"** sollen die `Init` Methodenaufruf innerhalb der `OnLaunched` Methode der Nähe von Zeile 52:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7. Bearbeiten Sie in der UWP-Projekt, **"MainPage.xaml"** durch das Entfernen der `Grid` innerhalb der `Page` Element.

 8. In **"MainPage.xaml"**, fügen Sie einen neuen `xmlns` Eintrag für `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9. In **"MainPage.xaml"**, ändern Sie den Stamm `<Page` Element `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10. Im UWP-Projekt bearbeiten **"MainPage.Xaml.cs"** Entfernen der `: Page` Vererbung Bezeichner für den Namen der Klasse (da es nun erbt, `WindowsPage` aufgrund der Änderung, die im vorherigen Schritt):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11. In **"MainPage.Xaml.cs"**, Hinzufügen der `LoadApplication` rufen Sie in der `MainPage` Konstruktor, um die Xamarin.Forms-app zu starten:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12. Fügen Sie alle lokalen Ressourcen (z. b. Bilddateien) aus den vorhandenen Plattform-Projekten, die erforderlich sind.

## <a name="troubleshooting"></a>Problembehandlung

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"Target Aufruf Ausnahme" Verwendung "Mit .NET Native-toolkette kompilieren"

Wenn die UWP-app mehrere Assemblys (beispielsweise Drittanbieter-Bibliotheken Steuerelement oder der app selbst wird in mehrere Bibliotheken unterteilt) verweist Xamarin.Forms ist möglicherweise nicht zum Laden von Objekten aus diesen Assemblys (z. B. benutzerdefinierte Renderer).

Dies geschieht, wenn Sie mit der **mit .NET Native-toolkette Kompilieren** Dies ist eine Option für die UWP-apps in der **Eigenschaften > Erstellen > Allgemein** Fenster für das Projekt.

Sie können dies beheben, indem Sie mithilfe einer UWP-spezifischen Überladung von der `Forms.Init` Aufrufen in **"App.Xaml.cs"** wie im folgenden Code gezeigt (ersetzen Sie `ClassInOtherAssembly` mit einer Klasse Code verweist auf):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Fügen Sie einen Eintrag für jede Assembly, die als Verweis im Projektmappen-Explorer, entweder über einen direkten Verweis oder einen NuGet hinzugefügt wurden.

#### <a name="dependency-services-and-net-native-compilation"></a>Dependency Services und .NET Native-Kompilierung

Releasebuilds, die mit .NET Native-Kompilierung können fehlschlagen, zum Auflösen der Abhängigkeitsdienste, die außerhalb der Haupt-app ausführbaren Datei (z. B. in einem separaten Projekt oder eine Bibliothek) definiert sind.

Verwenden der `DependencyService.Register<T>()` Methode, um die Abhängigkeit Dienstklassen manuell zu registrieren. Basierend auf dem obigen Beispiel, fügen Sie die Register-Methode wie folgt hinzu:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
