---
title: Verwendung von XAML-Feldmodifizierern in Xamarin.Forms
description: 'X: FieldModifier-Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209505"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Verwendung von XAML-Feldmodifizierern in Xamarin.Forms

_Die `x:FieldModifier` Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente._

## <a name="overview"></a>Übersicht

Gültige Werte des Attributs sind:

- `Public` – Gibt an, dass das generierte Feld für die Verwendung von XAML-Element ist `public`.
- `NotPublic` – Gibt an, dass das generierte Feld für die Verwendung von XAML-Element ist `internal` auf die Assembly.

Wenn der Wert des Attributs nicht festgelegt ist, werden die generierte Feld für das Element `private`.

Die folgenden Bedingungen müssen erfüllt sein, für die ein `x:FieldModifier` Attribut verarbeitet werden:

- XAML-Elements der obersten Ebene muss ein gültiger `x:Class`.
- Das aktuelle XAML-Element verfügt über eine `x:Name` angegebenen.

Der folgende XAML-Code zeigt Beispiele für das Attribut festlegen:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> Die `x:FieldModifier` Attribut kann nicht verwendet werden, an die Zugriffsebene einer XAML-Klasse.
