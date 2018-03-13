---
title: Verwenden von SkiaSharp in Xamarin.Forms
description: "Verwenden Sie SkiaSharp für 2D Grafiken in Ihren Anwendungen Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: eaebd8cbae996e9a5792d0a4898fafb72bdded47
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>Verwenden von SkiaSharp in Xamarin.Forms

_Verwenden Sie SkiaSharp für 2D Grafiken in Ihren Anwendungen Xamarin.Forms_

SkiaSharp wird eine 2D-Drehung Grafiksystem für .NET und C#-Karten von der Open Source-Skia Graphics-Engine, die umfassend in Google-Produkten verwendet wird. Sie können SkiaSharp in Ihren Anwendungen Xamarin.Forms verwenden, 2D Vektorgrafiken, Bitmaps und Text gezeichnet werden soll. Finden Sie unter der [2D Zeichnung](~/graphics-games/skiasharp/index.md) Handbuch für weitere allgemeine Informationen zu der Bibliothek SkiaSharp und einigen anderen Lernprogrammen.

Dieses Handbuch setzt voraus, dass Sie mit Xamarin.Forms Programmierung vertraut sind.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinar: SkiaSharp für Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>SkiaSharp vorbereitende Maßnahmen

SkiaSharp für Xamarin.Forms wird als ein NuGet-Paket verpackt. Nachdem Sie eine Xamarin.Forms-Projektmappe in Visual Studio oder Visual Studio für Mac erstellt haben, können Sie den NuGet-Paket-Manager für die Suche nach der **SkiaSharp.Views.Forms** Verpacken, und fügen sie der Projektmappe. Wenn Sie eine Überprüfung auf die **Verweise** Abschnitt jedes Projekts nach dem Hinzufügen der SkiaSharp, können Sie sehen, die verschiedene **SkiaSharp** Bibliotheken wurden auf die einzelnen Projekte in der Projektmappe hinzugefügt.

Wenn Ihre Anwendung Xamarin.Forms iOS gerichtet ist, verwenden Sie die Projekteigenschaftenseite so ändern Sie das minimale Bereitstellungsziel entweder in iOS 8.0 oder höher.

Auf alle c#-Seite, SkiaSharp verwendet, werden sollen eine `using` Richtlinie für die [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) umfasst alle der SkiaSharp Klassen, Strukturen und Enumerationen, mit dem Sie in der Grafik-Namespace Programmierung. Sollten Sie auch eine `using` Richtlinie für die [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) Namespace-URI für die Klassen, die speziell für Xamarin.Forms. Dies ist ein viel kleinere Namespace, wobei die wichtigste Klasse [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Diese Klasse leitet sich von der Xamarin.Forms `View` Klasse und Ihre SkiaSharp Grafikausgabe hostet.

> [!IMPORTANT]
> Die `SkiaSharp.Views.Forms` Namespace enthält außerdem eine `SKGLView` von abgeleitete Klasse `View` aber OpenGL für Rendern von Grafiken verwendet. Dieses Handbuch aus Gründen der Einfachheit halber schränkt selbst `SKCanvasView`, während mit `SKGLView` stattdessen sind sehr ähnlich.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Grundlagen von SkiaSharp-Zeichnungen](basics/index.md)

Einige einfachste Grafiken Abbildungen mit SkiaSharp gezeichneten sind Kreise, Ellipsen und Rechtecke. Anzeigen von obenstehenden, erfahren Sie mehr zu SkiaSharp Koordinaten, Größen und Farben.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp-Linien und -Pfade](paths/index.md)

Ein Grafikpfad ist eine Reihe verbundener gerader Linien und Kurven. Pfade können gezeichnet, gefüllt ist, oder beides. Dieses Thema umfasst zahlreiche Aspekte der Linie zeichnen, einschließlich Linienenden und Joins sowie gestrichelte und gepunkteten Linien, aber nicht genügend Kurve Geometrien beendet.

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp-Transformationen](transforms/index.md)

Umwandlungen Grafikobjekten gleichmäßig übersetzt, skaliert, gedreht oder verfälscht werden, zu ermöglichen. In diesem Artikel erfahren auch, wie Sie eine standardmäßige 3 x 3-Transformationsmatrix für nicht-affine Transformationen erstellen und Anwenden von Transformationen in Pfade verwenden können.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp-Kurven und -Pfade](curves/index.md)

Die Untersuchung des Pfade weiterhin mit dem Hinzufügen von Kurven auf einem Pfadobjekte und andere Funktionen leistungsstarke Pfad ausnutzen. Sie sehen, wie Sie einen vollständigen Pfad in eine präzise Textzeichenfolge angeben können, wie Pfad Effekte verwenden und wie in den Pfad Internals vorzudringen.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [SkiaSharp mit Xamarin.Forms Webinar (Video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
