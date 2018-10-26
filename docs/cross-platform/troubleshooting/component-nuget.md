---
title: Aktualisieren von komponentenverweisen auf NuGet
description: Dieses Dokument beschreibt, wie Sie die Komponente verweist durch NuGet-Pakete in Zukunft Ihrer apps, ersetzen, da der Xamarin Component Store eingestellt wurde.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: 70ca9a73c83bed5233b77a6f7be80a13f04f2bcb
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122152"
---
# <a name="updating-component-references-to-nuget"></a>Aktualisieren von komponentenverweisen auf NuGet

> [!IMPORTANT]
> Die Store-Komponente wurde ab 15. Mai 2018 eingestellt (dieses Abschlusses war ursprünglich [angekündigt](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) im November 2017).
>
> Xamarin-Komponenten werden in Visual Studio nicht mehr unterstützt und sollte durch NuGet-Pakete ersetzt werden. Führen Sie die nachstehenden Anweisungen zum manuellen Entfernen von komponentenverweisen aus Projekten.

Finden Sie diese Anweisungen zum Hinzufügen von NuGet-Pakete auf [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) oder [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Eine Liste mit beliebten Xamarin [-Plug-Ins und -Bibliotheken](https://github.com/xamarin/XamarinComponents/blob/master/README.md) ist verfügbar, um Alternativen zu den Komponenten zu finden, die als NuGet-Pakete nicht verfügbar sind.

## <a name="manually-removing-component-references"></a>Manuelles Entfernen von komponentenverweisen

Release von Visual Studio 15.6 und Version von Visual Studio für Mac 7.4 unterstützen Komponenten nicht mehr in Ihrem Projekt. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wenn Sie ein Projekt in Visual Studio laden, wird das folgende Dialogfeld angezeigt, dass Sie alle Komponenten manuell aus Ihrem Projekt entfernen müssen:

![Warnung Dialogfeld erläutert, dass eine Komponente in Ihr Projekt gefunden wurde und entfernt werden müssen](component-nuget-images/component-alert-vs.png)

Entfernen eine Komponente aus Ihrem Projekt:

1. Öffnen der **csproj** Datei. Zu diesem Zweck mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Projekt entladen**. 

2. Mit der rechten Maustaste erneut auf das Projekt entladen, und wählen Sie **{your-Project-Name} .csproj bearbeiten**.

3. "Alle Verweise suchen" in der Datei `XamarinComponentReference`. Es sollte ähnlich wie im folgenden Beispiel aussehen:

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

4. Entfernen Sie die Verweise auf `XamarinComponentReference` und speichern Sie die Datei. Im obigen Beispiel ist es sicher ist, entfernen Sie die gesamte `ItemGroup`.

5. Nachdem die Datei gespeichert wurde, mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Projekt erneut laden**.

6. Wiederholen Sie die oben genannten Schritte für jedes Projekt in der Projektmappe ein.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wenn Sie ein Projekt in Visual Studio für Mac laden, wird das folgende Dialogfeld angezeigt, dass Sie alle Komponenten manuell aus Ihrem Projekt entfernen müssen:

![Warnung Dialogfeld erläutert, dass eine Komponente in Ihr Projekt gefunden wurde und entfernt werden müssen](component-nuget-images/component-alert.png)

Entfernen eine Komponente aus Ihrem Projekt:

1. Öffnen Sie die CSPROJ-Datei. Zu diesem Zweck mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Extras > Datei bearbeiten**.

2. "Alle Verweise suchen" in der Datei `XamarinComponentReference`. Es sollte ähnlich wie im folgenden Beispiel aussehen:

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

3. Entfernen Sie die Verweise auf `XamarinComponentReference` und speichern Sie die Datei. Im obigen Beispiel ist es sicher ist, entfernen Sie die gesamte `ItemGroup`

4. Wiederholen Sie die oben genannten Schritte für jedes Projekt in der Projektmappe ein.

-----

> [!WARNING]
> Die folgenden Anweisungen funktionieren nur mit älteren Versionen von Visual Studio.
> Die **Komponenten** Knoten ist nicht mehr zur Verfügung, in der aktuellen Versionen von Visual Studio 2017 oder Visual Studio für Mac.

In den folgenden Abschnitten wird erläutert, wie vorhandene Xamarin-Lösungen zum Ändern von komponentenverweisen auf NuGet-Pakete zu aktualisieren.

- [Komponenten, die NuGet-Pakete enthalten](#contain)
- [Komponenten mit NuGet-Ersetzungen](#replace)

Die meisten Komponenten gehören zu einer der oben genannten Kategorien.
Bei Verwendung eine Komponente, die nicht angezeigt wird, haben eine entsprechende NuGet-Paket, das Lesen der [-Komponenten ohne einen Migrationspfad NuGet](#require-update) Abschnitt weiter unten.

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Komponenten, die NuGet-Pakete enthalten

Viele Komponenten bereits NuGet-Pakete enthalten, und der Migrationspfad ist sich einfach für den Komponentenverweis zu löschen.

Sie können ermitteln, ob die Komponente bereits ein NuGet-Paket enthält, durch Doppelklicken auf die Komponente in der Projektmappe:

![Erweitert Knoten "Komponenten"](component-nuget-images/solution-sml.png)

Die **Pakete** Registerkarte werden alle NuGet-Pakete enthalten, die in der Komponente aufgeführt:

![Registerkarte "Pakete" enthält NuGet](component-nuget-images/packages-tab-sml.png)

Beachten Sie, dass die **Assemblys** Registerkarte leer sein wird:

![Registerkarte "Assemblys" ist leer](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualisierung der Lösung

Um die Projektmappe zu aktualisieren, löschen Sie die **Komponente** Eintrag aus der Projektmappe:

![Löschen der Komponente](component-nuget-images/delete-component-sml.png)

Das NuGet-Paket in aufgelistet bleiben die **Pakete** Knoten und Ihrer app kompiliert und führen Sie wie gewohnt. In Zukunft werden Updates für dieses Paket ausgeführt werden, über die **Nuget** Updatefunktion:

![Aktualisieren von NuGet-Paket](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Komponenten mit NuGet-Ersetzungen

Wenn die Seite "Komponente" **Assemblys** Registerkarte enthält Einträge, wie unten dargestellt, müssen Sie das entsprechende NuGet-Paket manuell zu suchen.

![Enthält Assemblys](component-nuget-images/assemblies-tab-sml.png)

Beachten Sie, dass die **Pakete** Registerkarte wird wahrscheinlich leer sein:

![](component-nuget-images/packages-tab-empty-sml.png)

_Es kann NuGet-Abhängigkeiten enthalten, aber diese können ignoriert werden._

Um ein Ersatz-NuGet-Paket ist vorhanden zu bestätigen, suchen Sie nach [NuGet.org](https://www.nuget.org/packages), verwenden den Namen der Komponente, oder Sie können auch vom Autor.

Als Beispiel finden Sie das beliebte **Sqlite-Net-Pcl** Paket durch Suchen nach:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – der Name des Produkts.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – der Autor des Profils.

### <a name="updating-the-solution"></a>Aktualisierung der Lösung

Nachdem Sie, dass die Komponente ist in NuGet verfügbar ist bestätigt haben, gehen Sie wie folgt vor:

#### <a name="delete-the-component"></a>Löschen Sie die Komponente

Klicken Sie mit der rechten Maustaste auf die Komponente in der Projektmappe, und wählen Sie **entfernen**:

![Entfernen von Komponenten](component-nuget-images/remove-component-sml.png)

Dadurch wird die Komponente und alle Verweise löschen. Dadurch wird den Build, unterbrochen, bis Sie das entsprechende NuGet-Paket, um es zu ersetzen hinzufügen.

#### <a name="add-the-nuget-package"></a>Fügen Sie das NuGet-Paket hinzu.

1. Mit der rechten Maustaste auf die **Pakete** Knoten, und wählen Sie **Pakete hinzufügen...** .
2. Suchen Sie nach der NuGet-Ersatz anhand des Namens oder der Autor:

  ![](component-nuget-images/nuget-search-sml.png)

3. Drücken Sie **Paket hinzufügen**.

Das NuGet-Paket wird dem Projekt mit allen Abhängigkeiten hinzugefügt werden.
Dies sollte den Build behoben werden. Wenn der Build ein Fehler auftritt weiterhin, untersuchen Sie jeden Fehler aus, um festzustellen, ob API-Unterschiede zwischen der Komponente und das NuGet-Paket.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>-Komponenten ohne einen Migrationspfad von NuGet

Seien Sie nicht beunruhigt, wenn Sie nicht sofort einen Ersatz für Komponenten, die in Ihrer Anwendung finden. Vorhandene Komponenten in Visual Studio 15.5 funktionieren weiterhin und **Komponenten** -Knoten wird wie gewohnt in der Projektmappe angezeigt.

Zukünftige Versionen von Visual Studio, jedoch werden _nicht_ wiederherstellen oder Aktualisieren von Komponenten.
Das heißt, wenn Sie die Lösung auf einem neuen Computer öffnen, die Komponente nicht heruntergeladen und installiert werden wird. und der Autor ist nicht in der Lage, auf die Bereitstellung von Updates. Sie sollten zu planen:

* Extrahieren Sie die Assemblys von der Komponente, und verweisen sie direkt in Ihrem Projekt.
* Wenden Sie sich an den Komponentenautor, und bitten Sie Informationen zu den Tarifen zu NuGet migriert.
* Untersuchen Sie alternative NuGet-Pakete, oder suchen Sie den Quellcode die Komponente ist Open Source.

Viele Komponentenhersteller noch über die Migration zu NuGet arbeiten, und andere Personen (einschließlich im Handel erhältlich Produkte) werden alternative Übermittlungsoptionen untersuchen.


## <a name="related-links"></a>Verwandte Links
- [Liste der beliebtesten Xamarin Plugins und Bibliotheken](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [Installieren und Verwenden eines NuGet-Pakets (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Einschließen eines NuGet-Pakets (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
