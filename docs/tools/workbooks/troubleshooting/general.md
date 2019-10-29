---
title: Bekannte Probleme & Problem Umgehungen
description: In diesem Dokument werden bekannte Probleme und Problem Umgehungen für Xamarin Workbooks beschrieben. Es werden CultureInfo-Probleme, JSON-Probleme und mehr erläutert.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c7b9f93c2d6339ba1fd26b27742ecfc0f438c5de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018163"
---
# <a name="known-issues--workarounds"></a>Bekannte Probleme & Problem Umgehungen

## <a name="persistence-of-cultureinfo-across-cells"></a>Dauerhaftigkeit von CultureInfo über Zellen hinweg

Das Festlegen von `System.Threading.CurrentThread.CurrentCulture` oder `System.Globalization.CultureInfo.CurrentCulture` wird nicht in Arbeitsmappenzellen auf Mono-basierten arbeitsmappenzielen (Mac, IOS und Android) aufgrund eines [Fehlers in der `AppContext.SetSwitch`Implementierung von Mono][appcontext-bug] beibehalten.

### <a name="workarounds"></a>Problemumgehung

- Legen Sie den `DefaultThreadCurrentCulture`"Application-Domain-Local" fest:

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- Oder aktualisieren Sie die Arbeitsmappen 1.2.1 oder höher, wodurch Zuweisungen in `System.Threading.CurrentThread.CurrentCulture` und `System.Globalization.CultureInfo.CurrentCulture` neu geschrieben werden, um das gewünschte Verhalten (umgehen des Mono-Fehlers) bereitzustellen.

## <a name="unable-to-use-newtonsoftjson"></a>"Newtonsoft. JSON" kann nicht verwendet werden.

### <a name="workaround"></a>Problemumgehung

- Aktualisieren Sie die Arbeitsmappen 1.2.1, mit denen die newtonsoft. JSON-9.0.1 installiert wird.
  Arbeitsmappen 1,3, derzeit im Alphakanal, unterstützen Version 10 und höher.

### <a name="details"></a>Details

"Newtonsoft. JSON 10" wurde freigegeben, was seine Abhängigkeit von Microsoft. CSharp widerspricht, was zu einem Konflikt mit der Versions Arbeitsmappe führt, um `dynamic`zu unterstützen. Dies wird in der Vorschauversion der Arbeitsmappen 1,3 behandelt, aber vorerst haben wir das Thema "newtonsoft. JSON" genauer an die Version 9.0.1 angeheftetes.

Nuget-Pakete, die explizit in Abhängigkeit von Newton Soft. JSON 10 oder neueren basieren, werden nur in-Arbeitsmappen 1,3 unterstützt, derzeit im Alphakanal.

## <a name="code-tooltips-are-blank"></a>Die Code Quick Infos sind leer.

[Der Monaco-Editor][monaco-bug] in Safari/WebKit enthält einen Fehler, der in der Mac-Arbeitsmappen-App verwendet wird, die dazu führt, dass Code Quick Infos ohne Text gerendert werden.

![](general-images/monaco-signature-help-bug.png)

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
