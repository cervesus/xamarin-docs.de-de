---
title: Erstellen einer Xamarin.Forms-App mit einer Seite
description: In diesem Artikel wird erläutert, wie Sie eine einfache plattformübergreifende Single-Page-Anwendung mit Xamarin.Forms erstellen, mit der Sie eine Notiz eingeben und auf dem Gerät speichern können.
zone_pivot_groups: platform-dev16
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: c1d7aa1535fe979df222aaedc6ba2cf3bae0d51c
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "71679984"
---
# <a name="create-a-single-page-xamarinforms-application"></a>Erstellen einer Single-Page-Anwendung mit Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)

In diesem Schnellstart lernen Sie Folgendes:

- Erstellen einer plattformübergreifenden Xamarin.Forms-Anwendung
- Definieren der Benutzeroberfläche für eine Seite mithilfe der Extensible Application Markup Language (XAML)
- Interagieren mit XAML-Benutzeroberflächenelementen über Code

In diesem Schnellstart wird erläutert, wie Sie eine plattformübergreifende Xamarin.Forms-Anwendung erstellen, mit der Sie eine Notiz eingeben und auf dem Gerät speichern können. Die fertige Anwendung wird unten gezeigt:

[![](single-page-images/screenshots-sml.png "Notes Application")](single-page-images/screenshots.png#lightbox "Notes Application")

::: zone pivot="windows"

### <a name="prerequisites"></a>Voraussetzungen

- Visual Studio 2019 (neueste Version) mit der Workload **Mobile-Entwicklung mit .NET**
- Kenntnisse zu C#
- (optional) einen gekoppelten Mac, um die Anwendung unter iOS zu erstellen

Weitere Informationen zu diesen Voraussetzungen finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="get-started-with-visual-studio-2019"></a>Erste Schritte mit Visual Studio 2019

1. Starten Sie Visual Studio 2019, und klicken Sie im Startfenster auf **Neues Projekt erstellen**, um ein neues Projekt zu erstellen:

    ![](single-page-images/vs/new-solution-2019.png "New Project")

2. Klicken Sie im Fenster **Neues Projekt erstellen** in der Dropdownliste **Projekttyp** auf **Mobil**, wählen Sie die Vorlage **Mobile App (Xamarin.Forms)** aus, und klicken Sie auf **Weiter**:

    ![](single-page-images/vs/new-project-2019.png "Cross-Platform Project Templates")

3. Geben Sie im Fenster **Neues Projekt konfigurieren** den **Projektnamen** **Notes** (Notizen) ein, wählen Sie einen passenden Speicherort für das Projekt aus, und klicken Sie anschließend auf **Erstellen**:

    ![](single-page-images/vs/configure-project.png "Configure your Project")

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Schnellstart ist es erforderlich, dass die Projektmappe **Notes** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Schnellstart in die Projektmappe kopieren.

4. Klicken Sie im Dialogfeld **New Cross Platform App** (Neue plattformübergreifende App) auf **Leere App**, und klicken Sie auf **OK**:

    ![](single-page-images/vs/new-app-2019.png "New Cross-Platform App")

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](deepdive.md#anatomy-of-a-xamarinforms-application) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](deepdive.md).

5. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notes** auf die Datei **MainPage.xaml**, um sie zu öffnen:

    ![](single-page-images/vs/open-mainpage-xaml-2019.png "Open MainPage.xaml")

6. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Label`](xref:Xamarin.Forms.Label)-Klasse zum Anzeigen von Text, einem [`Editor`](xref:Xamarin.Forms.Editor) für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid) angeordnet, wobei die `Label`, `Editor` und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) angeordnet sind. Weitere Informationen zum Erstellen der Benutzeroberfläche finden Sie unter [Benutzeroberfläche](deepdive.md#user-interface) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

7. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **Notes**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen:

    ![](single-page-images/vs/open-mainpage-codebehind-2019.png "Open MainPage.xaml.cs")

8. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Dieser Code definiert ein `_fileName`-Feld, das auf eine Datei namens `notes.txt` verweist, die Notes-Daten im lokalen Anwendungsdatenordner der Anwendung speichert. Wenn der Seitenkonstruktor ausgeführt wird, wird die Datei gelesen, falls sie vorhanden, und im [`Editor`](xref:Xamarin.Forms.Editor) angezeigt. Wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Element **Speichern** geklickt wird, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt, der den Inhalt des `Editor` in der Datei speichert. Wenn Sie auf das `Button`-Element **Löschen** klicken, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, der die Datei löscht, wenn sie vorhanden ist, und jeglichen Text aus dem `Editor` entfernt. Weitere Informationen zur Benutzerinteraktion finden Sie unter [Reagieren auf eine Benutzerinteraktion](deepdive.md#responding-to-user-interaction) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

### <a name="building-the-quickstart"></a>Erstellen des Schnellstarts

1. Klicken Sie in Visual Studio auf die Menüelemente **Erstellen > Projektmappe erstellen**, oder drücken Sie F6. Die Projektmappe wird erstellt, und eine Erfolgsmeldung wird in der Statusleiste von Visual Studio angezeigt:

      ![](single-page-images/vs/build-succeeded.png "Build Succeeded")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Projektmappe erfolgreich erstellt wird.

2. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Wiedergabetaste), um die Anwendung in Ihrem ausgewählten Android-Emulator zu starten:

    ![](single-page-images/vs/android-start.png "Visual Studio Android Toolbar")

    [![](single-page-images/vs/notes-android.png "Notes in the Android Emulator")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    Weitere Informationen zum Starten der Anwendung auf den verschiedenen Plattformen finden Sie unter [Starten der Anwendung auf den verschiedenen Plattformen](deepdive.md#launching-the-application-on-each-platform) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    > [!NOTE]
    > Die folgenden Schritte sollten nur durchgeführt werden, wenn Sie einen [gekoppelten Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) haben, der die Systemanforderungen für die Entwicklung von Xamarin.Forms erfüllt.

3. Klicken Sie in der Visual Studio-Symbolleiste mit der rechten Maustaste auf das Projekt **Notes.iOS**, und wählen Sie **Als Startprojekt festlegen** aus.

      ![](single-page-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. Drücken Sie in der Symbolleiste von Visual Studio die Schaltfläche **Start** (die dreieckige Schaltfläche, die einer Play-Taste ähnelt), um die Anwendung in dem von Ihnen gewählten [iOS-Remotesimulator](~/tools/ios-simulator/index.md) zu starten:

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    [![](single-page-images/vs/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vs/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    Weitere Informationen zum Starten der Anwendung auf den verschiedenen Plattformen finden Sie unter [Starten der Anwendung auf den verschiedenen Plattformen](deepdive.md#launching-the-application-on-each-platform) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

::: zone-end
::: zone pivot="win-vs2017"

### <a name="prerequisites"></a>Voraussetzungen

- Visual Studio 2017 mit der Workload **Mobile-Entwicklung mit .NET**
- Kenntnisse zu C#
- (optional) einen gekoppelten Mac, um die Anwendung unter iOS zu erstellen

Weitere Informationen zu diesen Voraussetzungen finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="get-started-with-visual-studio-2017"></a>Erste Schritte mit Visual Studio 2017

1. Starten Sie Visual Studio 2017, und klicken Sie auf der Startseite auf **Neues Projekt erstellen...** , um ein neues Projekt zu erstellen:

    ![](single-page-images/vs/new-solution.png "New Project")

2. Klicken Sie im Dialogfeld **Neues Projekt** auf **Plattformübergreifend**, wählen Sie die Vorlage **Mobile App (Xamarin.Forms)** aus, geben Sie für „Name“ **Notes** ein, wählen Sie einen geeigneten Speicherort für das Projekt aus, und klicken Sie auf **OK**:

    ![](single-page-images/vs/new-project.png "Cross-Platform Project Templates")

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Schnellstart ist es erforderlich, dass die Projektmappe **Notes** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Schnellstart in die Projektmappe kopieren.

3. Klicken Sie im Dialogfeld **New Cross Platform App** (Neue plattformübergreifende App) auf **Leere App**, wählen Sie **.NET Standard** als Strategie für die Codefreigabe aus, und klicken Sie auf **OK**:

    ![](single-page-images/vs/new-app.png "New Cross-Platform App")

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](deepdive.md#anatomy-of-a-xamarinforms-application) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](deepdive.md).

4. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notes** auf die Datei **MainPage.xaml**, um sie zu öffnen:

    ![](single-page-images/vs/open-mainpage-xaml.png "Open MainPage.xaml")

5. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Label`](xref:Xamarin.Forms.Label)-Klasse zum Anzeigen von Text, einem [`Editor`](xref:Xamarin.Forms.Editor) für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid) angeordnet, wobei die `Label`, `Editor` und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) angeordnet sind. Weitere Informationen zum Erstellen der Benutzeroberfläche finden Sie unter [Benutzeroberfläche](deepdive.md#user-interface) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

6. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **Notes**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen:

    ![](single-page-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

7. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Dieser Code definiert ein `_fileName`-Feld, das auf eine Datei namens `notes.txt` verweist, die Notes-Daten im lokalen Anwendungsdatenordner der Anwendung speichert. Wenn der Seitenkonstruktor ausgeführt wird, wird die Datei gelesen, falls sie vorhanden, und im [`Editor`](xref:Xamarin.Forms.Editor) angezeigt. Wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Element **Speichern** geklickt wird, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt, der den Inhalt des `Editor` in der Datei speichert. Wenn Sie auf das `Button`-Element **Löschen** klicken, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, der die Datei löscht, wenn sie vorhanden ist, und jeglichen Text aus dem `Editor` entfernt. Weitere Informationen zur Benutzerinteraktion finden Sie unter [Reagieren auf eine Benutzerinteraktion](deepdive.md#responding-to-user-interaction) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

### <a name="building-the-quickstart"></a>Erstellen des Schnellstarts

1. Klicken Sie in Visual Studio auf die Menüelemente **Erstellen > Projektmappe erstellen**, oder drücken Sie F6. Die Projektmappe wird erstellt, und eine Erfolgsmeldung wird in der Statusleiste von Visual Studio angezeigt:

      ![](single-page-images/vs/build-succeeded.png "Build Succeeded")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Projektmappe erfolgreich erstellt wird.

2. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Wiedergabetaste), um die Anwendung in Ihrem ausgewählten Android-Emulator zu starten:

    ![](single-page-images/vs/android-start.png "Visual Studio Android Toolbar")

    [![](single-page-images/vs/notes-android.png "Notes in the Android Emulator")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    Weitere Informationen zum Starten der Anwendung auf den verschiedenen Plattformen finden Sie unter [Starten der Anwendung auf den verschiedenen Plattformen](deepdive.md#launching-the-application-on-each-platform) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    > [!NOTE]
    > Die folgenden Schritte sollten nur durchgeführt werden, wenn Sie einen [gekoppelten Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) haben, der die Systemanforderungen für die Entwicklung von Xamarin.Forms erfüllt.

3. Klicken Sie in der Visual Studio-Symbolleiste mit der rechten Maustaste auf das Projekt **Notes.iOS**, und wählen Sie **Als Startprojekt festlegen** aus.

      ![](single-page-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. Drücken Sie in der Symbolleiste von Visual Studio die Schaltfläche **Start** (die dreieckige Schaltfläche, die einer Play-Taste ähnelt), um die Anwendung in dem von Ihnen gewählten [iOS-Remotesimulator](~/tools/ios-simulator/index.md) zu starten:

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    [![](single-page-images/vs/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vs/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    Weitere Informationen zum Starten der Anwendung auf den verschiedenen Plattformen finden Sie unter [Starten der Anwendung auf den verschiedenen Plattformen](deepdive.md#launching-the-application-on-each-platform) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>Voraussetzungen

- Visual Studio für Mac (neueste Version) mit installierter iOS- und Android-Unterstützung
- Xcode (neueste Version)
- Kenntnisse zu C#

Weitere Informationen zu diesen Voraussetzungen finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

## <a name="get-started-with-visual-studio-for-mac"></a>Erste Schritte mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und klicken Sie im Startfenster auf **Neu**, um ein neues Projekt zu erstellen:

    ![](single-page-images/vsmac/new-project.png "New Solution")

2. Klicken Sie im Dialogfeld **Vorlage für neues Projekt auswählen** auf **Multi-Plattform > App**, wählen Sie die Vorlage **Leere Forms-App** aus, und klicken Sie auf **Weiter**:

    ![](single-page-images/vsmac/choose-template.png "Choose a Template")

3. Benennen Sie im Dialog **Leere Formularanwendung konfigurieren** die neue Anwendung mit **Notes**, stellen Sie sicher, dass die Option **.NET-Standard verwenden** ausgewählt ist, und klicken Sie auf die Schaltfläche **Weiter**:    

    ![](single-page-images/vsmac/configure-app.png "Configure the Forms Application")

4. Behalten Sie im Dialogfeld **Ihre neue leere Formularanwendung konfigurieren** die Namen für die Projektmappe und das Projekt als **Notes** bei, wählen Sie einen passenden Speicherort für das Projekt, und klicken Sie auf die Schaltfläche **Erstellen**, um das Projekt zu erstellen:

    ![](single-page-images/vsmac/configure-project.png "Configure the Forms Project")

    > [!IMPORTANT]
    > Die C#- und XAML-Codeausschnitte in diesem Schnellstart erfordern, dass die Projektmappe und das Projekt beide den Namen **Notes** tragen. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Schnellstart in das Projekt kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](deepdive.md#anatomy-of-a-xamarinforms-application) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](deepdive.md).

5. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf die Datei **MainPage.xaml**, um sie zu öffnen:

    ![](single-page-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. Entfernen Sie in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus einer [`Label`](xref:Xamarin.Forms.Label)-Klasse zum Anzeigen von Text, einem [`Editor`](xref:Xamarin.Forms.Editor) für die Texteingabe und zwei [`Button`](xref:Xamarin.Forms.Button)-Instanzen besteht, die die Anwendung zum Speichern oder Löschen einer Datei anweisen. Die beiden `Button`-Instanzen sind horizontal in einem [`Grid`](xref:Xamarin.Forms.Grid) angeordnet, wobei die `Label`, `Editor` und `Grid` vertikal in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) angeordnet sind. Weitere Informationen zum Erstellen der Benutzeroberfläche finden Sie unter [Benutzeroberfläche](deepdive.md#user-interface) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

7. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **Notes**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen:

    ![](single-page-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. Entfernen Sie in **MainPage.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    Dieser Code definiert ein `_fileName`-Feld, das auf eine Datei namens `notes.txt` verweist, die Notes-Daten im lokalen Anwendungsdatenordner der Anwendung speichert. Wenn der Seitenkonstruktor ausgeführt wird, wird die Datei gelesen, falls sie vorhanden, und im [`Editor`](xref:Xamarin.Forms.Editor) angezeigt. Wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Element **Speichern** geklickt wird, wird der `OnSaveButtonClicked`-Ereignishandler ausgeführt, der den Inhalt des `Editor` in der Datei speichert. Wenn Sie auf das `Button`-Element **Löschen** klicken, wird der `OnDeleteButtonClicked`-Ereignishandler ausgeführt, der die Datei löscht, wenn sie vorhanden ist, und jeglichen Text aus dem `Editor` entfernt. Weitere Informationen zur Benutzerinteraktion finden Sie unter [Reagieren auf eine Benutzerinteraktion](deepdive.md#responding-to-user-interaction) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

### <a name="building-the-quickstart"></a>Erstellen des Schnellstarts

1. Wählen Sie in Visual Studio für Mac die Menüelemente **Erstellen > Alle erstellen** aus, oder drücken Sie **&#8984;+B**. Das Projekt wird erstellt, und in der Symbolleiste von Visual Studio für Mac wird eine Erfolgsmeldung angezeigt.

      ![](single-page-images/vsmac/build-successful.png "Build Successful")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis das Projekt erfolgreich erstellt wird.

2. Klicken Sie im **Lösungspad** erst mit der rechten Maustaste auf das Projekt **Notes.iOS** und anschließend mit der linken auf **Set As Startup Project** (Als Startprojekt festlegen):

      ![](single-page-images/vsmac/set-startup-project-ios.png "Set iOS as Startup Project")

3. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator zu starten:

      ![](single-page-images/vsmac/start.png "Visual Studio for Mac Toolbar")

      [![](single-page-images/vsmac/notes-ios.png "Notes in the iOS Simulator")](single-page-images/vsmac/notes-ios-large.png#lightbox "Notes in the iOS Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    Weitere Informationen zum Starten der Anwendung auf den verschiedenen Plattformen finden Sie unter [Starten der Anwendung auf den verschiedenen Plattformen](deepdive.md#launching-the-application-on-each-platform) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

4. Klicken Sie im **Lösungspad** erst mit der rechten Maustaste auf das Projekt **Notes.Droid** und anschließend mit der linken auf **Als Startprojekt festlegen**:

      ![](single-page-images/vsmac/set-startup-project-android.png "Set Android as Startup Project")

5. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten Android-Emulator zu starten:

      [![](single-page-images/vsmac/notes-android.png "Notes in the Android Emulator")](single-page-images/vsmac/notes-android-large.png#lightbox "Notes in the Android Simulator")

    Geben Sie eine Notiz ein, und drücken Sie auf die Schaltfläche **Speichern**.

    Weitere Informationen zum Starten der Anwendung auf den verschiedenen Plattformen finden Sie unter [Starten der Anwendung auf den verschiedenen Plattformen](deepdive.md#launching-the-application-on-each-platform) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

- Erstellen einer plattformübergreifenden Xamarin.Forms-Anwendung
- Definieren der Benutzeroberfläche für eine Seite mithilfe der Extensible Application Markup Language (XAML)
- Interagieren mit XAML-Benutzeroberflächenelementen über Code

Fahren Sie mit dem nächsten Schnellstart fort, um diese Single-Page-Anwendung in eine Multi-Page-Anwendung umzuwandeln.

> [!div class="nextstepaction"]
> [Nächste](multi-page.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)
- [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md)
