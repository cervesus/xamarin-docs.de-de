---
title: Eingabehilfen unter iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: 39fa99ba534655331098abcb55789c8f4a8afb07
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="accessibility-on-ios"></a>Eingabehilfen unter iOS

Diese Seite beschreibt, wie die iOS-Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Eingabehilfen Prüfliste](~/cross-platform/app-fundamentals/accessibility.md).
Finden Sie in der [Android Eingabehilfen](~/android/app-fundamentals/accessibility.md) und [OS X-Eingabehilfen](~/mac/app-fundamentals/accessibility.md) Seiten für andere Plattform-APIs.

## <a name="describing-ui-elements"></a>Beschreibt die Elemente der Benutzeroberfläche

iOS bietet die `AccessibilityLabel` und `AccessibilityHint` Bildschirmsprachausgabe, die Steuerelemente leichter zugänglich machen Eigenschaften für Entwickler beschreibenden Text hinzufügen, die von der VoiceOver verwendet werden können. Steuerelemente können auch mit ein oder mehrere Merkmale gekennzeichnet werden, die zusätzlichen Kontext zugegriffen werden Modi bereitstellen.

Einige Steuerelemente möglicherweise nicht möglich (z. B., eine Bezeichnung auf eine Texteingabe oder ein Bild, die rein dekorativen Zwecken dient) – die `IsAccessibilityElement` wird bereitgestellt, um die Barrierefreiheit in diesen Fällen zu deaktivieren.

**UI-Designer**

Die **Eigenschaften Pad** enthält einen Abschnitt "Eingabehilfen", die diese Einstellungen bearbeitet werden, wenn ein Steuerelement, in der iOS-UI-Designer ausgewählt ist ermöglicht:

![](accessibility-images/ios-designer-sml.png "Einstellungen für die Barrierefreiheit")

**C#**

Diese Eigenschaften können auch direkt im Code festgelegt werden:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Was ist AccessibilityIdentifier?

Die `AccessibilityIdentifier` wird verwendet, um einen eindeutigen Schlüssel festlegen, die zum Verweisen auf Elemente der Benutzeroberfläche über die UIAutomation-API verwendet werden kann.

Der Wert des `AccessibilityIdentifier` nie gesprochen oder dem Benutzer angezeigt wird.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

Die `UIAccessibility.PostNotification` Methode können Ereignisse ausgelöst werden, für den Benutzer außerhalb der direkten Interaktion (z. B. bei der Interaktion mit einem bestimmten Steuerelement).

### <a name="announcement"></a>Ankündigung

Eine Ankündigung kann aus Code gesendet werden, um den Benutzer zu informieren, den einige Status geändert wurde (z. B. ein Hintergrundvorgang abgeschlossen wurde). Dies könnte durch einen visuellen Hinweis auf der Benutzeroberfläche begleitet:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

Die `LayoutChanged` Ankündigung wird verwendet, wenn das Bildschirmlayout:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Eingabehilfen und Lokalisierung

Eingabehilfeeigenschaften, wie die Beschriftung als auch Hinweis nur lokalisiert werden können wie andere Text in der Benutzeroberfläche.

**MainStoryboard.strings**

Wenn die Benutzeroberfläche in eine Storyboard angeordnet sind, können Sie Übersetzungen für Eingabehilfeeigenschaften auf die gleiche Weise wie andere Eigenschaften angeben. Im folgenden Beispiel wird eine `UITextField` verfügt über eine **Lokalisierungs-ID** von `Pqa-aa-ury` und zwei Eingabehilfeeigenschaften auf Spanisch festgelegt wird:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Diese Datei würde platziert werden, in der **es.lproj** für Spanisch Inhalt.

**Localizable.strings**

Alternativ können die Übersetzungen hinzugefügt werden, um die **Localizable.strings** -Datei in der lokalisierten Verzeichnis des Websiteinhalts (z. b. **es.lproj** für Spanisch):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Diese Übersetzungen können verwendet werden, in c# über die `LocalizedString` Methode:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Finden Sie in der [iOS-lokalisierungsleitfaden](~/ios/app-fundamentals/localization/index.md) für Weitere Informationen zum Lokalisieren von Inhalt.

<a name="testing" />

## <a name="testing-accessibility"></a>Barrierefreiheit

VoiceOver aktiviert ist, der **Einstellungen** app durch Navigieren zum **Allgemein > Eingabehilfen > VoiceOver**:

![](accessibility-images/settings-sml.png "Die sprechgeschwindigkeit festlegen")

Die **Eingabehilfen** Bildschirm bietet auch Einstellungen für Zoom, Textgröße, Farbe und Kontrast Optionen, Spracherkennung Einstellungen und weitere Konfigurationsoptionen.

Befolgen Sie diese [VoiceOver Anweisungen](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) zum Zugriff auf iOS-Geräten zu testen.


## <a name="simulator-testing"></a>Simulator testen

Beim Testen im Simulator die **Eingabehilfen Inspektor** zur Verfügung, vergewissern Sie sich Zugriff auf Eigenschaften und Ereignisse sind richtig konfiguriert. Aktivieren der Inspektor in die **Einstellungen** app durch Navigieren zum **Allgemein > Eingabehilfen > Eingabehilfen Inspektor**:

![](accessibility-images/settings-inspector-sml.png "Aktivieren von Eingabehilfen-Inspektor")

Nach der Aktivierung bewegt Inspektor-Fenster jederzeit über den iOS-Bildschirm wird.
Hier ist ein Beispiel der Ausgabe, wenn eine Zeile einer Tabelle ausgewählt ist – Beachten Sie die **Bezeichnung** enthält einen Satz an, die den Inhalt der Zeile aus, und auch, es wird "Fertig" bietet (ie. die Teilstriche angezeigt wird):

![](accessibility-images/tableview-a11y-sml.png "Verwenden von Eingabehilfen-Inspektor")

Während der Inspektor angezeigt wird, verwenden Sie das Symbol "X" auf der linken oberen Ecke, um vorübergehend anzeigen und Ausblenden der Überlagerung und aktiviert bzw. deaktiviert die barrierefreiheitseinstellung.



## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Eingabehilfen](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS Eingabehilfen (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS Voice-over](http://www.apple.com/accessibility/ios/voiceover/)
