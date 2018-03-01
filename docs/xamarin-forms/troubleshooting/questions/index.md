---
title: "Häufig gestellte Fragen (FAQs)"
ms.topic: article
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 95728bf6bf1009db1cc834bf1d9d0be6a8fc5ef5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen (FAQs)


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Kann die Xamarin.Forms-Standardvorlage auf eine neuere NuGet-Paket werden aktualisiert?](update-forms-template.md)
Dieses Handbuch verwendet die Xamarin.Forms-PCL-Vorlage als Beispiel, aber die gleiche allgemeine Methode funktioniert auch für die freigegebene Xamarin.Forms-Projektvorlage. 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Warum funktioniert nicht Visual Studio XAML-Designer für Xamarin.Forms XAML-Dateien?](forms-xaml-designer.md)
Xamarin.Forms unterstützt keine derzeit visuelle Designer für XAML-Dateien.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Android Buildfehler: Unerwarteter Fehler bei die Aufgabe "LinkAssemblies"](android-linkassemblies-error.md)
Möglicherweise folgende Fehlermeldung `The "LinkAssemblies" task failed unexpectedly` beim Erstellen eines Projekts Xamarin.Android, verwendet Formulare. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf ein *Version* Build zum Verringern der Größe des app-Pakets); und es tritt auf, da Android Zielen auf die neueste Framework aktualisiert werden. 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["Warum Meine Xamarin.Forms.Maps Android-Projekt nicht mit COMPILETODALVIK: UNERWARTETER Fehler von der obersten Ebene?"](maps-compiletodalvik-error.md)
Dieser Fehler kann in das Auffüllzeichen Fehler von Visual Studio für Mac oder im Fenster Ausgabe Erstellen von Visual Studio angezeigt werden; in der Android-Projekte mit Xamarin.Forms.Maps. Es wird am häufigsten behoben, durch die Java-Heapgröße für Ihr Projekt Xamarin.Android erhöhen.

