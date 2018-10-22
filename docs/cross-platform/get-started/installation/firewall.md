---
title: Xamarin-Firewall-Konfigurationsanleitung
description: In diesem Dokument wird eine Liste der Hosts bereitgestellt, die in der Whitelist Ihrer Firewall enthalten sein müssen, damit Xamarin in einer Unternehmensumgebung funktioniert.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: dab4aeccd58da22736aea3139f11cbdff259ef7b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781090"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin-Firewall-Konfigurationsanleitung

_Eine Liste der Hosts, die von Ihrer Firewall zugelassen werden müssen, damit die Xamarin-Plattform in Ihrem Unternehmen funktioniert._

Für die korrekte Installation und Funktionsweise von Xamarin-Produkten müssen die erforderlichen Tools und Updates für Ihre Software über bestimmte Endpunkte heruntergeladen werden können. Wenn Sie oder Ihr Unternehmen über strenge Firewalleinstellungen verfügen, könnten u.a. Probleme mit der Installation, der Lizenzierung und den Komponenten auftreten. Dieses Dokument erläutert einige der bekannten Endpunkte, die von Ihrer Firewall zugelassen werden müssen, damit Xamarin funktioniert. Die Liste enthält nicht die Endpunkte, die für im Download enthaltene Drittanbietertools erforderlich sind. Wenn Sie immer noch Probleme haben, nachdem Sie diese Liste durchgegangen sind, lesen Sie sich die Apple- oder Android-Handbücher zur Behebung von Installationsproblemen durch.

## <a name="endpoints-to-whitelist"></a>Endpunkte für die Whitelist

### <a name="xamarin-installer"></a>Xamarin-Installer

Bei Verwendung des neuesten Releases des Xamarin-Installers müssen für die ordnungsgemäße Installation der Software folgende bekannte Adressen hinzugefügt werden:

-  xamarin.com (Installermanifeste)
-  dl.xamarin.com (Speicherort des Paketdownload)
-  dl.google.com (um das Android SDK herunterzuladen)
-  download.oracle.com (JDK)
-  visualstudio.com (Einrichten des Speicherorts des Paketdownloads)
-  go.microsoft.com (Einrichten der URL-Auflösung)
-  aka.ms (Einrichten der URL-Auflösung)

Wenn Sie einen Mac verwenden und bei der Installation von Xamarin.Android Probleme auftreten, vergewissern Sie sich, dass macOS Java herunterladen kann.


### <a name="components-store-and-nuget-including-xamarinforms"></a>Komponentenstore und NuGet (einschließlich Xamarin.Forms)

Die folgenden Adressen müssen hinzugefügt werden, um auf den Xamarin-Komponentenstore oder NuGet (Xamarin.Forms ist als NuGet-Paket verpackt) zuzugreifen:

-  components.xamarin.com (zur Verwendung des Xamarin Komponentenstore)
-  xampubdl.blob.core.windows.net (hostet Downloads aus dem Components Store)
-  www\.nuget.org (für den Zugriff auf NuGet)
-  az320820.vo.msecnd.net (NuGet-Downloads)
-  dl-ssl.google.com (Google-Komponenten)


### <a name="software-updates"></a>Softwareaktualisierungen

Die folgenden Adressen müssen hinzugefügt werden, um sicherzustellen, dass Softwareupdates ordnungsgemäß heruntergeladen werden können:

-  software.xamarin.com (Updater-Dienst)
-  download.visualstudio.microsoft.com
-  dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac-Agent

Der SSH-Port muss offen sein, damit Visual Studio mithilfe des Xamarin Mac-Agents mit einem Mac-Buildhost verbunden werden kann. Standardmäßig ist es **Port 22**.

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch wurden die Endpunkte behandelt, die zugänglich sein müssen, damit Xamarin-Produkte auf Ihrem Computer ordnungsgemäß installiert und aktualisiert werden.
