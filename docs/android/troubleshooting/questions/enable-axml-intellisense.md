---
title: Wie aktiviere ich Intellisense in Android .axml-Dateien?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 8278576355299894eab1c7c6d3ceaadb607fe915
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
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






