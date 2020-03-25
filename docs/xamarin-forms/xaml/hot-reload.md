---
title: XAML-Hot-Upload für xamarin. Forms
description: Erneutes Laden von Änderungen an ihrer XAML-Datei in der laufenden Anwendung, damit Sie Ihr xamarin. Forms-Projekt nicht nach jeder XAML-Änderung erstellen müssen.
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/14/2020
ms.openlocfilehash: a6cb5a0e3573ebf998bb2f81c08ff63c81678b54
ms.sourcegitcommit: ec112800a76089ab1db66fe24b8bbcc510e067b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159807"
---
# <a name="xaml-hot-reload-for-xamarinforms"></a>XAML-Hot-Upload für xamarin. Forms

XAML Hot Upload wird in Ihren vorhandenen Workflow integriert, um Ihre Produktivität zu steigern und Zeit zu sparen. Ohne das erneute Laden von XAML müssen Sie die APP jedes Mal erstellen und bereitstellen, wenn Sie eine XAML-Änderung sehen möchten. Wenn Sie beim Laden der XAML-Datei die XAML-Datei speichern, werden die Änderungen in der laufenden app Live übernommen. Außerdem werden der Navigations Zustand und die Daten beibehalten, sodass Sie Ihre Benutzeroberfläche schnell durchlaufen können, ohne Ihre Position in der APP zu verlieren. Mit XAML Hot Neuladen verbringen Sie daher weniger Zeit mit der Neuerstellung und Bereitstellung Ihrer Apps, um Änderungen an der Benutzeroberfläche zu überprüfen.

> [!NOTE]
> Wenn Sie eine WPF-oder UWP-app schreiben, finden Sie weitere Informationen unter [XAML-Hot-Upload für UWP und WPF](/visualstudio/debugger/xaml-hot-reload).
>
> Das Laden von XAML-Hot für xamarin. Forms funktioniert zurzeit _nicht_ für xamarin. Forms-UWP-Projekte.

## <a name="system-requirements"></a>Systemanforderungen

| IDE/Framework | Erforderliche Version |
|------|------------------|
|Visual Studio 2019 | 16,4 oder höher
Visual Studio 2019 für Mac | 8,4 oder höher
Xamarin.Forms | 4,1 oder höher

## <a name="enable-xaml-hot-reload-for-xamarinforms"></a>XAML-Hot-Upload für xamarin. Forms aktivieren

Wenn Sie mit einer Vorlage beginnen, ist das aktive XAML-Neuladen standardmäßig aktiviert, und das Projekt ist so konfiguriert, dass es ohne zusätzliches Setup funktioniert. Debuggen Sie Ihre APP auf einem Android-oder IOS-Emulator, Simulator oder einem physischen Gerät, ändern Sie den XAML-Code, und speichern Sie die Datei, um ein XAML-Hot-Neuladen

Wenn Sie mit einer vorhandenen xamarin. Forms-Projekt Mappe arbeiten, ist keine weitere Installation erforderlich, um das Laden von XAML-Hot zu verwenden, aber Sie müssen möglicherweise die Konfiguration überprüfen, um die bestmögliche Benutzererfahrung zu gewährleisten. Aktivieren Sie es zunächst in ihren IDE-Einstellungen:

* Aktivieren **Sie unter Windows** das Kontrollkästchen **xamarin Hot Neuladen aktivieren** unter Extras > **Optionen** > **xamarin** > **Hot Neuladen**.
* Aktivieren Sie auf dem Mac das Kontrollkästchen **xamarin Hot Neuladen aktivieren** unter **Visual Studio** > **Einstellungen** > **Tools für xamarin** > **XAML Hot Neuladen**.
  * In früheren Versionen von Visual Studio für Mac befindet sich das Menü unter **Visual Studio** > **Einstellungen** > **Projekte** > **xamarin Hot Neuladen**.

Überprüfen Sie dann in den Einstellungen für Android-und IOS-Builds, ob der Linker auf "nicht verknüpfen" oder "Link None" festgelegt ist. Wenn Sie XAML-Hot-Upload mit einem physischen IOS-Gerät verwenden möchten, müssen Sie auch **den Mono-Interpreter aktivieren** (Visual Studio 16,4 und höher) oder Add **--Interpreter** zu den **zusätzlichen mtouchscreen** -Argumenten (Visual Studio 16,3 und niedriger) aktivieren.

Sie können das folgende Flussdiagramm verwenden, um das Setup des vorhandenen Projekts für die Verwendung mit XAML Hot Neuladen zu überprüfen:

![Setup für das erneute Laden von XAML](hot-reload-images/hotreloadflowchart.png "Flussdiagramm für das XAML-Hot-Setup")

## <a name="resilient-reloading"></a>Resilientes erneutes Laden

Wenn Sie eine Änderung vornehmen, dass das Laden von XAML-Hot nicht erneut geladen werden kann, wird eine Fehlermeldung mit IntelliSense angezeigt. Diese Änderungen, die als grobe Bearbeitungen bezeichnet werden, beinhalten das falsche Schreibweise Ihrer XAML oder das Verknüpfen eines Steuer Elements mit einem Ereignishandler, der nicht vorhanden ist. Auch bei einer unhöflichen Bearbeitung können Sie den erneuten Ladevorgang fortsetzen, ohne die APP neu zu starten. nehmen Sie an anderer Stelle in der XAML-Datei eine andere Änderung vor, und Die ungrobe Bearbeitung wird nicht erneut geladen, aber Ihre anderen Änderungen werden weiterhin angewendet.

## <a name="reload-on-multiple-platforms-at-once"></a>Auf mehreren Plattformen gleichzeitig neu laden

XAML Hot Neuladen unterstützt das gleichzeitige Debuggen in Visual Studio und Visual Studio für Mac. Sie können ein Android-und ein IOS-Ziel gleichzeitig bereitstellen, um die Änderungen auf beiden Plattformen gleichzeitig anzuzeigen. Informationen zum Debuggen auf mehreren Plattformen finden Sie unter
* **Windows** Gewusst [wie: Festlegen mehrerer Start Projekte](https://docs.microsoft.com/visualstudio/ide/how-to-set-multiple-startup-projects?view=vs-2019)
* **Mac** [Festlegen mehrerer Start Projekte](https://docs.microsoft.com/visualstudio/mac/set-startup-projects?view=vsmac-2019)

## <a name="known-limitations"></a>Bekannte Einschränkungen

* Andere xamarin. Forms-Ziele, z. b. UWP und macOS, werden noch *nicht* unterstützt. Sie können den Fortschritt der UWP-Unterstützung [hier](https://developercommunity.visualstudio.com/idea/661682/xaml-hot-reload-for-xamarinforms-on-uwp.html)verfolgen.
* Sie können während einer XAML-Sitzung zum aktiven erneuten Laden keine Dateien oder nuget-Pakete hinzufügen, entfernen oder umbenennen. Wenn Sie eine Datei oder ein nuget-Paket hinzufügen oder entfernen, erstellen Sie Ihre APP erneut, und stellen Sie Sie erneut bereit.
* Legen Sie fest, dass der Linker **nicht verknüpft** oder **verknüpft** werden soll, um eine optimale Leistung zu erzielen Die Einstellung **Link SDK only** funktioniert meistens, kann jedoch in bestimmten Fällen fehlschlagen. Die Linker-Einstellungen finden Sie in ihren Android-und IOS-Buildoptionen.
* Das Debuggen auf einem physischen iPhone erfordert, dass der Interpreter XAML-Hot-Neuladen verwendet. Öffnen Sie hierzu die Projekteinstellungen, wählen Sie die Registerkarte IOS-Build aus, und vergewissern Sie sich, dass **die Einstellung Mono-Interpreter aktivieren** aktiviert ist. Möglicherweise müssen Sie die **Platt Form** Option oben auf der Eigenschaften Seite in **iPhone**ändern.
* Alle Verweise, `x:Name` die durch Zuweisen eines Steuer Elements zu einem anderen Feld oder einer Eigenschaft erstellt werden
* Das Aktualisieren der visuellen Hierarchie Ihrer Shell-Anwendung in appshell. XAML kann Probleme beim Verwalten des Zustands der Anwendung verursachen. Wenn Probleme auftreten, erstellen Sie die APP neu, damit Sie erneut geladen werden kann.
* Beim erneuten Laden von XAML C# kann Code nicht neu geladen werden, einschließlich Ereignis Handlern, benutzerdefinierte Steuerelemente, Seitencode Behind und zusätzliche Klassen.

## <a name="more-resources"></a>Weitere Ressourcen

* [Tipps und Tricks für das Laden von XAML-Hot](https://devblogs.microsoft.com/xamarin/tips-tricks-xaml-hot-reload/)
* [XAML-Hot-Upload für xamarin. Forms eingehend: xamarin Show](https://www.youtube.com/watch?v=crhjjPjzknk)

## <a name="troubleshooting"></a>Problembehandlung

* Wenn XAML-Hot-Neuladen nicht initialisiert werden kann:
  * Aktualisieren Sie die xamarin. Forms-Version.
  * Stellen Sie sicher, dass Sie über die neueste Version der IDE verfügen.
  * Legen Sie die Einstellungen für den Android-oder IOS-Linker auf **keine Verknüpfung** in den Buildeinstellungen des Projekts fest.
* Wenn beim Speichern der XAML-Datei nichts passiert, stellen Sie sicher, dass das XAML-Hot-Neuladen in der IDE aktiviert ist.
* Wenn Sie auf einem physischen iPhone Debuggen und Ihre APP nicht mehr reagiert, überprüfen Sie, ob der Interpreter aktiviert ist. Um dies zu **aktivieren, aktivieren Sie die Option Mono-Interpreter aktivieren** (Visual Studio 16,4/8.4 und up) oder Add **--Interpreter** in den **zusätzlichen mberührungs-Argument** Feldern (Visual Studio 16.3/8.3 und früher) in ihren IOS-Buildeinstellungen.

Um einen Fehler zu melden, verwenden Sie die **Hilfe** > **Senden Sie Feedback** > **melden Sie ein Problem** unter Windows, und **helfen** Sie > , **ein Problem auf dem** Mac zu melden.
