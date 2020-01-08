---
title: XAML-Hot-Upload für xamarin. Forms
description: Erneutes Laden von Änderungen an ihrer XAML-Datei in der laufenden Anwendung, sodass Sie das xamarin. Forms-Projekt nicht nach jeder XAML-Änderung erstellen müssen.
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 08/13/2019
ms.openlocfilehash: d94f18d00ebf6eeec5f33343b5c0f985ba2a6ea8
ms.sourcegitcommit: 9ab907e053c57fc96419149f83187bc3e8983a6b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75655406"
---
# <a name="xaml-hot-reload-for-xamarinforms-preview"></a>XAML-Hot-Upload für xamarin. Forms (Vorschau)

XAML Hot Upload wird in Ihren vorhandenen Workflow integriert, um Ihre Produktivität zu steigern und Zeit zu sparen. Ohne das erneute Laden von XAML müssen Sie die APP jedes Mal erstellen und bereitstellen, wenn Sie eine XAML-Änderung sehen möchten. Wenn Sie beim Laden der XAML-Datei die XAML-Datei speichern, werden die Änderungen in der laufenden app Live übernommen. Außerdem werden der Navigations Zustand und die Daten beibehalten, sodass Sie Ihre Benutzeroberfläche schnell durchlaufen können, ohne Ihre Position in der APP zu verlieren. Mit XAML Hot Neuladen verbringen Sie daher weniger Zeit mit der Neuerstellung und Bereitstellung Ihrer Apps, um Änderungen an der Benutzeroberfläche zu überprüfen.

> [!NOTE]
> Wenn Sie eine WPF-oder UWP-app schreiben, finden Sie weitere Informationen unter [XAML-Hot-Upload für UWP und WPF](/visualstudio/debugger/xaml-hot-reload).
>
> Das Laden von XAML-Hot für xamarin. Forms funktioniert zurzeit _nicht_ für xamarin. Forms-UWP-Projekte.

## <a name="system-requirements"></a>Systemanforderungen

| IDE/Framework | Erforderliche Version |
|------|------------------|
|Visual Studio 2019 | 16,4 oder höher
Visual Studio 2019 für Mac | 8,4 oder höher
Xamarin.Forms | 4,1 oder höher

## <a name="use-xaml-hot-reload-for-xamarinforms"></a>Verwenden von XAML-Hot-Neuladen für xamarin. Forms

Für die Verwendung von XAML Hot Neuladen ist keine zusätzliche Installation oder Einrichtung erforderlich. Es ist in Visual Studio integriert und kann in den IDE-Einstellungen aktiviert werden. Nach der Aktivierung können Sie mit der Verwendung von XAML Hot Neuladen beginnen, indem Sie Ihre APP auf einem Emulator, Simulator oder einem physischen Gerät Debuggen. Derzeit funktioniert das Laden von XAML-Hot nur beim Debuggen unter IOS oder Android.

Unter Windows kann das aktive XAML-Neuladen durch Aktivieren des Kontrollkästchens " **xamarin Hot Neuladen aktivieren** " unter **Extras > ** **Optionen** > **xamarin** > **Hot Neuladen**aktiviert werden.

Auf einem Mac kann das aktive XAML-Neuladen durch Aktivieren des Kontrollkästchens " **xamarin Hot Neuladen aktivieren** " in **Visual Studio** > **Einstellungen** > **Projekten** > **xamarin Hot Neuladen**aktiviert werden.

## <a name="resilient-reloading"></a>Resilientes erneutes Laden

Wenn Sie eine Änderung vornehmen, dass das Laden von XAML-Hot nicht erneut geladen werden kann, wird eine Fehlermeldung mit IntelliSense angezeigt. Diese Änderungen, die als grobe Bearbeitungen bezeichnet werden, beinhalten das falsche Schreibweise Ihrer XAML oder das Verknüpfen eines Steuer Elements mit einem Ereignishandler, der nicht vorhanden ist. Auch bei einer unhöflichen Bearbeitung können Sie den erneuten Ladevorgang fortsetzen, ohne die APP neu zu starten. nehmen Sie an anderer Stelle in der XAML-Datei eine andere Änderung vor, und Die ungrobe Bearbeitung wird nicht erneut geladen, aber Ihre anderen Änderungen werden weiterhin angewendet.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Sie können während einer XAML-Sitzung zum aktiven erneuten Laden keine Dateien oder nuget-Pakete hinzufügen, entfernen oder umbenennen. Wenn Sie eine Datei oder ein nuget-Paket hinzufügen oder entfernen, erstellen Sie Ihre APP erneut, und stellen Sie Sie erneut bereit.
- Legen Sie den Linker auf **keine Verknüpfung** fest, um eine optimale Leistung zu erzielen. Die Einstellung **Link SDK only** funktioniert meistens, kann jedoch in bestimmten Fällen fehlschlagen.
- Das Debuggen auf einem physischen iPhone erfordert, dass der Interpreter XAML-Hot-Neuladen verwendet. Öffnen Sie hierzu die Projekteinstellungen, wählen Sie die Registerkarte IOS-Build aus, und vergewissern Sie sich, dass **die Einstellung Mono-Interpreter aktivieren** aktiviert ist. Möglicherweise müssen Sie die **Platt Form** Option oben auf der Eigenschaften Seite in **iPhone**ändern.
- Alle Verweise, `x:Name` die durch Zuweisen eines Steuer Elements zu einem anderen Feld oder einer Eigenschaft erstellt werden
- Das Aktualisieren der visuellen Hierarchie Ihrer Shell-Anwendung in **appshell. XAML** kann Probleme beim Verwalten des Zustands der Anwendung verursachen. Erstellen Sie die APP neu, damit Sie erneut geladen wird.
- Beim erneuten Laden von XAML C# kann Code nicht neu geladen werden, einschließlich Ereignis Handlern, benutzerdefinierte Steuerelemente, Seitencode Behind und zusätzliche Klassen.
- Funktioniert _nicht_ auf anderen xamarin. Forms-unterstützten Plattformen (z. b. Mac OS oder UWP).

## <a name="migrate-from-the-private-preview"></a>Migrieren von der privaten Vorschau

Wenn Sie Teil der privaten Vorschau sind, wird die XAML-Erweiterung zum erneuten Laden automatisch aktualisiert, wenn Visual Studio aktualisiert wird. Wenn Sie Visual Studio nicht aktualisieren möchten, können Sie weiterhin die aktuelle Version von XAML Hot Neuladen verwenden. Sie erhalten jedoch keine weiteren Updates über den Feed der privaten Vorschau Erweiterung.

## <a name="troubleshooting"></a>Problembehandlung

- Wenn XAML-Hot-Neuladen nicht initialisiert werden kann:
  - Aktualisieren Sie die xamarin. Forms-Version.
  - Stellen Sie sicher, dass Sie über die neueste Version der IDE verfügen.
  - Legen Sie die Einstellungen für den Android-oder IOS-Linker auf **keine Verknüpfung** in den Buildeinstellungen des Projekts fest.
- Wenn beim Speichern der XAML-Datei nichts passiert, stellen Sie sicher, dass das heiße laden in der IDE aktiviert ist.
- Wenn Sie auf einem physischen iPhone Debuggen und Ihre APP nicht mehr reagiert, überprüfen Sie, ob der Interpreter aktiviert ist. Um es zu aktivieren, öffnen Sie die Projekteinstellungen, wählen Sie die Registerkarte IOS-Build aus, und aktivieren Sie die Einstellung **Mono-Interpreter aktivieren** .

Um einen Fehler zu melden, verwenden Sie das Feedback Tool in der **Hilfe** > Senden von Feedback > **melden Sie ein Problem** Menü unter Windows, **und > ** melden **Sie** **ein problemmenü** auf einem Mac.
