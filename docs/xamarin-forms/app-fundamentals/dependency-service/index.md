---
title: Xamarin.Forms-DependencyService
description: Die Xamarin.Forms DependencyService-Klasse ist ein Dienst-Locator, der es Xamarin.Forms-Anwendungen ermöglicht, native Plattformfunktionen aus freigegebenem Code aufzurufen.
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
ms.openlocfilehash: ea259d1ee9dc4a94322c38b3e96bee654197bb87
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "67650453"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms-DependencyService

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

Die [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse ist ein Dienst-Locator, der es Xamarin.Forms-Anwendungen ermöglicht, native Plattformfunktionen aus freigegebenem Code aufzurufen.

## <a name="registration-and-resolution"></a>[Registrierung und Lösung](registration-and-resolution.md)

Plattformimplementierungen müssen beim [`DependencyService`](xref:Xamarin.Forms.DependencyService) registriert und dann aus freigegebenem Code aufgelöst werden, um diese aufzurufen.

## <a name="picking-a-photo-from-the-library"></a>[Auswählen eines Fotos aus der Bibliothek](photo-picker.md)

In diesem Artikel wird beschrieben, wie die [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse von Xamarin.Forms verwendet wird, um ein Foto aus der Bildbibliothek des Smartphones auszuwählen.
