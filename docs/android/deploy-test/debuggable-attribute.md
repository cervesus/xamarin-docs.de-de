---
title: "Attribut „Debuggable“"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>Attribut „Debuggable“

<a name="Overview" />


Um das Debuggen zu ermöglichen, unterstützt Android das Java Debug Wire Protocol (JDWP). Dies ist eine Technologie, die es Tools wie adb ermöglicht, mit einer JVM zu kommunizieren. Während JDQP während der Entwicklung wichtig ist, sollte es vor der Veröffentlichung der Anwendung deaktiviert werden.

JDWP kann dem Wert des `android:debuggable`-Attributs in einer Android-Anwendung entsprechen. Xamarin.Android bietet die folgenden Möglichkeiten, dieses Attribut festzulegen:

1.  Erstellen einer`AndroidManifext.xml`-Datei und Festlegen des `android:debuggable`-Attributs
1.  Einschließen des `ApplicationAttribute` in einer `.CS`-Datei, wie hier dargestellt: `[assembly: Application(Debuggable=false)]`.


Wenn jeweils `AndroidManifest.xml` und `ApplicationAttribute` vorhanden sind, hat der Inhalt von `AndroidManifest.xml` Priorität vor dem, was durch `ApplicationAttribute` angegeben wird.

Wenn weder `AndroidManifest.xml` noch `ApplicationAttribute` vorhanden sind, hängt der Standardwert des `android:debuggable`-Attributs davon ab, ob Debugsymbole generiert werden oder nicht. Wenn Debugsymbole vorhanden sind, legt Xamarin.Android das `android:debuggable`-Attribut auf `true` fest.

Beachten Sie, dass der Wert des `android:debuggable`-Attributs NICHT zwingend von der Buildkonfiguration abhängt. Es ist möglich, für Releasebuilds das `android:debuggable`-Attribut auf TRUE festzulegen.


## <a name="related-links"></a>Verwandte Links

- [Debuggable apps in the Android market (Debugfähige Apps im Android Market)](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
