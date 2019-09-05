---
title: Von xamarin für tvos unterstützte Assemblys
description: Um die für tvos-Anwendungen verfügbaren Features zu verdeutlichen, bietet dieses Dokument eine Liste der Assemblys, die von xamarin für die tvos-Entwicklung unterstützt werden.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/07/2016
ms.openlocfilehash: 193f4a445e21416abf2fd6279cdc18228e16c985
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283615"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Von xamarin für tvos unterstützte Assemblys

## <a name="supported-assemblies"></a>Unterstützte Assembly

Dies ist eine Liste der Assemblys, die von xamarin für Ihre xamarin. tvos-Apps unterstützt werden. Unten finden Sie eine ausführliche Liste.  Einige wichtige Ausfälle sind `System.EnterpriseServices`, der ASP.net Stack und Windows. Forms.

|Assembly|Hinzugefügt|API-Kompatibilität|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Für Compiler-Writer.|
|Mono.Data.Sqlite.dll|1.2|ADO.NET-Anbieter für SQLite; siehe [Einschränkungen](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Unterstützung des TDS-Protokolls; wird für die [System. Data. SqlClient](xref:System.Data.SqlClient) -Unterstützung in [System. Data](~/ios/data-cloud/system.data.md)verwendet.|
|Mono.Security.dll|1.0|Kryptografische APIs.|
|monotouch.dll|1.0|Diese Assembly enthält die [ C# Bindung an die cocoatouch-API](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|Die objektorientierten OpenGL/OpenAL-APIs, die [zur Bereitstellung der iPhone-Geräte Unterstützung erweitert](xref:OpenGLES)werden.|
|System.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus Typen aus den folgenden Namespaces: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System. Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3,5](https://msdn.microsoft.com/library/ms229335.aspx), [wobei einige Funktionen entfernt](~/ios/data-cloud/system.data.md)wurden.|
|System.Data.Service.Client.dll|3. x|Vollständiger odata-Client.|
|System.Drawing|1.0|System. Drawing-API-Classic API.<br />_"System. Drawing" wird in den Unified API für die xamarin. Mac .NET 4,5-oder Mobile-Frameworks nicht unterstützt._|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) -Stapel, wie er in [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx) vorhanden ist|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus Typen aus den folgenden Namespaces: <ul><li>System</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3,5](https://msdn.microsoft.com/library/ms229335.aspx); Teil der [System. Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) -Unterstützung.|
|System.Web.Services|1.1|[Grundlegende Webdienste](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) aus dem .NET 3,5-Profil, wobei die Serverfunktionen entfernt wurden.|
|System.Xml.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

Zusätzlich zu den Mac-Bindungen kann xamarin. tvos [Portable .NET-Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)nutzen.

## <a name="related-links"></a>Verwandte Links

- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
