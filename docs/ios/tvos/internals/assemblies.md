---
title: Assemblys
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 0a435e80173ca3ccb76dba0719ec2eea6eb72611
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="assemblies"></a>Assemblys

## <a name="supported-assemblies"></a>Unterstützten Assemblys

Dies ist eine Liste der Assemblys, die für Ihre apps Xamarin.tvOS von Xamarin unterstützt. Eine ausführliche Liste der diese aufgeführt ist.  Schließen Sie einige wichtigen unterlassungen `System.EnterpriseServices`, den ASP.NET-Stapel und Windows.Forms.

<table align="center" border="1" cellpadding="1" cellspacing="1" width="90%">
      <tbody>
        <tr>
          <td>
            <strong>Assembly</strong>
          </td>
          <td>
            <strong>Hinzugefügt</strong>
          </td>
          <td>
            <strong>API-Kompatibilität</strong>
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.CompilerServices.SymbolWriter.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td valign="top">
Für den Compiler vorgesehen.
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.Data.Sqlite.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
ADO.NET-Anbieter für SQLite; finden Sie unter&nbsp;<a href="~/ios/data-cloud/system.data.md">Einschränkungen</a>.
          </td>
        </tr>
        <tr>
          <td valign="top">
Mono.Data.Tds.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
Unterstützung des TDS-Protokolls; verwendet für&nbsp;<a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/" target="_blank">System.Data.SqlClient</a>&nbsp;Unterstützung innerhalb&nbsp;<a href="~/ios/data-cloud/system.data.md">"System.Data"</a>.
          </td>
        </tr>
        <tr>
          <td>
Mono.Security.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
Kryptografie-APIs.
          </td>
        </tr>
        <tr>
          <td valign="top">
monotouch.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
Diese Assembly enthält die&nbsp;<a href="https://developer.xamarin.com/api/root/ios-unified/" target="_blank">C#-Bindung an die API CocoaTouch</a>.
          </td>
        </tr>
        <tr>
          <td valign="top">
mscorlib.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
OpenTK.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
APIs, die OpenGL/OpenAL objektorientierten&nbsp;<a href="https://developer.xamarin.com/api/namespace/OpenGLES/" target="_blank">erweiterte Unterstützung für iPhone-Geräte bereitstellen</a>.
          </td>
        </tr>
        <tr>
          <td align="left" valign="top">
System.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
            <p><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>, sowie Typen aus den folgenden Namespaces:</p>
    
            <ul>
              <li>System.Collections.Specialized</li>
              <li>System.ComponentModel</li>
              <li>System.ComponentModel.Design</li>
              <li>System.Diagnostics</li>
              <li>System.IO.Compression</li>
              <li>System.Net</li>
              <li>System.Net.Cache</li>
              <li>System.Net.Mail</li>
              <li>System.Net.Mime</li>
              <li>System.Net.NetworkInformation</li>
              <li>System.Net.Security</li>
              <li>System.Net.Sockets</li>
              <li>System.Security.Authentication</li>
              <li>System.Security.Cryptography</li>
              <li>System.Timers</li>
            </ul>
    
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Core.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Data.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>&nbsp;,&nbsp;<a href="~/ios/data-cloud/system.data.md">mit einige Funktionen entfernt</a>.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Data.Service.Client.dll </td>
          <td align="center" valign="top">
3.x </td>
          <td>
Vollständige OData-Client.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Drawing </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
            <p>"System.Drawing" API - Classic-API.</p>
            <p><i>"System.Drawing" wird in die einheitliche API für die Xamarin.Mac .NET 4.5 oder Mobile-Frameworks nicht unterstützt.</i></p>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Json.dll </td>
          <td align="center" valign="top">
1,1 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Runtime.Serialization.dll </td>
          <td align="center" valign="top">
?
          </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.ServiceModel.dll </td>
          <td align="center" valign="top">
1,1 </td>
          <td>
            <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">WCF</a>&nbsp;als im Stapel&nbsp;<a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.ServiceModel.Web.dll </td>
          <td align="center" valign="top">
?
          </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>, sowie Typen aus den folgenden Namespaces: <p>&nbsp;</p>
    
            <ul>
              <li>System</li>
              <li>System.ServiceModel.Channels</li>
              <li>System.ServiceModel.Description</li>
              <li>System.ServiceModel.Web</li>
            </ul>
    
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Transactions.dll </td>
          <td align="center" valign="top">
1.2 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>; Teil&nbsp;<a href="~/ios/data-cloud/system.data.md">"System.Data"</a>&nbsp;unterstützen.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Web.Services </td>
          <td align="center" valign="top">
1,1 </td>
          <td>
            <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">Grundlegende Webdienste</a>&nbsp;aus dem .NET 3.5-Profil, mit dem Server-Funktionen entfernt.
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Xml.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>
          </td>
        </tr>
        <tr>
          <td valign="top">
System.Xml.Linq.dll </td>
          <td align="center" valign="top">
1,0 </td>
          <td>
            <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a><br>
            <br>
&nbsp; </td>
        </tr>
      </tbody>
    </table>

<a name="Summary" />

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

Zusätzlich zu den Bindungen Mac Xamarin.tvOS nutzen kann [.NET Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>Verwandte Links

- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
