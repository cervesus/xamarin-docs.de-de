---
title: Formatieren Sie Xamarin.Forms-Apps, die mit der Verwendung von XAML-Stile
description: Dieses Handbuch erläutert, wie die Darstellung einer Xamarin.Forms-Anwendung mithilfe von XAML-Stile anpassen.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f439e3ba888b67ac1752eae95149adcf55055943
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245871"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Formatieren Sie Xamarin.Forms-Apps, die mit der Verwendung von XAML-Stile

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Xamarin.Forms-Anwendungen enthalten häufig mehrere Steuerelemente, die eine identische Darstellung haben. Die Darstellung der einzelnen Steuerelemente festlegen kann sich wiederholende und fehleranfällig. Stattdessen können Stile erstellt werden, die Darstellung anpassen Gruppierung und legt die Eigenschaften für den Steuerelementtyp "verfügbar.

## <a name="explicit-stylesexplicitmd"></a>[Explizite Stile](explicit.md)

Ein *explizite* Stil ist eine, die selektiv auf Steuerelemente angewendet wird, durch Festlegen von deren [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften.

## <a name="implicit-stylesimplicitmd"></a>[Implizite Stile](implicit.md)

Ein *implizite* Stil ist eine, die von allen Steuerelementen des gleichen verwendet wird [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), ohne dass jedes Steuerelement auf den Stil zu verweisen.

## <a name="global-stylesapplicationmd"></a>[Globale Stile](application.md)

Stile verfügbar gemacht werden global von der Anwendungsverzeichnis hinzugefügt [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Dies hilft, um Formatvorlagen auf Seiten oder Steuerelemente Rechnung zu tragen.

## <a name="style-inheritanceinheritancemd"></a>[Stilvererbung](inheritance.md)

Stile können von anderen Formatvorlagen Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben.

## <a name="dynamic-stylesdynamicmd"></a>[Dynamische Stile](dynamic.md)

Stile nicht reagieren auf eigenschaftenänderungen, und für die Dauer einer Anwendung unverändert bleiben. Allerdings können Anwendungen auf Änderungen an Formatvorlagen zur Laufzeit dynamisch zu reagieren, dynamische Ressourcen nutzen.

## <a name="device-stylesdevicemd"></a>[Gerätestile](device.md)

Xamarin.Forms enthält sechs *dynamische* Formatvorlagen, bekannt als *Gerät* Stile, in der [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Klasse. Alle sechs Formatvorlagen können angewendet werden, um [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) nur Instanzen.
