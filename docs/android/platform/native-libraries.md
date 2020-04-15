---
title: Verwenden nativer Bibliotheken
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: b7d69e99327aa3d3e3e1f5e5dbc61697d1fb9b71
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "75489166"
---
# <a name="using-native-libraries"></a>Verwenden nativer Bibliotheken

Xamarin.Android unterstützt die Verwendung nativer Bibliotheken über den PInvoke-Standardmechanismus. Sie können native Bibliotheken, die nicht Teil des Betriebssystems in Ihrer APK-Datei sind, auch bündeln.

Wenn Sie eine native Bibliothek mit einer Xamarin.Android-Anwendung bereitstellen möchten, fügen Sie dem Projekt die binäre Bibliotheksdatei hinzu, und legen Sie die **Buildaktion** auf **AndroidNativeLibrary** fest.

Wenn Sie eine native Bibliotheken mit einem Xamarin.Android-Bibliotheksprojekt bereitstellen möchten, fügen Sie dem Projekt die binäre Bibliotheksdatei hinzu, und legen Sie dessen **Buildaktion** auf **EmbeddedNativeLibrary** fest.

Beachten Sie Folgendes: Da Android mehrere ABIs (Application Binary Interfaces) unterstützt, muss Xamarin.Android wissen, für welche ABI die native Bibliothek erstellt wird.
Es gibt zwei Möglichkeiten, dies zu erreichen:

1. Pfadermittlung
1. Verwendung eines `AndroidNativeLibrary/Abi`-Elements in der Projektdatei

Bei der Pfadermittlung wird der Name des übergeordneten Verzeichnisses der nativen Bibliothek verwendet, um die ABI anzugeben, die die Bibliothek als Ziel verwendet. Wenn Sie dem Projekt also `lib/armeabi/libfoo.so` hinzufügen, wird die ABI als `armeabi` „ermittelt“.

Alternativ können Sie Ihre Projektdatei auch so ändern, dass Sie explizit die zu verwendende ABI angibt.

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Weitere Informationen zum Verwenden nativer Bibliotheken finden Sie auf der Seite zu [Interop mit nativen Bibliotheken](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio"></a>Debuggen von nativem Code mit Visual Studio

Wenn Sie *Visual Studio 2019* oder *Visual Studio 2017* verwenden, müssen Sie Ihre Projektdatei nicht wie oben beschrieben ändern.
Sie können C++ innerhalb Ihrer Xamarin.Android-Projektmappe ändern, indem Sie einen Projektverweis zu einem **Projekt für die dynamische freigegebene Bibliothek in C++** hinzufügen.

Führen Sie folgende Schritt durch, um nativen C++-Code in Ihrem Projekt zu debuggen:

1. Doppelklicken Sie auf die **Projekteigenschaften**, und klicken Sie auf die Seite **Android-Optionen**.
2. Scrollen Sie nach unten zu **Debugoptionen**.
3. Wählen Sie im Dropdownmenü zu **Debugger** die Option **C++** (anstelle der Standardeinstellung **.NET (Xamarin)** ) aus.

Visual Studio C++-Entwickler können das Beispiel [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk) anzeigen, um C++ über Visual Studio 2019 oder Visual Studio 2017 mit Xamarin zu debuggen. Weitere Informationen finden Sie in unserem [Blogbeitrag](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/).

## <a name="related-links"></a>Verwandte Links

- [SanAngeles_NativeDebug (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Entwickeln nativer Xamarin.Android-Anwendungen](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
