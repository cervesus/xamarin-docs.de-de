---
title: Aktualisieren von Komponenten verweisen auf nuget
description: In diesem Dokument wird beschrieben, wie Sie Ihre Komponenten Verweise durch nuget-Pakete zu zukünftigen nachweisen Ihrer Apps ersetzen, da der xamarin Component Store nicht mehr unterstützt wird.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: conceptdev
ms.author: crdun
ms.date: 04/18/2018
ms.openlocfilehash: 6d40555c70072a4c057739b39cc24a4f885f2dc9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765295"
---
# <a name="updating-component-references-to-nuget"></a>Aktualisieren von Komponenten verweisen auf nuget

> [!IMPORTANT]
> Der Komponenten Speicher wurde seit dem 15. Mai 2018 nicht mehr unterstützt (dieser Abschluss wurde ursprünglich im November 2017 [angekündigt](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) ).
>
> Xamarin-Komponenten werden in Visual Studio nicht mehr unterstützt und sollten durch nuget-Pakete ersetzt werden. Befolgen Sie die nachfolgenden Anweisungen, um Komponenten Verweise aus Ihren Projekten manuell zu entfernen.

Weitere Informationen finden Sie unter Hinzufügen von nuget-Paketen unter [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) oder [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Eine Liste beliebter xamarin [-Plug-ins und-Bibliotheken](https://github.com/xamarin/XamarinComponents/blob/master/README.md) ist verfügbar, um Alternativen zu Komponenten zu finden, die nicht als nuget-Pakete verfügbar sind.

## <a name="manually-removing-component-references"></a>Manuelles Entfernen von Komponenten verweisen

Die Version 15,6 von Visual Studio und 7,4 Release von Visual Studio für Mac unterstützt keine Komponenten mehr in Ihrem Projekt. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wenn Sie ein Projekt in Visual Studio laden, wird das folgende Dialogfeld angezeigt, in dem erläutert wird, dass alle Komponenten aus dem Projekt manuell entfernt werden müssen:

![Warnungs Dialogfeld, in dem erläutert wird, dass eine Komponente in Ihrem Projekt gefunden wurde und entfernt werden muss](component-nuget-images/component-alert-vs.png)

So entfernen Sie eine Komponente aus dem Projekt:

1. Öffnen Sie die **csproj** -Datei. Klicken Sie dazu mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Projekt entladen**aus. 

2. Klicken Sie erneut mit der rechten Maustaste auf das entladene Projekt, und wählen Sie dann **Bearbeiten {your-Project-Name}. csproj**aus.

3. Suchen Sie Verweise in der Datei auf `XamarinComponentReference`. Dies sollte in etwa wie im folgenden Beispiel aussehen:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Entfernen Sie die Verweise `XamarinComponentReference` auf, und speichern Sie die Datei. Im obigen Beispiel ist es sicher, die gesamte `ItemGroup`zu entfernen.

5. Nachdem die Datei gespeichert wurde, klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Projekt erneut laden**aus.

6. Wiederholen Sie die obigen Schritte für jedes Projekt in der Projekt Mappe.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie ein Projekt in Visual Studio für Mac laden, wird das folgende Dialogfeld angezeigt, in dem erläutert wird, dass alle Komponenten aus dem Projekt manuell entfernt werden müssen:

![Warnungs Dialogfeld, in dem erläutert wird, dass eine Komponente in Ihrem Projekt gefunden wurde und entfernt werden muss](component-nuget-images/component-alert.png)

So entfernen Sie eine Komponente aus dem Projekt:

1. Öffnen Sie die CSPROJ-Datei. Klicken Sie dazu mit der rechten Maustaste auf den Projektnamen, und wählen Sie Extras **> Datei bearbeiten**aus.

2. Suchen Sie Verweise in der Datei auf `XamarinComponentReference`. Dies sollte in etwa wie im folgenden Beispiel aussehen:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Entfernen Sie die Verweise `XamarinComponentReference` auf, und speichern Sie die Datei. Im obigen Beispiel ist es sicher, das gesamte`ItemGroup`

4. Wiederholen Sie die obigen Schritte für jedes Projekt in der Projekt Mappe.

-----

> [!WARNING]
> Die folgenden Anweisungen funktionieren nur mit älteren Versionen von Visual Studio.
> Der Knoten **Komponenten** ist nicht mehr in den aktuellen Versionen von Visual Studio 2017 oder Visual Studio für Mac verfügbar.

In den folgenden Abschnitten wird erläutert, wie Sie vorhandene xamarin-Lösungen aktualisieren, um Komponenten Verweise auf nuget-Pakete zu ändern.

- [Komponenten, die nuget-Pakete enthalten](#contain)
- [Komponenten mit nuget-Ersetzungen](#replace)

Die meisten Komponenten fallen in eine der oben genannten Kategorien.
Wenn Sie eine Komponente verwenden, die anscheinend kein entsprechendes nuget-Paket enthält, lesen Sie den Abschnitt [Komponenten ohne nuget-Migrationspfad](#require-update) weiter unten.

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Komponenten, die nuget-Pakete enthalten

Viele Komponenten enthalten bereits nuget-Pakete, und der Migrationspfad dient lediglich zum Löschen des Komponenten Verweises.

Sie können bestimmen, ob die Komponente bereits ein nuget-Paket enthält, indem Sie auf die Komponente in der Projekt Mappe doppelklicken:

![Komponenten Knoten erweitert](component-nuget-images/solution-sml.png)

Auf der Registerkarte **Pakete** werden alle nuget-Pakete aufgelistet, die in der Komponente enthalten sind:

![Registerkarte "Pakete" enthält nuget](component-nuget-images/packages-tab-sml.png)

Beachten Sie, dass **die Registerkarte** Assemblys leer ist:

![Assemblyregister Karte ist leer](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualisieren der Lösung

Löschen Sie den **Komponenten** Eintrag aus der Projekt Mappe, um die Projekt Mappe zu aktualisieren:

![Komponente löschen](component-nuget-images/delete-component-sml.png)

Das nuget-Paket wird weiterhin im Knoten **Pakete** aufgeführt, und Ihre APP wird wie gewohnt kompiliert und ausgeführt. In Zukunft werden Updates für dieses Paket über das **nuget** -Update Feature ausgeführt:

![Nuget-Paket aktualisieren](component-nuget-images/nuget-update-sml.png)

<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Komponenten mit nuget-Ersetzungen

Wenn **die Registerkarte** Assemblys der Komponenten Info-Assemblys Einträge enthält, wie unten gezeigt, müssen Sie das entsprechende nuget-Paket manuell suchen.

![Enthält Assemblys](component-nuget-images/assemblies-tab-sml.png)

Beachten Sie, dass die Registerkarte **Pakete** wahrscheinlich leer ist:

![](component-nuget-images/packages-tab-empty-sml.png)

_Sie kann nuget-Abhängigkeiten enthalten, aber diese können ignoriert werden._

Um zu bestätigen, dass ein Ersatz-nuget-Paket vorhanden ist, suchen Sie nach [NuGet.org](https://www.nuget.org/packages), oder verwenden Sie den Komponentennamen oder alternativ Autor.

Beispielsweise können Sie das beliebte **SQLite-net-PCL-** Paket suchen, indem Sie Folgendes suchen:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl)– der Produktname.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum)– das Profil des Autors.

### <a name="updating-the-solution"></a>Aktualisieren der Lösung

Nachdem Sie bestätigt haben, dass die Komponente in nuget verfügbar ist, führen Sie die folgenden Schritte aus:

#### <a name="delete-the-component"></a>Löschen der Komponente

Klicken Sie mit der rechten Maustaste auf die Komponente in der Projekt Mappe, und wählen Sie **Entfernen**

![Komponente entfernen](component-nuget-images/remove-component-sml.png)

Hierdurch werden die Komponente und alle Verweise gelöscht. Dadurch wird der Build unterbricht, bis Sie das entsprechende nuget-Paket hinzufügen, um es zu ersetzen.

#### <a name="add-the-nuget-package"></a>Hinzufügen des nuget-Pakets

1. Klicken Sie mit der rechten Maustaste auf den Knoten **Pakete** , und wählen Sie **Pakete hinzufügen...** aus.
2. Suchen Sie nach dem Namen oder dem Autor des nuget-Austauschs:

    ![](component-nuget-images/nuget-search-sml.png)

3. Klicken **Sie auf Paket hinzufügen**.

Das nuget-Paket wird dem Projekt zusammen mit allen Abhängigkeiten hinzugefügt.
Dadurch sollte der Build behoben werden. Wenn der Buildvorgang weiterhin fehlschlägt, überprüfen Sie jeden Fehler, um festzustellen, ob API-Unterschiede zwischen der Komponente und dem nuget-Paket vorliegen.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Komponenten ohne nuget-Migrationspfad

Machen Sie sich keine Gedanken, wenn Sie nicht sofort einen Ersatz für die in der Anwendung verwendeten Komponenten finden. Vorhandene Komponenten können weiterhin in Visual Studio 15,5 verwendet werden, und der Knoten **Komponenten** wird wie gewohnt in der Projekt Mappe angezeigt.

In zukünftigen Versionen von Visual Studio werden Komponenten jedoch _nicht_ wieder hergestellt oder aktualisiert.
Dies bedeutet, dass die Komponente nicht heruntergeladen und installiert wird, wenn Sie die Projekt Mappe auf einem neuen Computer öffnen. und der Autor kann Ihnen keine Updates bereitstellen. Planen Sie Folgendes:

- Extrahieren Sie die Assemblys aus der Komponente, und verweisen Sie direkt in Ihrem Projekt darauf.
- Wenden Sie sich an den Autor der Komponente, und Fragen Sie nach Plänen zur Migration zu nuget.
- Untersuchen Sie alternative nuget-Pakete, oder suchen Sie den Quellcode, wenn es sich um eine Open Source-Komponente handelt.

Viele Komponentenhersteller arbeiten noch an der Migration zu nuget, und andere (einschließlich kommerziell verfügbarer Produkte) untersuchen möglicherweise Alternative Übermittlungs Optionen.

## <a name="related-links"></a>Verwandte Links
- [Liste der gängigen xamarin-Plug-ins und-Bibliotheken](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [Installieren und Verwenden eines nuget-Pakets (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Einschließen eines nuget-Pakets (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
