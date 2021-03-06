---
title: Einstieg in das Xamarin Workbooks SDK
description: In diesem Dokument wird beschrieben, wie Sie mit dem Xamarin Workbooks SDK beginnen, das zum Entwickeln von Integrationen für Xamarin Workbooks verwendet werden kann.
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: e4a9e9113f83dd89b622de3e1f74f458efd4f07f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018684"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Einstieg in das Xamarin Workbooks SDK

Dieses Dokument enthält eine kurze Anleitung für den Einstieg in die Entwicklung von Integrationen für Xamarin Workbooks. Vieles davon funktioniert mit dem stabilen Xamarin Workbooks, das Laden von **Integrationen über nuget-Pakete wird jedoch nur in-Arbeitsmappen 1,3 unterstützt**, die zum Zeitpunkt des Schreibens des Alphakanals verwendet werden.

## <a name="general-overview"></a>Allgemeine Übersicht

Xamarin Workbooks Integrationen sind kleine Bibliotheken, die das [`Xamarin.Workbooks.Integrations` nuget][nuget] -SDK für die Integration in die Xamarin Workbooks-und Inspektor-Agents verwenden, um eine verbesserte Umgebung bereitzustellen.

Es gibt drei wichtige Schritte für die ersten Schritte bei der Entwicklung einer Integration – wir erläutern Sie hier.

## <a name="creating-the-integration-project"></a>Erstellen des Integrationsprojekts

Integrations Bibliotheken werden am besten als Bibliotheken mit mehreren Plattformen entwickelt. Da Sie die beste Integration für alle verfügbaren Agents in der Vergangenheit und in Zukunft bereitstellen möchten, sollten Sie einen weit verbreiteten Satz von Bibliotheken auswählen. Es wird empfohlen, die Vorlage "Portable Bibliothek" für die breiteste Unterstützung zu verwenden:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![portablen Bibliotheks Vorlage Visual Studio für Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![portablen Bibliotheks Vorlage in Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

In Visual Studio sollten Sie sicherstellen, dass Sie die folgenden Zielplattformen für Ihre Portable Bibliothek auswählen:

[![Portable Bibliothek Plattformen Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

Nachdem Sie das Bibliotheksprojekt erstellt haben, fügen Sie über den nuget-Paket-Manager einen Verweis auf unsere `Xamarin.Workbooks.Integration` nuget-Bibliothek hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![nuget-Visual Studio für Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![nuget Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

Sie müssen die leere Klasse löschen, die Sie als Teil des Projekts erstellt haben – Sie benötigen Sie nicht für dieses. Nachdem Sie diese Schritte ausgeführt haben, können Sie mit der Erstellung ihrer Integration beginnen.

## <a name="building-an-integration"></a>Integrieren einer Integration

Wir werden eine einfache Integration erstellen. Wir schätzen die Farbe Grün, daher fügen wir die Farbe Grün als Darstellung zu jedem Objekt hinzu. Erstellen Sie zunächst eine neue Klasse mit dem Namen `SampleIntegration`, und legen Sie Sie als Implementierung unserer `IAgentIntegration` Schnittstelle:

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

Wir möchten für jedes Objekt, das eine grüne Farbe darstellt, eine [Darstellung](~/tools/workbooks/sdk/representations.md) hinzufügen. Hierfür wird ein Darstellungs Anbieter verwendet. Anbieter erben von der `RepresentationProvider`-Klasse – wir müssen lediglich `ProvideRepresentations`außer Kraft setzen:

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

Wir geben einen `Color`zurück, einen vordefinierten Darstellungs Typen in unserem SDK.
Sie werden feststellen, dass der Rückgabetyp hier ein `IEnumerable<object>`&mdash;ein Darstellungs Anbieter möglicherweise viele Darstellungen für ein Objekt zurückgibt. Alle Darstellungs Anbieter werden für jedes Objekt aufgerufen, daher ist es wichtig, keine Annahmen darüber zu treffen, welche Objekte an Sie übermittelt werden.

Der letzte Schritt besteht darin, den Anbieter tatsächlich beim Agent zu registrieren und Arbeitsmappen mitzuteilen, wo der Integrationstyp zu finden ist. Fügen Sie den folgenden Code der `IntegrateWith`-Methode in der `SampleIntegration`-Klasse hinzu, die wir zuvor erstellt haben, um den Anbieter zu registrieren:

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

Das Festlegen des Integrations Typs erfolgt über ein assemblyweites Attribut. Sie können dies in ihrer AssemblyInfo.cs oder in derselben Klasse wie Ihr Integrationstyp platzieren:

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

Während der Entwicklung ist es möglicherweise bequemer, `AddProvider` Überladungen auf `RepresentationManager` zu verwenden, die es Ihnen ermöglichen, einen einfachen Rückruf zu registrieren, um Darstellungen innerhalb einer Arbeitsmappe bereitzustellen, und diesen Code dann in die `RepresentationProvider` Implementierung zu verschieben, sobald Sie fertig sind. Ein Beispiel für das Rendern einer [`OxyPlot`][oxyplot] `PlotModel` könnte wie folgt aussehen:

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> Mit diesen APIs können Sie schnell loslegen, aber es wird nicht empfohlen, eine vollständige Integration nur mit Ihnen zu senden,&mdash;Sie nur sehr wenig Kontrolle darüber haben, wie Ihre Typen vom Client verarbeitet werden.

Wenn die Darstellung registriert ist, ist ihre Integration bereit für die Bereitstellung!

## <a name="shipping-your-integration"></a>Versenden der Integration

Um ihre Integration zu senden, müssen Sie Sie einem nuget-Paket hinzufügen.
Sie können Sie mit dem nuget-Element Ihrer vorhandenen Bibliothek versenden, oder wenn Sie ein neues Paket erstellen, können Sie diese Vorlage. nuspec-Datei als Ausgangspunkt verwenden.
Sie müssen die für ihre Integration relevanten Abschnitte ausfüllen. Der wichtigste Teil ist, dass sich alle Dateien für ihre Integration in einem `xamarin.interactive` Verzeichnis im Stammverzeichnis des Pakets befinden müssen. Dies ermöglicht es uns, alle relevanten Dateien für ihre Integration problemlos zu finden, unabhängig davon, ob Sie ein vorhandenes Paket verwenden oder ein neues erstellen.

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

Nachdem Sie die nuspec-Datei erstellt haben, können Sie Ihre nuget-Datei wie folgt Verpacken:

```csharp
nuget pack MyIntegration.nuspec
```

und veröffentlichen Sie Sie dann auf [nuget][nugetorg]. Sobald Sie vorhanden sind, können Sie von einer beliebigen Arbeitsmappe aus darauf verweisen und Sie in Aktion sehen. Im folgenden Screenshot haben wir die Beispiel Integration verpackt, die wir in diesem Dokument erstellt und das nuget-Paket in einer-Arbeitsmappe installiert haben:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Arbeitsmappe mit Integration](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Arbeitsmappe mit Integration](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

Beachten Sie, dass Sie keine `#r` Direktiven oder nichts sehen, um die Integration zu initialisieren – Arbeitsmappen haben all dies für Sie im Hintergrund erledigt!

## <a name="next-steps"></a>Nächste Schritte

In unserer weiteren Dokumentation finden Sie weitere Informationen zu den beweglichen Komponenten, aus denen das SDK besteht, sowie unsere [Beispiel Integrationen](~/tools/workbooks/samples/index.md) für weitere Dinge, die Sie in ihrer Integration durchführen können, wie z. b. das Bereitstellen benutzerdefinierter JavaScript, das im Arbeitsmappen-Client ausgeführt wird

[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[oxyplot]: http://www.oxyplot.org/
