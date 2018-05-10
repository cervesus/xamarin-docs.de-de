---
title: Aktualisieren von komponentenverweisen zu NuGet
description: Ersetzen Sie die Komponente verweist mit NuGet-Pakete in Zukunft Ihrer apps.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: 020a5a2182458e759626b9bdbf45b62b6e13d71a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="updating-component-references-to-nuget"></a>Aktualisieren von komponentenverweisen zu NuGet

> [!NOTE]
> Xamarin-Komponenten sind in Visual Studio nicht mehr unterstützt und durch NuGet-Pakete ersetzt werden sollte. Führen Sie die nachstehenden Anweisungen zum Komponentenverweise manuell aus Projekten zu entfernen.

Verweisen Sie mit den vorliegenden Anleitungen zum Hinzufügen von NuGet-Pakete auf [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) oder [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

## <a name="manually-removing-component-references"></a>Manuelles Entfernen Komponentenverweise

In Version für November 2017 war [angekündigt](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) , die von der Xamarin-Komponentenspeicher würde nicht mehr unterstützt werden. In dem Bestreben, die mit der Sunsetting Komponenten weitergehen unterstützen das 15.6 Release von Visual Studio und 7.4 Release von Visual Studio für Mac nicht mehr Komponenten in Ihrem Projekt. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie ein Projekt in Visual Studio laden, wird das folgende Dialogfeld angezeigt, erläutert wird, dass Sie alle Komponenten aus dem Projekt manuell entfernen müssen:

![Warnung Dialogfeld erläutert wird, dass eine Komponente in Ihr Projekt gefunden wurde und entfernt werden müssen](component-nuget-images/component-alert-vs.png)

So entfernen Sie eine Komponente aus Ihrem Projekt:

1. Öffnen der **csproj** Datei. Klicken Sie hierzu mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Projekt entladen**. 

2. Mit der rechten Maustaste erneut auf das Projekt entladen, und wählen Sie **{Ihr Projekt-Name} .csproj bearbeiten**.

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

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn Sie ein Projekt in Visual Studio für Mac laden, wird das folgende Dialogfeld angezeigt, erläutert wird, dass Sie alle Komponenten aus dem Projekt manuell entfernen müssen:

![Warnung Dialogfeld erläutert wird, dass eine Komponente in Ihr Projekt gefunden wurde und entfernt werden müssen](component-nuget-images/component-alert.png)

So entfernen Sie eine Komponente aus Ihrem Projekt:

1. Öffnen Sie die CSPROJ-Datei. Klicken Sie hierzu mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Tools > Datei bearbeiten**.

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
> Die **Komponenten** Knoten ist nicht mehr verfügbar ist, in den aktuellen Versionen von Visual Studio 2017 oder Visual Studio für Mac.

In den folgenden Abschnitten wird erläutert, wie Aktualisieren von vorhandenen Xamarin-Projektmappen, um Komponentenverweise auf NuGet-Pakete zu ändern.

- [Komponenten, die NuGet-Pakete enthalten](#contain)
- [Komponenten mit NuGet-Ersetzungen](#replace)

Die meisten Komponenten werden in eine der oben genannten Kategorien fallen.
Bei Verwendung eine Komponente, die nicht angezeigt wird, ein Äquivalent NuGet-Paket haben, lesen Sie die [Komponenten ohne einen Migrationspfad NuGet](#require-update) Abschnitt weiter unten.

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Komponenten, die NuGet-Pakete enthalten

Viele Komponenten bereits NuGet-Pakete enthalten, und der Migrationspfad besteht darin, das Verweisen auf Komponenten zu löschen.

Sie können ermitteln, ob die Komponente bereits ein NuGet-Paket enthält, durch Doppelklicken auf die Komponente in der Projektmappe:

![Komponenten Knoten erweitert](component-nuget-images/solution-sml.png)

Die **Pakete** Registerkarte listet alle NuGet-Pakete, die in der Komponente enthalten:

![Registerkarte "Pakete" enthält NuGet](component-nuget-images/packages-tab-sml.png)

Beachten Sie, dass die **Assemblys** Registerkarte leer sein wird:

![Assemblyregisterkarte ist leer](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualisieren die Projektmappe

Um die Projektmappe zu aktualisieren, löschen Sie die **Komponente** Eintrag aus der Projektmappe:

![Löschen Sie die Komponente](component-nuget-images/delete-component-sml.png)

Das NuGet-Paket bleibt aufgelisteten in der **Pakete** Knoten und die app kompilieren, und wie gewohnt ausgeführt. In zukünftigen Updates für dieses Paket erfolgt über die **Nuget** Updatefunktion:

![Aktualisieren von NuGet-Paket](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Komponenten mit NuGet-Ersetzungen

Wenn die Seite "Komponente" **Assemblys** Registerkarte enthält Einträge, wie unten dargestellt, müssen Sie manuell das entsprechende NuGet-Paket zu suchen.

![Enthält von Assemblys](component-nuget-images/assemblies-tab-sml.png)

Beachten Sie, dass die **Pakete** Registerkarte wahrscheinlich leer sein:

![](component-nuget-images/packages-tab-empty-sml.png)

_Sie kann NuGet Abhängigkeiten enthalten, aber diese Fehler können ignoriert werden._

Um ein Ersatz NuGet bestätigen Paket vorhanden ist, suchen Sie auf [NuGet.org](https://www.nuget.org/packages), verwenden den Namen der Komponente, oder Sie können auch vom Autor.

Beispielsweise können Sie dem beliebten finden **Sqlite-Net-Pcl** Paket durch Suchen nach:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – der Name des Produkts.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – der Autor Profil.

### <a name="updating-the-solution"></a>Aktualisieren die Projektmappe

Nachdem Sie, dass die Komponente im NuGet verfügbar ist sichergestellt haben, gehen Sie folgendermaßen vor:

#### <a name="delete-the-component"></a>Löschen Sie die Komponente

Klicken Sie mit der rechten Maustaste auf die Komponente in der Projektmappe, und wählen Sie **entfernen**:

![Entfernen Sie die Komponente](component-nuget-images/remove-component-sml.png)

Hierdurch wird die Komponente und alle Verweise gelöscht. Dies wird den Build unterbrochen, bis Sie das entsprechende NuGet-Paket, um ihn zu ersetzen hinzufügen.

#### <a name="add-the-nuget-package"></a>Fügen Sie das NuGet-Paket hinzu

1. Mit der rechten Maustaste auf die **Pakete** Knoten, und wählen Sie **Pakete hinzufügen...** .
2. Suchen Sie nach der NuGet-Ersatz anhand des Namens oder Autor:

  ![](component-nuget-images/nuget-search-sml.png)

3. Drücken Sie **Paket hinzufügen**.

Das NuGet-Paket wird dem Projekt, und alle Abhängigkeiten hinzugefügt werden.
Den Build sollte korrigiert werden. Wenn der Build fehlschlägt weiterhin, untersuchen Sie jeden Fehler, um festzustellen, ob die API-Unterschiede zwischen der Komponente und das NuGet-Paket wurden.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Ohne einen Migrationspfad NuGet-Komponenten

Lassen Sie sich Gedanken machen, wenn Sie sofort Ersatz für Komponenten, die in der Anwendung verwendet, die nicht finden. Vorhandene Komponenten werden weiterhin in Visual Studio 15.5 arbeiten und die **Komponenten** Knoten erscheint in der Projektmappe wie gewohnt.

Zukünftige Versionen von Visual Studio, jedoch werden _nicht_ wiederherstellen oder Aktualisieren von Komponenten.
Dies bedeutet, wenn Sie die Projektmappe auf einem neuen Computer öffnen, die Komponente nicht heruntergeladen und installiert werden. und der Autor ist nicht in der Lage, um Sie mit Updates bereitzustellen. Sie sollten zu planen:

* Extrahieren Sie die Assemblys von der Komponente, und verweisen sie direkt in Ihrem Projekt.
* Wenden Sie sich an den Komponentenautor, und bitten Sie Pläne zu NuGet migrieren.
* Untersuchen von alternativen NuGet-Pakete, oder den Quellcode seek, wenn die Komponente Open Source-ist.

Viele Komponente Anbieter weiterhin zum Migrieren zu NuGet arbeiten, und anderen Benutzern (einschließlich erhältlicher Produkte) untersucht alternative Übermittlungsoptionen.


## <a name="related-links"></a>Verwandte Links

- [Installieren und Verwenden von NuGet-Paket (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Sowie ein NuGet-Paket (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
