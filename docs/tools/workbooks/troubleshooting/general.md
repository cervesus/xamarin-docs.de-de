---
title: Bekannte Probleme und Problemumgehungen
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 0dcff6402612fb8dad1e80f9158298b1171da75a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="known-issues--workarounds"></a>Bekannte Probleme und Problemumgehungen

## <a name="persistence-of-cultureinfo-across-cells"></a>Dauerhaftigkeit von CultureInfo in Zellen

Festlegen von `System.Threading.CurrentThread.CurrentCulture` oder `System.Globalization.CultureInfo.CurrentCulture` über Arbeitsmappe Zellen auf Arbeitsmappen Mono-basierte Zielen (Mac, iOS und Android) aufgrund von nicht beibehalten eine [Fehler in der Mono `AppContext.SetSwitch` ] [ appcontext-bug] Implementierung .

### <a name="workarounds"></a>Problemumgehung

* Legen Sie die Anwendung Domäne lokale `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Oder Aktualisierung in Arbeitsmappen 1.2.1 oder höher ist, werden die Zuweisungen zu schreiben `System.Threading.CurrentThread.CurrentCulture` und `System.Globalization.CultureInfo.CurrentCulture` das gewünschte Verhalten (arbeiten für den Mono / Fehler) bereit.

## <a name="unable-to-use-newtonsoftjson"></a>Keine Newtonsoft.Json verwenden

### <a name="workaround"></a>Problemumgehung

* Aktualisieren Sie auf Arbeitsmappen 1.2.1, wodurch Newtonsoft.Json 9.0.1 installiert werden.
  Arbeitsmappen 1.3 derzeit in den alpha-Kanal Versionen 10 und höher unterstützt.

### <a name="details"></a>Details

Newtonsoft.Json 10 wurde veröffentlicht, das Deaktivieren von seiner Abhängigkeit Microsoft.CSharp, wodurch ein Konflikt mit der Arbeitsmappen der Version ausgeliefert wird, zur Unterstützung `dynamic`. Dies wird im Preview-Release Arbeitsmappen 1.3 adressiert, aber vorläufig aufweisen gearbeitet umgehen dieses durch anheften Newtonsoft.Json speziell für die Version 9.0.1.

NuGet-Pakete explizit je nach Newtonsoft.Json 10 oder höher werden nur in Arbeitsmappen 1.3 derzeit in den alpha-Kanal unterstützt.

## <a name="code-tooltips-are-blank"></a>Code QuickInfos sind leer

Es ist ein [Fehler in dem Monaco-Editor] [ monaco-bug] in Safari/WebKit, die in der Mac-Arbeitsmappen-app verwendet wird, das Code QuickInfos Rendering ohne Text ergibt.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Problemumgehung

* Klicken auf die QuickInfo ein, sobald es angezeigt wird, erzwingt den Text gerendert.

* Oder aktualisieren Sie Arbeitsmappen 1.2.1 oder höher

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>SkiaSharp-Renderer fehlen in Arbeitsmappen 1.3.

Arbeitsmappen 1.3 ab, wurde entfernt und die SkiaSharp Renderern, die wir ausgeliefert wurden in Arbeitsmappen 0.99.0, zugunsten SkiaSharp bietet den Renderern mit sich selbst, indem Sie unsere [SDK] [/ Führungslinien/Cross-Platform/Arbeitsmappen/Sdk /].

### <a name="workaround"></a>Problemumgehung

* Aktualisieren Sie SkiaSharp auf die neueste Version in NuGet. Dies ist zum Zeitpunkt der Verfassung 1.57.1.

## <a name="related-links"></a>Verwandte Links

- [Melden von Fehlern](~/tools/workbooks/install.md#reporting-bugs)
