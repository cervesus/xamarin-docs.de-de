---
title: Binden einer AAR-Datei
description: Diese exemplarische Vorgehensweise enthält Schritt-für-Schritt-Anleitungen zum Erstellen einer xamarin. Android-Java-Bindungs Bibliothek von einem Android. AAR-Datei.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: cdd68def895fea362d9ad3147e3d622471d73a63
ms.sourcegitcommit: 4ff181101d76f048b949c9613b2c72cf02618f8b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "71994891"
---
# <a name="binding-an-aar"></a>Binden einer AAR-Datei

_Diese exemplarische Vorgehensweise enthält Schritt-für-Schritt-Anleitungen zum Erstellen einer xamarin. Android-Java-Bindungs Bibliothek von einem Android. AAR-Datei._

## <a name="overview"></a>Übersicht

Das *Android-Archiv (. Aar)* ist das Dateiformat für Android-Bibliotheken.
Halbe. Die Aar-Datei ist ein. ZIP-Archiv, das Folgendes enthält:

- Kompilierter Java-Code
- Ressourcen-IDs
- Ressourcen
- Metadatendaten (z. b. Aktivitäts Deklarationen, Berechtigungen)

In dieser Anleitung werden die Grundlagen der Erstellung einer Bindungs Bibliothek für einen einzelnen Schritt erläutert. AAR-Datei. Eine Übersicht über die Java-Bibliotheks Bindung im allgemeinen (mit einem einfachen Codebeispiel) finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md).

> [!IMPORTANT]
> Ein Bindungs Projekt kann nur eines enthalten. AAR-Datei. , Wenn die. AAR-Abhängigkeiten von anderen. Aar, dann sollten diese Abhängigkeiten in Ihrem eigenen Bindungs Projekt enthalten sein und dann referenziert werden. Siehe [Bug 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wir erstellen eine Bindungs Bibliothek für ein Beispiel für eine Android-Archivdatei, die in Android Studio, [Textanalyzer. Aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true), erstellt wurde. Dafür. AAR enthält eine `TextCounter`-Klasse mit statischen Methoden, die die Anzahl der Vokale und Konsonanten in einer Zeichenfolge zählen. Außerdem enthält **Textanalyzer. Aar** eine Bildressource, mit der die Zählergebnisse angezeigt werden können.

Wir verwenden die folgenden Schritte, um eine Bindungs Bibliothek aus der zu erstellen. AAR-Datei:

1. Erstellen Sie ein neues Java-Bindungs Bibliotheksprojekt.

2. Fügen Sie ein einzelnes hinzu. AAR-Datei zum Projekt. Ein Bindungs Projekt darf nur ein einzelnes enthalten. Aar.

3. Legen Sie die entsprechende Buildaktion für das fest. AAR-Datei.

4. Wählen Sie das Ziel Framework aus. AAR unterstützt.

5. Erstellen Sie die Bindungs Bibliothek.

Nachdem wir die Bindungs Bibliothek erstellt haben, entwickeln wir eine kleine Android-App, die den Benutzer zur Eingabe einer Text Zeichenfolge auffordert. AAR-Methoden, um den Text zu analysieren, ruft das Bild aus ab. Aar und zeigt die Ergebnisse zusammen mit dem Bild an.

Die Beispiel-App greift auf die `TextCounter`-Klasse von **Textanalyzer. Aar**zu:

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

Außerdem wird in dieser Beispiel-App eine Bildressource abgerufen und angezeigt, die in **Textanalyzer. Aar**verpackt ist:

[![xamarin-affabbild](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Diese Bildressource befindet sich in " **Textanalyzer. Aar**" in " **Res/drawable/Monkey. png** ".

### <a name="creating-the-bindings-library"></a>Erstellen der Bindungs Bibliothek

Bevor Sie mit den folgenden Schritten beginnen, laden Sie die Beispieldatei " [Textanalyzer. Aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android Archive" herunter:

1. Erstellen Sie ein neues Bindungs Bibliotheksprojekt, das mit der Vorlage Android-Bindungs Bibliothek beginnt. Sie können entweder Visual Studio für Mac oder Visual Studio verwenden (die folgenden Screenshots zeigen Visual Studio, aber Visual Studio für Mac ist sehr ähnlich). Nennen Sie die Projekt Mappe " **aarbinding**":

    [![Create-Projekt "aarbindungen"](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. Die Vorlage **enthält einen jar** -Ordner, in dem Sie Ihre hinzufügen. Aar (s) zum Bindungs Bibliotheksprojekt. Klicken Sie mit der rechten Maustaste auf den Ordner **Jars** , und wählen Sie dann **> vorhandenes Element**

    [![Vorhandenes Element hinzufügen](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. Navigieren Sie zur zuvor heruntergeladenen Datei **Textanalyzer. Aar** , wählen Sie Sie aus, und klicken Sie auf **Hinzufügen**:

    [![fügen Sie textanalayzer. Aar hinzu.](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. Vergewissern Sie sich, dass die Datei **Textanalyzer. Aar** dem Projekt erfolgreich hinzugefügt wurde:

    [![Die Datei "Textanalyzer. Aar" wurde hinzugefügt.](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. Legen Sie die Buildaktion für **Textanalyzer. Aar** auf `LibraryProjectZip` fest. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Textanalyzer. Aar** , um die Buildaktion festzulegen. In Visual Studio kann die Buildaktion im Bereich " **Eigenschaften** " festgelegt werden:

    [![setting the Textanalyzer. Aar Build Action to libraryprojectzip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. Öffnen Sie die Projekteigenschaften, um das *Ziel Framework*zu konfigurieren. , Wenn die. AAR verwendet alle Android-APIs, legen Sie das Ziel Framework auf die API-Ebene fest, auf der das. AAR erwartet. (Weitere Informationen zur Ziel Framework-Einstellung und den Android-API-Ebenen im Allgemeinen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).)

    Legen Sie die Ziel-API-Ebene für Ihre Bindungs Bibliothek fest. In diesem Beispiel können Sie die neueste Plattform-API-Ebene (API-Ebene 23) verwenden, da der **Textanalyzer** nicht von Android-APIs abhängig ist:

    [![festlegen der Zielebene auf API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. Erstellen Sie die Bindungs Bibliothek. Das Bindungs Bibliotheksprojekt sollte erfolgreich erstellt werden und eine Ausgabe erstellen. DLL an folgendem Speicherort: **' Aarbinding/bin/Debug/'**

### <a name="using-the-bindings-library"></a>Verwenden der Bindungs Bibliothek

, Um dieses zu verwenden. DLL in ihrer xamarin. Android-App müssen Sie zuerst einen Verweis auf die Bindungs Bibliothek hinzufügen. Führen Sie die folgenden Schritte aus:

1. Diese APP wird in derselben Projekt Mappe wie die Bindungs Bibliothek erstellt, um diese exemplarische Vorgehensweise zu vereinfachen. (Die APP, die die Bindungs Bibliothek verwendet, kann sich auch in einer anderen Projekt Mappe befinden.) Erstellen einer neuen xamarin. Android-App: Klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Neues Projekt hinzufügen** Benennen Sie das neue Projekt " **bindingtest**":

    [![Neues bindingtest-Projekt erstellen](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. Klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** des Projekts **bindingtest** , und wählen Sie **Verweis hinzufügen...** aus:

    [![ Klicken Sie auf Verweis hinzufügen.](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. Wählen Sie das zuvor erstellte Projekt **aarbinding** aus, und klicken Sie auf **OK**:

    [![Das Aar-Bindungs Projekt überprüfen](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. Öffnen Sie den Knoten **Verweise** des Projekts **bindingtest** , um zu überprüfen, ob **der Verweis auf** die verweisende Datei vorhanden ist:

    [![aarbinding ist unter "Verweise" aufgeführt.](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

Wenn Sie den Inhalt des Bindungs Bibliotheks Projekts anzeigen möchten, können Sie auf den Verweis doppelklicken, um ihn im **Objektkatalog**zu öffnen. Sie können den zugeordneten Inhalt des `Com.Xamarin.Textcounter`-Namespace sehen (zugeordnet aus dem Java-`com.xamarin.textanalyzezr`-Paket), und Sie können die Member der `TextCounter`-Klasse anzeigen:

[![Anzeigen der Objektkatalog](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Der obige Screenshot hebt die zwei `TextAnalyzer`-Methoden hervor, die von der Beispiel-app aufgerufen werden: `NumConsonants` (das die zugrunde liegende Java `numConsonants`-Methode umschließt) und `NumVowels` (das die zugrunde liegende Java `numVowels`-Methode umschließt).

### <a name="accessing-aar-types"></a>Den. AAR-Typen

Nachdem Sie einen Verweis auf die-app hinzugefügt haben, der auf die Bindungs Bibliothek verweist, können Sie in auf Java-Typen zugreifen. AAR wie Sie auf Typen C# zugreifen würden (Dank der C# Wrapper). C#App-Code kann `TextAnalyzer`-Methoden wie im folgenden Beispiel gezeigt aufgerufen werden:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

Im obigen Beispiel rufen wir statische Methoden in der `TextCounter`-Klasse auf. Sie können jedoch auch Klassen instanziieren und Instanzmethoden aufzurufen. Beispielsweise, wenn Ihre. AAR umschließt eine Klasse mit dem Namen "`Employee`", die die Instanzmethode `buildFullName` aufweist. Sie können `MyClass` instanziieren und verwenden, wie hier zu sehen ist:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Mit den folgenden Schritten wird der app Code hinzugefügt, sodass der Benutzer zur Eingabe von Text aufgefordert wird, `TextCounter` verwendet wird, um den Text zu analysieren, und dann die Ergebnisse anzeigt.

Ersetzen Sie das **bindingtest** -Layout (**Main. axml**) durch den folgenden XML-Code. Dieses Layout verfügt über eine `EditText` für Texteingabe und zwei Schaltflächen zum Initiieren von Vokal-und Konsonanten Anzahlen:

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

Ersetzen Sie den Inhalt von **MainActivity.cs** durch den folgenden Code. Wie in diesem Beispiel gezeigt, werden von den Schaltflächen-Ereignis Handlern umschließende `TextCounter`-Methoden aufgerufen, die sich in befinden. Aar und verwenden Sie zum Anzeigen der Ergebnisse die Ergebnisse. Beachten Sie die `using`-Anweisung für den Namespace der gebundenen Bibliothek (in diesem Fall `Com.Xamarin.Textcounter`):

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

Kompilieren Sie das Projekt **bindingtest** , und führen Sie es aus. Die APP wird gestartet und zeigt den Screenshot auf der linken Seite an (der `EditText` wird mit Text initialisiert, Sie können ihn aber tippen, um ihn zu ändern). Wenn Sie auf **Anzahl Vokale**tippen, zeigt ein Toast die Anzahl von Vokalen an, die auf der rechten Seite angezeigt werden:

[![ Screenshots aus dem Ausführen von bindingtest](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Tippen Sie auf die Schaltfläche **Zähler Konsonanten** . Außerdem können Sie die Textzeile ändern und erneut auf diese Schaltflächen tippen, um eine andere Anzahl von Vokal-und Konsonanten zu testen.

### <a name="accessing-aar-resources"></a>Den. AAR-Ressourcen

Die xamarin-Tools werden die **R** -Daten aus der zusammengeführt. Sie werden in die **Ressourcen** Klasse Ihrer APP eingeteilt. Daher können Sie auf zugreifen. AAR-Ressourcen aus dem Layout (und Code Behind) auf die gleiche Weise wie Sie auf Ressourcen zugreifen, die sich im **Ressourcen** Pfad Ihres Projekts befinden.

Für den Zugriff auf eine Bildressource verwenden Sie den **Resource. drawable** -Namen für das Bild, das in der verpackt ist. Aar. Beispielsweise können Sie in der auf " **Image. png** " verweisen. AAR-Datei mit `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Sie können auch auf Ressourcen Layouts zugreifen, die sich in befinden. Aar. Zu diesem Zweck verwenden Sie den **Resource. Layout** -Namen für das Layout, das in der verpackt ist. Aar. Zum Beispiel:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

Das **Textanalyzer. Aar** -Beispiel enthält eine Bilddatei, die sich in " **Res/drawable/Monkey. png**" befindet. Wir greifen auf diese Bildressource zu und verwenden Sie in unserer Beispiel-App:

Bearbeiten Sie das **bindingtest** -Layout (**Main. axml**), und fügen Sie am Ende des `LinearLayout`-Containers eine `ImageView` hinzu. Dieses `ImageView` zeigt das Bild an **\@drawable/Monkey**; Dieses Bild wird aus dem Ressourcenabschnitt von **Textanalyzer. Aar**geladen:

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

Kompilieren Sie das Projekt **bindingtest** , und führen Sie es aus. Die APP wird gestartet und zeigt den Screenshot auf der linken Seite an &ndash; Wenn Sie auf **Anzahl Konsonanten**tippen, werden die Ergebnisse wie auf der rechten Seite angezeigt:

[![bindingtest zeigt die Anzahl der Konsonanten an](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

Herzlichen Glückwunsch! Sie haben erfolgreich eine Java-Bibliothek gebunden. Aar!

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben wir eine Bindungs Bibliothek für erstellt. AAR-Datei, Hinzufügen der Bindungs Bibliothek zu einer minimalen Test-App und durchlaufen der APP, C# um zu überprüfen, ob der Code Java-Code in der-Datei abrufen kann. AAR-Datei.
Außerdem haben wir die APP für den Zugriff auf und die Anzeige einer Bildressource erweitert, die sich in befindet. AAR-Datei.

## <a name="related-links"></a>Verwandte Links

- [Aufbauen einer Java-Bindungs Bibliothek (Video)](https://university.xamarin.com/classes#10090)
- [Binden einer JAR-Datei](~/android/platform/binding-java-library/binding-a-jar.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [Aarbinding (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [Fehler 44573-ein Projekt kann nicht mehrere. Aar-Dateien binden.](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
