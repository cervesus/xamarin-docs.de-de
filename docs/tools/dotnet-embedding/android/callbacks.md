---
title: Rückrufe für Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 1b2584d1e76c2f4ed0fb0a924beda3fd2554a57d
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="callbacks-on-android"></a>Rückrufe für Android

Aufrufen in Java in c# ist in einem gewissen eine *riskant*. Das heißt besteht eine *Muster* für Rückrufe in c#, Java; es ist jedoch komplizierter als wir möchten.

Wir behandeln die drei Optionen für die auf diese Weise Rückrufe, die sinnvollsten für Java:

- Abstrakte Klassen
- Schnittstellen
- Virtuelle Methoden

## <a name="abstract-classes"></a>Abstrakte Klassen

Dies ist der einfachste Weg für Rückrufe, damit würde sollten mit `abstract` , wenn Sie nur einen Rückruf, der in der einfachsten Form arbeiten abrufen möchten.

Beginnen wir mit einer c#-Klasse, die es Java implementieren möchten:

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

Hier werden die Details zu dieser funktioniert:

- `[Register]` generiert einen nice Paketnamen in Java – erhalten Sie eine automatisch generierte Paketname ohne.
- Erstellen von Unterklassen `Java.Lang.Object` signalisiert, führen Sie die Klasse über Xamarin.Android des Java-Generator .NET einbetten.
- Leeren Konstruktor: ist, was Sie von Java-Code verwenden möchten.
- `(IntPtr, JniHandleOwnership)` Konstruktor: wird von Xamarin.Android verwendet wird, zum Erstellen der c#-Äquivalent von Java-Objekten.
- `[Export]` signalisiert Xamarin.Android, um die Methode, um Java verfügbar zu machen. Wir können auch den Methodennamen ändern, da die Java-Welt gerne Kleinbuchstaben Methoden verwenden.

Nächste stellen eine C#-Methode zum Testen des Szenarios:

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
`JavaCallbacks` kann jede Klasse, um dies zu testen, solange es ist ein `Java.Lang.Object`.

Führen Sie nun .NET einbetten für Ihr .NET Assembly um eine AAR zu generieren. Finden Sie unter der [Getting Started Guide](~/tools/dotnet-embedding/get-started/java/android.md) Details.

Nach dem Importieren der Datei AAR in Android Studio, einen Komponententest schreiben:

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
- Sicherstellen, dass unsere Instanz zurückgibt `"Java"` in c#
- Hinzugefügt `throws Throwable`, da C#-Konstruktoren mit derzeit markiert sind `throws`

Wenn es war dieser Komponententest als-ist es mit einem Fehler wie z. B. fehl:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Was fehlt hier ist ein `Invoker` Typ. Dies ist eine Unterklasse von `AbstractClass` c# Aufrufe Java weiterleitet. Wenn ein Java-Objekt geht der C#-Welt in der entsprechende c#-Typ abstrakt ist, und Xamarin.Android automatisch nach einem C#-Typ mit dem Suffix sucht `Invoker` zur Verwendung in C#-Code.

Xamarin.Android verwendet diese `Invoker` Muster für Java Bindung Projekte unter anderem.

Hier wird die Implementierung von `AbstractClassInvoker`:
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

Es ist recht einfach, wir:

- Eine Klasse mit dem Suffix hinzugefügt `Invoker` , Unterklassen `AbstractClass`
- Hinzugefügt `class_ref` zum Speichern der JNI verweisen auf die Java-Klasse, Unterklassen die C#-Klasse
- Hinzugefügt `id_gettext` mit dem JNI Verweis auf die Java `getText` Methode
- Enthalten eine `(IntPtr, JniHandleOwnership)` Konstruktor
- Implementiert `ThresholdType` und `ThresholdClass` als Voraussetzung für die Xamarin.Android Details kennen die `Invoker`
- `GetText` benötigt zum Suchen der Java `getText` Methode mit der entsprechenden JNI-Signatur und Aufruf
- `Dispose` ist nur erforderlich, löschen Sie den Verweis auf `class_ref`

Nach dem Hinzufügen von dieser Klasse, und generieren eine neue AAR, testen Sie unser Einheit übergibt. Wie Sie sehen können, ist dieses Muster für Rückrufe *ideal*, aber dies realisieren.

Ausführliche Informationen zum Java-Interop finden Sie unter der erstaunlich [Xamarin.Android Dokumentation](~/android/platform/java-integration/working-with-jni.md) zu diesem Thema.

## <a name="interfaces"></a>Schnittstellen

Schnittstellen sind ähnlich wie abstrakte Klassen enthalten, mit Ausnahme von ein Detail: Xamarin.Android generiert Java nicht für sie. Dies ist vor dem Einbetten von .NET sind nicht viele Szenarien, in denen Java eine C#-Schnittstelle implementiert werden würde.

Angenommen, wir die folgende C#-Schnittstelle haben:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` signalisiert .NET einbetten, dass dies eine Xamarin.Android-Schnittstelle ist, aber ansonsten dies exakt der als entspricht ein `abstract` Klasse.

Weil Xamarin.Android derzeit keinen den Java-Code für diese Schnittstelle generiert wird, fügen Sie der folgenden Java dem C#-Projekt hinzu:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

Die Datei an einer beliebigen Stelle platzieren, aber stellen Sie sicher, dass seine Buildaktion auf `AndroidJavaSource`. Dies wird als .NET Einbetten in das richtige Verzeichnis in Ihre AAR-Datei kompiliert abrufen kopieren signalisieren.

Als Nächstes wird die `Invoker` Implementierung wird ganz identisch sein:

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

Testen Sie nach dem Generieren einer AAR-Datei, in Android Studio wir die folgenden übergeben Einheit schreiben können:

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

Überschreiben einer `virtual` in Java ist möglich, aber nicht für eine optimale Erfahrung.

Nehmen wir an, dass Sie die folgende C#-Klasse verfügen:

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

Verwenden Sie und die `abstract` Klasse Beispiel oben würde es außer ein Detail funktionieren: _Xamarin.Android suchen wird nicht die `Invoker`_ .

Um dieses Problem zu beheben, ändern Sie die C#-Klasse, um werden `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Dies ist nicht ideal, aber es ruft Szenario funktioniert. Xamarin.Android übernimmt die `VirtualClassInvoker` und Java können `@Override` der Methode.

## <a name="callbacks-in-the-future"></a>Rückrufe in der Zukunft

Es gibt einige Dinge wo zum Verbessern dieser Szenarien möglich:

1. `throws Throwable` C#-Konstruktoren wird als fester Port festgelegt auf diesem [PR](https://github.com/xamarin/java.interop/pull/170).
1. Stellen Sie die Java-Generator in Xamarin.Android Schnittstellen unterstützen.
    - Dies beseitigt die Notwendigkeit der für das Hinzufügen von Java-Quelldatei mit dem Buildvorgang `AndroidJavaSource`.
1. Stellen Sie eine Möglichkeit, Xamarin.Android Laden ein `Invoker` für virtuellen Klassen.
    - Dies beseitigt die Notwendigkeit markieren Sie die Klasse in unserer `virtual` Beispiel `abstract`.
1. Generieren von `Invoker` automatisch für .NET Einbetten von Klassen
    - Dies ist kompliziert, aber dies realisieren werden soll. Xamarin.Android ist bereits in etwa wie folgt für Java Bindung Projekte machen.

Es wird ein Großteil der Arbeit, die hier auszuführen, aber diese Erweiterungen für .NET einbetten sind möglich.

## <a name="further-reading"></a>Weiterführende Themen

* [Erste Schritte für Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Vorläufige Android Research](~/tools/dotnet-embedding/android/index.md)
* [.NET Einbetten von Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
