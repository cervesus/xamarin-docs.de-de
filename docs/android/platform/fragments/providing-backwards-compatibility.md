---
title: Bereitstellen von Abwärtskompatibilität mit dem Android-Unterstützungspaket (Android Support Package)
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027298"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Bereitstellen von Abwärtskompatibilität mit dem Android-Unterstützungspaket (Android Support Package)

Ohne Abwärtskompatibilität mit Geräten, auf denen Versionen vor Android 3.0 (API-Ebene 11) installiert sind, wären Fragmenten nur begrenzt nützlich. Um diese Funktionalität bereitzustellen, hat Google die [Support Library](https://developer.android.com/sdk/compatibility-library.html) (bei der Veröffentlichung ursprünglich als *Android Compatibility Library* bezeichnet) eingeführt, die einige der APIs von neueren Android-Versionen zu älteren Android-Versionen zurückportiert. Das Android-Unterstützungspaket ermöglicht die Nutzung dieser APIs auf Geräten unter Android 1.6 (API-Ebene 4) bis Android 2.3.3. (API-Ebene 10).

> [!NOTE]
> Über das Android-Unterstützungspaket sind nur das `ListFragment` und das `DialogFragment` verfügbar. Keine der anderen Fragmentunterklassen, wie z. B. `PreferenceFragment,`, wird im Android-Unterstützungspaket unterstützt. Sie funktionieren in Anwendungen vor Android 3.0 nicht. 

## <a name="adding-the-support-package"></a>Hinzufügen des Unterstützungspakets

Das Android-Unterstützungspaket wird einer Xamarin.Android-Anwendung nicht automatisch hinzugefügt. Xamarin stellt das [Android Support Library v4 NuGet-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) bereit, um das Hinzufügen der Unterstützungsbibliotheken zu einer Xamarin.Android-Anwendung zu vereinfachen. Um die Unterstützungspakete in Ihre Xamarin.Android-Anwendung einzubinden, fügen Sie die Komponente [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) zu Ihrem Xamarin.Android-Projekt hinzu, wie im folgenden Screenshot gezeigt: 

[![Screenshot eines Android Support Library v4-Pakets, das dem Projekt hinzugefügt wird](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

Nachdem diese Schritte ausgeführt wurden, ist es möglich, Fragmente auch in früheren Versionen von Android zu verwenden. Die Fragment-APIs funktionieren in diesen früheren Versionen genauso, mit folgenden Ausnahmen: 

- **Ändern der Android-Mindestversion** &ndash; die Anwendung muss nicht mehr für Android 3.0 oder entwickelt werden, wie unten gezeigt: 

    [![Screenshot der Android-Mindestversion, die im Android-Manifest festgelegt wird](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **Erweitern von FragmentActivity** &ndash; die Aktivitäten, die Fragmente hosten, müssen jetzt von `Android.Support.V4.App.FragmentActivity` erben, nicht von `Android.App.Activity`. 

- **Aktualisieren von Namespaces** &ndash; Klassen, die von `Android.App.Fragment` erben, müssen jetzt von `Android.Support.V4.App.Fragment` erben. Entfernen Sie die using-Anweisung `using Android.App;` oben in der Quellcodedatei, und ersetzen Sie sie durch `using Android.Support.V4.App`. 

- **Verwenden von SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` macht eine `SupportingFragmentManager`-Eigenschaft verfügbar, die zum Abrufen eines Verweises auf den `FragmentManager` verwendet werden muss. Zum Beispiel: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Mit diesen Änderungen ist es möglich, eine fragmentbasierte Anwendung unter Android 1.6 oder 2.x sowie unter Honeycomb und Ice Cream Sandwich auszuführen. 

## <a name="related-links"></a>Verwandte Links

- [Android Support Library v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
