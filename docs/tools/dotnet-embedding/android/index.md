---
title: .Net-Einbettung unter Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: davidortinau
ms.author: daortin
ms.date: 06/15/2018
ms.openlocfilehash: fef422b799ab5280aef205f4d5e55fd91050da39
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007340"
---
# <a name="net-embedding-on-android"></a>.Net-Einbettung unter Android

In einigen Fällen möchten Sie möglicherweise eine xamarin .NET-Bibliothek zu einem vorhandenen nativen Android-Projekt hinzufügen. Zu diesem Zweck können Sie das Tool [embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/) verwenden, um die .NET-Bibliothek in eine native Bibliothek umzuwandeln, die in eine native Java-basierte Android-App integriert werden kann.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="xamarinandroid-requirements"></a>Xamarin. Android-Anforderungen

Damit xamarin. Android mit der .net-Einbettung funktioniert, benötigen Sie Folgendes:

- **Xamarin. Android** &ndash;   [xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) oder höher muss installiert sein.

- **Android Studio** &ndash;   [Android Studio 3. x](https://developer.android.com/studio/) oder höher muss installiert sein.

- **Java Developer Kit** &ndash;   [Java 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.

## <a name="using-embeddinator-4000"></a>Verwenden von embeddinator-4000

Führen Sie die folgenden Schritte aus, um eine .NET-Bibliothek in einem nativen Android-Projekt zu verwenden:

1. Erstellen Sie C# ein Android-Bibliotheksprojekt.

2. Installieren Sie [embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3. Suchen Sie **Embeddinator-4000. exe** , und fügen Sie es dem **Pfad**hinzu. Beispiel:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4. Führen Sie embeddinator-4000 für die Bibliotheksassembly aus. Beispiel:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Verwenden Sie die generierte Aar-Datei in einem Java-Projekt in Android Studio.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="xamarinandroid-requirements"></a>Xamarin. Android-Anforderungen

Damit xamarin. Android mit der .net-Einbettung funktioniert, benötigen Sie Folgendes:

- **Xamarin. Android** &ndash;   [xamarin. Android 7,5](https://visualstudio.microsoft.com/xamarin/) oder höher muss installiert sein.

- **Android Studio** &ndash;   [Android Studio 3. x](https://developer.android.com/studio/) oder höher muss installiert sein.

- **Java Developer Kit** &ndash;   [Java 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.

- **Mono** &ndash;   [Mono 5,0](https://www.mono-project.com/download/) oder höher muss installiert sein (Mono wird mit Visual Studio für Mac installiert).

## <a name="using-embeddinator-4000"></a>Verwenden von embeddinator-4000

Führen Sie die folgenden Schritte aus, um eine .NET-Bibliothek in einem nativen Android-Projekt zu verwenden:

1. Erstellen Sie C# ein Android-Bibliotheksprojekt.

2. Installieren Sie [embeddinator-4000](https://www.nuget.org/packages/Embeddinator-4000/).

3. Suchen Sie **Embeddinator-4000. exe** , und fügen Sie **Mono** zu Ihrem Pfad hinzu. Beispiel:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4. Führen Sie embeddinator-4000 für die Bibliotheksassembly aus. Beispiel:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5. Verwenden Sie die generierte Aar-Datei in einem Java-Projekt in Android Studio.

-----

Verwendungs-und Befehlszeilenoptionen werden in der Dokumentation zu [embeddinator-4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) beschrieben.

## <a name="callbacks"></a>Rückrufe

Erfahren Sie, wie Sie [Aufrufe zwischen C# und Java tätigen](callbacks.md).

## <a name="samples"></a>Proben

- [Beispiel-App für Wetter](https://github.com/jamesmontemagno/embeddinator-weather)
