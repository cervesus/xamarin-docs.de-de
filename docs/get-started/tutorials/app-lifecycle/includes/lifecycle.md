---
ms.openlocfilehash: 247e75435f42a49d5d1ea01a4d0ec3da67866156
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "67277250"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **AppLifecycleTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **AppLifecycleTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Erweitern Sie **App.xaml** im **Projektmappen-Explorer** im Projekt **AppLifecycleTutorial**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen. Aktualisieren Sie anschließend in **App.xaml.cs** die Überschreibungen `OnStart`, `OnSleep` und `OnResume` wie folgt:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    Diese Code aktualisiert die Methodenüberschreibungen für den Lebenszyklus der App mit `Console.WriteLine`-Anweisungen, die angeben, wann jede Methode aufgerufen wurde:

    - Die `OnStart`-Methode wird aufgerufen, wenn die Anwendung gestartet wird.
    - Die `OnSleep`-Methode wird aufgerufen, wenn die Anwendung in den Hintergrund wechselt.
    - Die `OnResume`-Methode wird aufgerufen, wenn die Anwendung im Hintergrund fortgesetzt wird.

    > [!NOTE]
    > Es gibt keine Methode zum Beenden der App. Unter normalen Umständen erfolgt die Beendigung der Anwendung durch die `OnSleep`-Methode.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Beim Starten der Anwendung wird die `OnStart`-Methode aufgerufen, und **OnStart** wird an das **Ausgabe**-Fenster in Visual Studio ausgegeben:

    ```
    [Mono] Found as 'java_interop_jnienv_get_object_array_element'.
    OnStart
    [OpenGLRenderer] HWUI GL Pipeline
    ```

    Wenn die Anwendung in den Hintergrund wechselt (durch Tippen auf die „Home“-Taste unter iOS oder Android), wird die `OnSleep`-Methode ausgeführt:

    ```
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    OnSleep
    [Mono] Image addref System.Runtime.Serialization[0x83ee19c0] -> System.Runtime.Serialization.dll[0x83f57b00]: 2
    ```

    Wenn die Ausführung der Anwendung dann im Hintergrund fortgesetzt wird (tippen Sie unter iOS auf das Anwendungssymbol und unter Android auf das Übersichtssymbol, und wählen Sie die Anwendung „AppLifecycleTutorial“ aus), wird die `OnResume`-Methode aufgerufen:

    ```
    Thread finished: <Thread Pool> #5
    OnResume
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    ```

    > [!NOTE]
    > Diese Codeblöcke zeigen eine Beispielausgabe, wenn die Anwendung unter Android ausgeführt wird.

    Weitere Informationen zum Lebenszyklus der Xamarin.Forms-App finden Sie unter [Lebenszyklus der Xamarin.Forms-App](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **AppLifecycleTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **AppLifecycleTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Erweitern Sie **App.xaml** im **Lösungspad** im Projekt **AppLifecycleTutorial**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen. Aktualisieren Sie anschließend in **App.xaml.cs** die Überschreibungen `OnStart`, `OnSleep` und `OnResume` wie folgt:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    Diese Code aktualisiert die Methodenüberschreibungen für den Lebenszyklus der App mit `Console.WriteLine`-Anweisungen, die angeben, wann jede Methode aufgerufen wurde:

    - Die `OnStart`-Methode wird aufgerufen, wenn die Anwendung gestartet wird.
    - Die `OnSleep`-Methode wird aufgerufen, wenn die Anwendung in den Hintergrund wechselt.
    - Die `OnResume`-Methode wird aufgerufen, wenn die Anwendung im Hintergrund fortgesetzt wird.

    > [!NOTE]
    > Es gibt keine Methode zum Beenden der App. Unter normalen Umständen erfolgt die Beendigung der Anwendung durch die `OnSleep`-Methode.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten: Beim Starten der Anwendung wird die `OnStart`-Methode aufgerufen, und **OnStart** wird an das **Anwendungsausgabe**-Fenster in Visual Studio für Mac ausgegeben:

    ```
    2019-02-11 12:05:23.164761+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:05:23.165297+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    2019-02-11 12:05:23.685430+0000 AppLifecycleTutorial.iOS[4089:361037] OnStart
    ```

    Wenn die Anwendung in den Hintergrund wechselt (durch Tippen auf die „Home“-Taste unter iOS oder Android), wird die `OnSleep`-Methode ausgeführt:

    ```
    2019-02-11 12:06:12.394301+0000 AppLifecycleTutorial.iOS[4089:361037] OnSleep
    Thread started: <Thread Pool> #3
    Thread started: <Thread Pool> #4
    Thread started: <Thread Pool> #5
    Thread started: <Thread Pool> #6
    2019-02-11 12:06:13.056968+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:06:13.057264+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    ```

    Wenn die Ausführung der Anwendung dann im Hintergrund fortgesetzt wird (tippen Sie unter iOS auf das Anwendungssymbol und unter Android auf das Übersichtssymbol, und wählen Sie die Anwendung „AppLifecycleTutorial“ aus), wird die `OnResume`-Methode aufgerufen:

    ```
    2019-02-11 12:07:10.321905+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4130
    2019-02-11 12:07:10.322557+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskCopyDebugDescription: AppLifecycleTuto[4130]/0#-1 LF=0
    2019-02-11 12:07:17.110938+0000 AppLifecycleTutorial.iOS[4130:365695] OnResume
    ```

    > [!NOTE]
    > Diese Codeblöcke zeigen eine Beispielausgabe, wenn die Anwendung unter iOS ausgeführt wird.

    Weitere Informationen zum Lebenszyklus der Xamarin.Forms-App finden Sie unter [Lebenszyklus der Xamarin.Forms-App](~/xamarin-forms/app-fundamentals/app-lifecycle.md).
