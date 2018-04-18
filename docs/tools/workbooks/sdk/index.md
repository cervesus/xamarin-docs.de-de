---
title: Erste Schritte mit Xamarin-Arbeitsmappen SDK
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: ad7341776c2f37d4eb6238a26d5b12ee9e340ff7
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Erste Schritte mit Xamarin-Arbeitsmappen SDK

Dieses Dokument enthält eine kurze Einführung in die erste Schritte mit der Entwicklung von Integrationen für Xamarin-Arbeitsmappen. Viele dieser mit stabilen Xamarin-Arbeitsmappen verwendet werden, funktioniert aber **Integrationen über NuGet-Pakete laden wird nur in Arbeitsmappen 1.3 unterstützt**, in den alpha-Kanal zum Zeitpunkt der Verfassung.

## <a name="general-overview"></a>Allgemeine Übersicht

Xamarin-Arbeitsmappen sind kleine Bibliotheken, mit denen die [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK verwenden, um die Integration in die Xamarin-Arbeitsmappen und Inspektor Agents, um verbesserte Erfahrungen bereitzustellen.

Es gibt 3 wichtigsten Schritte, um erste Schritte mit der Entwicklung eines Integration – wir fügen sie hier zeigen.

## <a name="creating-the-integration-project"></a>Erstellen des Projekts Integration

Integration von Bibliotheken werden am besten als Multiplattform-Bibliotheken entwickelt. Da Sie die optimale Integration auf alle verfügbaren Agents, vergangene und zukünftige bereitstellen möchten, sollten Sie eine allgemein unterstützte Gruppe von Bibliotheken auswählen. Es wird empfohlen, mithilfe der Vorlage "Portable Library" für die größte Unterstützung:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Portable Library-Vorlage, Visual Studio für Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Portable Library Vorlage Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

In Visual Studio benötigen Sie stellen Sie sicher, dass Sie die folgenden Zielplattformen für Ihre portable Bibliothek auswählen:

[![Portable Library-Plattformen Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Nachdem Sie das Steuerelementbibliothek-Projekt erstellt haben, fügen Sie einen Verweis auf unserer `Xamarin.Workbooks.Integration` NuGet-Bibliothek über NuGet-Paket-Manager.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![NuGet Visual Studio für Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Möchten Sie die leere Klasse zu löschen, die als Teil des Projekts erstellt wird – Sie wird nicht werden benötigen dafür. Nachdem Sie diese Schritte ausgeführt haben, können Sie die Erstellung der Integration.

## <a name="building-an-integration"></a>Erstellen ein Integration

Fügen Sie eine einfache Integration erstellen. Wir schätzen tatsächlich die Farbe Grün, damit wir die Farbe Grün als Darstellung auf jedes Objekt hinzufügen. Erstellen Sie zunächst eine neue Klasse namens `SampleIntegration`, machen sie das Implementieren und unsere [ `IAgentIntegration` ] [ integration-type] Schnittstelle:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Was wir wollen ist das Hinzufügen einer [Darstellung](~/tools/workbooks/sdk/representations.md) für jedes Objekt, das grün ist. Wir müssen mithilfe eines Anbieters Darstellung hierzu. Anbieter von erben die [ `RepresentationProvider` ] [ reppr] Klasse – für unsere, es muss lediglich überschreiben [ `ProvideRepresentations` ] [ prrep]:

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

Zurückgegeben eine [ `Color` ] [ color], eine vorkonfigurierte Darstellungstyp in unserer SDK.
Sie werden feststellen, dass der Rückgabetyp ist ein `IEnumerable<object>` &mdash;ein Darstellung Anbieter möglicherweise viele Darstellungen für ein Objekt zurück! Alle Darstellung Anbieter sind für jedes Objekt aufgerufen, daher ist es wichtig, die keine Annahmen über welche Objekte an Sie übergeben werden.

Der letzte Schritt besteht tatsächlich unsere Anbieter registrieren, mit dem Agent und teilen Sie Arbeitsmappen, wo Typ für die Integration zu finden. Um den Provider zu registrieren, fügen Sie diesen Code auf die `IntegrateWith` Methode in der `SampleIntegration` Klasse, die wir zuvor erstellt haben:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Festlegen des Typs für die Integration erfolgt über ein assemblyweit-Attribut. Sie können dies in der Datei "AssemblyInfo.cs" oder in derselben Klasse wie der Typ für die Integration der Einfachheit halber einfügen:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Während der Entwicklung sicherlich bequemer verwenden [ `AddProvider` Überladungen] [ addprovider] auf `RepresentationManager` , mit denen Sie einen einfachen Rückruf zum Bereitstellen von Darstellungen in eine Arbeitsmappe zu registrieren und verschieben Sie diesen Code in Ihrem `RepresentationProvider` Implementierung, sobald Sie fertig sind. Ein Beispiel für das Rendern einer [ `OxyPlot` ] [ oxyplot] `PlotModel` kann wie folgt aussehen:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Diese APIs bieten Ihnen eine schnelle Möglichkeit, einsatzbereit, aber es würde nicht empfehlenswert, Versand ein gesamtes Integration nur ihrer Verwendung&mdash;angebotenen kaum steuern, wie die Typen vom Client verarbeitet werden.

Mit der Darstellung, die registriert wird kann Ihre Integration geliefert.

## <a name="shipping-your-integration"></a>Protokollversand von der integration

Um die Integration zu liefern, müssen Sie es ein NuGet-Paket hinzufügen.
Sie können es mit Ihrer vorhandenen Bibliotheksfreigabe NuGet geliefert, oder, wenn Sie ein neues Paket erstellen, können Sie diese Vorlage .nuspec-Datei als Ausgangspunkt verwenden.
Sie müssen in den Abschnitten, die relevant für Ihre Integration ausfüllen. Der wichtigste Teil Transportservers ist, dass alle Dateien für die Integration in eine `xamarin.interactive` Verzeichnis im Stammverzeichnis des Pakets. Dadurch kann wir die relevanten Dateien für die Integration, unabhängig davon, ob Sie ein vorhandenes Paket verwenden, oder erstellen Sie eine neue leicht zu finden.

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

Sie können die NuGet-Paket nach der Erstellung der .nuspec-Datei wie folgt:

```csharp
nuget pack MyIntegration.nuspec
```

und veröffentlichen Sie ihn [NuGet][nugetorg]. Nachdem dort vorhanden ist, müssen Sie möglicherweise, verweisen ihn aus jeder Arbeitsmappe und in Aktion erleben. Im folgenden Screenshot haben wir die Beispiel-Integration gepackt sind, dass es in diesem Dokument erstellt und das NuGet-Paket in einer Arbeitsmappe installiert:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Die Arbeitsmappe mit Integration](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Die Arbeitsmappe mit Integration](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Beachten Sie, dass Sie alle nicht sehen `#r` Direktiven oder einem beliebigen Element initialisiert werden, die Integration – Arbeitsmappen hat gekümmert alles für Sie im Hintergrund!

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich der übrigen Dokumentation weitere Informationen zu den gleitenden Teile, aus denen das SDK besteht und auf unserer [Beispiel Integrationen](~/tools/workbooks/samples/index.md) für zusätzliche Elemente, die über die Integration, wie die Bereitstellung von benutzerdefinierten JavaScript-Code, der ausgeführt wird, im durchführen die Arbeitsmappen-Client.

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
