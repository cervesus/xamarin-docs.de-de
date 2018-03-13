---
title: Zugriff auf macOS
description: "Dieses Handbuch beschreibt die Features für die Erstellung einer Xamarin.Mac-Anwendung."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4363258a9047ee4e2de4f53595a6eedc5dfe5861
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="accessibility-on-macos"></a>Zugriff auf macOS

Diese Seite beschreibt, wie die MacOS Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Eingabehilfen Prüfliste](~/cross-platform/app-fundamentals/accessibility.md).
Finden Sie in der [Android Eingabehilfen](~/android/app-fundamentals/accessibility.md) und [iOS Eingabehilfen](~/ios/app-fundamentals/accessibility.md) Seiten für andere Plattform-APIs.

Um die Arbeitsweise der Zugriff auf APIs in MacOS (ehemals: OS X), erste Überprüfung der [Zugriffsmodell für OS X](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Beschreibt die Elemente der Benutzeroberfläche

AppKit verwendet die `NSAccessibility` Protokoll APIs verfügbar zu machen, mit deren Hilfe die Benutzeroberfläche zugänglich zu machen. Dies schließt ein Standardverhalten aus, die versucht, legen Sie sinnvolle Werte für Eingabehilfeeigenschaften, z. B. einer Schaltfläche festlegen `AccessibilityLabel`. Die Bezeichnung ist in der Regel ein einzelnes Wort oder eine kurze Beschreibung des Steuerelements oder eine Sicht Ausdruck.

### <a name="storyboard-files"></a>Storyboard-Dateien

Xamarin.Mac verwendet die Xcode-Benutzeroberflächen-Generator Storyboard-Dateien zu bearbeiten.
Informationen über Eingabehilfen kann bearbeitet werden, der **Identität Inspektor** Wenn ein Steuerelement aktiviert ist auf der Entwurfsoberfläche angezeigt (wie im folgenden Screenshot gezeigt):

[![Hinzufügen von Eingabehilfen in Xcodes Benutzeroberflächen-Generator](accessibility-images/xcode.png "Barrierefreiheit in Xcodes Benutzeroberflächen-Generator hinzufügen")](accessibility-images/xcode-large.png#lightbox)

### <a name="code"></a>Code

Xamarin.Mac macht derzeit nicht als `AccessibilityLabel` Setter-Methode.  Fügen Sie die folgende Hilfsmethode, um die Eingabehilfen Bezeichnung festgelegt:

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

Die `AccessibilityHelp` Eigenschaft ist eine Erläuterung der Wirkungsweise der Steuerelement- oder die Sicht und sollten nur hinzugefügt werden, wenn die Bezeichnung keine ausreichenden Informationen bereitstellt, kann. Der Hilfetext sollten weiterhin beibehalten werden so kurz wie möglich, für das Beispiel "löscht das Dokument".

Einige Benutzeroberflächenelemente sind nicht relevant für den Zugriff (z. B. eine Bezeichnung neben einer Eingabe, die eine eigene Eingabehilfen Bezeichnung und die Hilfe hat).
Legen Sie in diesen Fällen `AccessibilityElement = false` , damit diese Steuerelemente oder Sichten von Bildschirmleseprogrammen oder anderen Tools für Barrierefreiheit übersprungen werden.

Apple bietet [Richtlinien für Eingabehilfen](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) , erklärt die bewährten Methoden für Eingabehilfen-Text für Bezeichnungen und Hilfe.

## <a name="custom-controls"></a>Benutzerdefinierte Steuerelemente

Finden Sie in der Apple- [Richtlinien für zugegriffen werden kann, benutzerdefinierte Steuerelemente](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) ausführliche Informationen über die weiteren Schritte erforderlich.

## <a name="testing-accessibility"></a>Barrierefreiheit

MacOS bietet eine **Eingabehilfen Inspektor** , hilft Barrierefreiheitsfunktionen testen. Der Inspektor wird in Xcode enthalten.

Beim ersten es gestartet wird, die **Eingabehilfen Inspektor** benötigen die Berechtigung zum Steuern des Computers über Zugriff auf:

![Anfordern der Berechtigung zum Ausführen von Eingabehilfen-Inspektor](accessibility-images/accessibility-inspector-1.png "Anfordern der Berechtigung zum Ausführen von Eingabehilfen-Inspektor")

Der Bildschirm "Einstellungen" (falls auf der linken unteren Ecke erforderlich) und Teilstriche Entsperren der **Eingabehilfen Inspektor**:

![Bildschirm "Einstellungen" Aktivieren von Eingabehilfen-Inspektor](accessibility-images/accessibility-inspector-2.png "Bildschirm "Einstellungen" Aktivieren von Eingabehilfen-Inspektor")

Nach der Aktivierung wird der Inspektor angezeigt, als ein unverankertes Fenster, das auf dem Bildschirm verschoben werden kann. Der folgende Screenshot zeigt den Inspektor neben einem Beispiel-app für Mac ausgeführt wird. Wie das Fenster der Mauszeiger bewegt wird, werden der Inspektor alle zugegriffen werden kann Eigenschaften der einzelnen Steuerelemente angezeigt:

[![Beispiel für Eingabehilfen Inspektor ausführen](accessibility-images/accessibility-example.png "Beispiel von Eingabehilfen-Inspektor ausgeführt wird")](accessibility-images/accessibility-example-large.png#lightbox)

Weitere Informationen finden Sie unter der [Barrierefreiheit für OS X-Handbuch](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Eingabehilfen](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac-Eingabehilfen](https://www.apple.com/accessibility/mac/)
