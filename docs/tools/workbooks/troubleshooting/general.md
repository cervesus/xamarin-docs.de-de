---
title: Bekannte Probleme & Problem Umgehungen
description: In diesem Dokument werden bekannte Probleme und Problem Umgehungen für Xamarin Workbooks beschrieben. Es werden CultureInfo-Probleme, JSON-Probleme und mehr erläutert.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: aa6bbf9336acbff8558744220b9f4b8634f46421
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997227"
---
# <a name="known-issues--workarounds"></a>Bekannte Probleme & Problem Umgehungen

## <a name="persistence-of-cultureinfo-across-cells"></a>Dauerhaftigkeit von CultureInfo über Zellen hinweg

`System.Threading.CurrentThread.CurrentCulture`Das Festlegen `System.Globalization.CultureInfo.CurrentCulture` von oder wird nicht in Arbeitsmappenzellen auf Mono-basierten Arbeitsmappen-Zielen (Mac, IOS und Android) aufgrund eines [Fehlers in `AppContext.SetSwitch` der Mono][appcontext-bug] -Implementierung beibehalten.

### <a name="workarounds"></a>Problemumgehungen

- Legen Sie "Application-Domain-Local" fest `DefaultThreadCurrentCulture` :

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- Oder aktualisieren Sie die Arbeitsmappen 1.2.1 oder höher, wodurch Zuweisungen zu `System.Threading.CurrentThread.CurrentCulture` und `System.Globalization.CultureInfo.CurrentCulture` für das gewünschte Verhalten neu geschrieben werden (umgehen des Mono-Fehlers).

## <a name="unable-to-use-newtonsoftjson"></a>Newtonsoft.Jskann nicht verwendet werden.

### <a name="workaround"></a>Problemumgehung

- Aktualisieren Sie die Arbeitsmappen 1.2.1, die Newtonsoft.Jsauf 9.0.1 installieren.
  Arbeitsmappen 1,3, derzeit im Alphakanal, unterstützen Version 10 und höher.

### <a name="details"></a>Details

Newtonsoft.Jsauf 10 wurde freigegeben, was die Abhängigkeit von Microsoft. CSharp widerspricht, was zu einem Konflikt mit der Version der zu unterstützten Arbeitsmappen führt `dynamic` . Dies wird in der Vorschauversion der Arbeitsmappen 1,3 behandelt, aber bisher haben wir dies umgangen, indem wir Newtonsoft.Jsspeziell an Version 9.0.1 angeheftetes.

Nuget-Pakete, die explizit in Abhängigkeit von Newtonsoft.Jsauf 10 oder höher sind, werden nur in-Arbeitsmappen 1,3 unterstützt, derzeit im Alphakanal.

## <a name="code-tooltips-are-blank"></a>Die Code Quick Infos sind leer.

[Der Monaco-Editor][monaco-bug] in Safari/WebKit enthält einen Fehler, der in der Mac-Arbeitsmappen-App verwendet wird, die dazu führt, dass Code Quick Infos ohne Text gerendert werden.

![Monaco-Quick Infos-Rendering ohne Text](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Problemumgehung

- Wenn Sie auf die QuickInfo klicken, nachdem Sie angezeigt wird, wird der Text zum Rendering gezwungen.

- Oder aktualisieren Sie die Arbeitsmappen 1.2.1 oder neuer.

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Skiasharp-Renderer fehlen in Arbeitsmappen 1,3

Beginnend mit den Arbeitsmappen 1,3 haben wir die skiasharp-Renderer entfernt, die wir in den Arbeitsmappen 0.99.0 haben. Dies hat den Vorteil, dass über skiasharp die Renderer mit unserem [SDK](~/tools/workbooks/sdk/index.md)bereitgestellt werden.

### <a name="workaround"></a>Problemumgehung

- Aktualisieren Sie skiasharp auf die neueste Version von nuget. Zum Zeitpunkt des Schreibens ist dies 1.57.1.

## <a name="related-links"></a>Verwandte Links

- [Melden von Fehlern](~/tools/workbooks/install.md#reporting-bugs)
