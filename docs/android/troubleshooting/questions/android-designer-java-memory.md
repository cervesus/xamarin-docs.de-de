---
title: Anpassen der Java-Speicherparameter für Android Designer
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: 4a3f3849725f0d3b8e8bc8d43c1cd3f87f044616
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761050"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Anpassen der Java-Speicherparameter für Android Designer

Die standardmäßigen Arbeitsspeicher Parameter, die beim Starten `java` des Prozesses für den Android-Designer verwendet werden, sind möglicherweise mit einigen Systemkonfigurationen nicht kompatibel.

Beginnend mit Xamarin Studio 5.7.2.7 (und höher Visual Studio für Mac) und Visual Studio-Tools für xamarin 3.9.344 können diese Einstellungen auf Projektbasis angepasst werden.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Neue Android-Designer-Eigenschaften und zugehörige Java-Optionen

Die folgenden Eigenschaftsnamen entsprechen der angegeben Java [-Befehlszeilenoption](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html) .

- **Androiddesignerjavarendererminmemory** -XMS

- **Androiddesignerjavarenderermaxmemory** -xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie Ihre Projektmappe in Visual Studio.

2. Wählen Sie jedes Android-Projekt nacheinander im Projektmappen-Explorer aus, und klicken Sie für jedes Projekt zweimal auf [alle Dateien anzeigen](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) . Sie können Projekte überspringen, die keine `.axml` Layoutdateien enthalten. Mit diesem Schritt wird sichergestellt, dass jedes Projekt `.csproj.user` Verzeichnis eine Datei enthält.

3. Verlassen Sie Visual Studio.

4. Suchen Sie `.csproj.user` die Datei für jedes der Projekte aus Schritt 2.

5. Bearbeiten Sie `.csproj.user` jede Datei in einem Text-Editor.

6. Fügen Sie eine oder alle der neuen Android-Designer-Arbeitsspeicher `<PropertyGroup>` Eigenschaften in einem-Element hinzu. Sie können eine vorhandene `<PropertyGroup>` verwenden oder eine neue erstellen. Im folgenden finden Sie eine `.csproj.user` vollständige Beispieldatei mit allen drei Attributen, die auf ihre Standardwerte festgelegt sind:

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

7. Speichern und schließen Sie alle aktualisierten `.csproj.user` Dateien.

8. Starten Sie Visual Studio neu, und öffnen Sie die Lösung erneut

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die Projekt Mappe in Visual Studio für Mac, damit das Projektmappenverzeichnis eine `.userprefs` Datei enthält.

2. Beenden Sie Visual Studio für Mac.

3. Suchen Sie `.userprefs` die Datei im Projektmappenverzeichnis.

4. Bearbeiten Sie `.userprefs` die Datei in einem Text-Editor.

5. Suchen Sie das vorhandene XML-Element im folgenden Format. Der letzte Teil dieses Element namens entspricht dem Namen des Projekts: "AndroidApplication1" in diesem Beispiel:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. Wenn das `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` Element nicht vorhanden ist, erstellen Sie es an beliebiger Stelle `<Properties>` innerhalb des einschließenden Elements. Stellen Sie sicher, dass Sie "AndroidApplication1" durch den Namen Ihres Projekts ersetzen.

7. Fügen Sie eine oder alle der neuen Arbeitsspeicher Eigenschaften für Android-Designer als Attribute für das-Element hinzu. Im folgenden finden Sie eine `.userprefs` vollständige Beispieldatei mit allen drei Attributen, die auf ihre Standardwerte festgelegt sind:

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

8. Wiederholen Sie die Schritte 5-7 für jedes Android-Projekt in der `.axml` Projekt Mappe, das alle Layoutdateien enthält. (Fügen Sie also ein `<MonoDevelop.Ide.ItemProperties.ProjectName>` -Element für jedes Projekt hinzu.)

9. Speichern und schließen Sie `.userprefs` die Datei.

10. Starten Sie Visual Studio für Mac neu, und öffnen Sie die Lösung erneut.

-----
