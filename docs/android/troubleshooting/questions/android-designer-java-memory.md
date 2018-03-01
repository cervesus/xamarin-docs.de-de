---
title: "Anpassen der Java-Speicher-Parameter für den Android-designer"
ms.topic: article
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 354a750fc57fd28754aea902d293e75b65d63997
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Anpassen der Java-Speicher-Parameter für den Android-designer

Die Standardparameter für den Arbeitsspeicher, die verwendet werden, beim Starten der `java` für der Android-Designer möglicherweise nicht kompatibel mit einigen Systemkonfigurationen verarbeiten.

Beginnend mit Xamarin Studio 5.7.2.7 (und höher, Visual Studio für Mac) und Xamarin für Visual Studio 3.9.344 diese Einstellungen auf der Basis eines pro Projekt angepasst werden können.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Neue Android-Designer-Eigenschaften und die entsprechenden Java-Optionen

Die folgenden Namen entsprechen den angegebenen Java [Befehlszeilenoption](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Öffnen Sie Ihre Projektmappe in Visual Studio.

2.  Wählen Sie jede Android-Projekt einzeln-im Projektmappen-Explorer, und klicken Sie auf [alle Dateien anzeigen](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) zweimal auf jedes Projekt. Sie können Projekte, die keine enthalten überspringen `.axml` Layout-Dateien. Dieser Schritt wird sichergestellt, dass jede Projektverzeichnis enthält eine `.csproj.user` Datei.

3.  Verlassen Sie Visual Studio.

4.  Suchen Sie die `.csproj.user` -Datei für jedes der Projekte aus Schritt 2.

5.  Bearbeiten Sie jede `.csproj.user` -Datei in einem Text-Editor.

6.  Hinzufügen einiger oder aller Eigenschaften für die neue Android-Designer-Arbeitsspeicher innerhalb einer `<PropertyGroup>` Element. Sie können ein vorhandenes `<PropertyGroup>` oder ein neues erstellen. Hier ist ein vollständiges Beispiel `.csproj.user` Datei, die alle 3 Attribute umfasst auf ihre Standardwerte festgelegt:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  Speichern und schließen Sie alle aktualisierten `.csproj.user` Dateien.

8.  Starten Sie Visual Studio neu, und öffnen Sie die Projektmappe.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Öffnen Sie die Projektmappe in Visual Studio für Mac, um sicherzustellen, dass das Projektmappenverzeichnis enthält eine `.userprefs` Datei.

2.  Beenden Sie Visual Studio für Mac.

3.  Suchen Sie die `.userprefs` Datei in dem Projektmappenverzeichnis.

4.  Bearbeiten der `.userprefs` -Datei in einem Text-Editor.

5.  Suchen Sie das vorhandene XML-Element mit dem folgenden Format an. Der letzte Teil dieses Elementname entspricht der Name des Projekts: "AndroidApplication1" in diesem Beispiel:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Wenn die `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` Element ist nicht vorhanden, erstellen Sie ihn an einer beliebigen Stelle innerhalb der einschließenden `<Properties>` Element. Achten Sie darauf, dass "AndroidApplication1" durch den Namen des Projekts zu ersetzen.

7.  Fügen Sie einige oder alle der neuen Android-Designer-Memory-Eigenschaften als Attribute für das Element hinzu. Hier ist ein vollständiges Beispiel `.userprefs` Datei, die alle 3 Attribute umfasst auf ihre Standardwerte festgelegt:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  Wiederholen Sie die Schritte 5 bis 7 für jede Android-Projekt in der Projektmappe, die alle enthält `.axml` Layout-Dateien. (D. h. Hinzufügen einer `<MonoDevelop.Ide.ItemProperties.ProjectName>` -Element für jedes Projekt.)

9.  Speichern und schließen Sie die `.userprefs` Datei.

10. Starten Sie Visual Studio für Mac, und öffnen Sie die Projektmappe.

-----

