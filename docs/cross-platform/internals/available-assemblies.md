---
title: Verfügbare Assemblys
description: Dieses Dokument Listet die Assemblys, die für die Verwendung in Xamarin.iOS, Xamarin.Android und Xamarin.Mac verfügbar. Auch verknüpft in der Dokumentation zu .NET Standard-Bibliotheken und Portable Class Libraries.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: 213632ae26ae60797e39bc718a95057fb7238609
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113351"
---
# <a name="available-assemblies"></a>Verfügbare Assemblys

Xamarin.iOS, Xamarin.Android und Xamarin.Mac alle im Lieferumfang von mehr als ein Dutzend Assemblys. Genau wie Silverlight eine erweiterte Teilmenge der desktop .NET Assemblys ist, ist Xamarin-Plattformen auch eine erweiterte Teilmenge von mehreren Silverlight und desktop .NET-Assemblys.

Xamarin-Plattformen sind nicht kompatibel mit vorhandenen Assemblys, die für ein anderes Profil kompiliert ABI. Sie müssen neu kompilieren, Ihren Quellcode zum Generieren von Assemblys, die für das richtige Profil aus, (ebenso wie Sie separat Entwickeln von Silverlight und .NET 3.5 Quellcode neu kompilieren müssen).

Xamarin.Mac-Anwendungen können in drei Modi kompiliert werden: Xamarin verwendet der Mobile Profile kuratierten, dadurch können Sie Xamarin.Mac .NET 4.5 Framework als Ziel vorhandenen desktop-Assemblys und eine nicht unterstützte, die die .NET API wird verwendet, finden Sie in einem System Mono die Installation. Weitere Informationen finden Sie unserem [Zielframeworks](~/mac/platform/target-framework.md) Dokumentation.

## <a name="net-standard-libraries"></a>.NET standard-Bibliotheken

Zusätzlich zu den iOS, Android und Mac-Bindungen, Xamarin-Projekten verwendet werden können [.NET Standard-Bibliotheken](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

Xamarin-Projekte können auch nutzen [.NET Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md), obwohl diese Technologie zugunsten von .NET Standard als veraltet markiert wird.

## <a name="supported-assemblies"></a>Unterstützten Assemblys

Hierbei handelt es sich um die Assemblys zur Verfügung, in der **Verweis-Manager > Assemblys > Framework** (Visual Studio 2017) und **Verweise Bearbeiten > Pakete** (Visual Studio für Mac), und ihre Kompatibilität mit Xamarin-Plattformen.

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API-Kompatibilität|Xamarin iOS|Xamarin Android|Xamarin-Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|CJK, Naher Osten, andere, selten, enthält West|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|ADO NET-Anbieter für SQLite; finden Sie in der.|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS-Protokolls unterstützt zum ["System.Data.SqlClient"](xref:System.Data.SqlClient) -Unterstützung in ["System.Data"](xref:System.Data).|✓|✓|✓|
> |Mono.Dynamic. &#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Kryptografie-APIs.|✓|✓|✓|
> |monotouch.dll|Diese Assembly enthält, die C#-Bindung an die CocoaTouch-API. Dies ist nur in klassischen iOS-Projekte verfügbar.|✓| | |
> |MonoTouch. &#8203;1.dll-Dialogfeld| |✓| | |
> |MonoTouch. &#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|Die OpenGL/OpenAL objektorientierten APIs erweitert, um Unterstützung für iPhone-Geräte bereitzustellen.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces:<br />System.Collections.Specialized<br />System. &#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net. &#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime. &#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security-Namespace. &#8203;AccessControl<br />System.Security.Authentication<br />System.Security-Namespace. &#8203;Kryptografie<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System. &#8203;ComponentModel. &#8203;Composition.dll| |✓|✓|✓|
> |System. &#8203;ComponentModel. &#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , mit [einige Funktionen entfernt](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |"System.Data". &#8203;Dienste. &#8203;Client.dll|Vollständige oData-Client.|✓|✓|✓|
> |System.IO. &#8203;Komprimierung| |✓|✓|✓|
> |System.IO. &#8203;Komprimierung. &#8203;Dateisystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System. &#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime. &#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System. &#8203;ServiceModel.dll|WCF-Stapel als im [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System. &#8203;ServiceModel. &#8203;Internals.dll| |✓|✓|✓|
> |System. &#8203;ServiceModel. &#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System. &#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); Teil ["System.Data"](~/ios/data-cloud/system.data.md) unterstützen.|✓|✓|✓|
> |"System.Web". &#8203;Services.dll|Grundlegende Webdienste aus dem .NET 3.5-Profil, mit dem Server-Funktionen entfernt.|✓|✓|✓|
> |System. &#8203;Windows.dll| |✓|✓|✓|
> |System. &#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |"System.xml". &#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Diese Assembly enthält, die C#-Bindung an die CocoaTouch-API. Dies ist nur in Unified-iOS-Projekte verwendet.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |"Mono.Android". &#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System. &#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android. &#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices. &#8203;SymbolWriter.dll|Für Compilerwriter.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System. &#8203;Drawing.dll|"System.Drawing" wird nicht in der Unified API für die Xamarin.Mac, .NET 4.5 oder Mobile-Frameworks unterstützt. System.Drawing-Unterstützung kann hinzugefügt werden, auf IOS- und MacOS verwenden, die [Sysdrawing-Coregraphics](https://github.com/mono/sysdrawing-coregraphics) Bibliothek|✓| |✓|
