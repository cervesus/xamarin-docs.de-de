---
title: Anpassen der Java-Speicherparameter für Android Designer
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 9c9b9f5a205a2eef7db9f27e8d09b10ce65a4318
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027053"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Anpassen der Java-Speicherparameter für Android Designer

Die standardmäßigen Arbeitsspeicher Parameter, die beim Starten des `java` Prozesses für den Android-Designer verwendet werden, sind möglicherweise mit einigen Systemkonfigurationen nicht kompatibel.

Beginnend mit Xamarin Studio 5.7.2.7 (und höher Visual Studio für Mac) und Visual Studio-Tools für xamarin 3.9.344 können diese Einstellungen auf Projektbasis angepasst werden.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Neue Android-Designer-Eigenschaften und zugehörige Java-Optionen

Die folgenden Eigenschaftsnamen entsprechen der angegeben Java [-Befehlszeilenoption](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html) .

- **Androiddesignerjavarendererminmemory** -XMS

- **Androiddesignerjavarenderermaxmemory** -xmx

- **Androiddesignerjavarendererpermsize** -XX: maxpermsize

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie Ihre Projektmappe in Visual Studio.

2. Wählen Sie jedes Android-Projekt nacheinander im Projektmappen-Explorer aus, und klicken Sie für jedes Projekt zweimal auf [alle Dateien anzeigen](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) . Sie können Projekte überspringen, die keine `.axml` Layoutdateien enthalten. Mit diesem Schritt wird sichergestellt, dass jedes Projektverzeichnis eine `.csproj.user` Datei enthält.

3. Verlassen Sie Visual Studio.

4. Suchen Sie die `.csproj.user` Datei für jedes der Projekte aus Schritt 2.

5. Bearbeiten Sie jede `.csproj.user` Datei in einem Text-Editor.

6. Fügen Sie eine oder alle der neuen Arbeitsspeicher Eigenschaften für Android-Designer in einem `<PropertyGroup>`-Element hinzu. Sie können eine vorhandene `<PropertyGroup>` verwenden oder eine neue erstellen. Im folgenden finden Sie ein vollständiges Beispiel `.csproj.user` Datei mit allen drei Attributen, die auf ihre Standardwerte festgelegt sind:

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

1. Öffnen Sie die Projekt Mappe in Visual Studio für Mac, um sicherzustellen, dass das Projektmappenverzeichnis eine `.userprefs` Datei enthält

2. Beenden Sie Visual Studio für Mac.

3. Suchen Sie die `.userprefs`-Datei im Projektmappenverzeichnis.

4. Bearbeiten Sie die `.userprefs` Datei in einem Text-Editor.

5. Suchen Sie das vorhandene XML-Element im folgenden Format. Der letzte Teil dieses Element namens entspricht dem Namen Ihres Projekts: "AndroidApplication1" in diesem Beispiel:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. Wenn das `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` Element nicht vorhanden ist, erstellen Sie es an beliebiger Stelle innerhalb des einschließenden `<Properties>` Elements. Stellen Sie sicher, dass Sie "AndroidApplication1" durch den Namen Ihres Projekts ersetzen.

7. Fügen Sie eine oder alle der neuen Arbeitsspeicher Eigenschaften für Android-Designer als Attribute für das-Element hinzu. Im folgenden finden Sie ein vollständiges Beispiel `.userprefs` Datei mit allen drei Attributen, die auf ihre Standardwerte festgelegt sind:

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

8. Wiederholen Sie die Schritte 5-7 für jedes Android-Projekt in der Projekt Mappe, das alle `.axml` Layoutdateien enthält. (Fügen Sie also für jedes Projekt ein `<MonoDevelop.Ide.ItemProperties.ProjectName>`-Element hinzu.)

9. Speichern und schließen Sie die Datei `.userprefs`.

10. Starten Sie Visual Studio für Mac neu, und öffnen Sie die Lösung erneut.

-----
