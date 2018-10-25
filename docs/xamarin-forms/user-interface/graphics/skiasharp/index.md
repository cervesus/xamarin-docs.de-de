---
title: SkiaSharp-Grafiken in Xamarin.Forms
description: SkiaSharp ist ein 2D-Grafiken für .NET und c# unterstützt, die von der Open-Source-Skia-Grafik-Engine, die häufig in Google-Produkten verwendet wird. Dieses Handbuch erklärt, wie SkiaSharp für 2D-Grafiken in Ihrer Xamarin.Forms-Anwendungen verwendet wird.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 05c4b00f1551ffe21b3042a7da6bf0483dacf620
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "39615872"
---
# <a name="skiasharp-graphics-in-xamarinforms"></a>SkiaSharp-Grafiken in Xamarin.Forms

_Verwenden von SkiaSharp für 2D-Grafiken in Ihrer Xamarin.Forms-Anwendungen_

SkiaSharp ist ein 2D-Grafiken für .NET und c# unterstützt, die von der Open-Source-Skia-Grafik-Engine, die häufig in Google-Produkten verwendet wird. Sie können SkiaSharp in Ihrer Xamarin.Forms-Anwendungen verwenden, um 2D-Vektorgrafiken, Bitmaps und Text zu zeichnen. Finden Sie unter den [Direct2D-Zeichnung](~/graphics-games/skiasharp/index.md) Handbuch für weitere allgemeine Informationen über die SkiaSharp-Bibliothek und einige andere Lernprogramme.

Dieses Handbuch wird davon ausgegangen, dass Sie mit Xamarin.Forms-Programmierung vertraut sind.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinar: SkiaSharp für Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>SkiaSharp-Vorbereitungen

SkiaSharp für Xamarin.Forms wird als ein NuGet-Paket verpackt. Nachdem Sie eine Xamarin.Forms-Projektmappe in Visual Studio oder Visual Studio für Mac erstellt haben, können Sie den NuGet-Paket-Manager für die Suche nach der **SkiaSharp.Views.Forms** -Paket, und fügen sie der Projektmappe hinzu. Wenn Sie überprüfen die **Verweise** Abschnitt jedes Projekts nach dem Hinzufügen von SkiaSharp, Sie sehen, dass verschiedene **SkiaSharp** Bibliotheken wurden auf die einzelnen Projekte in der Projektmappe hinzugefügt.

Wenn Ihre Xamarin.Forms-Anwendung iOS ausgerichtet ist, verwenden Sie Eigenschaftenseite des Projekts, um die mindestbereitstellungsziels auf iOS 8.0 zu ändern.

In jeder C#-Seite, die SkiaSharp verwendet werden sollen eine `using` -Direktive für den [ `SkiaSharp` ](xref:SkiaSharp) -Namespace, der umfasst alle der SkiaSharp-Klassen, Strukturen und Enumerationen, die Sie in Ihren Grafiken verwenden Programmierung. Sollten Sie auch eine `using` -Direktive für den [ `SkiaSharp.Views.Forms` ](xref:SkiaSharp.Views.Forms) Namespace-URI für die Klassen, die spezifisch für Xamarin.Forms. Dies ist eine viel kleinere Namespace, wobei die wichtigste Klasse [ `SKCanvasView` ](xref:SkiaSharp.Views.Forms.SKCanvasView). Diese Klasse wird von der Xamarin.Forms `View` Klasse und Ihre SkiaSharp-Grafikausgabe hostet.

> [!IMPORTANT]
> Die `SkiaSharp.Views.Forms` Namespace enthält auch eine `SKGLView` abgeleitete Klasse `View` , aber OpenGL für Rendern von Grafiken verwendet. Zum Zwecke der Einfachheit halber wird in der vorliegenden beschränkt selbst `SKCanvasView`, während mit `SKGLView` stattdessen ist sehr ähnlich.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Grundlagen von SkiaSharp-Zeichnungen](basics/index.md)

Einige der einfachste Grafiken Abbildungen, die Sie mit SkiaSharp zeichnen können sind Kreise, Ovale und Rechtecke. Bei der Anzeige dieser Zahlen, erfahren Sie mehr über SkiaSharp-Koordinaten, Größen und Farben. Die Anzeige von Text und Bitmaps ist komplexer, aber in diesen Artikeln werden auch diese Techniken eingeführt.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp-Linien und -Pfade](paths/index.md)

Ein Grafikpfad ist eine Reihe verbundener gerader Linien und Kurven. Pfade können gestrichelt, ausgefüllt wird, oder beides. In diesem Artikel umfasst viele Aspekte der einschließlich Linienenden und Joins sowie gestrichelte Linie zeichnen und gepunkteten Linien, aber beendet, ohne die Kurve Geometrien.

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp-Transformationen](transforms/index.md)

Transformationen können Grafikobjekte, einheitlich übersetzt, skaliert, gedreht oder verzerrt werden. In diesem Artikel wird auch gezeigt, wie Sie eine standardmäßige 3 x 3-Transformationsmatrix für nicht affine Transformationen zu erstellen und Anwenden von Transformationen auf Pfade verwenden können.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp-Kurven und -Pfade](curves/index.md)

Die Auswertung der Pfade, die mit dem Hinzufügen der Kurven in einer Pfadobjekte und Ausnutzen von anderen leistungsstarken Pfad-Funktionen wird fortgesetzt. Sie sehen, wie Sie einen vollständigen Pfad in einer präzisen Textzeichenfolge angeben können, wie pfadeffekte verwendet und wie Sie die Pfad-Interna sprengen.

## <a name="skiasharp-bitmapsbitmapsindexmd"></a>[SkiaSharp-Bitmaps](bitmaps/index.md)

Bitmaps sind rechteckige Arrays von Bits, die für die Pixel der ein Anzeigegerät. Diese Artikelreihe veranschaulicht laden, speichern, anzeigen, erstellen, gezeichnet werden soll, animieren und Zugriff auf die Bits von SkiaSharp-Bitmaps.

## <a name="skiasharp-effectseffectsindexmd"></a>[SkiaSharp-Auswirkungen](effects/index.md)

Effekte sind Eigenschaften, die die normale Anzeige von Grafiken, einschließlich der Gradienten für lineare und runde Ausführungen ändern, bitmap-Kacheln, blend-Modi, blur- und andere.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SkiaSharp mit Xamarin.Forms Webinar (Video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
