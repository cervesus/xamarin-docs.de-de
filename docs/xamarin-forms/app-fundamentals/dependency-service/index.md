---
title: Xamarin.Forms-DependencyService
description: Die „DependencyService“-Klasse von Xamarin.Forms ist ein Dienstlocator, der es Xamarin.Forms-Anwendungen ermöglicht, native Plattformfunktionen aus freigegebenem Code aufzurufen.
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 126e2d2373bad923fe1d66fe355ad811c15fbe4f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138371"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms-DependencyService

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

Die [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse ist ein Dienstlocator, der es Xamarin.Forms-Anwendungen ermöglicht, native Plattformfunktionen aus freigegebenem Code aufzurufen.

## <a name="registration-and-resolution"></a>[Registrierung und Lösung](registration-and-resolution.md)

Plattformimplementierungen müssen beim [`DependencyService`](xref:Xamarin.Forms.DependencyService) registriert und dann aus freigegebenem Code aufgelöst werden, um diese aufzurufen.

## <a name="picking-a-photo-from-the-library"></a>[Auswählen eines Fotos aus der Bibliothek](photo-picker.md)

In diesem Artikel wird beschrieben, wie die [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse von Xamarin.Forms verwendet wird, um ein Foto aus der Bildbibliothek des Smartphones auszuwählen.
