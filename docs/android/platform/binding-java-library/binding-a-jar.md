---
title: Binden einer JAR-Datei
description: Diese exemplarische Vorgehensweise enthält ausführliche Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek aus einem Android-JAR-Datei.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 336462176813b9985e2c269273d08c4aacbea9b5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853096"
---
# <a name="binding-a-jar"></a>Binden einer JAR-Datei

> [!IMPORTANT]
> Wir untersuchen derzeit die Nutzung benutzerdefinierter Bindungen auf der Xamarin-Plattform. Nehmen Sie an [**dieser Umfrage**](https://www.surveymonkey.com/r/KKBHNLT) teil, um zukünftige Entwicklungsarbeiten zu unterstützen.

_Diese exemplarische Vorgehensweise enthält ausführliche Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek aus einem Android-JAR-Datei._

## <a name="overview"></a>Übersicht

Die Android-Community hat viele Java-Bibliotheken veröffentlicht, die Sie für Ihre App verwenden können. Diese Java-Bibliotheken werden häufig im JAR-Format (Java Archive) gepackt. Sie können eine JAR-Datei jedoch in eine *Bibliothek für Java-Bindungen* packen, sodass deren Funktionen von Xamarin.Android-Apps genutzt werden können. Der Zweck der Bibliothek für Java-Bindungen besteht darin, die in der JAR-Datei enthaltenen APIs über automatisch generierte Codewrapper dem C#-Code zur Verfügung zu stellen.

Xamarin-Tools können eine Bindungsbibliothek aus mindestens einer JAR-Eingabedatei generieren. Die Bindungsbibliothek (DLL-Assembly) enthält Folgendes: 

- den Inhalt der ursprünglichen JAR-Datei(en)

- verwaltete Aufrufwrapper (Managed Callable Wrappers, MCW), bei denen es sich um C#-Typen handelt, die entsprechende Java-Typen in den JAR-Datei(en) umschließen

Der generierte MCW-Code verwendet JNI (Java Native Interface), um Ihre API-Aufrufe an die zugrunde liegende JAR-Datei weiterzuleiten. Sie können Bindungsbibliotheken für eine beliebige JAR-Datei erstellen, die ursprünglich für die Verwendung mit Android konzipiert war. Beachten Sie jedoch, dass Xamarin-Tools die Bindung von Nicht-Android-Java-Bibliotheken derzeit nicht unterstützen. Sie können auch festlegen, dass die Bindungsbibliothek erstellt werden soll, ohne den Inhalt der JAR-Datei einzubeziehen, sodass die DLL zur Laufzeit eine Abhängigkeit zur JAR-Datei aufweist.

In dieser Anleitung werden die Grundlagen der Erstellung einer Bindungsbibliothek für eine einzelne JAR-Datei ausführlich erläutert. Das wird mit einem Beispiel veranschaulicht, bei dem alles wie beabsichtigt abläuft. Das bedeutet, dass bei diesem keine Anpassung und kein Debugging der Bindungen erforderlich ist. 
[Das Erstellen von Bindungen mithilfe von Metadaten](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) ist ein Beispiel für ein erweitertes Szenario, bei dem der Bindungsprozess nicht vollständig automatisch erfolgt und ein gewisser manueller Eingriff erforderlich ist. Eine allgemeine Übersicht über die Java-Bibliotheksbindung (mit einem einfachen Codebeispiel) finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md). 

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In der folgenden exemplarischen Vorgehensweise erstellen Sie eine Bindungsbibliothek für [Picasso](https://square.github.io/picasso/), eine beliebte Android-JAR-Datei, die Funktionen zum Laden und Zwischenspeichern von Bildern bereitstellt. Führen Sie die folgenden Schritte aus, um **picasso-2.x.x.jar** zu binden und somit eine neue .NET-Assembly zu erstellen, die Sie in einem Xamarin.Android-Projekt verwenden können: 

1. Erstellen Sie ein neues Java-Bindungsbibliotheksprojekt.

2. Fügen Sie die JAR-Datei zum Projekt hinzu.

3. Legen Sie die entsprechende Buildaktion für die JAR-Datei fest.

4. Wählen Sie das Zielframework aus, das die JAR-Datei unterstützt.

5. Erstellen Sie die Bindungsbibliothek.

Nachdem Sie die Bindungsbibliothek erstellt haben, entwickeln Sie eine einfache Android-App, die das Abrufen von APIs in der Bindungsbibliothek veranschaulicht. In diesem Beispiel möchten Sie auf Methoden von **picasso-2.x.x.jar** zugreifen:

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

Nachdem Sie eine Bindungsbibliothek für **picasso-2.x.x.jar** generiert haben, können Sie diese Methoden von C# abrufen. Zum Beispiel:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>Erstellen der Bindungbibliothek

Bevor Sie mit den folgenden Schritten beginnen, müssen Sie [picasso-2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar) herunterladen.

Erstellen Sie zunächst ein neues Bindungsbibliotheksprojekt. Erstellen Sie in Visual Studio für Mac oder Visual Studio eine neue Projektmappe, und wählen Sie die Vorlage *Android-Bindungsbibliothek* aus. (Die Screenshots in dieser exemplarischen Vorgehensweise stammen aus Visual Studio, aber Visual Studio für Mac ist sehr ähnlich.) Nennen Sie die Projektmappe **JarBinding**: 

[![JarBinding-Bibliotheksprojekt erstellen](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

Die Vorlage enthält den Ordner **Jars**, in dem Sie Ihre JAR-Datei(en) zum Bindungsbibliotheksprojekt hinzufügen können. Klicken Sie mit der rechten Maustaste auf den Ordner **Jars**, und wählen Sie **Hinzufügen > Vorhandenes Element** aus: 

[![Vorhandenes Element hinzufügen](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Navigieren Sie zur zuvor heruntergeladenen Datei **picasso-2.x.x.jar**, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**: 

[![JAR-Datei auswählen und auf „Hinzufügen“ klicken](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Vergewissern Sie sich, dass die Datei **picasso-2.x.x.jar** dem Projekt erfolgreich hinzugefügt wurde: 

[![JAR-Datei wurde dem Projekt hinzugefügt](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Wenn Sie ein Java-Bindungsbibliotheksprojekt erstellen, müssen Sie angeben, ob die JAR-Datei in die Bindungsbibliothek eingebettet oder separat gepackt ist. Zu diesem Zweck geben Sie eine der folgenden *Buildaktionen* an: 

- **EmbeddedJar:** Die JAR-Datei wird in die Bindungsbibliothek eingebettet.

- **InputJar:** Die JAR-Datei wird von der Bindungsbibliothek getrennt gespeichert.

In der Regel verwenden Sie die Buildaktion **EmbeddedJar**, damit die JAR-Datei automatisch in die Bindungsbibliothek gepackt wird. Dabei handelt es sich um die einfachste Option, da Java-Bytecode in der JAR-Datei in DEX-Bytecode konvertiert und (zusammen mit den verwalteten Aufrufwrappern) in Ihr Android-Anwendungspaket eingebettet wird. Wenn die JAR-Datei getrennt von der Bindungsbibliothek gespeichert werden soll, können Sie die Option **InputJar** verwenden. Sie müssen jedoch sicherstellen, dass die JAR-Datei auf dem Gerät verfügbar ist, auf dem die App ausgeführt wird. 

Legen Sie die Buildaktion auf **EmbeddedJar** fest: 

[![EmbeddedJar-Buildaktion auswählen](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Öffnen Sie als Nächstes die Projekteigenschaften, um das *Zielframework* zu konfigurieren. Wenn die JAR-Datei Android-APIs verwendet, sollten Sie das Zielframework auf die API-Ebene festlegen, die die JAR-Datei erwartet. In der Regel gibt der Entwickler der JAR-Datei an, mit welchen API-Ebenen die JAR-Datei kompatibel ist. Weitere Informationen zur Einstellung des Zielframeworks und den Android-API-Ebenen im Allgemeinen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

Legen Sie die API-Zielebene für Ihre Bindungsbibliothek fest (in diesem Beispiel wird API-Ebene 19 verwendet): 

[![API-Zielebene auf API 19 festgelegt](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

Erstellen Sie abschließend die Bindungsbibliothek. Obwohl möglicherweise einige Warnmeldungen angezeigt werden, sollte das Projekt für die Bindungsbibliothek erfolgreich erstellt und eine Ausgabe-DLL an folgendem Speicherort erzeugt werden: **JarBinding/bin/Debug/JarBinding.dll**

### <a name="using-the-bindings-library"></a>Verwenden der Bindungsbibliothek

Sie können die DLL folgendermaßen in Ihrer Xamarin.Android-App nutzen:

1. Fügen Sie einen Verweis auf die Bindungsbibliothek hinzu.

2. Führen Sie über die verwalteten Aufrufwrapper Aufrufe der JAR-Datei durch. 

In den folgenden Schritten erstellen Sie eine einfache App, die die Bindungsbibliothek zum Herunterladen und Anzeigen eines Bilds in einem `ImageView`-Element verwendet. Die meiste Arbeit verrichtet dabei der Code in der JAR-Datei. 

Erstellen Sie zunächst eine neue Xamarin.Android-App, die die Bindungsbibliothek nutzt. Klicken Sie mit der rechten Maustaste auf die Projektmappe, wählen Sie **Neues Projekt hinzufügen** aus, und benennen Sie das neue Projekt **BindingTest**. Diese App wird in derselben Projektmappe wie die Bindungsbibliothek erstellt, um diese exemplarische Vorgehensweise zu vereinfachen. Allerdings kann sich die App, die die Bindungsbibliothek verwendet, auch in einer anderen Projektmappe befinden: 

[![Neues BindingTest-Projekt hinzufügen](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

Klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** des **BindingTest**-Projekts, und wählen Sie **Verweis hinzufügen...** aus:

[![Rechtsklick > Verweis hinzufügen](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Wählen Sie das zuvor erstellte Projekt **JarBinding** aus, und klicken Sie auf **OK**:

[![JarBinding-Projekt auswählen](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Öffnen Sie den Knoten **Verweise** des **BindingTest**-Projekts, und vergewissern Sie sich, dass der Verweis auf **JarBinding** vorhanden ist: 

[![JarBinding wird unter „Verweise“ aufgeführt](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Ändern Sie das Layout von **BindingTest** (**Main.axml**), sodass nur ein `ImageView`-Element vorhanden ist:

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

Fügen Sie in **MainActivity.cs&ndash;** die folgende `using`-Anweisung hinzu, um den Zugriff auf die Methoden der Java-basierten `Picasso`-Klasse zu ermöglichen, die sich in der Bindungsbibliothek befindet:

```csharp
using Com.Squareup.Picasso;
```

Ändern Sie die `OnCreate`-Methode so, dass sie die `Picasso`-Klasse verwendet, um ein Bild aus einer URL zu laden und in `ImageView` anzuzeigen: 

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

Kompilieren Sie das Projekt **BindingTest**, und führen Sie es aus. Die App wird gestartet, und nach einer kurzen Verzögerung (abhängig von den Netzwerkbedingungen) sollte ähnlich wie auf dem folgenden Screenshot ein Bild heruntergeladen und angezeigt werden:

[![Screenshot der Ausführung von BindingTest](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine JAR-Datei für eine Java-Bibliothek gebunden und in Ihrer Xamarin.Android-App eingesetzt.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben Sie eine Bindungsbibliothek für eine JAR-Datei eines Drittanbieters erstellt, die Bindungsbibliothek zu einer einfachen Test-App hinzugefügt und die App ausgeführt, um sich davon zu überzeugen, dass der C#-Code den Java-Code in der JAR-Datei aufrufen kann. 

## <a name="related-links"></a>Verwandte Links

- [Erstellen einer Java-Bindungsbibliothek (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
