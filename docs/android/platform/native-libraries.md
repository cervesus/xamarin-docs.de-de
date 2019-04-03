---
title: Verwenden nativer Bibliotheken
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1b0771a0ccc2597ebd800468b82044e4020d9d94
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854612"
---
# <a name="using-native-libraries"></a>Verwenden nativer Bibliotheken

Xamarin.Android unterstützt die Verwendung von systemeigenen Bibliotheken mithilfe der standardmäßigen PInvoke-Mechanismus. Sie können auch zusätzliche native Bibliotheken bündeln, die nicht Teil des Betriebssystems in Ihrem apk-Datei sind.

Um eine native Bibliothek mit einer Xamarin.Android-Anwendung bereitzustellen, die binäre-Bibliothek zum Projekt hinzufügen, und legen Sie dessen **Buildvorgang** zu **AndroidNativeLibrary**.

Um eine native Bibliothek mit einer Xamarin.Android-Library-Projekt bereitzustellen, die binäre-Bibliothek zum Projekt hinzufügen, und legen Sie dessen **Buildvorgang** zu **EmbeddedNativeLibrary**.

Beachten Sie, dass da Android mehrere Application Binary Interfaces (ABIs) unterstützt, Xamarin.Android welche ABI die native Bibliothek wissen müssen, ist für konzipiert.
Es gibt zwei Möglichkeiten, dies zu erreichen:

1.  Pfad "sniffing"
1.  Mithilfe einer `AndroidNativeLibrary/Abi` Element in der Projektdatei


Bei der Pfadermittlung wird der Name des übergeordneten Verzeichnisses der nativen Bibliothek verwendet, um die ABI anzugeben, die die Bibliothek als Ziel verwendet. Also, wenn Sie hinzufügen `lib/armeabi/libfoo.so` auf das Projekt, klicken Sie dann die ABI wird "ermittelt" als `armeabi`.

Alternativ können Sie Ihrer Projektdatei, um explizit anzugeben, die ABI mit bearbeiten:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Weitere Informationen zur Verwendung von systemeigenen Bibliotheken finden Sie unter [Interop mit systemeigenen Bibliotheken](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio"></a>Debuggen von nativem Code mit Visual Studio

Bei Verwendung von *Visual Studio-2019* oder *Visual Studio 2017*, müssen Sie nicht Ihre Projektdateien zu ändern, wie oben beschrieben.
Erstellen und Debuggen von C++ in Ihrer Xamarin.Android-Projektmappe durch Hinzufügen eines Projektverweises ein c++ **dynamische freigegebene Bibliothek (Android)** Projekt.

Um systemeigenen C++-Code in Ihrem Projekt zu debuggen, gehen Sie folgendermaßen vor:

1. Doppelklicken Sie auf Projekt **Eigenschaften** , und wählen Sie die **Android-Optionen** Seite.
2. Führen Sie einen Bildlauf nach unten zum **Debugoptionen**.
3. In der **Debugger** wählen Sie im Dropdownmenü **C++** (anstelle der standardmäßigen **.Net (Xamarin)**).

Visual Studio C++-Entwickler können finden Sie unter der [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) Beispiel um testen, Debuggen von C++ 2019 für Visual Studio oder Visual Studio 2017 mit Xamarin, und beziehen sich auf unsere [Blogbeitrag](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) für Weitere Informationen.



## <a name="related-links"></a>Verwandte Links

- [SanAngeles_NativeDebug (Beispiel)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Entwickeln nativer Xamarin Android-Anwendungen](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
