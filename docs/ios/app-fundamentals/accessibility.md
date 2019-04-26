---
title: Eingabehilfen unter iOS
description: Dieses Dokument beschreibt die Eingabehilfen in iOS, erläutern die verschiedenen Eigenschaften und Funktionen, die von wie vielen Benutzern, Ihre Anwendung verwendet werden kann verwendet werden können, wie möglich.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/18/2016
ms.openlocfilehash: aa3e15797ae1dac621ea8a78345044be1387ebaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179548"
---
# <a name="accessibility-on-ios"></a>Eingabehilfen unter iOS

Diese Seite beschreibt, wie die iOS-Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Checkliste für die Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md).
Finden Sie in der [Android Barrierefreiheit](~/android/app-fundamentals/accessibility.md) und [OS X Accessibility](~/mac/app-fundamentals/accessibility.md) Seiten für andere Plattform-APIs.

## <a name="describing-ui-elements"></a>Beschreibt die Elemente der Benutzeroberfläche

iOS bietet die `AccessibilityLabel` und `AccessibilityHint` Bildschirmsprachausgabe der Steuerelemente leichter zugänglich machen Eigenschaften für Entwickler zum Hinzufügen von beschreibendem Text der von der VoiceOver verwendet werden kann. Steuerelemente können auch mit mindestens eine "traits" gekennzeichnet werden, die zusätzlichen Kontext zugegriffen werden kann Modi bereitstellen.

Einige Steuerelemente müssen nicht darauf zugreifen können (z. B. eine Bezeichnung für eine Texteingabe, oder ein Bild, das nur dekorativen Zwecken dient) – die `IsAccessibilityElement` wird bereitgestellt, um die Barrierefreiheit in diesen Fällen deaktivieren.

**Benutzeroberflächen-Designer**

Die **Pad "Eigenschaften"** enthält einen Eingabehilfen-Abschnitt, der diese Einstellungen bearbeitet werden, wenn ein Steuerelement, in der iOS-Benutzeroberflächen-Designer ausgewählt ist ermöglicht:

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

Der Wert des `AccessibilityIdentifier` wird nie gesprochene oder dem Benutzer angezeigt.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

Die `UIAccessibility.PostNotification` Methode können Sie Ereignisse ausgelöst werden, für den Benutzer außerhalb der direkte Interaktion (z. B., wenn eine Interaktion mit einem bestimmten Steuerelement).

### <a name="announcement"></a>Ankündigung

Eine Ankündigung kann aus Code gesendet werden, um den Benutzer zu informieren, den ein Status sich geändert hat (z. B. ein Vorgang im Hintergrund abgeschlossen wurde). Dies könnte durch einen visuellen Hinweis auf der Benutzeroberfläche begleitet werden:

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


## <a name="accessibility-and-localization"></a>Barrierefreiheit und Lokalisierung

Eigenschaften von Bedienungshilfen wie die Bezeichnung und einen Hinweis nur lokalisiert werden können wie andere Text in der Benutzeroberfläche.

**MainStoryboard.strings**

Wenn die Benutzeroberfläche in einem Storyboard angeordnet ist, können Sie Übersetzungen für die Eigenschaften von Bedienungshilfen auf die gleiche Weise wie andere Eigenschaften bereitstellen. Im folgenden Beispiel wird eine `UITextField` verfügt über eine **Lokalisierungs-ID** von `Pqa-aa-ury` und zwei Eigenschaften von Bedienungshilfen auf Spanisch festgelegt wird:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Diese Datei würde platziert werden, der **es.lproj** Verzeichnis für Spanisch Inhalt.

**Localizable.strings**

Alternativ können die Übersetzungen hinzugefügt werden, um die **Localizable.strings** -Datei im lokalisierten Verzeichnis mit Inhalt (z. b. **es.lproj** für Spanisch):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Diese Übersetzungen können verwendet werden, C# über die `LocalizedString` Methode:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Finden Sie in der [iOS Lokalisierung Entwicklerhandbuch](~/ios/app-fundamentals/localization/index.md) ausführliche Informationen zum Lokalisieren von Inhalten.

<a name="testing" />

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

VoiceOver aktiviert ist, der **Einstellungen** app durch Navigieren zu **Allgemein > Zugriff > VoiceOver**:

![](accessibility-images/settings-sml.png "Die sprechrate festlegen")

Die **Barrierefreiheit** Bildschirm bietet auch die Einstellungen für Zoom, Textgröße, Farbe und Kontrast Optionen, Einstellungen für Sprache und andere Konfigurationsoptionen.

Befolgen Sie diese [VoiceOver Anweisungen](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) zum Zugriff auf iOS-Geräten zu testen.


## <a name="simulator-testing"></a>Simulatortests

Beim Testen im Simulator die **Barrierefreiheit Inspektor** verfügbar, mit denen überprüfen, ob Zugriff auf Eigenschaften und Ereignisse sind richtig konfiguriert ist. Aktivieren Sie den Inspektor in die **Einstellungen** app durch Navigieren zu **Allgemein > Zugriff > Barrierefreiheit Inspektor**:

![](accessibility-images/settings-inspector-sml.png "Aktivieren von Eingabehilfen-Inspektor")

Nach der Aktivierung können Sie Inspektor-Fenster auf iOS-Bildschirms zeigen, zu jeder Zeit.
Hier ist ein Beispiel der Ausgabe, wenn eine Zeile einer Tabelle aktiviert ist – Beachten Sie, dass der **Bezeichnung** enthält einen Satz, der die Inhalte der Zeile aus, und auch, die es "done" (ie. die Teilstriche angezeigt wird):

![](accessibility-images/tableview-a11y-sml.png "Verwenden Accessibility Seitenprüfung")

Während der Inspektor angezeigt wird, verwenden Sie das Symbol "X" auf der linken oberen Ecke, um vorübergehend anzeigen und Ausblenden der Überlagerung und Einstellungen für die Barrierefreiheit aktivieren/deaktivieren.



## <a name="related-links"></a>Verwandte Links

- [Cross-platform Accessibility (Plattformübergreifende Barrierefreiheit)](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS-Zugriff (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
