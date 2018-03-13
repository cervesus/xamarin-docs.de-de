---
title: Windows-Plattform-Funktionen
ms.topic: article
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: f77290a8c780d7dd5c936af576b39228d91687aa
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="windows-platform-features"></a>Windows-Plattform-Funktionen

Anwendungsentwicklung Xamarin.Forms für Windows-Plattformen erfordert Visual Studio. Die [Anforderungsseite](~/xamarin-forms/get-started/installation.md) enthält weitere Informationen über die erforderlichen Komponenten.

![](images/allhanselman.png "Xamarin.Forms-Anwendungen unter Windows")

## <a name="platform-support"></a>Plattformunterstützung

Die Xamarin.Forms-Vorlagen in Visual Studio verfügbaren enthalten ein Windows-Projekt standardmäßig:

* **Universelle Windows-Plattform-Apps** -apps mit Xamarin.Forms können auch für Windows 10 optimiert werden. Universelle (UWP)-Apps können auf Telefon, Tablet und desktop-Geräte ausführen.

Wenn Sie die richtige Entwicklungsoptionen in Visual Studio installiert haben, ist es auch möglich, [hinzufügen](installation/index.md) diese Projekttypen zur Unterstützung von früheren Versionen von Windows:

* **Windows 8.1** – können Sie Xamarin.Forms-apps, tablet bereitstellen und desktop Formfaktoren als Windows 8.1-app Projekt mithilfe von WinRT-Steuerelementen.
* **Windows Phone 8.1** -Xamarin.Forms bietet vollständige Unterstützung für die Windows Phone 8.1-Plattform, die mit dem WinRT. Das Aussehen und Verhalten von apps mithilfe von Windows Phone 8.1-Unterstützung kann auf Ihre früheren Xamarin.Forms Windows Phone-apps unterscheiden, die auf Silverlight basieren.


> [!NOTE]
> Xamarin.Forms 1.x und 2.x Unterstützung _Silverlight für Windows Phone 8_ Anwendungsentwicklung jedoch dieser Projekttyp veraltet ist.


## <a name="getting-started"></a>Erste Schritte

Wechseln Sie zu **Datei > Neu > Projekt** in Visual Studio und wählen Sie eines der **plattformübergreifende > leere App (Xamarin.Forms)** Vorlagen, um zu beginnen.

Ältere Xamarin.Forms-Lösungen oder erstellten auf MacOS, müssen nicht alle oben aufgeführten Windows-Projekte (aber sie müssen manuell hinzugefügt werden).
Wenn die Windows-Plattform, die Sie abzielen möchten nicht, die sich bereits in der Projektmappe besuchen ist die [setupanweisungen](installation/index.md) Hinzufügen der gewünschten Windows-Projekt Typ/s.


## <a name="samples"></a>Proben

[Die Beispiele sind](https://github.com/xamarin/xamarin-forms-book-preview-2) für Charless Buchs [ *Erstellen mobiler Apps mit Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) umfassen Windows Phone 8.1, Windows 8.1 und Projekte der universellen Windows-Plattform (für Windows 10).

Die ["Scott Hanselman" demo-app](https://github.com/jamesmontemagno/Hanselman.Forms) separat verfügbar ist, und weist daneben Apple Watch und tragen Sie Android-Projekte (bzw. mithilfe von Xamarin.iOS und Xamarin.Android, Xamarin.Forms wird nicht ausgeführt auf diesen Plattformen).


## <a name="related-links"></a>Verwandte Links

- [Einrichten der Windows-Projekte](~/xamarin-forms/platform/windows/installation/index.md)
