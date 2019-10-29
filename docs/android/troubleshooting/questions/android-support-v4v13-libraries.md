---
title: NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: c74cac6a6d669385855999a565711a3fdc85f3b7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019543"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13

## <a name="about-the-android-support-libraries"></a>Informationen zu den Android-Unterstützungs Bibliotheken

Google hat Unterstützungs Bibliotheken erstellt, um älteren Versionen von Android neue Features zur Verfügung zu stellen. Im Allgemeinen erhalten Support Bibliotheken eine Versionsnummer in Ihrem Namen, bei der es sich um die niedrigste Android-API-Ebene handelt, mit der Sie kompatibel sind (z. b., wenn Support-V4 nur auf API-Ebene 4 oder höher verwendet werden kann. Weitere Informationen zu dieser [Stack Overflow Diskussion](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Zwei der Unterstützungs Bibliotheken: "`Support-v4`" und "`Support-v13`" können nicht gemeinsam in derselben App verwendet werden, d. h., Sie schließen sich gegenseitig aus. Der Grund hierfür ist, dass `Support-v13` tatsächlich alle Typen und die Implementierung von `Support-v4`enthält. Wenn Sie versuchen, auf beide im gleichen Projekt zu verweisen, werden doppelte Typfehler auftreten.

## <a name="problems-with-referencing"></a>Probleme beim verweisen

Da `Support-v4` so beliebt geworden ist, hängen viele Bibliotheken von Drittanbietern ab. Möglicherweise haben Sie sich für die Abhängigkeit von "Support V13" entschieden, aber es ist eher üblich, von _V4_ abhängig zu sein, da dadurch alle apps, die diese Drittanbieterbibliotheken verwenden, die Möglichkeit haben, API-Ebenen bis zu 4 zu unterstützen.

Wenn eine xamarin-Drittanbieter Bibliothek auf die `Xamarin.Android.Support.v4.dll` Bindung an `Support-v4`verweist, muss jede APP, die diese Bibliothek verwendet, auch auf `Xamarin.Android.Support.v4.dll`verweisen. Dies wird zu einem Problem, wenn dieselbe APP auch einige der Funktionen von der `Xamarin.Android.Support.v13.dll` Bindung an `Support-v13`verwenden möchte. Wenn Sie auf beide Bindungen verweisen, treten doppelte Typfehler auf.

## <a name="type-forwarded-v4-binding-assembly"></a>Typweiter geleitete V4-bindungsassembly

Um dieses Problem zu umgehen, haben wir eine spezielle `Xamarin.Android.Support.v4.dll` Assembly erstellt, die keine Implementierung aufweist, sondern lediglich Attribute `[assembly: TypeForwardedTo (..)]`, die alle `Support-v4` Typen an die Implementierung innerhalb der `Xamarin.Android.Support.v13.dll` Assembly weiterleiten.

Dies bedeutet, dass ein Entwickler in der APP auf diese _Typ-weitergeleitete_ Assembly verweisen kann, die den Verweis auf die `Xamarin.Android.Support.v4.dll` von Drittanbieterbibliotheken erfüllt, während `Xamarin.Android.Support.v13.dll` weiterhin in der APP verwendet werden kann.

## <a name="nuget-assistance"></a>Nuget-Unterstützung

Während ein Entwickler die richtigen Verweise manuell hinzufügen konnte, können Sie nuget verwenden, um die richtige Assembly (entweder die normale _V4_ -Bindung oder die typweiter geleitete _V4_ -Assembly) zu verwenden, wenn das nuget-Paket installiert wird.

Das `Xamarin.Android.Support.v4` nuget-Paket enthält nun die folgende Logik:

Wenn Ihre APP auf API-Ebene 13 (Lebkuchen 3,2) oder höher ausgerichtet ist:

* `Xamarin.Android.Support.v13` nuget automatisch als Abhängigkeit hinzugefügt wird
* Auf den Typ, auf den _weitergeleitet_ wird, `Xamarin.Android.Support.v4.dll` im Projekt verwiesen wird.

Wenn Ihre APP etwas niedriger als API-Ebene 13 ist, erhalten Sie die normale `Xamarin.Android.Support.v4.dll` Bindung, auf die in Ihrem Projekt verwiesen wird.

## <a name="do-i-have-to-use-support-v13"></a>Muss ich Support V13 verwenden?

Wenn Ihre APP auf API-Ebene 13 oder höher ausgerichtet ist und Sie das `Xamarin Android Support-v4` nuget-Paket verwenden möchten, ist das `Xamarin Android Support v13` nuget-Paket eine erforderliche Abhängigkeit.

Wir sind der Meinung, dass der geringfügige Anstieg der APP-Größe (die beiden JAR-Dateien unterscheiden sich um 17 KB) die Kompatibilität und weniger Probleme, die Sie verursacht.

Wenn Sie `Support-v4` in einer App verwenden, die auf API-Ebene 13 oder höher ausgerichtet ist, können Sie die `.nupkg`jederzeit manuell herunterladen, extrahieren und auf die Assembly verweisen.
