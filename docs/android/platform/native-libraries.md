---
title: Verwenden von systemeigenen Bibliotheken
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 0fa66f3a16047c18af19cb7257c778b498bc0c9b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="using-native-libraries"></a>Verwenden von systemeigenen Bibliotheken

Xamarin.Android unterstützt die Verwendung von systemeigenen Bibliotheken über PInvoke der Standardmechanismus. Sie können auch zusätzliche systemeigene Bibliotheken bündeln, die nicht Teil des Betriebssystems in Ihrer .apk sind.

Um eine systemeigene Bibliothek mit einer Anwendung Xamarin.Android bereitzustellen, die binäre Bibliothek zum Projekt hinzufügen, und legen Sie dessen **Buildvorgang** auf **AndroidNativeLibrary**.

Um eine systemeigene Bibliothek mit einem Xamarin.Android-Steuerelementbibliothek-Projekt bereitstellen, die binäre Bibliothek zum Projekt hinzufügen, und legen Sie dessen **Buildvorgang** auf **EmbeddedNativeLibrary**.

Beachten Sie, dass seit Android mehrere binäre Anwendungsschnittstellen (ABIs) unterstützt, Xamarin.Android welche ABI kennen muss für die systemeigene Bibliothek erstellt wird.
Es gibt zwei Möglichkeiten, dies zu erreichen:

1.  Pfad "sniffing"
1.  Mithilfe einer `AndroidNativeLibrary/Abi` Element in der Projektdatei


Bei der Pfadermittlung wird der Name des übergeordneten Verzeichnisses der nativen Bibliothek verwendet, um die ABI anzugeben, die die Bibliothek als Ziel verwendet. Daher, wenn Sie hinzufügen `lib/armeabi/libfoo.so` für das Projekt, klicken Sie dann die ABI wird werden "generieren" als `armeabi`.

Alternativ können Sie die Projektdatei, um explizit anzugeben, die ABI mit bearbeiten:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Weitere Informationen zur Verwendung von systemeigenen Bibliotheken finden Sie unter [Interop mit systemeigenen Bibliotheken](http://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio-2015"></a>Debuggen von systemeigenem Code mit Visual Studio 2015

Bei Verwendung von *Visual Studio 2015*, Sie müssen die Projektdateien ändern (wie oben beschrieben).
Erstellen und Debuggen von C++ innerhalb der Projektmappe Xamarin.Android einfach durch Hinzufügen eines Projekt-Verweises auf eine C++- **dynamische freigegebene Bibliothek (Android)** Projekt.

Visual Studio C++-Entwickler können finden Sie unter der [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) Beispiel zum Debuggen von C++ in Visual Studio 2015 mit Xamarin; und beziehen sich auf unser [Blogbeitrag](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) für Weitere Informationen.



## <a name="related-links"></a>Verwandte Links

- [SanAngeles_NativeDebug (sample)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
