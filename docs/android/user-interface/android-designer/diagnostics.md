---
title: Android-layoutdiagnose
description: Erläutert die Android-layoutdiagnose und den Einstieg
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 5c29a1a80d8c1f599f0bbc750d22d8334ddb3494
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247815"
---
# <a name="android-layout-diagnostics"></a>Android-layoutdiagnose

Die Android-layoutdiagnose wurde entwickelt, um die Qualität von Android-Layoutdateien zu verbessern, indem allgemeine Qualitätsprobleme und hilfreiche Optimierungen hervorgehoben werden. Diese Funktion ist sowohl für Visual Studio 16.5 + als auch für Visual Studio für Mac 8.5 + verfügbar.

Ein Standardsatz von Analyzern wird für eine Vielzahl von Problemen bereitgestellt, die jeweils angepasst werden können, um die spezifischen Anforderungen eines Projekts abzudecken. Die Analysen basieren auf dem Android-linting-System locker.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Aktivieren der Android-layoutdiagnose in Visual Studio 2019

Stellen Sie sicher, dass die layoutdiagnose-Einstellung **layoutdiagnose aktivieren**aktiviert ist. Um auf diese Optionsseite zuzugreifen, wählen Sie Extras > **Optionen**aus, und **Wählen Sie dann** **Text-Editor** > **Android-XML** - > **erweitert**:

![Dialogfeld "Optionen" zum Aktivieren der Diagnose Option](diagnostics-images/AndroidDiagnosticsEnableOption.png)

Nach der Aktivierung zeigt der Android-Layouteditor Probleme an:

![Aktivieren der Android-Diagnose in Visual Studio 2019](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Android-layoutdiagnose auf Visual Studio für Mac aktivieren

Stellen Sie sicher, dass die layoutdiagnose-Einstellung **layoutdiagnose aktivieren**aktiviert ist. Um auf diese Optionsseite zuzugreifen, wählen Sie **Visual Studio** > **Einstellungen...** , und wählen Sie dann **Text-Editor** > **Android-XML**aus:

![Dialogfeld "Einstellungen" mit dem Aktivieren der Diagnose Option](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

Nach der Aktivierung zeigt der Android-Layouteditor Probleme an:

![Android-Diagnose auf Visual Studio für Mac aktiviert](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>Features

In den folgenden Abschnitten werden die verfügbaren Funktionen in der Android-layoutdiagnose erläutert.

### <a name="analyzers"></a>Analysemodule

Analyzers werden verwendet, um Probleme in Layoutdateien zu erkennen. Einige Möglichkeiten sind das reduzieren hart codierter Werte, das Verbessern der Leistung und das Kennzeichnen von Fehlern.

### <a name="diagnostic-configuration"></a>Diagnose Konfiguration

Analyzers können mithilfe einer XML-Datei konfiguriert werden, die es Ihnen ermöglicht, den Standard Schweregrad zu ändern, bestimmte Dateien zu ignorieren und Variablen zu übergeben.

Sie können eine Baselineversion verwenden, wenn Sie über eine Reihe von Konfigurationen verfügen, die Sie für mehrere Android-Apps freigeben möchten. Um dieses Feature zu verwenden, erstellen Sie eine neue Konfigurationsdatei, und fügen Sie `-baseline` an den Dateinamen an. Zuerst werden die Basis Linien Konfigurationen und dann die restlichen Konfigurationsdateien angewendet.

> [!TIP]
> Dies kann hilfreich sein, wenn Sie eine Reihe von Problemen in einer neuen oder vorhandenen Android-App ignorieren möchten.

Das Format lautet:

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<configuration>
    <issue id="DuplicateIDs" severity="warning">
        <ignore path="Resources/layout/layout1.xml" />
    </issue>
    <issue id="HardcodedText" severity="informational">
        <ignore path="Resources/layout/layout1.xml" />
        <ignore path="Resource/layout/layout2.xml" />
    </issue>
    <issue id="TooManyViews">
        <variable name="MAX_VIEW_COUNT" value="12" />
    </issue>
    <issue id="TooDeepLayout">
        <variable name="MAX_DEPTH" value="12" />
    </issue>
</configuration>
```

> [!NOTE]
> Zurzeit sind die einzigen Variablen `MAX_VIEW_COUNT` (Standard: 80) und `MAX_DEPTH` (Standardwert: 10) für `TooManyViews` und `TooDeepLayout`.

Die Schweregrade lauten:

- Vorschlag
- Info
- Warnung
- Fehler
- Ignorieren

### <a name="add-a-configuration-file"></a>Hinzufügen einer Konfigurationsdatei

Erstellen Sie eine neue XML-Datei im Stammverzeichnis eines Android-App-Projekts. Der Name der Datei ist nicht wichtig, aber in diesem Beispiel wird `AndroidLayoutDiagnostics.xml`verwendet:

![Neues Element hinzufügen](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

Nachdem die neue XML-Datei hinzugefügt wurde, sollte Sie in der Android-App-Projektstruktur angezeigt werden:

![Android-App-Projektstruktur](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

Stellen Sie sicher, dass die Buildaktion im Eigenschaften Panel auf **androidresourceanalysisconfig** festgelegt ist.
Die einfachste Möglichkeit zum Abrufen des Eigenschaften Panels für die neue Datei besteht darin, mit der rechten Maustaste auf die Datei zu klicken und Eigenschaften auszuwählen. Nachdem der Eigenschaften Bereich angezeigt wird, müssen Sie die **Buildaktion** in **androidresourceanalysisconfig**ändern:

![Buildaktion in Element Eigenschaften festlegen](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

Nachdem Sie nun über eine leere XML-Datei verfügen, müssen Sie das `<configuration>` root-Element hinzufügen. An diesem Punkt können Sie das Standardverhalten aller unterstützten Probleme anpassen.
Wenn Sie sicherstellen möchten, dass hart codierte Zeichen folgen als Fehler behandelt werden, fügen Sie Folgendes hinzu:

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![Diagnose Konfigurationsdatei](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

Da der hart codierte Text nun als Fehler betrachtet wird, wird er nun mit einer roten Wellenlinie im Layout-Editor gekennzeichnet:

![Layout mithilfe der Diagnose Konfiguration](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> Damit neue Änderungen an der Konfigurationsdatei wirksam werden, müssen alle momentan geöffneten Layoutdateien erneut geöffnet werden.
>

## <a name="troubleshooting"></a>Problembehandlung

Im folgenden finden Sie einige mögliche häufige Probleme.

- Stellen Sie sicher, dass kein XML-Format Fehler vorliegt.
- Die Buildaktion ist auf " **androidresourceanalysisconfig**" ordnungsgemäß festgelegt.

## <a name="known-issues"></a>Bekannte Probleme

- Das fehlerpad wird erst aufgefüllt, wenn die Datei zum ersten Mal geändert wird.

## <a name="related-links"></a>Verwandte Links

- [Android-lint-Prüfungen](http://tools.android.com/tips/lint-checks)
- [Verbessern Sie Ihren Code mit lint-Überprüfungen](https://developer.android.com/studio/write/lint)
