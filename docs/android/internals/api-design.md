---
title: Entwurfsprinzipien für Xamarin.Android-API
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: e762a286069d5ef1db90f3c45808eee0a7a04a7f
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668490"
---
# <a name="xamarinandroid-api-design-principles"></a>Entwurfsprinzipien für Xamarin.Android-API


## <a name="overview"></a>Übersicht

Zusätzlich zu den Basisklassenbibliotheken, die Teil von Mono, Lieferumfang von Xamarin.Android-Bindungen für verschiedene Android-APIs, um Entwicklern das Erstellen von nativen Android-Anwendungen mit Mono zu ermöglichen.

Das Herzstück von Xamarin.Android gibt es ist eine interop-Engine diese Bridges die C#-Welt mit der Java-Welt und bietet Entwicklern Zugriff auf die Java-APIs von C#- oder andere .NET-Sprachen:.


## <a name="design-principles"></a>Entwurfsprinzipien

Hier sind einige unserer Entwurfsprinzipien für die Xamarin.Android-Bindung

-  Entsprechen die [.NET Framework-Entwurfsrichtlinien](https://docs.microsoft.com/dotnet/standard/design-guidelines/).

-  Ermöglicht Entwicklern von Unterklasse Java-Klassen.

-  Mit C#-standard-Konstrukten sollte die Unterklasse funktionieren.

-  Leiten Sie von einer vorhandenen Klasse.

-  Rufen Sie Basiskonstruktor zu verketten.

-  Überschreiben von Methoden sollten mit # Außerkraftsetzung System durchgeführt werden.

-  Stellen Sie den allgemeinen Java-Aufgaben, die einfach und schwer Java-Aufgaben möglich.

-  Machen Sie JavaBean Eigenschaften als c#-Eigenschaften verfügbar.

-  Machen Sie eine stark typisierte API verfügbar:

    - Typsicherheit zu erhöhen.

    - Minimieren Sie die Runtime-Fehler.

    - IDE Intellisense für Rückgabetypen zu erhalten.

    - Ermöglicht die IDE-Popup-Dokumentation.

-  Empfehlen Sie in der IDE Durchsuchen von APIs:

    - Verwenden Sie die Framework-Alternativen für die Offenlegung von Java-Classlib zu minimieren.

    - Machen Sie c#-Delegaten (Lambdas, anonyme Methoden "und" System.Delegate) anstelle von nur einer Methode Schnittstellen verfügbar, wenn es sich bei entsprechenden und anwendbar.

    - Geben Sie einen Mechanismus zum Aufrufen von beliebiger Java-Bibliotheken ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


## <a name="assemblies"></a>Assemblys

Xamarin.Android umfasst eine Reihe von Assemblys, die bilden die *MonoMobile Profil*. Die [Assemblys](~/cross-platform/internals/available-assemblies.md) Seite enthält weitere Informationen.

Die Bindungen für die Android-Plattform befinden sich die `Mono.Android.dll` Assembly. Diese Assembly enthält die gesamte Bindung für Android-Consumer-APIs und die Kommunikation mit der Android-Runtime-VM.


## <a name="binding-design"></a>Design binden


### <a name="collections"></a>Auflistungen

Die Android-APIs nutzen die java.util Sammlungen sehr häufig, um Listen, Sätze und Zuordnungen zu ermöglichen. Wir machen diese Elemente mithilfe der [System.Collections.Generic](xref:System.Collections.Generic) Schnittstellen in der Bindung. Die grundlegenden Zuordnungen sind:

-   [java.util.Set<E> ](https://developer.android.com/reference/java/util/Set.html) Systemtyp ordnet [ICollection<T>](xref:System.Collections.Generic.ICollection`1), Hilfsklasse [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/).

-   [java.util.List<E> ](https://developer.android.com/reference/java/util/List.html) Systemtyp ordnet [IList<T>](xref:System.Collections.Generic.IList`1), Hilfsklasse [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/).

-   [java.util.Map < K, V >](https://developer.android.com/reference/java/util/Map.html) Systemtyp ordnet [IDictionary < TKey, TValue >](xref:System.Collections.Generic.IDictionary`2), Hilfsklasse [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/).

-   [java.util.Collection<E> ](https://developer.android.com/reference/java/util/Collection.html) Systemtyp ordnet [ICollection<T>](xref:System.Collections.Generic.ICollection`1), Hilfsklasse [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/).

Wir haben Hilfsklassen, um schneller copyless Marshallen dieser Typen zu ermöglichen, bereitgestellt. Wenn möglich, es wird empfohlen, diese Auflistungen anstelle der bereitgestellte Framework-Implementierung bereitgestellt, wie [ `List<T>` ](xref:System.Collections.Generic.List`1) oder [ `Dictionary<TKey, TValue>` ](xref:System.Collections.Generic.Dictionary`2). Die [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) Implementierungen eine systemeigene Java-Auflistung intern zu nutzen und benötigen daher keine kopieren in und aus eine systemeigene-Auflistung, bei der Übergabe an ein Android-API-Element.

Können Sie Implementierung der Schnittstelle an eine Android-Methode akzeptiert diese Schnittstelle übergeben, übergeben Sie z. B. eine `List<int>` auf die [ArrayAdapter&lt;Int&gt;(Kontext "," Int "," IList&lt;Int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)Konstruktor. *Jedoch*, für alle Implementierungen *außer* für die Android.Runtime-Implementierungen dazu *kopieren* der Liste, aus der Mono-VM in der Android-Runtime-VM. Wenn die Liste weiter unten geändert wird, in der Android-Laufzeit (z. B. durch Aufrufen der [ArrayAdapter&lt;T&gt;. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) Methode), diese Änderungen *nicht* in verwaltetem Code angezeigt werden. Wenn eine `JavaList<int>` wurden verwendet, die Änderungen würden angezeigt werden.

Umformuliert, Sammlungen schnittstellenimplementierungen, die *nicht* eine der oben aufgeführten **Hilfsklasse**es nur zu marshallen, [In]:

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

Java-Methoden werden in Eigenschaften, die bei Bedarf transformiert:

-  Das Java-Methodenpaar `T getFoo()` und `void setFoo(T)` werden in transformiert die `Foo` Eigenschaft. Beispiel: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/).

-  Die Java-Methode `getFoo()` wird umgewandelt in die schreibgeschützte Eigenschaft "Foo". Beispiel: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  Lesegeschützte Eigenschaften werden nicht generiert.

-  Eigenschaften sind *nicht* generiert, wenn der Eigenschaftentyp ein Array ist.



### <a name="events-and-listeners"></a>Ereignisse und -Listener

Die Android-APIs basieren auf Java und seine Komponenten führen Sie das Java-Muster für das Einbinden von Ereignislistener. Dieses Muster wird zumeist auf das umständlich sein, da den Benutzer eine anonyme Klasse erstellen, und deklarieren Sie die Methoden zum Überschreiben erforderlich ist, sieht z. B., wie Dinge in Android mit Java ausgeführt werden sollen:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

Der entsprechende Code in c# mithilfe von Ereignissen wäre:

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Beachten Sie, dass sowohl der oben genannten Mechanismen, die mit Xamarin.Android verfügbar sind. Sie können eine listenerschnittstelle implementieren, und fügen Sie es mit View.SetOnClickListener, oder Sie können einen Delegaten erstellt, die über eine der üblichen c#-Paradigmen für das Click-Ereignis anfügen.

Wenn die Listener-Callback-Methode eine void-Rückgabe verfügt, erstellen wir API-Elemente, die auf der Grundlage einer [EventHandler&lt;TEventArgs&gt; ](xref:System.EventHandler`1) delegieren. Generiert ein Ereignis wie im obigen Beispiel für diesen Listenertypen. Jedoch, wenn die Listener ein nicht "void" und nicht der Rückruf- **booleschen** Wert "," Ereignisse "und" EventHandlers werden nicht verwendet. Wir stattdessen einen bestimmten Delegaten für die Signatur des Rückrufs zu generieren und Hinzufügen von Eigenschaften anstelle von Ereignissen. Der Grund ist mit Delegaten Aufrufreihenfolge und Verarbeitung zurückzugeben. Dieser Ansatz spiegelt wider, was mit Xamarin.iOS-API erfolgt.

C#-Ereignissen oder Eigenschaften werden nur automatisch generiert, wenn die Registrierung von Android-Ereignisses-Methode:

1. Verfügt über eine `set` Präfix, z. B. [ *festgelegt*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/).

1. Verfügt über eine `void` Typ zurückgeben.

1. Akzeptiert nur einen Parameter, der Parametertyp ist eine Schnittstelle, die Schnittstelle verfügt über nur eine Methode und der Schnittstellenname endet auf `Listener` , z. B. [View.OnClick *Listener*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/).


Darüber hinaus, wenn der Listener Methode Schnittstelle hat den Rückgabetyp **booleschen** anstelle von **"void"**, klicken Sie dann die generierte *EventArgs* Unterklasse enthält eine *Behandelt* Eigenschaft. Der Wert des der *behandelt* Eigenschaft wird verwendet, als der Rückgabewert für die *Listener* -Methode, und es standardmäßig `true`.

Zum Beispiel der Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) -Methode akzeptiert die [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) -Schnittstelle, und die [View.OnKeyListener.onKey ("View", "Int", "KeyEvent")](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) Methode hat einen Rückgabetyp boolean. Xamarin.Android generiert einen entsprechenden [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) -Ereignis, das ist ein [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/).
Die *KeyEventArgs* Klasse wiederum verfügt über eine [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) -Eigenschaft, die verwendet wird, als der Rückgabewert für die *View.OnKeyListener.onKey()* Methode.

Wir beabsichtigen, Hinzufügen von Überladungen für andere Methoden und Ctors, die Delegate-basierten Verbindung verfügbar zu machen. Listener mit mehreren Rückrufen erfordern auch einige zusätzliche Überprüfung, um festzustellen, ob die Implementierung der einzelnen Rückrufe sinnvoll ist, ist, damit wir diese konvertiert werden, sobald sie identifiziert werden. Ist kein entsprechendes Ereignis, Listener in c# verwendet werden müssen, aber Sie bringen, die Sie denken möglicherweise Delegaten Nutzung unsere Aufmerksamkeit. Wir haben die bei einigen Konvertierungen von Schnittstellen, ohne das Suffix "Listener" auch ausgeführt, wenn es wurde deutlich, dass sie über eine Alternative Delegaten profitieren würden.

Alle Listener Schnittstellen implementieren die [`Android.Runtime.IJavaObject`](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)
-Schnittstelle, da die Implementierungsdetails der Bindung, damit die Listenerklassen müssen diese Schnittstelle implementieren. Dies ist möglich durch die Listener-Schnittstelle implementieren, auf eine Unterklasse von [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) oder jede andere Java-Objekt, z. B. eine Android-Aktivität umschlossen.


### <a name="runnables"></a>Runnables

Java nutzt die [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) Schnittstelle, um einen delegierungsmechanismus bereitzustellen. Die [java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) Klasse ist eine wichtige Consumer dieser Schnittstelle. Android hat die Schnittstelle in der API auch eingesetzt werden.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) und [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) werden relevante Beispiele.

Die `Runnable` Schnittstelle enthält nur die "void" Methode [run()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/). Er aus diesem Grund eignet sich für Datenbindung in c# als eine [System.Action](xref:System.Action) delegieren. Bereitgestellte Überladungen in der Bindung, die akzeptieren ein `Action` Parameter für alle API-Elemente die nutzen eine `Runnable` in der systemeigenen API, z. B. [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) und [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

Wir bleiben die [IRunnable](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) Überladungen vorhanden, statt diese zu ersetzen, da mehrere Typen die Schnittstelle implementieren und daher können als Runnables direkt übergeben werden.


### <a name="inner-classes"></a>Interne Klassen

Java weist zwei verschiedene Arten von [geschachtelte Klassen](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): statischen geschachtelten Klassen und nicht statische Klassen.

Java statische geschachtelte Klassen sind identisch mit der C#-geschachtelten Typen.

Nicht statische geschachtelte Klassen, die so genannte *innere Klassen*, unterscheiden sich grundlegend. Sie enthalten einen impliziten Verweis auf eine Instanz des einschließenden Typ und nicht statische Elemente (von andere Unterschiede würde den Rahmen dieser Übersicht) enthalten.

Bei Bindungs- und c# verwenden, werden die statische geschachtelte Klassen als normale geschachtelte Typen behandelt. Interne Klassen ist in der Zwischenzeit zwei wichtige Unterschiede:

1. Die implizite Verweis auf den enthaltenden Typ muss explizit als Konstruktorparameter bereitgestellt werden.

1. Beim Erben von einer inneren Klasse, die interne Klasse *müssen* in einem Typ geschachtelt werden, die von der enthaltende Typ der inneren Basisklasse erbt, und der abgeleitete Typ muss einen Konstruktor, der den gleichen Typ wie der C#-angeben, die Typ enthält.


Betrachten Sie beispielsweise die [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) inneren Klasse. Da es sich um eine interne Klasse, ist die [WallpaperService.Engine() Konstruktor](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) akzeptiert einen Verweis auf eine [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) Instanz (verglichen und gegenübergestellt, die Java [WallpaperService.Engine ( ) Konstruktor](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) der keine Parameter akzeptiert).

Eine Beispiel-Ableitung von einer inneren Klasse handelt es sich um CubeWallpaper.CubeEngine:

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

Hinweis wie `CubeWallpaper.CubeEngine` geschachtelt ist `CubeWallpaper`, `CubeWallpaper` erbt von der enthaltenden Klasse der `WallpaperService.Engine`, und `CubeWallpaper.CubeEngine` verfügt über einen Konstruktor, den deklarierenden Typ – dadurch `CubeWallpaper` in diesem Fall – in dem alle als oben angegeben.


### <a name="interfaces"></a>Schnittstellen

Java-Schnittstellen können drei Gruppen von Elementen, enthalten, von denen zwei in c# zu Problemen führen:

1. Methoden

1. Typen

1. Felder


Java-Schnittstellen sind in zwei Typen übersetzt:

1. Eine (optional)-Schnittstelle, die die Deklarationen enthält. Diese Schnittstelle hat den gleichen Namen wie die Java-Schnittstelle, *außer* verfügt auch über eine " *ich* " Präfix.

1. Eine (optionale) statische Klasse, die alle Felder enthält, die in der Java-Schnittstelle deklariert werden.

Geschachtelte Typen werden "verschoben" als gleichgeordnete Elemente der einschließenden-Schnittstelle anstelle von geschachtelten Typen, mit den Schnittstellennamen der einschließenden als Präfix.

Betrachten Sie beispielsweise die [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) Schnittstelle.
Die *Parcelable* Schnittstelle enthält Methoden, geschachtelte Typen und Konstanten. Die *Parcelable* Schnittstellenmethoden befinden sich in der [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) Schnittstelle.
Die *Parcelable* Schnittstelle Konstanten befinden sich in der [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) Typ. Die geschachtelte [android.os.Parcelable.ClassLoaderCreator <t> </t> ](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) und [android.os.Parcelable.Creator <t> </t> ](https://developer.android.com/reference/android/os/Parcelable.Creator.html) Typen sind derzeit nicht aufgrund der Einschränkungen für unsere Unterstützung von Generika; Wenn sie unterstützt wurden, werden als die *Android.OS.IParcelableClassLoaderCreator* und *Android.OS.IParcelableCreator* Schnittstellen. Z. B. den geschachtelten [android.os.IBinder.DeathRecpient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) Schnittstelle gebunden ist, als die [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) Schnittstelle.


> [!NOTE]
> Ab Xamarin.Android 1.9 können Java-Schnittstelle Konstanten sind <em>dupliziert</em> code zu Portieren von Java zu vereinfachen. Portieren von Java-Code zu verbessern, die verwendet werden dadurch [android Anbieter](https://developer.android.com/reference/android/provider/package-summary.html) Schnittstelle Konstanten.

Zusätzlich zu den oben genannten Typen gibt es vier weitere Änderungen:

1. Ein Typ mit dem gleichen Namen wie die Java-Schnittstelle wird generiert, auf die Konstanten enthalten.

1. Typen, die auch mit der Konstanten aus Schnittstelle enthalten alle Konstanten, die aus implementierten Schnittstellen von Java stammen.

1. Alle Klassen, die implementieren eine Java-Schnittstelle, die mit Konstanten aus, erhalten einen neuen geschachtelten InterfaceConsts-Typ der Konstanten aus allen implementierten Schnittstellen enthält.

1. Die *Kosten* Typ ist jetzt veraltet.


Für die *android.os.Parcelable* -Schnittstelle, dies bedeutet, dass es wird nun ein [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) Typ die Konstanten enthalten. Z. B. die [Parcelable.CONTENTS_FILE_DESCRIPTOR](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) Konstante gebunden werden wird, als die [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) Konstanten, statt als die  *ParcelableConsts.ContentsFileDescriptor* Konstanten.

Für Schnittstellen, die Konstanten, die mit anderen Schnittstellen noch weitere Konstanten enthält, wird die Union aller Konstanten generiert. Z. B. die [android.provider.MediaStore.Video.VideoColumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) -Schnittstelle implementiert die [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) Schnittstelle. Allerdings vor 1.9 die [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) Typ hat keine Möglichkeit für den Zugriff auf die Konstanten, die auf [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/).
Als Ergebnis der Java-Ausdruck *MediaStore.Video.VideoColumns.TITLE* muss auf den C#-Ausdruck gebunden werden *MediaStore.Video.MediaColumnsConsts.Title* dies nur schwer ermitteln ohne lesen Viele der Java-Dokumentation. In 1.9, werden der entsprechende C#-Ausdruck [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/).

Darüber hinaus sollten Sie die [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Typ, der implementiert das Java *Parcelable* Schnittstelle. Da die Schnittstelle implementiert, alle Konstanten, die auf dieser Schnittstelle können Sie "über" der Bundle-Typ, z. B. *Bundle.CONTENTS_FILE_DESCRIPTOR* ist ein absolut gültiger Java-Ausdruck.
Zuvor So portieren Sie diesen Ausdruck als C# müssten Sie alle Schnittstellen betrachten, die implementiert werden, aus welchen Typ die *CONTENTS_FILE_DESCRIPTOR* stammt. Ab Xamarin.Android 1.9 können Klassen implementieren die Java-Schnittstellen, die Konstanten enthalten, verfügen über eine geschachtelte *InterfaceConsts* Typ, der alle Konstanten geerbte Schnittstelle enthalten soll. Dadurch wird übersetzen *Bundle.CONTENTS_FILE_DESCRIPTOR* zu [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/).

Abschließend Typen mit einem *Kosten* suffix wie z. B. *Android.OS.ParcelableConsts* sind jetzt veraltet, außer den neu eingeführten InterfaceConsts Typen geschachtelt. Sie werden in Xamarin.Android 3.0 entfernt werden.


## <a name="resources"></a>Ressourcen

Bilder, Layout, Beschreibungen, binäre Blobs und zeichenfolgenwörterbücher in Ihrer Anwendung enthalten sein können [Ressourcendateien](https://developer.android.com/guide/topics/resources/providing-resources.html).
Verschiedene Android-APIs dienen zur [ausgeführt werden, auf die Ressourcen-IDs](https://developer.android.com/guide/topics/resources/accessing-resources.html) anstelle der Umgang mit Bildern, Zeichenfolgen oder binäre blobs direkt.

Z. B. eine Android-beispielanwendung, die ein Layout für die Benutzeroberfläche enthält ( `main.axml`), eine Zeichenfolge der Internationalisierung-Tabelle ( `strings.xml`) und einige Symbole ( `drawable-*/icon.png`) behalten ihre Ressourcen im Verzeichnis "Resources" der Anwendung:

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

Die Native Android-APIs funktionieren nicht direkt mit dem Dateinamen, aber er stattdessen auf die Ressourcen-IDs ausgeführt werden. Beim Kompilieren einer Android-Anwendung, die Ressourcen verwendet, das Buildsystem wird die Ressourcen für die Verteilung Verpacken, und generiert eine Klasse namens `Resource` , der die Token für jede der enthaltenen Ressourcen enthält. Beispielsweise ist für die oben genannten Ressourcen Layout dies was die R-Klasse verfügbar gemacht wird:

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

Die Native Android-APIs haben die viele Methoden, die übernehmen oder eine ganze Zahl, die zugeordnet werden muss, um ein konstantes Feld, um zu bestimmen, was bedeutet, dass der int-Wert zurückgeben. Um diese Methoden zu verwenden, muss der Benutzer, die in der Dokumentation, um festzustellen, welche Konstanten entsprechenden Werte ein, sind die andere als ideal ist.

Betrachten Sie z. B. [Activity.requestWindowFeature (Int-FeatureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int)).

In diesen Fällen sich bemühen wir zum Gruppieren verwandter Konstanten in einer .NET-Enumeration, und ordnen die Methode, um die Enumeration stattdessen zu nutzen.
Auf diese Weise können wir IntelliSense-Auswahl mit möglichen Werten bieten.

Im obige Beispiel wird: [Activity.RequestWindowFeature(WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/).

Beachten Sie, dass dies ein sehr manueller Prozess, um zu ermitteln, welche Konstanten zusammengehören, und die APIs, diese Konstanten nutzen. Melden Sie Fehler für alle Konstanten, die in der API, die besser ausgedrückt als Enumeration verwendet.
