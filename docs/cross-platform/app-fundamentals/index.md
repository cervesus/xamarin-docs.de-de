---
title: Application Fundamentals (Anwendungsgrundlagen)
description: Kernkonzepte für die Anwendung
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: f5bd66cfcfb6ee06abac7bec9151e7325ebb32a2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>Application Fundamentals (Anwendungsgrundlagen)

Dieser Abschnitt enthält eine Anleitung für einige der häufiger Aufgaben Dinge oder Konzepte, denen Entwickler beim Entwickeln von mobilen Anwendungen berücksichtigen müssen.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Durch Auswählen von Xamarin, und halten einige Dinge beachten Sie beim Entwerfen und Entwickeln von mobilen Anwendungen, können Sie enormen Code alle mobilen Plattformen gemeinsam nutzen, verkürzen Sie die Zeit auf dem Markt, nutzen vorhandene Talent, für den mobilen Zugriff Nachfrage erfüllen, und plattformübergreifende Komplexität zu reduzieren. &nbsp;In diesem Dokument werden wichtige Richtlinien bietet folgende Vorteile für Hilfsprogramm und Produktivität bemerken.

## <a name="code-sharing-optionscode-sharingmd"></a>[Optionen für die Codefreigabe](code-sharing.md)

Informationen Sie zu den verschiedenen Code Freigabeoptionen für Xamarin-Projekte, einschließlich portablen Klassenbibliotheken (PCLs), freigegebene Projekte und Standardbibliotheken .NET verfügbar.


## <a name="accessibilityaccessibilitymd"></a>[Barrierefreiheit](accessibility.md)

Tipps zum Erstellen von Anwendungen zugegriffen werden kann.


## <a name="localizationlocalizationmd"></a>[Lokalisierung](localization.md)

Richtlinien für die gebietsschemabasierte-apps vorgenommen werden, können in mehrere Sprachen übersetzt werden.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)

Portable Class Library-Projekte können beim Erstellen und Verteilen von Assemblys, die freigegebenen Code zur Ausführung auf mehreren Plattformen enthalten. Zum Erstellen eines Portable Class Library (oder "PCL") Wählen Sie zunächst die Plattformen abzielen, Sie schreiben Code für eine Teilmenge von .NET Framework, die im Profil definierten für diese Plattformen verfügbar ist. Dieses Dokument beschreibt das Erstellen und Verwenden von PCLs mit Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)

Gemeinsam genutzte Projekte können Sie die gemeinsamen Code schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisenden Projekts kompiliert und kann Compilerdirektiven können Sie die Übernahme von Clientplattform-spezifische Funktionen in der gemeinsamen Codebasis enthalten. In diesem Artikel wird erläutert, wie freigegebene Projekte funktionieren und wie Sie erstellen und mit Xamarin-Projekten zu verwenden.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard ist eine neue Option für die Freigabe von Code. Funktioniert in ähnlicher Weise wie portablen Klassenbibliotheken; Code wird für eine bestimmte Version (derzeit 1.0 bis 1.6) erstellt und kann von anderen Projekten, die dieser Ebene unterstützen verbrauchten oder höher sein. .NET standard Projekte werden in Xamarin Studio 6.2, Visual Studio für Windows und Visual Studio für Mac unterstützt.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet-Projekte: Multiplattform-Bibliotheken für die Freigabe von Code](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet-Pakete können von PCL oder .NET Standardprojekten automatisch generiert werden; und freigegebene Projekte können in "Köder And Switch" NuGet-Pakete, die mit separaten NuGet-Projekttyp verpackt werden. In diesem Abschnitt erläutert das NuGet-Pakete für jedes Szenario Freigeben von Code zu erstellen.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Manuelles Erstellen von NuGet-Pakete für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tipps zum Erstellen von NuGet-Pakete, die mit der Xamarin-Plattform arbeiten.

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[Cross-Platform-Datenzugriff](~/xamarin-forms/data-cloud/index.md)

Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS und Android haben die SQLite-Datenbank-Engine "integriert" und Zugriff zum Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Die [Android Datenzugriff](~/android/data-cloud/data-access/index.md), [iOS Datenzugriff](~/ios/data-cloud/data/index.md), und [Xamarin.Forms Datenzugriff](~/xamarin-forms/data-cloud/index.md) Anleitungen bieten Beispiele zum SQLite auf jeder Plattform zugreifen.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[Transport Layer Security](transport-layer-security.md)

Informationen zur Selectingthe korrekten SSL/TLS-Implementierung zum Sichern Ihrer app über eine Netzwerkverbindung.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[Benachrichtigungen](~/xamarin-forms/data-cloud/push-notifications/index.md)

Mobile Anwendungen mit Benachrichtigungen unaufdringlichen ganz informieren des Benutzers, den eine bestimmte Anwendung-Ereignis aufgetreten ist, hat. Benachrichtigungen werden in der Regel verwendet, um Benutzer über den Status des Prozesses zu benachrichtigen, die im Hintergrund ausgeführt wird. Ein Beispiel hierfür möglicherweise eine große Datei heruntergeladen werden. Diese Datei kann viel Zeit zum Herunterladen, dauern, damit diese Aktivität im Hintergrund ausgeführt werden soll. Wenn der Download abgeschlossen ist, wird der Benutzer durch eine Benachrichtigung der Tatsache informiert.
Darüber hinaus Benachrichtigung ar nicht nur auf lokale Anwendungen beschränkt. Es ist auch möglich, für serveranwendungen Benachrichtigungen für mobile Anwendungen zu veröffentlichen. In diesem Artikel besprechen wie Benachrichtigungen in Android und iOS verwendet.
