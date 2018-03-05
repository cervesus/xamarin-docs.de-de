---
title: Die Anwendungslokalisierung und Zeichenfolgenressourcen
ms.topic: article
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: 9c65672ef2c3f968e76c6180da07f5daf9f5b68a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="application-localization-and-string-resources"></a>Die Anwendungslokalisierung und Zeichenfolgenressourcen

Die anwendungslokalisierung dient das Bereitstellen alternativer Ressourcen, um eine bestimmte Region oder das Gebietsschema als Ziel. Angenommen, Sie lokalisierte Zeichenfolgen für verschiedene Länder bereitstellen, oder Sie möglicherweise ändern, Farben oder Layout für bestimmte Kulturen übereinstimmen. Android geladen und die Ressourcen für das Gerät Gebietsschema entsprechende zur Laufzeit ohne Änderungen auf den Quellcode zu verwenden.

Z. B. die folgende Abbildung zeigt dieselbe Anwendung in drei verschiedenen Geräten Gebietsschemas ausgeführt, aber jede Schaltfläche angezeigte Text bezieht sich auf das Gebietsschema, das auf jedem Gerät festgelegt ist:

[![Beispiele für drei verschiedene Gebietsschemas](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png)

In diesem Beispiel den Inhalt einer Datei Layout **Main.axml** sieht etwa wie folgt:

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

Im obigen Beispiel wurde die Zeichenfolge für die Schaltfläche mit den von den Ressourcen geladen, indem Sie die Ressourcen-ID für die Zeichenfolge angeben:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ressourcenzeichenfolgen für drei Sprachen](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Ressourcenzeichenfolgen für drei Sprachen](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Lokalisieren von Android-Apps

Lesen der [Einführung zur Lokalisierung](~/cross-platform/app-fundamentals/localization.md) für Tipps und Anleitungen zum Lokalisieren von mobilen apps.

Die [Lokalisieren von Android-Apps](~/android/app-fundamentals/localization.md) Anleitung enthält genauere Beispiele zum Übersetzen von Zeichenfolgen und Lokalisieren von Abbildern mithilfe von Xamarin.Android.



## <a name="related-links"></a>Verwandte Links

- [Lokalisieren von Android-Apps](~/android/app-fundamentals/localization.md)
- [Plattformübergreifende Lokalisierung (Übersicht)](~/cross-platform/app-fundamentals/localization.md)
