---
title: .NET Einbetten von Fehlern
ms.topic: article
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 90d30b92069bcd6a5c008fa8009c0392c4d26473
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="em0xxx-binding-error-messages"></a>EM0xxx: Fehlermeldungen Bindung

Z. B. Parameter, Umgebung

<!-- 0xxx: the generator itself, e.g. parameters, environment -->
<h3><a name="EM0000"/>EM0000: Unerwarteter Fehler: Bitte geben Sie einen Fehlerbericht an https://github.com/mono/Embeddinator-4000/issues</h3>

Ein unerwarteter Fehler aufgetreten ist. Bitte [eine](https://github.com/mono/Embeddinator-4000/issues) durch so viele Informationen wie möglich, einschließlich:

* Vollständige Buildprotokolle, mit maximale Ausführlichkeit
* Eine minimale Testfall, der den Fehler reproduzieren
* Alle Informationen der version

Die einfachste Möglichkeit zum Abrufen von Informationen zur exakten Version ist die Verwendung der **Xamarin Studio** Menü **über Xamarin Studio** Element **Details anzeigen** Schaltfläche und die Version, kopieren und einfügen Informationen (Sie können die **Informationen kopieren** Schaltfläche).

<h3><a name="EM0001"/>EM0001: Ausgabeverzeichnis konnte nicht erstellt werden. `X`</h3>

Der Verzeichnisname angegeben `-o=DIR` ist nicht vorhanden und konnte nicht erstellt werden. Er möglicherweise einen ungültigen Namen für das Dateisystem.

<h3><a name="EM0002"/>EM0002: Option `X` wird nicht unterstützt</h3>

Das Tool unterstützt nicht die Option `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<h3><a name="EM0003"/>EM0003: Die Plattform `X` ist ungültig.</h3>

Das Tool unterstützt die Plattform nicht `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<h3><a name="EM0004"/>EM0004: Das Ziel `X` ist ungültig.</h3>

Das Tool unterstützt nicht das Ziel `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<h3><a name="EM0005"/>EM0005: Kompilierungsziel `X` ist ungültig.</h3>

Das Tool unterstützt nicht das Kompilierungsziel `X`. Es ist möglich, dass eine andere Version des Tools unterstützt, oder dass sie nicht in dieser Umgebung zutreffen.

<h3><a name="EM0006"/>EM0006: Den Speicherort Xcode nicht gefunden.</h3>

Das Tool konnte nicht gefunden werden die aktuell ausgewählten Xcode Speicherort mit der `xcode-select -p` Befehl. Stellen Sie sicher, dass dieser Befehl ist erfolgreich, und gibt den Speicherort der richtigen Xcode zurück.

<h3><a name="EM0007"/>EM0007: Die Sdk-Version für "{Sdk}" konnte nicht abgerufen werden.</h3>

Das Tool konnte nicht abgerufen werden die SDK-Version mithilfe der `xcrun --show-sdk-version --sdk {sdk}` Befehl. Stellen Sie sicher, dass dieser Befehl ist erfolgreich, und die SDK-Version zurückgegeben.

<h3><a name="EM0008"/>EM0008: Die Architektur "{Arch}" ist ungültig für {Plattform}. Gültige Architekturen für {Plattform} sind: "{Architekturen}".</h3>

Die Architektur in der Fehlermeldung angegebene gilt nicht für die Zielplattform. Stellen Sie sicher, dass die Option – Abi gültige Architektur übergeben wird.

<h3><a name="EM0009"/>EM0009: Das Feature `X` ist derzeit nicht vom Generator implementiert</h3>

Dies ist ein bekanntes Problem, das wir in einer zukünftigen Version des Generators beheben möchten. Beiträge sind Willkommen.

<h3><a name="EM0010"/>EM0010: Nicht die Frameworks {SimulatorFramework} und {DeviceFramework} zusammengeführt, da die Datei "{}" in beiden vorhanden ist.</h3>

Das Tool konnte die Frameworks, die in der Fehlermeldung genannten nicht zusammenführen, weil eine gemeinsame Datei zwischen ihnen vorhanden ist.

Dies weist möglicherweise einen Fehler in der Embeddinator-4000 hin; Bitte der Datei eines Fehlerberichts an [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) zu einem Testfall.

<h3><a name="EM0011"/>EM0011: Die Assembly `X` ist nicht vorhanden.</h3>

Das Tool die Assembly wurde nicht gefunden `X` in den Argumenten angegeben.

<h3><a name="EM0012"/>EM0012: Der Name der Assembly `X` ist nicht eindeutig.</h3>

Mehr als eine Assembly angegeben haben, den gleichen, internen Namen, und es nicht möglich wäre, zur Laufzeit unterschieden.

Die wahrscheinlichste Ursache ist, dass eine Assembly auf die Befehlszeilenargumente mehr als einmal angegeben wird. Wird jedoch gleichzeitig einen behält der umbenannten Assembly noch wird der ursprüngliche Name und mehrere Kopien nicht möglich.

<h3><a name="EM0013"/>EM0013: Findet nicht die Assembly 'X', 'Y' verweist.</h3>

Das Tool wurde die Assembly 'X', auf die verwiesen wird durch die Assembly 'Y' nicht gefunden werden. Stellen Sie sicher, dass alle referenzierten Assemblys im gleichen Verzeichnis wie die Assembly gebunden werden.

<h3><a name="EM0014"/>EM0014: Wurde nicht gefunden {Produkt} ({Produkt} {Min_version} erforderlich ist).</h3>

Die Abhängigkeit, die in der Fehlermeldung genannten konnte auf dem System nicht gefunden werden.

<h3><a name="EM0015"/>EM0015: Eine gültige Version {Produkts} nicht gefunden ({Version}, gefunden jedoch mindestens {Min_version} ist erforderlich).</h3>

Die Abhängigkeit, die im Fehler genannt werden, Nachricht wurde auf dem System gefunden, aber es ist zu alt. Aktualisieren Sie auf eine neuere Version.

<h3><a name="EM0016">EM0016: Konnte nicht erstellt werden "Symlink" "{file}" -> "{target}": Fehler {Number}</h3>

Die in der Fehlermeldung genannten "Symlink" konnte nicht erstellt werden.

<h3><a name="EM0026"/>Das Befehlszeilenargument "A" EM0026 konnte nicht analysiert werden: *</h3>

Die Syntax für die Befehlszeilenoption `A` konnte nicht vom Tool analysiert werden. Es ist wahrscheinlich falsch ist, wenden Sie sich mit der Dokumentation oder der Hilfe für die richtige Syntax.

<h3><a name="EM0099"/>EM0099: Interner Fehler *. Bitte der Datei eines Fehlerberichts zu einem Testfall (https://github.com/mono/Embeddinator-4000/issues).</h3>

Diese Fehlermeldung wird gemeldet, wenn es sich bei eine internen konsistenzüberprüfung in der Embeddinator-4000 ein Fehler auftritt.

Gibt einen Fehler in der Embeddinator-4000. Bitte der Datei eines Fehlerberichts an [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) zu einem Testfall.


<!-- 1xxx: code processing -->

# <a name="em1xxx-code-processing"></a>EM1xxx: Code-Verarbeitung

<h3><a name="EM1010"/>Typ `T` wird nicht generiert werden, da `X` werden nicht unterstützt.</h3>

Dies ist eine **Warnung** , den Typ `T` werden ignoriert (d. h. keine generiert werden), da er verwendet `X`, eine Funktion, die nicht unterstützt wird.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.


<h3><a name="EM1011"/>Typ `T` wird nicht generiert werden, da es sich um einen einheitlichen Gegenstück fehlt.</h3>

Dies ist eine **Warnung** , die den Typ `T` werden ignoriert (d. h. keine generiert werden) da es machen etwas aus .NET Framework verwendet, die über keine Entsprechung in die systemeigene Plattform verfügt.


<h3><a name="EM1011"/>Typ `T` wird nicht generiert werden, weil sie nicht mit einem systemeigenen Gegenstück Marshallingcode enthält.</h3>

Dies ist eine **Warnung** , die den Typ `T` werden ignoriert (d. h. keine generiert werden) da es machen etwas aus .NET Framework verwendet, die zusätzliche Marshalling erfordert.

Hinweis: Dies ist etwas, das ist möglicherweise eine unterstützt abrufen, mit einigen Einschränkungen in einer zukünftigen Version des Tools.


<h3><a name="EM1020"/>Konstruktor `C` wird nicht aufgrund der Parametertyp generiert `T` wird nicht unterstützt.</h3>

Dies ist eine **Warnung** , die der Konstruktor `C` werden ignoriert (d. h. keine generiert werden) da einen Parameter vom Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.


<h3><a name="EM1021"/>Konstruktor `C` Standardwerte für die kein Wrapper generiert wurde.</h3>

Dies ist eine **Warnung** , die Standardparameter Konstruktors `C` keine zusätzlichen Code generieren. Die häufigste Ursache ist, dass eine vorhandene Methode bereits die gleiche Signatur verfügt. Z. B. in .net ist es möglich, dass:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

In solchen Fällen nur zwei generiert `init` Selektoren erstellt werden, beide aufrufen Mono, aber kein Wrapper für die spätere vorhanden.


<h3><a name="EM1030"/>Methode `M` generiert, da kein Rückgabetyp `T` wird nicht unterstützt.</h3>

Dies ist eine **Warnung** , die Methode `M` werden ignoriert (d. h. keine generiert werden), da es Rückgabetyp ist `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.


<h3><a name="EM1031"/>Methode `M` wird nicht aufgrund der Parametertyp generiert `T` wird nicht unterstützt.</h3>

Dies ist eine **Warnung** , die die Methode `M` werden ignoriert (d. h. keine generiert werden) da einen Parameter vom Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.


<h3><a name="EM1032"/>Methode `M` Standardwerte für die kein Wrapper generiert wurde.</h3>

Dies ist eine **Warnung** , die Standardparameter Methode `M` keine zusätzlichen Code generieren. Die häufigste Ursache ist, dass eine vorhandene Methode bereits die gleiche Signatur verfügt. Z. B. in .net ist es möglich, dass:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

In solchen Fällen nur zwei generiert `increment` Selektoren erstellt werden, beide aufrufen Mono, aber kein Wrapper für die spätere vorhanden.


<h3><a name="EM1033"/>Methode `M` wird nicht generiert werden, da eine andere Methode den Operator mit einem Anzeigenamen verfügbar macht.</h3>

Dies ist eine **Warnung** , die die Methode `M` wird nicht generiert werden, da eine andere Methode den Operator mit einem Anzeigenamen verfügbar macht. (https://msdn.microsoft.com/en-us/library/ms229032 (v=vs.110).aspx)


<h3><a name="EM1034"/>Erweiterungsmethode `M` generiert, da sie für den primitiven Typ erstellt werden, können nicht innerhalb einer Kategorie `T`. Eine normale, statische Methode generiert wurde.</h3>

Dies ist eine **Warnung** , dass eine Erweiterungsmethode für einen Primivite eingeben (z. B. `System.Int32`) wurde gefunden. In ObjC ist es nicht möglich, um Kategorien für den primitiven Typ zu erstellen. Stattdessen werden der Generator erzeugt eine normale, statische Methode.



<h3><a name="EM1040"/>Eigenschaft `P` wird nicht aufgrund der Parametertyp generiert `T` wird nicht unterstützt.</h3>

Dies ist eine **Warnung** , die die Eigenschaft `P` werden ignoriert (d. h. keine generiert werden) da den verfügbar gemachten Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<h3><a name="EM1041"/>Indizierte Eigenschaften auf `T` wird nicht generiert werden, da mehrere indizierte Eigenschaften nicht unterstützt werden.</h3>

Dies ist eine **Warnung** , die indizierte Eigenschaften auf `T` werden ignoriert (d. h. keine generiert werden), weil mehrere indizierte Eigenschaften nicht unterstützt werden.


<h3><a name="EM1050"/>Feld `F` wird nicht aufgrund der Feldtyp generiert `T` wird nicht unterstützt.</h3>

Dies ist eine **Warnung** , die das Feld `F` werden ignoriert (d. h. keine generiert werden) da den verfügbar gemachten Typ `T` wird nicht unterstützt.

Geben Sie eine frühere Warnung mit weiteren Informationen, warum sollte `T` wird nicht unterstützt.

Hinweis: Unterstützte Funktionen werden durch neue Versionen des Tools weiterentwickelt.

<h3><a name="EM1051"/>Element `E` generiert stattdessen als `F` weil dessen Name mit einer wichtigen Objective-c-Selektor in Konflikt steht.</h3>

Dies ist eine **Warnung** , die das Element `E` generiert werden stattdessen als `F` weil dessen Name mit einer wichtigen Objective-c-Selektor in Konflikt steht.

Selektoren auf die [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) haben wichtige Bedeutung in Objective-c und muss sorgfältig überschrieben werden.

Hinweis: Die Liste der reservierten Selektoren wird mit neuen Versionen des Tools weiterentwickelt.


<!-- 2xxx: code generation -->

# <a name="em2xxx-code-generation"></a>EM2xxx: Codegenerierung


<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
