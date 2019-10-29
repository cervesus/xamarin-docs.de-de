---
title: Bereitstellen der Abwärtskompatibilität mit dem Android-Unterstützungspaket
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027298"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Bereitstellen der Abwärtskompatibilität mit dem Android-Unterstützungspaket

Die Nützlichkeit von Fragmenten wäre ohne Abwärtskompatibilität mit Geräten vor Android 3,0 (API-Ebene 11) eingeschränkt. Um diese Funktion bereitzustellen, hat Google die [Unterstützungs Bibliothek](https://developer.android.com/sdk/compatibility-library.html) (ursprünglich als *Android Compatibility Library* bezeichnet, als Sie veröffentlicht wurde) eingeführt, mit der einige der APIs von neueren Versionen von Android auf ältere Versionen von Android zurückportiert werden. Dabei handelt es sich um das Android-Unterstützungspaket, das Geräten mit Android 1,6 (API-Ebene 4) Android-2.3.3 ermöglicht. (API-Ebene 10).

> [!NOTE]
> Nur die `ListFragment` und die `DialogFragment` sind über das Android-Unterstützungspaket verfügbar. Keines der anderen Fragment-Unterklassen, z. b. die `PreferenceFragment,` werden im Android-Unterstützungspaket unterstützt. Sie können in Anwendungen vor Android 3,0 nicht verwendet werden. 

## <a name="adding-the-support-package"></a>Hinzufügen des Unterstützungspakets

Das Android-Unterstützungspaket wird nicht automatisch einer xamarin. Android-Anwendung hinzugefügt. Xamarin bietet das [nuget-Paket für Android-Unterstützungs Bibliotheken V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) , um das Hinzufügen von Unterstützungs Bibliotheken zu einer xamarin. Android-Anwendung zu vereinfachen. Wenn Sie die Support Pakete in Ihre xamarin. Android-Anwendung einschließen möchten, schließen Sie die Komponente [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) in Ihr xamarin. Android-Projekt ein, wie im folgenden Screenshot veranschaulicht: 

[![Screenshot des Android-Unterstützungs Bibliothek V4-Pakets, das dem Projekt hinzugefügt wird](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Nachdem diese Schritte ausgeführt wurden, ist es möglich, Fragmente in früheren Versionen von Android zu verwenden. Die fragmentapis funktionieren in diesen früheren Versionen jetzt genauso wie die folgenden Ausnahmen: 

- **Ändern Sie die Android-Mindestversion** &ndash; die Anwendung nicht mehr auf Android 3,0 oder höher abzielen muss, wie unten dargestellt: 

    [Screenshot des mindestens festgelegten Android-Ziels unter dem Android-Manifest![](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **Erweitern von fragmentactivity** &ndash; die Aktivitäten, die Fragmente als Host Fragmente haben, müssen nun von `Android.Support.V4.App.FragmentActivity` erben, nicht von `Android.App.Activity`. 

- **Namespaces aktualisieren** &ndash; Klassen, die von `Android.App.Fragment` erben, müssen nun von `Android.Support.V4.App.Fragment` erben. Entfernen Sie die using-Anweisung "`using Android.App;`" am Anfang der Quell Code Datei, und ersetzen Sie Sie durch "`using Android.Support.V4.App`". 

- **Verwenden Sie supportfragmentmanager** &ndash; `Android.Support.V4.App.FragmentActivity` eine `SupportingFragmentManager` Eigenschaft verfügbar macht, die zum erhalten eines Verweises auf die `FragmentManager` verwendet werden muss. Beispiel: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Nachdem diese Änderungen vorgenommen wurden, ist es möglich, eine auf einem Fragment basierende Anwendung unter Android 1,6 oder 2. x sowie in Honeycomb und Ice Cream Sandwich auszuführen. 

## <a name="related-links"></a>Verwandte Links

- [Android-Unterstützungs Bibliothek V4 nuget](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
