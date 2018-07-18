---
title: .NET einbetten für Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067355"
---
# <a name="net-embedding-on-android"></a>.NET einbetten für Android

In einigen Fällen können Sie eine Xamarin-.NET Bibliothek zu einem vorhandenen systemeigenen Android-Projekt hinzufügen möchten. Zu diesem Zweck können Sie die [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/) Tool, um eine systemeigene Bibliothek .NET Bibliothek verwandeln, die in einer systemeigenen Java-basierte Android-app integriert werden können.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android-Anforderungen

Für Xamarin.Android einbetten .NET arbeiten benötigen Sie Folgendes:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) or later must be installed.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.


## <a name="using-embeddinator-4000"></a>Verwenden von Embeddinator 4000

Um eine .NET-Bibliothek in einem systemeigenen Android-Projekt nutzen zu können, verwenden Sie die folgenden Schritte aus:

1.  Erstellen Sie eine C#-Android-Steuerelementbibliothek-Projekt.

2.  Installieren Sie [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Suchen Sie **Embeddinator 4000.exe** und fügen sie Ihrem **Pfad**. Zum Beispiel:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Führen Sie für die Bibliotheksassembly Embeddinator-4000. Zum Beispiel:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Xamarin.Android-Anforderungen

Für Xamarin.Android einbetten .NET arbeiten benötigen Sie Folgendes:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) or later must be installed.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) or later must be installed.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.

-   **Mono-/** &ndash; [Mono / 5.0](http://www.mono-project.com/download/) oder höher muss installiert sein (Mono wird mit Visual Studio für Mac installiert).


## <a name="using-embeddinator-4000"></a>Verwenden von Embeddinator 4000

Um eine .NET-Bibliothek in einem systemeigenen Android-Projekt nutzen zu können, verwenden Sie die folgenden Schritte aus:

1.  Erstellen Sie eine C#-Android-Steuerelementbibliothek-Projekt.

2.  Installieren Sie [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Suchen Sie **Embeddinator 4000.exe** und hinzufügen **Mono /** zu Ihrem Pfad. Zum Beispiel:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Führen Sie für die Bibliotheksassembly Embeddinator-4000. Zum Beispiel:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio.

-----

Verwendung und über die Befehlszeile Optionen werden beschrieben, der [Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) Dokumentation.


## <a name="callbacks"></a>Rückrufe

Erfahren Sie mehr über [Aufrufe zwischen c# und Java](callbacks.md).

## <a name="samples"></a>Proben

* [Wetter-Beispiel-app](https://github.com/jamesmontemagno/embeddinator-weather)
