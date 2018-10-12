---
title: Android-Plattformfeatures
description: In diesem Artikel wird erläutert, wie apps mit Xamarin.Forms Android-spezifische Funktionen hinzu, und Material Entwurf zu realisieren.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 2eada518586f222d200ec19aeddc65107d7603b3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242403"
---
# <a name="android-platform-features"></a>Android-Plattformfeatures

## <a name="platform-support"></a>Plattformunterstützung

Für das Xamarin.Forms-Standardprojekt für Android wurde ursprünglich ein älteres Format des Steuerelementrendererings verwendet, das allgemein vor Android 5.0 genutzt wurde. Anwendungen, die mithilfe der Vorlage erstellt wurden, besitzen`FormsApplicationActivity`als Basisklasse für die Hauptaktivität.

## <a name="material-design-via-appcompat"></a>Materialdesign über AppCompat

Xamarin.Forms verwendet nun auch die optionale `FormsAppCompatActivity`-Klasse als Basisklasse der Hauptaktivität. Sie verwendet **AppCompat**-Features von Android, um Materialdesigns zu implementieren.

Befolgen Sie die [Installationsanweisungen für die AppCompat-Unterstützung](appcompat.md), um Materialdesigns zu Ihrem Xamarin.Forms-Projekt für Android hinzuzufügen.

So sieht das **Todo**-Beispiel (Aufgaben) mit dem Standardwert `FormsApplicationActivity` aus:

[![](images/before-appcompat-sml.png "TODO-Beispielanwendung ohne AppCompat")](images/before-appcompat.png#lightbox "TODO-Beispielanwendung ohne AppCompat")

Nachfolgend ist der gleiche Code nach dem Upgrade des Projekts auf `FormsAppCompatActivity`dargestellt (weitere Designinformationen werden ebenso hinzugefügt):

[![](images/post-appcompat-sml.png "TODO-Beispielanwendung mit AppCompat und Designumgebung")](images/post-appcompat.png#lightbox "TODO-Beispielanwendung mit AppCompat und Designs")

> [!NOTE]
> Unter Verwendung von `FormsAppCompatActivity` können sich die [Basisklassen für einige benutzerdefinierte Android-Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) unterscheiden.


## <a name="related-links"></a>Verwandte Links

- [Material Design-Unterstützung hinzufügen](appcompat.md)
