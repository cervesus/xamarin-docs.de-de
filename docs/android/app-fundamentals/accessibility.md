---
title: Eingabehilfen unter Android
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: d004b753c89f3995e8dc511877bd115a894396fc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61018619"
---
# <a name="accessibility-on-android"></a>Eingabehilfen unter Android

Diese Seite beschreibt, wie die Android Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Checkliste für die Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md).
Finden Sie in der [iOS Barrierefreiheit](~/ios/app-fundamentals/accessibility.md) und [OS X Accessibility](~/mac/app-fundamentals/accessibility.md) Seiten für andere Plattform-APIs.


## <a name="describing-ui-elements"></a>Beschreibt die Elemente der Benutzeroberfläche

Android bietet eine `ContentDescription` -Eigenschaft, die durch Screenreader-APIs verwendet wird, um eine barrierefreie Beschreibung des Zwecks der Steuerelements bereitzustellen.

Die Beschreibung der Inhalte kann festgelegt werden, entweder in C# oder in der AXML-Layoutdatei.

**C#**

Die Beschreibung kann im Code, um eine beliebige Zeichenfolge (oder eine Zeichenfolgenressource) festgelegt werden:

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML-layout**

Im XML-Layouts verwendet die `android:contentDescription` Attribut:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Use Hint-Hinweis für TextView

Für `EditText` und `TextView` Steuerelemente für die Dateneingabe, verwenden die `Hint` Eigenschaft beschrieben, welche Eingabe erwartet wird (anstelle von `ContentDescription`).
Wenn Text eingegeben wurde, wird der Text "anstelle der Hinweis gelesen werden".

**C#**

Legen Sie die `Hint` Eigenschaft im Code:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML-layout**

Im XML-Layoutdateien verwenden die `android:hint` Attribut:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor Links Eingabefelder mit Bezeichnungen

Um eine Bezeichnung mit einem Eingabe-Steuerelement zuzuordnen, verwenden die `LabelFor` Eigenschaft

**C#**

In C#legen die `LabelFor` Eigenschaft, um die Ressourcen-ID des Steuerelements, das diesen Inhalt beschreibt (in der Regel diese Eigenschaft wird auf eine Bezeichnung festgelegt und verweist auf einige andere Eingabesteuerelement):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML-layout**

Im Layout-XML-Verwendung der `android:labelFor` -Eigenschaft zum Verweisen auf ein anderes Steuerelement-ID:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Ankündigen zu können, für den Zugriff auf

Verwenden der `AnnounceForAccessibility` Methode auf einem anzeigen Steuerelement auf ein Ereignis oder den Status ändern, Benutzern zu kommunizieren, wenn der Zugriff aktiviert ist. Diese Methode ist nicht erforderlich, für die meisten Vorgänge, in denen die integrierten audioaufzeichnung ausreichend Feedback bereitgestellt, sollte aber verwendet werden, in dem zusätzliche Informationen für den Benutzer sinnvoll sein.

Der folgende Code zeigt ein einfaches Beispiel Aufrufen `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Änderung der Einstellungen für den Fokus

Zugänglich Navigation basiert auf Steuerelemente den Fokus auf unterstützen den Benutzer zu verstehen, welche Vorgänge verfügbar sind. Android bietet eine `Focusable` Eigenschaft, die Steuerelemente z. B. insbesondere kann den Fokus erhalten, während der Navigation kennzeichnen kann.

**C#**

Um zu verhindern, dass ein Steuerelement den Fokus erhalten C#legen die `Focusable` Eigenschaft `false`:

```csharp
label.Focusable = false;
```

**AXML-layout**

Im Layout-XML-Dateien Satz der `android:focusable` Attribut:

```xml
<android:focusable="false" />
```

Sie können auch steuern die Reihenfolge der Fokus mit der `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` Attribute, die in der Regel im AXML-Layout festgelegt. Verwenden Sie diese Attribute, um sicherzustellen, dass der Benutzer leicht die Steuerelemente auf dem Bildschirm navigieren kann.


## <a name="accessibility-and-localization"></a>Barrierefreiheit und Lokalisierung

Legen in die obigen Beispielen die-Hinweis und Inhalt Beschreibung finden Sie direkt auf der Anzeigewert. Es ist empfehlenswert, verwenden Sie die Werte in einer **von "Strings.xml"** Datei, wie diese:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

Dabei wird Text aus einer Datei Zeichenfolgen wird unten gezeigt C# und AXML-Layoutdateien:

**C#**

Statt im Code mithilfe von Zeichenfolgenliteralen, suchen Sie die übersetzten Werte aus Zeichenfolgen-Dateien mit `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

Wie in der Layout-XML-Attribute als Eingabehilfe `hint` und `contentDescription` kann auf einen Zeichenfolgenbezeichner festgelegt werden:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Der Vorteil von Text in einer separaten Datei gespeichert ist, dass mehrere sprachübersetzungen der Datei in Ihre app bereitgestellt werden können. Finden Sie unter den [Android Lokalisierung Handbuch](~/android/app-fundamentals/localization.md) , erfahren, wie ein Projekt für die lokalisierte Zeichenfolge-Dateien hinzuzufügen.


## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

Führen Sie [folgendermaßen](https://developer.android.com/training/accessibility/testing.html#how-to) So aktivieren Sie TalkBack und Durchsuchen von Touch, um Zugriff auf Android-Geräten zu testen.

Müssen Sie möglicherweise installieren [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) aus Google Play erscheint es nicht in **Einstellungen > Barrierefreiheit**.


## <a name="related-links"></a>Verwandte Links

- [Cross-platform Accessibility (Plattformübergreifende Barrierefreiheit)](~/cross-platform/app-fundamentals/accessibility.md)
- [Zugriff auf Android-APIs](https://developer.android.com/guide/topics/ui/accessibility/index.html)
