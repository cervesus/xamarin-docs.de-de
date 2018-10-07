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

Die Standardeinstellung eines Xamarin.Forms Android-Projektes verwendet ein älteres Format des Steuerelement Rendererings, welches von Android 5.0 wurde verwendet. Anwendungen, die mit dieser Vorlage erstellt wurden haben `FormsApplicationActivity` als Basisklasse für die Hauptaktivität.

## <a name="material-design-via-appcompat"></a>Material Design per AppCompat

Xamarin.Forms verfügt auch über ein optionales `FormsAppCompatActivity` , welches Android **AppCompat** Funktionen verwendet um Material Design zu implementieren.

Um dem Xamarin.Forms-Android-Projekt Material Designs Themese hinzuzufügen, befolgen Sie die [installationsanweisungen für AppCompat Unterstützung](appcompat.md)

So sieht das **Todo** Beispiel mit dem Standardwert `FormsApplicationActivity` aus:

[![](images/before-appcompat-sml.png "TODO-Beispielanwendung ohne AppCompat")](images/before-appcompat.png#lightbox "TODO-Beispielanwendung ohne AppCompat")

Und dies ist der gleiche Code nach dem Upgrade des Projekts auf `FormsAppCompatActivity` (und Hinzufügen der zusätzlicher Designinformationen):

[![](images/post-appcompat-sml.png "TODO-Beispielanwendung mit AppCompat und Designumgebung")](images/post-appcompat.png#lightbox "TODO-Beispielanwendung mit AppCompat und Designs")

> [!NOTE]
> Bei Verwendung von `FormsAppCompatActivity`, sind die [Basisklassen für einige Android benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) unterschiedlich.


## <a name="related-links"></a>Verwandte Links

- [Material Design-Unterstützung hinzufügen](appcompat.md)
