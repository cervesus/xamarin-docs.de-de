---
title: "Schnellstart für „Hallo, Android“-Multiscreen"
description: "In diesem zweiteiligen Handbuch wird die Phoneword-Anwendung, um einen zweiten Bildschirm erweitert. Gleichzeitig werden die grundlegenden Bausteine von Android-Anwendungen erläutert und sie erhalten einen tieferen Einblick in die Architektur von Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 4c61a588eafdf0a86f4124d264c41cabef3e7a14
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="hello-android-multiscreen-quickstart"></a>Schnellstart für „Hallo, Android“-Multiscreen

_In diesem zweiteiligen Leitfaden wird die Phoneword-Anwendung, um einen zweiten Bildschirm erweitert. Gleichzeitig werden die grundlegenden Bausteine von Android-Anwendungen erläutert, und Sie erhalten einen tieferen Einblick in die Architektur von Android._

## <a name="hello-android-multiscreen-quickstart"></a>Schnellstart für „Hallo, Android“-Multiscreen

Im Teil des Handbuchs mit der exemplarischen Vorgehensweise fügen Sie einen zweiten Bildschirm zur [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/)-Anwendung hinzu, um mithilfe der App den Verlauf der übersetzten Nummern nachzuverfolgen. Die [endgültige Anwendung](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen/) verfügt dann über einen zweiten Bildschirm, auf dem wie im Screenshot auf der rechten Seite veranschaulicht die übersetzten Nummern angezeigt werden:

[![Screenshots der Beispiel-App](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png)

Im begleitenden Artikel [Hello, Android Multiscreen: Deep Dive (Hallo, Android: Ausführliche Erläuterungen)](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md) wird wiederholt, was erstellt wurde, und es werden die Architektur, die Navigation und andere neue Android-Konzepte diskutiert.


## <a name="requirements"></a>Anforderungen

Da dieses Handbuch dort ansetzt, wo [Hallo, Android](~/android/get-started/hello-android/index.md) aufhört, muss der [Schnellstart für „Hallo, Android“](~/android/get-started/hello-android/hello-android-quickstart.md) abgeschlossen werden.
Wenn Sie direkt mit der exemplarischen Vorgehensweise weiter unten beginnen möchten, können Sie die vollständige Version von [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) aus dem Artikel Schnellstart für „Hallo, Android“ herunterladen und damit die exemplarische Vorgehensweise durcharbeiten.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise fügen Sie der **Phoneword**-Anwendung einen Bildschirm für den **Übersetzungsverlauf** hinzu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Öffnen Sie zunächst die **Phoneword**-Anwendung in Visual Studio, und bearbeiten Sie die Datei **Main.axml** über den **Projektmappen-Explorer**.

### <a name="updating-the-layout"></a>Aktualisieren des Layouts

Ziehen Sie in der **Toolbox** eine **Schaltfläche** auf die Designoberfläche, und platzieren Sie sie unterhalb der TextView **TranslatedPhoneWord**. Ändern Sie im Bereich **Eigenschaften** die **ID** der Schaltfläche in `@+id/TranslationHistoryButton`. 

[![Hineinziehen einer neuen Schaltfläche](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png)

Legen Sie die Eigenschaft **Text** der Schaltfläche auf `@string/translationHistory` fest. Android Designer wird dies wörtlich interpretieren, aber Sie werden ein paar Änderungen vornehmen, sodass der Text der Schaltfläche richtig angezeigt wird:

[![Festlegen des Schaltflächentexts für „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png)

Erweitern Sie den Knoten **Werte** unter dem Ordner **Ressourcen** im **Projektmappen-Explorer**, und doppelklicken Sie auf die Datei für Zeichenfolgenressourcen **Strings.xml**:

[![Öffnen von „Strings.xml“](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png)

Fügen Sie den Namen und den Wert der Zeichenfolge `translationHistory` zur Datei **Strings.xml** hinzu, und speichern Sie sie:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

Der Schaltflächentext für **Übersetzungsverlauf** sollte aktualisiert werden, um den neuen Zeichenfolgenwert widerzuspiegeln:

[![Schaltfläche mit übernommenem neuen Zeichenfolgenwert](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)

Wenn Sie auf der Designoberfläche auf die Schaltfläche **Übersetzungsverlauf** geklickt haben, suchen Sie im Pad **Eigenschaften** die Einstellung `enabled`, und legen Sie ihren Wert auf `false` fest, um die Schaltfläche zu deaktivieren. Dadurch wird die Schaltfläche in der Designoberfläche dunkler:

[![Deaktivieren der Schaltfläche „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png)

### <a name="creating-the-second-activity"></a>Erstellen der zweiten Aktivität

Erstellen Sie eine zweite Aktivität, um den zweiten Bildschirm einzurichten. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…**:

[![Hinzufügen einer neuen Datei](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png)

Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C# > Aktivität**, und nennen Sie die Aktivitätsdatei **TranslationHistoryActivity.cs**.

Ersetzen Sie den Vorlagencode in **TranslationHistoryActivity.cs** durch Folgendes:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

In dieser Klasse erstellen Sie eine `ListActivity` und füllen sie programmgesteuert, damit Sie keine neue Layoutdatei für diese Aktivität erstellen müssen. Dies wird unter [Hello, Android Multiscreen Deep Dive („Hallo, Android“-Multiscreen – Ausführliche Erläuterungen)](~/android/get-started/hello-android/hello-android-deepdive.md) genauer erläutert.

### <a name="adding-translation-history-code"></a>Hinzufügen des Übersetzungsverlaufcodes

Diese App sammelt Telefonnummern, die der Benutzer im ersten Bildschirm übersetzt hat, und übergibt sie an den zweiten Bildschirm. Die Telefonnummern werden als Liste von Zeichenfolgen gespeichert. Fügen Sie am Anfang der Klasse `MainActivity` die folgende `using`-Anweisung hinzu, um die Listen zu unterstützen:

```csharp
using System.Collections.Generic;
```

Erstellen Sie als Nächstes eine leere Liste, die mit Telefonnummern befüllt werden kann.
Die Klasse `MainActivity` sieht nun wie folgt aus:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

Fügen Sie den folgenden Code in der Klasse `MainActivity` hinzu, um die Schaltfläche **Übersetzungsverlauf** zu registrieren (erstellen Sie diese Zeile nach der Deklaration `translationHistory`):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Fügen Sie den folgenden Code zum Ende der Methode `OnCreate` hinzu, um die Schaltfläche **Übersetzungsverlauf** zu verknüpfen:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Aktualisieren Sie die Schaltfläche **Übersetzen**, um die Telefonnummer zur Liste `phoneNumbers` hinzuzufügen. Der `Click`-Handler für `TranslateHistoryButton` sollte in etwa dem folgenden Code entsprechen:

```csharp
// Add code to translate number
string translatedNumber = string.Empty;
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

Speichern und kompilieren Sie die Anwendung, um sicherzustellen, dass keine Fehler auftreten.

### <a name="running-the-app"></a>Ausführen der App

Stellen Sie die Anwendung für einen Emulator oder ein Gerät bereit. Die folgenden Screenshots zeigen die laufende **Phoneword**-App:

[![Beispielscreenshots](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Öffnen Sie zunächst das **Phoneword**-Projekt in Visual Studio für Mac, und bearbeiten Sie die Datei **Main.axml** aus dem **Lösungspad**.

### <a name="updating-the-layout"></a>Aktualisieren des Layouts

Ziehen Sie in der **Toolbox** eine **Schaltfläche** auf die Designoberfläche, und platzieren Sie sie unterhalb der TextView **TranslatedPhoneWord**. Ändern Sie im **Eigenschaftenpad** die **ID** der Schaltfläche in `@+id/TranslationHistoryButton`. 

[![Hineinziehen einer neuen Schaltfläche](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png)

Legen Sie die Eigenschaft **Text** der Schaltfläche auf `@string/translationHistory` fest. Android Designer wird dies wörtlich interpretieren, aber Sie werden ein paar Änderungen vornehmen, sodass der Text der Schaltfläche richtig angezeigt wird:

[![Festlegen des Schaltflächentexts für „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png)


Erweitern Sie den Knoten **Werte** unter dem Ordner **Ressourcen** im **Lösungspad**, und doppelklicken Sie auf die Datei für Zeichenfolgenressourcen **Strings.xml**:

[![Geöffnete Zeichenfolgen](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png)


Fügen Sie den Namen und den Wert der Zeichenfolge `translationHistory` zur Datei **Strings.xml** hinzu, und speichern Sie sie:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

Der Schaltflächentext für **Übersetzungsverlauf** sollte aktualisiert werden, um den neuen Zeichenfolgenwert widerzuspiegeln:

[![Schaltfläche mit übernommenem neuen Zeichenfolgenwert](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png)


Klicken Sie auf der Designoberfläche die Schaltfläche **Übersetzungsverlauf** an, öffnen Sie die Registerkarte **Verhalten** im **Eigenschaftenpad**, und deaktivieren Sie die Schaltfläche mit einem Doppelklick auf das Kontrollkästchen **Aktiviert**. Dadurch wird die Schaltfläche in der Designoberfläche dunkler:

[![Deaktivieren der Schaltfläche „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png)

### <a name="creating-the-second-activity"></a>Erstellen der zweiten Aktivität

Erstellen Sie eine zweite Aktivität, um den zweiten Bildschirm einzurichten. Klicken Sie im **Lösungspad** auf das graue Zahnradsymbol neben dem Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neue Datei...**:

Klicken Sie im Dialogfeld **Neue Datei** auf **Android > Aktivität**, nennen Sie die Aktivität `TranslationHistoryActivity`, und klicken Sie anschließend auf **Hinzufügen**.

Ersetzen Sie den Vorlagencode in `TranslationHistoryActivity` mit Folgendem:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

In dieser Klasse wird eine `ListActivity` erstellt und programmgesteuert befüllt, sodass Sie keine neue Layoutdatei für diese Aktivität erstellen müssen. Dies wird im Artikel [Hello, Android Multiscreen Deep Dive („Hallo, Android“-Multiscreen – Ausführliche Erläuterungen)](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md) genauer erläutert.

### <a name="adding-translation-history-code"></a>Hinzufügen des Übersetzungsverlaufcodes

Diese App sammelt Telefonnummern, die der Benutzer im ersten Bildschirm übersetzt hat, und übergibt sie an den zweiten Bildschirm. Die Telefonnummern werden als Liste von Zeichenfolgen gespeichert. Fügen Sie am Anfang der Klasse `MainActivity` die folgende `using`-Anweisung hinzu, um die Listen zu unterstützen:

```csharp
using System.Collections.Generic;
```

Erstellen Sie als Nächstes eine leere Liste, die mit Telefonnummern befüllt werden kann. Die Klasse `MainActivity` sieht nun wie folgt aus:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

Fügen Sie den folgenden Code in der Klasse `MainActivity` hinzu, um die Schaltfläche **Übersetzungsverlauf** zu registrieren (erstellen Sie diese Zeile nach der Deklaration `TranslationHistoryButton`):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Fügen Sie den folgenden Code zum Ende der Methode `OnCreate` hinzu, um die Schaltfläche **Übersetzungsverlauf** zu verknüpfen:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Aktualisieren Sie die Schaltfläche **Übersetzen**, um die Telefonnummer zur Liste `phoneNumbers` hinzuzufügen. Der `Click`-Handler für `TranslateHistoryButton` sollte in etwa dem folgenden Code entsprechen:

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

### <a name="running-the-app"></a>Ausführen der App

Stellen Sie die Anwendung für einen Emulator oder ein Gerät bereit. Die folgenden Screenshots zeigen die laufende **Phoneword**-App:

[![Beispielscreenshots](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png)

-----

Herzlichen Glückwunsch, Sie haben die Anwendung Xamarin.Android-Multiscreen fertiggestellt! Nun ist es an der Zeit, die Tools und Fähigkeiten zu analysieren, die Sie gerade gelernt haben &ndash; weiter geht es mit [Hello, Android Multiscreen Deep Dive („Hallo, Android“-Multiscreen – Ausführliche Erläuterungen)](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).


## <a name="related-links"></a>Verwandte Links

- [Xamarin-App-Symbole & -Startbildschirme (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (Beispiel)](https://developer.xamarin.com/samples/monodroid/Phoneword)
- [PhonewordMultiscreen (Beispiel)](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen)
