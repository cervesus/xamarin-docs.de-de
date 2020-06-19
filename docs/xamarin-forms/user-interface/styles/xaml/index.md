---
title: Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen
description: In diesem Handbuch wird erläutert, wie die Darstellung einer- Xamarin.Forms Anwendung mithilfe von XAML-Stilen angepasst wird.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72effe15d3456b5a48cbf5d09e889600134ac686
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84903089"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

Xamarin.FormsAnwendungen enthalten häufig mehrere Steuerelemente mit identischer Darstellung. Das Festlegen der Darstellung der einzelnen Steuerelemente kann sich wiederholt und fehleranfällig sein. Stattdessen können Stile erstellt werden, die die Darstellung des Steuer Elements anpassen, indem Sie die im Steuerelement-Typ verfügbaren Eigenschaften gruppieren und festlegen.

## <a name="explicit-styles"></a>[Explizite Stile](explicit.md)

Eine *explizite* Formatvorlage ist eine, die selektiv auf Steuerelemente angewendet wird, indem ihre Eigenschaften festgelegt werden [`Style`](xref:Xamarin.Forms.NavigableElement.Style) .

## <a name="implicit-styles"></a>[Implizite Stile](implicit.md)

Ein *impliziter* Stil ist ein Wert, der von allen Steuerelementen desselben verwendet wird [`TargetType`](xref:Xamarin.Forms.Style.TargetType) , ohne dass jedes Steuerelement auf den Stil verweisen muss.

## <a name="global-styles"></a>[Globale Stile](application.md)

Stile können global verfügbar gemacht werden, indem Sie dem der Anwendung hinzugefügt werden [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Dadurch wird das Duplizieren von Stilen über mehrere Seiten oder Steuerelemente hinweg vermieden.

## <a name="style-inheritance"></a>[Stil Vererbung](inheritance.md)

Stile können von anderen Formaten erben, um Duplizierungen zu verringern und die Wiederverwendung zu ermöglichen.

## <a name="dynamic-styles"></a>[Dynamische Stile](dynamic.md)

Stile reagieren nicht auf Eigenschafts Änderungen und bleiben für die Dauer einer Anwendung unverändert. Anwendungen können jedoch zur Laufzeit dynamisch auf Formatänderungen reagieren, indem Sie dynamische Ressourcen verwenden.

## <a name="device-styles"></a>[Geräte Stile](device.md)

Xamarin.Formsenthält sechs *dynamische* Stile, die als *Geräte* Stile bezeichnet werden, in der- [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) Klasse. Alle sechs Stile können nur auf- [`Label`](xref:Xamarin.Forms.Label) Instanzen angewendet werden.

## <a name="style-classes"></a>[Stil Klassen](style-class.md)

Xamarin.FormsStil Klassen ermöglichen das Anwenden mehrerer Stile auf ein Steuerelement, ohne dass auf eine Format Vererbung zurückgegriffen wird.
