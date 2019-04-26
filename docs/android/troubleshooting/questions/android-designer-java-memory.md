---
title: Anpassen der Java-Speicherparameter für Android Designer
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: 9c564789f704180e9acc9f96dcba5e7d6eb20634
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946737"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Anpassen der Java-Speicherparameter für Android Designer

Die Standardparameter für den Arbeitsspeicher, die verwendet werden, beim Starten der `java` für Android Designer möglicherweise nicht kompatibel mit einigen Systemkonfigurationen zu verarbeiten.

Beginnend mit Xamarin Studio 5.7.2.7 (und höher, Visual Studio für Mac) und Visual Studio-Tools für Xamarin 3.9.344, diese Einstellungen können individuell pro Projekt angepasst werden.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Neue Android-Designer-Eigenschaften und die entsprechenden Java-Optionen

Die folgenden Eigenschaftennamen entsprechen, zu dem angegebenen Java [Befehlszeilenoption](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Öffnen Sie Ihre Projektmappe in Visual Studio.

2.  Wählen Sie einzeln nacheinander jede Android-Projekt im Projektmappen-Explorer aus, und klicken Sie auf [alle Dateien anzeigen](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) zweimal auf jedes Projekt. Sie können Projekte, die keine enthalten überspringen `.axml` Layout-Dateien. Dieser Schritt wird sichergestellt, dass jede Projektverzeichnis enthält eine `.csproj.user` Datei.

3.  Verlassen Sie Visual Studio.

4.  Suchen Sie die `.csproj.user` -Datei für jedes der Projekte aus Schritt 2.

5.  Bearbeiten Sie `.csproj.user` -Datei in einem Text-Editor.

6.  Fügen Sie einige oder alle der neuen Android Designer Arbeitsspeicher Eigenschaften innerhalb einer `<PropertyGroup>` Element. Sie können eine vorhandene `<PropertyGroup>` oder ein neues erstellen. Hier ist ein vollständiges Beispiel `.csproj.user` Datei, die alle 3 Attribute enthält auf ihre Standardwerte festgelegt:

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

7.  Speichern und schließen Sie alle der aktualisierten `.csproj.user` Dateien.

8.  Starten Sie Visual Studio neu, und Ihre Projektmappe geöffnet.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Öffnen Sie die Projektmappe in Visual Studio für Mac, um sicherzustellen, dass das Projektmappenverzeichnis enthält eine `.userprefs` Datei.

2.  Visual Studio für Mac beenden

3.  Suchen Sie die `.userprefs` -Datei in das Projektmappenverzeichnis.

4.  Bearbeiten der `.userprefs` -Datei in einem Text-Editor.

5.  Suchen Sie das vorhandene XML-Element mit dem folgenden Format ein. Der letzte Teil der Name dieses Elements wird mit dem Namen des Projekts übereinstimmen: "AndroidApplication1" in diesem Beispiel:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Wenn die `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` Element ist nicht vorhanden, erstellen Sie ihn an einer beliebigen Stelle innerhalb der einschließenden `<Properties>` Element. Achten Sie darauf, dass "AndroidApplication1" durch den Namen des Projekts zu ersetzen.

7.  Fügen Sie einige oder alle der neuen Android Designer Arbeitsspeicher Eigenschaften als Attribute für das Element hinzu. Hier ist ein vollständiges Beispiel `.userprefs` Datei, die alle 3 Attribute enthält auf ihre Standardwerte festgelegt:

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

10. Starten Sie Visual Studio für Mac, und Ihre Projektmappe geöffnet.

-----

