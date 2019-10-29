---
title: Barrierefreiheit unter Android
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: 6e86663be0bb06697fbfe0e8c360d733bca18da0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017019"
---
# <a name="accessibility-on-android"></a>Barrierefreiheit unter Android

Auf dieser Seite wird beschrieben, wie Sie die Android-Barrierefreiheits-APIs zum Erstellen von apps entsprechend der [Eincheck Checkliste](~/cross-platform/app-fundamentals/accessibility.md)verwenden.
Weitere Plattform-APIs finden Sie unter [IOS-Barrierefreiheit](~/ios/app-fundamentals/accessibility.md) und [OS X-Barrierefreiheits](~/mac/app-fundamentals/accessibility.md) Seiten.

## <a name="describing-ui-elements"></a>Beschreiben von UI-Elementen

Android stellt eine `ContentDescription` Eigenschaft bereit, die von Bildschirm Lese-APIs verwendet wird, um eine barrierefreie Beschreibung des Steuer Elements bereitzustellen.

Die Inhaltsbeschreibung kann entweder C# in oder in der axml-Layoutdatei festgelegt werden.

**C#**

Die Beschreibung kann im Code auf eine beliebige Zeichenfolge (oder eine Zeichen folgen Ressource) festgelegt werden:

```csharp
saveButton.ContentDescription = "Save data";
```

**Axml-Layout**

Verwenden Sie in XML-Layouts das `android:contentDescription`-Attribut:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Use Hint für TextView

Verwenden Sie für `EditText`-und `TextView`-Steuerelemente für die Dateneingabe die `Hint`-Eigenschaft, um eine Beschreibung der erwarteten Eingabe anzugeben (anstatt `ContentDescription`).
Wenn Text eingegeben wurde, wird der Text selbst "Read" anstelle des Hinweises.

**C#**

Legen Sie die `Hint`-Eigenschaft im Code fest:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**Axml-Layout**

Verwenden Sie in XML-Layoutdateien das `android:hint`-Attribut:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```

### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor verknüpft Eingabefelder mit Bezeichnungen

Um eine Bezeichnung einem Dateneingabe-Steuerelement zuzuordnen, verwenden Sie die `LabelFor`-Eigenschaft, um

**C#**

Legen C#Sie in für die `LabelFor`-Eigenschaft die Ressourcen-ID des Steuer Elements fest, das von diesem Inhalt beschrieben wird (in der Regel wird diese Eigenschaft für eine Bezeichnung festgelegt und verweist auf ein anderes Eingabe Steuerelement):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**Axml-Layout**

Verwenden Sie im Layout-XML die `android:labelFor`-Eigenschaft, um auf den Bezeichner eines anderen Steuer Elements

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Ankündigen für Barrierefreiheit

Verwenden Sie die `AnnounceForAccessibility`-Methode für ein beliebiges Ansicht-Steuerelement, um Benutzern bei aktivierter Barrierefreiheit ein Ereignis oder eine Statusänderung mitzuteilen. Diese Methode ist für die meisten Vorgänge nicht erforderlich, bei denen die integrierte-Erzählung ein ausreichendes Feedback bereitstellt, aber verwendet werden sollte, wenn zusätzliche Informationen für den Benutzer hilfreich sind.

Der folgende Code zeigt ein einfaches Beispiel für das Aufrufen von `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Ändern der Fokuseinstellungen

Barrierefreie Navigation basiert darauf, dass Steuerelemente den Fokus haben, um dem Benutzer zu helfen, die verfügbaren Vorgänge zu verstehen. Android stellt eine `Focusable`-Eigenschaft bereit, die Steuerelemente so markieren kann, dass Sie den Fokus während der Navigation erhalten.

**C#**

Um zu verhindern C#, dass ein Steuerelement den Fokus erhält, legen Sie die `Focusable`-Eigenschaft auf `false` fest:

```csharp
label.Focusable = false;
```

**Axml-Layout**

Legen Sie in Layout-XML-Dateien das `android:focusable`-Attribut fest:

```xml
<android:focusable="false" />
```

Sie können auch die Reihenfolge der Nachrichten mit dem `nextFocusDown`, `nextFocusLeft` `nextFocusRight`, `nextFocusUp` Attributen steuern, die in der Regel im Layout axml festgelegt sind. Verwenden Sie diese Attribute, um sicherzustellen, dass der Benutzer einfach durch die Steuerelemente auf dem Bildschirm navigieren kann.

## <a name="accessibility-and-localization"></a>Barrierefreiheit und Lokalisierung

In den obigen Beispielen werden der Hinweis und die Inhaltsbeschreibung direkt auf den Anzeige Wert festgelegt. Es ist vorzuziehen, Werte in einer **Strings. XML** -Datei zu verwenden, wie z. b.:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

Die Verwendung von Text aus einer Zeichen folgen Datei ist C# unten in und in den axml-Layoutdateien dargestellt:

**C#**

Suchen Sie anstelle von Zeichenfolgenliteralen im Code übersetzte Werte aus Strings-Dateien mit `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

In LayoutXml-Barrierefreiheits Attributen wie `hint` und `contentDescription` können auf einen Zeichen folgen Bezeichner festgelegt werden:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Der Vorteil der Speicherung von Text in einer separaten Datei besteht darin, dass mehrere Sprachübersetzungen der Datei in Ihrer APP bereitgestellt werden können. Weitere Informationen zum Hinzufügen lokalisierter Zeichen folgen Dateien zu einem Anwendungsprojekt finden Sie im [Leitfaden zur Android-Lokalisierung](~/android/app-fundamentals/localization.md) .

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

Führen Sie [die folgenden Schritte](https://developer.android.com/training/accessibility/testing.html#how-to) aus, um talkbacks zu aktivieren und per Fingerabdruck zu untersuchen, um den Zugriff auf Android

Möglicherweise müssen Sie [Talkback](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) von Google Play installieren, wenn es nicht in den **Einstellungen > Barrierefreiheit**angezeigt wird.

## <a name="related-links"></a>Verwandte Links

- [Cross-platform Accessibility (Plattformübergreifende Barrierefreiheit)](~/cross-platform/app-fundamentals/accessibility.md)
- [Android-Barrierefreiheit-APIs](https://developer.android.com/guide/topics/ui/accessibility/index.html)
