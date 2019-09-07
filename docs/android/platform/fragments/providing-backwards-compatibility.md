---
title: Bereitstellen der Abwärtskompatibilität mit dem Android-Unterstützungspaket
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 838f8bdcf3bd82a31bf0d033eee628bd19ad1c30
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757551"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Bereitstellen der Abwärtskompatibilität mit dem Android-Unterstützungspaket

Die Nützlichkeit von Fragmenten wäre ohne Abwärtskompatibilität mit Geräten vor Android 3,0 (API-Ebene 11) eingeschränkt. Um diese Funktion bereitzustellen, hat Google die [Unterstützungs Bibliothek](https://developer.android.com/sdk/compatibility-library.html) (ursprünglich als *Android Compatibility Library* bezeichnet, als Sie veröffentlicht wurde) eingeführt, mit der einige der APIs von neueren Versionen von Android auf ältere Versionen von Android zurückportiert werden. Dabei handelt es sich um das Android-Unterstützungspaket, das Geräten mit Android 1,6 (API-Ebene 4) Android-2.3.3 ermöglicht. (API-Ebene 10).

> [!NOTE]
> `ListFragment` Nur`DialogFragment` und sind über das Android-Unterstützungspaket verfügbar. Keines der anderen Fragment-Unterklassen, z `PreferenceFragment,` . b., wird im Android-Unterstützungspaket unterstützt. Sie können in Anwendungen vor Android 3,0 nicht verwendet werden. 

## <a name="adding-the-support-package"></a>Hinzufügen des Unterstützungspakets

Das Android-Unterstützungspaket wird nicht automatisch einer xamarin. Android-Anwendung hinzugefügt. Xamarin bietet das [nuget-Paket für Android-Unterstützungs Bibliotheken V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) , um das Hinzufügen von Unterstützungs Bibliotheken zu einer xamarin. Android-Anwendung zu vereinfachen. Wenn Sie die Support Pakete in Ihre xamarin. Android-Anwendung einschließen möchten, schließen Sie die Komponente [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) in Ihr xamarin. Android-Projekt ein, wie im folgenden Screenshot veranschaulicht: 

[![Screenshot des Android-Unterstützungs Bibliotheken V4-Pakets, das dem Projekt hinzugefügt wird](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Nachdem diese Schritte ausgeführt wurden, ist es möglich, Fragmente in früheren Versionen von Android zu verwenden. Die fragmentapis funktionieren in diesen früheren Versionen jetzt genauso wie die folgenden Ausnahmen: 

- **Ändern der Android-Mindestversion** &ndash; Die Anwendung muss Android 3,0 oder höher nicht mehr als Ziel haben, wie unten dargestellt: 

    [![Screenshot des minimalen festgelegten Android-Ziels unter Android-Manifest](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- " **Fragmentactivity" erweitern** Die Aktivitäten, die als Host Fragmente dienen, müssen `Android.Support.V4.App.FragmentActivity` jetzt von und nicht `Android.App.Activity` von erben. &ndash; 

- **Namespaces aktualisieren** Klassen, die von `Android.App.Fragment` erben, müssen jetzt `Android.Support.V4.App.Fragment` von erben. &ndash; Entfernen Sie die using- `using Android.App;` Anweisung "" am Anfang der Quell Code Datei, und ersetzen Sie Sie `using Android.Support.V4.App` durch "". 

- **Verwenden von supportfragmentmanager** macht eine -Eigenschaft`FragmentManager` verfügbar, die verwendet werden muss, um einen Verweis auf das zu erhalten. `SupportingFragmentManager` &ndash; `Android.Support.V4.App.FragmentActivity` Beispiel: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Nachdem diese Änderungen vorgenommen wurden, ist es möglich, eine auf einem Fragment basierende Anwendung unter Android 1,6 oder 2. x sowie in Honeycomb und Ice Cream Sandwich auszuführen. 

## <a name="related-links"></a>Verwandte Links

- [Android-Unterstützungs Bibliothek V4 nuget](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
