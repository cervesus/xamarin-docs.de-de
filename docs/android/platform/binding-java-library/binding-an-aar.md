---
title: Binden von ein. AAR
description: Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android Java Bindungen Bibliothek aus einer Android. AAR-Datei.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 54708a7cfd071f77968991c9fe4e52938697c9bb
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="binding-an-aar"></a>Binden von ein. AAR

_Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android Java Bindungen Bibliothek aus einer Android. AAR-Datei._


## <a name="overview"></a>Übersicht

Die *Android Archiv (. AAR)* Datei ist das Dateiformat für Android-Bibliotheken.
Ein. AAR-Datei ist ein. ZIP-Archiv mit den folgenden Komponenten:

-   Kompilierte Java-code
-   Ressourcen-IDs
-   Ressourcen
-   Metadaten (z. B. aktivitätsdeklarationen, Berechtigungen)

In diesem Handbuch werden wir die Grundlagen der Erstellung einer Bibliothek Bindungen für eine einzelne durchlaufen. AAR-Datei. Einen Überblick über das Java Bibliothek binding im Allgemeinen (mit einem Beispiel für basic-Code) finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Ein bindungsprojekt kann nur eine enthalten. AAR-Datei. Wenn die. AAR Abhängigkeiten von anderen. AAR, und klicken Sie dann auf diese Abhängigkeiten sollten in einem eigenen bindungsprojekt enthalten sind und klicken Sie dann auf die verwiesen wird. Finden Sie unter [44573 Bug](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wir erstellen eine Bibliothek Bindungen für ein Beispiel für Android Archivdatei, die in Android Studio erstellten [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). Dies. AAR enthält eine `TextCounter` Klasse mit statischen Methoden, die die Anzahl der Vokale und Konsonanten in einer Zeichenfolge. Darüber hinaus **textanalyzer.aar** enthält eine Bildressource zum Anzeigen der Inventur Ergebnisse anzuzeigen.

Wir verwenden die folgenden Schritte zum Erstellen einer Bibliothek Bindungen aus der. AAR-Datei:

1. Erstellen Sie ein neues Klassenbibliothekprojekt mit Java-Bindungen.

2. Fügen Sie ein einzelnes hinzu. AAR-Datei zum Projekt. Ein bindungsprojekt darf nur ein einzelnes enthalten. AAR.

3. Legen Sie den entsprechenden Buildvorgang für die. AAR-Datei.

4. Wählen Sie ein Zielframework, die die. AAR unterstützt.

5. Erstellen der Bindungen-Bibliothek.

Nachdem wir die Bindungen Bibliothek erstellt haben, müssen wir eine kleine Android-app entwickeln, die fordert den Benutzer für eine Textzeichenfolge, die Aufrufe. AAR-Methoden zum Analysieren von Text, ruft das Bild aus der. AAR, und zeigt die Ergebnisse zusammen mit dem Bild.

Die Beispiel-app greift die `TextCounter` Abfrageklasse **textanalyzer.aar**:

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

Darüber hinaus diese Beispiel-app abrufen und anzeigen können eine Bildressource, die in gepackt ist **textanalyzer.aar**:

[![Xamarin Affe Bild](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Diese Ressource Image befindet sich auf **res/drawable/monkey.png** in **textanalyzer.aar**.



### <a name="creating-the-bindings-library"></a>Erstellen der Bindungen-Bibliothek

Bevor Sie mit den folgenden Schritten fortfahren, laden Sie das Beispiel [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android Archivdatei:

1.  Erstellen Sie ein neues Bindungen-Klassenbibliothekprojekt mit der Vorlage für die Bibliothek für Android Bindungen ab. Sie können Visual Studio für Mac oder Visual Studio (die in den folgenden Screenshots zeigen die Visual Studio, aber Visual Studio für Mac ist sehr ähnlich). Nennen Sie die Projektmappe **AarBinding**:

    [![AarBindings-Projekt erstellen](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  Die Vorlage enthält eine **JAR-Dateien** Ordner, in dem Sie hinzufügen, Ihrer. AAR(s) auf das Bindings-Steuerelementbibliothek-Projekt. Mit der rechten Maustaste die **JAR-Dateien** Ordner, und wählen **hinzufügen > Vorhandenes Element**:

    [![Vorhandenes Element hinzufügen](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  Navigieren Sie zu der **textanalyzer.aar** Datei zuvor heruntergeladen haben, auszuwählen, und klicken Sie auf **hinzufügen**:

    [![Textanalayzer.aar hinzufügen](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  Überprüfen Sie, ob die **textanalyzer.aar** Datei wurde erfolgreich dem Projekt hinzugefügt:

    [![Die Datei textanalyzer.aar wurde hinzugefügt.](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  Legen Sie den Buildvorgang für **textanalyzer.aar** auf `LibraryProjectZip`. In Visual Studio für Mac, mit der Maustaste **textanalyzer.aar** Buildvorgang festlegen. In Visual Studio den Buildvorgang kann festgelegt werden der **Eigenschaften** Bereich):

    [![Wenn den Buildvorgang textanalyzer.aar auf LibraryProjectZip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  Öffnen Sie das Projekt konfigurierbaren Eigenschaften der *Zielframework*. Wenn die. AAR alle Android-APIs verwendet, legen Sie das Zielframework auf API-Ebene, die. AAR erwartet. (Weitere Informationen zu den Zielframework-Einstellung und Android-API-Ebenen im Allgemeinen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).)

    Festlegen Sie das Ziel-API-Ebene für die Bibliothek Bindungen. In diesem Beispiel sind wir frei, um die neueste API-Ebene (API-Ebene 23)-Plattform verwenden, da unsere **Textanalyzer** verfügt nicht über eine Abhängigkeit für Android-APIs:

    [![Festlegen der Zielebene bis 23-API](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  Erstellen der Bindungen-Bibliothek. Das Bindings-Steuerelementbibliothek-Projekt sollte erfolgreich erstellt und eine Ausgabe aus. Die DLL an folgender Stelle: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>Mithilfe der Bindungen-Bibliothek

Um dies zu nutzen. Die DLL in Ihrer app Xamarin.Android, müssen Sie zunächst einen Verweis auf die Bibliothek Bindungen hinzufügen. Verwenden Sie die folgenden Schritte aus:

1.  Wir entwickeln dieser app in derselben Projektmappe wie die Bindungen Bibliothek an, in dieser exemplarischen Vorgehensweise zu vereinfachen. (Die app, die die Bindungen Bibliothek nutzt konnte auch in einer anderen Projektmappe befinden.) Erstellen Sie eine neue Xamarin.Android-app: mit der rechten Maustaste in der Projektmappe, und wählen Sie **neues Projekt hinzufügen**. Nennen Sie das neue Projekt **Namen "bindingtest"**:

    [![Erstellen Sie neue Namen "bindingtest"-Projekt](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  Mit der rechten Maustaste die **Verweise** Knoten der **Namen "bindingtest"** Projekt, und wählen Sie **Verweis hinzufügen...** :

    [![Klicken Sie auf Hinzufügen, Referenz](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  Wählen Sie die **AarBinding** Projekt zuvor erstellt haben, und klicken Sie auf **OK**:

    [![Überprüfen Sie die AAR bindungsprojekt](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  Öffnen der **Verweise** Knoten der **Namen "bindingtest"** Projekt zu überprüfen, ob die **AarBinding** Verweis vorhanden ist:

    [![AarBinding wird unter Verweise aufgeführt.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Wenn Sie den Inhalt der Bindung-Steuerelementbibliothek-Projekt anzeigen möchten, können Sie doppelklicken, den Verweis zum Öffnen der **Objektkatalog**. Sehen Sie den zugeordneten Inhalt der `Com.Xamarin.Textcounter` Namespace (zugeordnet, die von der Java- `com.xamarin.textanalyzezr` Paket) und Sie können die Mitglieder anzeigen der `TextCounter` Klasse:

[![Objektkatalog Anzeigen](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Die obige Screenshot werden hervorgehoben, die zwei `TextAnalyzer` Methoden, die die Beispiel-app aufruft: `NumConsonants` (die dient als Wrapper für der zugrunde liegenden Java `numConsonants` Methode), und `NumVowels` (die dient als Wrapper für der zugrunde liegenden Java `numVowels` Methode).



### <a name="accessing-aar-types"></a>Beim Zugriff auf. AAR-Typen

Nachdem Sie einen Verweis auf Ihre app, die auf der Bindung-Bibliothek verweist hinzufügen, können Sie zugreifen, Java-Typen in der. AAR, als Sie würden C#-Typen (Dank der C#-Wrapper) zugreifen. C#-app-Code aufrufen kann `TextAnalyzer` Methoden, wie im folgenden Beispiel dargestellt:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

Im obigen Beispiel entgegen statische Methoden der `TextCounter` Klasse. Sie können jedoch auch Klassen instanziiert und Instanzmethoden aufrufen. Z. B. Wenn Ihr. AAR dient als Wrapper für eine Klasse mit dem Namen `Employee` , die die Instanzmethode hat `buildFullName`, instanziieren Sie `MyClass` und verwenden sie wie hier:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Die folgenden Schritte fügen Sie Code für die app, damit die eingabeaufforderungen für Text, verwendet `TextCounter` auf den Text, und zeigt die Ergebnisse analysieren.

Ersetzen Sie die **Namen "bindingtest"** Layout (**Main.axml**) durch folgendes XML. Dieses Layout enthält eine `EditText` für Texteingabe und zwei Schaltflächen für das Initiieren von Vokal und Konsonanten Anzahlen:

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

Ersetzen Sie den Inhalt des **MainActivity.cs** durch den folgenden Code. Wie in diesem Beispiel sehen können, die Schaltfläche Ereignis-Handler-Aufruf umschlossen `TextCounter` Methoden, die sich in der. Popups für AAR und verwenden Sie zum Anzeigen der Ergebnisse. Beachten Sie, dass die `using` -Anweisung für den Namespace der gebundenen-Bibliothek (in diesem Fall `Com.Xamarin.Textcounter`):

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

Kompilieren und Ausführen der **Namen "bindingtest"** Projekt. Die app startet und präsentieren Sie den Screenshot auf der linken Seite (die `EditText` wird initialisiert, indem Text, aber Sie können tippen, um ihn zu ändern). Wenn Sie tippen Sie auf **Anzahl VOKALE**, ein Toast zeigt die Anzahl der Vokale, wie auf der rechten Seite dargestellt:

[![Bildschirmfotos mit Namen "bindingtest"](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Tippen Sie auf die **Anzahl KONSONANTEN** Schaltfläche. Darüber hinaus können Sie die Zeile des Texts zu ändern und tippen Sie auf diese Schaltflächen erneut aus, um verschiedene Vokal testen und Konsonant zählt.



### <a name="accessing-aar-resources"></a>Beim Zugriff auf. AAR-Ressourcen

Die Xamarin-Tools Zusammenführungen der **R** Daten aus der. AAR in Ihrer app **Ressource** Klasse. Daher können Sie zugreifen. AAR Ressourcen von Ihr Layout (und Code-Behind) in der gleichen Weise, wie Sie Ressourcen zugreifen, die in der **Ressourcen** Pfad Ihres Projekts.

Um auf eine Bildressource zuzugreifen, verwenden Sie die **Resource.Drawable** Namen für das Bild zurück, der innerhalb der. AAR. Sie können z. B. verweisen **image.png** in der. AAR-Datei mit `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Sie können auch Ressourcen-Layouts, mit denen in befinden zugreifen der. AAR. Zu diesem Zweck verwenden Sie die **Resource.Layout** Namen für das Layout in verpackten der. AAR. Zum Beispiel:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Die **textanalyzer.aar** Beispiel enthält eine Bilddatei, die am befindet **res/drawable/monkey.png**. Wir Zugriff auf diese Bildressource und deren Verwendung in unserem Beispiel-app:

Bearbeiten der **Namen "bindingtest"** Layout (**Main.axml**) und fügen eine `ImageView` bis zum Ende der `LinearLayout` Container. Dies `ImageView` zeigt das Bild finden Sie unter **@drawable/monkey**; dieses Bild wird im Abschnitt "Ressourcen" der geladen **textanalyzer.aar**:

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

Kompilieren und Ausführen der **Namen "bindingtest"** Projekt. Die app startet und präsentieren Sie den Screenshot auf der linken Seite &ndash; beim Tippen auf **Anzahl KONSONANTEN**, die Ergebnisse werden angezeigt, wie auf der rechten Seite dargestellt:

[![Namen "bindingtest" Einlagensicherungssystem anschließt Anzahl anzeigen](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Herzlichen Glückwunsch! Sie haben erfolgreich eine Java-Bibliothek gebunden. AAR!



## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erstellt es eine Bibliothek mit Bindungen ein. AAR-Datei einen minimalen Test-app der Bindungen-Bibliothek hinzugefügt und Ausführung die app, um sicherzustellen, dass unsere C#-Code in den Java-Code aufrufen kann die. AAR-Datei.
Darüber hinaus wurde erweitert, die app, um den Zugriff und können eine Bildressource, die befindet angezeigt der. AAR-Datei.


## <a name="related-links"></a>Verwandte Links

- [Zum Erstellen einer Bibliothek für Java-Bindungen (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer JAR-Datei](~/android/platform/binding-java-library/binding-a-jar.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [AarBinding (Beispiel)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [Fehler 44573: 1-Projekt kann nicht mehrere aar-Dateien gebunden werden.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
