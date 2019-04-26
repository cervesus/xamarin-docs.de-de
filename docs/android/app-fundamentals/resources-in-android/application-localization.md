---
title: Anwendungslokalisierung und Zeichenfolgenressourcen
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: d9d90e371199c8587d61199240523cf0a23f5efd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013273"
---
# <a name="application-localization-and-string-resources"></a>Anwendungslokalisierung und Zeichenfolgenressourcen

Anwendungslokalisierung umfasst das Bereitstellen alternativer Ressourcen zur Ausrichtung auf eine bestimmte Region oder ein Gebietsschema. Beispielsweise können Sie Zeichenfolgen in einer lokalisierten Sprache für verschiedene Länder angeben, oder Sie möglicherweise ändern, Farben oder Layout mit bestimmte Kulturen übereinstimmen. Android lädt und verwenden Sie die Ressourcen, die für das Gerät, Gebietsschema geeignet, zur Laufzeit ohne Änderungen an den Quellcode.

Z. B. die folgende Abbildung zeigt der gleichen Anwendung in drei verschiedenen Geräten Gebietsschemas, aber jede Schaltfläche angezeigte Text bezieht sich auf das Gebietsschema, das auf jedem Gerät festgelegt ist:

[![Beispiele für drei verschiedene Gebietsschemas](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

In diesem Beispiel wird der Inhalt einer Datei Layout **Main.axml** sieht etwa folgendermaßen aus:

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

Im obigen Beispiel wurde die Zeichenfolge für die Schaltfläche mit den von den Ressourcen durch die Bereitstellung der Ressourcen-ID für die Zeichenfolge geladen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ressourcenzeichenfolgen für drei Sprachen](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ressourcenzeichenfolgen für drei Sprachen](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Lokalisieren von Android-Apps

Lesen der [Einführung in die Lokalisierung](~/cross-platform/app-fundamentals/localization.md) für Tipps und Anleitungen zum Lokalisieren von mobilen apps.

Die [Lokalisieren von Android-Apps](~/android/app-fundamentals/localization.md) Anleitung enthält genauere Beispiele zum Übersetzen von Zeichenfolgen und Bilder, die mithilfe von Xamarin.Android zu lokalisieren.



## <a name="related-links"></a>Verwandte Links

- [Lokalisieren von Android-Apps](~/android/app-fundamentals/localization.md)
- [Übersicht über die plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
