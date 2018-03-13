---
title: Android-Plattform-Funktionen
description: "Hinzufügen von Android-spezifische Funktionalität zu apps mit Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 9b5e9a4c449bc99bd88fc415f5ebb969d2c2a08a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-features"></a>Android-Plattform-Funktionen

## <a name="platform-support"></a>Plattformunterstützung

Die Standardeinstellung Xamarin.Forms Android-Projekt wird im älteren Format des Steuerelements Renderering, die vor Android 5.0 wurde verwendet. Anwendungen, die mit der Vorlage erstellt haben `FormsApplicationActivity` als Basisklasse für die Hauptaktivität.

## <a name="material-design-via-appcompat"></a>Material Entwurf über AppCompat

Xamarin.Forms verfügt auch über ein optionales `FormsAppCompatActivity` , verwendet **AppCompat** Features von Android Designs Material Entwurf zu implementieren.

Um das Xamarin.Forms-Android-Projekt Material Entwurf Designs hinzugefügt haben, befolgen Sie die [installationsanweisungen für AppCompat unterstützen](appcompat.md)

So sieht die **Todo** Beispiel hat den Standardwert `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "TODO-Beispielanwendung ohne AppCompat")](images/before-appcompat.png#lightbox "TODO-Beispielanwendung ohne AppCompat")

Und dies ist der gleiche Code nach dem Upgrade des Projekts verwenden `FormsAppCompatActivity` (und Hinzufügen der zusätzlichen Designinformationen):

[![](images/post-appcompat-sml.png "TODO-Beispielanwendung mit AppCompat und Designumgebung")](images/post-appcompat.png#lightbox "TODO-Beispielanwendung mit AppCompat und Designs")

> [!NOTE]
> Bei Verwendung `FormsAppCompatActivity`, [Basisklassen für einige Android benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) unterscheiden.


## <a name="related-links"></a>Verwandte Links

- [Material Design-Unterstützung hinzufügen](appcompat.md)
