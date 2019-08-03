---
title: XAML-Feldmodifizierer in xamarin. Forms
description: Das Namespace-Attribut "x:FieldModifier" gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente an.
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/02/2019
ms.openlocfilehash: 0f6050de943ca9878cf41b448d44bf222689be56
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739451"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>XAML-Feldmodifizierer in xamarin. Forms

Das `x:FieldModifier` Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente an. Gültige Werte für das-Attribut sind:

- `private`– Gibt an, dass auf das generierte Feld für das XAML-Element nur innerhalb des Texts der Klasse, in der es deklariert ist, zugegriffen werden kann.
- `public`– Gibt an, dass das generierte Feld für das XAML-Element keine Zugriffs Einschränkungen hat.
- `protected`– Gibt an, dass der Zugriff auf das generierte Feld für das XAML-Element innerhalb der Klasse und von abgeleiteten Klassen Instanzen möglich ist.
- `internal`– Gibt an, dass auf das generierte Feld für das XAML-Element nur innerhalb von Typen in derselben Assembly zugegriffen werden kann.
- `notpublic`– Gibt an, dass auf das generierte Feld für das XAML-Element nur innerhalb von Typen in derselben Assembly zugegriffen werden kann.

Wenn der Wert des-Attributs nicht festgelegt ist, ist das generierte Feld für das-Element `private`standardmäßig.

> [!NOTE]
> Der Wert des-Attributs kann jede beliebige Schreibweise verwenden, da er von xamarin. Forms in Kleinbuchstaben konvertiert wird.

Die folgenden Bedingungen müssen erfüllt sein, damit `x:FieldModifier` ein Attribut verarbeitet werden muss:

- Das XAML-Element der obersten Ebene muss gültig `x:Class`sein.
- Das aktuelle XAML-Element verfügt `x:Name` über einen angegebenen.

Der folgende XAML-Code zeigt Beispiele für das Festlegen des-Attributs:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> Das `x:FieldModifier` -Attribut kann nicht verwendet werden, um die Zugriffsebene einer XAML-Klasse anzugeben.
