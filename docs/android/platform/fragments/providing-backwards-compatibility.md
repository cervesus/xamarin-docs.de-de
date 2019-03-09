---
title: Bereitstellen von Abwärtskompatibilität mit dem Paket für Android-Unterstützung
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 48ef40ce8560fd9fbb842dde70622d968591ab98
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666904"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Bereitstellen von Abwärtskompatibilität mit dem Paket für Android-Unterstützung

Die Nützlichkeit der Fragmente wäre begrenzen, ohne dass rückwärts Kompatibilität mit vor Android 3.0 (API-Ebene 11) Geräte. Eingeführt, um diese Funktion zu gewährleisten, Google der [Support-Bibliothek](https://developer.android.com/sdk/compatibility-library.html) (ursprünglich die *Android Compatibility Library* wann es veröffentlicht wurde) die Backports einige der APIs aus neueren Versionen von Android auf ältere Versionen von Android. Es ist das Android Support-Paket, das Geräte mit Android 1.6 (API-Ebene 4), Android 2.3.3 ermöglicht. (API-Ebene von 10).

> [!NOTE]
> Nur die `ListFragment` und `DialogFragment` über die Android Support-Paket verfügbar sind. Keine der anderen Fragment Unterklassen, z. B. die `PreferenceFragment,` im Android Support-Paket verwendet werden. Sie funktionieren nicht in vor Android 3.0-Anwendungen. 


## <a name="adding-the-support-package"></a>Hinzufügen des Unterstützungspakets

Das Paket für Android-Unterstützung wird an eine Xamarin.Android-Anwendung nicht automatisch hinzugefügt. Xamarin bietet die [NuGet-Paket für Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) zur Vereinfachung einer Xamarin.Android-Anwendung die Unterstützungsbibliotheken hinzugefügt. Sollen die Supportpakete in Ihre Xamarin.Android-Anwendung enthalten die [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Komponente in Ihr Xamarin.Android-Projekt, wie im folgenden Screenshot dargestellt: 

[![Screenshot des Android-Unterstützungsbibliothek v4-Paket zum Projekt hinzugefügt wird](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Nachdem diese Schritte durchgeführt wurden, wird es möglich, Fragmente in früheren Versionen von Android zu verwenden. Die Fragment-APIs funktioniert die gleiche jetzt bei diesen älteren Versionen, mit den folgenden Ausnahmen: 

-   **Ändern Sie die Android-Mindestversion** &ndash; muss die Anwendung nicht mehr Android 3.0 oder höher abzielen, wie unten dargestellt: 

    [![Screenshot des Android-Mindestversion Ziel, die unter Android-Manifest festgelegt wird](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **Erweitern Sie FragmentActivity** &ndash; der Aktivitäten, die Fragmente hosten müssen erben nun von `Android.Support.V4.App.FragmentActivity` , und nicht von `Android.App.Activity` . 

-   **Aktualisieren Sie die Namespaces** &ndash; Klassen, die von erben `Android.App.Fragment` müssen erben nun von `Android.Support.V4.App.Fragment` . Entfernen Sie die Anweisung " `using Android.App;` " am Anfang des Quellcodes Datei, und Ersetzen Sie diese " `using Android.Support.V4.App` ". 

-   **Verwenden SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` macht eine `SupportingFragmentManager` -Eigenschaft, die verwendet werden muss, um einen Verweis auf die `FragmentManager` . Zum Beispiel: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Diese Änderungen vorgenommen wird es möglich, führen Sie eine Fragment-basierten Anwendung auf Android 1.6 oder 2.x sowie Honeycomb und Ice Cream Sandwich sein. 


## <a name="related-links"></a>Verwandte Links

- [Android Support-Bibliothek v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
