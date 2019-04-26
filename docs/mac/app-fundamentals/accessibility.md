---
title: Eingabehilfen unter macOS
description: Dieses Dokument beschreibt, wie Sie mit Funktionen zur Barrierefreiheit von MacOS in einer Xamarin.Mac-app arbeiten. Es wird erläutert, beschreibt Benutzeroberflächenelemente in Storyboards und Code, benutzerdefinierte Steuerelemente und Zugriff auf Tests.
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: fdda52309ffdb0d32cc42a4dff052cd9050b1e4f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378477"
---
# <a name="accessibility-on-macos"></a>Eingabehilfen unter macOS

Diese Seite beschreibt, wie der MacOS-Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Checkliste für die Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md).
Finden Sie in der [Android Barrierefreiheit](~/android/app-fundamentals/accessibility.md) und [iOS Barrierefreiheit](~/ios/app-fundamentals/accessibility.md) Seiten für andere Plattform-APIs.

Um die Funktionsweise von Eingabehilfen-APIs in MacOS (ehemals: OS X), erste Überprüfung der [OS X-Zugriffsmodell](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Beschreibt die Elemente der Benutzeroberfläche

AppKit verwendet die `NSAccessibility` Protokoll APIs verfügbar zu machen, mit deren Hilfe die Benutzeroberfläche zugänglich zu machen. Dies schließt ein Standardverhalten, das versucht, sinnvolle Werte für Eigenschaften von Bedienungshilfen, z. B. das Festlegen einer Schaltfläche festzulegen `AccessibilityLabel`. Die Bezeichnung ist in der Regel ein einzelnes Wort oder einen kurzen Ausdruck, die das Steuerelement oder der Ansicht beschreibt.

### <a name="storyboard-files"></a>Storyboard-Dateien

Xamarin.Mac verwendet die Interface Builder von Xcode, um Storyboard-Dateien zu bearbeiten.
Informationen zur Barrierefreiheit kann bearbeitet werden, der **identitätsinspektor** Wenn ein Steuerelement ausgewählt ist auf der Entwurfsoberfläche (wie im folgenden Screenshot gezeigt):

[![Hinzufügen von Eingabehilfen in Interface Builder von Xcode](accessibility-images/xcode.png "Hinzufügen von Eingabehilfen in Interface Builder von Xcode")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Code

Xamarin.Mac macht keine derzeit verfügbar als `AccessibilityLabel` Setter.  Fügen Sie die folgende Hilfsmethode, um die Barrierefreiheit Bezeichnung festgelegt:

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

Diese Methode kann dann im Code verwendet werden, wie gezeigt:

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

Die `AccessibilityHelp` Eigenschaft ist eine Erläuterung der Funktionsweise der-Steuerelement oder eine Sicht und sollte nur hinzugefügt werden, wenn die Bezeichnung keine ausreichenden Informationen bereitstellt, kann. Der Hilfetext sollten weiterhin beibehalten werden so kurz wie möglich, für das Beispiel "löscht das Dokument".

Einige Benutzeroberflächenelemente sind nicht relevant für zugänglichen Zugriff (z. B. eine Bezeichnung neben der Eingabe, die über eine eigene Eingabehilfen-Bezeichnung und eine Hilfe verfügt).
Legen Sie in diesen Fällen `AccessibilityElement = false` , damit diese Steuerelemente oder Ansichten von Bildschirmsprachausgaben sowie andere Tools für Barrierefreiheit wird übersprungen.

Apple bietet [Richtlinien für Eingabehilfen](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) , erläutert die bewährten Methoden für Eingabehilfen-Bezeichnungen und Hilfe-Text.

## <a name="custom-controls"></a>Benutzerdefinierte Steuerelemente

Finden Sie in Apple [Richtlinien für zugegriffen werden kann, benutzerdefinierte Steuerelemente](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) ausführliche Informationen über die weiteren Schritte erforderlich.

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

MacOS bietet eine **Barrierefreiheit Inspektor** , unterstützt die Barrierefreiheitsfunktionen testen. Der Inspector ist in Xcode enthalten.

Beim ersten Start, die **Barrierefreiheit Inspektor** benötigen die Berechtigung zum Steuern der Computer über Zugriff auf:

![Anfordern der Berechtigung zum Ausführen von Eingabehilfen-Inspektor](accessibility-images/accessibility-inspector-1.png "Anfordern der Berechtigung zum Ausführen von Eingabehilfen-Inspektor")

Entsperren Sie den Bildschirm "Einstellungen" (falls auf der linken unteren Ecke, erforderlich) und -Tick der **Barrierefreiheit Inspektor**:

![Bildschirm "Einstellungen" Barrierefreiheit Inspector aktivieren](accessibility-images/accessibility-inspector-2.png "Bildschirm \"Einstellungen\" auf die Barrierefreiheit Inspector zu aktivieren")

Nach der Aktivierung wird der Inspektor angezeigt, als unverankertes Fenster, das auf dem Bildschirm verschoben werden kann. Der folgende Screenshot zeigt den Inspektor neben einer Beispiel-app für Mac ausgeführt wird. Wie sich der Cursor über dem Fenster bewegt wird, zeigt der Inspektor alle zugegriffen werden Eigenschaften der einzelnen Steuerelemente:

[![Beispiel für die Barrierefreiheit Inspector ausführen](accessibility-images/accessibility-example.png "Beispiel der Barrierefreiheit Inspector ausführen")](accessibility-images/accessibility-example-large.png#lightbox)

Weitere Informationen finden Sie in der [Testen der Barrierefreiheit für OS X-Handbuch](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Verwandte Links

- [Cross-Platform-Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac-Barrierefreiheit](https://www.apple.com/accessibility/mac/)
