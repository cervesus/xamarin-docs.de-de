---
title: Rückrufe unter Android
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2d1d5b8985d132e5a5839e3cd23aaec32fc3815a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725113"
---
# <a name="callbacks-on-android"></a>Rückrufe unter Android

Das Aufrufen von Java C# aus ist ein wenig *riskantes Unternehmen*. Das heißt, es gibt ein *Muster* für Rückrufe von C# nach Java. Es ist jedoch komplizierter, als wir möchten.

Wir behandeln die drei Optionen für Rückrufe, die für Java den sinnvollsten machen:

- Abstrakte Klassen
- Schnittstellen
- Virtuelle Methoden

## <a name="abstract-classes"></a>Abstrakte Klassen

Dies ist die einfachste Route für Rückrufe. Daher empfiehlt es sich, `abstract` zu verwenden, wenn Sie nur versuchen, einen Rückruf in der einfachsten Form zu verwenden.

Wir beginnen mit einer C# Klasse, die Java implementieren soll:

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

Dies sind die folgenden Details:

- `[Register]` generiert einen netten Paketnamen in Java. Sie erhalten einen automatisch generierten Paketnamen ohne diesen.
- Die Unterklassen `Java.Lang.Object` signalisiert dem .net-Einbettungs Vorgang, die Klasse über den Java-Generator von xamarin. Android auszuführen.
- Leerer Konstruktor: ist das, was Sie aus Java-Code verwenden möchten.
- `(IntPtr, JniHandleOwnership)`-Konstruktor: ist das, was xamarin. Android zum Erstellen C#der-Entsprechung von Java-Objekten verwendet.
- `[Export]` signalisiert xamarin. Android, die Methode für Java verfügbar zu machen. Wir können auch den Methodennamen ändern, da die Java-Welt eine Methode für die Verwendung von Kleinbuchstaben verwendet.

Als Nächstes erstellen wir eine C# Methode zum Testen des Szenarios:

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

`JavaCallbacks` kann eine beliebige Klasse sein, um dies zu testen, solange es sich um eine `Java.Lang.Object`handelt.

Führen Sie nun .net-Einbettungen in der .NET-Assembly aus, um ein Aar zu generieren. Weitere Informationen finden Sie im [Leitfaden zu](~/tools/dotnet-embedding/get-started/java/android.md) den ersten Schritten.

Nachdem Sie die Aar-Datei in Android Studio importiert haben, können Sie einen Komponenten Test schreiben:

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

Wir haben also Folgendes:

- Der `AbstractClass` wurde in Java mit einem anonymen Typ implementiert.
- Sicherstellen, dass unsere Instanz `"Java"` aus Java zurückgibt
- Es wurde sichergestellt, dass unsere Instanz `"Java"` aus zurückgibtC#
- `throws Throwable`hinzugefügt, C# da Konstruktoren derzeit mit markiert sind `throws`

Wenn dieser Komponenten Test unverändert ausgeführt wird, tritt ein Fehler auf, wie z. b.:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Hier fehlt ein `Invoker` Typ. Dies ist eine Unterklasse von `AbstractClass`, die C# Aufrufe an Java weiterleitet. Wenn ein Java-Objekt in C# die Welt gelangt und C# der äquivalente Typ abstrakt ist, sucht xamarin. Android automatisch C# nach einem Typ mit dem Suffix `Invoker` zur C# Verwendung im Code.

Xamarin. Android verwendet dieses `Invoker` Muster für Java-Bindungs Projekte unter anderem.

Im folgenden finden Sie eine `AbstractClassInvoker`Implementierung:

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

Hier geht es um Folgendes:

- Es wurde eine Klasse mit dem Suffix hinzugefügt, `Invoker` diese Unterklassen `AbstractClass`
- Es wurden `class_ref` hinzugefügt, die den jni-Verweis auf die Java- C# Klasse enthalten, die die Klasse Unterklassen
- Hinzugefügter `id_gettext` zum Speichern des jni-Verweises auf die Java `getText`-Methode
- Enthielt einen `(IntPtr, JniHandleOwnership)`-Konstruktor.
- Implementiert `ThresholdType` und `ThresholdClass` als Voraussetzung für xamarin. Android, um Details zum `Invoker`
- `GetText` erforderlich, um die Java `getText`-Methode mit der entsprechenden jni-Signatur zu suchen und Sie aufzurufen.
- `Dispose` ist nur erforderlich, um den Verweis auf zu löschen `class_ref`

Nachdem Sie diese Klasse hinzugefügt und ein neues Aar erstellt haben, wird der Komponenten Test durchlaufen. Wie Sie sehen können, ist dieses Muster für Rückrufe nicht *ideal*, aber doable.

Weitere Informationen zu Java-Interop finden Sie in der beeindruckenden [xamarin. Android-Dokumentation](~/android/platform/java-integration/working-with-jni.md) zu diesem Thema.

## <a name="interfaces"></a>Schnittstellen

Schnittstellen sind ähnlich wie abstrakte Klassen, mit Ausnahme eines Details: xamarin. Android generiert Java nicht für Sie. Dies liegt daran, dass vor dem Einbetten von .net nicht viele Szenarien vorhanden sind, C# in denen Java eine-Schnittstelle implementieren würde.

Nehmen wir an, wir haben die C# folgende Schnittstelle:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```

`IJavaObject` signalisiert dem Einbetten von .net, dass es sich hierbei um eine xamarin. Android-Schnittstelle handelt, aber andernfalls identisch mit einer `abstract` Klasse.

Da xamarin. Android derzeit nicht den Java-Code für diese Schnittstelle generiert, fügen Sie dem C# Projekt den folgenden Java-Code hinzu:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```

Sie können die Datei an einem beliebigen Ort platzieren, aber stellen Sie sicher, dass Sie die Buildaktion auf `AndroidJavaSource`festlegen. Dadurch wird die .net-Einbettung signalisiert, Sie in das richtige Verzeichnis zu kopieren, um Sie in Ihre Aar-Datei zu kompilieren.

Im nächsten Schritt wird die `Invoker`-Implementierung ganz identisch sein:

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

Nachdem Sie eine Aar-Datei erstellt haben, können Sie in Android Studio den folgenden bestandenen Komponenten Test schreiben:

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

Das Überschreiben einer `virtual` in Java ist möglich, aber nicht besonders gut.

Angenommen, Sie haben die folgende C# Klasse:

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

Wenn Sie das obige Beispiel der `abstract`-Klasse befolgt haben, funktioniert es mit Ausnahme eines einzelnen Details: _xamarin. Android sucht nicht nach dem `Invoker`_ .

Um dies zu beheben, ändern C# Sie die Klasse so, dass Sie `abstract`wird:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Dies ist nicht ideal, aber dieses Szenario funktioniert. Xamarin. Android übernimmt die `VirtualClassInvoker`, und Java kann `@Override` auf der-Methode verwenden.

## <a name="callbacks-in-the-future"></a>Rückrufe in der Zukunft

Es gibt einige Möglichkeiten, diese Szenarios zu verbessern:

1. `throws Throwable` für C# Konstruktoren ist für diesen [PR](https://github.com/xamarin/java.interop/pull/170)korrigiert.
1. Erstellen Sie den Java-Generator in xamarin. Android-Unterstützungs Schnittstellen.
    - Dadurch entfällt die Notwendigkeit, eine Java-Quelldatei mit einer Buildaktion von `AndroidJavaSource`hinzuzufügen.
1. Erstellen Sie eine Methode für xamarin. Android, um eine `Invoker` für virtuelle Klassen zu laden.
    - Dadurch entfällt die Notwendigkeit, die Klasse in unserem `virtual` Beispiel `abstract`zu markieren.
1. Generieren von `Invoker` Klassen für .net-Einbettungen automatisch
    - Dies ist kompliziert, aber doable. Xamarin. Android verhält sich bereits ähnlich wie bei Java-Bindungs Projekten.

Hier sind viele Aufgaben zu erledigen, aber diese Verbesserungen an der .net-Einbettung sind möglich.

## <a name="further-reading"></a>Weiterführende Themen

- [Einstieg in Android](~/tools/dotnet-embedding/get-started/java/android.md)
- [Vorläufige Android-Recherche](~/tools/dotnet-embedding/android/index.md)
- [.Net-Einbettungs Einschränkungen](~/tools/dotnet-embedding/limitations.md)
- [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
