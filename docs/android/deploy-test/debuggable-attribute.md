---
title: "Attribut „Debuggable“"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 65037029d01d499421fd825f72347ae1bebd9966
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="debuggable-attribute"></a>Attribut „Debuggable“



Um das Debuggen zu ermöglichen, unterstützt Android das Java Debug Wire Protocol (JDWP). Dies ist eine Technologie, die es Tools wie adb ermöglicht, mit einer JVM zu kommunizieren. Während JDQP während der Entwicklung wichtig ist, sollte es vor der Veröffentlichung der Anwendung deaktiviert werden.

JDWP kann dem Wert des `android:debuggable`-Attributs in einer Android-Anwendung entsprechen. Xamarin.Android bietet die folgenden Möglichkeiten, dieses Attribut festzulegen:

1.  Erstellen einer`AndroidManifext.xml`-Datei und Festlegen des `android:debuggable`-Attributs
1.  Einschließen des `ApplicationAttribute` in einer `.CS`-Datei, wie hier dargestellt: `[assembly: Application(Debuggable=false)]`.


Wenn jeweils `AndroidManifest.xml` und `ApplicationAttribute` vorhanden sind, hat der Inhalt von `AndroidManifest.xml` Priorität vor dem, was durch `ApplicationAttribute` angegeben wird.

Wenn weder `AndroidManifest.xml` noch `ApplicationAttribute` vorhanden sind, hängt der Standardwert des `android:debuggable`-Attributs davon ab, ob Debugsymbole generiert werden oder nicht. Wenn Debugsymbole vorhanden sind, legt Xamarin.Android das `android:debuggable`-Attribut auf `true` fest.

Beachten Sie, dass der Wert des `android:debuggable`-Attributs NICHT zwingend von der Buildkonfiguration abhängt. Es ist möglich, für Releasebuilds das `android:debuggable`-Attribut auf TRUE festzulegen.


## <a name="related-links"></a>Verwandte Links

- [Debuggable apps in the Android market (Debugfähige Apps im Android Market)](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
