---
title: Android Layout Diagnostics Analyzer
description: Dieses Handbuch listet alle derzeit unterstützten Android-Layout-Diagnose-Analysatoren auf Xamarin.Android auf.
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 04/07/2020
ms.openlocfilehash: 6203ce444bb97fa453a912a724f7f5724558b32a
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987617"
---
# <a name="android-designer-diagnostic-analyzers"></a>Android-Designer-Diagnoseanalysatoren

In diesem Handbuch werden alle derzeit unterstützten Android-Layoutdiagnoseanalysatoren aufgeführt.

## <a name="accessibility"></a>Zugriff

Die folgenden Analysatoren tragen zur Verbesserung der Barrierefreiheitsunterstützung bei:

| id | Titel | severity | BESCHREIBUNG |
|----|-------|----------|-------------|
| ContentDescription | Bild ohne`contentDescription` | Warnung | Fehlendes `contentDescription` Attribut auf dem Bild |

## <a name="correctness"></a>Richtigkeit

Die folgenden Analysatoren helfen bei der Behebung von Korrekturproblemen in einem Layout:

| id | Titel | severity | BESCHREIBUNG | Hilfe |
|----|-------|----------|-------------|------|
| AdapterViewChildren | AdapterView mit Kindern | Warnung | AdapterViews kann keine untergeordneten Elemente in XML haben | [Link](xref:Android.Widget.AdapterView) |
| MissingId | Fragmente sollten `id` eine oder`tag` | Warnung |Dieses `<fragment>` Tag sollte `id` einen `tag` oder ein angeben, um den Status über Aktivitätsneustarts hinweg beizubehalten. | [Link](xref:Android.App.Fragment) |
| NestedScrollingVertical | Verschachtelte vertikal scrollende Elemente | Warnung | Verschachtelte Scrolling-Widgets |
| NestedScrollingHorizontal | Verschachtelte horizontale Bildlaufelemente | Warnung | Verschachtelte Scrolling-Widgets |
| ScrollViewSize | ScrollView Kinder mit falschen fill_parent/match_parent Größen | Warnung | ScrollView Kinder mit falschen fill_parent/match_parent Größen |
| ScrollViewCount | ScrollViews kann nur ein untergeordnetes Element haben | Warnung | Eine Bildlaufansicht kann nur ein untergeordnetes Element haben |
| FehlendeAndroidNamespace | Fehlender Android-Namespace für Attribut | Fehler | Fehlende Android XML-Namespace; Ihr Attribut wird als benutzerdefiniertes Attribut interpretiert |
| DuplikatiDs | Duplikat-IDs | Fehler | Duplikat-IDs innerhalb eines einzelnen Layouts |
| IncludeLayoutParamsMissingWidthAndHeight | Fehlende Breite und Höhe | Fehler | Ignorierte Layoutparams auf include | [Link](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| IncludeLayoutParamsMissingWidth | Fehlende Breite | Fehler | Ignorierte Layoutparams auf include | [Link](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| IncludeLayoutParamsMissingHeight | Fehlende Höhe | Fehler | Ignorierte Layoutparams auf include | [Link](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| Ausrichtung | Fehlende explizite Ausrichtung | Fehler | Fehlende explizite Ausrichtung |
| Suspicious0dp | Verdächtige 0dp-Dimension | Fehler | Verdächtige 0dp-Dimension |
| RequiredSizeWidth | Fehlendes Breitenattribut | Fehler | Fehlendes Attribut: layout_width |
| RequiredSizeHeight | Fehlendes Höhenattribut | Fehler | Fehlendes Attribut: layout_height |
| WebViewLayout | WebViews in wrap_content Eltern | Fehler |
| WrongCase | Falscher Fall für View-Tag | Fehler | Falscher Fall für View-Tag | [Link](xref:Android.App.Fragment) |

## <a name="design"></a>Entwurf

Die folgenden Analysatoren helfen Ihnen, die Verknüpfung von Layoutdateien zu verbessern:

| id | Titel | severity | BESCHREIBUNG |
|----|-------|----------|-------------|
| HardcodedColor | Hartcodierte Farbe | Info | Hartcodierte Farbe führt oft zu Inkonsistenzen |
| HardcodedSize  | Hardcodierte Größe  | Info | Hartcodierte Größe führt oft zu Inkonsistenzen  |
| HardcodedText  | Hartcodierter Text  | Warnung | Hartcodierter Text |
| UnresolvedResource | Nicht aufgelöste Ressourcen-URL | Warnung | Diese Ressourcen-URL kann nicht aufgelöst werden. |
| XmlErrors | XML-Syntaxfehler | Fehler | XML-Syntaxfehler |

## <a name="performance"></a>Leistung

Die folgenden Analysatoren tragen zur Verbesserung der Leistung Ihres Layouts bei:

| id | Titel | severity | BESCHREIBUNG |
|----|-------|----------|-------------|
| NestedWeights | Verschachtelte Layoutgewichtungen | Warnung | Verschachtelte Gewichte sind schlecht für die Leistung |
| TooManyViews | Layout hat zu viele Ansichten | Warnung | Layout hat zu viele Ansichten |
| TooDeepLayout | Layouthierarchie ist zu tief | Warnung | Layouthierarchie ist zu tief |
| NutzloseEltern | Unnützes übergeordnetes Layout | Warnung | Unnützes übergeordnetes Layout |
| UselessLeaf | Nutzloses Blattlayout | Warnung | Diese `%1$s` Ansicht ist nutzlos (keine `background`Kinder, nein, nein, `id`nein) `style` |

## <a name="usability"></a>Benutzerfreundlichkeit

Die folgenden Analysatoren tragen zur Verbesserung der Layout-Usability für Ihre Kunden bei:

| id | Titel | severity | BESCHREIBUNG |
|----|-------|----------|-------------|
| NegativeMargin | Negative Margen | Warnung | Negative Margen |
| MissingInputType | EditText ohne inputType | Warnung | Kein Eingabetyp angegeben |
| InputTypePhone | EditText scheint eine Telefonnummer zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass `phone` es sich um eine Telefonnummer handelt, die jedoch nicht in der`inputType` |
| InputTypeNumber | EditText scheint eine Zahl zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass es `inputType` sich `numberDecimal`um eine Zahl handelt, die jedoch keine numerische (z. B. ) enthält. |
| InputTypePassword | EditText scheint ein Kennwort zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass `password` es `inputType` sich `textVisiblePassword`um ein Kennwort handelt, das jedoch nicht in die (z. B. ) |
| InputTypePIN | EditText scheint eine PIN zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass es `numberPassword` sich um ein Kennwort (PIN) handelt, das jedoch nicht in die`inputType` |
| InputTypeEmail | EditText scheint eine E-Mail zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass es `email` sich `inputType` um eine `textEmailAddress`E-Mail-Adresse handelt, die jedoch nicht in die (z. B. ) |
| InputTypeURI | EditText scheint ein URI zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass `textUri` es sich um einen URI handelt, der jedoch nicht in die`inputType` |
| InputTypeDate | EditText scheint ein Datum zu sein | Warnung | Der Ansichtsname deutet darauf hin, dass `date` es `inputType` sich `datetime`um ein Datum handelt, das jedoch nicht in die (z. B. ) |
