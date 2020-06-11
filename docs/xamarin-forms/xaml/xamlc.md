---
Title: "XAML-Kompilierung in Xamarin.Forms " Beschreibung: "in diesem Artikel wird erläutert, wie XAML optional mit dem Xamarin.Forms XAML-Compiler (xamlc) direkt in Intermediate Language (IL) kompiliert werden kann."
ms. Prod: xamarin ms. assetid: 9a2d10a6-5dfc-485 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 08/22/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xaml-compilation-in-xamarinforms"></a>XAML-Kompilierung inXamarin.Forms

_XAML kann optional auch direkt mit dem XAML-Compiler (XAMLC) in der Zwischensprache (Intermediate Language, IL) kompiliert werden._

Die XAML-Kompilierung bietet eine Reihe von Vorteilen:

- Er führt eine XAML-Überprüfung zur Kompilierzeit durch und benachrichtigt den Benutzer über eventuelle Fehler.
- Er entfernt etwas Lade- und Instanziierungszeit für XAML-Elemente.
- Er hilft bei der Verringerung der Dateigröße der finalen Assembly, indem XAML-Dateien nicht mehr eingeschlossen werden.

Die XAML-Kompilierung ist standardmäßig deaktiviert, um die Abwärtskompatibilität sicherzustellen. Sie kann auf der Assembly-und Klassenebene durch Hinzufügen des- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) Attributs aktiviert werden.

Im folgenden Codebeispiel wird das Aktivieren der XAML-Kompilierung auf Assemblyebene veranschaulicht:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

In diesem Beispiel wird die Überprüfung der Kompilierzeit des gesamten XAML-Code, der in der Assembly enthalten ist, durchgeführt, wobei XAML-Fehler zur Kompilierzeit statt zur Laufzeit gemeldet werden. Daher gibt das `assembly` Präfix des [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) Attributs an, dass das Attribut für die gesamte Assembly gilt.

> [!NOTE]
> Das [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) -Attribut und die- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) Enumeration befinden sich im- `Xamarin.Forms.Xaml` Namespace, der für deren Verwendung importiert werden muss.

Im folgenden Codebeispiel wird das Aktivieren der XAML-Kompilierung auf Klassenebene veranschaulicht:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

In diesem Beispiel wird die Kompilierzeit Überprüfung der XAML-Datei für die `HomePage` Klasse ausgeführt, und die Fehler werden als Teil des Kompilierungsprozesses gemeldet.

> [!NOTE]
> Kompilierte Bindungen können aktiviert werden, um die Daten bindungsleistung in-Anwendungen zu verbessern Xamarin.Forms . Weitere Informationen finden Sie unter [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## <a name="related-links"></a>Verwandte Links

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
