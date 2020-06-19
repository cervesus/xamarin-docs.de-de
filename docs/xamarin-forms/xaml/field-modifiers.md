---
title: XAML-Feldmodifizierer inXamarin.Forms
description: Das Namespace-Attribut "x:FieldModifier" gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente an.
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/02/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: db00f522b71a8993ef0f7f6cf5070813ce07396a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138124"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>XAML-Feldmodifizierer inXamarin.Forms

Das `x:FieldModifier` Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente an. Gültige Werte für das-Attribut sind:

- `private`– Gibt an, dass auf das generierte Feld für das XAML-Element nur innerhalb des Texts der Klasse, in der es deklariert ist, zugegriffen werden kann.
- `public`– Gibt an, dass das generierte Feld für das XAML-Element keine Zugriffs Einschränkungen hat.
- `protected`– Gibt an, dass der Zugriff auf das generierte Feld für das XAML-Element innerhalb der Klasse und von abgeleiteten Klassen Instanzen möglich ist.
- `internal`– Gibt an, dass auf das generierte Feld für das XAML-Element nur innerhalb von Typen in derselben Assembly zugegriffen werden kann.
- `notpublic`– Gibt an, dass auf das generierte Feld für das XAML-Element nur innerhalb von Typen in derselben Assembly zugegriffen werden kann.

Wenn der Wert des-Attributs nicht festgelegt ist, ist das generierte Feld für das-Element standardmäßig `private` .

> [!NOTE]
> Der Wert des-Attributs kann jede beliebige Schreibweise verwenden, da Sie von in Kleinbuchstaben konvertiert wird Xamarin.Forms .

Die folgenden Bedingungen müssen erfüllt sein, damit ein `x:FieldModifier` Attribut verarbeitet werden muss:

- Das XAML-Element der obersten Ebene muss gültig sein `x:Class` .
- Das aktuelle XAML-Element verfügt über einen `x:Name` angegebenen.

Der folgende XAML-Code zeigt Beispiele für das Festlegen des-Attributs:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> Das- `x:FieldModifier` Attribut kann nicht verwendet werden, um die Zugriffsebene einer XAML-Klasse anzugeben.
