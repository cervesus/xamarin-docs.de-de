---
title: Formatieren von Xamarin.Forms-Apps, die mithilfe von XAML-Stile
description: Dieses Handbuch wird erläutert, wie die Darstellung einer Xamarin.Forms-Anwendung mithilfe von XAML-Stile anpassen.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 5145572b30302e58c36250fff40e8b637fcd221f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995074"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Formatieren von Xamarin.Forms-Apps, die mithilfe von XAML-Stile

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Xamarin.Forms-Anwendungen enthalten häufig mehrere Steuerelemente, die eine identische Darstellung. Die Darstellung der einzelnen Steuerelemente festlegen kann sein, sich wiederholende und fehleranfällig. Stattdessen können Stile erstellt werden, die steuerelementdarstellung anpassen, Gruppierung und legt die Eigenschaften für den Steuerelementtyp verfügbar.

## <a name="explicit-stylesexplicitmd"></a>[Explizite Stile](explicit.md)

Ein *explizite* Stil ist, die selektiv auf Steuerelemente, durch Festlegen angewendet wird ihrer [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaften.

## <a name="implicit-stylesimplicitmd"></a>[Implizite Stile](implicit.md)

Ein *implizite* Stil ist eine, mit dem alle Steuerelemente des gleichen [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), ohne jedes Steuerelement auf die Formatvorlage verweisen.

## <a name="global-stylesapplicationmd"></a>[Globale Stile](application.md)

Stile verfügbar gemacht werden global durch Hinzufügen zu der Anwendung [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Dadurch wird die um Duplizierung von Formatvorlagen für Seiten oder Steuerelemente zu vermeiden.

## <a name="style-inheritanceinheritancemd"></a>[Stilvererbung](inheritance.md)

Stile können andere Stile Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben.

## <a name="dynamic-stylesdynamicmd"></a>[Dynamische Stile](dynamic.md)

Stile nicht reagieren auf Änderungen der Eigenschaften, und für die Dauer einer Anwendung unverändert bleiben. Allerdings können Anwendungen mithilfe von dynamischen Ressourcen auf Änderungen an Formatvorlagen dynamisch zur Laufzeit reagieren.

## <a name="device-stylesdevicemd"></a>[Gerätestile](device.md)

Xamarin.Forms umfasst sechs *dynamische* Formatvorlagen, bekannt als *Gerät* Stile, in der [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) Klasse. Alle sechs Formatvorlagen können angewendet werden, um [ `Label` ](xref:Xamarin.Forms.Label) nur Instanzen.
