---
title: Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms-XAML-Dateien?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 43088beba6c6a86330cac164856be98d88f07fe2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61247795"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms-XAML-Dateien?

Xamarin.Forms unterstützt derzeit keine visuelle Designer für XAML-Dateien. Aus diesem Grund beim Versuch, eine Forms XAML-Datei in der Visual Studio öffnen *XAML-Benutzeroberflächen-Designer* oder *XAML-Benutzeroberflächen-Designer mit Codierung*, wird ausgelöst, die folgende Fehlermeldung angezeigt:

> "Die Datei kann nicht mit dem ausgewählten-Editor geöffnet werden. Wählen Sie einen anderen Editor."

Diese Einschränkung wird beschrieben, der [Übersicht über die](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) im Abschnitt der [Xamarin.Forms XAML Basics](~/xamarin-forms/xaml/xaml-basics/index.md) Handbuch:

> "Es ist noch keinem visuellen Designer für die Generierung von XAML in Xamarin.Forms-Anwendungen, sodass alle XAML muss handschriftlichen."

Allerdings kann die Xamarin.Forms-XAML-Vorschau angezeigt werden, durch Auswählen der **Ansicht > Other Windows > Xamarin.Forms-Vorschau** Menüoption.
