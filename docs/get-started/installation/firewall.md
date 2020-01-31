---
title: Xamarin-Firewall-Konfigurationsanleitung
description: Dieses Dokument enthält eine Liste von Hosts, die in Ihrer Firewall zugelassen werden müssen, damit Xamarin in einer Unternehmensumgebung arbeiten kann.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: conceptdev
ms.author: crdun
ms.date: 07/17/2019
ms.openlocfilehash: 2b52dfd55194ec076f28f8c33e758a39d14f5943
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "70291328"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin-Firewallkonfigurationsanleitung

_Eine Liste von Hosts, die Sie in Ihrer Firewall zulassen müssen, damit die Plattform von Xamarin in Ihrem Unternehmen funktionieren kann._

Für die korrekte Installation und Funktionsweise von Xamarin-Produkten müssen die erforderlichen Tools und Updates für Ihre Software über bestimmte Endpunkte heruntergeladen werden können. Wenn Sie oder Ihr Unternehmen über strenge Firewalleinstellungen verfügen, könnten u.a. Probleme mit der Installation, der Lizenzierung und den Komponenten auftreten. Dieses Dokument beschreibt einige der bekannten Endpunkte, die in Ihrer Firewall zugelassen werden müssen, damit Xamarin funktioniert. Die Liste enthält nicht die Endpunkte, die für im Download enthaltene Drittanbietertools erforderlich sind. Wenn Sie immer noch Probleme haben, nachdem Sie diese Liste durchgegangen sind, lesen Sie sich die Apple- oder Android-Handbücher zur Behebung von Installationsproblemen durch.

## <a name="endpoints-to-allow"></a>Endpunkte, die zugelassen werden müssen

### <a name="xamarin-installer"></a>Xamarin-Installer

Bei Verwendung des neuesten Releases des Xamarin-Installers müssen für die ordnungsgemäße Installation der Software folgende bekannte Adressen hinzugefügt werden:

- xamarin.com (Installermanifeste)
- dl.xamarin.com (Speicherort des Paketdownload)
- dl.google.com (um das Android SDK herunterzuladen)
- download.oracle.com (JDK)
- visualstudio.com (Einrichten des Speicherorts des Paketdownloads)
- go.microsoft.com (Einrichten der URL-Auflösung)
- aka.ms (Einrichten der URL-Auflösung)

Wenn Sie einen Mac verwenden und bei der Installation von Xamarin.Android Probleme auftreten, vergewissern Sie sich, dass macOS Java herunterladen kann.

### <a name="nuget-including-xamarinforms"></a>NuGet (einschließlich Xamarin.Forms)

Die folgenden Adressen müssen hinzugefügt werden, um auf NuGet (Xamarin.Forms ist als NuGet-Paket verpackt) zuzugreifen:

- www.nuget.org (für den Zugriff auf NuGet)
- globalcdn.nuget.org (NuGet-Downloads)
- dl-ssl.google.com (Google-Komponenten für Android und Xamarin.Forms)

### <a name="software-updates"></a>Softwareupdates

Die folgenden Adressen müssen hinzugefügt werden, um sicherzustellen, dass Softwareupdates ordnungsgemäß heruntergeladen werden können:

- software.xamarin.com (Updater-Dienst)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac-Agent

Der SSH-Port muss offen sein, damit Visual Studio mithilfe des Xamarin Mac-Agents mit einem Mac-Buildhost verbunden werden kann. Standardmäßig ist es **Port 22**.
