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
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027053"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Anpassen der Java-Speicherparameter für Android Designer

Die Standardspeicherparameter, die beim Starten des `java`-Prozesses für den Android-Designer verwendet werden, sind möglicherweise mit einigen Systemkonfigurationen nicht kompatibel.

Ab Xamarin Studio 5.7.2.7 (und höher, Visual Studio für Mac) und Visual Studio-Tools für Xamarin 3.9.344 können diese Einstellungen projektbezogen angepasst werden.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Neue Eigenschaften für den Android-Designer und entsprechende Java-Optionen

Die folgenden Eigenschaftsnamen entsprechen der angegebenen [Befehlszeilenoption](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html) für Java.

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie Ihre Projektmappe in Visual Studio.

2. Wählen Sie die Android-Projekte einzeln im Projektmappen-Explorer aus, und doppelklicken Sie in jedem Projekt auf [Alle Dateien anzeigen](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)). Sie können Projekte überspringen, die keine `.axml`-Layoutdateien enthalten. Mit diesem Schritt stellen Sie sicher, dass jedes Projektverzeichnis eine `.csproj.user`-Datei enthält.

3. Verlassen Sie Visual Studio.

4. Suchen Sie die `.csproj.user`-Datei für jedes Projekt aus Schritt 2.

5. Bearbeiten Sie jede `.csproj.user`-Datei in einem Text-Editor.

6. Fügen Sie einige oder alle neuen Arbeitsspeicheroptionen für Android Designer in einem `<PropertyGroup>`-Element hinzu. Sie können ein vorhandenes `<PropertyGroup>`-Element verwenden oder ein neues erstellen. Im Folgenden sehen Sie eine vollständige `.csproj.user`-Beispieldatei mit allen drei Attributen, die auf ihre jeweiligen Standardwerte festgelegt sind:

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

7. Speichern und schließen Sie alle aktualisierten `.csproj.user`-Dateien.

8. Starten Sie Visual Studio neu, und öffnen Sie Ihre Projektmappe erneut.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie Ihre Projektmappe in Visual Studio für Mac, um sich zu vergewissern, dass das Projektmappenverzeichnis eine `.userprefs`-Datei enthält.

2. Beenden Sie Visual Studio für Mac.

3. Suchen Sie die `.userprefs`-Datei im Projektmappenverzeichnis.

4. Bearbeiten Sie die `.userprefs`-Datei in einem Text-Editor.

5. Suchen Sie das vorhandene XML-Element mit folgendem Format. Der letzte Teil des Namens dieses Elements entspricht dem Namen Ihres Projekts: In diesem Beispiel „AndroidApplication1“:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. Wenn das `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >`-Element nicht vorhanden ist, erstellen Sie es an einer beliebigen Stelle im umschließenden `<Properties>`-Element. Stellen Sie sicher, dass Sie „AndroidApplication1“ durch den Namen Ihres Projekts ersetzen.

7. Fügen Sie einige oder alle neuen Arbeitsspeicheroptionen für Android Designer als Attribut im Element hinzu. Im Folgenden sehen Sie eine vollständige `.userprefs`-Beispieldatei mit allen drei Attributen, die auf ihre jeweiligen Standardwerte festgelegt sind:

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

8. Wiederholen Sie die Schritte 5–7 für jedes Android-Projekt in der Projektmappe, das `.axml`-Layoutdateien enthält. (Fügen Sie also ein `<MonoDevelop.Ide.ItemProperties.ProjectName>`-Element für jedes Projekt hinzu.)

9. Speichern und schließen Sie die `.userprefs`-Datei.

10. Starten Sie Visual Studio für Mac neu, und öffnen Sie Ihre Projektmappe erneut.

-----
