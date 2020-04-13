---
title: Android Layout-Diagnose
description: Erläutert die Android-Layoutdiagnose und den Einstieg
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 746f74e68fa4816f1f7979980af9506dc0173542
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987581"
---
# <a name="android-layout-diagnostics"></a>Android-Layout-Diagnose

Android-Layout-Diagnose wurde entwickelt, um die Qualität von Android-Layout-Dateien zu verbessern, indem allgemeine Qualitätsprobleme und hilfreiche Optimierungen hervorgehoben werden. Diese Funktion ist sowohl für Visual Studio 16.5+ als auch für Visual Studio für Mac 8.5+ verfügbar.

Für eine Vielzahl von Problemen wird ein Standardsatz von Analysatoren bereitgestellt, der an die spezifischen Anforderungen eines Projekts angepasst werden kann. Die Analysatoren basieren lose auf dem Android-Linting-System.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>Android-Layoutdiagnose in Visual Studio 2019 aktivieren

Stellen Sie sicher, dass die Einstellung für die Layoutdiagnose aktiviert ist, **Layoutdiagnose aktivieren.** Um auf diese Optionsseite zuzugreifen, wählen Sie > **Extraoptionen**aus, und wählen Sie dann **Texteditor** > **Android XML** >  **Tools****Advanced**:

![Dialogmitenfeld Optionen zum Aktivieren der Diagnoseoption](diagnostics-images/AndroidDiagnosticsEnableOption.png)

Sobald der Android-Layout-Editor aktiviert ist, werden folgende Probleme angezeigt:

![Android-Diagnose in Visual Studio 2019 aktiviert](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>Android-Layoutdiagnose auf Visual Studio für Mac aktivieren

Stellen Sie sicher, dass die Einstellung für die Layoutdiagnose aktiviert ist, **Layoutdiagnose aktivieren.** Um auf diese Optionsseite zuzugreifen, wählen Sie Visual > **Studio-Einstellungen...** aus, und wählen Sie dann **Texteditor** > **Android XML**: **Visual Studio**

![Dialog mit den Einstellungen, in dem die Diagnoseoption aktiviert wird](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

Sobald der Android-Layout-Editor aktiviert ist, werden folgende Probleme angezeigt:

![Android-Diagnose in Visual Studio für Mac aktiviert](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>Features

In den folgenden Abschnitten werden die verfügbaren Funktionen in der Android-Layoutdiagnose beschrieben.

### <a name="analyzers"></a>Analyzer

Analyzer werden verwendet, um Probleme in Layoutdateien zu erkennen, hartcodierte Werte zu reduzieren, die Leistung zu verbessern und Fehler zu kennzeichnen. Eine Liste der Analysatoren finden Sie unter [Android Designer-Diagnoseanalysatoren](diagnostic-analyzers.md)

### <a name="diagnostic-configuration"></a>Diagnosekonfiguration

Analyzer können mithilfe einer XML-Datei konfiguriert werden, sodass Sie den Standardschweregrad ändern, bestimmte Dateien ignorieren und Variablen übergeben können.

Sie können eine Basisdatei verwenden, wenn Sie über eine Reihe von Konfigurationen verfügen, die Sie für mehrere Android-Apps freigeben möchten. Um diese Funktion zu verwenden, erstellen `-baseline` Sie eine neue Konfigurationsdatei und fügen Sie den Dateinamen an. Die Basiskonfigurationen werden zuerst und dann die verbleibenden Konfigurationsdateien angewendet.

> [!TIP]
> Dies kann nützlich sein, wenn Sie eine Reihe von Problemen in einer neuen oder vorhandenen Android-App ignorieren möchten.

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
> Derzeit `MAX_VIEW_COUNT` sind die einzigen Variablen (Standard: 80) `MAX_DEPTH` und `TooManyViews` `TooDeepLayout` (Standard: 10) für bzw..

Die Schweregrade lauten:

- Vorschlag
- Info
- Warnung
- Fehler
- Ignorieren

### <a name="add-a-configuration-file"></a>Hinzufügen einer Konfigurationsdatei

Erstellen Sie eine neue XML-Datei im Stammverzeichnis eines Android-App-Projekts. Der Name der Datei ist nicht wichtig, `AndroidLayoutDiagnostics.xml`aber in diesem Beispiel wird verwendet:

![Neues Element hinzufügen](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

Sobald die neue XML-Datei hinzugefügt wurde, sollte sie in der Android-App-Projektstruktur angezeigt werden:

![Android App Projektbaum](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

Stellen Sie sicher, dass die Buildaktion im Eigenschaftenbedienfeld auf **AndroidResourceAnalysisConfig** festgelegt ist.
Die einfachste Möglichkeit, das Eigenschaftenfenster für die neue Datei hochzuziehen, besteht darin, mit der rechten Maustaste auf die Datei zu klicken und Eigenschaften auszuwählen. Sobald das Eigenschaftenbedienfeld angezeigt wird, sollten Sie die **Buildaktion** in **AndroidResourceAnalysisConfig**ändern:

![Festlegen der Buildaktion in Elementeigenschaften](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

Nachdem Sie nun über eine leere XML-Datei verfügen, müssen Sie das `<configuration>` Stammelement hinzufügen. An diesem Punkt können Sie das Standardverhalten aller unterstützten Probleme anpassen.
Wenn Sie sicherstellen möchten, dass hartcodierte Zeichenfolgen als Fehler behandelt werden, fügen Sie hinzu:

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![Diagnosekonfigurationsdatei](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

Da hartcodierter Text als Fehler betrachtet wird, wird er nun im Layout-Editor mit einem roten Wellenschalter gekennzeichnet:

![Layout mit Diagnosekonfiguration](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> Damit neue Konfigurationsdateiänderungen wirksam werden, müssen alle derzeit geöffneten Layoutdateien erneut geöffnet werden.
>

## <a name="troubleshooting"></a>Problembehandlung

Hier sind einige mögliche häufige Probleme.

- Stellen Sie sicher, dass kein XML-Formatfehler vorliegt.
- Die Buildaktion ist korrekt auf **AndroidResourceAnalysisConfig**festgelegt.

## <a name="known-issues"></a>Bekannte Probleme

- Das Fehlerfeld wird erst aufgefüllt, nachdem die Datei zum ersten Mal geändert wurde.

## <a name="related-links"></a>Verwandte Links

- [Android Lint-Prüfungen](http://tools.android.com/tips/lint-checks)
- [Verbessern Sie Ihren Code mit Lint-Checks](https://developer.android.com/studio/write/lint)
