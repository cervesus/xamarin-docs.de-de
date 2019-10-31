---
title: 'Hello, Android Multiscreen (Hallo, Android-Multiscreen): Schnellstart'
description: In diesem zweiteiligen Handbuch wird die Phoneword-Anwendung, um einen zweiten Bildschirm erweitert. Gleichzeitig werden die grundlegenden Bausteine von Android-Anwendungen erläutert und sie erhalten einen tieferen Einblick in die Architektur von Android.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 114373b6c4b194fe6e566255eb09eb82a8208312
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020964"
---
# <a name="hello-android-multiscreen-quickstart"></a>Hello, Android Multiscreen (Hallo, Android-Multiscreen): Schnellstart

_In diesem zweiteiligen Leitfaden wird die Phoneword-Anwendung, um einen zweiten Bildschirm erweitert. Gleichzeitig werden die grundlegenden Bausteine von Android-Anwendungen erläutert, und Sie erhalten einen tieferen Einblick in die Architektur von Android._

Im Teil des Handbuchs mit der exemplarischen Vorgehensweise fügen Sie einen zweiten Bildschirm zur [Phoneword](https://docs.microsoft.com/samples/xamarin/monodroid-samples/phoneword)-Anwendung hinzu, um mithilfe der App den Verlauf der übersetzten Nummern nachzuverfolgen. Die [endgültige Anwendung](https://docs.microsoft.com/samples/xamarin/monodroid-samples/phonewordmultiscreen) verfügt dann über einen zweiten Bildschirm, auf dem wie im Screenshot auf der rechten Seite veranschaulicht die übersetzten Nummern angezeigt werden:

[![Screenshots der Beispiel-App](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

Im begleitenden Artikel [Hello, Android Multiscreen: Deep Dive (Hallo, Android: Ausführliche Erläuterungen)](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md) wird wiederholt, was erstellt wurde, und es werden die Architektur, die Navigation und andere neue Android-Konzepte diskutiert.

## <a name="requirements"></a>Requirements (Anforderungen)

Da dieses Handbuch dort ansetzt, wo [Hallo, Android](~/android/get-started/hello-android/index.md) aufhört, muss der [Schnellstart für „Hallo, Android“](~/android/get-started/hello-android/hello-android-quickstart.md) abgeschlossen werden.
Wenn Sie direkt mit der exemplarischen Vorgehensweise weiter unten beginnen möchten, können Sie die vollständige Version von [Phoneword](https://docs.microsoft.com/samples/xamarin/monodroid-samples/phoneword) aus dem Artikel Schnellstart für „Hallo, Android“ herunterladen und damit die exemplarische Vorgehensweise durcharbeiten.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

In dieser exemplarischen Vorgehensweise fügen Sie der **Phoneword**-Anwendung einen Bildschirm für den **Übersetzungsverlauf** hinzu.

::: zone pivot="windows"

Öffnen Sie zunächst die **Phoneword**-Anwendung in Visual Studio, und bearbeiten Sie die Datei **Main.axml** über den **Projektmappen-Explorer**.

> [!TIP]
> Neuere Releases von Visual Studio unterstützen das Öffnen von XML-Dateien in Android Designer.
>
> Sowohl AXML- als auch XML-Dateien werden in Android Designer unterstützt.

### <a name="updating-the-layout"></a>Aktualisieren des Layouts

Ziehen Sie in der **Toolbox** eine **Schaltfläche** auf die Designoberfläche, und platzieren Sie sie unterhalb der TextView **TranslatedPhoneWord**. Ändern Sie im Bereich **Eigenschaften** die **ID** der Schaltfläche in `@+id/TranslationHistoryButton`.

[![Hineinziehen einer neuen Schaltfläche](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png#lightbox)

Legen Sie die Eigenschaft **Text** der Schaltfläche auf `@string/translationHistory` fest. Android Designer wird dies wörtlich interpretieren, aber Sie werden ein paar Änderungen vornehmen, sodass der Text der Schaltfläche richtig angezeigt wird:

[![Festlegen des Schaltflächentexts für „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png#lightbox)

Erweitern Sie den Knoten **Werte** unter dem Ordner **Ressourcen** im **Projektmappen-Explorer**, und doppelklicken Sie auf die Datei für Zeichenfolgenressourcen **Strings.xml**:

[![Öffnen von „Strings.xml“](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png#lightbox)

Fügen Sie den Namen und den Wert der Zeichenfolge `translationHistory` zur Datei **Strings.xml** hinzu, und speichern Sie sie:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

Der Schaltflächentext für **Übersetzungsverlauf** sollte aktualisiert werden, um den neuen Zeichenfolgenwert widerzuspiegeln:

[![Schaltfläche mit übernommenem neuen Zeichenfolgenwert](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png#lightbox)

Wenn Sie auf der Designoberfläche auf die Schaltfläche **Übersetzungsverlauf** geklickt haben, suchen Sie im Pad **Eigenschaften** die Einstellung `enabled`, und legen Sie ihren Wert auf `false` fest, um die Schaltfläche zu deaktivieren. Dadurch wird die Schaltfläche in der Designoberfläche dunkler:

[![Deaktivieren der Schaltfläche „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>Erstellen der zweiten Aktivität

Erstellen Sie eine zweite Aktivität, um den zweiten Bildschirm einzurichten. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…** :

[![Hinzufügen einer neuen Datei](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png#lightbox)

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

### <a name="adding-a-list"></a>Hinzufügen einer Liste

Diese App sammelt Telefonnummern, die der Benutzer im ersten Bildschirm übersetzt hat, und übergibt sie an den zweiten Bildschirm. Die Telefonnummern werden als Liste von Zeichenfolgen gespeichert. Um Listen (und Prioritäten, die später verwendet werden) zu unterstützen, fügen Sie die folgenden `using`-Anweisungen an den Anfang von **MainActivity.cs** hinzu:

```csharp
using System.Collections.Generic;
using Android.Content;
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

Fügen Sie den folgenden Code in der Klasse `MainActivity` hinzu, um die Schaltfläche **Übersetzungsverlauf** zu registrieren (erstellen Sie diese Zeile nach der Deklaration `translateButton`):

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

Aktualisieren Sie die Schaltfläche **Übersetzen**, um die Telefonnummer zur Liste `phoneNumbers` hinzuzufügen. Der `Click`-Handler für `translateButton` sollte in etwa dem folgenden Code entsprechen:

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

[![Beispielscreenshots](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

::: zone-end
::: zone pivot="macos"

Öffnen Sie zunächst das **Phoneword**-Projekt in Visual Studio für Mac, und bearbeiten Sie die Datei **Main.axml** aus dem **Lösungspad**.

> [!TIP]
> Neuere Releases von Visual Studio unterstützen das Öffnen von XML-Dateien in Android Designer.
>
> Sowohl AXML- als auch XML-Dateien werden in Android Designer unterstützt.

### <a name="updating-the-layout"></a>Aktualisieren des Layouts

Ziehen Sie in der **Toolbox** eine **Schaltfläche** auf die Designoberfläche, und platzieren Sie sie unterhalb der TextView **TranslatedPhoneWord**. Ändern Sie im **Eigenschaftenpad** die **ID** der Schaltfläche in `@+id/TranslationHistoryButton`:

[![Hineinziehen einer neuen Schaltfläche](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png#lightbox)

Legen Sie die Eigenschaft **Text** der Schaltfläche auf `@string/translationHistory` fest. Android Designer wird dies wörtlich interpretieren, aber Sie werden ein paar Änderungen vornehmen, sodass der Text der Schaltfläche richtig angezeigt wird:

[![Festlegen des Schaltflächentexts für „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png#lightbox)

Erweitern Sie den Knoten **Werte** unter dem Ordner **Ressourcen** im **Lösungspad**, und doppelklicken Sie auf die Datei für Zeichenfolgenressourcen **Strings.xml**:

[![Geöffnete Zeichenfolgen](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png#lightbox)

Fügen Sie den Namen und den Wert der Zeichenfolge `translationHistory` zur Datei **Strings.xml** hinzu, und speichern Sie sie:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

Der Schaltflächentext für **Übersetzungsverlauf** sollte aktualisiert werden, um den neuen Zeichenfolgenwert widerzuspiegeln:

[![Schaltfläche mit übernommenem neuen Zeichenfolgenwert](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png#lightbox)

Klicken Sie auf der Designoberfläche die Schaltfläche **Übersetzungsverlauf** an, öffnen Sie die Registerkarte **Verhalten** im **Eigenschaftenpad**, und deaktivieren Sie die Schaltfläche mit einem Doppelklick auf das Kontrollkästchen **Aktiviert**. Dadurch wird die Schaltfläche in der Designoberfläche dunkler:

[![Deaktivieren der Schaltfläche „Übersetzungsverlauf“](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>Erstellen der zweiten Aktivität

Erstellen Sie eine zweite Aktivität, um den zweiten Bildschirm einzurichten. Klicken Sie im **Lösungspad** auf das graue Zahnradsymbol neben dem Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neue Datei...** :

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

### <a name="adding-a-list"></a>Hinzufügen einer Liste

Diese App sammelt Telefonnummern, die der Benutzer im ersten Bildschirm übersetzt hat, und übergibt sie an den zweiten Bildschirm. Die Telefonnummern werden als Liste von Zeichenfolgen gespeichert. Um Listen (und Prioritäten, die später verwendet werden) zu unterstützen, fügen Sie die folgenden `using`-Anweisungen an den Anfang von **MainActivity.cs** hinzu:

```csharp
using System.Collections.Generic;
using Android.Content;
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

[![Beispielscreenshots](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

::: zone-end

Herzlichen Glückwunsch, Sie haben die Anwendung Xamarin.Android-Multiscreen fertiggestellt! Nun ist es an der Zeit, die Tools und Fähigkeiten zu analysieren, die Sie gerade gelernt haben &ndash; weiter geht es mit [Hello, Android Multiscreen Deep Dive („Hallo, Android“-Multiscreen – Ausführliche Erläuterungen)](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).

## <a name="related-links"></a>Verwandte Links

- [Xamarin-App-Symbole & -Startbildschirme (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/phoneword)
- [PhonewordMultiscreen (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/phonewordmultiscreen)
