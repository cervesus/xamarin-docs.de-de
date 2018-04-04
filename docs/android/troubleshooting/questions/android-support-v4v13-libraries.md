---
title: Intelligentere Xamarin Android unterstützt v4 / v13 NuGet-Pakete
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 47ae63fbd7062be97be04bfc1f1244ec63a1c5bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>Intelligentere Xamarin Android unterstützt v4 / v13 NuGet-Pakete

## <a name="about-the-android-support-libraries"></a>Über die Bibliotheken für Android-Unterstützung

Google hat Unterstützungsbibliotheken, um neue Funktionen für ältere Versionen von Android verfügbar zu machen. Im Allgemeinen sind Unterstützungsbibliotheken eine Versionsnummer zugewiesen, in deren Namen, also die niedrigste Android-API-Ebene sind kompatibel mit (z. B.: Unterstützung v4 kann nur verwendet werden, für die API-Ebene 4 und höher. Weitere Informationen in diesem [Stack Overflow Diskussion](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

Zwei der für Unterstützungsbibliotheken: `Support-v4` und `Support-v13` kann nicht verwendet werden zusammen in derselben app, d. h. sie sind sich gegenseitig ausschließende. Grund hierfür ist, `Support-v13` tatsächlich enthält alle Typen und Implementierung von `Support-v4`. Wenn Sie versuchen, und verweisen beide im selben Projekt treten doppelte Typfehler.

## <a name="problems-with-referencing"></a>Probleme mit Verweisen auf

Da `Support-v4` geworden ist so beliebt, viele 3rd Party-Bibliotheken von ihm abhängig sind. Diese Unterstützung v13 stattdessen abhängig hätten, aber es ist mehr, hängen von allgemeinen _v4_ seit, durch die Option unterstützen die API-Ebenen bis 4 alle apps verwenden diese 3rd Party Bibliotheken erhalten.

Wenn Sie eine Xamarin-3rd Party-Bibliothek verweist die `Xamarin.Android.Support.v4.dll` Binden an `Support-v4`, jeder app, die diese Bibliothek verwendet, muss auch verweisen `Xamarin.Android.Support.v4.dll`. Dies ist ein Problem auf, wenn die gleiche app auch einige der Funktionen von verwenden möchte die `Xamarin.Android.Support.v13.dll` Binden an `Support-v13`. Wenn Sie beide Bindungen verweisen, treten Fehler Doppelter Typ.

## <a name="type-forwarded-v4-binding-assembly"></a>Typweiterleitung v4 der Assemblybindung

Um dieses Problem zu umgehen, haben wir eine spezielle erstellt `Xamarin.Android.Support.v4.dll` Assembly, die keine Implementierung, sondern vielmehr hat `[assembly: TypeForwardedTo (..)]` Attribute aller Weiterleiten der `Support-v4` Typen die Implementierung innerhalb der `Xamarin.Android.Support.v13.dll` Assembly.

Dies bedeutet, dass ein Entwickler kann dies verweisen _typweiterleitung_ Assembly in ihre app verwendet, die den Verweis auf erfüllen `Xamarin.Android.Support.v4.dll` durch alle 3rd Party Bibliotheken während nach wie vor `Xamarin.Android.Support.v13.dll` in der app verwendet werden.

## <a name="nuget-assistance"></a>NuGet-Unterstützung

Während ein Entwickler die richtigen Verweise erforderlichen manuell hinzufügen konnte, werden wir können NuGet verwenden, um die richtige Assembly auszuwählen (entweder der normalen _v4_ Bindung oder die typweiterleitung _v4_ Assembly) beim Das NuGet-Paket installiert ist.

Deshalb die `Xamarin.Android.Support.v4` NuGet-Paket enthält jetzt die folgende Logik:

Wenn Ihre app auf API-Ebene 13 (Gingerbread 3.2) oder höher:

*   `Xamarin.Android.Support.v13` NuGet wird automatisch als Abhängigkeit hinzugefügt werden
*   Die _typweiterleitung_ `Xamarin.Android.Support.v4.dll` erfolgt der Verweis im Projekt

Wenn Ihre app etwas kleiner als-API-Ebene 13 abzielt, erhalten Sie die normalen `Xamarin.Android.Support.v4.dll` Bindung, die in Ihrem Projekt referenziert.

## <a name="do-i-have-to-use-support-v13"></a>Habe ich Unterstützung v13 verwenden?

Wenn Ihre app die API-Ebene 13 oder höher vorgesehen ist und Sie verwenden die `Xamarin Android Support-v4` NuGet-Paket, das `Xamarin Android Support v13` NuGet-Paket ist eine erforderliche Abhängigkeit.

Wir Ihrer Meinung nach der sehr geringe Anstieg bei app-Größe (zwei JAR-Dateien unterschiedlich 17 KB sind) lohnt Informationen zur Kompatibilität und weniger Verwaltungsaufwand, der als Ergebnis.

Wenn Sie mit unnachgiebig sind `Support-v4` in eine app, die API-Ebene 13 abzielt oder höher verwenden, können Sie immer manuell herunterladen der `.nupkg`, extrahieren Sie es, und auf die Assembly verweisen.
