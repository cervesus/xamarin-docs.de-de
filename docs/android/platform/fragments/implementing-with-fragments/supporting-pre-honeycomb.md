---
title: Unterstützende vor Wabe Android mit Supportpakete
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 75e12821d96b98037c568fb5dac69ba53a507670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>Unterstützende vor Wabe Android mit Supportpakete

Die *Android Supportpaket* besteht aus Bibliotheken, die wieder einige der neuen APIs port &ndash; z. B. Fragmente &ndash; älteren Versionen von Android. Daher können durch Hinzufügen von Android Supportpaket können wir unsere Anwendung auf Android 2.3-Geräten ausführen wie den folgenden Bildschirmen gezeigt:

[![Exemplarische Vorgehensweise und Details Aktivität Screenshots Fragmente](supporting-pre-honeycomb-images/01-sml.png)](supporting-pre-honeycomb-images/01.png#lightbox)

## <a name="adding-the-support-package"></a>Hinzufügen des Supportpakets

Das Paket für Android-Unterstützung wird an eine Anwendung Xamarin.Android nicht automatisch hinzugefügt. Xamarin bietet die [NuGet-Paket für die Bibliothek für Android unterstützt v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) zu vereinfachen, die Unterstützungsbibliotheken einer Xamarin.Android-Anwendung hinzufügen.
Supportpakete in Ihrer Anwendung enthalten Xamarin.Android enthalten die [Bibliothek für Android unterstützt v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Komponente in Ihr Projekt Xamarin.Android, wie im folgenden Screenshot gezeigt:

[![Hinzufügen der Android-Unterstützungsbibliothek v4-Pakets](supporting-pre-honeycomb-images/02-sml.png)](supporting-pre-honeycomb-images/02.png#lightbox)

Nachdem das Paket hinzugefügt wurde, ändern Sie das Zielframework auf Android 2.2 oder höher:

[![Screenshot, der Ändern der Ziel-Framework-API-Ebene](supporting-pre-honeycomb-images/03-sml.png)](supporting-pre-honeycomb-images/03.png#lightbox)

Stellen Sie außerdem sicher, dass die Mindestversion von Android auf den gleiche API-Ebene ausgerichtet ist:

[![Screenshot der Festlegen der mindestens Android-version](supporting-pre-honeycomb-images/04-sml.png)](supporting-pre-honeycomb-images/04.png#lightbox)

### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>Ändern Sie die Verwendung des Layoutnamens FragmentActivity Ableitung

Jede Aktivität, die Fragmente verwendet erben muss `Xamarin.Support.V4.App.FragmentActivity`. Diese Klasse ist ein wichtiger Bestandteil des Supportpakets, da Fragmente von Aktivitäten, unabhängig von der Version von Android gehostet werden kann. `MainActivity` erfordert nur eine kleine Änderung – sie müssen jetzt erbt von `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>Ändern von DetailsActivity FragmentActivity Ableitung

`DetailsActivity` muss auch geändert werden, aus einer `Activity` auf eine `FragmentActivity`. Als `FragmentManager` ist nicht kompatibel mit Android-Versionen vor Wabe Android Supportpaket enthält eine Wrapperklasse `SupportFragmentManager`, Abwärtskompatibilität bereitstellt. Jede `FragmentActivity` verfügt über eine `SupportFragmentManager` -Eigenschaft, und `DetailsActivity` wird geändert, dass die `SupportFragmentManager` anstelle von der `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Nachdem wir diese Änderungen abgeschlossen haben, haben wir jetzt eine Anwendung kann auf Android 1.6 und höher ausführen, und Fragmente zum unserer GUI auf die Größe der unsere Zielgerät anpassen.


## <a name="related-links"></a>Verwandte Links

- [Android unterstützt Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
