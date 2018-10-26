---
title: Erste Schritte mit der Xamarin-Arbeitsmappen-SDK
description: Dieses Dokument beschreibt, wie Sie zum Einstieg in der SDK Xamarin-Arbeitsmappen, die zum Entwickeln von Integrationen für Xamarin Workbooks verwendet werden kann.
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 5800e98acbff147735ae4a6125979a4b47be2367
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115366"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Erste Schritte mit der Xamarin-Arbeitsmappen-SDK

Dieses Dokument enthält eine kurze Einführung in die erste Schritte beim Entwickeln von Integrationen für Xamarin Workbooks. Viele dieser funktioniert mit den stabilen Xamarin Workbooks, aber **Integrationen über NuGet-Pakete laden wird nur in Arbeitsmappen 1.3 unterstützt**, in der alpha-Kanal zum Zeitpunkt der Verfassung.

## <a name="general-overview"></a>Allgemeine Übersicht

Xamarin Workbooks Integrationen sind kleine Bibliotheken, mit denen die [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK, um die Integration in die Xamarin Workbooks und Inspector Agents zur verbesserten Erlebnis ermöglichen.

Es gibt 3 wichtigsten Schritte für den Einstieg in die Entwicklung eines Integration – sie werden hier beschrieben.

## <a name="creating-the-integration-project"></a>Erstellen des Projekts für die Integration

Von integrationsbibliotheken werden am besten als plattformübergreifende Bibliotheken entwickelt. Da Sie die optimale Integration auf alle verfügbaren Agents, Vergangenheit und Zukunft bereitstellen möchten, sollten Sie einen allgemein unterstützten Satz von Bibliotheken zu wählen. Es wird empfohlen, mithilfe der "Portable Library" Vorlage für die umfassendste Unterstützung:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Portable Bibliothek Vorlage Visual Studio für Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Portable Bibliothek Vorlage Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

In Visual Studio sollten Sie sicherstellen, dass Sie die folgenden Zielplattformen für Ihre portable Bibliothek auswählen:

[![Portable Bibliothek Plattformen Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Nachdem Sie das Bibliotheksprojekt erstellt haben, fügen Sie einen Verweis auf unsere `Xamarin.Workbooks.Integration` NuGet-Bibliothek über den NuGet-Paket-Manager.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![NuGet Visual Studio für Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Sie sollten die leere Klasse zu löschen, die für Sie als Teil des Projekts erstellt wird – Sie wird nicht benötigen sie für diese. Nachdem Sie diese Schritte durchgeführt haben, können Sie mit der Entwicklung Ihrer Integration beginnen.

## <a name="building-an-integration"></a>Erstellen eine Integration

Wir erstellen eine einfache Integration. Wir schätzen sehr die Farbe Grün angezeigt werden, damit wir die Farbe Grün als Darstellung für jedes Objekt hinzufügen. Erstellen Sie zunächst eine neue Klasse namens `SampleIntegration`, und legen Sie ihn implementieren unsere [ `IAgentIntegration` ] [ integration-type] Schnittstelle:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Was wir tun möchten ist das Hinzufügen einer [Darstellung](~/tools/workbooks/sdk/representations.md) für jedes Objekt, das eine grüne Farbe ist. Wir werden mithilfe eines Anbieters Darstellung hierzu. Anbieter, die von erben die [ `RepresentationProvider` ] [ reppr] Klasse – für unsere, es muss lediglich überschreiben [ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

Zurückgegeben eine [ `Color` ] [ color], eine vorab erstellte Darstellungstyp in unser SDK.
Sie werden feststellen, dass der Rückgabetyp ist ein `IEnumerable<object>` &mdash;ein Darstellung Anbieter möglicherweise viele Darstellungen für ein Objekt zurück. Alle Darstellung-Anbieter werden für jedes Objekt bezeichnet, daher ist es wichtig, die keine Annahmen über die Objekte an Sie übergeben werden.

Der letzte Schritt besteht unsere Anbieter registrieren, mit dem Agent und teilen Sie Arbeitsmappen, wo unsere Integrationstyp zu finden. Um den Anbieter zu registrieren, fügen Sie diesen Code in die `IntegrateWith` -Methode in der die `SampleIntegration` Klasse, die wir zuvor erstellt haben:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Festlegen des Typs für die Integration erfolgt über ein assemblyweit-Attribut. Sie können dies in Ihrer Datei "AssemblyInfo.cs" oder in derselben Klasse wie den Integrationstyp der Einfachheit halber nehmen:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Während der Entwicklung Umständen ist es bequemer, mithilfe [ `AddProvider` Überladungen] [ addprovider] auf `RepresentationManager` , mit denen Sie Darstellungen in einer Arbeitsmappe, bieten einen einfachen Rückruf zu registrieren. und verschieben Sie dann diesen Code in Ihre `RepresentationProvider` Implementierung, sobald Sie fertig sind. Ein Beispiel für das Rendern einer [ `OxyPlot` ] [ oxyplot] `PlotModel` könnte wie folgt aussehen:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Diese APIs bieten Ihnen eine schnelle Möglichkeit zum Abrufen und ausführen, aber es wäre nicht empfehlenswert, versenden ein gesamtes Integration, nur diese&mdash;bieten nur sehr wenig steuern, wie die Typen vom Client verarbeitet werden.

Mit der Darstellung, die registriert wird kann die Integration geliefert.

## <a name="shipping-your-integration"></a>Protokollversand von der integration

Die Integration zu liefern, müssen Sie ihn in ein NuGet-Paket hinzufügen.
Sie können es mit Ihrer vorhandenen Bibliothek NuGet schicken, oder wenn Sie ein neues Paket erstellen, können Sie diese Vorlage NuSpec-Datei als Ausgangspunkt verwenden.
Sie müssen in den Abschnitten, die relevant für die Integration ausfüllen. Der wichtigste Teil Transportservers ist, dass alle Dateien für die Integration in eine `xamarin.interactive` Verzeichnis im Stammverzeichnis des Pakets. Dies ermöglicht uns die relevanten Dateien für die Integration, unabhängig davon, ob Sie ein vorhandenes Paket verwenden, oder erstellen Sie eine leicht zu finden.

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

Nachdem Sie die NuSpec-Datei erstellt haben, können Sie Ihres NuGet pack wie folgt:

```csharp
nuget pack MyIntegration.nuspec
```

und dann zu veröffentlichen, [NuGet][nugetorg]. Sobald es vorliegt, werden Sie möglicherweise, verweisen sie aus jeder Arbeitsmappe und in Aktion erleben. Im nachfolgenden Screenshot haben wir die Beispiel-Integration verpackt, dass wir in diesem Dokument erstellt und das NuGet-Paket in einer Arbeitsmappe installiert:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Die Arbeitsmappe mit Integration](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Die Arbeitsmappe mit Integration](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Beachten Sie, dass Sie keine sehen `#r` Direktiven oder etwas zum Initialisieren der Integration – Arbeitsmappen kümmert sich all das für Sie hinter den Kulissen!

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich unsere Dokumentationen für Weitere Informationen zu den bewegliche Teile, aus denen das SDK besteht und unseren [Beispiel Integrationen](~/tools/workbooks/samples/index.md) für zusätzliche Elemente können Sie über Ihre Integration, beispielsweise die Bereitstellung der benutzerdefinierten JavaScript-Code, der ausgeführt wird, im durchführen der Arbeitsmappen-Client.

[integration-type]: https://developer.xamarin.com/api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[xir]: https://developer.xamarin.com/api/namespace/Xamarin.Interactive.Representations/
[reppr]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
