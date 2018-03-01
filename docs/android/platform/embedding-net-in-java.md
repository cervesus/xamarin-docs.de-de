---
title: Einbetten von .NET in Java
description: 'Gewusst wie: Verwenden Sie eine Xamarin-.NET Bibliothek in einer Java-basierte systemeigenen Android-Projekt'
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 1a25f4bc39e39ce58a07ed399082bf13284c16e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="embedding-net-in-java"></a>Einbetten von .NET in Java

In einigen Fällen können Sie eine Xamarin-.NET Bibliothek zu einem vorhandenen systemeigenen Android-Projekt hinzufügen möchten. Zu diesem Zweck können Sie die [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) Tool, um eine systemeigene Bibliothek .NET Bibliothek verwandeln, die in einer systemeigenen Java-basierte Android-app integriert werden können.

 
## <a name="requirements"></a>Anforderungen

Um die Embeddinator-4000 mit Java auf Android verwenden zu können, benötigen Sie Folgendes:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) or later must be installed.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.4.99](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/) or later must be installed.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher muss installiert sein.

-   **Mono** &ndash; [Mono 5.0](http://www.mono-project.com/download/) oder höher muss installiert sein.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie können Visual Studio verwenden, bearbeiten und Kompilieren von C#-Code.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Sie können Visual Studio für Mac verwenden, bearbeiten und Kompilieren von C#-Code.

-----

 
## <a name="using-the-embeddinator-4000"></a>Verwenden die Embeddinator-4000

Um eine .NET-Bibliothek in einem systemeigenen Android-Projekt nutzen zu können, verwenden Sie die folgenden Schritte aus:

1.  Erstellen Sie eine C#-Android-Steuerelementbibliothek-Projekt.

2.  Installieren Sie Embeddinator-4000 über NuGet.

3.  Führen Sie Embeddinator für die Bibliothek für Android-Assembly.

4.  Verwenden Sie die generierte AAR-Datei in einem Java-Projekt in Android Studio.

Diese Schritte werden detailliert beschrieben die [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html) Dokumentation.
