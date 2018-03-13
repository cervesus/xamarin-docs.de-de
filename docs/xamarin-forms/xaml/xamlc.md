---
title: Verwendung von XAML-Kompilierung
description: XAML kann optional auch direkt mit dem XAML-Compiler (XAMLC) in der Zwischensprache (Intermediate Language, IL) kompiliert werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: c6fb404919621e1b22217b4461597ae07a5624c4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="xaml-compilation"></a>Verwendung von XAML-Kompilierung

_XAML kann optional auch direkt in intermediate Language (IL) mit der Verwendung von XAML-Compiler (XAMLC) kompiliert werden._

XAMLC bietet eine Reihe von Vorteilen:

- Er führt eine XAML-Überprüfung zur Kompilierzeit durch und benachrichtigt den Benutzer über eventuelle Fehler.
- Er entfernt etwas Lade- und Instanziierungszeit für XAML-Elemente.
- Er hilft bei der Verringerung der Dateigröße der finalen Assembly, indem XAML-Dateien nicht mehr eingeschlossen werden.

XAMLC ist standardmäßig deaktiviert, damit die Abwärtskompatibilität sichergestellt ist. Dies kann durch Hinzufügen von auf die Assembly und die Klasse Ebene aktiviert werden die `XamlCompilation` Attribut.

Das folgende Codebeispiel veranschaulicht das Aktivieren von XAMLC auf Assemblyebene:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

In diesem Beispiel kompilierzeitüberprüfung der XAML-Code enthaltenen der `PhotoApp` Namespace werden ausgeführt, mit der Verwendung von XAML-Fehler zur Kompilierzeit statt zur Laufzeit gemeldet wird.
Die `assembly` als Präfix für die `XamlCompilation` Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

Das folgende Codebeispiel veranschaulicht das Aktivieren von XAMLC auf Klassenebene:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

In diesem Beispiel der Kompilierung der XAML für die `HomePage` Klasse erfolgen und Fehler, die als Teil des Kompilierungsprozesses gemeldet.

> [!NOTE]
> Die `XamlCompilation` Attribut und der `XamlCompilationOptions` Enumeration befinden sich in der `Xamarin.Forms.Xaml0` -Namespace, die importiert werden muss, deren Verwendung.


## <a name="related-links"></a>Verwandte Links

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
