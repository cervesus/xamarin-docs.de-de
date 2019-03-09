---
title: .NET für Android einbetten
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: lobrien
ms.author: laobri
ms.date: 06/15/2018
---

# <a name="net-embedding-on-android"></a>.NET für Android einbetten

In einigen Fällen können Sie eine Bibliothek für .NET von Xamarin zu einem vorhandenen systemeigenen Android-Projekt hinzufügen möchten. Zu diesem Zweck können Sie die [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/) Tool, um Ihre .NET Bibliothek in eine systemeigene Bibliothek zu verwandeln, die in eine native Java-basierte Android-app integriert werden können.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android-Anforderungen

Für Xamarin.Android zum Einbetten von .NET arbeiten benötigen Sie Folgendes:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) or later must be installed.

-   **Java Developer Kit** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.


## <a name="using-embeddinator-4000"></a>Verwenden von Embeddinator-4000

Um eine .NET Bibliothek in ein systemeigenes Android-Projekt nutzen zu können, verwenden Sie die folgenden Schritte aus:

1.  Erstellen Sie eine C# Android-Bibliotheksprojekt.

2.  Installieren Sie [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Suchen Sie **Embeddinator-4000.exe** und Hinzufügen zu Ihrem **Pfad**. Zum Beispiel:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Führen Sie auf die Bibliotheksassembly Embeddinator-4000. Zum Beispiel:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android-Anforderungen

Für Xamarin.Android zum Einbetten von .NET arbeiten benötigen Sie Folgendes:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) or later must be installed.

-   **Java Developer Kit** &ndash; [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.

-   **Mono** &ndash; [Mono 5.0](https://www.mono-project.com/download/) oder höher muss installiert sein (Mono wird mit Visual Studio für Mac installiert).


## <a name="using-embeddinator-4000"></a>Verwenden von Embeddinator-4000

Um eine .NET Bibliothek in ein systemeigenes Android-Projekt nutzen zu können, verwenden Sie die folgenden Schritte aus:

1.  Erstellen Sie eine C# Android-Bibliotheksprojekt.

2.  Installieren Sie [Embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Suchen Sie **Embeddinator-4000.exe** und fügen **mono** zu Ihrem Pfad. Zum Beispiel:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Führen Sie auf die Bibliotheksassembly Embeddinator-4000. Zum Beispiel:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio.

-----

Nutzung und über die Befehlszeile Optionen werden beschrieben, der [Embeddinator-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) Dokumentation.


## <a name="callbacks"></a>Callbacks

Erfahren Sie mehr über [Aufrufe zwischen C# und Java](callbacks.md).

## <a name="samples"></a>Proben

* [Wetter-Beispiel-app](https://github.com/jamesmontemagno/embeddinator-weather)
