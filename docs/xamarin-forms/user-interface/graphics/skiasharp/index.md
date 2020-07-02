---
title: Skiasharp-Grafiken inXamarin.Forms
description: Skiasharp ist ein 2D-Grafiksystem für .net und c#, das von der Open Source-Skia-Grafik-Engine genutzt wird, die in Google-Produkten häufig verwendet wird. In diesem Handbuch wird erläutert, wie Sie skiasharp für 2D-Grafiken in Ihren Xamarin.Forms Anwendungen verwenden.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da00aafb9f659d8af119e00476a9a243a2f91023
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85795074"
---
# <a name="skiasharp-graphics-in-xamarinforms"></a>Skiasharp-Grafiken inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Verwenden von skiasharp für 2D-Grafiken in Ihren Xamarin.Forms Anwendungen_

Skiasharp ist ein 2D-Grafiksystem für .net und c#, das von der Open Source-Skia-Grafik-Engine genutzt wird, die in Google-Produkten häufig verwendet wird. Sie können skiasharp in Ihren Xamarin.Forms Anwendungen verwenden, um 2D-Vektorgrafiken, Bitmaps und Text zu zeichnen. Weitere allgemeine Informationen zur skiasharp-Bibliothek und anderen Tutorials finden Sie im [2D-Zeichnungs](~/graphics-games/skiasharp/index.md) Handbuch.

In dieser Anleitung wird davon ausgegangen, dass Sie mit der Programmierung vertraut sind Xamarin.Forms .

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinar: skiasharp fürXamarin.Forms**

## <a name="skiasharp-preliminaries"></a>Skiasharp-Vorbereitungen

Skiasharp für Xamarin.Forms ist als nuget-Paket verpackt. Nachdem Sie eine Projekt Mappe Xamarin.Forms in Visual Studio oder Visual Studio für Mac erstellt haben, können Sie den nuget-Paket-Manager verwenden, um nach dem **skiasharp. views. Forms** -Paket zu suchen und es der Projekt Mappe hinzuzufügen. Wenn Sie nach dem Hinzufügen von skiasharp den Abschnitt **Verweise** der einzelnen Projekte überprüfen, können Sie sehen, dass jedem Projekt in der Projekt Mappe verschiedene **skiasharp** -Bibliotheken hinzugefügt wurden.

Wenn Ihre Xamarin.Forms Anwendung auf IOS ausgerichtet ist, bearbeiten Sie die Datei " **Info. plist** ", um das minimale Bereitstellungs Ziel in ios 8,0 zu ändern.

Auf jeder c#-Seite, die skiasharp verwendet, sollten Sie eine- `using` Direktive für den- [`SkiaSharp`](xref:SkiaSharp) Namespace einschließen, die alle skiasharp-Klassen,-Strukturen und-Enumerationen umfasst, die Sie in der Grafik Programmierung verwenden. Außerdem benötigen Sie eine- `using` Direktive für den- [`SkiaSharp.Views.Forms`](xref:SkiaSharp.Views.Forms) Namespace für die Klassen, die für spezifisch sind Xamarin.Forms . Dabei handelt es sich um einen viel kleineren Namespace, bei dem es sich um die wichtigste Klasse handelt [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) . Diese Klasse wird von der Xamarin.Forms `View` -Klasse abgeleitet und hostet ihre skiasharp-Grafikausgabe.

> [!IMPORTANT]
> Der- `SkiaSharp.Views.Forms` Namespace enthält auch eine- `SKGLView` Klasse, die von abgeleitet ist, `View` aber zum Rendern von Grafiken OpenGL verwendet. Aus Gründen der Einfachheit beschränkt sich dieses Handbuch auf `SKCanvasView` , aber die Verwendung von `SKGLView` stattdessen ist ziemlich ähnlich.

## <a name="skiasharp-drawing-basics"></a>[Grundlagen von SkiaSharp-Zeichnungen](basics/index.md)

Einige der einfachsten Grafik Abbildungen, die Sie mit skiasharp zeichnen können, sind Kreise, ovale und Rechtecke. Wenn Sie diese Abbildungen anzeigen, erfahren Sie mehr über skiasharp-Koordinaten,-Größen und-Farben. Die Anzeige von Text und Bitmaps ist komplexer. in diesen Artikeln werden diese Verfahren jedoch ebenfalls vorgestellt.

## <a name="skiasharp-lines-and-paths"></a>[SkiaSharp-Linien und -Pfade](paths/index.md)

Bei einem Grafik Pfad handelt es sich um eine Reihe verbundener gerader Linien und Kurven. Pfade können mit Strichen, gefüllt oder beides gezeichnet werden. Dieser Artikel umfasst viele Aspekte der Zeilen Zeichnung, einschließlich Strich enden und Joins sowie gestrichelte und gepunktete Linien, hält aber nur wenige Kurven Geometrien an.

## <a name="skiasharp-transforms"></a>[SkiaSharp-Transformationen](transforms/index.md)

Mit Transformationen können Grafik Objekte einheitlich übersetzt, skaliert, gedreht oder verzerrt werden. In diesem Artikel wird auch erläutert, wie Sie eine standardmäßige 3-by-3-Transformationsmatrix zum Erstellen von nicht affinen Transformationen und zum Anwenden von Transformationen auf Pfade verwenden können.

## <a name="skiasharp-curves-and-paths"></a>[SkiaSharp-Kurven und -Pfade](curves/index.md)

Durch das Durchsuchen von Pfaden wird das Hinzufügen von Kurven zu Pfad Objekten und das ausnutzen anderer leistungsfähiger Pfad Funktionen fortgesetzt. Sie werden erfahren, wie Sie einen vollständigen Pfad in einer präzisen Text Zeichenfolge angeben, wie Sie Pfad Effekte verwenden können und wie Sie in Pfad internale untersuchen können.

## <a name="skiasharp-bitmaps"></a>[SkiaSharp-Bitmaps](bitmaps/index.md)

Bitmaps sind rechteckige Arrays von Bits, die den Pixeln eines Anzeige Geräts entsprechen. Diese Artikel Reihe zeigt, wie die Bits von skiasharp-Bitmaps geladen, gespeichert, angezeigt, erstellt, gezeichnet, animiert und darauf zugegriffen wird.

## <a name="skiasharp-effects"></a>[Skiasharp-Effekte](effects/index.md)

Effekte sind Eigenschaften, die die normale Darstellung von Grafiken ändern, einschließlich linearer und Zirkel Gradienten, Bitmap-tiult, Blend-Modi, weich Zeichen und anderen.

## <a name="related-links"></a>Ähnliche Themen

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [Skiasharp mit Xamarin.Forms Webinar (Video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
