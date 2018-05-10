---
title: .NET Einbetten von Fehlern
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: topgenorth
ms.author: toopge
ms.date: 04/11/2018
ms.openlocfilehash: fcfeaa2d98a28723f95a9bf417e4bed81fe0dec3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="net-embedding-errors"></a>Einbetten von Fehlern .NET

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: Binden von Fehlermeldungen

Beispiel: Parameter, Umgebung

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: Unerwarteter Fehler: Bitte geben Sie einen Fehlerbericht an https://github.com/mono/Embeddinator-4000/issues

Ein unerwarteter Fehler aufgetreten ist. Bitte [eine](https://github.com/mono/Embeddinator-4000/issues) durch so viele Informationen wie möglich, einschließlich:

* Vollständige Buildprotokolle, mit maximale Ausführlichkeit
* Eine minimale Testfall, der den Fehler reproduzieren
* Alle Informationen der version

Die einfachste Möglichkeit zum Abrufen von Informationen zur exakten Version ist die Verwendung der **Xamarin Studio** Menü **über Xamarin Studio** Element **Details anzeigen** Schaltfläche und die Version, kopieren und einfügen Informationen (Sie können die **Informationen kopieren** Schaltfläche).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: Ausgabeverzeichnis konnte nicht erstellt werden. `X`

Der Verzeichnisname angegeben `-o=DIR` ist nicht vorhanden und konnte nicht erstellt werden. Er möglicherweise einen ungültigen Namen für das Dateisystem.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: Option `X` wird nicht unterstützt

Das Tool unterstützt nicht die Option `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: Die Plattform `X` ist ungültig.

Das Tool unterstützt die Plattform nicht `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: Das Ziel `X` ist ungültig.

Das Tool unterstützt nicht das Ziel `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: Kompilierungsziel `X` ist ungültig.

Das Tool unterstützt nicht das Kompilierungsziel `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Den Speicherort Xcode nicht gefunden.

Das Tool konnte nicht gefunden werden die aktuell ausgewählten Xcode Speicherort mit der `xcode-select -p` Befehl. Stellen Sie sicher, dass dieser Befehl ist erfolgreich, und gibt den Speicherort der richtigen Xcode zurück.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: Die Sdk-Version für "{Sdk}" konnte nicht abgerufen werden.

Das Tool konnte nicht abgerufen werden die SDK-Version mithilfe der `xcrun --show-sdk-version --sdk {sdk}` Befehl. Stellen Sie sicher, dass dieser Befehl ist erfolgreich, und die SDK-Version zurückgegeben.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: Die Architektur "{Arch}" ist ungültig für {Plattform}. Gültige Architekturen für {Plattform} sind: "{Architekturen}".

Die Architektur in der Fehlermeldung angegebene gilt nicht für die Zielplattform. Stellen Sie sicher, dass die Option – Abi gültige Architektur übergeben wird.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: Das Feature `X` ist derzeit nicht vom Generator implementiert

Dies ist ein bekanntes Problem, das wir in einer zukünftigen Version des Generators beheben möchten. Beiträge sind Willkommen.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: Nicht die Frameworks {SimulatorFramework} und {DeviceFramework} zusammengeführt, da die Datei "{}" in beiden vorhanden ist.

Das Tool konnte die Frameworks, die in der Fehlermeldung genannten nicht zusammenführen, weil eine gemeinsame Datei zwischen ihnen vorhanden ist.

Dies weist möglicherweise einen Fehler im .NET einbetten hin; Bitte der Datei eines Fehlerberichts an [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) zu einem Testfall.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: Die Assembly `X` ist nicht vorhanden.

Das Tool die Assembly wurde nicht gefunden `X` in den Argumenten angegeben.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: Der Name der Assembly `X` ist nicht eindeutig.

Mehr als eine Assembly angegeben haben, den gleichen, internen Namen, und es nicht möglich wäre, zur Laufzeit unterschieden.

Die wahrscheinlichste Ursache ist, dass eine Assembly auf die Befehlszeilenargumente mehr als einmal angegeben wird. Wird jedoch gleichzeitig einen behält der umbenannten Assembly noch wird der ursprüngliche Name und mehrere Kopien nicht möglich.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: Findet nicht die Assembly 'X', 'Y' verweist.

Das Tool wurde die Assembly 'X', auf die verwiesen wird durch die Assembly 'Y' nicht gefunden werden. Stellen Sie sicher, dass alle referenzierten Assemblys im gleichen Verzeichnis wie die Assembly gebunden werden.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: Wurde nicht gefunden {Produkt} ({Produkt} {Min_version} erforderlich ist).

Die Abhängigkeit, die in der Fehlermeldung genannten konnte auf dem System nicht gefunden werden.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: Eine gültige Version {Produkts} nicht gefunden ({Version}, gefunden jedoch mindestens {Min_version} ist erforderlich).

Die Abhängigkeit, die im Fehler genannt werden, Nachricht wurde auf dem System gefunden, aber es ist zu alt. Aktualisieren Sie auf eine neuere Version.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: Konnte nicht erstellt werden "Symlink" "{file}" -> "{target}": Fehler {Number}

Die in der Fehlermeldung genannten "Symlink" konnte nicht erstellt werden.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>Das Befehlszeilenargument "A" EM0026 konnte nicht analysiert werden: *

Die Syntax für die Befehlszeilenoption `A` konnte nicht vom Tool analysiert werden. Es ist wahrscheinlich falsch ist, wenden Sie sich mit der Dokumentation oder der Hilfe für die richtige Syntax.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: Interner Fehler *. Bitte der Datei eines Fehlerberichts zu einem Testfall (https://github.com/mono/Embeddinator-4000/issues).

Diese Fehlermeldung wird gemeldet, wenn es sich bei eine internen konsistenzüberprüfung in .NET einbetten schlägt fehl.

Gibt einen Fehler in .NET einbetten. Bitte der Datei eines Fehlerberichts an [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) zu einem Testfall.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: Code-Verarbeitung

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: Geben Sie `T` wird nicht generiert werden, da `X` werden nicht unterstützt.

Dies ist eine **Warnung** , den Typ `T` werden ignoriert (d. h. keine generiert werden), da er verwendet `X`, eine Funktion, die nicht unterstützt wird.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: Geben Sie `T` wird nicht generiert werden, weil sie nicht mit einem systemeigenen Gegenstück Marshallingcode enthält.

Dies ist eine **Warnung** , die den Typ `T` werden ignoriert (d. h. keine generiert werden), da es etwas von .NET Framework verfügbar macht, die zusätzliche Marshalling erfordert.

Hinweis: Dies ist etwas, das unterstützt abrufen kann, mit einigen Einschränkungen in einer zukünftigen Version des Tools.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: Konstruktor `C` wird nicht aufgrund der Parametertyp generiert `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die der Konstruktor `C` werden ignoriert (d. h. keine generiert werden) da einen Parameter vom Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: Konstruktor `C` Standardwerte für die kein Wrapper generiert wurde.

Dies ist eine **Warnung** , die Standardparameter Konstruktors `C` keine zusätzlichen Code generieren. Die häufigste Ursache ist, dass eine vorhandene Methode bereits die gleiche Signatur verfügt. Beispiel: in .net ist es möglich, dass:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

In solchen Fällen nur zwei generiert `init` Selektoren erstellt werden, beide aufrufen Mono, aber kein Wrapper für die spätere vorhanden.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: Methode `M` generiert, da kein Rückgabetyp `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die Methode `M` werden ignoriert (d. h. keine generiert werden), da es Rückgabetyp ist `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: Methode `M` wird nicht aufgrund der Parametertyp generiert `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die die Methode `M` werden ignoriert (d. h. keine generiert werden) da einen Parameter vom Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: Methode `M` Standardwerte für die kein Wrapper generiert wurde.

Dies ist eine **Warnung** , die Standardparameter Methode `M` keine zusätzlichen Code generieren. Die häufigste Ursache ist, dass eine vorhandene Methode bereits die gleiche Signatur verfügt. Beispiel: in .net ist es möglich, dass:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

In solchen Fällen nur zwei generiert `increment` Selektoren erstellt werden, beide aufrufen Mono, aber kein Wrapper für die spätere vorhanden.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: Methode `M` wird nicht generiert werden, da eine andere Methode den Operator mit einem Anzeigenamen verfügbar macht.

Dies ist eine **Warnung** , die die Methode `M` wird nicht generiert werden, da eine andere Methode den Operator mit einem Anzeigenamen verfügbar macht. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: Erweiterungsmethode `M` generiert, da sie für den primitiven Typ erstellt werden, können nicht innerhalb einer Kategorie `T`. Eine normale, statische Methode generiert wurde.

Dies ist eine **Warnung** , dass eine Erweiterungsmethode für einen Primivite eingeben (z. B. `System.Int32`) wurde gefunden. In Objective-C ist es nicht möglich, um Kategorien für den primitiven Typ zu erstellen. Stattdessen werden der Generator erzeugt eine normale, statische Methode.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: Eigenschaft `P` wird nicht aufgrund der Parametertyp generiert `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die die Eigenschaft `P` werden ignoriert (d. h. keine generiert werden) da den verfügbar gemachten Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: Indizierte Eigenschaften auf `T` wird nicht generiert werden, da mehrere indizierte Eigenschaften nicht unterstützt werden.

Dies ist eine **Warnung** , die indizierte Eigenschaften auf `T` werden ignoriert (d. h. keine generiert werden), weil mehrere indizierte Eigenschaften nicht unterstützt werden.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: Feld `F` wird nicht aufgrund der Feldtyp generiert `T` wird nicht unterstützt.

Dies ist eine **Warnung** , die das Feld `F` werden ignoriert (d. h. keine generiert werden) da den verfügbar gemachten Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: Element `E` generiert stattdessen als `F` weil dessen Name mit einer wichtigen Objective-c-Selektor in Konflikt steht.

Dies ist eine **Warnung** , die das Element `E` generiert werden stattdessen als `F` weil dessen Name mit einer wichtigen Objective-c-Selektor in Konflikt steht.

Selektoren auf die [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) haben wichtige Bedeutung in Objective-c und muss sorgfältig überschrieben werden.

Hinweis: Die Liste der reservierten Selektoren wird mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: Element `E` wird nicht generiert, dessen Name steht in Konflikt mit anderen Elementen in der gleichen Klasse.

Dies ist eine **Warnung** dieses Element `E` wird nicht generiert werden, weil dessen Name mit anderen Elementen in der gleichen Klasse steht in Konflikt.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Ziel `E` wird für Xamarin.iOS und Xamarin.Mac nicht unterstützt. Nur die `framework` Option gilt unterstützt und sollte verwendet werden.

Dies ist eine **Warnung** , die auf `E` wird nicht unterstützt für Xamarin.iOS und Xamarin.Mac Anwendungsfällen betrachtet. 

Verbrauch von statischen oder dynamischen .NET Einbetten von Bibliotheken erfordern möglicherweise zusätzliche Arbeitsschritte oder sein und sollte vermieden werden, in den meisten Fällen zu verwenden.

Entfernen Sie die `--target` Parameter "oder" Pass `--target=framework` stattdessen.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: Codegenerierung

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
