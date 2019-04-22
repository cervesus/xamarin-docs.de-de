---
title: Benutzerdefinierte Steuerelemente in der XAML-Vorschau zu rendern
description: In diesem Artikel wird beschrieben, wie Sie Ihre benutzerdefinierten Steuerelemente in der XAML-Vorschau anzeigen.
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 977c29312e0be8b92f216c224414f9bd03f8562d
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58858970"
---
# <a name="render-custom-controls-in-the-xaml-previewer"></a>Benutzerdefinierte Steuerelemente in der XAML-Vorschau zu rendern

_Benutzerdefinierte Steuerelemente funktioniert nicht in einigen Fällen wie erwartet in der XAML-Vorschau. Verwenden Sie die Anleitung in diesem Artikel, um die Einschränkungen der Vorschau von Ihrem benutzerdefinierten Steuerelementen zu verstehen._

## <a name="basic-preview-mode"></a>Standardmodus (Vorschau)

Auch wenn Sie Ihr Projekt erstellt haben, wird die XAML-Vorschau Ihrer Seiten gerendert. Bis Sie erstellt haben, wird jedes Steuerelement, das Code-Behind basiert Xamarin.Forms-Basistyp angezeigt. Wenn Ihr Projekt erstellt wird, versucht die XAML-Vorschau zum Anzeigen von benutzerdefinierter Steuerelementen mit Design Time Rendering. Wenn die renderschaltfläche fehlschlägt, wird den Basistyp für Xamarin.Forms angezeigt.

## <a name="enable-design-time-rendering-for-custom-controls"></a>Aktivieren der Design-Time Rendering für benutzerdefinierte Steuerelemente

Wenn Sie Ihre eigenen benutzerdefinierten Steuerelemente stellen oder Steuerelemente von einer Drittanbieter-Bibliothek verwenden, können Sie durch die Vorschau nicht ordnungsgemäß angezeigt werden. Benutzerdefinierte Steuerelemente müssen sich entscheiden, zum Entwerfen von Zeit zu rendern, in der Vorschau angezeigt werden soll, ob Sie das Steuerelement geschrieben oder aus einer Bibliothek importiert werden. Mit Steuerelementen, die Sie erstellt haben, fügen die [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute) Ihres Steuerelements-Klasse, um sie in der Vorschau anzuzeigen:

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

Verwendung [montemagnoss ImageCirclePlugins Basisklasse](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs) als Beispiel.


## <a name="skiasharp-controls"></a>SkiaSharp-Steuerelemente

SkiaSharp-Steuerelemente werden derzeit nur unterstützt, wenn unter iOS in der Vorschau angezeigt werden. Sie wird nicht auf die Android-Vorschau gerendert.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="check-your-xamarinforms-version"></a>Überprüfen Sie Ihre Xamarin.Forms-version
Stellen Sie sicher, dass mindestens Xamarin.Forms 3.6 installiert. Sie können Ihre Xamarin.Forms-Version auf NuGet aktualisieren.

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>Selbst bei `[DesignTimeVisible(true)]`, meines benutzerdefinierten Steuerelements ist nicht ordnungsgemäß gerendert.
Benutzerdefinierte Steuerelemente, die häufig auf Code-Behind- oder Back-End-Daten basieren, funktionieren nicht immer in der XAML-Vorschau. Sie können Folgendes versuchen:
* Verschieben das Steuerelement aus, damit er nicht initialisiert, dass wenn [Entwurfsmodus aktiviert ist](index.md#detect-design-mode)
* Einrichten von [entwerfen Zeitdaten](design-time-data.md) gefälschte Daten aus dem Back-End anzeigen

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>Die XAML-Vorschau, wird die Fehlermeldung "Benutzerdefinierte Steuerelemente sind nicht ordnungsgemäß gerendert."
Versuchen Sie es zu bereinigen und das Projekt, erneut erstellen oder schließen und erneuten Öffnen der XAML-Datei.
