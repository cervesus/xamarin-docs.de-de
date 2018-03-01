---
title: "Bereitstellen von Abwärtskompatibilität Kompatibilität mit dem Paket für Android-Unterstützung"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: f1567815ec342a958b48ec4801e2918f2981de3d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Bereitstellen von Abwärtskompatibilität Kompatibilität mit dem Paket für Android-Unterstützung

Die Nützlichkeit der Fragmente wäre Abwärtskompatibilität Kompatibilität mit vorab Android 3.0 (API-Ebene 11)-Geräte ohne beschränkt. Um diese Funktion zu gewährleisten, Google eingeführt der [Unterstützungsbibliothek](http://developer.android.com/sdk/compatibility-library.html) (ursprünglich aufgerufen der *Bibliothek für Android Kompatibilität* wann es freigegeben wurde) die Backports einige der APIs von neueren Versionen von Android mit älteren Versionen von Android. Es ist das Android Supportpaket, die Geräten mit Android 1.6 (API-Ebene 4) an Android 2.3.3 ermöglicht. (API-Ebene 10).

> [!NOTE]
> **Hinweis**: nur die `ListFragment` und `DialogFragment` stehen über das Paket für Android unterstützt. Keine der anderen Fragment Unterklassen, z. B. die `PreferenceFragment,` werden in das Supportpaket Android unterstützt. Sie funktionieren nicht in vorab Android 3.0-Anwendungen. 

<a name="Adding_the_Support_Package" /> 

## <a name="adding-the-support-package"></a>Hinzufügen des Supportpakets

Das Paket für Android-Unterstützung wird an eine Anwendung Xamarin.Android nicht automatisch hinzugefügt. Xamarin bietet die [NuGet-Paket für die Bibliothek für Android unterstützt v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) zu vereinfachen, die Unterstützungsbibliotheken einer Xamarin.Android-Anwendung hinzufügen. Supportpakete in Ihrer Anwendung enthalten Xamarin.Android enthalten die [Bibliothek für Android unterstützt v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Komponente in Ihr Projekt Xamarin.Android, wie im folgenden Screenshot gezeigt: 

[![Screenshot der Android-Unterstützungsbibliothek v4-Paket zum Projekt hinzugefügt wird](providing-backwards-compatibility-images/02.png)](providing-backwards-compatibility-images/02.png)

Nachdem diese Schritte durchgeführt wurden, wird es möglich, Fragmente in früheren Versionen von Android verwenden. Das Fragment-APIs funktioniert die gleiche jetzt bei diesen älteren Versionen, mit folgenden Ausnahmen: 

-   **Ändern Sie die Mindestversion von Android** &ndash; die Anwendung muss nicht mehr Android 3.0 oder höher abzielen, wie unten dargestellt: 

    [![Der Minimum Android Screenshot Zielsatz wird unter den Anwendungseigenschaften](providing-backwards-compatibility-images/03.png)](providing-backwards-compatibility-images/03.png)

-   **Erweitern Sie FragmentActivity** &ndash; der Aktivitäten, die Fragmente hosten müssen erben nun von `Android.Support.V4.App.FragmentActivity` , und nicht aus `Android.App.Activity` . 

-   **Aktualisieren von Namespaces** &ndash; Klassen, die von erben `Android.App.Fragment` müssen erben nun von `Android.Support.V4.App.Fragment` . Entfernen Sie die mit Anweisung " `using Android.App;` " am Anfang des Quellcodes-Datei und Ersetzen Sie sie durch " `using Android.Support.V4.App` ". 

-   **Verwenden Sie SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` verfügbar macht eine `SupportingFragmentManager` -Eigenschaft, die verwendet werden muss, um das Abrufen eines Verweises auf die `FragmentManager` . Zum Beispiel: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Mit diesen Änderungen vorhanden wird es möglich, führen Sie eine Fragment basierenden Anwendung für Android 1.6 oder 2.x sowie für Wabe und Eis Rustikal Sandwich sein. 


## <a name="related-links"></a>Verwandte Links

- [Android-Unterstützung Bibliothek v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
