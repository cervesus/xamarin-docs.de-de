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
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019543"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13

## <a name="about-the-android-support-libraries"></a>Informationen zu den Unterstützungsbibliotheken für Android

Google hat Unterstützungsbibliotheken erstellt, um neue Features für ältere Android-Versionen zur Verfügung zu stellen. Im Allgemeinen weisen Unterstützungsbibliotheken eine Versionsnummer im Namen auf, die der niedrigsten Android-API-Ebene entspricht, mit der sie kompatibel sind (z. B.: „Support-v4“ kann nur auf API-Ebene 4 und höher verwendet werden. Weitere Informationen dazu finden Sie in dieser Diskussion zu [Stack Overflow](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Zwei der Unterstützungsbibliotheken – `Support-v4` und `Support-v13` – können nicht in der gleichen App verwendet werden, sie schließen sich gegenseitig aus. Dies liegt daran, dass `Support-v13` alle Typen und Implementierungen von `Support-v4` enthält. Wenn Sie versuchen, in ein und demselben Projekt auf beide Bibliotheken zu verweisen, werden Fehler aufgrund doppelter Typen zurückgegeben.

## <a name="problems-with-referencing"></a>Probleme beim Verweisen

Da `Support-v4` so beliebt geworden ist, sind mittlerweile viele Bibliotheken von Drittanbietern davon abhängig. Entwickler könnten auch eine Abhängigkeit von „Support-v13“ auswählen, aber _v4_ ist gängiger, weil diese Version allen Apps, die Drittanbieterbibliotheken verwenden, die Möglichkeit bietet, API-Ebenen bis hinunter zu Ebene 4 zu unterstützen.

Wenn die Xamarin-Bibliothek eines Drittanbieters auf die `Xamarin.Android.Support.v4.dll`-Bindung an `Support-v4` verweist, müssen alle Apps, die diese Bibliothek verwenden, ebenfalls auf `Xamarin.Android.Support.v4.dll` verweisen. Das wird dann zum Problem, wenn dieselbe App auch Funktionen der `Xamarin.Android.Support.v13.dll`-Bindung an `Support-v13` nutzen soll. Wenn Sie auf beide Bindungen verweisen, treten Fehler aufgrund doppelter Typen auf.

## <a name="type-forwarded-v4-binding-assembly"></a>v4-Bindungsassembly mit Typweiterleitung

Um dieses Problem zu umgehen, haben wir eine spezielle `Xamarin.Android.Support.v4.dll`-Assembly erstellt, die keine Implementierung besitzt, sondern einfach `[assembly: TypeForwardedTo (..)]`-Attribute enthält, die alle `Support-v4`-Typen an die Implementierung innerhalb der `Xamarin.Android.Support.v13.dll`-Assembly weiterleiten.

Das bedeutet, dass ein Entwickler in einer App auf diese Assembly mit _Typweiterleitung_ verweisen kann. Damit wird der Verweis auf `Xamarin.Android.Support.v4.dll` von allen Drittanbieterbibliotheken erfüllt, und die Verwendung von `Xamarin.Android.Support.v13.dll` in der App ist weiterhin zulässig.

## <a name="nuget-assistance"></a>Unterstützung für NuGet

Entwickler könnten zwar die erforderlichen korrekten Verweise manuell hinzufügen, aber wir können auch NuGet verwenden, um die richtige Assembly auszuwählen (entweder die normale _v4_-Bindung oder die _v4_-Assembly mit Typweiterleitung), wenn das NuGet-Paket installiert ist.

Das NuGet-Paket `Xamarin.Android.Support.v4` enthält jetzt also die folgende Logik:

Wenn Ihre App für die API-Ebene 13 (Gingerbread 3.2) oder höher konzipiert ist, gilt Folgendes:

* Das NuGet-Paket `Xamarin.Android.Support.v13` wird automatisch als Abhängigkeit hinzugefügt.
* Im Projekt wird auf `Xamarin.Android.Support.v4.dll` _mit Typweiterleitung_ verwiesen.

Wenn Ihre App für eine niedrigere Ebene als die API-Ebene 13 konzipiert ist, können Sie in Ihrem Projekt auf die normale `Xamarin.Android.Support.v4.dll`-Bindung verweisen.

## <a name="do-i-have-to-use-support-v13"></a>Muss ich „Support-v13“ verwenden?

Wenn Ihre App für die API-Ebene 13 oder höher konzipiert ist und Sie das NuGet-Paket `Xamarin Android Support-v4` verwenden möchten, ist das NuGet-Paket `Xamarin Android Support v13` eine erforderliche Abhängigkeit.

Wir sind der Meinung, dass die unwesentliche Erhöhung der App-Größe (die beiden JAR-Dateien unterscheiden sich um 17 KB) die Kompatibilität und Vereinfachung wert ist.

Wenn Sie unbedingt `Support-v4` in einer App verwenden müssen, die für die API-Ebene 13 oder höher konzipiert ist, können Sie jederzeit die `.nupkg`-Datei herunterladen, extrahieren und auf die Assembly verweisen.
