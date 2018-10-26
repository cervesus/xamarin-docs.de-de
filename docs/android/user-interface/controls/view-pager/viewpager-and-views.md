---
title: ViewPager mit Ansichten
description: ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer Wischen nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch wird erläutert, wie eine "wischfähige" Benutzeroberfläche mit ViewPager und PagerTabStrip, verwenden von Ansichten als die Datenseiten zu implementieren (nachfolgenden Leitfaden wird beschrieben, wie Fragmente für die Seiten zu verwenden).
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a8b7fa53d3384821d028e4a88ba22071a17e5bd9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113923"
---
# <a name="viewpager-with-views"></a>ViewPager mit Ansichten

_ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer Wischen nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch wird erläutert, wie eine "wischfähige" Benutzeroberfläche mit ViewPager und PagerTabStrip, verwenden von Ansichten als die Datenseiten zu implementieren (nachfolgenden Leitfaden wird beschrieben, wie Fragmente für die Seiten zu verwenden)._

 
## <a name="overview"></a>Übersicht

Dieses Handbuch enthält eine exemplarische Vorgehensweise, die Schritt, wie Sie mit bietet `ViewPager` auf einen imagekatalog Laubwerfender Erstellung langlebiger und dauerhafter Strukturen zu implementieren. In dieser app der Benutzer einer wischbewegung, linken und rechten über eine "Struktur-Catalog" Struktur-Bilder anzeigen. Am oberen Rand jeder Seite des Katalogs, der Name der Struktur finden Sie eine`PagerTabStrip`, und ein Bild der Struktur angezeigt wird, eine `ImageView`. Ein Adapter wird verwendet, zur Schnittstelle die `ViewPager` der zugrunde liegenden Datenmodell. Diese app implementiert einen Adapter, die von abgeleiteten `PagerAdapter`. 

Obwohl `ViewPager`-Basis apps werden häufig implementiert, mit `Fragment`s, es gibt einige Anwendungsfälle für relativ einfache, in dem die zusätzliche Komplexität des `Fragment`s ist nicht erforderlich. Die Basis-Image-Katalog-app, die in dieser exemplarischen Vorgehensweise erfordert z. B. nicht die Verwendung von `Fragment`s. Da der Inhalt statisch ist, und der Benutzer nur Kundenkarte zwischen verschiedenen Bildern, die Implementierung kann einfacher verwaltet werden mithilfe von standard-Android-Ansichten und Layouts. 



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen Sie ein neues Android-Projekt namens **TreePager** (finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md) für Weitere Informationen zum Erstellen von neuen Android-Projekte). Als Nächstes starten Sie den NuGet-Paket-Manager. (Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: Einschließen eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Suchen und Installieren von **Android Support-Bibliothek v4**: 

[![Screenshot der Unterstützung v4 Nuget in der NuGet-Paket-Manager ausgewählt](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

Dadurch werden auch alle zusätzlichen Pakete Reaquired durch installiert **Android Support-Bibliothek v4**.



## <a name="add-an-example-data-source"></a>Hinzufügen einer Beispiel-Datenquelle

In diesem Beispiel ist die Struktur Catalog-Datenquelle (dargestellt durch die `TreeCatalog` Klasse) stellt der `ViewPager` mit Inhalt des Elements. 
`TreeCatalog` enthält eine vorgefertigte Sammlung von Tree-Bilder und Titel Struktur, die der Adapter zum Erstellen von `View`s. Die `TreeCatalog` -Konstruktor erfordert keine Argumente:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Die Auflistung der Bilder in `TreeCatalog` ist so organisiert, dass jedes Bild von einem Indexer zugegriffen werden kann. Die folgende Codezeile werden beispielsweise die Image-Ressourcen-ID für das dritte Bild in der Auflistung abgerufen: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Da die Implementierungsdetails der `TreeCatalog` sind nicht relevant für ein Verständnis `ViewPager`, `TreeCatalog` Code hier nicht aufgeführt ist. Der Quellcode `TreeCatalog` finden Sie unter [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs). Laden Sie diese Quelldatei (oder kopieren und fügen Sie den Code in eine neue **TreeCatalog.cs** Datei) und fügen sie Ihrem Projekt hinzu. Herunterladen, und Entzippen Sie die [Bilddateien](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) in Ihre **Ressourcen/drawable** Ordner und in das Projekt zu integrieren. 



## <a name="create-a-viewpager-layout"></a>Erstellen eines Layouts ViewPager

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

1.  Legt die Ansicht aus der **Main.axml** layoutressource.

2.  Ruft einen Verweis auf die `ViewPager` aus dem Layout.

3.  Instanziiert ein neues `TreeCatalog` als Datenquelle.

Wenn Sie erstellen und diesen Code ausführen, sehen Sie eine Anzeige, die im folgenden Screenshot ähnelt: 

[![Screenshot der app, die eine leere ViewPager anzeigen](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

An diesem Punkt die `ViewPager` ist leer, da es einen Adapter verfügt, ist für den Zugriff auf den Inhalt in **TreeCatalog**. Im nächsten Abschnitt eine **PagerAdapter** wird erstellt, um eine Verbindung herstellen die `ViewPager` auf die **TreeCatalog**. 


## <a name="create-the-adapter"></a>Erstellen des Adapters

`ViewPager` verwendet ein Adapter-Controller-Objekt, das zwischen dem `ViewPager` und der Datenquelle (finden Sie unter der Abbildung in [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter)). Um diese Daten zugreifen `ViewPager` erfordert, dass Sie einen benutzerdefinierten Adapter, die von abgeleiteten bereitstellen `PagerAdapter`. Dieser Adapter füllt jede `ViewPager` -Seite mit Inhalten aus der Datenquelle. Da diese Datenquelle app-spezifisch ist, ist der benutzerdefinierte Adapter den Code, der auf die Daten zugreifen kann. Als die Kundenkarte Benutzer durch die Datenseiten der `ViewPager`, der Adapter extrahiert Informationen aus der Datenquelle und lädt sie in den Seiten für die `ViewPager` angezeigt. 

Bei der Implementierung einer `PagerAdapter`, müssen Sie die folgenden überschreiben:

-   **InstantiateItem** &ndash; erstellt die Seite (`View`) für eine bestimmte Position und fügt es der `ViewPager`die Auflistung von Sichten. 

-   **DestroyItem** &ndash; entfernt eine Seite aus einer angegebenen Position.

-   **Anzahl** &ndash; schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Ansichten (Seiten) zurückgibt. 

-   **IsViewFromObject** &ndash; bestimmt, ob eine Seite ein bestimmtes Objekt zugeordnet ist. (Dieses Objekt wird erstellt, indem die `InstantiateItem` Methode.) In diesem Beispiel wird das Schlüsselobjekt der `TreeCatalog` Datenobjekt.

Fügen Sie eine neue Datei namens **TreePagerAdapter.cs** und Ersetzen Sie den Inhalt durch den folgenden Code: 

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

Dieser Code versieht die grundlegende `PagerAdapter` Implementierung. In den folgenden Abschnitten wird jede dieser Methoden mit den funktionierenden Code ersetzt. 



### <a name="implement-the-constructor"></a>Implementieren des Konstruktors

Wenn die app instanziiert die `TreePagerAdapter`, gibt er einen Kontext an (die `MainActivity`) und instanziierten `TreeCatalog`. Fügen Sie den folgenden Membervariablen und den Konstruktor am Anfang der `TreePagerAdapter` -Klasse im **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Dieser Konstruktor zum Speichern des Kontexts dient und `TreeCatalog` Instanz, die `TreePagerAdapter` verwenden. 



### <a name="implement-count"></a>Anzahl der implementieren

Die `Count` Implementierung ist relativ einfach: Es gibt die Anzahl der Strukturen im Katalog Struktur zurück. Ersetzen Sie den Code `Count` durch folgenden Code:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

Die `NumTrees` Eigenschaft `TreeCatalog` gibt die Anzahl der Strukturen (Anzahl der Seiten) in den Daten zurück.



### <a name="implement-instantiateitem"></a>Implementieren von InstantiateItem

Die `InstantiateItem` Methode erstellt die Seite für eine bestimmte Position. Sie müssen auch die neu erstellte Ansicht zum Hinzufügen der `ViewPager`der Sammlung anzeigen. Um dies zu ermöglichen, wird die `ViewPager` selbst als der Container-Parameter übergibt. 

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

1.  Instanziiert ein neues `ImageView` , um die Struktur-Bild an der angegebenen Position anzuzeigen. Der app `MainActivity` ist der Kontext, der übergeben der `ImageView` Konstruktor.

2.  Legt die `ImageView` Ressource, die die `TreeCatalog` image-Ressourcen-ID an der angegebenen Position.

3.  Wandelt die übergebenen Container `View` zu einem `ViewPager` Verweis.
    Beachten Sie, die Sie verwenden, müssen `JavaCast<ViewPager>()` ordnungsgemäß diese Umwandlung ausführen (Dies ist erforderlich, damit Android eine Laufzeit geprüfte typkonvertierung ausführt).

4.  Fügt der instanziierten `ImageView` auf die `ViewPager` und gibt die `ImageView` an den Aufrufer.

Wenn die `ViewPager` zeigt das Bild am `position`, zeigt dies `ImageView`. Zunächst `InstantiateItem` heißt zweimal aus, um die ersten beiden Seiten mit Ansichten zu füllen. Wenn der Benutzer einen Bildlauf durchführt, wird es erneut aufgerufen, um Ansichten direkt hinter und vor das aktuell angezeigte Element zu gewährleisten. 



### <a name="implement-destroyitem"></a>Implementieren von DestroyItem

Die `DestroyItem` Methode entfernt eine Seite aus der angegebenen Position. In apps, die in dem die Ansicht in einer bestimmten Position ändern kann, `ViewPager` müssen irgendwie eine veraltete Ansicht an dieser Position zu entfernen, bevor sie mit einer neuen Ansicht ersetzt. In der `TreeCatalog` Beispiel, dass die Ansicht in jede Position wird nicht geändert, damit eine Ansicht von entfernt `DestroyItem` , einfach werden erneut hinzugefügt Wenn `InstantiateItem` für diese Position aufgerufen wird. (Für bessere Effizienz, konnte eine implementieren einen Pool wiederzuverwenden, `View`s, die an der gleichen Position neu angezeigt werden.) 

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

2.  Wandelt das übergebene Objekt mit Java (`view`) in einem C# `View` (`view as View`);

3.  Entfernt die Ansicht aus der `ViewPager`. 



### <a name="implement-isviewfromobject"></a>Implementieren von IsViewFromObject

Als Benutzer Folien links und rechts durch Seiteninhalte `ViewPager` Aufrufe `IsViewFromObject` zu überprüfen, ob das untergeordnete Element `View` an der angegebenen Position des Adapters-Objekt für diese dieselbe Position zugeordnet ist (daher heißt das-Objekt des Adapters ein *Objektschlüssel*). Für relativ einfache apps, die Zuordnung ist einer der Identität &ndash; des Adapters-Objektschlüssel auf dieser Instanz ist, wird die Ansicht, die zuvor an zurückgegeben wurde die `ViewPager` über `InstantiateItem`. Jedoch für andere apps, die Schlüssel des Objekts möglicherweise einige andere adapterspezifische Klasseninstanz, die zugeordnet ist (aber nicht identisch) das untergeordnete Element anzuzeigen, die `ViewPager` an dieser Position angezeigt. Nur mit der Adapter weiß, und zwar unabhängig davon, ob die übergebene Ansicht und der Objektschlüssel verknüpft sind. 

`IsViewFromObject` implementiert werden muss, für die `PagerAdapter` ordnungsgemäß funktioniert. Wenn `IsViewFromObject` gibt `false` für eine bestimmte Position, `ViewPager` an dieser Position nicht die Ansicht angezeigt. In der `TreePager` -app, das das vom zurückgegebenen Schlüssels `InstantiateItem` *ist* Seite `View` einer Struktur, damit der Code nur für die Identität zu überprüfen muss (d. h., der Objektschlüssel und der Ansicht sind identisch). Ersetzen Sie den Code `IsViewFromObject` durch folgenden Code: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zu dem ViewPager

Nachdem der `TreePagerAdapter` wird implementiert, ist es Zeit, die sie zum Hinzufügen der `ViewPager`. In **"mainactivity.cs"**, fügen Sie die folgende Codezeile am Ende der `OnCreate` Methode:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Dieser Code instanziiert die `TreePagerAdapter`, und übergeben Sie die `MainActivity` als Kontext (`this`). Das instanziierte `TreeCatalog` zweites Argument des Konstruktors übergeben wird. Die `ViewPager`des `Adapter` festgelegt wird, die instanziiert `TreePagerAdapter` Objekt; dieses Plug-In für die `TreePagerAdapter` in die `ViewPager`. 

Die Core-Implementierung ist jetzt vollständig &ndash; erstellen und Ausführen der app. Daraufhin sollte das erste Bild des Katalogs, Struktur, die auf dem Bildschirm angezeigt wird, wie auf der linken Seite im nächsten Screenshot gezeigt. Streichen Sie nach links, um weitere Strukturansichten, finden Sie unter Klicken Sie dann Wischen nach rechts zurück über den Katalog Struktur verschieben: 

[![Screenshots der TreePager app Wischen über Struktur-images](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Einen Pager-Indikator hinzufügen

Diesem minimale `ViewPager` Implementierung zeigt die Bilder des Katalogs, Struktur, bietet aber keine visuell dargestellt, in denen der Benutzer innerhalb des Katalogs ist. Der nächste Schritt besteht, zum Hinzufügen einer `PagerTabStrip`. Die `PagerTabStrip` informiert den Benutzer, welche Seite wird angezeigt, und bietet von Navigationskontext durch einen Hinweis in der vorherigen und nächsten Seiten anzeigen. `PagerTabStrip` Dient als Indikator für die aktuelle Seite des zu verwendenden eine `ViewPager`; es führt einen Bildlauf durch, und aktualisiert, wenn sich der Benutzer Kundenkarte durch die einzelnen Seiten. 

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

`ViewPager` und `PagerTabStrip` zusammen funktionieren. Wenn Sie deklarieren eine `PagerTabStrip` innerhalb einer `ViewPager` Layout der `ViewPager` findet automatisch die `PagerTabStrip` und verbinden Sie es an den Adapter. Wenn Sie erstellen und der app ausführen, sollte die leere `PagerTabStrip` am oberen Rand jedes Bildschirms angezeigt: 

[![Nahaufnahme-Screenshot, der eine leere PagerTabStrip](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>Anzeigen eines Titels

Zum Hinzufügen eines Titels zu jeder Seitenregisterkarte implementieren die `GetPageTitleFormatted` -Methode in der die `PagerAdapter`-abgeleitete Klasse. `ViewPager` Aufrufe `GetPageTitleFormatted` (wenn implementiert) auf die Title-Zeichenfolge abzurufen, die beschreibt, die Seite an der angegebenen Position. Fügen Sie die folgende Methode der `TreePagerAdapter` -Klasse im **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Dieser Code Ruft die Beschriftungszeichenfolge Struktur aus der angegebenen Seite (Position) im Katalog Struktur ab, konvertiert es in einer Java `String`, und gibt zurück, damit die `ViewPager`. Beim Ausführen der app mit dieser neuen Methode zeigt jede Seite der Beschriftung "Struktur" in der `PagerTabStrip`. Der Strukturname am oberen Rand des Bildschirms ohne Unterstreichung wird angezeigt: 

[![Screenshots der Seiten, die mit Text ausgefüllte PagerTabStrip Registerkarten](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Sie können streichen Sie nach hin und her, um jedes PRESSEPHOTOGRAPHIEN Struktur-Image im Katalog anzeigen. 



### <a name="pagertitlestrip-variation"></a>PagerTitleStrip-Variante

`PagerTitleStrip` ist sehr ähnlich `PagerTabStrip` mit dem Unterschied, dass `PagerTabStrip` Fügt eine Unterstreichung für die derzeit ausgewählten Registerkarte. Sie können ersetzen `PagerTabStrip` mit `PagerTitleStrip` in den oben aufgeführten Layout und führen Sie die app erneut aus, um die Anzeige prüfen mit `PagerTitleStrip`: 

[![PagerTitleStrip mit unterstreichungen aus Text entfernt](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Beachten Sie, dass die Unterstreichung entfernt wird, wenn Sie zum Konvertieren `PagerTitleStrip`. 


 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise bereitgestellt, ein ausführliches Beispiel zum Erstellen eines Grundlegendes `ViewPager`-basierten app ohne `Fragment`s. Sie eine Beispiel-Datenquelle mit Bildern und Beschriftung Zeichenfolgen angezeigt ein `ViewPager` Layout der Bilder anzuzeigen, und ein `PagerAdapter` Unterklasse, die eine Verbindung herstellt der `ViewPager` mit der Datenquelle. Damit den Benutzer, die durch das DataSet navigieren können, sind Anweisungen enthalten, die erläutern, dass das Hinzufügen einer `PagerTabStrip` oder `PagerTitleStrip` den Titel der Abbildung am oberen Rand jeder Seite angezeigt. 


## <a name="related-links"></a>Verwandte Links

- [TreePager (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
