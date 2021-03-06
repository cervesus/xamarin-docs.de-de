---
title: Empfohlene Präfixe für den XAML-Namespace inXamarin.Forms
description: Die xmlnspree fixattribute-Klasse kann von Steuerelement Autoren verwendet werden, um ein empfohlenes Präfix anzugeben, das einem XAML-Namespace für die XAML-Verwendung zugeordnet werden soll.
ms.prod: xamarin
ms.assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 71ae523f40f3f7529c12f853778404e224fbae30
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138137"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>Empfohlene Präfixe für den XAML-Namespace inXamarin.Forms

Die- `XmlnsPrefixAttribute` Klasse kann von Steuerelement Autoren verwendet werden, um ein empfohlenes Präfix anzugeben, das einem XAML-Namespace für die XAML-Verwendung zugeordnet werden soll. Das Präfix ist nützlich, wenn die objektstrukturserialisierung in XAML unterstützt wird, oder wenn mit einer Entwurfs Umgebung interagiert wird, die XAML-Bearbeitungsfunktionen hat. Beispiel:

- XAML-Text-Editoren können `XmlnsPrefixAttribute` als Hinweis für eine anfängliche XAML-Namespace `xmlns` Zuordnung verwenden.
- XAML-Entwurfs Umgebungen können verwenden `XmlnsPrefixAttribute` , um dem XAML Zuordnungen hinzuzufügen, wenn Objekte aus einer Toolbox auf eine visuelle Entwurfs Oberfläche gezogen werden.

Empfohlene Namespace Präfixe sollten auf Assemblyebene mit dem `XmlnsPrefixAttribute` Konstruktor definiert werden, der zwei Argumente annimmt: eine Zeichenfolge, die den Bezeichner eines XAML-Namespace angibt, und eine Zeichenfolge, die ein empfohlenes Präfix angibt:

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

Präfixe sollten kurze Zeichen folgen verwenden, da das Präfix in der Regel auf alle serialisierten Elemente angewendet wird, die aus dem XAML-Namespace stammen. Daher kann die Länge der Präfix Zeichenfolge eine spürbare Auswirkung auf die Größe der serialisierten XAML-Ausgabe haben.

> [!NOTE]
> Es können mehrere `XmlnsPrefixAttribute` auf eine Assembly angewendet werden. Wenn Sie z. b. über eine Assembly verfügen, die Typen für mehr als einen XAML-Namespace definiert, können Sie für jeden XAML-Namespace unterschiedliche Präfix Werte definieren.

## <a name="related-links"></a>Verwandte Links

- [Schemas für den benutzerdefinierten XAML-Namespace](custom-namespace-schemas.md)
- [XAML-Namespaces inXamarin.Forms](namespaces.md)
