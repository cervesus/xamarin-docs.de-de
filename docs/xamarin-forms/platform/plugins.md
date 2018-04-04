---
title: Plug-Ins
description: Fügen Sie einfaches nativen Funktionen zu Xamarin.Forms-apps hinzu
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: 5770d13c46998872752820b7a0cbb222a04c3ff8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="plugins"></a>Plug-Ins

Es gibt viele native Plattformfunktionen, die auf allen Plattformen vorhanden sind, aber haben geringfügig APIs. Entwickler schreiben, um eine abstrakte plattformübergreifende-Schnittstelle für diese Funktionen zu erstellen, die sie auch für andere Benutzer freigeben können-Plug-Ins.

Zu diesen Funktionen gehören: Akkustatus, Kompass während des Verschiebens Sensoren, Geolocation, Sprachausgabe und vieles mehr. -Plug-Ins ermöglichen diese Funktionen durch Xamarin.Forms Anwendungen leicht zugegriffen werden kann.

## <a name="finding-and-adding-plugins"></a>Suchen und Hinzufügen von Plug-Ins

Die Xamarin-Community viele plattformübergreifende-Plug-Ins mit Xamarin.Forms - kompatiblen erstellt hat eine große Auflistung finden Sie unter:

[**Xamarin Plugins**](https://github.com/xamarin/plugins)

Eine Anleitung zum Hinzufügen von NuGet-Pakete zu Ihrem Projekt, finden Sie unter exemplarischen auf [einschließlich NuGet-Paket im Projekt](/visualstudio/mac/nuget-walkthrough/).


## <a name="creating-plugins"></a>Erstellen von Plug-Ins

Es ist auch möglich, zum Erstellen und Veröffentlichen Ihrer eigenen Plug-Ins als NuGet-Pakete (und Xamarin-Komponenten). Viele vorhandene-Plug-Ins sind Open Source-können Sie ihren Code aus, um zu verstehen, wie sie Writtern wurden überprüfen.

Z. B. die Liste der Plug-Ins unten werden alle open-Source und entsprechen der Beispiele in der [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) Abschnitt:

- **Sprachausgabe** von James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) und [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **Akkustatus** von James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) und [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Diese Projekte Github können einen guten Ausgangspunkt zum Erstellen Ihrer eigenen Plug-Ins über Plattformen hinweg bereitstellen, wie diese Anweisungen für [erstellen ein Plug-In für Xamarin](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Strukturieren plattformübergreifende-Plug-in-Projekte

Es gibt, zwar keine speziellen Anforderungen für das Entwerfen von NuGet-Paket gibt es einige Richtlinien zum Erstellen eines Pakets für die plattformübergreifende apps.

Ein Plug-In für die plattformübergreifende sollte in der Regel die folgenden Komponenten umfassen:

- PCL mit einer Schnittstelle, die die API für das Plug-in darstellt,
- iOS, Android und Windows-Klassenbibliotheken mit einer Implementierung der Schnittstelle.

Lesen James Montemagno [Blogbeitrag](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) Beschreiben des Prozesses für Xamarin-Plug-Ins zu erstellen.

Es ist empfehlenswert, vermeiden Sie Verweise auf Xamarin.Forms direkt über ein plug-in.
Dadurch kann Versionskonflikt Probleme erstellt, wenn andere Entwickler versuchen, das plug-in verwendet. Versuchen Sie stattdessen die API so entwerfen, dass es von einer Anwendung Xamarin oder .NET verwendet werden kann.

### <a name="publishing-nuget-packages"></a>Veröffentlichen von NuGet-Pakete

NuGet-Pakete ein **Nuspec** -Datei, die eine XML-Datei ist, die definiert, welche Teile des Projekts in das Paket veröffentlicht werden. Die **Nuspec** Datei enthält auch Informationen zu dem Paket, z. B. Id, Titel und Autoren.

Finden Sie unter [NuGet Dokumentation](http://docs.nuget.org/create/creating-and-publishing-a-package) für Weitere Informationen zum Erstellen und Veröffentlichen von NuGet-Pakete.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Wiederverwendbaren-Plug-Ins für Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Mit & beim Entwickeln von Plug-Ins für Xamarin (Video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
