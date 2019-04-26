---
title: Einbetten von .NET Fehler
description: Dieses Dokument beschreibt die Fehler, die durch das Einbetten von .NET generiert. Fehler werden aufgelistet, die von Code und erhalten eine Beschreibung an, die Problembehandlung erleichtern.
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: lobrien
ms.author: laobri
ms.date: 04/11/2018
ms.openlocfilehash: 5c3dd406f1132f51a86ddf574ab7ad0b279bc9ec
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61215340"
---
# <a name="net-embedding-errors"></a>Einbetten von .NET Fehler

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: Binden von Fehlermeldungen

Beispiel: Parameter, der Umgebung

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: Unerwarteter Fehler: Geben Sie bitte einen Fehlerbericht an https://github.com/mono/Embeddinator-4000/issues

Ein unerwarteter Fehler ist aufgetreten. Bitte [ein Problem](https://github.com/mono/Embeddinator-4000/issues) so viele Informationen wie möglich, einschließlich:

* Vollständiger Build erstellt Protokolle mit maximaler Ausführlichkeit
* Eine minimale Testfall, der den Fehler reproduzieren
* Alle Informationen für version

Die einfachste Möglichkeit zum Abrufen von Informationen zur genauen Version ist die Verwendung der **Xamarin Studio** Menü **Info zu Xamarin Studio** Element **Details anzeigen** Schaltfläche und die Version, kopieren und einfügen Informationen (Sie können die **Informationen kopieren** Schaltfläche).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: Ausgabeverzeichnis konnte nicht erstellt werden. `X`

Der Verzeichnisname gemäß `-o=DIR` ist nicht vorhanden und konnte nicht erstellt werden. Ein ungültiger Name für das Dateisystem möglicherweise.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: Option `X` wird nicht unterstützt

Das Tool unterstützt nicht die Option `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder, dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: Die Plattform `X` ist ungültig.

Das Tool unterstützt die Plattform nicht `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder, dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: Das Ziel `X` ist ungültig.

Das Tool unterstützt nicht das Ziel `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder, dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: Kompilierungszielparameter `X` ist ungültig.

Das Tool unterstützt keine kompilierungszielparameter `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder, dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Den Xcode-Speicherort wurde nicht gefunden werden.

Das Tool wurde nicht gefunden, die aktuell ausgewählten Xcode-Speicherort, mit der `xcode-select -p` Befehl. Stellen Sie sicher, dass dieser Befehl ist erfolgreich, und gibt den richtigen Xcode-Speicherort zurück.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: Die Sdk-Version konnte für "{Sdk}" nicht abgerufen werden.

Das Tool konnte nicht abgerufen werden die SDK-Version mithilfe der `xcrun --show-sdk-version --sdk {sdk}` Befehl. Stellen Sie sicher, dass dieser Befehl ist erfolgreich, und die SDK-Version zurückgegeben.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: Die Architektur "{Arch}" ist ungültig für {}-Plattform. Gültige für {}-Plattform-Architekturen sind: "{Architekturen}".

Die Architektur in der Fehlermeldung gilt nicht für die Zielplattform. Stellen Sie sicher, dass die Option--Abi gültige Architektur übergeben wird.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: Das Feature `X` ist zurzeit nicht vom Generator implementiert

Dies ist ein bekanntes Problem, das wir in einer zukünftigen Version des Generators beheben möchten. Beiträge sind Willkommen.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: Die Frameworks '{SimulatorFramework}' und '{DeviceFramework}' können nicht zusammengeführt werden, da die Datei "{File}" in beiden vorhanden ist.

Das Tool konnten nicht die Frameworks, die in der Fehlermeldung genannten zusammengeführt, da eine allgemeine Datei zwischen ihnen vorhanden ist.

Dies weist möglicherweise einen Fehler in das Einbetten von .NET hin; Melden Sie bitte einen Fehlerbericht an [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) zu einem Testfall.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: Die Assembly `X` ist nicht vorhanden.

Das Tool die Assembly wurde nicht gefunden `X` in den Argumenten angegeben.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: Der Name der Assembly `X` ist nicht eindeutig.

Mehr als eine Assembly, die angegeben haben, der interne Name derselben, und es wäre nicht in der zur Unterscheidung zwischen ihnen zur Laufzeit möglich.

Die wahrscheinlichste Ursache ist, dass eine Assembly auf die Befehlszeilenargumente mehr als einmal angegeben wird. Wird jedoch gleichzeitig eine noch umbenannt Assembly-hält, kann der ursprüngliche Name und mehrere Kopien nicht möglich.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: Die Assembly 'X', 'Y' verweist, wurde nicht gefunden.

Das Tool wurde die Assembly 'X', auf die verwiesen wird durch die Assembly 'Y' nicht gefunden werden. Stellen Sie sicher, dass alle referenzierten Assemblys im gleichen Verzeichnis wie die Assembly gebunden werden soll.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: {Product} wurde nicht gefunden ({Product} {Min_version} erforderlich ist).

Die Abhängigkeit, die in der Fehlermeldung genannten konnte auf dem System nicht gefunden werden.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: Eine gültige Version von {Product} wurde nicht gefunden ({Version}, gefunden jedoch mindestens {Min_version} ist erforderlich).

Die Abhängigkeit, die im Fehler genannt werden, Nachricht wurde auf dem System gefunden, aber es ist zu alt. Aktualisieren Sie auf eine neuere Version.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: "Symlink" konnte nicht erstellt werden "{file}" -> "{target}": Fehler beim {Number}

Die symbolische Verknüpfung, die in der Fehlermeldung genannten konnte nicht erstellt werden.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>Das Befehlszeilenargument 'A' EM0026 konnte nicht analysiert werden: *

Die Syntax für die die Befehlszeilenoption folgender Option `A` konnte nicht vom Tool analysiert werden. Es ist wahrscheinlich falsch ist, überprüfen Sie mit der Dokumentation oder der Hilfe für die richtige Syntax.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: Interner Fehler *. Melden Sie bitte einen Fehlerbericht zu einem Testfall (https://github.com/mono/Embeddinator-4000/issues).

Diese Fehlermeldung wird gemeldet, wenn es sich bei interner konsistenzprüfung im Einbetten von .NET ein Fehler auftritt.

Gibt einen Fehler in das Einbetten von .NET. Melden Sie bitte einen Fehlerbericht an [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) zu einem Testfall.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: Verarbeitung der Code

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: Typ `T` wird nicht generiert werden, da `X` werden nicht unterstützt.

Dies ist eine **Warnung** , die den Typ `T` ignoriert werden (d. h. "nothing" generiert werden), da er verwendet `X`, ein Feature, das nicht unterstützt wird.

Hinweis: Unterstützte Funktionen werden mit neuen Versionen des Tools entwickeln.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: Typ `T` wird nicht generiert werden, weil Marshallingcode mit einem systemeigenen Gegenstück fehlt.

Dies ist eine **Warnung** , die den Typ `T` ignoriert werden (d. h. "nothing" generiert.) da es etwas von .NET Framework verfügbar macht, die zusätzliche Marshalling erforderlich sind.

Hinweis: Dies ist etwas, das unterstützt werden kann, mit einigen Einschränkungen in einer zukünftigen Version des Tools.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: Konstruktor `C` wird nicht generiert werden, aufgrund der Parametertyp `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die den Konstruktor `C` werden ignoriert (d. h. "nothing" generiert.) da einen Parameter vom Typ `T` wird nicht unterstützt.

Geben Sie eine Warnung zu eine früheren mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden mit neuen Versionen des Tools entwickeln.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: Konstruktor `C` verfügt über Standardwerte für die kein Wrapper generiert wird.

Dies ist eine **Warnung** , die Standardparameter des Konstruktors `C` keine zusätzlichen Code generieren. Die häufigste Ursache ist, dass eine vorhandene Methode bereits die gleiche Signatur aufweist. Beispiel: in .net ist es möglich, dass:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

In solchen Fällen, die nur zwei generiert `init` Selektoren erstellt werden, beide Aufruf in Mono, jedoch kein Wrapper für das spätere bestehen.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: Methode `M` wird nicht generiert werden, da Rückgabetyp `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die die Methode `M` werden ignoriert (d. h. "nothing" generiert werden), da es sich um Rückgabetyp ist `T` wird nicht unterstützt.

Geben Sie eine Warnung zu eine früheren mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden mit neuen Versionen des Tools entwickeln.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: Methode `M` wird nicht generiert werden, aufgrund der Parametertyp `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die die Methode `M` werden ignoriert (d. h. "nothing" generiert.) da einen Parameter vom Typ `T` wird nicht unterstützt.

Geben Sie eine Warnung zu eine früheren mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden mit neuen Versionen des Tools entwickeln.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: Methode `M` verfügt über Standardwerte für die kein Wrapper generiert wird.

Dies ist eine **Warnung** , die die Standardparameter der Methode `M` keine zusätzlichen Code generieren. Die häufigste Ursache ist, dass eine vorhandene Methode bereits die gleiche Signatur aufweist. Beispiel: in .net ist es möglich, dass:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

In solchen Fällen, die nur zwei generiert `increment` Selektoren erstellt werden, beide Aufruf in Mono, jedoch kein Wrapper für das spätere bestehen.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: Methode `M` wird nicht generiert werden, da eine andere Methode den Operator mit einem Anzeigenamen verfügbar macht.

Dies ist eine **Warnung** , die die Methode `M` wird nicht generiert werden, da eine andere Methode den Operator mit einem Anzeigenamen verfügbar macht. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: Erweiterungsmethode `M` generiert, da sie für primitiven Typ erstellt werden können nicht innerhalb einer Kategorie `T`. Eine normale statische Methode wurde generiert.

Dies ist eine **Warnung** , dass eine Erweiterungsmethode für eine Primivite eingeben (z. B. `System.Int32`) wurde gefunden. In Objective-C ist es nicht möglich, so erstellen Sie Kategorien für primitive-Typ. Stattdessen erstellt der werden Generator einer normale statische Methode.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: Eigenschaft `P` wird nicht generiert werden, aufgrund der Parametertyp `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die die Eigenschaft `P` werden ignoriert (d. h. "nothing" generiert.) da den verfügbar gemachten Typ `T` wird nicht unterstützt.

Geben Sie eine Warnung zu eine früheren mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden mit neuen Versionen des Tools entwickeln.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: Indizierte Eigenschaften auf `T` wird nicht generiert werden, da mehrere indizierte Eigenschaften nicht unterstützt werden.

Dies ist eine **Warnung** , die die indizierten Eigenschaften `T` ignoriert werden (d. h. "nothing" generiert.) da mehrere indizierte Eigenschaften nicht unterstützt werden.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: Feld `F` wird aufgrund der Feldtyp nicht generiert `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die das Feld `F` werden ignoriert (d. h. "nothing" generiert.) da den verfügbar gemachten Typ `T` wird nicht unterstützt.

Geben Sie eine Warnung zu eine früheren mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden mit neuen Versionen des Tools entwickeln.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: Element `E` generiert stattdessen als `F` , da dessen Name mit einer wichtigen Objective-c-Selektor in Konflikt steht.

Dies ist eine **Warnung** , die das Element `E` generiert werden stattdessen als `F` , da dessen Name mit einer wichtigen Objective-c-Selektor in Konflikt steht.

Selektoren auf die [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) haben wichtige Bedeutung in Objective-c und muss sorgfältig überschrieben werden.

Hinweis: Die Liste der reservierten Selektoren wird mit neuen Versionen des Tools entwickeln.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: Element `E` wird nicht generiert, der Namen steht in Konflikt mit anderen Elementen auf der gleichen Klasse.

Dies ist eine **Warnung** dieses Element `E` wird nicht generiert werden, wie Sie der Namen in Konflikt mit anderen Elementen in derselben Klasse steht.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Ziel `E` wird für Xamarin.iOS- und Xamarin.Mac nicht unterstützt. Nur die `framework` Option wird berücksichtigt, unterstützt und sollte verwendet werden.

Dies ist eine **Warnung** , die auf `E` gilt als nicht unterstützt, für die Xamarin.iOS- und Xamarin.Mac-Anwendungsfälle. 

Nutzung von statischen oder dynamischen Einbetten von .NET Bibliotheken möglicherweise zusätzliche Arbeitsschritte oder Anpassungen erforderlich, und sollte in den meisten Fällen vermieden werden.

Entfernen Sie ggf. Ihre `--target` Parameter oder Pass `--target=framework` stattdessen.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: Codeerzeugung

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
