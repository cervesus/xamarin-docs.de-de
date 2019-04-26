---
title: Binden einer JAR-Datei
description: Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek von Android. JAR-Datei.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 3c84b29807fd4a181ed867095645005bf9ba4df0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957335"
---
# <a name="binding-a-jar"></a>Binden einer JAR-Datei

_Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek von Android. JAR-Datei._

## <a name="overview"></a>Übersicht

Die Android-Community bietet zahlreiche Java-Bibliotheken, die Sie in Ihrer app verwenden möchten. Diese Bibliotheken für Java werden häufig in gepackt. JAR-Datei (Java Archive)-Format, aber Sie können Paket ein. In der JAR-eine *Java Bindungsbibliothek* , damit die Funktionen für Xamarin.Android-apps verfügbar sind. Gibt einen Überblick der Bibliothek für Java-Bindungen, stellen die APIs in der. JAR-Datei zur C# Code über automatisch generierte Code Wrapper.

Xamarin-Tools kann ein Bindungsbibliothek aus ein oder mehrere Eingaben generieren. JAR-Dateien. Die Bindungsbibliothek (. DLL-Assembly) enthält Folgendes: 

-   Der Inhalt des Originals. Die JAR-Dateien.

-   Verwaltete Callable Wrapper (MCW), die C# eingibt, Wrap entsprechende Java-in Typen der. Die JAR-Dateien.

Der generierte MCW-Code verwendet die JNI (Java Native Interface) Ihre API-Aufrufe weiterleiten an den zugrunde liegenden. JAR-Datei. Sie können für alle Bindungen Bibliotheken erstellen. JAR-Datei, die ursprünglich abgefragt wurde, um mit Android (Beachten Sie, dass Xamarin-Tools die Bindung von nicht - Android Java-Bibliotheken derzeit nicht unterstützt wird) verwendet werden. Sie können auch festlegen, ob der Bindings-Bibliothek zu erstellen, ohne den Inhalt der. Die JAR-Datei, damit die DLL abhängig ist, auf die. Die JAR-Laufzeit.

In diesem Handbuch werden wir die Grundlagen der Erstellung einer Bibliothek Bindungen für eine einzelne durchlaufen. JAR-Datei. Wir illustriere das mit einem Beispiel, in denen alles rechten &ndash; , in denen keine anpassungs- oder Debugzwecke Bindungen erforderlich ist. 
[Erstellen von Bindungen mithilfe von Metadaten](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bietet ein Beispiel für ein erweitertes Szenario, in dem der Bindungsprozess ist nicht vollständig automatisch und einigen Aufwand an manuellen Eingriffen ist erforderlich. Eine Übersicht über Java Bibliothek Bindung in der Regel (mit einem einfachen Codebeispiel), finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md). 

 
## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In der folgenden exemplarischen Vorgehensweise erstellen wir eine Bibliothek mit Bindungen [Picasso](http://square.github.io/picasso/), eine beliebte Android. JAR-Datei, das Image laden und die Funktionalität zum Zwischenspeichern bietet. Wir verwenden die folgenden Schritte zum Binden **Picasso-2.x.x.jar** in eine Xamarin.Android-Projekt erstellen, eine neue .NET Assembly, die wir verwenden können: 

1. Erstellen Sie ein neues Java Bindungsbibliothek-Projekt.

2. Hinzufügen der. JAR-Datei zum Projekt.

3. Legen Sie den entsprechenden Buildvorgang für die. JAR-Datei.

4. Wählen Sie ein Zielframework, die die. JAR-Datei unterstützt.

5. Erstellen Sie die Bindungsbibliothek.

Nachdem wir die Bibliothek Bindungen erstellt haben, müssen wir eine kleine Android-app entwickeln, die unsere Fähigkeit zum Aufrufen von APIs in der Bibliothek von Bindungen veranschaulicht. In diesem Beispiel möchten wir den Zugriff auf Methoden der **Picasso-2.x.x.jar**:

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

Nachdem wir eine Bibliothek mit Bindungen generiert **Picasso-2.x.x.jar**, wir können diese Methoden aus aufrufen C#. Zum Beispiel:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>Erstellen der Bindungsbibliothek

Bevor Sie mit den nächsten Schritten fortfahren, laden Sie [Picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

Erstellen Sie zunächst ein neues Bindings Library-Projekt. Klicken Sie in Visual Studio für Mac oder Visual Studio eine neue Projektmappe erstellen, und wählen Sie die *Android-Bindungsbibliothek* Vorlage. (Die Screenshots in dieser exemplarischen Vorgehensweise verwenden Sie Visual Studio, aber Visual Studio für Mac ist sehr ähnlich.) Nennen Sie die Projektmappe **JarBinding**: 

[![JarBinding-Steuerelementbibliothek-Projekt erstellen](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

Die Vorlage enthält einen **Jars** Ordner, in dem Sie sich hinzufügen, Ihrer. JAR(s) auf das Bindings-Bibliotheksprojekt hinzu. Mit der rechten Maustaste die **Jars** Ordner, und wählen **hinzufügen > Vorhandenes Element**: 

[![Vorhandenes Element hinzufügen](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Navigieren Sie zu der **Picasso-2.x.x.jar** Datei zuvor heruntergeladen haben, wählen Sie ihn aus, und klicken Sie auf **hinzufügen**: 

[![Wählen Sie die JAR-Datei, und klicken Sie auf Hinzufügen](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Überprüfen Sie, ob die **Picasso-2.x.x.jar** Datei wurde erfolgreich dem Projekt hinzugefügt: 

[![JAR-Datei zum Projekt hinzugefügt](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Wenn Sie ein Java-Bindungen-Steuerelementbibliothek-Projekt erstellen, müssen Sie angeben, ob die. JAR-Datei befindet sich in der Bindings-Bibliothek eingebettet oder separat gepackt werden soll. Zu diesem Zweck geben Sie einen der folgenden *Buildvorgänge*: 

-   **EmbeddedJar** &ndash; der. JAR-Datei wird in der Bindings-Bibliothek eingebettet werden.

-   **InputJar** &ndash; der. JAR-Datei wird von der Bindungsbibliothek getrennt werden.

Normalerweise verwenden Sie die **EmbeddedJar** Buildvorgang, damit die. JAR-Datei wird automatisch in der Bindings-Bibliothek gepackt. Dies ist die einfachste Option &ndash; Java-Bytecode in der. JAR-Datei in Dex-Bytecode konvertiert und in Ihrem APK (zusammen mit den verwalteten Callable Wrapper) eingebettet ist. Wenn Sie beibehalten möchten die. Aus der bindungsbibliothek separate JAR, können Sie mit der **InputJar** option; Sie müssen jedoch sicherstellen, dass die. JAR-Datei steht auf dem Gerät, das Ihre app ausgeführt wird. 

Legen Sie die Buildaktion auf **EmbeddedJar**: 

[![Wählen Sie die Buildaktion EmbeddedJar](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Öffnen Sie die Projekteigenschaften so konfigurieren Sie als Nächstes die *Zielframework*. Wenn die. JAR-Datei verwendet alle Android-APIs, die das Zielframework auf die API-Ebene festgelegt, die die. JAR-Datei erwartet wird. In der Regel vom Entwickler des der. JAR-Datei wird angegeben, welche API-Ebene (oder Ebenen), die die. JAR-Datei ist mit kompatibel. (Weitere Informationen zu den Zielframework-Einstellung und Android-API-Ebenen im Allgemeinen finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).)

Festlegen das Ziel-API von Ihrer Bindungsbibliothek (in diesem Beispiel verwenden wir API-Ebene 19): 

[![Legen Sie auf der API-19-API-Zielebene](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


Erstellen Sie schließlich die Bindungen-Bibliothek. Obwohl einige Warnmeldungen angezeigt werden können, sollte das Bindings-Steuerelementbibliothek-Projekt erfolgreich erstellt werden, und eine Ausgabe generieren. Die DLL an folgendem Speicherort: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>Mithilfe der Bindings-Bibliothek

Um dies zu nutzen. Die DLL in Ihrer Xamarin.Android-app wie folgt vor:

1.  Fügen Sie einen Verweis auf das Bindings-Bibliothek hinzu.

2.  Aufrufe in die. Die JAR-durch die verwalteten Callable Wrapper. 

In den folgenden Schritten erstellen wir eine minimale app, die die Bindungsbibliothek herunterladen, und zeigen Sie ein Bild in verwendet eine `ImageView`; der "Arbeit" erfolgt durch den Code, der in befindet sich die. JAR-Datei. 

Erstellen Sie zunächst eine neue Xamarin.Android-app, die die Bindungsbibliothek nutzt. Mit der rechten Maustaste in der Projektmappe, und wählen Sie **neues Projekt hinzufügen**, nennen Sie das neue Projekt **BindingTest**. Wir sind diese app in der gleichen Projektmappe wie der Bindings-Bibliothek erstellen, um in dieser exemplarischen Vorgehensweise zu vereinfachen; Allerdings könnten Sie die app, die die Bindungsbibliothek nutzt stattdessen in einer anderen Projektmappe befinden: 

[![Fügen Sie neue BindingTest-Projekt hinzu](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

Mit der rechten Maustaste die **Verweise** Knoten die **BindingTest** Projekt, und wählen **Verweis hinzufügen...** :

[![Fügen Sie direkt Verweis hinzu.](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Überprüfen Sie die **JarBinding** zuvor erstellte Projekt, und klicken Sie auf **OK**:

[![Wählen Sie JarBinding-Projekt](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Öffnen der **Verweise** Knoten die **BindingTest** Projekt, und überprüfen Sie, ob die **JarBinding** Verweis vorhanden ist: 

[![JarBinding wird unter Verweise angezeigt.](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Ändern der **BindingTest** Layout (**Main.axml**) so, dass sie eine einzelne `ImageView`:

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

Fügen Sie die folgenden `using` Anweisung **"mainactivity.cs"** &ndash; Dadurch kann einfach die Methoden der Java-basierten Zugriff auf `Picasso` -Klasse, die in der Bindings-Bibliothek befindet:

```csharp
using Com.Squareup.Picasso;
```

Ändern der `OnCreate` Methode, sodass die It verwendet die `Picasso` -Klasse zum Laden eines Bilds aus einer URL und Anzeigen der Daten in die `ImageView`: 

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

Kompilieren und Ausführen der **BindingTest** Projekt. Die app wird gestartet, und nach einer kurzen Verzögerung (je nach netzwerkbedingungen), sollte er herunterladen und zeigen Sie ein Bild im folgenden Screenshot ähnelt:

[![Screenshot der BindingTest ausgeführt wird](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine Java-Bibliothek gebunden. JAR aus, und es in Ihrer Xamarin.Android-app verwendet.
 
 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben wir eine Bibliothek mit Bindungen eines Drittanbieters. Die JAR-Datei, die Bindungsbibliothek eine minimale app hinzugefügt, und klicken Sie dann die app aus, um sicherzustellen, dass ausgeführt unsere C# Code kann sich befinden, der Java-Code Aufrufen der. JAR-Datei. 



## <a name="related-links"></a>Verwandte Links

- [Erstellen einer Java Bindungsbibliothek (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
