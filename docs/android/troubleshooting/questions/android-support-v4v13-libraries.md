---
title: NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: a990d933c258812b2b3d3374fb6435af06f729ea
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671792"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>NuGet-Pakete für eine intelligentere Xamarin Android-Unterstützung v4/v13

## <a name="about-the-android-support-libraries"></a>Über die Android-Unterstützungsbibliotheken

Google hat Unterstützungsbibliotheken, um neue Features auf ältere Versionen von Android verfügbar zu machen. Im Allgemeinen sind Unterstützungsbibliotheken eine Versionsnummer angegeben, in deren Namen, die die niedrigste Android-API-Ebene ist, sind kompatibel mit (z.B.: Support-v4-kann nur auf API-Ebene-4 und höher verwendet werden. Weitere Informationen in diesem [Diskussion auf Stack Overflow](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Zwei der Unterstützungsbibliotheken: `Support-v4` und `Support-v13` kann nicht verwendet werden zusammen in derselben app, also stellen sich gegenseitig ausschließende. Grund hierfür ist, `Support-v13` enthält alle Typen und Implementierung von `Support-v4`. Wenn Sie versuchen, und verweisen auf beide im selben Projekt werden doppelte Typnamen Fehler auftreten.

## <a name="problems-with-referencing"></a>Probleme mit Verweisen auf

Da `Support-v4` geworden ist so beliebt, viele 3. Bibliotheken von Drittanbietern jetzt ihm abhängig sind. Sie hängt von Support-v13 stattdessen hätten, aber dies ist eher üblich, die von abhängig sind _v4_ da, die alle apps, die diese 3. Bibliotheken von Drittanbietern mithilfe die Option für die Unterstützung von API-Ebenen bis 4 bietet.

Wenn Sie eine Xamarin-3rd Party-Bibliothek verweist die `Xamarin.Android.Support.v4.dll` Binden an `Support-v4`, muss auch auf jede app, die diese Bibliothek verwendet verweisen `Xamarin.Android.Support.v4.dll`. Dies ist ein Problem, wenn die gleiche app auch einige der Funktionen verwenden möchte die `Xamarin.Android.Support.v13.dll` Binden an `Support-v13`. Wenn Sie beide Bindungen verweisen, werden doppelte Typnamen Fehler auftreten.

## <a name="type-forwarded-v4-binding-assembly"></a>Typweiterleitung v4 der Assemblybindung

Um dieses Problem zu umgehen, haben wir eine spezielle `Xamarin.Android.Support.v4.dll` Assembly, die keine Implementierung, sondern vielmehr hat `[assembly: TypeForwardedTo (..)]` Attribute, die alle Weiterleiten der `Support-v4` Typen für die Implementierung innerhalb der `Xamarin.Android.Support.v13.dll` Assembly.

Dies bedeutet, dass ein Entwickler kann dies verweisen _typweiterleitung_ Assembly in der app, die den Verweis auf erfüllt `Xamarin.Android.Support.v4.dll` durch alle 3rd Party-Bibliotheken, während nach wie vor `Xamarin.Android.Support.v13.dll` in der app verwendet werden.

## <a name="nuget-assistance"></a>NuGet-Unterstützung

Während ein Entwickler die richtigen Verweise erforderlichen manuell hinzufügen kann, wir können NuGet verwenden, um die richtige Assembly auswählen (entweder die normale _v4_ Bindung oder die typweiterleitung _v4_ Assembly) bei Das NuGet-Paket installiert ist.

Also die `Xamarin.Android.Support.v4` NuGet-Paket enthält nun die folgende Logik:

Wenn Ihre app-API-Ebene 13 (Gingerbread 3.2) ausgerichtet ist oder höher:

*   `Xamarin.Android.Support.v13` NuGet wird automatisch als Abhängigkeit hinzugefügt werden
*   Die _typweiterleitung_ `Xamarin.Android.Support.v4.dll` wird im Projekt verwiesen werden

Wenn Ihre app etwas niedriger als API-Ebene 13 ausgelegt ist, erhalten Sie die normale `Xamarin.Android.Support.v4.dll` Bindung, die in Ihrem Projekt referenziert.

## <a name="do-i-have-to-use-support-v13"></a>Habe ich v13-Unterstützung verwenden?

Wenn Ihre app die API-Ebene 13 oder höher ausgelegt ist und Sie verwenden die `Xamarin Android Support-v4` NuGet-Paket, und klicken Sie dann die `Xamarin Android Support v13` NuGet-Paket ist eine erforderliche Abhängigkeit.

Wir können, dass die sehr geringfügige Erhöhung der app-Größe (die beiden JAR-Dateien unterscheiden sich von 17 kb) lohnt sich die Kompatibilität und weniger Aufwand, die ausgeführt werden.

Wenn Sie zur Verwendung von das Ding ziemlich bombenfest sind `Support-v4` in einer app, die auf API-Ebene 13 oder höher verwenden, können Sie immer manuell herunterladen der `.nupkg`, extrahieren Sie es aus, und verweisen auf die Assembly.
