---
Title: "Rendering von benutzerdefinierten Steuerelementen im XAML-Previewer" Description: "in diesem Artikel wird beschrieben, wie Sie Ihre benutzerdefinierten Steuerelemente im XAML-Previewer anzeigen."
ms. Prod: xamarin ms. assetid: 4d795372-cb8f -48f 4-b63d-845e44b261f 7 ms. Technology: xamarin-Forms Author: maddyleger1 ms. Author: maleger ms. Date: 03/27/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="render-custom-controls-in-the-xaml-previewer"></a>Rendering von benutzerdefinierten Steuerelementen im XAML-Previewer

_Benutzerdefinierte Steuerelemente funktionieren manchmal nicht wie erwartet im XAML-Previewer. Verwenden Sie die Anleitungen in diesem Artikel, um die Einschränkungen der Vorschau der benutzerdefinierten Steuerelemente zu verstehen._

## <a name="basic-preview-mode"></a>Basic-Vorschaumodus

Auch wenn Sie Ihr Projekt nicht erstellt haben, werden die Seiten von der XAML-Vorschau gerenkt. Bis zum Erstellen zeigt jedes Steuerelement, das auf Code Behind basiert, seinen Xamarin.Forms Basistyp an. Wenn das Projekt erstellt wurde, versucht der XAML-previewer, benutzerdefinierte Steuerelemente mit aktiviertem Entwurfszeit Rendering anzuzeigen. Wenn das Rendering fehlschlägt, wird der Xamarin.Forms Basistyp angezeigt.

## <a name="enable-design-time-rendering-for-custom-controls"></a>Aktivieren des Entwurfszeit Rendering für benutzerdefinierte Steuerelemente

Wenn Sie eigene benutzerdefinierte Steuerelemente erstellen oder Steuerelemente aus einer Bibliothek eines Drittanbieters verwenden, zeigt der Previewer diese möglicherweise falsch an. Benutzerdefinierte Steuerelemente müssen sich dafür entscheiden, das Zeit Rendering zu entwerfen, das in der Vorschau angezeigt wird, unabhängig davon, ob Sie das Steuerelement geschrieben oder aus einer Bibliothek importiert haben. Fügen Sie mit den von Ihnen erstellten Steuerelementen der [`[DesignTimeVisible(true)]`](xref:System.ComponentModel.DesignTimeVisibleAttribute) -Klasse Ihres Steuer Elements hinzu, um es in der Previewer anzuzeigen:

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

Verwenden Sie [die-Basisklasse von James Montemagno](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs) als Beispiel.

## <a name="skiasharp-controls"></a>Skiasharp-Steuerelemente

Derzeit werden skiasharp-Steuerelemente nur unterstützt, wenn Sie eine Vorschau auf IOS anzeigen. Sie werden nicht in der Android-Vorschau gerenbt.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="check-your-xamarinforms-version"></a>Überprüfen Sie Ihre Xamarin.Forms Version
Stellen Sie sicher, dass mindestens Xamarin.Forms 3,6 installiert ist. Sie können Ihre Xamarin.Forms Version auf nuget aktualisieren.

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>Auch mit `[DesignTimeVisible(true)]` wird das benutzerdefinierte Steuerelement nicht ordnungsgemäß gerendert.
Benutzerdefinierte Steuerelemente, die sich stark auf Code-Behind-oder Back-End-Daten stützen, funktionieren nicht immer im XAML-Previewer. Sie können Folgendes ausprobieren:

* Verschieben des Steuer Elements, sodass es nicht initialisiert wird, wenn der [Entwurfs Modus aktiviert ist](index.md#detect-design-mode)
* Einrichten von [Entwurfszeit Daten](design-time-data.md) zum Anzeigen gefälschter Daten aus dem Back-End

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>Der XAML-Previewer zeigt den Fehler "benutzerdefinierte Steuerelemente werden nicht ordnungsgemäß gerendert"
Versuchen Sie, das Projekt zu bereinigen und neu zu erstellen, oder schließen Sie die XAML-Datei, und
