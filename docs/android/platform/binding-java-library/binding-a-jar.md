---
title: Binden einer JAR-Datei
description: Diese exemplarische Vorgehensweise enthält Schritt-für-Schritt-Anleitungen zum Erstellen einer xamarin. Android-Java-Bindungs Bibliothek von einem Android. JAR-Datei.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 9a9e7e9c5d189527d4fbdcc2001d6f003fa63dd7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757873"
---
# <a name="binding-a-jar"></a>Binden einer JAR-Datei

_Diese exemplarische Vorgehensweise enthält Schritt-für-Schritt-Anleitungen zum Erstellen einer xamarin. Android-Java-Bindungs Bibliothek von einem Android. JAR-Datei._

## <a name="overview"></a>Übersicht

Die Android-Community bietet viele Java-Bibliotheken, die Sie möglicherweise in Ihrer APP verwenden möchten. Diese Java-Bibliotheken werden häufig in verpackt. JAR-Format (Java Archive), Sie können jedoch ein verpacken. JAR-Datei in einer *Java-Bindungs Bibliothek* , damit ihre Funktionalität für xamarin. Android-Apps verfügbar ist. Der Zweck der Bibliothek für Java-Bindungen besteht darin, die APIs in zu erstellen. JAR-Datei, C# die durch automatisch generierte Codewrapper für Code verfügbar ist.

Xamarin-Tools können eine Bindungs Bibliothek aus mindestens einer Eingabe generieren. JAR-Dateien. Die Bindungs Bibliothek (. DLL-Assembly) enthält Folgendes: 

- Der Inhalt der ursprünglichen. JAR-Datei (en).

- Von verwalteten Callable Wrapper (MCW), bei denen C# es sich um Typen handelt, die entsprechende Java-Typen in einschließen. JAR-Datei (en).

Der generierte MCW-Code verwendet jni (Native Java-Schnittstelle), um Ihre API-Aufrufe an den zugrunde liegenden weiterzuleiten. JAR-Datei. Sie können Bindungs Bibliotheken für beliebige erstellen. JAR-Datei, die ursprünglich für die Verwendung mit Android konzipiert war (Beachten Sie, dass xamarin-Tools die Bindung von nicht-Android-Java-Bibliotheken derzeit nicht unterstützen). Sie können auch festlegen, dass die Bindungs Bibliothek erstellt werden soll, ohne den Inhalt von einzubeziehen. JAR-Datei, sodass die DLL eine Abhängigkeit von aufweist. JAR zur Laufzeit.

In dieser Anleitung werden die Grundlagen der Erstellung einer Bindungs Bibliothek für einen einzelnen Schritt erläutert. JAR-Datei. Wir veranschaulichen mit einem Beispiel, bei dem alles richtig &ndash; ist, wenn keine Anpassung oder das Debuggen von Bindungen erforderlich ist. 
Das [Erstellen von Bindungen mithilfe von Metadaten](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bietet ein Beispiel für ein erweitertes Szenario, bei dem der Bindungsprozess nicht vollständig automatisch erfolgt und ein gewisser manueller Eingriff erforderlich ist. Eine Übersicht über die Java-Bibliotheks Bindung im allgemeinen (mit einem einfachen Codebeispiel) finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md). 

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In der folgenden exemplarischen Vorgehensweise erstellen wir eine Bindungs Bibliothek für [Picasso](http://square.github.io/picasso/), eine beliebte Android. JAR, das Funktionen zum Laden und Zwischenspeichern von Bildern bereitstellt. Wir verwenden die folgenden Schritte, um **Picasso-2. x. x. jar** zu binden, um eine neue .NET-Assembly zu erstellen, die wir in einem xamarin. Android-Projekt verwenden können: 

1. Erstellen Sie ein neues Java-Bindungs Bibliotheksprojekt.

2. Fügen Sie hinzu. JAR-Datei für das Projekt.

3. Legen Sie die entsprechende Buildaktion für das fest. JAR-Datei.

4. Wählen Sie das Ziel Framework aus. JAR unterstützt.

5. Erstellen Sie die Bindungs Bibliothek.

Nachdem wir die Bindungs Bibliothek erstellt haben, entwickeln wir eine kleine Android-App, die die Fähigkeit zum Abrufen von APIs in der Bindungs Bibliothek veranschaulicht. In diesem Beispiel möchten wir auf Methoden von **Picasso-2. x. x. jar**zugreifen:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}
```

Nachdem wir eine Bindungs Bibliothek für **Picasso-2. x. x. jar**generiert haben, können wir diese Methoden von C#abrufen. Beispiel:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>Erstellen der Bindungs Bibliothek

Bevor Sie mit den folgenden Schritten beginnen, müssen Sie [Picasso-2. x. x. jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)herunterladen.

Erstellen Sie zunächst ein neues Bindungs Bibliotheksprojekt. Erstellen Sie in Visual Studio für Mac oder Visual Studio eine neue Projekt Mappe, und wählen Sie die Vorlage *Android-Bindungs Bibliothek* aus. (Die Screenshots in dieser exemplarischen Vorgehensweise verwenden Visual Studio, aber Visual Studio für Mac ist sehr ähnlich.) Nennen Sie die Projekt Mappe **jarbinding**: 

[![Erstellen eines jarbinding-Bibliotheks Projekts](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

Die Vorlage **enthält einen jar** -Ordner, in dem Sie Ihre hinzufügen. JAR (s) zum Bindungs Bibliotheksprojekt. Klicken Sie mit der rechten Maustaste auf den Ordner **Jars** , und wählen Sie dann **> vorhandenes Element** 

[![Vorhandenes Element hinzufügen](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Navigieren Sie zur zuvor heruntergeladenen Datei **Picasso-2. x. x. jar** , wählen Sie Sie aus, und klicken Sie auf **Hinzufügen**: 

[![JAR-Datei auswählen und auf Hinzufügen klicken](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Überprüfen Sie, ob die Datei **Picasso-2. x. x. jar** dem Projekt erfolgreich hinzugefügt wurde: 

[![JAR wurde dem Projekt hinzugefügt.](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Wenn Sie ein Java-Bindungs Bibliotheksprojekt erstellen, müssen Sie angeben, ob. JAR muss in die Bindungs Bibliothek eingebettet oder separat verpackt werden. Zu diesem Zweck geben Sie eine der folgenden *Buildaktionen*an: 

- **Embeddebug** &ndash; die. JAR wird in die Bindungs Bibliothek eingebettet.

- **Input jar** &ndash; die. JAR wird von der Bindungs Bibliothek getrennt aufbewahrt.

In der Regel verwenden Sie die **embeddedjar** -Buildaktion, damit das. JAR wird automatisch in die Bindungs Bibliothek gepackt. Dies ist die einfachste Option &ndash; Java-Bytecode in. JAR wird in den DEX-Bytecode konvertiert und ist (zusammen mit den verwalteten Callable Wrappern) in Ihr APK eingebettet. , Wenn die beibehalten werden soll. JAR getrennt von der Bindungs Bibliothek können Sie die **inputjar** -Option verwenden. Sie müssen jedoch sicherstellen, dass die. Die JAR-Datei ist auf dem Gerät verfügbar, auf dem die app ausgeführt wird. 

Legen Sie die Buildaktion auf **embeddedjar**fest: 

[![Embeddedjar-Buildaktion auswählen](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Öffnen Sie als nächstes die Projekteigenschaften, um das *Ziel Framework*zu konfigurieren. , Wenn die. JAR verwendet alle Android-APIs, legen Sie das Ziel Framework auf die API-Ebene fest, auf der das. JAR erwartet. In der Regel ist dies der Entwickler von. In der JAR-Datei wird angegeben, auf welcher API-Ebene (oder welchen Ebenen) das. JAR ist mit kompatibel. (Weitere Informationen zur Ziel Framework-Einstellung und den Android-API-Ebenen im Allgemeinen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).)

Legen Sie die Ziel-API-Ebene für Ihre Bindungs Bibliothek fest (in diesem Beispiel verwenden wir API-Ebene 19): 

[![Ziel-API-Ebene auf API 19 festgelegt](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

Erstellen Sie abschließend die Bindungs Bibliothek. Obwohl einige Warnmeldungen angezeigt werden können, sollte das Projekt der Bindungs Bibliothek erfolgreich erstellt und eine Ausgabe erstellt werden. DLL an folgendem Speicherort: **Jarbinding/bin/debug/jarbinding. dll**

### <a name="using-the-bindings-library"></a>Verwenden der Bindungs Bibliothek

, Um dieses zu verwenden. Führen Sie in der xamarin. Android-App die folgenden Schritte aus:

1. Fügen Sie einen Verweis auf die Bindungs Bibliothek hinzu.

2. Nehmen Sie Aufrufe an. JAR durch die verwalteten Callable Wrapper. 

In den folgenden Schritten erstellen wir eine minimale APP, die die Bindungs Bibliothek zum herunterladen und Anzeigen eines Bilds in einem `ImageView`verwendet. der Code, der sich in der befindet, wird durch den Code, der in der enthalten ist, als "intensive Arbeit". JAR-Datei. 

Erstellen Sie zunächst eine neue xamarin. Android-App, die die Bindungs Bibliothek nutzt. Klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Neues Projekt** nennen Sie das neue Projekt **bindingtest**. Diese APP wird in derselben Projekt Mappe wie die Bindungs Bibliothek erstellt, um diese exemplarische Vorgehensweise zu vereinfachen. Allerdings kann sich die APP, die die Bindungs Bibliothek verwendet, stattdessen in einer anderen Projekt Mappe befinden: 

[![Neues bindingtest-Projekt hinzufügen](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

Klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** des Projekts **bindingtest** , und wählen Sie **Verweis hinzufügen...** aus:

[![Rechte Verweis hinzufügen](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Überprüfen Sie das zuvor erstellte Projekt **jarbinding** , und klicken Sie auf **OK**:

[![Jarbinding-Projekt auswählen](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Öffnen Sie den Knoten **Verweise** des **bindingtest** -Projekts, und überprüfen Sie, ob der **jarbinding** -Verweis vorhanden ist: 

[![Jarbinding wird unter "Verweise" angezeigt](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Ändern Sie das **bindingtest** -Layout (**Main. axml**) so, dass es `ImageView`ein einzelnes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

Fügen Sie die `using` folgende-Anweisung zu **MainActivity.cs** &ndash; hinzu. dadurch ist es möglich, auf einfache Weise auf die `Picasso` Methoden der Java-basierten Klasse zuzugreifen, die sich in der Bindungs Bibliothek befindet:

```csharp
using Com.Squareup.Picasso;
```

Ändern Sie `OnCreate` die-Methode so, dass `Picasso` Sie die-Klasse zum Laden eines Bilds aus einer URL und `ImageView`zum Anzeigen in der verwendet: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

Kompilieren Sie das Projekt **bindingtest** , und führen Sie es aus. Die APP wird gestartet, und nach einer kurzen Verzögerung (abhängig von den Netzwerkbedingungen) sollte ein Bild heruntergeladen und angezeigt werden, das dem folgenden Screenshot ähnelt:

[![Screenshot der Ausführung von "bindingtest"](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine Java-Bibliothek gebunden. JAR und in ihrer xamarin. Android-App verwendet.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben wir eine Bindungs Bibliothek für einen Drittanbieter erstellt. JAR-Datei, Hinzufügen der Bindungs Bibliothek zu einer minimalen Test-App und anschließendes durchlaufen der APP C# , um zu überprüfen, ob der Code Java-Code in der-Datei abrufen kann. JAR-Datei. 

## <a name="related-links"></a>Verwandte Links

- [Aufbauen einer Java-Bindungs Bibliothek (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
