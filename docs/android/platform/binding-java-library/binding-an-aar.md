---
title: Binden einer AAR-Datei
description: Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek von Android. Zusätzlich zum AAR-Datei.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 7f71ccf4ff61c176e73be6d3855136697a5c2130
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60958106"
---
# <a name="binding-an-aar"></a>Binden einer AAR-Datei

_Diese exemplarische Vorgehensweise enthält schrittweise Anleitungen zum Erstellen einer Xamarin.Android-Java-Bindungsbibliothek von Android. Zusätzlich zum AAR-Datei._


## <a name="overview"></a>Übersicht

Die *Android-Archiv (. Zusätzlich zum AAR)* Datei ist das Dateiformat für Android-Bibliotheken verwenden.
Ein. Zusätzlich zum AAR-Datei ist ein. ZIP-Archiv, das Folgendes enthält:

-   Kompilierten Java-code
-   Ressourcen-IDs
-   Ressourcen
-   Metadaten (z. B. aktivitätsdeklarationen, Berechtigungen)

In diesem Handbuch werden wir die Grundlagen der Erstellung einer Bibliothek Bindungen für eine einzelne durchlaufen. Zusätzlich zum AAR-Datei. Eine Übersicht über Java Bibliothek Bindung in der Regel (mit einem einfachen Codebeispiel), finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Ein bindungsprojekt kann nur eine enthalten. Zusätzlich zum AAR-Datei. Wenn die. Zusätzlich zum AAR-Abhängigkeiten auf anderen. Zusätzlich zum AAR, und klicken Sie dann auf diese Abhängigkeiten enthalten, die in einem eigenen bindungsprojekt und klicken Sie dann auf die verwiesen wird. Finden Sie unter [Fehler 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wir erstellen eine Bibliothek für die Bindungen für ein Beispiel für Android-Archiv-Datei, die in Android Studio erstellt wurde [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). Dies. Zusätzlich zum AAR enthält eine `TextCounter` Klasse mit statischen Methoden, die die Anzahl der Vokale und Konsonanten in einer Zeichenfolge zu zählen. Darüber hinaus **textanalyzer.aar** enthält eine Bildressource, um die Zählung Ergebnisse anzuzeigen.

Wir verwenden die folgenden Schritte zum Erstellen einer Bibliothek Bindungen aus der. Zusätzlich zum AAR-Datei:

1. Erstellen Sie ein neues Java Bindungsbibliothek-Projekt.

2. Fügen Sie ein einzelnes hinzu. Zusätzlich zum AAR-Datei zum Projekt. Ein bindungsprojekt darf nur ein einzelnes enthalten. ZUSÄTZLICH ZUM AAR.

3. Legen Sie den entsprechenden Buildvorgang für die. Zusätzlich zum AAR-Datei.

4. Wählen Sie ein Zielframework, die die. Zusätzlich zum AAR unterstützt.

5. Erstellen Sie die Bindungsbibliothek.

Nachdem wir die Bibliothek Bindungen erstellt haben, werden wir eine kleine Android-app entwickeln, die für eine Textzeichenfolge, die Aufrufe der eingabeaufforderungen. Zusätzlich zum AAR-Methoden zum Analysieren des Texts, ruft das Bild aus der. Zusätzlich zum AAR, und zeigt die Ergebnisse zusammen mit dem Bild.

Die Beispiel-app greift die `TextCounter` Klasse **textanalyzer.aar**:

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

Darüber hinaus diese Beispiel-app auf und zeigt eine Bildressource, die in gepackt ist **textanalyzer.aar**:

[![Xamarin-Monkey-Bild](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Dieses Image-Ressource befindet sich **res/drawable/monkey.png** in **textanalyzer.aar**.



### <a name="creating-the-bindings-library"></a>Erstellen der Bindungsbibliothek

Laden Sie das Beispiel, bevor Sie mit den nächsten Schritten fortfahren, [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android-Archiv-Datei:

1.  Erstellen Sie ein neues Bindings Library-Projekt, das mit der Android-Bindungsbibliothek Vorlage ab. Sie können Visual Studio für Mac oder Visual Studio (die folgenden Screenshots zeigen Visual Studio, aber Visual Studio für Mac ist sehr ähnlich). Nennen Sie die Projektmappe **AarBinding**:

    [![Erstellen Sie AarBindings-Projekt](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  Die Vorlage enthält einen **Jars** Ordner, in dem Sie sich hinzufügen, Ihrer. AAR(s) auf das Bindings-Bibliotheksprojekt hinzu. Mit der rechten Maustaste die **Jars** Ordner, und wählen **hinzufügen > Vorhandenes Element**:

    [![Vorhandenes Element hinzufügen](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  Navigieren Sie zu der **textanalyzer.aar** Datei zuvor heruntergeladen haben, wählen Sie ihn, und klicken Sie auf **hinzufügen**:

    [![Hinzufügen von textanalayzer.aar](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  Überprüfen Sie, ob die **textanalyzer.aar** Datei wurde erfolgreich dem Projekt hinzugefügt:

    [![Die Datei textanalyzer.aar wurde hinzugefügt](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  Legen Sie den Buildvorgang für **textanalyzer.aar** zu `LibraryProjectZip`. In Visual Studio für Mac, mit der Maustaste **textanalyzer.aar** die Build-Aktion festlegen. In Visual Studio kann die Build-Aktion kann festgelegt werden der **Eigenschaften** Bereich):

    [![Die Buildaktion textanalyzer.aar festlegen auf LibraryProjectZip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  Öffnen Sie das Projekt Eigenschaften zum Konfigurieren der *Zielframework*. Wenn die. Zusätzlich zum AAR wird jede Android-APIs verwendet, legen Sie das Zielframework auf API-Ebene, die die. Zusätzlich zum AAR erwartet. (Weitere Informationen zu den Zielframework-Einstellung und Android-API-Ebenen im Allgemeinen finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).)

    Legen Sie das Ziel-API-Ebene für Ihre Bindungen-Bibliothek. In diesem Beispiel werden die neueste API-Ebene (API-Ebene 23)-Plattform verwenden, da unsere **Textanalyzer** verfügt nicht über eine Abhängigkeit für Android-APIs:

    [![Durch Festlegen der Zielebene auf API-23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  Erstellen Sie die Bindungsbibliothek. Sollte das Bindings-Steuerelementbibliothek-Projekt erfolgreich erstellt werden und eine Ausgabe generieren. Die DLL an folgendem Speicherort: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>Mithilfe der Bindings-Bibliothek

Um dies zu nutzen. Die DLL in Ihrer Xamarin.Android-app, müssen Sie zuerst einen Verweis auf das Bindings-Bibliothek hinzufügen. Verwenden Sie die folgenden Schritte aus:

1.  Wir erstellen diese app in der gleichen Projektmappe wie der Bindings-Bibliothek in dieser exemplarischen Vorgehensweise zu vereinfachen. (Die app, die die Bindungsbibliothek nutzt kann auch in einer anderen Projektmappe befinden.) Erstellen einer neuen Xamarin.Android-app: mit der rechten Maustaste in der Projektmappe, und wählen Sie **neues Projekt hinzufügen**. Nennen Sie das neue Projekt **BindingTest**:

    [![Erstellen Sie neues BindingTest-Projekt](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  Mit der rechten Maustaste die **Verweise** Knoten die **BindingTest** Projekt, und wählen **Verweis hinzufügen...** :

    [![Klicken Sie auf Hinzufügen, Referenz](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  Wählen Sie die **AarBinding** zuvor erstellte Projekt, und klicken Sie auf **OK**:

    [![Überprüfen Sie das Projekt der AAR-Bindung](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  Öffnen der **Verweise** Knoten die **BindingTest** Projekt, um zu überprüfen, ob die **AarBinding** Verweis vorhanden ist:

    [![AarBinding wird unter Verweise aufgeführt.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Wenn Sie den Inhalt des Binding-Library-Projekts anzeigen möchten, können Sie den Verweis zum Öffnen doppelklicken dem **Objektkatalog**. Sehen Sie den zugeordneten Inhalt, der die `Com.Xamarin.Textcounter` Namespace (zugeordnet, die von der Java- `com.xamarin.textanalyzezr` Paket) und Sie können die Mitglieder anzeigen der `TextCounter` Klasse:

[![Objektkatalog Anzeigen](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Im obigen Screenshot hervorgehoben, die beiden `TextAnalyzer` Methoden, die die Beispiel-app aufruft: `NumConsonants` (die dient als Wrapper für der zugrunde liegenden Java `numConsonants` Methode), und `NumVowels` (die dient als Wrapper für der zugrunde liegenden Java `numVowels` Methode).



### <a name="accessing-aar-types"></a>Zugriff auf. Zusätzlich zum AAR-Typen

Nachdem Sie einen Verweis auf Ihre app, die auf der Bindung-Bibliothek verweist hinzufügen, erreichen Sie Java-Typen in der. Zusätzlich zum AAR, wie Sie greifen C# Typen (vielen Dank an die C# Wrapper). C#App-Code aufrufen kann `TextAnalyzer` Methoden, wie in diesem Beispiel dargestellt:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

Im obigen Beispiel rufen wir statische Methoden der `TextCounter` Klasse. Sie können jedoch auch Klassen instanziieren und Aufrufen von Instanzmethoden. Beispielsweise wenn Ihre. Zusätzlich zum AAR dient als Wrapper für eine Klasse namens `Employee` , die die Instanzmethode hat `buildFullName`, instanziieren Sie `MyClass` und verwenden sie wie hier:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Die folgenden Schritte fügen Sie Code für die app, damit sie die eingabeaufforderungen für Text, verwendet `TextCounter` der Text, und zeigt die Ergebnisse analysieren.

Ersetzen Sie die **BindingTest** Layout (**Main.axml**) durch das folgende XML. Dieses Layout verfügt über eine `EditText` für die Texteingabe und zwei Schaltflächen für die Anzahl von Vokal und Konsonanten initiieren:

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

Ersetzen Sie den Inhalt der **"mainactivity.cs"** durch den folgenden Code. Wie in diesem Beispiel wird dargestellt, mit der Schaltfläche Event Handler-Aufruf umschlossen `TextCounter` Methoden, die im befinden die. AAR und verwenden Sie Popupbenachrichtigungen verwendet, um die Ergebnisse anzuzeigen. Beachten Sie, dass die `using` -Anweisung für den Namespace der gebundenen-Bibliothek (in diesem Fall `Com.Xamarin.Textcounter`):

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

Kompilieren und Ausführen der **BindingTest** Projekt. Die app startet und darstellen im Screenshot auf der linken Seite (die `EditText` wird initialisiert, indem der Text, aber Sie können durch Tippen, um ihn zu ändern). Wenn Sie tippen Sie auf **Anzahl VOKALE**, ein Popup zeigt die Anzahl der Vokale aus, wie auf der rechten Seite dargestellt:

[![Screenshots aus BindingTest ausführen](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Tippen Sie auf die **Anzahl KONSONANTEN** Schaltfläche. Außerdem können Sie die Zeile des Texts zu ändern, und tippen Sie auf diese Schaltfläche erneut aus, um verschiedene Vokal testen und Konsonant zählt.



### <a name="accessing-aar-resources"></a>Zugriff auf. Zusätzlich zum AAR-Ressourcen

Das Xamarin-Tools Zusammenführungen der **R** Daten aus der. Zusätzlich zum AAR in Ihrer app **Ressource** Klasse. Daher können Sie zugreifen. Zusätzlich zum AAR-Ressourcen aus Ihrem Layout (und Code-Behind) in der gleichen Weise, wie Sie auf Ressourcen zugreifen sollen, die in der **Ressourcen** Pfad Ihres Projekts.

Um eine Bildressource zuzugreifen, verwenden Sie die **Resource.Drawable** Namen für das Abbild in ist die. ZUSÄTZLICH ZUM AAR. Sie können z. B. verweisen **image.png** in der. Zusätzlich zum AAR-Datei mit `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Sie können auch zugreifen, Resource-Layouts, mit denen in befinden die. ZUSÄTZLICH ZUM AAR. Zu diesem Zweck verwenden Sie die **Resource.Layout** Name für das Layout in verpackten der. ZUSÄTZLICH ZUM AAR. Zum Beispiel:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Die **textanalyzer.aar** Beispiel enthält eine Bilddatei, die sich unter befindet **res/drawable/monkey.png**. Lassen Sie uns Zugriff auf diese imageressource, und verwenden Sie sie in unserem Beispiel-app:

Bearbeiten der **BindingTest** Layout (**Main.axml**) und fügen eine `ImageView` bis zum Ende der `LinearLayout` Container. Dies `ImageView` zeigt das Bild, finden Sie unter **@drawable/monkey**; dieses Image wird im Ressourcenabschnitt "der geladen **textanalyzer.aar**:

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

Kompilieren und Ausführen der **BindingTest** Projekt. Die app startet und darstellen im Screenshot auf der linken Seite &ndash; Sie tippen, wenn **Anzahl KONSONANTEN**, die Ergebnisse werden angezeigt, wie auf der rechten Seite dargestellt:

[![BindingTest Einlagensicherungssystem anschließt Anzahl anzeigen](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Herzlichen Glückwunsch! Sie haben erfolgreich eine Java-Bibliothek gebunden. ZUSÄTZLICH ZUM AAR!



## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise, die wir erstellt eine Bibliothek mit Bindungen ein. Zusätzlich zum AAR-Datei, die Bindungsbibliothek eine minimale app hinzugefügt, und die app aus, um sicherzustellen, dass ausgeführt unsere C# Code kann sich befinden, der Java-Code Aufrufen der. Zusätzlich zum AAR-Datei.
Darüber hinaus wir die app zum Zugriff auf und zeigt eine Bildressource, die befindet erweitert die. Zusätzlich zum AAR-Datei.


## <a name="related-links"></a>Verwandte Links

- [Erstellen einer Java Bindungsbibliothek (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer JAR-Datei](~/android/platform/binding-java-library/binding-a-jar.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [AarBinding (Beispiel)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [Fehler 44573: 1-Projekt kann nicht mehrere aar-Dateien gebunden werden.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
