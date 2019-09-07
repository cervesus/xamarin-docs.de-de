---
title: ViewPager mit Ansichten
description: Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie Sie eine Benutzeroberfläche mit "viewpager" und "PagerTabStrip" implementieren können, indem Sie Ansichten als Datenseiten verwenden (im nachfolgenden Handbuch wird erläutert, wie Fragmente für die Seiten verwendet werden).
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 8e9788b31bc397a45e4ac98a01bc788096bbd523
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762368"
---
# <a name="viewpager-with-views"></a>ViewPager mit Ansichten

_Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie Sie eine Benutzeroberfläche mit "viewpager" und "PagerTabStrip" implementieren können, indem Sie Ansichten als Datenseiten verwenden (im nachfolgenden Handbuch wird erläutert, wie Fragmente für die Seiten verwendet werden)._

## <a name="overview"></a>Übersicht

Diese Anleitung enthält eine exemplarische Vorgehensweise, `ViewPager` in der eine schrittweise Anleitung zum Implementieren eines Bild Katalogs von Laub-und Evergreen-Strukturen erläutert wird. In dieser APP wird der Benutzer nach links und rechts durch einen "Struktur Katalog", um Baum Bilder anzuzeigen. Am oberen Rand jeder Seite des Katalogs wird der Name der Struktur in einer`PagerTabStrip`aufgeführt, und in einem `ImageView`wird ein Bild der Struktur angezeigt. Ein Adapter wird verwendet, um die `ViewPager` mit dem zugrunde liegenden Datenmodell zu verbinden. Diese APP implementiert einen von `PagerAdapter`abgeleiteten Adapter. 

Obwohl `ViewPager`-basierte apps oft mit `Fragment`s implementiert werden, gibt es einige relativ einfache Anwendungsfälle, in denen die zusätzliche `Fragment`Komplexität von s nicht erforderlich ist. Beispielsweise ist für die grundlegende Image Gallery-App, die in dieser exemplarischen Vorgehensweise `Fragment`veranschaulicht wird, keine Verwendung von erforderlich. Da der Inhalt statisch ist und der Benutzer nur zwischen verschiedenen Bildern hin-und hergleitet, kann die Implementierung mithilfe von standardmäßigen Android-Ansichten und-Layouts einfacher gehalten werden. 

## <a name="start-an-app-project"></a>Starten eines App-Projekts

Erstellen Sie ein neues Android-Projekt namens **treepager** (Weitere Informationen zum Erstellen neuer Android-Projekte finden Sie [unter Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) ). Starten Sie als nächstes den nuget-Paket-Manager. (Weitere Informationen zum Installieren von nuget-Paketen finden [Sie unter Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)Ihr Projekt). Suchen und Installieren der **Android-Unterstützungs Bibliothek V4**: 

[![Screenshot der im nuget-Paket-Manager ausgewählten Unterstützung von v4 nuget](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

Dadurch werden auch alle zusätzlichen Pakete installiert, die von der **Android-Unterstützungs Bibliothek V4**wieder hergestellt wurden.

## <a name="add-an-example-data-source"></a>Hinzufügen einer Beispiel Datenquelle

In diesem Beispiel stellt die Struktur Katalog-Datenquelle (dargestellt durch `TreeCatalog` die-Klasse) `ViewPager` den mit Element Inhalt bereit. 
`TreeCatalog`enthält eine vorgefertigte Auflistung von Struktur Bildern und Baum Titeln, die vom Adapter zum Erstellen `View`von s verwendet werden. Der `TreeCatalog` Konstruktor erfordert keine Argumente:

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

Die Auflistung von Bildern in `TreeCatalog` ist so organisiert, dass auf jedes Bild von einem Indexer zugegriffen werden kann. Mit der folgenden Codezeile wird beispielsweise die Image-Ressourcen-ID für das dritte Image in der-Auflistung abgerufen: 

```csharp
int imageId = treeCatalog[2].imageId;
```

Da die Implementierungsdetails von `TreeCatalog` nicht für das Verständnis `ViewPager`relevant sind, `TreeCatalog` wird der Code hier nicht aufgeführt. Der Quellcode für `TreeCatalog` ist unter [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)verfügbar. Laden Sie diese Quelldatei herunter (oder kopieren Sie den Code, und fügen Sie ihn in eine neue **TreeCatalog.cs** -Datei ein), und fügen Sie ihn dem Projekt hinzu Laden Sie außerdem die [Bilddateien](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) herunter, und entpacken Sie Sie in Ihren **Ressourcen/drawable** -Ordner, und fügen Sie Sie in das Projekt ein. 

## <a name="create-a-viewpager-layout"></a>Erstellen eines viewpager-Layouts

Öffnen Sie **Resources/Layout/Main. axml** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

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

Dieser Code führt folgende Schritte aus:

1. Legt die Ansicht aus der **Main. axml** -layoutressource fest.

2. Ruft einen Verweis auf den `ViewPager` aus dem Layout ab.

3. Instanziiert eine neue `TreeCatalog` als Datenquelle.

Wenn Sie diesen Code erstellen und ausführen, sollte eine Anzeige angezeigt werden, die dem folgenden Screenshot ähnelt: 

[![Screenshot der APP, die einen leeren viewpager anzeigt](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

An diesem Punkt ist der `ViewPager` leer, da ihm ein Adapter für den Zugriff auf den Inhalt in **treecatalog**fehlt. Im nächsten Abschnitt wird ein **PagerAdapter** erstellt, um eine Verbindung `ViewPager` mit der **treecatalog**herzustellen. 

## <a name="create-the-adapter"></a>Erstellen des Adapters

`ViewPager`verwendet ein Adapter Controller Objekt, das sich zwischen `ViewPager` der-und der-Datenquelle befindet (siehe Abbildung in [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter)). Um auf diese Daten zuzugreifen, erfordert `ViewPager` , dass Sie einen benutzerdefinierten Adapter bereitstellen `PagerAdapter`, der von abgeleitet ist. Dieser Adapter füllt jede `ViewPager` Seite mit Inhalt aus der Datenquelle auf. Da diese Datenquelle App-spezifisch ist, ist der benutzerdefinierte Adapter der Code, der den Zugriff auf die Daten versteht. Wenn der Benutzer die Seiten des `ViewPager`durchläuft, extrahiert der Adapter Informationen aus der Datenquelle und lädt Sie in die Seiten, die `ViewPager` angezeigt werden. 

Wenn Sie einen `PagerAdapter`implementieren, müssen Sie Folgendes überschreiben:

- **Instantiateitem** Erstellt die Seite (`View`) für eine angegebene Position und fügt Sie der `ViewPager`Auflistung von Ansichten hinzu. &ndash; 

- **Destroyitem** &ndash; Entfernt eine Seite aus einer angegebenen Position.

- **Anzahl** &ndash; Schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Sichten (Seiten) zurückgibt. 

- **Isviewfromubject** &ndash; Bestimmt, ob eine Seite mit einem bestimmten Schlüsselobjekt verknüpft ist. (Dieses Objekt wird von der `InstantiateItem` -Methode erstellt.) In diesem Beispiel ist das Schlüsselobjekt das `TreeCatalog` Datenobjekt.

Fügen Sie eine neue Datei mit dem Namen **TreePagerAdapter.cs** hinzu, und ersetzen Sie Ihren Inhalt durch den folgenden Code: 

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

Mit diesem Code wird die wesentliche `PagerAdapter` -Implementierung Stubel. In den folgenden Abschnitten wird jede dieser Methoden durch funktionierenden Code ersetzt. 

### <a name="implement-the-constructor"></a>Implementieren des Konstruktors

Wenn die APP instanziiert `TreePagerAdapter`, stellt Sie einen Kontext (die `MainActivity`) und eine instanziierte `TreeCatalog`bereit. Fügen Sie die folgenden Member-und Konstruktoren am Anfang der `TreePagerAdapter` -Klasse in **TreePagerAdapter.cs**hinzu: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

Der Zweck dieses Konstruktors besteht darin, den Kontext und `TreeCatalog` die Instanz zu speichern, die `TreePagerAdapter` von verwendet werden. 

### <a name="implement-count"></a>Anzahl implementieren

Die `Count` Implementierung ist relativ einfach: Sie gibt die Anzahl der Strukturen im Struktur Katalog zurück. Ersetzen Sie den Code `Count` durch folgenden Code:

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

Die `NumTrees` -Eigenschaft `TreeCatalog` von gibt die Anzahl der Bäume (Anzahl der Seiten) im DataSet zurück.

### <a name="implement-instantiateitem"></a>Instantiateitem implementieren

Die `InstantiateItem` -Methode erstellt die Seite für eine angegebene Position. Außerdem muss die neu erstellte Sicht der `ViewPager`Ansichts Auflistung von hinzugefügt werden. Um dies zu ermöglichen, übergibt `ViewPager` sich der als Container Parameter. 

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

Dieser Code führt folgende Schritte aus:

1. Instanziiert einen neuen `ImageView` , um das Struktur Bild an der angegebenen Position anzuzeigen. Der- `MainActivity` Wert der APP ist der Kontext, der an den `ImageView` -Konstruktor übergeben wird.

2. Legt die `ImageView` Ressource auf die `TreeCatalog` Bild Ressourcen-ID an der angegebenen Position fest.

3. Wandelt den bestandenen `View` Container in `ViewPager` einen-Verweis um.
    Beachten Sie, dass Sie `JavaCast<ViewPager>()` verwenden müssen, um diese Umwandlung ordnungsgemäß auszuführen (Dies ist erforderlich, damit Android eine vom Lauf Zeit Modul aktivierte Typkonvertierung ausführt).

4. Fügt der instanziierten `ImageView` `ViewPager` hinzu und gibt das `ImageView` an den Aufrufer zurück.

Wenn das Bild in `position` `ViewPager` anzeigt, wird dieses `ImageView`angezeigt. Anfänglich `InstantiateItem` wird zweimal aufgerufen, um die ersten beiden Seiten mit Sichten aufzufüllen. Wenn der Benutzer einen Bildlauf ausführt, wird er erneut aufgerufen, um Ansichten direkt hinter und vor dem aktuell angezeigten Element beizubehalten. 

### <a name="implement-destroyitem"></a>Implementieren von destroyitem

Die `DestroyItem` -Methode entfernt eine Seite aus der angegebenen Position. In apps, in denen sich die Ansicht an einer beliebigen Position `ViewPager` ändern kann, muss eine veraltete Ansicht an dieser Stelle entfernen, bevor Sie durch eine neue Ansicht ersetzt wird. Im Beispiel wird die Ansicht an den einzelnen Positionen nicht geändert, sodass eine Ansicht, die von `DestroyItem` entfernt wurde, einfach erneut hinzugefügt `InstantiateItem` wird, wenn für diese Position aufgerufen wird. `TreeCatalog` (Um eine bessere Effizienz zu erzielen, könnte ein Pool für `View`die Wiederverwendung implementiert werden, der an derselben Position erneut angezeigt wird.) 

Ersetzen Sie die `DestroyItem`-Methode durch folgenden Code: 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

Dieser Code führt folgende Schritte aus:

1. Wandelt den bestandenen `View` Container in `ViewPager` einen-Verweis um.

2. Wandelt das bestandene Java-`view`Objekt () C# `View` in`view as View`ein () um.

3. Entfernt die Ansicht aus `ViewPager`. 

### <a name="implement-isviewfromobject"></a>Implementieren von isviewfromubject

Wenn der Benutzer die Seiten des Inhalts nach links und rechts bewegt `ViewPager` , `IsViewFromObject` wird von aufgerufen, `View` um zu überprüfen, ob das untergeordnete Element an der angegebenen Position mit dem Objekt des Adapters für dieselbe Position verknüpft ist (daher wird das Objekt des Adapters als *Objekt Schlüssel*). Für relativ einfache Apps ist die Zuordnung eine der Identitäten &ndash; . der Objekt Schlüssel des Adapters bei dieser Instanz ist die Ansicht, die zuvor an die `ViewPager` via `InstantiateItem`zurückgegeben wurde. Bei anderen apps kann es sich bei dem Objekt Schlüssel jedoch um eine andere adapterspezifische Klasseninstanz handeln, die der untergeordneten Ansicht `ViewPager` zugeordnet ist, die an dieser Position angezeigt wird (aber nicht gleich). Nur der Adapter weiß, ob die bestandene Sicht und der Objekt Schlüssel zugeordnet sind oder nicht. 

`IsViewFromObject`muss implementiert werden, `PagerAdapter` damit ordnungsgemäß funktioniert. Wenn `IsViewFromObject` für `false` eine bestimmte Position zurückgibt `ViewPager` , wird die Ansicht an dieser Position nicht angezeigt. In der `TreePager` APP *ist* der von `InstantiateItem` zurückgegebene Objekt Schlüssel die Seite `View` einer Struktur, daher muss der Code nur die Identität überprüfen (d. h. der Objekt Schlüssel und die Ansicht sind gleich). Ersetzen Sie den Code `IsViewFromObject` durch folgenden Code: 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zum viewpager

Nachdem der `TreePagerAdapter` implementiert wurde, ist es an der Zeit, ihn dem `ViewPager`hinzuzufügen. Fügen Sie in **MainActivity.cs**die folgende Codezeile am Ende der `OnCreate` -Methode hinzu:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

Dieser Code instanziiert den `TreePagerAdapter`und übergibt `MainActivity` als Kontext (`this`). Die instanziierte `TreeCatalog` wird an das zweite Argument des Konstruktors übergeben. Die `ViewPager`- `Adapter` Eigenschaft wird auf das instanziierte `TreePagerAdapter` -Objekt festgelegt. dadurch `TreePagerAdapter` wird der `ViewPager`in das-Objekt eingebunden. 

Die Kern Implementierung ist nun Complete &ndash; Build und führt die APP aus. Es sollte angezeigt werden, dass das erste Bild des Struktur Katalogs auf dem Bildschirm angezeigt wird, wie auf der linken Seite im nächsten Screenshot dargestellt. Wischen Sie nach links, um weitere Struktur Ansichten anzuzeigen, und wischen Sie nach rechts, um durch den Struktur Katalog zurückzukehren: 

[![Screenshots der treepager-app durch Schwenken von Struktur Bildern](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>Hinzufügen eines Pager-Indikators

Diese minimale `ViewPager` Implementierung zeigt die Bilder des Struktur Katalogs an, gibt jedoch keinen Hinweis darauf, wo sich der Benutzer im Katalog befindet. Der nächste Schritt besteht darin, eine `PagerTabStrip`hinzuzufügen. Der `PagerTabStrip` informiert den Benutzer darüber, welche Seite angezeigt wird, und stellt Navigations Kontext bereit, indem er einen Hinweis der vorherigen und der nächsten Seiten anzeigt. `PagerTabStrip`ist für die Verwendung als Indikator für die aktuelle Seite eines-s gedacht `ViewPager`und führt einen Bildlauf durch und aktualisiert, wenn der Benutzer die einzelnen Seiten durchläuft. 

Öffnen Sie **Resources/Layout/Main. axml** , und `PagerTabStrip` fügen Sie dem Layout ein hinzu:

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

`ViewPager`und `PagerTabStrip` sind für die Zusammenarbeit konzipiert. Wenn Sie einen `PagerTabStrip` innerhalb eines `ViewPager` Layouts deklarieren, `ViewPager` findet automatisch den `PagerTabStrip` und verbindet ihn mit dem Adapter. Wenn Sie die APP erstellen und ausführen, sollte die leere `PagerTabStrip` am oberen Bildschirmrand angezeigt werden: 

[![Bildschirm Abbildung eines leeren PagerTabStrip](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)

### <a name="display-a-title"></a>Anzeigen eines Titels

Um jeder Seiten Registerkarte einen Titel hinzuzufügen, implementieren `GetPageTitleFormatted` Sie die- `PagerAdapter`Methode in der von abgeleiteten Klasse. `ViewPager`Ruft `GetPageTitleFormatted` (falls implementiert) zum Abrufen der Titel Zeichenfolge auf, die die Seite an der angegebenen Position beschreibt. Fügen Sie der- `TreePagerAdapter` Klasse in **TreePagerAdapter.cs**die folgende Methode hinzu: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

Dieser Code Ruft die Zeichenfolge der Struktur Beschriftung von der angegebenen Seite (Position) im Struktur Katalog ab, konvertiert sie in Java `String`und gibt Sie `ViewPager`an zurück. Wenn Sie die APP mit dieser neuen Methode ausführen, wird auf jeder Seite die Struktur Beschriftung im `PagerTabStrip`angezeigt. Der Name der Struktur sollte ganz oben auf dem Bildschirm angezeigt werden, ohne dass ein Unterstrich angezeigt wird: 

[![Screenshots von Seiten mit Text gefüllten PagerTabStrip Registerkarten](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

Sie können hin und her schwenken, um jedes Bild der beschrifteten Struktur im Katalog anzuzeigen. 

### <a name="pagertitlestrip-variation"></a>PagerTitleStrip-Variation

`PagerTitleStrip`ähnelt `PagerTabStrip` , mit der Ausnahme, `PagerTabStrip` dass eine Unterstreichung für die aktuell ausgewählte Registerkarte hinzufügt. Sie können durch `PagerTabStrip` `PagerTitleStrip` im obigen Layout ersetzen und die APP erneut ausführen, um zu `PagerTitleStrip`sehen, wie Sie aussieht: 

[![PagerTitleStrip mit unterstrichen aus Text entfernt](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

Beachten Sie, dass die Unterstreichung entfernt wird, `PagerTitleStrip`Wenn Sie in konvertieren. 

## <a name="summary"></a>Zusammenfassung

Diese exemplarische Vorgehensweise bietet ein schrittweises Beispiel für das Erstellen einer Basis `ViewPager`basierten App ohne Verwendung `Fragment`von s. Es wurde eine Beispiel Datenquelle mit Bildern und Beschriftungs Zeichenfolgen angezeigt, ein `ViewPager` Layout zum Anzeigen der Bilder und eine `PagerAdapter` Unterklasse `ViewPager` , die den mit der Datenquelle verbindet. Um dem Benutzer beim Navigieren durch das DataSet zu helfen, sind Anweisungen enthalten, die das hinzu `PagerTabStrip` fügen `PagerTitleStrip` von oder zum Anzeigen der Bild Beschriftung am oberen Rand jeder Seite erläutern. 

## <a name="related-links"></a>Verwandte Links

- [Treepager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
