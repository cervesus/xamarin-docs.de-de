---
title: Verfügbare Assemblys
description: In diesem Dokument sind die für die Verwendung in xamarin. IOS, xamarin. Android und xamarin. Mac verfügbaren Assemblys aufgelistet. Außerdem ist es mit der Dokumentation zu .NET Standard-Bibliotheken und portablen Klassenbibliotheken verknüpft.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 238011b4762f2d394629e75fbde476e618219df2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016354"
---
# <a name="available-assemblies"></a>Verfügbare Assemblys

Xamarin. IOS, xamarin. Android und xamarin. Mac werden mit mehr als einem Dutzend Assemblys ausgeliefert. Ebenso wie Silverlight eine erweiterte Teilmenge der Desktop-.NET-Assemblys ist, ist xamarin Platforms auch eine erweiterte Teilmenge von mehreren Silverlight-und Desktop-.NET-Assemblys.

Xamarin Platforms sind nicht ABI-kompatibel mit vorhandenen Assemblys, die für ein anderes Profil kompiliert wurden. Sie müssen den Quellcode neu kompilieren, um Assemblys zu generieren, die auf das korrekte Profil abzielen (genauso wie Sie den Quellcode für Silverlight und .NET 3,5 separat neu kompilieren müssen).

Xamarin. Mac-Anwendungen können in drei Modi kompiliert werden: eine, die das mit xamarin zusammengestellte Mobile Profil verwendet, das xamarin. Mac .NET 4,5-Framework, mit dem Sie auf vorhandene vollständige Desktopassemblys abzielen können, und ein nicht unterstütztes, das die in einem System Mono gefundene .NET-API verwendet. install. Weitere Informationen finden Sie in der [zielframeworkdokumentation](~/mac/platform/target-framework.md) .

## <a name="net-standard-libraries"></a>.NET Standard-Bibliotheken

Zusätzlich zu den IOS-, Android-und Mac-Bindungen können xamarin-Projekte [.NET Standard-Bibliotheken](~/cross-platform/app-fundamentals/net-standard.md)nutzen.

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

Xamarin-Projekte können auch [Portable .NET-Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)nutzen, obwohl diese Technologie als .NET Standard veraltet eingestuft wird.

## <a name="supported-assemblies"></a>Unterstützte Assembly

Dabei handelt es sich um die Assemblys, die im **Verweis-Manager >** Assemblys > Framework (Visual Studio 2017) verfügbar sind, und um **Verweise > Pakete** (Visual Studio für Mac) und ihre Kompatibilität mit xamarin-Plattformen

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API-Kompatibilität|Xamarin IOS|Xamarin Android|Xamarin-Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |.|.|.|
> |l18N. dll|Umfasst cjk, mideast, Other, Rare, West|.|.|.|
> |Microsoft.CSharp.dll| |.|.|.|
> |Mono.CSharp.dll| |.|.|.|
> |Mono.Data.Sqlite.dll|ADO.NET-Anbieter für SQLite; siehe Einschränkungen.|.|.|.|
> |Mono.Data.Tds.dll|Unterstützung des TDS-Protokolls; wird für die [System. Data. SqlClient](xref:System.Data.SqlClient) -Unterstützung in [System. Data](xref:System.Data)verwendet.|.|.|.|
> |Mono. Dynamic. &#8203;Interpreter. dll| |.| | |
> |Mono.Security.dll|Kryptografische APIs.|.|.|.|
> |monotouch.dll|Diese Assembly enthält die C# Bindung an die cocoatouch-API. Diese ist nur in klassischen IOS-Projekten verfügbar.|.| | |
> |MonoTouch. &#8203;Dialog-1. dll| |.| | |
> |MonoTouch. &#8203;Nunitlite. dll| |.| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|.|.|.|
> |OpenTK-1.0.dll|Die objektorientierten OpenGL/OpenAL-APIs, die zur Bereitstellung der iPhone-Geräte Unterstützung erweitert werden.|.|.|.|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus Typen aus den folgenden Namespaces:<br />System.Collections.Specialized<br />Anlage. &#8203;ComponentModel<br />System. ComponentModel. Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System .net. Cache<br />System.Net.Mail<br />System .net. MIME<br />System.net. &#8203;Network Information<br />System.Net.Security<br />System.Net.Sockets<br />System. Runtime. &#8203;InteropServices<br />System.Runtime.Versioning<br />System. Security. &#8203;AccessControl<br />System.Security.Authentication<br />System. Security. &#8203;Kryptografie<br />System.Security.Permissions<br />System.Threading<br />System. Timers|.|.|.|
> |Anlage. &#8203;ComponentModel. &#8203;Komposition. dll| |.|.|.|
> |Anlage. &#8203;ComponentModel. &#8203;DataAnnotations. dll| |.|.|.|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|.|.|.|
> |System.Data.dll|[.NET 3,5](https://msdn.microsoft.com/library/ms229335.aspx) , wobei [einige Funktionen entfernt](~/ios/data-cloud/system.data.md)wurden.|.|.|.|
> |System. Data. &#8203;Dienste: &#8203;Client. dll|Vollständiger odata-Client.|.|.|.|
> |System.IO. &#8203;Komprimierung| |.|.|.|
> |System.IO. &#8203;Komprimierung. &#8203;Dateisystem| |.|.|.|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|.|.|.|
> |System.net. &#8203;Http. dll| |.|.|.|
> |Anlage. &#8203;Numerics. dll| |.|.|.|
> |System. Runtime. &#8203;Serialization. dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|.|.|.|
> |Anlage. &#8203;Service Model. dll|WCF-Stapel, wie er in [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx) vorhanden ist|.|.|.|
> |Anlage. &#8203;Service Model. &#8203;Internals. dll| |.|.|.|
> |Anlage. &#8203;Service Model. &#8203;Web. dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus Typen aus den folgenden Namespaces: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|.|.|.|
> |Anlage. &#8203;Transactions. dll|[.NET 3,5](https://msdn.microsoft.com/library/ms229335.aspx); Teil der [System. Data](~/ios/data-cloud/system.data.md) -Unterstützung.|.|.|.|
> |System. Web. &#8203;Services. dll|Grundlegende Webdienste aus dem .NET 3,5-Profil, wobei die Serverfunktionen entfernt wurden.|.|.|.|
> |Anlage. &#8203;Windows. dll| |.|.|.|
> |Anlage. &#8203;XML. dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|.|.|.|
> |System. Xml. &#8203;LINQ. dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|.|.|.|
> |System.Xml.Serialization.dll| |.|.|.|
> |Xamarin.iOS.dll|Diese Assembly enthält die C# Bindung an die cocoatouch-API. Dies wird nur in vereinheitlichten IOS-Projekten verwendet.|.| | |
> |Java.Interop.dll| | |.| |
> |Mono.Android.dll| | |.| |
> |Mono. Android. &#8203;Export. dll| | |.| |
> |Mono.Posix.dll| | |.| |
> |Anlage. &#8203;EnterpriseServices. dll| | |.| |
> |Xamarin. Android. &#8203;Nunitlite. dll| | |.| |
> |Mono. CompilerServices. &#8203;Symbolwriter. dll|Für Compiler-Writer.| | |.|
> |Xamarin.Mac.dll| | | |.|
> |Anlage. &#8203;Drawing. dll|System. Drawing wird in den Unified API für die Frameworks xamarin. Mac, .NET 4,5 oder Mobile nicht unterstützt. Die Unterstützung von System. Drawing kann IOS und macOS mithilfe der [sysdrawing-CoreGraphics-](https://github.com/mono/sysdrawing-coregraphics) Bibliothek hinzugefügt werden.|.| |.|
