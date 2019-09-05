---
title: Barrierefreiheit unter IOS
description: Dieses Dokument beschreibt die Barrierefreiheit in IOS und erläutert verschiedene Eigenschaften und Features, mit denen die Anwendung von so vielen Benutzern wie möglich verwendet werden kann.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/18/2016
ms.openlocfilehash: 4a04f0ed4cbb336e331528c3d8265efb31388328
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289518"
---
# <a name="accessibility-on-ios"></a>Barrierefreiheit unter IOS

Auf dieser Seite wird beschrieben, wie Sie die IOS-Barrierefreiheits-APIs zum Erstellen von apps entsprechend der [Eincheck Checkliste](~/cross-platform/app-fundamentals/accessibility.md)verwenden.
Weitere Plattform-APIs finden Sie auf der Seite zu den Barrierefreiheits Seiten für [Android-Barrierefreiheit](~/android/app-fundamentals/accessibility.md) und [OS X](~/mac/app-fundamentals/accessibility.md) .

## <a name="describing-ui-elements"></a>Beschreiben von UI-Elementen

IOS stellt die `AccessibilityLabel` - `AccessibilityHint` und-Eigenschaften für Entwickler bereit, um beschreibenden Text hinzuzufügen, der vom VoiceOver-Bildschirm Reader verwendet werden kann, um den Zugriff auf die Steuerelemente zu erleichtern Steuerelemente können auch mit einem oder mehreren Merkmalen gekennzeichnet werden, die zusätzlichen Kontext in zugänglichen Modi bereitstellen.

Einige Steuerelemente müssen möglicherweise nicht zugänglich sein (z. b. eine Bezeichnung in einer Texteingabe oder ein Bild, das rein dekorativ `IsAccessibilityElement` ist) – das wird bereitgestellt, um die Barrierefreiheit in diesen Fällen zu deaktivieren.

**UI-Designer**

Die **Eigenschaftenpad** enthält einen Abschnitt zur Barrierefreiheit, mit dem diese Einstellungen bearbeitet werden können, wenn ein Steuerelement im IOS-Benutzeroberflächen-Designer ausgewählt wird:

![](accessibility-images/ios-designer-sml.png "Barrierefreiheits Einstellungen")

**C#**

Diese Eigenschaften können auch direkt im Code festgelegt werden:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Was ist accessibilityidentifier?

Wird `AccessibilityIdentifier` verwendet, um einen eindeutigen Schlüssel festzulegen, der zum Verweisen auf Benutzeroberflächen Elemente über die UIAutomation-API verwendet werden kann.

Der Wert von `AccessibilityIdentifier` wird niemals gesprochen oder für den Benutzer angezeigt.

<a name="postnotification" />

## <a name="postnotification"></a>Postnotification

Mit `UIAccessibility.PostNotification` der-Methode können Ereignisse außerhalb der direkten Interaktion an den Benutzer ausgelöst werden (z. b. Wenn Sie mit einem bestimmten Steuerelement interagieren).

### <a name="announcement"></a>Ankündigung

Eine Ankündigung kann vom Code gesendet werden, um den Benutzer darüber zu informieren, dass sich ein Zustand geändert hat (z. b. Wenn ein Hintergrund Vorgang abgeschlossen wurde). Dies kann mit einem visuellen Hinweis auf der Benutzeroberfläche einhergehen:

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

Barrierefreiheits Eigenschaften wie die Bezeichnung und der Hinweis können genau wie andere Text in der Benutzeroberfläche lokalisiert werden.

**MainStoryboard.strings**

Wenn die Benutzeroberfläche in einem Storyboard angeordnet ist, können Sie Übersetzungen für Barrierefreiheits Eigenschaften auf die gleiche Weise wie andere Eigenschaften bereitstellen. Im folgenden Beispiel verfügt ein `UITextField` über eine Lokalisierungs- **ID** von `Pqa-aa-ury` und zwei Barrierefreiheits Eigenschaften, die auf Spanisch festgelegt werden:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Diese Datei befindet sich im Verzeichnis " **es. lproj** " für spanische Inhalte.

**Localizable.strings**

Alternativ können die Übersetzungen der **lokalisierbaren Strings** -Datei im lokalisierten Inhaltsverzeichnis hinzugefügt werden (z. b. **es. lproj** für Spanisch):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Diese Übersetzungen können in C# über die `LocalizedString` -Methode verwendet werden:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Weitere Informationen zum Lokalisieren von Inhalten finden Sie im [IOS-Lokalisierungs Handbuch](~/ios/app-fundamentals/localization/index.md) .

<a name="testing" />

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

VoiceOver ist in der App " **Einstellungen** " aktiviert, indem Sie zu **Allgemein > Barrierefreiheits > VoiceOver**navigieren:

![](accessibility-images/settings-sml.png "Festlegen der Sprech Rate")

Der Bildschirm **Barrierefreiheit** enthält auch Einstellungen für Zoom, Textgröße, Farbe & Kontrast Optionen, Spracheinstellungen und andere Konfigurationsoptionen.

Befolgen Sie diese [VoiceOver-Anweisungen](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) , um die Barrierefreiheit auf IOS-Geräten zu testen.


## <a name="simulator-testing"></a>Simulatortests

Wenn Sie im Simulator testen, steht Ihnen der **Barrierefreiheits Inspektor** zur Verfügung, um zu überprüfen, ob Barrierefreiheits Eigenschaften und Ereignisse richtig konfiguriert sind. Aktivieren Sie den Inspektor in der App " **Einstellungen** ", indem Sie zu **Allgemein > Barrierefreiheit > Barrierefreiheits Inspektor**navigieren:

![](accessibility-images/settings-inspector-sml.png "Zugriffs Inspektor aktivieren")

Nach der Aktivierung zeigt das Inspektor-Fenster jederzeit auf den IOS-Bildschirm.
Im folgenden finden Sie ein Beispiel für die Ausgabe, wenn eine Tabellen Ansichts Zeile ausgewählt ist – beachten Sie, dass die **Bezeichnung** einen Satz enthält, der den Inhalt der Zeile und auch "Done" enthält (d. h. der Tick ist sichtbar):

![](accessibility-images/tableview-a11y-sml.png "Verwenden des Zugriffs Inspektors")

Während der Inspektor sichtbar ist, verwenden Sie das Symbol "X" in der oberen linken Ecke, um das Overlay vorübergehend anzuzeigen und auszublenden und Barrierefreiheits Einstellungen zu aktivieren/deaktivieren.



## <a name="related-links"></a>Verwandte Links

- [Cross-platform Accessibility (Plattformübergreifende Barrierefreiheit)](~/cross-platform/app-fundamentals/accessibility.md)
- [IOS-Barrierefreiheit (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [IOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
