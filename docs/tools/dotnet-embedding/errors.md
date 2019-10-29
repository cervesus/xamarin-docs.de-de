---
title: .Net-Einbettungs Fehler
description: In diesem Dokument werden die von der .net-Einbettung generierten Fehler beschrieben. Fehler werden nach Code aufgelistet und erhalten eine Beschreibung zur Unterstützung bei der Problembehandlung.
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: b7cdf56ac4d917c8692f33d388adc003fabd9cac
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007243"
---
# <a name="net-embedding-errors"></a>.Net-Einbettungs Fehler

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: Binden von Fehlermeldungen

Die Parameter, Umgebung

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: Unerwarteter Fehler: füllen Sie einen Fehlerbericht unter https://github.com/mono/Embeddinator-4000/issues

Es ist ein unerwarteter Fehler aufgetreten. Bitte melden Sie [ein Problem](https://github.com/mono/Embeddinator-4000/issues) mit so vielen Informationen wie möglich, einschließlich:

* Vollständige Buildprotokolle mit maximaler Ausführlichkeit
* Ein minimaler Testfall, der den Fehler reproduziert.
* Alle Versionsinformationen

Die einfachste Möglichkeit, um genaue Versionsinformationen zu erhalten, ist die Verwendung des Menüs " **Xamarin Studio** ", Informationen zu **Xamarin Studio** Element, Schaltfläche " **Details anzeigen** " und Kopieren/Einfügen der Versionsinformationen (die Schaltfläche " **Informationen kopieren** " können Sie verwenden).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: das Ausgabeverzeichnis konnte nicht erstellt werden `X`

Der durch `-o=DIR` angegebene Verzeichnisname ist nicht vorhanden und konnte nicht erstellt werden. Möglicherweise handelt es sich um einen ungültigen Namen für das Dateisystem.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: die Option `X` wird nicht unterstützt.

Das Tool unterstützt die Option `X`nicht. Möglicherweise wird dies von einer anderen Version des Tools unterstützt, oder es ist nicht in dieser Umgebung anwendbar.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: der `X` der Plattform ist ungültig.

Das Tool unterstützt die Platt Form `X`nicht. Möglicherweise wird dies von einer anderen Version des Tools unterstützt, oder es ist nicht in dieser Umgebung anwendbar.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: der Ziel `X` ist ungültig.

Das Tool unterstützt die Ziel `X`nicht. Möglicherweise wird dies von einer anderen Version des Tools unterstützt, oder es ist nicht in dieser Umgebung anwendbar.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: das `X` für den Kompilierungs Ziel ist ungültig.

Das Tool unterstützt das Kompilierungs Ziel `X`nicht. Möglicherweise wird dies von einer anderen Version des Tools unterstützt, oder es ist nicht in dieser Umgebung anwendbar.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: der Xcode-Speicherort wurde nicht gefunden.

Das Tool konnte den aktuell ausgewählten Xcode-Speicherort nicht mithilfe des `xcode-select -p`-Befehls finden. Überprüfen Sie, ob dieser Befehl erfolgreich ausgeführt wurde, und gibt den korrekten Xcode-Speicherort zurück.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: die SDK-Version für "{SDK}" konnte nicht gefunden werden.

Das Tool konnte die SDK-Version nicht mit dem `xcrun --show-sdk-version --sdk {sdk}` Befehl erhalten. Überprüfen Sie, ob dieser Befehl erfolgreich ausgeführt wurde, und gibt die SDK-Version zurück.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: die Architektur "{Arch}" ist für "{Platform}" nicht gültig. Gültige Architekturen für {Platform} sind: "{Architekturen}".

Die Architektur in der Fehlermeldung ist für die Zielplattform nicht gültig. Stellen Sie sicher, dass der Option--ABI eine gültige Architektur überlassen wird.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: die Funktions `X` wird vom Generator derzeit nicht implementiert.

Dies ist ein bekanntes Problem, das in einer zukünftigen Version des Generators behoben werden soll. Beiträge sind willkommen.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: die Frameworks "{simulatorframework}" und "{deviceframework}" können nicht zusammengeführt werden, da die Datei "{file}" in beiden vorhanden ist.

Das Tool konnte die in der Fehlermeldung erwähnten Frameworks nicht zusammenführen, da zwischen Ihnen eine gemeinsame Datei vorhanden ist.

Dies weist möglicherweise auf einen Fehler in der .net-Einbettung hin. Melden Sie einen Fehlerbericht auf [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) mit einem Testfall.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: die Assembly `X` ist nicht vorhanden.

Das Tool konnte die in den Argumenten angegebene Assembly `X` nicht finden.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: der AssemblyName `X` ist nicht eindeutig.

Mehrere angegebene Assemblys haben denselben internen Namen und können zur Laufzeit nicht unterschieden werden.

Die wahrscheinlichste Ursache ist, dass eine Assembly mehrmals in den Befehlszeilen Argumenten angegeben wird. Allerdings behält eine umbenannte Assembly weiterhin den ursprünglichen Namen bei, und mehrere Kopien können nicht coexistieren.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: die Assembly "X", auf die von "Y" verwiesen wird, kann nicht gefunden werden.

Das Tool konnte die Assembly "X", auf die von der Assembly "Y" verwiesen wird, nicht finden. Stellen Sie sicher, dass sich alle referenzierten Assemblys im gleichen Verzeichnis befinden wie die Assembly, die gebunden werden soll.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-min_version-is-required"></a>EM0014: "{Product}" wurde nicht gefunden ({Product} {min_version} ist erforderlich).

Die in der Fehlermeldung erwähnte Abhängigkeit wurde im System nicht gefunden.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-min_version-is-required"></a>EM0015: Es wurde keine gültige Version von "{Product}" gefunden ("{Version}" wurde gefunden, aber mindestens "{min_version}" ist erforderlich).

Die in der Fehlermeldung erwähnte Abhängigkeit wurde im System gefunden, aber Sie ist zu alt. Aktualisieren Sie auf eine neuere Version.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: der Symlink "{file}" konnte nicht erstellt werden-> "{target}": Fehler {Number}

Der in der Fehlermeldung erwähnte Symlink konnte nicht erstellt werden.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026 konnte das Befehlszeilenargument "A" nicht analysieren: *

Die für die Befehlszeilenoption angegebene Syntax `A` konnte nicht vom Tool analysiert werden. Es ist wahrscheinlich falsch, überprüfen Sie die Dokumentation oder die Hilfe, um die richtige Syntax zu erhalten.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: Interner Fehler *. Melden Sie einen Fehlerbericht mit einem Testfall (https://github.com/mono/Embeddinator-4000/issues).

Diese Fehlermeldung wird angezeigt, wenn bei einer internen Konsistenzprüfung in .net Einbettungs Fehler auftreten.

Dies deutet auf einen Fehler in .net-Einbettung hin. Melden Sie einen Fehlerbericht auf [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) mit einem Testfall.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: Code Verarbeitung

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: der Typ `T` wird nicht generiert, da `X` nicht unterstützt werden.

Dies ist eine **Warnung** , dass der Typ `T` ignoriert wird (d. h. es werden keine Elemente generiert), da er `X`verwendet, eine Funktion, die nicht unterstützt wird.

Hinweis: unterstützte Funktionen werden mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: der Typ `T` wird nicht generiert, da er keinen Marshallingcode mit einem systemeigenen Pendant enthält.

Dies ist eine **Warnung** , dass der Typ `T` ignoriert wird (d. h. es werden keine Elemente generiert), da er etwas von .NET Framework verfügbar macht, das zusätzliches Marshalling erfordert.

Hinweis: Dies kann mit einigen Einschränkungen in einer zukünftigen Version des Tools unterstützt werden.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: der Konstruktor `C` wird nicht generiert, weil der Parametertyp `T` nicht unterstützt wird.

Dies ist eine **Warnung** , dass der Konstruktor `C` ignoriert wird (d. h., es werden keine Elemente generiert), da ein Parameter vom Typ "`T`" nicht unterstützt wird.

Es sollte eine frühere Warnung geben, die weitere Informationen enthält, warum Type `T` nicht unterstützt wird.

Hinweis: unterstützte Funktionen werden mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: der Konstruktor `C` weist Standardwerte auf, für die kein Wrapper generiert wird.

Dies ist eine **Warnung** , dass die Standardparameter des Konstruktors `C` keinen zusätzlichen Code erzeugen. Die häufigste Ursache ist, dass eine vorhandene Methode bereits über dieselbe Signatur verfügt. Die in .net ist Folgendes möglich:

```csharp
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

In solchen Fällen werden nur zwei generierte `init` Selektoren erstellt, die beide Mono aufrufen, aber es ist kein Wrapper für die spätere vorhanden.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: die Methode `M` wird nicht generiert, weil der Rückgabetyp `T` nicht unterstützt wird.

Dies ist eine **Warnung** , dass die Methode `M` ignoriert wird (d. h., es werden keine Elemente generiert), da der Rückgabetyp `T` nicht unterstützt wird.

Es sollte eine frühere Warnung geben, die weitere Informationen enthält, warum Type `T` nicht unterstützt wird.

Hinweis: unterstützte Funktionen werden mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: die Methode `M` wird nicht generiert, weil der Parametertyp `T` nicht unterstützt wird.

Dies ist eine **Warnung** , dass die-Methode `M` ignoriert wird (d. h., es werden keine Elemente generiert), da ein Parameter vom Typ "`T`" nicht unterstützt wird.

Es sollte eine frühere Warnung geben, die weitere Informationen enthält, warum Type `T` nicht unterstützt wird.

Hinweis: unterstützte Funktionen werden mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: die Methode `M` hat Standardwerte, für die kein Wrapper generiert wird.

Dies ist eine **Warnung** , dass die Standardparameter der Methode `M` keinen zusätzlichen Code erzeugen. Die häufigste Ursache ist, dass eine vorhandene Methode bereits über dieselbe Signatur verfügt. Die in .net ist Folgendes möglich:

```csharp
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

In solchen Fällen werden nur zwei generierte `increment` Selektoren erstellt, die beide Mono aufrufen, aber es ist kein Wrapper für die spätere vorhanden.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: die Methode `M` wird nicht generiert, weil eine andere Methode den Operator mit einem anzeigen Amen verfügbar macht.

Dies ist eine **Warnung** , dass die-Methode `M` nicht generiert wird, weil eine andere Methode den Operator mit einem anzeigen Amen verfügbar macht. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: `M` der Erweiterungsmethode wird nicht innerhalb einer Kategorie generiert, da Sie nicht für primitive Typen `T`erstellt werden können. Es wurde eine normale, statische Methode generiert.

Dies ist eine **Warnung** , dass eine Erweiterungsmethode für einen priminungstyp (z. b. `System.Int32`) gefunden wurde. In Ziel-C ist es nicht möglich, Kategorien für den primitiven Typ zu erstellen. Stattdessen erzeugt der Generator eine normale, statische Methode.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: die Eigenschafts `P` wird nicht generiert, weil der Parametertyp `T` nicht unterstützt wird.

Dies ist eine **Warnung** , dass die Eigenschaft `P` ignoriert wird (d. h., es werden keine Elemente generiert), da der verfügbar gemachte Typ `T` nicht unterstützt wird.

Es sollte eine frühere Warnung geben, die weitere Informationen enthält, warum Type `T` nicht unterstützt wird.

Hinweis: unterstützte Funktionen werden mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: indizierte Eigenschaften für `T` werden nicht generiert, weil mehrere indizierte Eigenschaften nicht unterstützt werden.

Dies ist eine **Warnung** , dass die indizierten Eigenschaften auf `T` ignoriert werden (d. h., es werden keine Elemente generiert), da mehrere indizierte Eigenschaften nicht unterstützt werden.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: das Feld `F` wird nicht generiert, weil der Feldtyp `T` nicht unterstützt wird.

Dies ist eine **Warnung** , dass das Feld `F` ignoriert wird (d. h. es werden keine Elemente generiert), da der verfügbar gemachte Typ `T` nicht unterstützt wird.

Es sollte eine frühere Warnung geben, die weitere Informationen enthält, warum Type `T` nicht unterstützt wird.

Hinweis: unterstützte Funktionen werden mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: das Element `E` wird stattdessen als `F` generiert, da der Name mit einem wichtigen Ziel-c-Selektor in Konflikt steht.

Dies ist eine **Warnung** , dass das Element `E` stattdessen als `F` generiert wird, da der Name mit einem wichtigen Ziel-c-Selektor in Konflikt steht.

Selektoren für das [nsobjectprotocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) haben eine wichtige Bedeutung in Ziel-c und müssen sorgfältig überschrieben werden.

Hinweis: die Liste der reservierten Selektoren wird mit neuen Versionen des Tools weiterentwickelt.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: das Element `E` wird nicht generiert. der Name steht in Konflikt mit anderen Elementen in derselben Klasse.

Dies ist eine **Warnung** , dass Element `E` nicht generiert werden, da der Name mit anderen Elementen in der gleichen Klasse in Konflikt steht.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Ziel `E` wird für xamarin. IOS und xamarin. Mac nicht unterstützt. Nur die Option `framework` wird als unterstützt betrachtet und sollte verwendet werden.

Dies ist eine **Warnung** , dass Ziel `E` für xamarin. IOS-und xamarin. Mac-Anwendungsfälle als nicht unterstützt gilt. 

Der Verbrauch statischer oder dynamischer .net-Einbettungs Bibliotheken erfordert möglicherweise zusätzliche Arbeitsschritte oder Anpassungen und sollte in den meisten Anwendungsfällen vermieden werden.

Entfernen Sie ggf. den `--target` Parameter, oder übergeben Sie stattdessen `--target=framework`.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: Code Generierung

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
