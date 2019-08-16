---
title: NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 64ceacc37a303e69b0dd7073ec6547ed344af51d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523431"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13

## <a name="about-the-android-support-libraries"></a>Informationen zu den Android-Unterstützungs Bibliotheken

Google hat Unterstützungs Bibliotheken erstellt, um älteren Versionen von Android neue Features zur Verfügung zu stellen. Im Allgemeinen erhalten Support Bibliotheken eine Versionsnummer im Namen, bei der es sich um die niedrigste Android-API-Ebene handelt, mit der Sie kompatibel sind (z. b. Support-V4 kann nur auf API-Ebene 4 oder höher verwendet werden. Weitere Informationen zu dieser [Stack Overflow Diskussion](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Zwei der Unterstützungs Bibliotheken: `Support-v4` und `Support-v13` können nicht gemeinsam in der gleichen App verwendet werden, d. h., Sie schließen sich gegenseitig aus. Der Grund hierfür `Support-v13` ist, dass tatsächlich alle Typen und Implementierungen von `Support-v4`enthält. Wenn Sie versuchen, auf beide im gleichen Projekt zu verweisen, werden doppelte Typfehler auftreten.

## <a name="problems-with-referencing"></a>Probleme beim verweisen

Da `Support-v4` schon so beliebt geworden ist, sind viele Bibliotheken von Drittanbietern jetzt davon abhängig. Möglicherweise haben Sie sich für die Abhängigkeit von "Support V13" entschieden, aber es ist eher üblich, von _V4_ abhängig zu sein, da dadurch alle apps, die diese Drittanbieterbibliotheken verwenden, die Möglichkeit haben, API-Ebenen bis zu 4 zu unterstützen.

Wenn eine xamarin-Drittanbieter Bibliothek auf die `Xamarin.Android.Support.v4.dll` Bindung an `Support-v4`verweist, muss jede APP, die diese Bibliothek verwendet `Xamarin.Android.Support.v4.dll`, auf verweisen. Dies wird zu einem Problem, wenn dieselbe APP auch einige der Funktionen von der `Xamarin.Android.Support.v13.dll` Bindung an `Support-v13`verwenden möchte. Wenn Sie auf beide Bindungen verweisen, treten doppelte Typfehler auf.

## <a name="type-forwarded-v4-binding-assembly"></a>Typweiter geleitete V4-bindungsassembly

Um dieses Problem zu umgehen, haben wir eine `Xamarin.Android.Support.v4.dll` spezielle Assembly erstellt, die keine Implementierung aufweist, sondern lediglich `[assembly: TypeForwardedTo (..)]` Attribute, `Support-v4` die alle Typen an die Implementierung innerhalb der `Xamarin.Android.Support.v13.dll` Assembly weiterleiten.

Dies bedeutet, dass `Xamarin.Android.Support.v13.dll` ein Entwickler in der APP auf diese _Typ-weitergeleitete_ Assembly verweisen kann, die `Xamarin.Android.Support.v4.dll` den Verweis auf von Drittanbieterbibliotheken erfüllt, während er weiterhin in der APP verwendet werden kann.

## <a name="nuget-assistance"></a>Nuget-Unterstützung

Während ein Entwickler die richtigen Verweise manuell hinzufügen konnte, können Sie nuget verwenden, um die richtige Assembly (entweder die normale _V4_ -Bindung oder die typweiter geleitete _V4_ -Assembly) zu verwenden, wenn das nuget-Paket installiert wird.

Das `Xamarin.Android.Support.v4` nuget-Paket enthält nun die folgende Logik:

Wenn Ihre APP auf API-Ebene 13 (Lebkuchen 3,2) oder höher ausgerichtet ist:

* `Xamarin.Android.Support.v13`Nuget wird automatisch als Abhängigkeit hinzugefügt.
* Auf den _Typ "weitergeleitet_ `Xamarin.Android.Support.v4.dll` " wird im Projekt verwiesen.

Wenn Ihre APP etwas niedriger als API-Ebene 13 ist, erhalten Sie die normale `Xamarin.Android.Support.v4.dll` Bindung, auf die in Ihrem Projekt verwiesen wird.

## <a name="do-i-have-to-use-support-v13"></a>Muss ich Support V13 verwenden?

Wenn Ihre APP auf API-Ebene 13 oder höher ausgerichtet ist und Sie das `Xamarin Android Support-v4` nuget-Paket verwenden möchten, ist das `Xamarin Android Support v13` nuget-Paket eine erforderliche Abhängigkeit.

Wir sind der Meinung, dass der geringfügige Anstieg der APP-Größe (die beiden JAR-Dateien unterscheiden sich um 17 KB) die Kompatibilität und weniger Probleme, die Sie verursacht.

Wenn Sie in einer APP, die `Support-v4` auf API-Ebene 13 oder höher ausgerichtet ist, nicht mehr verwenden, können Sie jederzeit `.nupkg`manuell herunterladen, extrahieren und auf die Assembly verweisen.
