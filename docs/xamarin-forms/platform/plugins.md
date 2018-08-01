---
title: Nutzung und das Erstellen von Xamarin.Forms-Plug-Ins
description: In diesem Artikel wird erläutert, wie zu nutzen, und Erstellen von Xamarin.Forms-Plug-Ins wird. -Plug-Ins werden normalerweise verwendet, um native Plattformfunktionen leicht verfügbar zu machen.
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: 4d121c2dfcca380e1735da1a4ca47c42d1957b8a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854739"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>Nutzung und das Erstellen von Xamarin.Forms-Plug-Ins

Es gibt viele systemeigene Plattformfunktionen, die auf allen Plattformen vorhanden sind, aber leicht unterschiedliche APIs. Eine Möglichkeit für Entwickler, die diese Funktionen zu verwenden ist, erstellen eine abstrakte Schnittstelle für die plattformübergreifende, und klicken Sie dann diese Schnittstelle implementieren, in die verschiedenen Plattformen. Klicken Sie dann die Xamarin.Forms-Anwendung greift auf dieser plattformimplementierungen, die mit [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Entwickler können diese Arbeit freigeben, indem Sie das Schreiben einer _-Plug-Ins_ und auf NuGet veröffentlichen.

> [!NOTE]
> Viele plattformübergreifende Funktionen, die bisher nur in-Plug-Ins verfügbar sind jetzt Teil des Open-Source **[Xamarin.Essentials](~/essentials/index.md)** Bibliothek. Zu diesen Funktionen gehören: Akkustatus, Kompass, Bewegungssensoren, Geolocation, Sprachsynthese und viel mehr. In Zukunft **Xamarin.Essentials** werden die Hauptquelle von plattformübergreifenden Features für Xamarin.Forms-Anwendungen. Obwohl Entwickler weiterhin erstellen und veröffentlichen Plug-Ins können, sollten Sie in der Mitwirkung an **Xamarin.Essentials**.

## <a name="finding-and-adding-plugins"></a>Suchen und Hinzufügen von Plug-Ins

Die Xamarin-Community hat viele plattformübergreifende-Plug-Ins erstellt werden, die mit Xamarin.Forms kompatibel sind. Eine umfangreiche Sammlung finden Sie unter:

[**Xamarin-Plug-Ins**](https://github.com/xamarin/XamarinComponents)

Eine Anleitung zum Hinzufügen von NuGet-Pakete zu Ihrem Projekt werden soll, finden Sie in dieser exemplarischen Vorgehensweise auf [einschließen eines NuGet-Pakets in Ihr Projekt](/visualstudio/mac/nuget-walkthrough/).

## <a name="creating-plugins"></a>Erstellen von-Plug-Ins

Es ist auch möglich, erstellen und veröffentlichen Ihre eigenen Plug-Ins als Nuget-Pakete (und Xamarin-Komponenten). Viele vorhandene-Plug-Ins sind Open-Source, damit Sie ihren Code, um zu verstehen, wie sie Writtern wurden überprüfen können.

Z. B. die Liste der Plug-Ins unten sind alle open Source, und sie entsprechen einige Beispiele, in der [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Abschnitt:

- **Sprachsynthese** von James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin) und [NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **Akkustatus** von James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin) und [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

Die Github-Projekte können einen guten Ausgangspunkt zum Erstellen Ihrer eigenen Plug-Ins plattformübergreifenden bereitstellen, wie diese Anweisungen für [Erstellen eines Plug-Ins für Xamarin](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Strukturieren von Cross-Platform-Plug-in-Projekte

Es gibt, zwar keine speziellen Anforderungen für das Entwerfen eines NuGet-Pakets gibt es einige Richtlinien zum Erstellen eines Pakets für plattformübergreifende apps.

In der Vergangenheit bestand aus einer plattformübergreifenden-Plug-in in der Regel die folgenden Komponenten:

- PCL mit einer Schnittstelle, die die API für das Plug-in darstellt.
- iOS, Android und universelle Windows-Plattform (UWP)-Klassenbibliotheken mit einer Implementierung der Schnittstelle.

Lesen James Montemagno [Blogbeitrag](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) beschreiben den Prozess der Erstellung von Plug-Ins für Xamarin.

Jüngerer können-Plug-Ins werden erstellt werden mit einer einzigen Plattform mit festgelegten Zielversionen ausreicht. Dieser Ansatz wird erläutert, in der montemagnos [Blogbeitrag](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/). Dieser Ansatz wird im oben verknüpften montemagnoss-Plug-Ins verwendet und auch das Format dient **Xamarin.Essentials**.

Es wird empfohlen, vermeiden Sie Verweise auf Xamarin.Forms direkt über ein plug-in.
Dadurch kann Versionskonflikt Probleme erstellt, wenn andere Entwickler versuchen, das plug-in verwendet. Versuchen Sie stattdessen die API so entwerfen, dass sie von jeder Anwendung Xamarin oder .NET verwendet werden kann.

### <a name="publishing-nuget-packages"></a>Veröffentlichen von NuGet-Paketen

NuGet-Pakete ein **Nuspec** Datei, d.h. eine XML-Datei, die definiert, welche Teile des Projekts in das Paket veröffentlicht werden. Die **Nuspec** -Datei enthält auch Informationen zu dem Paket, z. B. Id, Titel und Autoren.

Finden Sie unter [NuGets-Dokumentation](/nuget/create-packages/creating-a-package.md) für Weitere Informationen zum Erstellen und Veröffentlichen von NuGet-Pakete.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Wiederverwendbaren-Plug-Ins für Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Mit & Entwickeln von Plug-Ins für Xamarin (Video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
