---
title: Anwendungslokalisierung und Zeichenfolgenressourcen
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 45fe5c783e737fb913730082841e0dfafc555684
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755134"
---
# <a name="application-localization-and-string-resources"></a>Anwendungslokalisierung und Zeichenfolgenressourcen

Die Anwendungs Lokalisierung dient zum Bereitstellen alternativer Ressourcen für eine bestimmte Region oder ein bestimmtes Gebiets Schema. Beispielsweise können Sie lokalisierte sprach Zeichenfolgen für verschiedene Länder bereitstellen, oder Sie können die Farben oder das Layout ändern, um bestimmte Kulturen abzugleichen. Android lädt und verwendet die Ressourcen, die für das Gebiets Schema des Geräts zur Laufzeit ohne Änderungen am Quellcode geeignet sind.

Die folgende Abbildung zeigt beispielsweise die gleiche Anwendung, die in drei verschiedenen Geräte Gebiets Schemas ausgeführt wird. der in jeder Schaltfläche angezeigte Text ist jedoch spezifisch für das Gebiets Schema, auf das jedes Gerät festgelegt ist:

[![Beispiele für drei verschiedene Gebiets Schemas](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

In diesem Beispiel sieht der Inhalt der Layoutdatei **Main. axml** etwa wie folgt aus:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

Im obigen Beispiel wurde die Zeichenfolge für die Schaltfläche aus den Ressourcen geladen, indem die Ressourcen-ID für die Zeichenfolge bereitgestellt wird:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ressourcen Zeichenfolgen für drei Sprachen](application-localization-images/02-resource-strings-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ressourcen Zeichenfolgen für drei Sprachen](application-localization-images/02-resource-strings-xs.png)

-----

## <a name="localizing-android-apps"></a>Lokalisieren von Android-Apps

Tipps und Anleitungen zum Lokalisieren von mobilen apps finden Sie unter [Einführung in die Lokalisierung](~/cross-platform/app-fundamentals/localization.md) .

Im Leitfaden zum [Lokalisieren von Android-Apps](~/android/app-fundamentals/localization.md) finden Sie spezifischere Beispiele zum Übersetzen von Zeichen folgen und Lokalisieren von Bildern mithilfe von xamarin. Android.

## <a name="related-links"></a>Verwandte Links

- [Lokalisieren von Android-Apps](~/android/app-fundamentals/localization.md)
- [Übersicht über die plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
