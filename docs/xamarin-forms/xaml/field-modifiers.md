---
title: XAML-Feldmodifizierern in Xamarin.Forms
description: 'Das X: FieldModifier-Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075297"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>XAML-Feldmodifizierern in Xamarin.Forms

_Die `x:FieldModifier` Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente._

## <a name="overview"></a>Übersicht

Gültige Werte des Attributs sind:

- `Public` – Gibt an, dass das generierte Feld für das XAML-Element `public`.
- `NotPublic` – Gibt an, dass das generierte Feld für das XAML-Element `internal` auf die Assembly.

Wenn der Wert des Attributs nicht festgelegt ist, kann das generierte Feld für das Element werden `private`.

Die folgenden Bedingungen müssen erfüllt sein, für eine `x:FieldModifier` Attribut verarbeitet werden:

- Das Element der obersten Ebene XAML muss ein gültiger `x:Class`.
- Das aktuelle XAML-Element verfügt über eine `x:Name` angegebenen.

Der folgende XAML zeigt Beispiele für das Attribut:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> Die `x:FieldModifier` Attribut kann nicht verwendet werden, um die Zugriffsebene einer XAML-Klasse anzugeben.
