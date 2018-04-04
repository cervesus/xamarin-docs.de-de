---
title: Verwenden eine ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b9b6340d4aaf386c7b4be8ebf366589582771be2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="using-a-contentprovider"></a>Verwenden eine ContentProvider

CursorAdapters kann auch verwendet werden, um Daten aus einer ContentProvider anzuzeigen.
ContentProviders ermöglichen es Ihnen, Daten von anderen Anwendungen verfügbar gemachten zugreifen (einschließlich Android Systemdaten wie Kontakte, Medien und Kalenderinformationen).

Die bevorzugte Methode für eine ContentProvider Zugriff ist mit einem CursorLoader mithilfe der LoaderManager. LoaderManager seit Android 3.0 (API-Ebene 11, Wabe) zu blockierenden Tasks deaktiviert den Hauptthread zu verschieben, und mit einem CursorLoader kann die Daten in einem Thread geladen werden, bevor Sie die Bindung an eine Listenansicht zum Anzeigen.

Verweisen auf [Einführung in ContentProviders](~/android/platform/content-providers/index.md) für Weitere Informationen.

