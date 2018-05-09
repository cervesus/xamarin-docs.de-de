---
title: Binden von ein. JAR-DATEI
description: Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android Java Bindungen Bibliothek aus einer Android. Die JAR-Datei.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 2d9f2198dbb88e7614944ac73729a4e6eca42647
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="binding-a-jar"></a>Binden von ein. JAR-DATEI

_Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android Java Bindungen Bibliothek aus einer Android. Die JAR-Datei._

## <a name="overview"></a>Übersicht

Die Android-Community bietet viele Java-Bibliotheken, die Sie in Ihrer app verwenden möchten. Diese Java-Bibliotheken werden in oft verpackt. JAR-Datei (Java-Archiv)-Format, aber Sie können Paket ein. JAR in einem *Java Bindungen Bibliothek* , damit die Funktionen für Xamarin.Android-apps verfügbar sind. Der Zweck der Bibliothek für Java-Bindungen ist, die APIs in der. Die JAR-Datei, die für C#-Code durch automatisch generierte Code Wrapper verfügbar.

Xamarin-Tools möglicherweise mindestens eine Eingabe einer Bibliothek Bindungen generiert. JAR-Dateien. Bindungen-Bibliothek (. DLL-Assembly) enthält Folgendes: 

-   Der Inhalt des ursprünglichen. JAR-Dateien.

-   Verwaltete Callable Wrapper (MCW), die C#-Typen, Wrap entsprechenden Java-in Typen der. JAR-Dateien.

Der generierte MCW-Code verwendet JNI (Java Native Interface) der API-Aufrufe weiterleiten auf die zugrunde liegende. Die JAR-Datei. Sie können für alle Bindungen Bibliotheken erstellen. Die JAR-Datei, die ursprünglich abgefragt wurde, um mit Android (Beachten Sie, dass der Xamarin-Tools sind derzeit die Bindung von nicht - Android Java-Bibliotheken nicht unterstützt) verwendet werden. Sie können auch festlegen, ob die Bindungen-Bibliothek erstellen möchten, ohne den Inhalt des der. JAR-Datei, sodass die DLL abhängig ist die. Die JAR-Laufzeit.

In diesem Handbuch werden wir die Grundlagen der Erstellung einer Bibliothek Bindungen für eine einzelne durchlaufen. Die JAR-Datei. Illustrieren sowie ein Beispiel dafür, wohin alle Rechte gehen &ndash; , in denen keine anpassungs- oder Debugzwecke Bindungen erforderlich ist. 
[Erstellen von Bindungen mithilfe von Metadaten](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bietet ein Beispiel für eine erweiterte Szenarios, in dem des Bindungsvorgangs nicht vollständig automatische und manuelle Eingriffe gewisse erforderlich ist. Einen Überblick über das Java Bibliothek binding im Allgemeinen (mit einem Beispiel für basic-Code) finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md). 

 
## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In der folgenden exemplarischen Vorgehensweise erstellen wir eine Bibliothek mit Bindungen [Picasso](http://square.github.io/picasso/), eine beliebte Android. JAR-Datei, das Image laden und Zwischenspeichern von Funktionen bereitstellt. Wir verwenden die folgenden Schritte zum Binden **Picasso 2.x.x.jar** So erstellen eine neue .NET Assembly, die wir verwenden können, in einem Projekt Xamarin.Android: 

1. Erstellen Sie ein neues Klassenbibliothekprojekt mit Java-Bindungen.

2. Hinzufügen der. Die JAR-Datei zum Projekt.

3. Legen Sie den entsprechenden Buildvorgang für die. Die JAR-Datei.

4. Wählen Sie ein Zielframework, die die. JAR-Datei unterstützt.

5. Erstellen der Bindungen-Bibliothek.

Nachdem wir die Bindungen Bibliothek erstellt haben, müssen wir eine kleine Android-app entwickeln, die unsere Fähigkeit, Aufrufen von APIs in der Bibliothek Bindungen veranschaulicht. In diesem Beispiel wird die Methoden der zugreifen möchten **Picasso 2.x.x.jar**:

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

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>Erstellen der Bindungen-Bibliothek

Bevor Sie mit den folgenden Schritten fortfahren, laden Sie Sie [Picasso 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

Erstellen Sie zunächst ein neues Klassenbibliothekprojekt von Bindungen. Klicken Sie in Visual Studio für Mac oder Visual Studio eine neue Projektmappe erstellen, und wählen Sie die *Bibliothek für Android Bindungen* Vorlage. (Die Screenshots in dieser exemplarischen Vorgehensweise verwenden Sie Visual Studio, aber Visual Studio für Mac ist sehr ähnlich.) Nennen Sie die Projektmappe **JarBinding**: 

[![JarBinding-Steuerelementbibliothek-Projekt erstellen](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

Die Vorlage enthält eine **JAR-Dateien** Ordner, in dem Sie hinzufügen, Ihrer. JAR(s) auf das Bindings-Steuerelementbibliothek-Projekt. Mit der rechten Maustaste die **JAR-Dateien** Ordner, und wählen **hinzufügen > Vorhandenes Element**: 

[![Vorhandenes Element hinzufügen](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Navigieren Sie zu der **Picasso 2.x.x.jar** Datei zuvor heruntergeladen haben, wählen Sie diese aus, und klicken Sie auf **hinzufügen**: 

[![Wählen Sie die Jar-Datei, und klicken Sie auf Hinzufügen](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Überprüfen Sie, ob die **Picasso 2.x.x.jar** Datei wurde erfolgreich dem Projekt hinzugefügt: 

[![JAR-Datei zum Projekt hinzugefügt](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Wenn Sie ein Klassenbibliotheksprojekt für die Java-Bindungen erstellen, müssen Sie angeben, ob die. JAR-Datei besteht darin, in der Bibliothek Bindungen eingebettet oder separat verpackt werden. Zu diesem Zweck geben Sie einen der folgenden *Buildvorgänge*: 

-   **EmbeddedJar** &ndash; der. JAR-Datei wird in der Bibliothek Bindungen eingebettet werden.

-   **InputJar** &ndash; der. JAR-Datei wird getrennt von der Bibliothek Bindungen gehalten werden.

Normalerweise verwenden Sie die **EmbeddedJar** Buildvorgang, damit die. JAR-Datei wird automatisch in der Bibliothek Bindungen gepackt. Dies ist die einfachste Möglichkeit &ndash; Java-Bytecode in der. JAR-Datei in Dex Bytecode konvertiert wird und in Ihre APK (zusammen mit der verwalteten Callable Wrapper) eingebettet ist. Wenn Sie möchten die. JAR getrennt von der Bibliothek Bindungen, können Sie mithilfe der **InputJar** option; Sie müssen jedoch sicherstellen, dass die. JAR-Datei steht auf dem Gerät, das Ihre app ausgeführt wird. 

Legen Sie die Buildaktion auf **EmbeddedJar**: 

[![Wählen Sie EmbeddedJar Buildvorgang](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Als Nächstes öffnen Sie das Projekt konfigurierbaren Eigenschaften der *Zielframework*. Wenn die. JAR-Datei werden alle Android-APIs verwendet, legen Sie das Zielframework auf API-Ebene, die. JAR-Datei wird erwartet. In der Regel der Entwickler die. JAR-Datei wird angegeben, welche API-Ebene (oder Ebenen), die die. JAR-Datei ist mit kompatibel. (Weitere Informationen zu den Zielframework-Einstellung und Android-API-Ebenen im Allgemeinen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).)

Das Ziel-API-Ebene festlegen, für die Bindungen-Bibliothek (in diesem Beispiel verwenden wir API Level 19): 

[![Ziel-API-Ebene 19 der API](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


Erstellen Sie abschließend die Bindungen-Bibliothek. Obwohl einige Warnmeldungen angezeigt werden können, sollte das Bindings-Steuerelementbibliothek-Projekt erfolgreich erstellt und eine Ausgabe aus. Die DLL an folgender Stelle: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>Mithilfe der Bindungen-Bibliothek

Um dies zu nutzen. Gehen Sie in Ihrer app Xamarin.Android DLL:

1.  Fügen Sie einen Verweis auf die Bibliothek Bindungen.

2.  Aufrufe in die. Über die verwaltete Callable Wrapper JAR. 

In den folgenden Schritten erstellen wir eine minimale-app, mit denen die Bindungen Bibliothek herunterladen und zeigen Sie ein Bild in ein `ImageView`; die "harte" erfolgt durch den Code, der im befindet sich die. Die JAR-Datei. 

Erstellen Sie zunächst eine neue Xamarin.Android-app, die die Bindungen Bibliothek nutzt. Mit der rechten Maustaste in der Projektmappe, und wählen Sie **neues Projekt hinzufügen**; benennen Sie das neue Projekt **Namen "bindingtest"**. Wir sind diese app in derselben Projektmappe wie die Bindungen-Bibliothek erstellen, um diese exemplarische Vorgehensweise zu vereinfachen; Allerdings kann die app, die die Bindungen Bibliothek nutzt stattdessen in einer anderen Projektmappe befinden: 

[![Hinzufügen eines neuen Namen "bindingtest"-Projekts](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

Mit der rechten Maustaste die **Verweise** Knoten der **Namen "bindingtest"** Projekt, und wählen Sie **Verweis hinzufügen...** :

[![Fügen Sie direkt Verweis hinzu.](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Überprüfen Sie die **JarBinding** Projekt zuvor erstellt haben, und klicken Sie auf **OK**:

[![Wählen Sie JarBinding-Projekt](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Öffnen der **Verweise** Knoten der **Namen "bindingtest"** Projekt, und überprüfen Sie, ob die **JarBinding** Verweis vorhanden ist: 

[![JarBinding wird unter Verweise angezeigt.](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Ändern der **Namen "bindingtest"** Layout (**Main.axml**) so, dass sie ein einzelnes `ImageView`:

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

Fügen Sie die folgenden `using` Anweisung **MainActivity.cs** &ndash; dadurch leicht die Methoden der Java-basierten Zugriff auf `Picasso` -Klasse, die in der Bibliothek Bindings befindet:

```csharp
using Com.Squareup.Picasso;
```

Ändern der `OnCreate` Methode, damit verwendet die `Picasso` Klasse, um ein Bild von einer URL geladen werden, und zeigen Sie es in der `ImageView`: 

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

Kompilieren und Ausführen der **Namen "bindingtest"** Projekt. Die app wird gestartet, und nach einer kurzen Verzögerung (je nach netzwerkbedingungen), sollte er herunterladen und zeigen Sie ein Bild mit dem folgenden Screenshot vergleichbarer:

[![Screenshot der Namen "bindingtest" ausgeführt wird](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine Java-Bibliothek gebunden. JAR und es in Ihrer app Xamarin.Android verwendet.
 
 
## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben wir eine Bibliothek mit Bindungen eines Drittanbieters. JAR-Datei, die Bindungen-Bibliothek einen minimalen Test-app hinzugefügt, und klicken Sie dann die app aus, um sicherzustellen, dass unsere C#-Code in den Java-Code aufrufen kann ausgeführt der. Die JAR-Datei. 



## <a name="related-links"></a>Verwandte Links

- [Zum Erstellen einer Bibliothek für Java-Bindungen (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
