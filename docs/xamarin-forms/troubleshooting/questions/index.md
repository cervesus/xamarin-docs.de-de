---
Title: " Xamarin.Forms häufig gestellte Fragen" MS. Topic: Problembehandlung ms. Prod: xamarin ms. assetid: 89364175-53ba-4a09-b3e2-44ac67dd971c ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 04/25/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.FormsHäufig gestellte Fragen

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Kann ich die Xamarin.Forms Standardvorlage auf ein neueres nuget-Paket aktualisieren?](update-forms-template.md)
In dieser Anleitung Xamarin.Forms wird die Vorlage für die .NET Standard-Bibliothek als Beispiel verwendet, aber dieselbe allgemeine Methode funktioniert auch für die Vorlage für frei Xamarin.Forms gegebene Projekte.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms XAML-Dateien?](forms-xaml-designer.md)
Xamarin.Formsunterstützt derzeit keine visuellen Designer für XAML-Dateien.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Android-Buildfehler: Unerwarteter Fehler beim Task "linkassemblys".](android-linkassemblies-error.md)
Möglicherweise wird eine Fehlermeldung angezeigt `The "LinkAssemblies" task failed unexpectedly` , wenn Sie ein xamarin. Android-Projekt mit Formularen verwenden. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf einem *Releasebuild* , um die Größe des App-Pakets zu verringern). Dies liegt daran, dass die Android-Ziele nicht auf das neueste Framework aktualisiert werden. 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["Warum geht's Xamarin.Forms . Das Android-Projekt von Maps schlägt fehl mit compilejedalvik: Unerwarteter Fehler auf oberster Ebene? "](maps-compiletodalvik-error.md)
Dieser Fehler wird möglicherweise im Fehler Fenster von Visual Studio für Mac oder im Fenster "Buildausgabe" von Visual Studio angezeigt. in Android-Projekten, die verwenden Xamarin.Forms . Maps. Dies wird am häufigsten durch Erhöhen der Java-Heapgröße für Ihr xamarin. Android-Projekt gelöst.
