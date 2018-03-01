---
title: Verwenden eine ContentProvider
ms.topic: article
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5c90b140719e0dafb36dad1da75f52ba27e75e25
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="using-a-contentprovider"></a>Verwenden eine ContentProvider

CursorAdapters kann auch verwendet werden, um Daten aus einer ContentProvider anzuzeigen.
ContentProviders ermöglichen es Ihnen, Daten von anderen Anwendungen verfügbar gemachten zugreifen (einschließlich Android Systemdaten wie Kontakte, Medien und Kalenderinformationen).

Die bevorzugte Methode für eine ContentProvider Zugriff ist mit einem CursorLoader mithilfe der LoaderManager. LoaderManager seit Android 3.0 (API-Ebene 11, Wabe) zu blockierenden Tasks deaktiviert den Hauptthread zu verschieben, und mit einem CursorLoader kann die Daten in einem Thread geladen werden, bevor Sie die Bindung an eine Listenansicht zum Anzeigen.

Verweisen auf [Einführung in ContentProviders](~/android/platform/content-providers/index.md) für Weitere Informationen.

