---
title: Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms XAML-Dateien?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9f9cf570da0d8e078d1d05e74dc0f65994080c6c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135888"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms XAML-Dateien?

Xamarin.Formsunterstützt derzeit keine visuellen Designer für XAML-Dateien. Aus diesem Grund wird beim Versuch, eine Forms-XAML-Datei im *XAML-Benutzeroberflächen Designer* von Visual Studio oder im *XAML-UI-Designer mit Codierung*zu öffnen, die folgende Fehlermeldung ausgelöst:

> "Die Datei kann nicht mit dem ausgewählten Editor geöffnet werden. Wählen Sie einen anderen Editor aus. "

Diese Einschränkung wird im Leitfaden für [ Xamarin.Forms XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md) beschrieben:

> "Es ist noch kein visueller Designer zum Erstellen von XAML-Code in Xamarin.Forms Anwendungen vorhanden. Deshalb müssen alle XAML-Code Hand geschrieben werden."

Die Xamarin.Forms XAML-Vorschau kann jedoch angezeigt werden, indem Sie die Menüoption **> anderen Windows > Xamarin.Forms Previewer anzeigen** auswählen.
