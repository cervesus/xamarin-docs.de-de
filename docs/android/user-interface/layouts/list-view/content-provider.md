---
title: Verwenden von ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d1ec628de3481820f320a5a8e6ef88fcbaab75a6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122113"
---
# <a name="using-a-contentprovider"></a>Verwenden von ContentProvider

Von CursorAdapters können auch verwendet werden, um Daten aus ContentProvider anzuzeigen.
ContentProviders ermöglichen Ihnen den Zugriff auf Daten, die von anderen Anwendungen verfügbar gemacht werden (einschließlich Android Systemdaten z. B. Kontakte, Medien und Kalenderinformationen).

ContentProvider den Zugriff auf die bevorzugte Methode ist mit einem mithilfe der LoaderManager CursorLoader. LoaderManager in Android 3.0 (API-Ebene 11, Honeycomb) zum Verschieben des blockierenden Tasks aus der Haupt-Thread eingeführt wurde und mit einem CursorLoader kann die Daten in einem Thread geladen werden, bevor Sie an einer ListView für die Anzeige gebunden wird.

Finden Sie unter [Einführung in die ContentProviders](~/android/platform/content-providers/index.md) für Weitere Informationen.

