---
title: API-Entwurf
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a9c0b02457f006f75dc5b6f0a52e68865d620f67
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="api-design"></a>API-Entwurf


## <a name="overview"></a>Übersicht

Bindungen für verschiedene Android-APIs ermöglichen Entwicklern das Erstellen von systemeigener Android-Anwendungen mit Mono Lieferumfang Xamarin.Android, zusätzlich zu den Kern des Basis-Klassenbibliotheken, die Teil der Mono sind.

Den Kern der Xamarin.Android es ist ein Interop-Modul, Bridges die C#-Welt mit der Außenwelt Java und bietet Entwicklern den Zugriff an die Java-APIs von C#- oder andere .NET-Sprachen:.


## <a name="design-principles"></a>Entwurfsprinzipien

Dies sind einige der für die Bindung Xamarin.Android unsere Entwurfsprinzipien

-  Entsprechen die [.NET Framework-Entwurfsrichtlinien](http://msdn.microsoft.com/en-us/library/ms229042.aspx).

-  Ermöglichen Sie Entwicklern Unterklasse Java-Klassen.

-  C#-standard-Konstrukte unterstützt auch Unterklasse.

-  Leiten Sie von einer vorhandenen Klasse.

-  Rufen Sie Basiskonstruktor zu verketten.

-  Überschreiben von Methoden sollten mit # Außerkraftsetzung System durchgeführt werden.

-  Stellen Sie Java allgemeiner easy und schwer Java-Aufgaben möglich.

-  Machen Sie JavaBean Eigenschaften als C#-Eigenschaften verfügbar.

-  Stellen Sie eine stark typisierte-API:

    - Typsicherheit zu erhöhen.

    - Minimieren Sie Laufzeitfehler.

    - Abrufen von IDE Intellisense für Rückgabetypen.

    - Ermöglicht die IDE-Popup-Dokumentation.

-  Ermutigen Sie in der IDE zum Durchsuchen von APIs:

    - Nutzen Sie Framework Alternativen zur Offenlegung von Java-Classlib zu minimieren.

    - Machen Sie C#-Delegaten (Lambdas, anonyme Methoden und System.Delegate) anstelle von einzelnen-Method-Schnittstellen verfügbar, wenn entsprechende und anwendbar.

    - Bereitstellen eines Mechanismus, um beliebige Java-Bibliotheken aufrufen ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).



## <a name="assemblies"></a>Assemblys

Xamarin.Android umfasst eine Reihe von Assemblys, die bilden die *MonoMobile Profil*. Die [Assemblys](~/cross-platform/internals/available-assemblies.md) Seite verfügt über mehr Informationen.

Die Bindungen für die Android-Plattform sind in enthalten die `Mono.Android.dll` Assembly. Diese Assembly enthält die gesamte Bindung für verbrauchende Android-APIs und zur Kommunikation mit der Android-Runtime-VM.


## <a name="binding-design"></a>Binden von Entwurf


### <a name="collections"></a>Auflistungen

Die Android-APIs nutzen, die java.util Sammlungen sehr häufig, um Listen, Sätze und Zuordnungen zu gewährleisten. Wir verfügbar zu machen, diese Elemente mithilfe der [System.Collections.Generic](https://developer.xamarin.com/api/namespace/System.Collections.Generic/) Schnittstellen in unserer Bindung. Die grundlegenden Zuordnungen sind:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) Systemtyp ordnet [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), Hilfsklasse [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) Systemtyp ordnet [IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx), Hilfsklasse [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](http://developer.android.com/reference/java/util/Map.html) Systemtyp ordnet [IDictionary < TKey, TValue >](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), Hilfsklasse [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) Systemtyp ordnet [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), Hilfsklasse [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Wir haben Hilfsklassen zur Erleichterung schneller copyless Marshallen dieser Typen bereitgestellt. Wenn möglich, es wird empfohlen, diese Sammlungen anstelle der bereitgestellten Framework-Implementierung bereitgestellt, wie z. B. [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/) oder [ `Dictionary<TKey, TValue>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/). Die [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) Implementierungen eine systemeigene Java-Auflistung intern zu nutzen und benötigen daher keine kopieren in und aus einer systemeigene-Auflistung, bei der Übergabe an ein Android-API-Element.

Können Sie die schnittstellenimplementierung einer zu einer Android-Methode akzeptiert diese Schnittstelle übergeben, übergeben Sie z. B. eine `List<int>` auf die [ArrayAdapter&lt;Int&gt;(Kontext, "Int", "IList&lt;Int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)Konstruktor. *Jedoch*, für alle Implementierungen *außer* für Implementierungen der Android.Runtime dazu *kopieren* der Liste, aus dem Mono virtuellen Computer in der Android-Runtime-VM. Falls die Liste liegt innerhalb der Android-Laufzeit geändert (z. B. durch Aufrufen der [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) Methode), diese Änderungen *nicht* in verwaltetem Code angezeigt werden. Wenn eine `JavaList<int>` wurden verwendet, diese Änderungen wäre sichtbar.

Rephrased, Sammlungen schnittstellenimplementierungen, die *nicht* eine der oben aufgeführten **Hilfsklasse**vorangestellten [In] nur zu marshallen:

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

Java-Methoden werden in den Eigenschaften, wenn dies angebracht transformiert:

-  Die Java-Methodenpaar `T getFoo()` und `void setFoo(T)` umgewandelt werden die `Foo` Eigenschaft. Beispiel: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Die Java-Methode `getFoo()` in die schreibgeschützte Eigenschaft Foo umgewandelt. Beispiel: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Lesegeschützte Eigenschaften werden nicht generiert.

-  Eigenschaften sind *nicht* generiert, wenn die Eigenschaft ein Array sein würde.



### <a name="events-and-listeners"></a>Ereignisse und die Listener

Android-APIs sind baut auf den Java und seine Komponenten folgen dem Java-Muster für Ereignislistener einbinden. Dieses Muster ist aufwendig voraussichtlich benötigt wird, den Benutzer zu erstellen, eine anonyme Klasse deklarieren Sie die Methoden überschreiben, z. B. sieht wie Dinge in Android mit Java ausgeführt werden würde:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Der entsprechende Code in c# mithilfe von Ereignissen würde folgendermaßen lauten:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Beachten Sie, dass beide der oben genannten Mechanismen mit Xamarin.Android verfügbar sind. Sie können eine listenerschnittstelle implementieren, und fügen Sie es mit View.SetOnClickListener, oder Sie können einen Delegaten-Computer erstellte Clientkonfigurationsanforderungen keines der üblichen c#-Paradigmen für das Click-Ereignis anfügen.

Wenn die Rückrufmethode Listener eine "void" zurückgeben enthält, erstellen wir API-Elemente, die auf der Grundlage einer [EventHandler&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/) delegieren. Es generiert ein Ereignis, wie im obigen Beispiel für diesen Listenertypen. Jedoch wenn der Listener-Rückruf, eine nicht "void" und nicht zurückgegeben- **booleschen** Wert, Ereignisse und Ereignishandler werden nicht verwendet. Wir stattdessen generieren einen bestimmten Delegaten für die Signatur des Rückrufs und Hinzufügen von Eigenschaften, die statt der Ereignisse. Der Grund besteht darin, Delegaten Aufrufreihenfolge behandeln und eine Behandlung zurück. Dieser Ansatz spiegelt, was mit der Xamarin.iOS-API erfolgt.

C#-Ereignissen oder Eigenschaften werden nur automatisch generiert, wenn die Registrierung von Android-Ereignisses-Methode:

1. Verfügt über eine `set` Präfix, z. B. [ *festgelegt*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Verfügt über eine `void` Typ zurückgeben.

1. Akzeptiert nur einen Parameter, der Parametertyp ist eine Schnittstelle, die Schnittstelle verfügt über nur eine Methode und der Schnittstellenname endet `Listener` , z. B. [View.OnClick *Listener*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Darüber hinaus, wenn der Listener Methode Schnittstelle hat den Rückgabetyp **booleschen** anstelle von **"void"**, klicken Sie dann auf die generierten *EventArgs* Unterklasse enthält eine *Bearbeitete* Eigenschaft. Der Wert, der die *bearbeitete* Eigenschaft dient als Rückgabewert für die *Listener* -Methode, und es wird standardmäßig auf `true`.

Z. B. das Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) Methode akzeptiert die [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) -Schnittstelle, und die [View.OnKeyListener.onKey (Sicht, "Int", "KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) Methode verfügt über einen Rückgabetyp boolean. Xamarin.Android generiert einen entsprechenden [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) Ereignis dar-ein [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
Die *KeyEventArgs* wiederum-Klasse verfügt über eine [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) -Eigenschaft, die verwendet wird, als der Rückgabewert für die *View.OnKeyListener.onKey()* Methode.

Wir möchten Hinzufügen von Überladungen für andere Methoden und Ctors die Delegaten-basierte Verbindung verfügbar machen. Listener mit mehrere Rückrufe benötigt auch einige weitere Überprüfung, ob einzelne Rückrufe implementieren angebracht ist, ist, damit wir diese konvertiert wird, sobald sie identifiziert werden. Ist kein entsprechendes Ereignis, Listener in c# verwendet werden müssen, aber versetzen Sie die Namenseinträge, die Sie denken möglicherweise Delegaten Nutzung unserer Kenntnis. Wir haben einige Konvertierungen von Schnittstellen, ohne das Suffix "Listener" auch ausgeführt, wenn es wurde deutlich, dass sie eine Alternative Delegaten profitieren würden.

Alle Listener Schnittstellen implementieren die [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) -Schnittstelle, aufgrund der Details zur Implementierung der Bindung, damit die Listenerklassen müssen diese Schnittstelle implementieren. Dies kann geschehen, indem Sie die Listener-Schnittstelle implementieren, auf eine Unterklasse von [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) bzw. alle anderen Java-Objekt, z. B. eine Android-Aktivität umbrochen.


### <a name="runnables"></a>Runnables

Java nutzt die [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) Schnittstelle, um einen delegierungsmechanismus bereitzustellen. Die [java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) Klasse ist eine wichtige Consumer dieser Schnittstelle. Android verfügt über die Schnittstelle in der API ebenfalls eingesetzt.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) und [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) werden relevante Beispiele.

Die `Runnable` Schnittstelle enthält nur die "void" Methode [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Es daher eignet sich für die Bindung als in c# eine [System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx) delegieren. Enthalten Überladungen, die in der Bindung akzeptieren ein `Action` -Parameter für alle API-Mitglieder, von denen Nutzen einer `Runnable` in der systemeigenen API, z. B. [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) und [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Es bleibt die [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) Überladungen vorhanden, statt diese zu ersetzen, da mehrere Typen die Schnittstelle implementieren und daher können direkt als Runnables übergeben werden.


### <a name="inner-classes"></a>Interne Klassen

Java bietet zwei verschiedene Arten von [geschachtelte Klassen](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statische geschachtelte Klassen und nicht statisch.

Java statische geschachtelte Klassen sind identisch mit C#-geschachtelten Typen.

Nicht statische geschachtelte Klassen, die so genannte *innere Klassen*, erheblich unterscheiden. Sie enthalten einen impliziten Verweis auf eine Instanz des einschließenden Typ übereinstimmen und dürfen keine statischen Member (neben anderen Unterschiede außerhalb des Bereichs dieser Übersicht) enthalten.

Bei der Bindung und c# verwenden, werden statische geschachtelte Klassen als normale geschachtelte Typen behandelt. Interne Klassen haben in der Zwischenzeit können zwei wichtige Unterschiede:

1. Der implizite Verweis auf den enthaltenden Typ muss explizit als Konstruktorparameter bereitgestellt werden.

1. Beim Erben von einer inneren Klasse, die die inneren Klasse *müssen* in einem Typ geschachtelt werden, die von der enthaltende Typ der innere Basisklasse erbt, und der abgeleitete Typ muss einen Konstruktor, der den gleichen Typ wie die C#-bereitzustellen, Typ enthält.


Betrachten Sie beispielsweise die [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) inneren Klasse. Da es sich um eine interne Klasse, ist die [WallpaperService.Engine()-Konstruktor](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) erfordert einen Verweis auf eine [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) Instanz (vergleichen und steht im Gegensatz zu der Programmiersprache Java [WallpaperService.Engine ( )-Konstruktor](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) nimmt keine Parameter an die).

Ein Beispiel-Ableitung von einer inneren Klasse handelt es sich um CubeWallpaper.CubeEngine:

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

Hinweis wie `CubeWallpaper.CubeEngine` geschachtelt ist `CubeWallpaper`, `CubeWallpaper` erbt von der enthaltenden Klasse des `WallpaperService.Engine`, und `CubeWallpaper.CubeEngine` verfügt über einen Konstruktor nimmt den deklarierenden Typ-- `CubeWallpaper` in diesem Fall – alle als oben angegeben.


### <a name="interfaces"></a>Schnittstellen

Java-Schnittstellen können drei Gruppen von Elementen enthalten, von denen zwei Probleme verursachen, die sich von c#:

1. Methoden

1. Typen

1. Felder


Java-Schnittstellen werden in zwei Arten übersetzt:

1. Eine (optional)-Schnittstelle, die Methodendeklarationen enthält. Diese Schnittstelle hat den gleichen Namen wie die Java-Schnittstelle *außer* verfügt auch über eine " *ich* ' Präfix.

1. Eine (optionale) statische Klasse, die alle Felder enthält, die in der Java-Schnittstelle deklariert werden.

Geschachtelte Typen sind "verschoben" um die gleichgeordneten Elemente der einschließenden Schnittstelle anstelle von geschachtelten Typen, mit dem einschließenden Schnittstellennamen als Präfix werden.

Betrachten Sie beispielsweise die [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) Schnittstelle.
Die *Parcelable* Schnittstelle enthält Methoden, geschachtelte Typen und Konstanten. Die *Parcelable* Schnittstellenmethoden befinden sich in der [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) Schnittstelle.
Die *Parcelable* Schnittstelle Konstanten befinden sich in der [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) Typ. Die geschachtelte [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) und [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) Typen sind derzeit nicht aufgrund der Einschränkungen in unsere Unterstützung von Generika; gebunden Wenn sie unterstützt wurden, werden als die *Android.OS.IParcelableClassLoaderCreator* und *Android.OS.IParcelableCreator* Schnittstellen. Beispielsweise der geschachtelte [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) Schnittstelle gebunden ist, als die [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) Schnittstelle.


> [!NOTE]
> Beginnen mit dem Xamarin.Android 1.9, Java-Schnittstelle Konstanten sind <em>dupliziert</em> in dem Bestreben, die zur Vereinfachung Portieren von Java-code. Dadurch wird die um Portierung von Java-Code zu verbessern, die benötigt [android Anbieter](http://developer.android.com/reference/android/provider/package-summary.html) Konstanten-Schnittstelle.

Zusätzlich zu den oben erwähnten Typen stehen Ihnen vier weitere Änderungen:

1. Ein Typ mit dem gleichen Namen wie die Java-Schnittstelle wird generiert, um Konstanten enthalten.

1. Typen, die auch mit der Schnittstelle Konstanten enthalten alle Konstanten, die von der Java-Schnittstellen implementiert stammen.

1. Alle Klassen, die Implementierung einer Java-Schnittstelle, die mit Konstanten aus abrufen ein neues geschachtelten InterfaceConsts-Typs mit Konstanten aus allen implementierten Schnittstellen.

1. Die *Consts* Typ ist mittlerweile veraltet.


Für die *android.os.Parcelable* -Schnittstelle, das bedeutet, dass es wird nun ein [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) Typ die Konstanten enthalten. Z. B. die [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) Konstante wird als gebunden werden die [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) konstant, statt als die  *ParcelableConsts.ContentsFileDescriptor* konstant.

Die Union aller Konstanten für Schnittstellen, die Konstanten, die mit anderen Schnittstellen noch weitere Konstanten enthält, jetzt generiert. Z. B. die [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) -Schnittstelle implementiert die [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) Schnittstelle. Allerdings vor 1.9 die [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) Typ hat keine Möglichkeit des Zugriffs auf die Konstanten, die auf [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Als Ergebnis der Java-Ausdruck *MediaStore.Video.VideoColumns.TITLE* muss an den C#-Ausdruck gebunden werden *MediaStore.Video.MediaColumnsConsts.Title* also nur schwer auffindbare ohne lesen große Anzahl von Java-Dokumentation. 1.9, werden der entsprechende C#-Ausdruck [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Darüber hinaus sollten Sie die [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) -Typ, der die Java implementiert *Parcelable* Schnittstelle. Da die Schnittstelle implementiert, alle Konstanten auf diese Schnittstelle zugegriffen werden "durch" Bundle-Typ, z. B. *Bundle.CONTENTS_FILE_DESCRIPTOR* uneingeschränkt Java-Ausdruck ist.
Bisher mussten Sie so portieren Sie diesen Ausdruck in c# alle Schnittstellen betrachten, die implementiert werden, aus welchen Typ die *CONTENTS_FILE_DESCRIPTOR* stammt. Ab Xamarin.Android 1.9 können Klassen implementieren von Java-Schnittstellen die Konstanten enthalten, verfügen über eine geschachtelte *InterfaceConsts* -Typ, der alle geerbten Schnittstelle Konstanten enthalten soll. Dies ermöglicht übersetzen *Bundle.CONTENTS_FILE_DESCRIPTOR* auf [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Zum Schluss-Typen eine *Consts* suffix z. B. *Android.OS.ParcelableConsts* nun veraltet, außer die neu eingeführte InterfaceConsts Typen geschachtelt sind. Diese werden Xamarin.Android 3.0 entfernt.


## <a name="resources"></a>Ressourcen

Bilder, Layout Beschreibungen, binäre Blobs und Wörterbücher Zeichenfolge können in der Anwendung als enthalten [Ressourcendateien](http://developer.android.com/guide/topics/resources/providing-resources.html).
Verschiedene Android-APIs dienen zur [ausgeführt werden, auf die Ressourcen-IDs](http://developer.android.com/guide/topics/resources/accessing-resources.html) anstelle von Bildern, Zeichenfolgen oder binäre blobs direkt.

Angenommen, eine Beispiel Android-app, die ein Layout für die Benutzeroberfläche enthält ( `main.axml`), eine Zeichenfolge der Internationalisierung-Tabelle ( `strings.xml`) und einige Symbole ( `drawable-*/icon.png`) ihre Ressourcen im Verzeichnis "Resources" der Anwendung beibehalten:

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

Das systemeigene Android-APIs funktionieren nicht direkt mit dem Dateinamen, aber er stattdessen auf die Ressourcen-IDs ausgeführt werden. Beim Kompilieren einer Android-Anwendung, die Ressourcen verwendet, das Buildsystem wird die Ressourcen für die Verteilung Verpacken und generieren Sie eine Klasse mit dem Namen `Resource` , das das Token für jede einzelne der enthaltenen Ressourcen enthält. Beispielsweise ist für die oben genannten Ressourcen Layout dies was die R-Klasse verfügbar gemacht würde:

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

Verwenden Sie dann `Resource.Drawable.icon` zu verweisen die `drawable/icon.png` -Datei oder `Resource.Layout.main` zu verweisen die `layout/main.xml` -Datei oder `Resource.String.first_string` Verweis auf die erste Zeichenfolge in der Wörterbuchdatei `values/strings.xml`.


## <a name="constants-and-enumerations"></a>Konstanten und Enumerationen

Das systemeigene Android-APIs haben viele Methoden, die übernehmen oder einen "Int", der zugeordnet werden muss, um ein konstantes Feld, um zu bestimmen, was bedeutet, dass der int-Wert zurückgeben. Der Benutzer muss für die Verwendung dieser Methoden finden Sie in der Dokumentation, um festzustellen, welche Konstanten geeignete Werte sind also nicht optimal.

Betrachten Sie z. B. [Activity.requestWindowFeature (Int FeatureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

In diesen Fällen bemüht wir zum Gruppieren verwandter Konstanten in einer Enumeration .NET und die Methode, um die Enumeration akzeptieren erneut zuordnen können.
Dadurch können wir bieten IntelliSense-Auswahl der möglichen Werte.

Im obigen Beispiel wird: [Activity.RequestWindowFeature (WindowFeatures FeatureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

Beachten Sie, dass dies ein sehr manueller Prozess zu ermitteln, welche Konstanten zusammen gehören, und welche APIs diese Konstanten nutzen. Bitte Datei Fehler für jede Verwendung der Konstanten in der API, die eine bessere Leistung als eine Enumeration ausgedrückt.
