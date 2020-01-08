---
title: Verwenden nativer Bibliotheken
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: b7d69e99327aa3d3e3e1f5e5dbc61697d1fb9b71
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489166"
---
# <a name="using-native-libraries"></a>Verwenden nativer Bibliotheken

Xamarin. Android unterstützt die Verwendung von nativen Bibliotheken über den standardmäßigen PInvoke-Mechanismus. Sie können auch zusätzliche systemeigene Bibliotheken bündeln, die nicht Teil des Betriebssystems in ihrer APK-APP sind.

Fügen Sie zum Bereitstellen einer nativen Bibliothek mit einer xamarin. Android-Anwendung die Bibliotheks Binärdatei zum Projekt hinzu, und legen Sie die **Buildaktion** auf **androidnativelibrary**fest.

Fügen Sie zum Bereitstellen einer nativen Bibliothek mit einem xamarin. Android-Bibliotheksprojekt die Bibliotheks Binärdatei zum Projekt hinzu, und legen Sie die **Buildaktion** auf **embeddednativelibrary**fest.

Da Android mehrere Anwendungs Binär Schnittstellen (ABIS) unterstützt, muss xamarin. Android wissen, für welche ABI die native Bibliothek erstellt wurde.
Es gibt zwei Möglichkeiten, dies zu erreichen:

1. Pfad "Sniffing"
1. Mithilfe eines `AndroidNativeLibrary/Abi`-Elements in der Projektdatei

Bei der Pfadermittlung wird der Name des übergeordneten Verzeichnisses der nativen Bibliothek verwendet, um die ABI anzugeben, die die Bibliothek als Ziel verwendet. Wenn Sie dem Projekt also `lib/armeabi/libfoo.so` hinzufügen, wird die ABI als `armeabi`"ermittelt".

Alternativ können Sie die Projektdatei bearbeiten, um die zu verwendende ABI explizit anzugeben:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Weitere Informationen zum Verwenden von nativen Bibliotheken finden Sie unter [Interop mit nativen Bibliotheken](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio"></a>Debuggen von nativem Code mit Visual Studio

Wenn Sie *Visual Studio 2019* oder *Visual Studio 2017*verwenden, müssen Sie die Projektdateien nicht wie oben beschrieben ändern.
Sie können in der xamarin. Android-Projekt Mappe erstellen und Debuggen C++ , indem Sie C++ einem Projekt für eine **dynamische freigegebene Bibliothek (Android)** einen Projekt Verweis hinzufügen.

Gehen Sie folgen C++ dermaßen vor, um systemeigenen Code in Ihrem Projekt zu Debuggen:

1. Doppelklicken Sie auf Projekt **Eigenschaften** , und wählen Sie die Seite **Android-Optionen** aus.
2. Scrollen Sie nach unten zu **Debugoptionen**.
3. Wählen Sie **C++** im Dropdown Menü des **Debuggers** (anstelle des standardmäßigen **.net (xamarin)** ) aus.

Visual Studio C++ -Entwickler können das [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk) Beispiel sehen, um C++ das Debuggen von Visual Studio 2019 oder Visual Studio 2017 mit xamarin zu testen. Weitere Informationen finden Sie in unserem [Blogbeitrag](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) .

## <a name="related-links"></a>Verwandte Themen

- [SanAngeles_NativeDebug (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Entwickeln von nativen xamarin Android-Anwendungen](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
