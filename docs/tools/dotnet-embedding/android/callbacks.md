---
title: Rückrufe, die unter Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: c6eaf4dd90b172053b4b87e3427cfe35213c6727
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215877"
---
# <a name="callbacks-on-android"></a>Rückrufe, die unter Android

Aufrufen von Java aus C# ein wenig einer *riskant*. D.h. es wird eine *Muster* für Rückrufe vom C# auf Java; es ist jedoch komplizierter, als wir möchten.

Ich werde drei Optionen für die Rückrufe, die die am sinnvollsten für Java:

- Abstrakte Klassen
- Schnittstellen
- Virtuelle Methoden

## <a name="abstract-classes"></a>Abstrakte Klassen

Dies ist der einfachste Weg für Rückrufe, mit sollten sich daher für würden `abstract` , wenn Sie nur einen Rückruf, der bei der einfachsten Form arbeiten erhalten möchten.

Beginnen wir mit einem C# Klasse, die wir von Java zum implementieren möchten:

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

Hier sind die Details, damit dies funktioniert:

- `[Register]` generiert einen gut Paketnamen in Java – erhalten Sie ein automatisch generierter Paketname ohne.
- Erstellen von Unterklassen für `Java.Lang.Object` Signale zum Einbetten von .NET die Klasse über die Xamarin.Android-Java-Generators ausführen.
- Leerer Konstruktor: gewünscht wird für die Verwendung von Java-Code.
- `(IntPtr, JniHandleOwnership)` Konstruktor: wird zum Erstellen von Xamarin.Android verwendet, die C#-Äquivalent der Java-Objekten.
- `[Export]` signalisiert, Xamarin.Android, um die Methode, die Java verfügbar zu machen. Wir können auch den Methodennamen angeben, ändern, da die Java-Welt gut Kleinbuchstaben Methoden verwendet.

Nächste lassen Sie uns eine C# Methode zum Testen des Szenarios:

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` möglicherweise jede Klasse, um dies zu testen, solange es ist ein `Java.Lang.Object`.

Führen Sie nun Einbetten von .NET für Ihre .NET Assembly einer AAR zu generieren. Finden Sie unter den [Handbuch Erste Schritte](~/tools/dotnet-embedding/get-started/java/android.md) Details.

Nach dem Importieren der zusätzlich zum AAR-Datei, in Android Studio, erstelle ich einen Komponententest:

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
Daher wir:

- Implementiert die `AbstractClass` in Java mit einem anonymen Typ
- Sicherstellen, dass unsere Instanz zurückgibt `"Java"` aus Java
- Sicherstellen, dass unsere Instanz zurückgibt `"Java"` ausC#
- Hinzugefügt `throws Throwable`, da C# Konstruktoren mit derzeit markiert sind `throws`

Wenn wir führten diese Komponententest als-ist, es zu einem Fehler ein Fehler wie z. B.:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Was fehlt so sieht ein `Invoker` Typ. Dies ist eine Unterklasse von `AbstractClass` weiterleitet C# Aufrufen von Java. Wenn Sie eine Java-Objekt gibt die C# Welt und die entsprechende C# Typ ist abstrakt, und klicken Sie dann Xamarin.Android sucht automatisch nach einer C# Typ mit dem Suffix `Invoker` für die Verwendung in C# Code.

Xamarin.Android verwendet diese `Invoker` Muster für die Java-bindungsprojekte unter anderem.

Dies ist unsere Implementierung des `AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Es ist einiges passiert hier können wir:

- Eine Klasse mit dem Suffix hinzugefügt `Invoker` , Unterklassen `AbstractClass`
- Hinzugefügt `class_ref` , die die JNI-verweisen auf die Java-Klasse, Unterklassen enthalten soll unser C# Klasse
- Hinzugefügt `id_gettext` zum Speichern der JNI-verweisen auf die Java `getText` Methode
- Enthalten eine `(IntPtr, JniHandleOwnership)` Konstruktor
- Implementiert `ThresholdType` und `ThresholdClass` als Voraussetzung für Xamarin.Android Details kennen die `Invoker`
- `GetText` benötigt zum Suchen des Java `getText` Methode mit der entsprechenden JNI-Signatur und nennen Sie sie
- `Dispose` ist nur erforderlich, löschen Sie den Verweis auf `class_ref`

Nach dem Hinzufügen dieser Klasse, und generieren eine neue AAR, für das Komponententestprojekt unsere übergibt. Wie Sie sehen dieses Muster für Rückrufe ist nicht *ideal*, aber machbar.

Ausführliche Informationen zu Java-Interop, finden Sie unter den praktischen [Xamarin.Android Dokumentation](~/android/platform/java-integration/working-with-jni.md) zu diesem Thema.

## <a name="interfaces"></a>Schnittstellen

Schnittstellen sind ähnlich wie abstrakte Klassen, mit Ausnahme von ein Detail: Xamarin.Android wird Java nicht für sie generiert werden. Das liegt vor dem Einbetten von .NET sind nicht viele Szenarios, bei denen Java implementiert würde, eine C# Schnittstelle.

Angenommen, wir haben die folgenden C# Schnittstelle:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` signalisiert für das Einbetten von .NET, dass dies eine Xamarin.Android-Schnittstelle ist, aber andernfalls ist dies genau identisch mit einer `abstract` Klasse.

Da Xamarin.Android den Java-Code für diese Schnittstelle gegenwärtig nicht generiert werden, fügen Sie folgende Java zu Ihrem C# Projekt:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

Legen Sie die Datei an einer beliebigen Stelle, aber Achten Sie darauf, legen Sie seine Buildaktion auf `AndroidJavaSource`. Dies wird als Einbetten von .NET zum richtigen Verzeichnis, in die AAR-Datei kompiliert, die einen kopieren signalisieren.

Als Nächstes die `Invoker` Implementierung ist nicht ganz identisch sein:

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Nach dem Generieren einer AAR-Datei, in Android Studio, die wir die folgende übergeben Einheit schreiben zu testen:

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>Virtuelle Methoden

Überschreiben einer `virtual` in Java ist möglich, aber nicht mit einer überzeugenden Umgebung.

Angenommen, Sie verfügen über die folgenden C# Klasse:

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

Wenn Sie befolgt die `abstract` Klassenbeispiel oben würde es außer ein Detail funktionieren: _Xamarin.Android suchen wird nicht die `Invoker`_ .

Um dieses Problem zu beheben, ändern Sie die C# Klasse `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Dies ist nicht ideal, jedoch wird dieses Szenario funktioniert. Xamarin.Android übernimmt die `VirtualClassInvoker` und Java kann `@Override` für die Methode.

## <a name="callbacks-in-the-future"></a>Rückrufe in der Zukunft

Es gibt einige Dinge, die wir um diese Szenarien verbessern können:

1. `throws Throwable` auf C# Konstruktoren wurde behoben, dazu [PR](https://github.com/xamarin/java.interop/pull/170).
1. Stellen Sie den Java-Generator in Xamarin.Android-Schnittstellen unterstützen.
    - Damit entfällt die Notwendigkeit für das Hinzufügen von Java-Quelldatei mit einer Buildaktion von `AndroidJavaSource`.
1. Stellen Sie eine Möglichkeit für Xamarin.Android zum Laden einer `Invoker` für virtuelle Klassen.
    - Damit entfällt die Notwendigkeit, markieren Sie die Klasse in unserer `virtual` Beispiel `abstract`.
1. Generieren von `Invoker` automatisch für das Einbetten von .NET Klassen
    - Dies ist kompliziert, aber machbar sein soll. Xamarin.Android ist bereits in etwa wie folgt für Java-bindungsprojekte ausführen.

Es ist viel Arbeit hier erfolgen, aber diese Erweiterungen für das Einbetten von .NET sind möglich.

## <a name="further-reading"></a>Weiterführende Themen

* [Erste Schritte für Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Vorläufige Android Research](~/tools/dotnet-embedding/android/index.md)
* [Einbetten von .NET-Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirkung an open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
