---
title: "Verfügbaren Assemblys"
description: "Verfügbaren Assemblys in Xamarin.Mac, Xamarin.iOS und Xamarin.Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 3a4e39582a389774e5d17327cefecb0f676adfc6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="available-assemblies"></a>Verfügbaren Assemblys

_Verfügbaren Assemblys in Xamarin.Mac, Xamarin.iOS und Xamarin.Android_

Xamarin.iOS Xamarin.Android und Xamarin.Mac alle im Lieferumfang von mehr als einem Dutzend Assemblys. Ebenso wie Silverlight eine erweiterte Teilmenge der desktop .NET-Assemblys ist, ist Xamarin-Plattformen auch eine erweiterte Teilmenge von mehreren Silverlight und desktop .NET-Assemblys.

Xamarin-Plattformen sind nicht kompatibel mit vorhandenen Assemblys, die für ein anderes Profil kompiliert ABI. Sie müssen zum Generieren von Assemblys mit den richtigen Quellcode neu kompilieren. Profil (wie müssen Sie separat Kompilieren von Quellcode und Ziel Silverlight und .NET 3.5).

Xamarin.Mac Anwendungen können in drei Modi kompiliert werden: mit Xamarin des Mobile Profile curated, dadurch können Sie Xamarin.Mac .NET 4.5 Framework als Ziel vorhandenen desktop-Assemblys und eine nicht unterstützte, die die .NET API verwendet ein System Mono gefunden die Installation. Weitere Informationen finden Sie unter unsere [Zielframeworks](~/mac/platform/target-framework.md) Dokumentation.


## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken
 
Zusätzlich zu den iOS, Android und Mac-Bindungen, Xamarin-Plattformen nutzen können [.NET Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md).

## <a name="supported-assemblies"></a>Unterstützten Assemblys

<style type="text/css"> .TG {Rahmen-reduzieren: reduzieren; Border-Abstand: 0;} .tg td {Auffüllung: 10px 5px; Border-Width: 1px; Überlauf: ausgeblendet ist, Word-Break: Normal;} .tg th {Font-Gewichtung: Normal, Auffüllung: 10px 5px; Border-Width: 1px; Überlauf: ausgeblendet ist, Word-Break: Normal;} .tg .tg-zx05 {Font-Weight: bold; Background-Color: #d1d9dd; Vertical-align: oben} .tg .tg-w6qd {Background-Color: #91e2f4; Text-align: center; Vertical-align: Top} .tg .tg-yw4l {Vertical-align: Top} .tg .tg-q3ci {Background-Color: #cfefa7; Text-align: center; Vertical-align: Top} .tg .tg-p7et {Background-Color: #cec0ec; Text-align: center; Vertical-align: Top} </style>
<table class="tg">
  <tr>
    <th class="tg-zx05">Assembly</th>
    <th class="tg-zx05">API-Kompatibilität</th>
    <th class="tg-zx05">Xamarin.iOS</th>
    <th class="tg-zx05">Xamarin.Android</th>
    <th class="tg-zx05">Xamarin.Mac</th>
  </tr>
  <tr>
    <td class="tg-yw4l">FSharp.Core.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">l18N.dll</td>
    <td class="tg-yw4l">Enthält von CJK-Schriftarten, Naher Osten, andere, nur selten auftreten, Westen</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Microsoft.CSharp.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.CSharp.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Data.Sqlite.dll</td>
    <td class="tg-yw4l">ADO.NET-Anbieter für SQLite; finden Sie unter <a href="~/ios/data-cloud/system.data.md">Einschränkungen</a>.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Data.Tds.dll</td>
    <td class="tg-yw4l">Unterstützung des TDS-Protokolls; verwendet für <a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/">System.Data.SqlClient</a> Unterstützung innerhalb <a href="https://developer.xamarin.com/api/namespace/System.Data/">"System.Data"</a>.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Dynamic.Interpreter.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Security.dll</td>
    <td class="tg-yw4l">Kryptografie-APIs.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">monotouch.dll</td>
    <td class="tg-yw4l">Diese Assembly enthält die C#-Bindung an die CocoaTouch-API. Dies ist nur innerhalb von klassischen iOS-Projekten verfügbar.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">MonoTouch.Dialog-1.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">MonoTouch.NUnitLite.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">mscorlib.dll</td>
    <td class="tg-yw4l"><a ref="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">OpenTK-1.0.dll</td>
    <td class="tg-yw4l">Die OpenGL/OpenAL objektorientierten APIs erweitert, um die Unterstützung der iPhone-Geräte bereitzustellen.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.dll</td>
    <td class="tg-yw4l"><a href="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a>, sowie Typen aus den folgenden Namespaces:<br>System.Collections.Specialized<br>System.ComponentModel<br>System.ComponentModel.Design<br>System.Diagnostics<br>System.IO<br>System.IO.Compression<br>System.IO.Compression.FileSystem<br>System.Net<br>System.Net.Cache<br>System.Net.Mail<br>System.Net.Mime<br>System.Net.NetworkInformation<br>System.Net.Security<br>System.Net.Sockets<br>System.Runtime.InteropServices<br>System.Runtime.Versioning<br>System.Security.AccessControl<br>System.Security.Authentication<br>System.Security.Cryptography<br>System.Security.Permissions<br>System.Threading<br>System.Timers</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ComponentModel.Composition.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ComponentModel.DataAnnotations.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Core.dll</td>
    <td class="tg-yw4l"><a ref="https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Data.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a> , <a href="~/ios/data-cloud/system.data.md">mit einige Funktionen entfernt</a>.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Data.Services.Client.dll</td>
    <td class="tg-yw4l">Vollständige OData-Client.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.IO.Compression</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.IO.Compression.FileSystem</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Json.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Net.Http.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Numerics.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Runtime.Serialization.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.dll</td>
    <td class="tg-yw4l">WCF-Stapel als im <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.Internals.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.ServiceModel.Web.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx">Silverlight</a>, sowie Typen aus den folgenden Namespaces: <br>System<br>System.ServiceModel.Channels<br>System.ServiceModel.Description<br>System.ServiceModel.Web</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Transactions.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a>; Teil <a href="~/ios/data-cloud/system.data.md">"System.Data"</a> unterstützen.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Web.Services.dll</td>
    <td class="tg-yw4l">Grundlegende Webdienste aus dem .NET 3.5-Profil, mit dem Server-Funktionen entfernt.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Windows.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.Linq.dll</td>
    <td class="tg-yw4l"><a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx">.NET 3.5</a></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Xml.Serialization.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.iOS.dll</td>
    <td class="tg-yw4l">Diese Assembly enthält die C#-Bindung an die CocoaTouch-API. Dies ist nur in Unified iOS-Projekten verwendet werden.</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Java.Interop.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Android.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Android.Export.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.Posix.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.EnterpriseServices.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.Android.NUnitLite.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-q3ci">✓</td>
    <td class="tg-yw4l"></td>
  </tr>
  <tr>
    <td class="tg-yw4l">Mono.CompilerServices.SymbolWriter.dll</td>
    <td class="tg-yw4l">Für den Compiler vorgesehen.</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">Xamarin.Mac.dll</td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
  <tr>
    <td class="tg-yw4l">System.Drawing.dll</td>
    <td class="tg-yw4l">"System.Drawing" API - Classic-API.<br>"System.Drawing" wird in die einheitliche API für die Xamarin.Mac .NET 4.5 oder Mobile-Frameworks nicht unterstützt.<br>"System.Drawing" Unterstützung kann hinzugefügt werden, um IOS- und OS X mit der <a href="https://github.com/mono/sysdrawing-coregraphics">Sysdrawing Coregraphics</a> Bibliothek</td>
    <td class="tg-w6qd">✓</td>
    <td class="tg-yw4l"></td>
    <td class="tg-p7et">✓</td>
  </tr>
</table>