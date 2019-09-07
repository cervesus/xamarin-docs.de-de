---
title: Verwenden von ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 5948d4b5db53db97c4e76cb7568c109b5faf90fe
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762200"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>Verwenden eines Contentprovider mit xamarin. Android

CursorAdapter können auch verwendet werden, um Daten von einem Contentprovider anzuzeigen.
Mit contentproviders können Sie auf Daten zugreifen, die von anderen Anwendungen (einschließlich Android-Systemdaten wie Kontakten, Medien und Kalenderinformationen) verfügbar gemacht werden.

Die bevorzugte Methode für den Zugriff auf einen Contentprovider besteht darin, dass ein Cursor Lader mithilfe von loadermanager verwendet wird. Loadermanager wurde in Android 3,0 (API-Ebene 11, Honeycomb) eingeführt, um blockierende Tasks aus dem Haupt Thread zu verschieben, und die Verwendung eines Cursor Loader ermöglicht, dass die Daten in einen Thread geladen werden, bevor Sie zur Anzeige an eine ListView gebunden werden.

Weitere Informationen finden Sie unter Einführung [zu contentproviders](~/android/platform/content-providers/index.md) .
