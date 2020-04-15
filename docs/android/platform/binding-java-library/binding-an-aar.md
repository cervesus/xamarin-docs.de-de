---
title: Binden einer AAR-Datei
description: Diese exemplarische Vorgehensweise enthält ausführliche Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek aus einer Android-AAR-Datei.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 103720c8cb47b1ac4cfe5cfadeb6b18828318ad3
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73618535"
---
# <a name="binding-an-aar"></a>Binden einer AAR-Datei

_Diese exemplarische Vorgehensweise enthält ausführliche Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek aus einer Android-AAR-Datei._

## <a name="overview"></a>Übersicht

Bei *Android Archive (.AAR)* -Dateien handelt es sich um das Dateiformat für Android-Bibliotheken.
Eine AAR-Datei ist ein ZIP-Archiv mit dem folgenden Inhalt:

- Kompilierter Java-Code
- Ressourcen-IDs
- Ressourcen
- Metadaten (z. B. Aktivitätsdeklarationen, Berechtigungen)

In dieser Anleitung werden die Grundlagen der Erstellung einer Bindungsbibliothek für eine einzelne AAR-Datei ausführlich erläutert. Eine allgemeine Übersicht über die Java-Bibliotheksbindung (mit einem einfachen Codebeispiel) finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md).

> [!IMPORTANT]
> Ein Bindungsprojekt kann nur eine AAR-Datei enthalten. Wenn die AAR-Datei von einer anderen AAR-Datei abhängt, müssen die entsprechenden Abhängigkeiten in Ihrem eigenen Bindungsprojekt enthalten sein und dann referenziert werden. Weitere Informationen finden Sie unter [Bug 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser Vorgehensweise wird eine Bindungsbibliothek für eine Android-Archivdatei, [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true), als Beispiel erstellt, die in Android Studio erstellt wurde. Diese AAR-Datei enthält eine `TextCounter`-Klasse mit statischen Methoden, die die Anzahl der Vokale und Konsonanten in einer Zeichenfolge zählen. Darüber hinaus enthält **textanalyzer.aar** eine Bildressource zum Anzeigen der Zählergebnisse.

Das Erstellen einer Bindungsbibliothek aus der AAR-Datei umfasst folgenden Schritte:

1. Erstellen eines neuen Java-Bindungsbibliotheksprojekts.

2. Hinzufügen einer einzelnen AAR-Datei zum Projekt. Ein Bindungsprojekt darf nur eine einzelne AAR-Datei enthalten.

3. Festlegen des entsprechenden Buildvorgangs für die AAR-Datei.

4. Auswählen eines Zielframeworks, das die AAR-Datei unterstützt.

5. Erstellen der Bindungsbibliothek.

Nachdem die Bindungsbibliothek erstellt wurde, wird eine kleine Android-App entwickelt, die den Benutzer zur Eingabe einer Textzeichenfolge auffordert, AAR-Methoden zum Analysieren des Texts aufruft, das Bild aus der AAR-Datei abruft und die Ergebnisse zusammen mit dem Bild anzeigt.

Die Beispiel-App greift auf die `TextCounter`-Klasse von **textanalyzer.aar** zu:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

Die Beispiel-App ruft außerdem eine Bildressource, die in **textanalyzer.aar** gepackt ist, ab und zeigt diese an:

[![Bild des Xamarin-Affen](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Diese Bildressource befindet sich in **res/drawable/monkey.png** in **textanalyzer.aar**.

### <a name="creating-the-bindings-library"></a>Erstellen der Bindungsbibliothek

Bevor Sie mit den folgenden Schritten beginnen, müssen Sie die Android-Archivdatei [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) für dieses Beispiel herunterladen:

1. Erstellen Sie ausgehend von der Android-Bindungsbibliotheksvorlage ein neues Bindungsbibliotheksprojekt. Sie können Visual Studio für Mac oder Visual Studio verwenden (die folgenden Screenshots zeigen Visual Studio, Visual Studio für Mac ist aber sehr ähnlich). Nennen Sie die Projektmappe **AarBinding**:

    [![AarBindings-Projekt erstellen](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. Die Vorlage enthält den Ordner **Jars**, in dem Sie Ihre AAR-Datei(en) zum Bindungsbibliotheksprojekt hinzufügen können. Klicken Sie mit der rechten Maustaste auf den Ordner **Jars**, und wählen Sie **Hinzufügen > Vorhandenes Element** aus:

    [![Vorhandenes Element hinzufügen](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. Navigieren Sie zur zuvor heruntergeladenen Datei **textanalyzer.aar**, wählen Sie sie aus, und klicken Sie auf **Hinzufügen**:

    [![„textanalyzer.aar“ hinzufügen](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. Vergewissern Sie sich, dass die Datei **textanalyzer.aar** erfolgreich zum Projekt hinzugefügt wurde:

    [![Die Datei „textanalyzer.aar“ wurde hinzugefügt](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. Legen Sie den Buildvorgang für **textanalyzer.aar** auf `LibraryProjectZip` fest. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **textanalyzer.aar**, um den Buildvorgang festzulegen. In Visual Studio kann der Buildvorgang im Bereich **Eigenschaften** festgelegt werden:

    [![Festlegen des Buildvorgangs für „textanalyzer.aar“ auf LibraryProjectZip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. Öffnen Sie die Projekteigenschaften, um das *Zielframework* zu konfigurieren. Wenn die AAR-Datei Android-APIs verwendet, sollten Sie das Zielframework auf die API-Ebene festlegen, die von der AAR-Datei erwartet wird. (Weitere Informationen zur Einstellung für das Zielframework und den Android-API-Ebenen im Allgemeinen finden Sie unter [Verstehen von Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md)).

    Legen Sie die Ziel-API-Ebene für Ihre Bindungsbibliothek fest. In diesem Beispiel kann die aktuelle Plattform-API-Ebene (API-Ebene 23) verwendet werden, weil **textanalyzer** keine Abhängigkeit zu Android-APIs aufweist:

    [![Festlegen der Zielebene auf API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. Erstellen Sie die Bindungsbibliothek. Das Bindungsbibliotheksprojekt sollte erfolgreich erstellt werden und eine Ausgabe-DLL am folgenden Speicherort erzeugen: **AarBinding/bin/Debug/AarBinding.dll**

### <a name="using-the-bindings-library"></a>Verwenden der Bindungsbibliothek

Um diese DLL in der Xamarin.Android-App zu verwenden, müssen Sie zuerst einen Verweis auf die Bindungsbibliothek hinzufügen. Führen Sie die folgenden Schritte durch:

1. Diese App wird in derselben Projektmappe wie die Bindungsbibliothek erstellt, um diese exemplarische Vorgehensweise zu vereinfachen. (Die App, die die Bindungsbibliothek verwendet, kann sich auch in einer anderen Projektmappe befinden.) Erstellen Sie eine neue Xamarin.Android-App: Klicken Sie mit der rechten Maustaste auf die Projektmappe, und wählen Sie **Neues Projekt hinzufügen** aus. Geben Sie dem neuen Projekt den Namen **BindingTest**:

    [![Neues BindingTest-Projekt erstellen](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. Klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** des **BindingTest**-Projekts, und wählen Sie **Verweis hinzufügen...** aus:

    [![Klicken auf „Verweis hinzufügen“](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. Wählen Sie das zuvor erstellte Projekt **AarBinding** aus, und klicken Sie auf **OK**:

    [![Überprüfen des AAR-Bindungsprojekts](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. Öffnen Sie den Knoten **Verweise** des **BindingTest**-Projekts, und vergewissern Sie sich, dass der Verweis auf **AarBinding** vorhanden ist:

    [![Auflisten von „AarBinding“ unter „Verweise“](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

Wenn Sie den Inhalt des Bindungsbibliotheksprojekts anzeigen möchten, können Sie auf den Verweis doppelklicken, um ihn im **Objektkatalog** zu öffnen. Daraufhin werden die zugeordneten Inhalte des `Com.Xamarin.Textcounter`-Namespaces (zugeordnet aus dem Java-`com.xamarin.textanalyzezr`-Paket) und die Member der `TextCounter`-Klasse angezeigt:

[![Anzeigen des Objektkatalogs](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Im obige Screenshot sind die beiden `TextAnalyzer` Methoden markiert, die von der Beispiel-App aufgerufen werden: `NumConsonants` (Wrapper für die zugrunde liegende `numConsonants`-Java-Methode) und `NumVowels` (Wrapper für die zugrunde liegende `numVowels`-Java-Methode).

### <a name="accessing-aar-types"></a>Zugriff auf AAR-Typen

Nachdem Sie einen Verweis auf die App hinzugefügt haben, der auf die Bindungsbibliothek verweist, können Sie auf die Java-Typen in der AAR-Datei genauso wie auf C#-Typen zugreifen (aufgrund der C#-Wrapper). Der C#-App-Code kann `TextAnalyzer`-Methoden wie im folgenden Beispiel gezeigt aufrufen:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

Im obigen Beispiel werden statische Methoden in der `TextCounter`-Klasse aufgerufen. Sie können jedoch auch Klassen instanziieren und Instanzmethoden aufrufen. Wenn die AAR beispielsweise als Wrapper einer Klasse namens `Employee` mit der Instanzmethode `buildFullName` dient, können Sie `MyClass` instanziieren und wie hier gezeigt verwenden:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

In den folgenden Schritten wird der Anwendungscode hinzugefügt, der den Benutzer auffordert, Text einzugeben, `TextCounter` verwendet, um den Text zu analysieren, und dann die Ergebnisse anzeigt.

Ersetzen Sie das **BindingTest**-Layout (**Main.axml**) durch den folgenden XML-Code. Dieses Layout umfasst `EditText` für die Texteingabe und zwei Schaltflächen, um die Zählung der Vokale und Konsonanten einzuleiten:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Ersetzen Sie den Inhalt von **MainActivity.cs** durch den folgenden Code. Wie in diesem Beispiel gezeigt, rufen die Ereignishandler der Schaltflächen `TextCounter`-Wrappermethoden auf, die sich in der AAR-Datei befinden, und verwenden Popups, um die Ergebnisse anzuzeigen. Beachten Sie die `using`-Anweisung für den Namespace der gebundenen Bibliothek (in diesem Fall `Com.Xamarin.Textcounter`):

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Kompilieren Sie das Projekt **BindingTest**, und führen Sie es aus. Die App wird gestartet und zeigt den Screenshot auf der linken Seite an (`EditText` wird mit einem beliebigen Text initialisiert, Sie können den Text aber ändern, indem Sie darauf tippen). Wenn Sie auf **COUNT VOWELS** tippen, zeigt ein Popup die Anzahl der Vokale an (rechter Screenshot):

[![Screenshots bei der Ausführung von BindingTest](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Tippen Sie auf die Schaltfläche **COUNT CONSONANTS**. Außerdem können Sie die Textzeile ändern und erneut auf diese Schaltflächen tippen, um eine andere Anzahl von Vokalen und Konsonanten zu ermitteln.

### <a name="accessing-aar-resources"></a>Zugreifen auf AAR-Ressourcen

Das Xamarin-Tool mergt die **R**-Daten aus der AAR-Datei in die **Resource**-Klasse der App. Daher können Sie im Layout (und in CodeBehind) genauso wie auf Ressourcen, die sich im **Ressourcen**-Pfad des Projekts befinden, auf AAR-Ressourcen zugreifen.

Für den Zugriff auf eine Bildressource verwenden Sie den **Resource.Drawable** -Namen für das in der AAR-Datei gepackte Bild. Sie können beispielsweise mit `@drawable/image` auf **image.png** in der AAR-Datei verweisen:

```xml
<ImageView android:src="@drawable/image" ... />
```

Sie können auch auf Ressourcenlayouts zugreifen, die sich in der AAR-Datei befinden. Dazu verwenden Sie den **Resource.Layout**-Namen für das Layout, das in der AAR-Datei gepackt ist. Zum Beispiel:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Das **textanalyzer.aar**-Beispiel enthält die Bilddatei **res/drawable/monkey.png**. Diese Bildressource soll in unserer Beispiel-App verwendet werden:

Bearbeiten Sie das **BindingTest**-Layout (**Main.axml**), und fügen Sie `ImageView` am Ende des `LinearLayout`-Containers hinzu. Dieses `ImageView` zeigt das Bild unter **\@drawable/monkey** an, das aus dem Ressourcenabschnitt von **textanalyzer.aar** geladen wird:

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Kompilieren Sie das Projekt **BindingTest**, und führen Sie es aus. Die App wird gestartet und zeigt den Screenshot auf der linken Seite an. Wenn Sie auf **COUNT CONSONANTS** tippen, werden die Ergebnisse angezeigt (rechter Screenshot):

[![BindingTest zeigt die Anzahl der Konsonanten an](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine AAR-Java-Bibliothek gebunden!

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben Sie eine Bindungsbibliothek für eine AAR-Datei erstellt, die Bindungsbibliothek zu einer einfachen Test-App hinzugefügt und die App ausgeführt, um sich davon zu überzeugen, dass der C#-Code den Java-Code in der AAR-Datei aufrufen kann.
Außerdem wurde die App so erweitert, dass eine Bildressource angezeigt wird, die sich in der AAR-Datei befindet.

## <a name="related-links"></a>Verwandte Links

- [Erstellen einer Java-Bindungsbibliothek (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer JAR-Datei](~/android/platform/binding-java-library/binding-a-jar.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [AarBinding (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [Bug 44573 – One project cannot bind multiple .aar files](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
