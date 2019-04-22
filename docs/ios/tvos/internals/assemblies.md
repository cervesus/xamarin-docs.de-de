---
title: Assemblys unterstützt von Xamarin für tvos verwendet.
description: Um die Features für TvOS-Anwendungen zu verdeutlichen, bietet dieses Dokument eine Liste der Assemblys, die von Xamarin für TvOS-Entwicklung unterstützt.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: df50b4280335001f2d27ff23a91e4098eed3ba99
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870208"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Assemblys unterstützt von Xamarin für tvos verwendet.

## <a name="supported-assemblies"></a>Unterstützten Assemblys

Dies ist eine Liste der Assemblys von Xamarin, die für Ihre Xamarin.tvOS-apps unterstützt werden. Eine ausführliche Liste der diese sind unten aufgeführt.  Einige wichtigen auslassungen enthalten `System.EnterpriseServices`, dem ASP.NET-Stapel und Windows.Forms.

|Assembly|Hinzugefügt|API-Kompatibilität|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Für Compilerwriter.|
|Mono.Data.Sqlite.dll|1.2|ADO NET-Anbieter für SQLite; finden Sie unter [Einschränkungen](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|TDS-Protokolls unterstützt zum ["System.Data.SqlClient"](xref:System.Data.SqlClient) -Unterstützung in ["System.Data"](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Kryptografie-APIs.|
|monotouch.dll|1.0|Diese Assembly enthält die [C#-Bindung an die API CocoaTouch](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|APIs, die OpenGL/OpenAL objektorientierten [erweitert, um die Unterstützung der iPhone-Geräte bieten](xref:OpenGLES).|
|System.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx), [mit einigen Funktionen entfernt](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Vollständige oData-Client.|
|System.Drawing|1.0|System.Drawing-API – nur klassische-API.<br />_"System.Drawing" wird nicht in der Unified API für die Xamarin.Mac .NET 4.5 oder einer mobilen Frameworks unterstützt._|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) als im Stapel [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces: <ul><li>System</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx); Teil ["System.Data"](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) unterstützen.|
|System.Web.Services|1.1|[Grundlegende Webdienste](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) aus dem .NET 3.5-Profil, mit dem Server-Funktionen entfernt.|
|System.Xml.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

Zusätzlich zu den Mac-Bindungen, Xamarin.tvOS nutzen können [.NET Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md).

## <a name="related-links"></a>Verwandte Links

- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
