---
title: Wie aktiviere ich Intellisense in Android .axml-Dateien?
ms.topic: article
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: ea5c4e9f7e7f1c487d4f53650b10f88afc1b41e6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Wie aktiviere ich Intellisense in Android .axml-Dateien?

Android Intellisense in Visual Studio benötigt zwei XML-Schemadateien ordnungsgemäß funktioniert. Um diese Dateien zu suchen, wechseln Sie zu diesem Ordner:

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (Zuvor: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

Finden Sie in zwei XSD-Dateien:

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio speichert alle XML-Schemas in den entsprechenden Ordner:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

Sie können diese beiden XSD-Dateien auf den oben genannten Speicherort zu verschieben oder einfach nur diese Schemas innerhalb von Visual Studio hinzufügen. Anschließend können Sie "hinzufügen" Diese Schemas innerhalb von Visual Studio über die **XML > Schemas > Add** Dialogfeld.






