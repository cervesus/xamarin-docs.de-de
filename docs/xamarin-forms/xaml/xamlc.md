---
title: XAML-Kompilierung in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie XAML optional direkt in intermediate Language (IL) mit der Xamarin.Forms-XAML-Compiler (XAMLC) kompiliert werden kann.
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/02/2018
ms.openlocfilehash: 4635857fc850b2985f988f8c20ff854e487f79ed
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403402"
---
# <a name="xaml-compilation-in-xamarinforms"></a>XAML-Kompilierung in Xamarin.Forms

_XAML kann optional auch direkt in intermediate Language (IL) mit dem XAML-Compiler (XAMLC) kompiliert werden._

XAMLC bietet eine Reihe von Vorteilen:

- Er führt eine XAML-Überprüfung zur Kompilierzeit durch und benachrichtigt den Benutzer über eventuelle Fehler.
- Er entfernt etwas Lade- und Instanziierungszeit für XAML-Elemente.
- Er hilft bei der Verringerung der Dateigröße der finalen Assembly, indem XAML-Dateien nicht mehr eingeschlossen werden.

XAMLC ist standardmäßig deaktiviert, damit die Abwärtskompatibilität sichergestellt ist. Es kann durch Hinzufügen der Assembly und Klasse-Ebene aktiviert werden die `XamlCompilation` Attribut.

Im folgenden Codebeispiel wird veranschaulicht, XAMLC auf Assemblyebene zu aktivieren:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

In diesem Beispiel wird mit XAML-Fehlern, die zur Kompilierzeit statt zur Laufzeit gemeldet wird zur Kompilierzeit von allen XAML, die innerhalb der Assembly enthaltenen ausgeführt werden. Aus diesem Grund die `assembly` Präfix für die `XamlCompilation` Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Im folgenden Codebeispiel wird veranschaulicht, XAMLC auf Klassenebene aktivieren:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

In diesem Beispiel während der Kompilierung der XAML für die Überprüfung der `HomePage` Klasse erfolgen und Fehler, die als Teil des Kompilierungsprozesses gemeldet.

> [!NOTE]
> Die `XamlCompilation` Attribut und die `XamlCompilationOptions` Enumeration befinden sich in der `Xamarin.Forms.Xaml` -Namespace, der für deren Verwendung importiert werden muss.


## <a name="related-links"></a>Verwandte Links

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
