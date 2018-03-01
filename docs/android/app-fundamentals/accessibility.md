---
title: Eingabehilfen unter Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/31/2018
ms.openlocfilehash: 0cf1557cea8d5adb3678ba5e424f9f23375e32bc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="accessibility-on-android"></a>Eingabehilfen unter Android

Diese Seite beschreibt, wie Android Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Eingabehilfen Prüfliste](~/cross-platform/app-fundamentals/accessibility.md).
Finden Sie in der [iOS Eingabehilfen](~/ios/app-fundamentals/accessibility.md) und [OS X-Eingabehilfen](~/mac/app-fundamentals/accessibility.md) Seiten für andere Plattform-APIs.


## <a name="describing-ui-elements"></a>Beschreibt die Elemente der Benutzeroberfläche

Android bietet eine `ContentDescription` -Eigenschaft, die durch Lesen der APIs Bildschirm verwendet wird, um eine barrierefreie Beschreibung des Zwecks des Steuerelements bereitzustellen.

Die inhaltsbeschreibung kann in entweder c# oder in der Layoutdatei AXML festgelegt werden.

**C#**

Die Beschreibung kann im Code auf eine beliebige Zeichenfolge (oder eine Zeichenfolgenressource) festgelegt werden:

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML-layout**

Bei XML Layouts verwenden die `android:contentDescription` Attribut:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Verwenden Sie für TextView-Hinweis

Für `EditText` und `TextView` Steuerelemente für die Dateneingabe, verwenden die `Hint` Eigenschaft zu beschreiben, welche Eingabe erwartet wird (anstelle von `ContentDescription`).
Wenn Text eingegeben wurde, wird der Text selbst "anstelle der Hinweis gelesen werden".

**C#**

Legen Sie die `Hint` -Eigenschaft im Code:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML-layout**

In XML-Layout-Dateien verwenden die `android:hint` Attribut:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor Links Eingabefelder mit Bezeichnungen

Um eine Bezeichnung eines Datensteuerelements zuzuordnen, verwenden Sie die `LabelFor` Eigenschaft

**C#**

Legen Sie in c# die `LabelFor` Eigenschaft, um die Ressourcen-ID des Steuerelements dieser diesem Inhalt beschreibt (in der Regel diese Eigenschaft für eine Bezeichnung festgelegt ist und einige andere Eingabesteuerelement verweist):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML-layout**

Im Layout-XML-Verwendung der `android:labelFor` Eigenschaft, um ein anderes Steuerelement Bezeichner verweisen:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Ankündigen für Eingabehilfen

Verwenden der `AnnounceForAccessibility` Methode für ein beliebiges anzeigen Steuerelement, um ein Ereignis oder Status Änderung an Benutzer zu kommunizieren, wenn der Zugriff aktiviert ist. Diese Methode ist nicht erforderlich, für die meisten Vorgänge der integrierten Kommentar stellt ausreichende Feedback bereit, wobei sollte verwendet werden, in denen zusätzliche Informationen für den Benutzer hilfreich sein kann.

Der folgende Code zeigt ein einfaches Beispiel Aufrufen `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Ändern den Fokus-Einstellungen

Navigation zugegriffen werden kann, basiert auf Steuerelemente den Fokus auf den Benutzer zu unterstützen, zu verstehen, welche Vorgänge verfügbar sind. Android bietet eine `Focusable` Eigenschaft, die Steuerelemente als speziell kann den Fokus erhalten, während der Navigation kennzeichnen kann.

**C#**

Um zu verhindern, dass ein Steuerelement erhält den Fokus mit c#, legen Sie die `Focusable` Eigenschaft `false`:

```csharp
label.Focusable = false;
```

**AXML-layout**

Im Layout-XML-Dateien Satz der `android:focusable` Attribut:

```xml
<android:focusable="false" />
```

Sie können auch steuern die Reihenfolge der Fokus mit der `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` Attribute, die in der Regel im Layout AXML festgelegt. Verwenden Sie diese Attribute, um sicherzustellen, dass der Benutzer leicht die Steuerelemente auf dem Bildschirm navigieren kann.


## <a name="accessibility-and-localization"></a>Eingabehilfen und Lokalisierung

In der obigen Beispiele werden die Beschreibung-Hinweis und Inhalt, die unmittelbar auf den Anzeigewert festgelegt werden. Ist es besser, verwenden Sie die Werte in einer **Strings.xml** Datei, wie diese:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

Mithilfe von Text aus einer Datei Zeichenfolgen wird in c# und AXML Layoutdateien unten gezeigt:

**C#**

Statt im Code mithilfe von Zeichenfolgenliteralen, suchen Sie die übersetzten Werte aus Zeichenfolgen-Dateien, deren `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

In der Layout-XML Eingabehilfen-Attribute wie `hint` und `contentDescription` kann auf ein Zeichenfolgenbezeichner festgelegt werden:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Der Vorteil der Text in einer separaten Datei gespeichert ist, dass mehrere sprachübersetzungen der Datei in die app bereitgestellt werden können. Finden Sie unter der [Android-lokalisierungsleitfaden](~/android/app-fundamentals/localization.md) um zu erfahren, wie lokalisierte Zeichenfolge Dateien hinzufügen, um ein Anwendungsprojekt.

<a name="testing" />

## <a name="testing-accessibility"></a>Barrierefreiheit

Führen Sie die [Schritte](http://developer.android.com/training/accessibility/testing.html#how-to) TalkBack und Durchsuchen von Touch So testen Sie den Zugriff auf Android-Geräten zu aktivieren.

Möglicherweise müssen Sie installieren [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) von Google Play herunter, wenn er nicht in erscheint **Einstellungen > Eingabehilfen**.



## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Eingabehilfen](~/cross-platform/app-fundamentals/accessibility.md)
- [Zugriff auf Android-APIs](http://developer.android.com/guide/topics/ui/accessibility/index.html)
