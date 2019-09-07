---
title: Xamarin. Android-API-Entwurfs Prinzipien
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 0b3d8fc4836f6f6d1f6bf30b555e3c5c285678f0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756866"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin. Android-API-Entwurfs Prinzipien

Zusätzlich zu den Basisklassen Bibliotheken, die Teil von Mono sind, werden in xamarin. Android Bindungen für verschiedene Android-APIs geliefert, damit Entwickler Native Android-Anwendungen mit Mono erstellen können.

Im Kern von xamarin. Android gibt es eine Interop-Engine, die die C# Welt mit der Java-Welt verbindet und Entwicklern den Zugriff auf die Java- C# APIs von oder anderen .NET-Sprachen ermöglicht.

## <a name="design-principles"></a>Entwurfs Prinzipien

Dies sind einige unserer Entwurfs Prinzipien für die xamarin. Android-Bindung.

- Entsprechen den [.NET Framework Entwurfs Richtlinien](https://docs.microsoft.com/dotnet/standard/design-guidelines/).

- Ermöglicht Entwicklern die Unterklasse von Java-Klassen.

- Die Unterklasse sollte mit C# standardkonstrukten funktionieren.

- Von einer vorhandenen Klasse ableiten.

- Ruft den Basiskonstruktor für die Verkettung auf.

- Über schreibende Methoden sollten mit C#dem Überschreibungs System durchgeführt werden.

- Vereinfachen Sie häufige Java-Aufgaben und harte Java-Aufgaben.

- Verfügbar machen von JavaBean C# -Eigenschaften als Eigenschaften.

- Stellen Sie eine stark typisierte API bereit:

  - Erhöhen Sie die Typsicherheit.

  - Minimieren Sie Laufzeitfehler.

  - IntelliSense der IDE bei Rückgabe Typen erhalten.

  - Ermöglicht die IDE-Popup Dokumentation.

- In-IDE-Untersuchung der APIs fördern:

  - Verwenden von frameworkalternativen zum Minimieren der Verfügbarkeit von Java-Klassen

  - C# Machen Sie, wenn zutreffend und anwendbar, Delegaten (Lambdas, anonyme Methoden und System. Delegat) anstelle von Einzel Methoden Schnittstellen verfügbar.

  - Stellen Sie einen Mechanismus bereit, um beliebige Java-Bibliotheken ( [Android. Runtime. jnienv](xref:Android.Runtime.JNIEnv)) aufzurufen.

## <a name="assemblies"></a>Assemblys

Xamarin. Android enthält eine Reihe von Assemblys, die das *monomobile-Profil*bilden. Die [Seite](~/cross-platform/internals/available-assemblies.md) "Assemblys" enthält weitere Informationen.

Die Bindungen an die Android-Plattform sind in der `Mono.Android.dll` Assembly enthalten. Diese Assembly enthält die gesamte Bindung für die Nutzung von Android-APIs und die Kommunikation mit der Android-Lauf Zeit-VM.

## <a name="binding-design"></a>Bindungs Entwurf

### <a name="collections"></a>Auflistungen

Die Android-APIs verwenden die Java. util-Auflistungen häufig zum Bereitstellen von Listen, Sätzen und Zuordnungen. Diese Elemente werden mithilfe der [System. Collections. Generic](xref:System.Collections.Generic) -Schnittstellen in unserer Bindung verfügbar gemacht. Die grundlegenden Zuordnungen lauten:

- ["java. util.\<Set E >](https://developer.android.com/reference/java/util/Set.html) " wird dem Systemtyp [ICollection\<t >](xref:System.Collections.Generic.ICollection`1), Hilfsklasse [Android. Runtime.\<javaset t >](xref:Android.Runtime.JavaSet`1)zugeordnet.

- ["java. util.\<List E >](https://developer.android.com/reference/java/util/List.html) " ist der Systemtyp [IList\<t >](xref:System.Collections.Generic.IList`1), Hilfsklasse [Android. Runtime. javalist\<T >](xref:Android.Runtime.JavaList`1)zugeordnet.

- [java. util. map < K, V >](https://developer.android.com/reference/java/util/Map.html) dem Systemtyp [IDictionary < TKey, TValue >](xref:System.Collections.Generic.IDictionary`2), Helper Class [Android. Runtime. javadictionary < K, V >](xref:Android.Runtime.JavaDictionary`2)zugeordnet.

- ["java. util.\<Collection E >](https://developer.android.com/reference/java/util/Collection.html) " ist dem Systemtyp [ICollection\<t >](xref:System.Collections.Generic.ICollection`1), Hilfsklasse [Android. Runtime. javacollection\<T >](xref:Android.Runtime.JavaCollection`1)zugeordnet.

Wir haben Hilfsklassen bereitgestellt, um ein schnelleres copyless-Marshalling dieser Typen zu ermöglichen. Wir empfehlen, nach Möglichkeit diese bereitgestellten Auflistungen anstelle der von Frameworks bereit [`List<T>`](xref:System.Collections.Generic.List`1) gestellten [`Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2)Implementierung wie oder zu verwenden. Die [Android. Runtime](xref:Android.Runtime) -Implementierungen verwenden intern eine native Java-Auflistung und müssen daher bei der Übergabe an ein Android-API-Mitglied nicht in eine und aus einer nativen Auflistung kopiert werden.

Sie können jede Schnittstellen Implementierung an eine Android-Methode übergeben, die diese Schnittstelle annimmt, `List<int>` z. b. ein an den [&lt;arrayadapter int&gt;(context&lt;,&gt;int, IList int)](xref:Android.Widget.ArrayAdapter`1) -Konstruktor übergeben. *Allerdings*muss bei allen Implementierungen *außer* den Android. Runtime-Implementierungen dies dazu führen, dass die Liste aus der Mono-VM in die Android-Lauf Zeit-VM *kopiert* wird. , Wenn die Liste später in der Android-Laufzeit geändert wird (z. b. durch Aufrufen von [arrayadapter&lt;&gt;T). Add (T)](xref:Android.Widget.ArrayAdapter`1.Add*) -Methode), werden diese Änderungen in verwaltetem Code *nicht* angezeigt. Wenn eine `JavaList<int>` verwendet wurde, sind diese Änderungen sichtbar.

Umformuliert: Auflistungs Schnittstellen Implementierungen, bei denen es sich *nicht* um eine der oben aufgeführten **Hilfsklassen**handelt, sind nur Marshal [in]:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```

### <a name="properties"></a>Eigenschaften

Java-Methoden werden nach Bedarf in Eigenschaften transformiert:

- Das Java-Methoden `T getFoo()` Paar `void setFoo(T)` und werden in die `Foo` -Eigenschaft transformiert. Beispiel: [Aktivität. Intent](xref:Android.App.Activity.Intent).

- Die Java- `getFoo()` Methode wird in die schreibgeschützte foo-Eigenschaft transformiert. Beispiel: [Context. PackageName](xref:Android.Content.Context.PackageName).

- Nur festgelegte Eigenschaften werden nicht generiert.

- Eigenschaften werden *nicht* generiert, wenn der Eigenschaftentyp ein Array wäre.

### <a name="events-and-listeners"></a>Ereignisse und Listener

Die Android-APIs basieren auf Java, und die zugehörigen Komponenten folgen dem Java-Muster zum Einbinden von Ereignislistenern. Dieses Muster ist tendenziell mühsam, da der Benutzer eine anonyme Klasse erstellen und die zu über schreibenden Methoden deklarieren muss, z. b., wie dies in Android mit Java durchgeführt werden soll:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Der entsprechende Code bei C# der Verwendung von Ereignissen lautet wie folgt:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Beachten Sie, dass beide oben genannten Mechanismen mit xamarin. Android verfügbar sind. Sie können eine listenerschnittstelle implementieren und Sie mit "View. seetonclicklistener" anfügen, oder Sie können einen Delegaten, der C# über eines der üblichen Paradigmen erstellt wurde, an das Click-Ereignis anfügen.

Wenn die Listener-Rückruf Methode eine void-Rückgabe aufweist, erstellen wir API-Elemente basierend auf einem [EventHandler&lt;&gt; TEventArgs](xref:System.EventHandler`1) -Delegaten. Wir generieren ein Ereignis wie das obige Beispiel für diese Listenertypen. Wenn der Listener-Rückruf jedoch einen nicht leeren und einen nicht **booleschen** Wert zurückgibt, werden keine Ereignisse und Eventhandlers verwendet. Stattdessen wird ein bestimmter Delegat für die Signatur des Rückrufs generiert und anstelle von Ereignissen Eigenschaften hinzugefügt. Der Grund hierfür ist das Behandeln der delegataufruhenfolge und die Rückgabe der Verarbeitung. Bei diesem Ansatz wird die Verwendung der xamarin. IOS-API widerspiegeln.

C#Ereignisse oder Eigenschaften werden nur automatisch generiert, wenn die Android-Ereignis Registrierungsmethode:

1. Verfügt über `set` ein Präfix, z. b. [ *set*onclicklistener](xref:Android.Views.View.SetOnClickListener*).

1. Hat einen `void` Rückgabetyp.

1. Akzeptiert nur einen Parameter, der Parametertyp ist eine Schnittstelle, die Schnittstelle verfügt nur über eine Methode, und der `Listener` Schnittstellen Name endet mit, z. b. [View. OnClick *Listener*](xref:Android.Views.View.IOnClickListener).

Wenn die listenerschnittstellenmethode den Rückgabetyp **booleschen** anstelle von **void**aufweist, enthält die generierte *EventArgs* -Unterklasse außerdem eine *behandelte* Eigenschaft. Der Wert der Eigenschaft *behandelt* wird als Rückgabewert für die *listenermethode* verwendet, und der Standard `true`Wert ist.

Beispielsweise akzeptiert die Android [View. log keylistener ()](xref:Android.Views.View.SetOnKeyListener*) -Methode die [View. onkeylistener](xref:Android.Views.View.IOnKeyListener) -Schnittstelle, und die [View. onkeylistener. OnKey (View, int, KeyEvent)](xref:Android.Views.View.IOnKeyListener.OnKey*) -Methode weist einen booleschen Rückgabetyp auf. Xamarin. Android generiert ein entsprechendes [View. KeyPress](xref:Android.Views.View.KeyPress) -Ereignis, bei dem es sich um eine [EventHandler&lt;-Ansicht handelt&gt;. KeyEventArgs](xref:Android.Views.View.KeyEventArgs).
Die *KeyEventArgs* -Klasse verfügt wiederum über eine [View. KeyEventArgs. behandelte](xref:Android.Views.View.KeyEventArgs.Handled) -Eigenschaft, die als Rückgabewert für die *View. onkeylistener. OnKey ()* -Methode verwendet wird.

Wir beabsichtigen, über Ladungen für andere Methoden und Ktoren hinzuzufügen, um die delegatbasierte Verbindung verfügbar zu machen. Außerdem erfordern Listener mit mehreren Rückrufen einige zusätzliche Überprüfungen, um zu bestimmen, ob die Implementierung einzelner Rückrufe sinnvoll ist. Daher werden diese bei der Identifizierung entsprechend umgerechnet. Wenn kein entsprechendes Ereignis vorhanden ist, müssen Listener in C#verwendet werden, aber bringen Sie alle Benutzer, die Sie in Betracht ziehen könnten, an unsere Aufmerksamkeit. Wir haben auch einige Konvertierungen von Schnittstellen ohne das Suffix "Listener" durchgeführt, wenn es klar war, dass Sie von einer delegatalternative profitieren würden.

Alle Listener-Schnittstellen implementieren das[`Android.Runtime.IJavaObject`](xref:Android.Runtime.IJavaObject)
Schnittstelle, aufgrund der Implementierungsdetails der Bindung, sodass Listenerklassen diese Schnittstelle implementieren müssen. Dies kann erreicht werden, indem die listenerschnittstelle in einer Unterklasse von [java. lang. Object](xref:Java.Lang.Object) oder einem beliebigen anderen umschließenden Java-Objekt, z. b. eine Android-Aktivität, implementiert wird.

### <a name="runnables"></a>Runnables

Java verwendet die [java. lang. RUNNABLE](xref:Java.Lang.Runnable) -Schnittstelle zur Bereitstellung eines Delegierungs Mechanismus. Die [java. lang. Thread](xref:Java.Lang.Thread) -Klasse ist ein bedeutender Consumer dieser Schnittstelle. Android hat auch die-Schnittstelle in der API verwendet.
" [Activity. runonuithread ()](xref:Android.App.Activity.RunOnUiThread*) " und " [View.Post ()](xref:Android.Views.View.Post*) " sind wichtige Beispiele.

Die `Runnable` -Schnittstelle enthält eine einzelne void-Methode, [Run ()](xref:Java.Lang.Runnable.Run). Daher eignet sich die Bindung in C# als [System. Action](xref:System.Action) -Delegat. Wir haben über Ladungen in der Bindung bereitgestellt, die `Action` einen Parameter für alle API-Member akzeptieren `Runnable` , die in der nativen API einen verwenden, z. b. " [Activity. runonuithread ()](xref:Android.App.Activity.RunOnUiThread*) " und " [View.Post ()](xref:Android.Views.View.Post*)".

Wir haben die [nicht überladbaren](xref:Java.Lang.IRunnable) über Ladungen an der Stelle belassen, anstatt Sie zu ersetzen, da mehrere Typen die-Schnittstelle implementieren und daher direkt als Runnables übermittelt werden können.

### <a name="inner-classes"></a>Innere Klassen

Java verfügt über zwei unterschiedliche Arten von [Klassen](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statische und nicht statische Klassen.

Statische Kryptografieklassen von Java sind C# identisch mit den Typen von Typen.

Nicht statische geschachtelte Klassen, auch als *innere Klassen*bezeichnet, unterscheiden sich erheblich. Sie enthalten einen impliziten Verweis auf eine Instanz Ihres einschließenden Typs und können keine statischen Member enthalten (neben anderen Abweichungen außerhalb des Bereichs dieser Übersicht).

Wenn es um Bindung und C# Verwendung geht, werden statische, in der Form verwendete Klassen als normale Typen behandelt. Innere Klassen haben in der Zwischenzeit zwei bedeutende Unterschiede:

1. Der implizite Verweis auf den enthaltenden Typ muss explizit als Konstruktorparameter bereitgestellt werden.

1. Beim Erben von einer inneren Klasse *muss* die innere Klasse in einem Typ geschachtelt sein, der vom enthaltenden Typ der Basisklasse erbt, und der abgeleitete Typ muss einen Konstruktor desselben Typs bereitstellen wie der C# enthaltende Typ.

Sehen Sie sich beispielsweise die innere Klasse [Android. Service. Wallpaper. wallpaperservice. Engine](xref:Android.Service.Wallpaper.WallpaperService.Engine) an. Da es sich um eine innere Klasse handelt, greift der [wallservice. Engine ()-Konstruktor](xref:Android.Service.Wallpaper.WallpaperService.Engine#ctor) auf eine [wallservice](xref:Android.Service.Wallpaper.WallpaperService) -Instanz zu (vergleichen und steht im Gegensatz zum Java wallservice. Engine ()-Konstruktor, der keine Parameter annimmt).

Eine Beispiel Ableitung einer inneren Klasse ist cubewallpaper. cubeengine:

```csharp
class CubeWallpaper : WallpaperService {
    public override WallpaperService.Engine OnCreateEngine ()
    {
        return new CubeEngine (this);
    }

    class CubeEngine : WallpaperService.Engine {
        public CubeEngine (CubeWallpaper s)
                : base (s)
        {
        }
    }
}
```

Beachten Sie `CubeWallpaper.CubeEngine` , dass in `CubeWallpaper`geschachtelt ist, `CubeWallpaper` von der enthaltenden `WallpaperService.Engine`Klasse von `CubeWallpaper.CubeEngine` erbt und über einen Konstruktor verfügt, der den `CubeWallpaper` deklarierenden Typ annimmt (in diesem Fall alle, wie oben angegeben).

### <a name="interfaces"></a>Schnittstellen

Java-Schnittstellen können drei Sätze von Membern enthalten, von denen zwei C#Probleme verursachen:

1. Methoden

1. Typen

1. Felder

Java-Schnittstellen werden in zwei Typen übersetzt:

1. Eine (optionale) Schnittstelle, die Methoden Deklarationen enthält. Diese Schnittstelle hat denselben Namen wie die Java-Schnittstelle, *mit* dem Unterschied, dass Sie auch ein Präfix " *I* " enthält.

1. Eine (optionale) statische Klasse, die alle in der Java-Schnittstelle deklarierten Felder enthält.

Bei der einschließenden-Schnittstelle werden die untergeordneten Typen als gleich geordnete Elemente der einschließenden Schnittstelle und nicht als untergeordnete Typen mit dem einschließenden Schnittstellennamen als Präfix umschlossen

Sehen Sie sich beispielsweise die " [Android. OS. paramecelable](xref:Android.OS.Parcelable) "-Schnittstelle an.
Die *Schnitt* Stelle, die eine Schnittstelle enthält, enthält Methoden, geschsted Typen und Konstanten. Die *paramenierbaren* Schnittstellen Methoden werden in die [Android. OS. iparamecelable](xref:Android.OS.IParcelable) -Schnittstelle eingefügt.
Die *parameterbaren* Schnittstellen Konstanten werden in den Typ " [Android. OS. parameterablekonsts](xref:Android.OS.ParcelableConsts) " eingefügt. Die geschposteten Typen " [Android. OS. parameterable. classloadercreator\<t >](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) " und " [Android. OS.\<Parser t >](https://developer.android.com/reference/android/os/Parcelable.Creator.html) " sind zurzeit aufgrund von Einschränkungen bei der Generika Unterstützung nicht gebunden. Wenn Sie unterstützt werden, wäre als *Android. OS. iparamecelableclassloadercreator* -und *Android. OS. iparamecelablecreator* -Schnittstelle vorhanden. So ist z. b. die Schnittstelle "netsted [Android. OS. IBinder. deathrecipient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) " als die [Android. OS. ibinderdeathrecipient](xref:Android.OS.IBinderDeathRecipient) -Schnittstelle gebunden.

> [!NOTE]
> Ab xamarin. Android 1,9 werden Java-Schnittstellen Konstanten _dupliziert_ , um das Portieren von Java-Code zu vereinfachen. Dadurch wird das Portieren von Java-Code verbessert, der auf [Android-Anbieter](https://developer.android.com/reference/android/provider/package-summary.html) Schnittstellen Konstanten basiert.

Zusätzlich zu den oben genannten Typen sind vier weitere Änderungen vorhanden:

1. Ein Typ mit dem gleichen Namen wie die Java-Schnittstelle wird generiert, um Konstanten zu enthalten.

1. Typen, die Schnittstellen Konstanten enthalten, enthalten auch alle Konstanten, die von implementierten Java-Schnittstellen stammen.

1. Alle Klassen, die eine Java-Schnittstelle mit Konstanten implementieren, erhalten einen neuen verschachtelten interfaceconsts-Typ, der Konstanten von allen implementierten Schnittstellen enthält.

1. Der Typ " *konsts* " ist mittlerweile veraltet.

Für die *Android. OS. parameterable* -Schnittstelle bedeutet dies, dass jetzt ein [*Android. OS. paramecelable*](xref:Android.OS.Parcelable) -Typ vorhanden ist, der die Konstanten enthalten soll. Beispielsweise wird die Konstante [Parcelable.CONTENTS_FILE_DESCRIPTOR](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) als die Konstante [*Parcelable.ContentsFileDescriptor*](xref:Android.OS.Parcelable.ContentsFileDescriptor) gebunden und nicht als *ParcelableConsts.ContentsFileDescriptor*.

Für Schnittstellen, die Konstanten enthalten, die andere Schnittstellen implementieren, die noch mehr Konstanten enthalten, wird die Vereinigung aller Konstanten nun generiert. Beispielsweise implementiert die [Android. Provider. MediaStore. Video. videocolumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) -Schnittstelle die [Android. Provider. MediaStore. mediacolumschlag](xref:Android.Provider.MediaStore.MediaColumns) -Schnittstelle. Vor 1,9 hat der [Android. Provider. MediaStore. Video. videocolumnskonsts](xref:Android.Provider.MediaStore.Video.VideoColumnsConsts) -Typ jedoch keinen Zugriff auf die Konstanten, die unter [Android. Provider. MediaStore. mediacolenumnskonsts](xref:Android.Provider.MediaStore.MediaColumnsConsts)deklariert werden.
Folglich muss der Java-Ausdruck *MediaStore. Video. videocolumns. Title* an den C# Ausdruck *MediaStore. Video. mediacolumschlag. Title* gebunden werden, der nur schwer zu erkennen ist, ohne viele Java-Dokumentationen zu lesen. In 1,9 lautet der äquivalente C# Ausdruck " [MediaStore. Video. videocolumns. Title](xref:Android.Provider.MediaStore.Video.VideoColumns.Title)".

Beachten Sie außerdem den Typ " [Android. OS. Bundle](xref:Android.OS.Bundle) ", der die *Schnittstelle* für die Java-Schnittstelle implementiert. Da die-Schnittstelle implementiert wird, kann auf alle Konstanten in dieser Schnittstelle "durch" der Bundle-Typ zugegriffen werden, z. b. *Bundle. CONTENTS_FILE_DESCRIPTOR* ist ein vollständig gültiger Java-Ausdruck.
Zuvor mussten C# Sie zum Portieren dieses Ausdrucks alle Schnittstellen betrachten, die implementiert werden, um zu sehen, von welchem Typ das *CONTENTS_FILE_DESCRIPTOR* stammt. Ab xamarin. Android 1,9 verfügen Klassen, die Java-Schnittstellen implementieren, die Konstanten enthalten, über einen verschachtelten *interfaceconsts* -Typ, der alle geerbten Schnittstellen Konstanten enthält. Dies ermöglicht das Übersetzen von *Bundle. CONTENTS_FILE_DESCRIPTOR* in [*Bundle. interfakeconsts. contentsfiledescriptor*](xref:Android.OS.Bundle.InterfaceConsts.ContentsFileDescriptor).

Schließlich sind Typen mit dem *Suffix "* Configuration Manager", wie z *. b. "Android. OS. Parser* ", mittlerweile veraltet, außer den neu eingeführten verschachtelten Typen "interfakeconsts". Sie werden in xamarin. Android 3,0 entfernt.

## <a name="resources"></a>Ressourcen

Bilder, Layoutbeschreibungen, binäre BLOB und Zeichen folgen Wörterbücher können als [Ressourcen Dateien](https://developer.android.com/guide/topics/resources/providing-resources.html)in Ihre Anwendung eingeschlossen werden.
Verschiedene Android-APIs sind so konzipiert, dass Sie mit [den Ressourcen-IDs arbeiten](https://developer.android.com/guide/topics/resources/accessing-resources.html) , anstatt Sie direkt mit Bildern, Zeichen folgen oder Binär-BLOB zu behandeln.

Beispiel: eine Android-Beispiel-APP, die ein Benutzeroberflächen Layout `main.axml`(), eine Internationalisierungs Tabellen `strings.xml`Zeichenfolge () `drawable-*/icon.png`und einige Symbole () enthält, behält ihre Ressourcen im Verzeichnis "Resources" der Anwendung bei:

```
Resources/
    drawable-hdpi/
        icon.png

    drawable-ldpi/
        icon.png

    drawable-mdpi/
        icon.png

    layout/
        main.axml

    values/
        strings.xml
```

Die nativen Android-APIs arbeiten nicht direkt mit Dateinamen, sondern arbeiten stattdessen mit Ressourcen-IDs. Wenn Sie eine Android-Anwendung kompilieren, die Ressourcen verwendet, wird das Buildsystem die Ressourcen für die Verteilung Verpacken und `Resource` eine Klasse mit dem Namen generieren, die die Token für jede enthaltene Ressource enthält. Beispielsweise wird für das oben genannte Ressourcen Layout Folgendes von der R-Klasse bereitgestellt:

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

Verwenden `Resource.Drawable.icon` Sie dann, um auf die `drawable/icon.png` Datei zu verweisen `Resource.Layout.main` , oder, `layout/main.xml` um auf die `Resource.String.first_string` Datei zu verweisen, oder um auf die erste `values/strings.xml`Zeichenfolge in der Wörterbuchdatei zu verweisen.

## <a name="constants-and-enumerations"></a>Konstanten und Enumerationen

Die nativen Android-APIs verfügen über viele Methoden, die einen int-Wert akzeptieren oder zurückgeben, der einem konstanten Feld zugeordnet werden muss, um zu bestimmen, was das int bedeutet. Um diese Methoden zu verwenden, muss der Benutzer die Dokumentation überprüfen, um zu sehen, welche Konstanten geeignete Werte sind, die kleiner als ideal sind.

Beachten Sie beispielsweise " [Activity. requestwindowfeature" (int FeatureId)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

In diesen Fällen ist es erforderlich, Verwandte Konstanten in eine .net-Enumeration zu gruppieren und die-Methode neu zuzuordnen, um stattdessen die-Enumeration zu verwenden.
Auf diese Weise können wir die IntelliSense-Auswahl der möglichen Werte anbieten.

Im obigen Beispiel wird Folgendes veranschaulicht: [Activity.RequestWindowFeature(WindowFeatures featureId)](xref:Android.App.Activity.RequestWindowFeature*).

Beachten Sie, dass es sich hierbei um einen sehr manuellen Prozess handelt, um herauszufinden, welche Konstanten zueinander gehören und welche APIs diese Konstanten nutzen. Melden Sie Fehler für alle Konstanten, die in der API verwendet werden und besser als Enumeration ausgedrückt werden könnten.
