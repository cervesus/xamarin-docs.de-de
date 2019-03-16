---
title: Häufig gestellte Fragen zu Xamarin.Forms
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
---

# <a name="xamarinforms-frequently-asked-questions"></a>Häufig gestellte Fragen zu Xamarin.Forms

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Kann ich die Xamarin.Forms-Standardvorlage auf ein neueres NuGet-Paket aktualisieren?](update-forms-template.md)
Dieser Anleitung wird die Vorlage für Xamarin.Forms .NET Standard-Bibliothek als Beispiel verwendet, aber die gleiche allgemeine Methode funktioniert auch für die Vorlage für freigegebene Xamarin.Forms-Projekt.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms-XAML-Dateien?](forms-xaml-designer.md)
Xamarin.Forms unterstützt derzeit keine visuelle Designer für XAML-Dateien.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Android-Buildfehler: Unerwarteter Fehler beim Task „LinkAssemblies“](android-linkassemblies-error.md)
Sie können eine Fehlermeldung angezeigt `The "LinkAssemblies" task failed unexpectedly` Forms Wenn ein Xamarin.Android-Projekt erstellen, die verwendet wird. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf ein *Version* Build zum Verringern der Größe des app-Pakets), und es tritt auf, da der Android-Ziele, die neueste Framework nicht aktualisiert werden. 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["Warum schlägt mein Xamarin.Forms.Maps-Android-Projekt fehl mit COMPILETODALVIK: UNERWARTETER AUF OBERSTER EBENE FEHLER?"](maps-compiletodalvik-error.md)
Dieser Fehler kann in der Fehlerbereich von Visual Studio für Mac oder im Fenster "Buildausgabe" von Visual Studio angezeigt werden; in der Android-Projekte mit Xamarin.Forms.Maps. Es wird am häufigsten behoben, durch das Erhöhen der Java-Heapgröße für Xamarin.Android-Projekt.
