---
title: Empfohlene Präfixe in Xamarin.Forms XAML-Namespace
description: Die XmlnsPrefixAttribute-Klasse kann von Autoren von Steuerelementen verwendet werden, an der ein empfohlenes Präfix, einen XAML-Namespace, für die XAML-Verwendung zugeordnet werden soll.
ms.prod: xamarin
ms.assetid: 7B315BEC-7A35-48F4-A9C7-EF40255E95FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: 33f18b3f9c9ddb6ab31ca92e2f192ffad783ec0c
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557472"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>Empfohlene Präfixe in Xamarin.Forms XAML-Namespace

Die `XmlnsPrefixAttribute` Klasse kann von Autoren von Steuerelementen verwendet werden, an der ein empfohlenes Präfix, einen XAML-Namespace, für die XAML-Verwendung zugeordnet werden soll. Das Präfix ist nützlich, wenn Objekt Struktur-Serialisierung in XAML unterstützt, oder mit einer entwurfsumgebung interagiert, die XAML-Bearbeitungsfunktionen hat. Zum Beispiel:

- XAML-Text-Editoren können die `XmlnsPrefixAttribute` als Hinweis für einen anfänglichen XAML-Namespace `xmlns` Zuordnung.
- XAML-entwurfsumgebungen können die `XmlnsPrefixAttribute` so Zuordnungen der XAML hinzu, wenn Objekte aus einer Toolbox, und klicken Sie auf eine visuelle Entwurfsoberfläche ziehen.

Empfohlene Namespacepräfixe sollte definiert werden, auf der Assemblyebene mit der `XmlnsPrefixAttribute` -Konstruktor, der zwei Argumente akzeptiert: eine Zeichenfolge, die den Bezeichner für einen XAML-Namespace angibt, und eine Zeichenfolge, die ein empfohlenes Präfix angibt:

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

Präfixe sollten kurze Zeichenfolgen zu verwenden, da das Präfix in der Regel auf alle serialisierten Elemente angewendet wird, die aus dem XAML-Namespace enthalten sind. Aus diesem Grund haben die Präfixlänge der Zeichenfolge eine spürbare Auswirkung auf die Größe der serialisierten XAML-Ausgabe.

> [!NOTE]
> Mehr als eine `XmlnsPrefixAttribute` kann auf eine Assembly angewendet werden. Wenn Sie eine Assembly, die Typen für mehr als eine XAML-Namespace definiert verfügen, können Sie z. B. unterschiedliche Werte für jeden XAML-Namespace definieren.

## <a name="related-links"></a>Verwandte Links

- [Schemas für den benutzerdefinierten XAML-Namespace](custom-namespace-schemas.md)
- [XAML-Namespaces in Xamarin.Forms](namespaces.md)
