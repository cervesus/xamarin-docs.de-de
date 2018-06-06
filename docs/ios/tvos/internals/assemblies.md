---
title: Assemblys unterstützt von Xamarin für tvos. außerdem wurden
description: Um die verfügbaren Anwendungen für tvos. außerdem wurden Funktionen zu verdeutlichen, enthält dieses Dokument eine Liste der Assemblys, die von Xamarin für tvos. außerdem wurden-Entwicklung unterstützt.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 217ec5ea81b304555bcaf19e53c8132628628627
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788822"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Assemblys unterstützt von Xamarin für tvos. außerdem wurden

## <a name="supported-assemblies"></a>Unterstützten Assemblys

Dies ist eine Liste der Assemblys, die für Ihre apps Xamarin.tvOS von Xamarin unterstützt. Eine ausführliche Liste der diese aufgeführt ist.  Schließen Sie einige wichtigen unterlassungen `System.EnterpriseServices`, den ASP.NET-Stapel und Windows.Forms.

|Assembly|Hinzugefügt|API-Kompatibilität|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1,0|Für den Compiler vorgesehen.|
|Mono.Data.Sqlite.dll|1.2|ADO.NET-Anbieter für SQLite; finden Sie unter [Einschränkungen](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Unterstützung des TDS-Protokolls; verwendet für [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) Unterstützung innerhalb ["System.Data"](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1,0|Kryptografie-APIs.|
|monotouch.dll|1,0|Diese Assembly enthält die [C#-Bindung an die API CocoaTouch](https://developer.xamarin.com/api/root/ios-unified/).|
|mscorlib.dll|1,0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1,0|APIs, die OpenGL/OpenAL objektorientierten [erweiterte Unterstützung für iPhone-Geräte bereitstellen](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1,0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1,0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [mit einige Funktionen entfernt](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Vollständige OData-Client.|
|System.Drawing|1,0|"System.Drawing" API - Classic-API.<br />_"System.Drawing" wird in die einheitliche API für die Xamarin.Mac .NET 4.5 oder Mobile-Frameworks nicht unterstützt._|
|System.Json.dll|1,1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1,1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) als im Stapel [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), sowie Typen aus den folgenden Namespaces: <ul><li>System</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); Teil ["System.Data"](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) unterstützen.|
|System.Web.Services|1,1|[Grundlegende Webdienste](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) aus dem .NET 3.5-Profil, mit dem Server-Funktionen entfernt.|
|System.Xml.dll|1,0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1,0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

Zusätzlich zu den Bindungen Mac Xamarin.tvOS nutzen kann [.NET Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md).

## <a name="related-links"></a>Verwandte Links

- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
