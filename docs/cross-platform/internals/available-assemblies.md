---
title: Verfügbaren Assemblys
description: Verfügbaren Assemblys in Xamarin.Mac, Xamarin.iOS und Xamarin.Android
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: e27ad3469a37634f5829f9c4c903808a74e4d6ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="available-assemblies"></a>Verfügbaren Assemblys

_Verfügbaren Assemblys in Xamarin.Mac, Xamarin.iOS und Xamarin.Android_

Xamarin.iOS Xamarin.Android und Xamarin.Mac alle im Lieferumfang von mehr als einem Dutzend Assemblys. Ebenso wie Silverlight eine erweiterte Teilmenge der desktop .NET-Assemblys ist, ist Xamarin-Plattformen auch eine erweiterte Teilmenge von mehreren Silverlight und desktop .NET-Assemblys.

Xamarin-Plattformen sind nicht kompatibel mit vorhandenen Assemblys, die für ein anderes Profil kompiliert ABI. Sie müssen zum Generieren von Assemblys, die für das korrekte Profil (wie in diesem Fall müssen Sie eine Silverlight und .NET 3.5 separat als Ziel-Quellcode neu kompilieren) den Quellcode neu kompilieren.

Xamarin.Mac Anwendungen können in drei Modi kompiliert werden: mit Xamarin des Mobile Profile curated, dadurch können Sie Xamarin.Mac .NET 4.5 Framework als Ziel vorhandenen desktop-Assemblys und eine nicht unterstützte, die die .NET API verwendet ein System Mono gefunden die Installation. Weitere Informationen finden Sie unter unsere [Zielframeworks](~/mac/platform/target-framework.md) Dokumentation.


## <a name="net-standard-libraries"></a>.NET Standardbibliotheken

Zusätzlich zu den iOS, Android und Mac-Bindungen, Xamarin-Projekten genutzt werden können [.NET Standardbibliotheken](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken
 
Xamarin-Projekten können Sie auch verwenden [.NET Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md), obwohl diese Technologie zugunsten .NET Standard als veraltet eingestuft ist.

## <a name="supported-assemblies"></a>Unterstützten Assemblys

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API-Kompatibilität|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Enthält von CJK-Schriftarten, Naher Osten, andere, nur selten auftreten, Westen|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|ADO.NET-Anbieter für SQLite; finden Sie unter Einschränkungen.|✓|✓|✓|
> |Mono.Data.Tds.dll|Unterstützung des TDS-Protokolls; verwendet für [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) Unterstützung innerhalb ["System.Data"](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Kryptografie-APIs.|✓|✓|✓|
> |monotouch.dll|Diese Assembly enthält die C#-Bindung an die CocoaTouch-API. Dies ist nur innerhalb von klassischen iOS-Projekten verfügbar.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|Die OpenGL/OpenAL objektorientierten APIs erweitert, um die Unterstützung der iPhone-Geräte bereitzustellen.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System. &#8203;ComponentModel. &#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx) , mit [einige Funktionen entfernt](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Vollständige OData-Client.|✓|✓|✓|
> |System.IO. &#8203;Komprimierung| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System. &#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|WCF-Stapel als im [Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System. &#8203;ServiceModel. &#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx); Teil ["System.Data"](~/ios/data-cloud/system.data.md) unterstützen.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Grundlegende Webdienste aus dem .NET 3.5-Profil, mit dem Server-Funktionen entfernt.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Diese Assembly enthält die C#-Bindung an die CocoaTouch-API. Dies ist nur in Unified iOS-Projekten verwendet werden.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Für den Compiler vorgesehen.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|"System.Drawing" API - Classic-API. "System.Drawing" wird in die einheitliche API für die Xamarin.Mac .NET 4.5 oder Mobile-Frameworks nicht unterstützt. "System.Drawing" Unterstützung kann hinzugefügt werden, um IOS- und OS X mit der [Sysdrawing Coregraphics](https://github.com/mono/sysdrawing-coregraphics) Bibliothek|✓| |✓|
