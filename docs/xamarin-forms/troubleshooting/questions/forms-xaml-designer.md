---
title: Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms-XAML-Dateien?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: ce318faab1af07b95a7769d81a506979b37a34e4
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73843003"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Warum funktioniert der Visual Studio-XAML-Designer nicht für Xamarin.Forms-XAML-Dateien?

Xamarin. Forms unterstützt derzeit keine visuellen Designer für XAML-Dateien. Aus diesem Grund wird beim Versuch, eine Forms-XAML-Datei im *XAML-Benutzeroberflächen Designer* von Visual Studio oder im *XAML-UI-Designer mit Codierung*zu öffnen, die folgende Fehlermeldung ausgelöst:

> "Die Datei kann nicht mit dem ausgewählten Editor geöffnet werden. Wählen Sie einen anderen Editor aus. "

Diese Einschränkung wird im Handbuch zu den [Grundlagen von xamarin. Forms-XAML](~/xamarin-forms/xaml/xaml-basics/index.md) beschrieben:

> "Es ist noch kein visueller Designer zum Erstellen von XAML-Code in xamarin. Forms-Anwendungen vorhanden. Deshalb müssen alle XAML-Code Hand geschrieben werden."

Allerdings kann xamarin. Forms-XAML-Previewer angezeigt werden, indem Sie die **Previewer-Option > andere Windows > xamarin. Forms-Vorschau** auswählen.
