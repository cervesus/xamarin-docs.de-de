---
title: ViewPager mit Ansichten
description: "ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer streichen Sie nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch wird erläutert, wie eine swipeable Benutzeroberfläche mit ViewPager und PagerTabStrip, verwenden von Ansichten wie die Datenseiten zu implementieren (eine nachfolgende Anleitung enthält Informationen zum Verwenden von Fragmenten für die Seiten)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 9c30cf9d76498e95aba6f9a003bc40c7d14e21de
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-views"></a>ViewPager mit Ansichten

_ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer streichen Sie nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch wird erläutert, wie eine swipeable Benutzeroberfläche mit ViewPager und PagerTabStrip, verwenden von Ansichten wie die Datenseiten zu implementieren (eine nachfolgende Anleitung enthält Informationen zum Verwenden von Fragmenten für die Seiten)._

 
## <a name="overview"></a>Übersicht

Dieses Handbuch ist eine exemplarische Vorgehensweise, die einer detaillierten Erläuterung zum Verwenden bietet `ViewPager` um ein Image-Katalog Laubwerfender und-evergreen Strukturen zu implementieren. In dieser app der Benutzer Kundenkarte links und rechts über einen Katalog"Struktur" Struktur-Bilder anzeigen. Am oberen Rand jeder Seite des Katalogs, der Name der Struktur abgelesen werden eine`PagerTabStrip`, und ein Bild der Struktur angezeigt wird, eine `ImageView`. Ein Adapter wird verwendet, für die Kommunikation der `ViewPager` an das zugrunde liegende Datenmodell. Diese app implementiert einen Adapter abgeleitet `PagerAdapter`. 

Obwohl `ViewPager`-Basis-apps werden häufig implementiert, mit `Fragment`s, stehen einige relativ einfachen Anwendungsfällen, in dem die zusätzliche Komplexität der `Fragment`s ist nicht erforderlich. Die grundlegende Image-Katalog-app, die in dieser exemplarischen Vorgehensweise erfordert z. B. nicht die Verwendung von `Fragment`s. Da der Inhalt statisch ist und der Benutzer nur Kundenkarte hin-und zwischen verschiedenen Bildern, die Implementierung werden, einfacher beibehalten kann mithilfe der standardmäßigen Android Ansichten und Layouts. 



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen Sie ein neues Android-Projekt namens **TreePager** (finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) für Weitere Informationen zum Erstellen von neuen Android-Projekte). Als Nächstes starten Sie den NuGet-Paket-Manager. (Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: einschließlich eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Suchen und installieren **Android Unterstützungsbibliothek v4**: 

[![Screenshot der Unterstützung v4 Nuget ausgewählt im NuGet-Paket-Manager](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

Dies wird auch zusätzliche Pakete Reaquired durch Installieren **Android Unterstützungsbibliothek v4**.



## <a name="add-an-example-data-source"></a>Fügen Sie eine Beispiel-Datenquelle hinzu

In diesem Beispiel wird die Struktur Katalog-Datenquelle (dargestellt durch die `TreeCatalog` Klasse) bereitstellt der `ViewPager` mit Elementinhalt. 
`TreeCatalog` enthält eine vorgefertigte Sammlung von Struktur Bilder und Struktur-Titel, die der Adapter zum Erstellen von `View`s. Die `TreeCatalog` Konstruktor erfordert keine Argumente:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Die Sammlung von Bildern in `TreeCatalog` wird so organisiert, dass jedes Bild über einen Indexer zugegriffen werden kann. Die folgende Codezeile ruft beispielsweise die Bildressourcen-ID für das dritte Bild in der Auflistung ab: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Da die Implementierungsdetails der `TreeCatalog` sind nicht mit der Funktionsweise relevanten `ViewPager`, die `TreeCatalog` Code ist hier nicht aufgeführt. Quellcode und `TreeCatalog` finden Sie unter [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Laden Sie diese Quelldatei (oder kopieren Sie den Code in ein neues **TreeCatalog.cs** Datei) und dem Projekt hinzugefügt. Darüber hinaus herunterladen und Entzippen der [Bilddateien](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) in Ihrer **Ressourcen und Ausgaben möglich** Ordner und in das Projekt aufzunehmen. 



## <a name="create-a-viewpager-layout"></a>Erstellen Sie ein Layout ViewPager

Open **Resources/layout/Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 


## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

Ersetzen Sie die `OnCreate`-Methode durch folgenden Code:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

Dieser Code führt Folgendes aus:

1.  Legt die Ansicht aus der **Main.axml** Layout-Ressource.

2.  Ruft einen Verweis auf die `ViewPager` aus dem Layout.

3.  Instanziiert eine neue `TreeCatalog` als Datenquelle.

Wenn Sie erstellen und diesen Code ausführen, sehen Sie eine Anzeige, die den folgenden Screenshot ähnelt: 

[![Screenshot der app, die eine leere ViewPager anzeigen](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

An diesem Punkt der `ViewPager` ist leer, da es einen Adapter fehlt für den Zugriff auf den Inhalt in **TreeCatalog**. Im nächsten Abschnitt eine **PagerAdapter** wird erstellt, um eine Verbindung herstellen die `ViewPager` auf die **TreeCatalog**. 


## <a name="create-the-adapter"></a>Erstellen des Adapters

`ViewPager` verwendet ein Adapter-Controller-Objekt, das zwischen der `ViewPager` und der Datenquelle (siehe Abbildung in [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter)). Zugriff auf diese Daten `ViewPager` erfordert, dass Sie einen benutzerdefinierten Adapter abgeleitet bereitstellen `PagerAdapter`. Dieser Adapter füllt jede `ViewPager` Seite mit dem Inhalt der Datenquelle. Da diese Datenquelle anwendungsspezifischen ist, ist der benutzerdefinierte Adapter den Code, der zum Zugreifen auf die Daten versteht. Als der Benutzer Kundenkarte durch Datenseiten der `ViewPager`, der Adapter extrahiert Informationen aus der Datenquelle und lädt sie in den Seiten für die `ViewPager` angezeigt. 

Beim Implementieren einer `PagerAdapter`, müssen Sie die folgenden überschreiben:

-   **InstantiateItem** &ndash; erstellt die Seite (`View`) für eine angegebene Position und fügt es der `ViewPager`der Sammlung von Ansichten. 

-   **DestroyItem** &ndash; entfernt eine Seite aus einer angegebenen Position.

-   **Anzahl** &ndash; schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Ansichten (Seiten) zurückgibt. 

-   **IsViewFromObject** &ndash; bestimmt, ob eine Seite mit einem bestimmten Schlüssel-Objekt zugeordnet ist. (Dieses Objekt wird erstellt, indem die `InstantiateItem` Methode.) In diesem Beispiel wird das Schlüsselobjekt der `TreeCatalog` Datenobjekt.

Fügen Sie eine neue Datei namens **TreePagerAdapter.cs** und Ersetzen Sie den Inhalt durch folgenden Code: 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

Dieser Code versieht die grundlegende `PagerAdapter` Implementierung. In den folgenden Abschnitten wird jede dieser Methoden mit Arbeitscode ersetzt. 



### <a name="implement-the-constructor"></a>Implementieren des Konstruktors

Wenn die app instanziiert die `TreePagerAdapter`, stellt einen Kontext (die `MainActivity`) und eine instanziierte `TreeCatalog`. Fügen Sie den folgenden Membervariablen und den Konstruktor an den Anfang der `TreePagerAdapter` -Klasse im **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Der Zweck dieses Konstruktors besteht darin, den Kontext zu speichern und `TreeCatalog` Instanz, die `TreePagerAdapter` verwenden. 



### <a name="implement-count"></a>Anzahl der implementieren

Die `Count` Implementierung ist relativ einfach: Es gibt die Anzahl der Strukturen im Struktur-Katalog. Ersetzen Sie den Code `Count` durch folgenden Code:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

Die `NumTrees` Eigenschaft `TreeCatalog` gibt die Anzahl von Strukturen (Anzahl der Seiten) in einem DataSet zurück.



### <a name="implement-instantiateitem"></a>Implementieren von InstantiateItem

Die `InstantiateItem` Methode erstellt, die Seite für eine angegebene Position. Außerdem müssen sie die neu erstellte Ansicht hinzufügen der `ViewPager`des Sammlung anzeigen. Um dies zu, ermöglichen die `ViewPager` selbst als der Container-Parameter übergibt. 

Ersetzen Sie die `InstantiateItem`-Methode durch folgenden Code:

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

Dieser Code führt Folgendes aus:

1.  Instanziiert eine neue `ImageView` für die Struktur-Darstellung der angegebenen Position. Der app `MainActivity` ist der Kontext, der übergeben wird die `ImageView` Konstruktor.

2.  Legt die `ImageView` Ressource der `TreeCatalog` Bild-Ressourcen-ID an der angegebenen Position.

3.  Wandelt die übergebenen Container `View` zu einem `ViewPager` Verweis.
    Beachten Sie, die Sie verwenden, müssen `JavaCast<ViewPager>()` ordnungsgemäß diese Umwandlung ausführen (Dies ist erforderlich, damit Android eine Laufzeit geprüfte typkonvertierung ausführt).

4.  Fügt der instanziierten `ImageView` auf die `ViewPager` und gibt die `ImageView` an den Aufrufer.

Wenn die `ViewPager` zeigt das Bild am `position`, es zeigt dies `ImageView`. Zu Beginn `InstantiateItem` heißt zweimal aus, um den ersten beiden Seiten mit Ansichten zu füllen. Wenn der Benutzer einen Bildlauf durchführt, wird er erneut aufgerufen, um Ansichten direkt hinter und vor der derzeit angezeigte Element zu gewährleisten. 



### <a name="implement-destroyitem"></a>Implementieren von DestroyItem

Die `DestroyItem` Methode entfernt eine Seite aus der angegebenen Position. In apps, die in dem die Ansicht in einer bestimmten Position ändern kann, `ViewPager` benötigen eine Möglichkeit, eine veraltete Ansicht an dieser Position vor dem Ersetzen durch eine neue Sicht zu entfernen. In der `TreeCatalog` beispielsweise die Sicht an jede Position wird nicht geändert, damit durch eine Sicht entfernt `DestroyItem` einfach werden neu hinzugefügten Wenn `InstantiateItem` für diese Position aufgerufen wird. (Für eine bessere Effizienz konnte eine implementieren ein Anwendungspool wiederverwendet `View`s, die an der gleichen Position neu angezeigt werden.) 

Ersetzen Sie die `DestroyItem`-Methode durch folgenden Code: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Dieser Code führt Folgendes aus:

1.  Wandelt die übergebenen Container `View` in einem `ViewPager` Verweis.

2.  Wandelt das übergebene Java-Objekt (`view`) in einem C#- `View` (`view as View`);

3.  Entfernt die Ansicht aus der `ViewPager`. 



### <a name="implement-isviewfromobject"></a>Implementieren von IsViewFromObject

Als Benutzer Folien linken und rechten Seiten von Inhalten die durch `ViewPager` Aufrufe `IsViewFromObject` zu überprüfen, ob das untergeordnete Element `View` an der angegebenen Position des Adapters-Objekt für diese dieselbe Position zugeordnet ist (daher heißt die Hostnetzwerkadapter-Objekt ein *Objektschlüssel*). Die Zuordnung für relativ einfache apps ist eine Identität &ndash; des Adapters-Objektschlüssel auf dieser Instanz ist, wird die Sicht, die zuvor an zurückgegeben wurde die `ViewPager` über `InstantiateItem`. Jedoch für andere apps, die Objektschlüssel werden möglicherweise einige andere adapterspezifische Klasseninstanz, die zugeordnet ist (jedoch nicht identisch) das untergeordnete Element anzuzeigen, die `ViewPager` an dieser Position angezeigt. Nur Adapter offensichtlich, und zwar unabhängig davon, ob die übergebene Ansicht und der Objektschlüssel verknüpft sind. 

`IsViewFromObject` muss implementiert werden, für die `PagerAdapter` ordnungsgemäß funktioniert. Wenn `IsViewFromObject` gibt `false` für eine angegebene Position `ViewPager` wird die Ansicht nicht an dieser Position angezeigt. In der `TreePager` app, die das Objekt vom zurückgegebenen Schlüssels `InstantiateItem` *ist* der Seite `View` einer Struktur, damit der Code nur hat zu suchende Identität (d. h., der Objektschlüssel und die Ansicht sind identisch). Ersetzen Sie den Code `IsViewFromObject` durch folgenden Code: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zu den ViewPager

Nun, dass die `TreePagerAdapter` wird implementiert, es ist Zeit, die sie zum Hinzufügen der `ViewPager`. In **MainActivity.cs**, fügen Sie die folgende Codezeile am Ende der `OnCreate` Methode:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Dieser Code instanziiert die `TreePagerAdapter`, und übergeben Sie die `MainActivity` als Kontext (`this`). Die instanziierte `TreeCatalog` wird in der zweiten Konstruktorargument übergeben. Die `ViewPager`des `Adapter` festgelegt wird, instanziierten `TreePagerAdapter` Objekt, und dieses Plug-In für die `TreePagerAdapter` in der `ViewPager`. 

Die basisimplementierung ist jetzt vollständig &ndash; erstellen und Ausführen der app. Daraufhin sollte das erste Bild des Katalogs, Struktur, die auf dem Bildschirm angezeigt werden, wie auf der linken Seite im nächsten Screenshot dargestellt. Streichen Sie nach links, um weitere Strukturansichten, finden Sie unter dann streichen Sie nach rechts wieder über den Struktur-Katalog zu verschieben: 

[![Screenshots der TreePager app Streifen über Struktur-images](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Hinzufügen eines Indikators Pager

Dieser minimale `ViewPager` Implementierung zeigt die Bilder des Katalogs, Struktur, verfügt aber keine Hinweis aufgeführt, in denen der Benutzer, in dem Katalog gehört. Der nächste Schritt besteht, zum Hinzufügen einer `PagerTabStrip`. Die `PagerTabStrip` informiert den Benutzer als auf der Seite angezeigt wird und bereitstellt Navigationskontext durch einen Hinweis auf den vorherigen und nächsten Seiten anzeigen. `PagerTabStrip` Dient als Indikator für die aktuelle Seite des zu verwendenden eine `ViewPager`; es führt einen Bildlauf durch, und aktualisiert, wenn sich der Benutzer Kundenkarte durch die einzelnen Seiten. 

Open **Resources/layout/Main.axml** und Hinzufügen einer `PagerTabStrip` des Layouts:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` und `PagerTabStrip` zusammen funktionieren. Beim Deklarieren einer `PagerTabStrip` innerhalb einer `ViewPager` Layout der `ViewPager` findet automatisch die `PagerTabStrip` und verbinden Sie ihn an den Adapter. Wenn Sie erstellen und der app ausführen, sehen Sie die leere `PagerTabStrip` am oberen Rand jeder Bildschirm angezeigt: 

[![Nahaufnahme-Screenshot, der eine leere PagerTabStrip](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>Anzeigen eines Titels

Zum Hinzufügen eines Titels zu jeder Seitenregisterkarte implementieren die `GetPageTitleFormatted` Methode in der `PagerAdapter`-abgeleitete Klasse. `ViewPager` Aufrufe `GetPageTitleFormatted` (falls implementiert zurzeit) Titelzeichenfolge abrufen, die der Seite an der angegebenen Position beschrieben. Fügen Sie die folgende Methode, die `TreePagerAdapter` -Klasse im **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Dieser Code Ruft die Beschriftungszeichenfolge Struktur aus der angegebenen Seite (Position) im Katalog Struktur ab, konvertiert sie in einer Java `String`, und gibt ihn zurück die `ViewPager`. Wenn Sie die app mit dieser neuen Methode ausführen, zeigt jeder Seite die Struktur der Beschriftung in der `PagerTabStrip`. Der Strukturname am oberen Rand des Bildschirms ohne Unterstrich sollte angezeigt werden: 

[![Screenshots von Seiten mit Text aufgefüllt PagerTabStrip Registerkarten](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Sie können Streifen hin-und herwechseln jedes PRESSEPHOTOGRAPHIEN Struktur Bild im Katalog angezeigt. 



### <a name="pagertitlestrip-variation"></a>PagerTitleStrip-Variante

`PagerTitleStrip` vergleichbar mit `PagerTabStrip` mit dem Unterschied, dass `PagerTabStrip` Fügt eine Unterstreichung für den derzeit ausgewählten Registerkarte. Ersetzen Sie `PagerTabStrip` mit `PagerTitleStrip` in den oben genannten Layout und Ausführen der app erneut aus, um die Anzeige prüfen mit `PagerTitleStrip`: 

[![PagerTitleStrip mit unterstreichungen aus Text entfernt](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Beachten Sie, dass die Unterstreichung entfernt wird, bei der Konvertierung in `PagerTitleStrip`. 


 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise bereitgestellt ein ausführliches Beispiel zum Erstellen einer grundlegenden `ViewPager`-basierten app ohne `Fragment`s. Sie eine Beispiel-Datenquelle mit Images und Beschriftung Zeichenfolgen angezeigt ein `ViewPager` Layout für die Bilder anzuzeigen und eine `PagerAdapter` Unterklasse, die eine Verbindung herstellt der `ViewPager` mit der Datenquelle. Um den Benutzer, die durch das DataSet Navigieren zu erleichtern, wurden Anweisungen enthalten, die erläutern das Hinzufügen einer `PagerTabStrip` oder `PagerTitleStrip` die Image-Beschriftung am oberen Rand jeder Seite angezeigt. 


## <a name="related-links"></a>Verwandte Links

- [TreePager (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
