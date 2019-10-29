---
title: Barrierefreiheit unter macOS
description: In diesem Dokument wird beschrieben, wie Sie in einer xamarin. Mac-app mit den Funktionen für die macOS-Barrierefreiheit arbeiten. Es wird erläutert, wie Benutzeroberflächen Elemente in Storyboards und Code, benutzerdefinierten Steuerelementen und das Testen der Barrierefreiheit beschrieben werden
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 3f3b9c84fad0bce8939187fcd0c91d18314ce8ab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032640"
---
# <a name="accessibility-on-macos"></a>Barrierefreiheit unter macOS

Auf dieser Seite wird beschrieben, wie Sie die Eingabe Hilfen-APIs für die Eingabe Hilfen verwenden, um apps entsprechend der [Eincheck Checkliste](~/cross-platform/app-fundamentals/accessibility.md)
Weitere Plattform-APIs finden Sie unter [Android-Barrierefreiheit](~/android/app-fundamentals/accessibility.md) und [IOS-Barrierefreiheits](~/ios/app-fundamentals/accessibility.md) Seiten.

Um zu verstehen, wie die Barrierefreiheits-APIs in macOS funktionieren (früher als OS x bezeichnet), überprüfen Sie zuerst das [OS x-Barrierefreiheits Modell](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Beschreiben von UI-Elementen

AppKit verwendet das `NSAccessibility`-Protokoll, um APIs verfügbar zu machen, mit denen die Benutzeroberfläche zugänglich gemacht werden kann. Dies schließt ein Standardverhalten ein, das versucht, sinnvolle Werte für Barrierefreiheits Eigenschaften festzulegen, z. b. das Festlegen der `AccessibilityLabel` einer Schaltfläche. Die Bezeichnung ist in der Regel ein einzelnes Wort oder ein kurzer Ausdruck, der das Steuerelement oder die Ansicht beschreibt.

### <a name="storyboard-files"></a>Storyboarddateien

Xamarin. Mac verwendet die Xcode-Interface Builder, um storyboarddateien zu bearbeiten.
Informationen zur Barrierefreiheit können im **Identitäts Inspektor** bearbeitet werden, wenn ein Steuerelement auf der Entwurfs Oberfläche ausgewählt wird (wie im folgenden Screenshot gezeigt):

[![Hinzufügen von Barrierefreiheit in der Interface Builder von Xcode](accessibility-images/xcode.png "Hinzufügen von Barrierefreiheit in der Interface Builder von Xcode")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Code

Xamarin. Mac macht derzeit nicht als `AccessibilityLabel` Setter verfügbar.  Fügen Sie die folgende Hilfsmethode hinzu, um die Barrierefreiheits Bezeichnung festzulegen:

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Diese Methode kann dann wie gezeigt im Code verwendet werden:

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

Die `AccessibilityHelp`-Eigenschaft ist eine Erläuterung der Funktionsweise des Steuer Elements oder der Ansicht und sollte nur dann hinzugefügt werden, wenn die Bezeichnung möglicherweise keine ausreichenden Informationen bereitstellt. Der Hilfetext sollte weiterhin so kurz wie möglich gehalten werden, z. b. "löscht das Dokument".

Einige Elemente der Benutzeroberfläche sind für den zugänglichen Zugriff nicht relevant (z. b. eine Bezeichnung neben einer Eingabe, die über eine eigene Barrierefreiheits Bezeichnung und Hilfe verfügt).
Legen Sie in diesen Fällen `AccessibilityElement = false` fest, damit diese Steuerelemente oder Ansichten von Sprachausgaben oder anderen Eingabe Hilfen übersprungen werden.

Apple bietet [Richtlinien für Barrierefreiheit](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) , in denen die bewährten Methoden für Eingabe Hilfen und Hilfe Text erläutert werden.

## <a name="custom-controls"></a>Benutzerdefinierte Steuerelemente

Ausführliche Informationen zu den zusätzlichen erforderlichen Schritten finden Sie in den [Richtlinien von Apple für barrierefreie benutzerdefinierte Steuerelemente](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) .

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

macOS bietet einen **Barrierefreiheits Inspektor** , mit dem Barrierefreiheits Funktionen getestet werden können. Der Inspektor ist in Xcode enthalten.

Beim ersten Start benötigt der **Zugriffs Steuerungs Inspektor** die Berechtigung zum Steuern des Computers über Barrierefreiheit:

![Zugriffs Inspektor, der die Berechtigung zum Ausführen anfordert](accessibility-images/accessibility-inspector-1.png "Zugriffs Inspektor, der die Berechtigung zum Ausführen anfordert")

Entsperren Sie den Bildschirm "Einstellungen" (falls erforderlich, in der linken unteren Ecke), und klicken Sie auf den **Barrierefreiheits Inspektor**:

![Bildschirm "Einstellungen" zum Aktivieren der Zugriffstaste](accessibility-images/accessibility-inspector-2.png "Bildschirm "Einstellungen" zum Aktivieren der Zugriffstaste")

Nach der Aktivierung wird der Inspektor als ein unverankertes Fenster angezeigt, das auf dem Bildschirm verschoben werden kann. Der folgende Screenshot zeigt den Inspektor, der neben einer Mac-Beispiel-app ausgeführt wird. Wenn der Cursor über das Fenster bewegt wird, zeigt der Inspektor alle zugänglichen Eigenschaften der einzelnen Steuerelemente an:

[![Beispiel für die Ausführung des Zugriffs Inspektors](accessibility-images/accessibility-example.png "Beispiel für die Ausführung des Zugriffs Inspektors")](accessibility-images/accessibility-example-large.png#lightbox)

Weitere Informationen finden Sie im [Leitfaden Testen von Barrierefreiheit für OS X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).

## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac-Barrierefreiheit](https://www.apple.com/accessibility/mac/)
