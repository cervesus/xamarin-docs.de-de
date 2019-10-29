---
title: Einführung in Storyboards in xamarin. Mac
description: Dieser Artikel bietet eine Einführung in die Arbeit mit Storyboards in einer xamarin. Mac-app. Sie erfahren, wie Sie die UI der App mit Storyboards und Interface Builder von Xcode erstellen und verwalten.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: b27a8d65ebaca6009d8310931b9dac3a4d7e12f3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026149"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Einführung in Storyboards in xamarin. Mac

_Dieser Artikel bietet eine Einführung in die Arbeit mit Storyboards in einer xamarin. Mac-app. Es behandelt die Erstellung und Verwaltung der Benutzeroberfläche der App mithilfe von Storyboards und der Interface Builder von Xcode._

Storyboards ermöglichen es Ihnen, eine Benutzeroberfläche für die xamarin. Mac-app zu entwickeln, die nicht nur die Fenster Definitionen und Steuerelemente enthält, sondern auch die Verknüpfungen zwischen verschiedenen Fenstern (über Seiten und Ansichts Zuständen) enthält.

[![](images/intro01.png "A sample UI in Xcode")](images/intro01.png#lightbox)

Dieser Artikel bietet eine Einführung in die Verwendung von Storyboards zum Definieren der Benutzeroberfläche einer xamarin. Mac-app.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Was sind Storyboards?

Mithilfe von Storyboards kann die gesamte Benutzeroberfläche einer xamarin. Mac-app an einer einzigen Stelle mit sämtlicher Navigation zwischen den einzelnen Elementen und Benutzeroberflächen definiert werden. Storyboards für xamarin. Mac funktionieren ähnlich wie Storyboards für xamarin. IOS. Sie enthalten jedoch aufgrund der unterschiedlichen Schnittstellen Ausdrücke einen anderen Satz von _segue-Typen_ .

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Arbeiten mit Kulissen

Wie bereits erwähnt, definiert ein Storyboard alle Benutzeroberflächen für eine bestimmte APP, die in einer funktionalen Übersicht der zugehörigen _Ansichts Controller_aufgeschlüsselt sind. In der Interface Builder von Xcode befindet sich jeder dieser Controller in einer eigenen _Szene_.

[![](images/intro02.png "An example view controller")](images/intro02.png#lightbox)

Jede Szene stellt ein angegebenes Ansichts-und Ansichts Controller Paar mit einem Satz von Zeilen (sogenannten Segues) dar, die jede Szene in der Benutzeroberfläche verbinden und so ihre Beziehungen anzeigen. Einige-Elemente definieren, wie ein Ansichts Controller eine oder mehrere untergeordnete Sichten oder Ansichts Controller enthält. Andere Segues definieren Übergänge zwischen Ansichts Controller (z. b. das Anzeigen eines popover oder Dialog Felds). 

[![](images/intro03.png "A sample segue")](images/intro03.png#lightbox)

Wichtig zu beachten ist, dass jeder-Typ den Datenfluss zwischen dem angegebenen Element der Benutzeroberfläche der APP darstellt.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Arbeiten mit Ansichts Controllern

Ansichts Controller definieren die Beziehungen zwischen einer bestimmten Ansicht von Informationen in einer Mac-app und dem Datenmodell, das diese Informationen bereitstellt. Jede Szene der obersten Ebene im Storyboard stellt einen Ansichts Controller im Code der xamarin. Mac-app dar.

[![](images/intro04.png "An example slips view controller")](images/intro04.png#lightbox)

Auf diese Weise ist jeder Ansichts Controller eine eigenständige, wiederverwendbare Kopplung der visuellen Darstellung der Informationen (Ansicht) und der Logik, um diese Informationen darzustellen und zu steuern.

Innerhalb einer bestimmten Szene können Sie alle Dinge durchführen, die normalerweise von einzelnen `.xib` Dateien behandelt wurden: 

- Platzieren Sie unter Ansichten und Steuerelemente (z. b. Schaltflächen und Textfelder).
- Definieren von Element Positionen und automatischen Layouteinschränkungen.
- Verbindungs Aktionen und Outlets zum verfügbar machen von UI-Elementen für Code.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Arbeiten mit einem anderen

Wie bereits erwähnt, stellen die Beziehungen zwischen allen Szenen bereit, in denen die Benutzeroberfläche Ihrer APP definiert ist. Wenn Sie mit der Arbeit in Storyboards in ios vertraut sind, wissen Sie, dass die Seiten für IOS in der Regel Übergänge zwischen den voll Bildansichten definieren. Dies unterscheidet sich von macOS, wenn in den Parameters in der Regel "Containment" definiert ist (wobei eine Szene das untergeordnete Element einer übergeordneten Szene ist).

Unter macOS gruppieren die meisten apps ihre Ansichten tendenziell innerhalb desselben Fensters mithilfe von Benutzeroberflächen Elementen wie z. b. geteilten Sichten und Registerkarten. Anders als bei IOS, bei dem Ansichten aufgrund von eingeschränktem physischem Anzeigebereich ein-und ausgeschaltet werden müssen.

Bei den Tendenzen von macOS in Bezug auf die _Kapselung_ gibt es Situationen, in denen Präsentationsseiten verwendet werden, wie z. b. modale Fenster, Blatt Ansichten und popovers.

Wenn Sie präsentationsegues verwenden, können Sie die `PrepareForSegue`-Methode des übergeordneten Ansichts Controllers überschreiben, um die Darstellung und Variablen zu initialisieren und alle Daten für den Ansichts Controller bereitzustellen, der angezeigt wird.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Entwurfs-und Laufzeiten

Zur Entwurfszeit (beim Layout der Benutzeroberfläche in Xcode-Interface Builder) wird jedes Element der Benutzeroberfläche der app in seine Bestandteile aufgeteilt:

- **Szenen** , die Folgendes umfassen:
  - **Ansichts Controller** : Hiermit werden die Beziehungen zwischen Sichten und den Daten definiert, die Sie unterstützen.
  - **Sichten und unter Ansichten** : die eigentlichen Elemente, die die Benutzeroberfläche bilden.
  - **Containment** -Elemente, die die Beziehungen zwischen übergeordneten und untergeordneten Elementen definieren.
- **Darstellungs** Modi, die einzelne präsentationsmodi definieren. 

Indem jedes Element auf diese Weise definiert wird, ermöglicht es das verzögerte Laden der einzelnen Elemente nur, da es während der Laufzeit benötigt wird. In macOS war der gesamte Prozess so konzipiert, dass der Entwickler komplexe, flexible Benutzeroberflächen erstellen kann, die ein Minimum an sicherem Code erfordern, damit Sie funktionieren und so effizient mit Systemressourcen wie möglich sind.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Storyboard-Schnellstart

Im [Storyboard-Schnellstart](~/mac/platform/storyboards/quickstart.md) Handbuch erstellen wir eine einfache xamarin. Mac-app, die die wichtigsten Konzepte für die Arbeit mit Storyboards zum Erstellen einer Benutzeroberfläche einführt. Die Beispiel-App besteht aus einer Überlauf Ansicht, die einen _Inhalts Bereich_ und einen _Inspektor-Bereich_ enthält, und es wird ein einfaches Einstellungs Dialogfeld angezeigt. Wir verwenden die Elemente, um alle Benutzeroberflächen Elemente miteinander zu verknüpfen.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Arbeiten mit Storyboards

In diesem Abschnitt werden ausführliche Informationen zum [Arbeiten mit Storyboards](~/mac/platform/storyboards/indepth.md) in einer xamarin. Mac-app behandelt. Wir sehen uns die Kulissen genauer an und wie Sie sich aus Ansichts Controllern und Ansichten zusammensetzen. Dann sehen wir uns an, wie Szenen miteinander verknüpft werden. Und schließlich wird die Arbeit mit benutzerdefinierten Enumerationstypen behandelt. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Beispiel für ein komplexes Storyboard

Ein Beispiel für ein komplexes Beispiel für das Arbeiten mit Storyboards in einer xamarin. Mac-app finden Sie in der [sourcewriter](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)-Beispiel-app. SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird kurz auf das Arbeiten mit Storyboards in einer xamarin. Mac-app Bezug genommen. Wir haben gesehen, wie Sie eine neue App mithilfe von Storyboards erstellen und wie Sie eine Benutzeroberfläche definieren. Wir haben auch gesehen, wie zwischen verschiedenen Fenstern und Ansichts Zuständen mithilfe von-Zuständen navigiert wird.

## <a name="related-links"></a>Verwandte Links

- [„Hallo, Mac“-Beispiel](https://docs.microsoft.com/samples/xamarin/mac-samples/hello-mac)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
