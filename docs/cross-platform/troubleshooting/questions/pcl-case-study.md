---
title: Beheben von Problemen im Zusammenhang mit System.Diagnostics.Tracing und TPL Dataflow
description: 'PLC-Fallstudie: Wie kann ich Probleme im Zusammenhang mit System.Diagnostics.Tracing für das NuGet-Paket für den Microsoft-TPL-Datenfluss lösen?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: d9aa85b946f20addb7d69c559bff68c6b1f75429
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342273"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PLC-Fallstudie: Wie kann ich Probleme im Zusammenhang mit System.Diagnostics.Tracing für das NuGet-Paket für den Microsoft-TPL-Datenfluss lösen?

> [!IMPORTANT]
> Von diesem speziellen Beispiel `System.Diagnostic.Tracing` mehr Fehler erzeugt, wird standardmäßig in den neuesten Versionen von Xamarin. Die vorgeschlagene problemumgehung weiterhin funktionieren, beachten Sie, dass einige der im Abschnitt "Ebenen der Fehler" erwähnt wurde Fehler behoben wurden.
> Darüber hinaus sollten Sie beachten, dass .NET Standard jetzt die bevorzugte Methode der Implementierung von plattformübergreifenden .NET APIs.

## <a name="summary"></a>Zusammenfassung

Xamarin.iOS und Xamarin.Android implementieren nicht 100 % der jedes PCL-Profil, die sie als Referenzen zu ermöglichen. Praktische der Einfachheit halber in Visual Studio für Mac und Visual Studio den NuGet-Paket-Manager-Xamarin-Projekte ermöglichen die Verwendung mehrere Profile, die nur _unvollständige_ Implementierungen. Weder Xamarin.iOS und Xamarin.Android derzeit enthält beispielsweise eine vollständige Implementierung der Typen in der PCL "System.Diagnostics.Tracing" Namespace. Diese Einschränkung führt mit drei Ebenen von Fehlern beim Versuch, die Standardeinstellung verwenden `portable-net45+win8+wpa81` Version des Microsoft-TPL Dataflow-NuGet-Pakets.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Problemumgehung: Wechseln Sie das app-Projekt auf die `portable-net45+win8+wp8+wpa81` Version von der TPL-Datenflussbibliothek

(Dadurch werden alle drei Ebenen von Fehlern und funktioniert für alle Versionen von Xamarin vermieden).

1. Öffnen Sie das Anwendungsprojekt **csproj** -Datei in einem Text-Editor.

2. Suchen Sie die Zeile, die in etwa wie folgt aussieht:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Änderung `portable-net45+win8+wpa81` zu `portable-net45+win8+wp8+wpa81` (`+wp8` wird hinzugefügt):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Erklärung

Die `portable-net45+win8+wp8+wpa81` Version der Bibliothek verweist nicht auf **System.Diagnostics.Tracing.dll** _überhaupt_, sodass alle drei Ebenen von Problemen vollständig verhindert.

### <a name="limitations"></a>Einschränkungen

- Die `portable-net45+win8+wp8+wpa81` Version der Bibliothek enthalten möglicherweise nicht die Funktionalität von 100 % der `portable-net45+win8+wpa81` Version.

- Installiert der NuGet-Paket-Manager die `portable-net45+win8+wpa81` Version des PCL-NuGet-Pakets in der Standardeinstellung, damit Sie den Verweis manuell anpassen müssen.

## <a name="details-about-the-three-layers-of-errors"></a>Details zu den drei Ebenen von Fehlern

1. Die **System.Diagnostics.Tracing.dll** Fassade Assembly befindet sich derzeit gelten alle Mac-Versionen von Xamarin.Android (nicht öffentlichen Fehler 34888) und das Fehlen von allen Xamarin.iOS-Versionen kleiner als 9.0 (oder niedriger als XamarinVS 3.11.1443 für Windows) (in festen [Fehler 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Dieses Problem bewirkt eine der folgenden Fehlermeldungen je nach Ziel der Bereitstellung und der Linker Einstellungen:

    - Xamarin.Android.Common.targets: Fehler: Ausnahme beim Laden von Assemblys: System.IO.FileNotFoundException: Assembly konnte nicht geladen "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a". Vielleicht ist es in der Mono für Android-Profil nicht vorhanden?

    - Datei oder Assembly 'System.Diagnostics.Tracing' oder eine ihrer Abhängigkeiten konnte nicht geladen werden. Die angegebene Datei wurde nicht gefunden.“ (System.IO.FileNotFoundException)

    - MTOUCH: Fehler MT3001: Konnte kein AOT der Assembly "/ Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll"

    - MTOUCH: Fehler MT2002: Fehler beim Auflösen des Assembly: 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

2. Die aktuelle [Mono-Implementierung der Typen in "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) fehlen einige Überladungen der Methode ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Dieses Problem bewirkt eine der folgenden Linkerfehler beim Erstellen einer Xamarin-app:

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: Fehler: Fehler beim Ausführen von task LinkAssemblies: Fehler XA2006: Verweis auf Metadaten, Element "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" (definiert "System.Threading.Tasks.Dataflow, Version 4.5.24.0, Kultur = = Neutral, PublicKeyToken = b03f5f7f11d50a3a') aus "System.Threading.Tasks.Dataflow, Version = 4.5.24.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a' konnte nicht aufgelöst werden.

    - MTOUCH: Fehler MT2002: Fehler beim Auflösen von "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" Verweis "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a"

3. Die aktuelle [Mono-Implementierung der Typen in "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) ist derzeit auch ein _leere_ "dummy"-Implementierung ([Fehler 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Jeder Versuch, diese Methoden verwenden, zur Laufzeit kann daher mit unerwarteten Ergebnissen führen. Für die _bestimmten_ Fall von der Microsoft-TPL-Datenflussbibliothek scheint es die Aufrufe an `WriteEvent(System.Int32,System.Object[])` sind nicht wichtig für den Großteil des Verhaltens der Bibliothek, also die Lösung für "Layer-2" ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), Hinzufügen von leeren -Implementierungen) werden wahrscheinlich Microsoft TPL Dataflow in den meisten Fällen ausreichend.

## <a name="questions--answers"></a>Fragen und Antworten

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Ich lassen die Verknüpfung mit aktiviertem konnte die <code>portable-net45+win8+wpa81</code> Version der Bibliothek auf älteren Versionen von Xamarin.iOS oder Xamarin.Android. Wie hat dies funktioniert?

#### <a name="answer"></a>Antwort

Es ist _möglich_ zum das Build abgeschlossen "erfolgreich auf" abrufen (mit Verknüpfung aktiviert) in älteren Versionen von Xamarin.iOS oder Xamarin.Android unter Mac, wenn Sie einen Verweis auf enthalten die `System.Diagnostics.Tracing.dll` _verweisen auf die Assembly_ \[1\] anstelle der _Fassade Assembly_ \[2], doch leider ist dies nicht "richtig" dieses Problem zu umgehen. Verweisassemblys werden sollten nur verwendet werden, beim Erstellen von _portablen Bibliotheken_, keine plattformspezifischen Code wie apps. Es wird versucht, _ausführen_ der Code in referenzierten Assemblys (statt bauen Sie einfach dafür) vermutlich zu unerwarteten Ergebnissen führen. Die richtige Lösung für das Mono-Team das fehlende hinzufügen werden `WriteEvent(System.Int32,System.Object[])` Überladung auf, um die [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) Typ ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Für die beste Option jetzt ist so wechseln Sie zu der `portable-net45+win8+wp8+wpa81` Version der Microsoft-TPL Dataflow Library wie im obigen Abschnitt zu Problemumgehung beschrieben.

(Für jeden Benutzer, der diesen Artikel lesen werden möglicherweise nach einer verwandten ältere, schneller Antwort StackOverflow angezeigt (<https://stackoverflow.com/a/23591322/2561894>), beachten Sie, die der Unterschied zwischen Verweisassemblys und Fassade Assembly wurde _nicht_ es erwähnt.)

**\[1\] Speicherorte "Verweisen auf Assembly"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "Fassaden" Assemblyspeicherorte**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Ist es hilfreich, wenn ich manuell einen Verweis auf die Assembly "System.Diagnostics.Tracing" Fassade hinzufügen?

_Insbesondere kann ich das Problem mithilfe der folgenden 2 Schritte gelöst?_

1. _Kopieren der `System.Diagnostics.Tracing.dll` Fassade-Assembly in den Projektordner der Anwendung von einem der folgenden Speicherorte:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Fügen Sie einen Verweis auf die Fassade-Assembly, in dem Projekt Xamarin.iOS oder Xamarin.Android-Anwendung._

#### <a name="answer"></a>Antwort

Nein, dies ist nicht hilfreich.

- Für Xamarin.iOS 9.0 oder jede halbwegs aktuelle Version von Xamarin.Android unter Windows diese problemumgehung ist streng redundant und kann dazu führen, dass Kompilierungsfehler, die ähnlich wie "verfügt über eine Assembly"System.Diagnostics.Tracing"mit der gleichen Identität bereits importiert.".

- Für Xamarin.iOS 8.10 oder niedriger oder Xamarin.Android unter Mac, hilft diese problemumgehung jedoch _nur_ für das "Layer 1"-Problem der fehlenden Assembly. Es wird _nicht_ "layer 2" Linker-Fehler zu beheben, daher ist es nicht über eine vollständige Lösung.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Kann ich verwenden die [System.Diagnostics.Tracing-NuGet-Paket](https://www.nuget.org/packages/System.Diagnostics.Tracing/) zur Lösung des Problems?

#### <a name="answer"></a>Antwort

Nein, enthält NuGet 3.0 "System.Diagnostics.Tracing"-Paket nur plattformspezifische Implementierungen für "DNXCore50" und "netcore50". Es explizit _lässt_ Implementierungen für Xamarin.Android ("MonoAndroid")- und Xamarin.iOS ("MonoTouch" und "Xamarinios"). Dies bedeutet, dass die Installation des Pakets hat _wirkungslos_ für Xamarin.Android und Xamarin.iOS-Projekte. Das NuGet-Paket wird davon ausgegangen, dass beide diese Plattformen bieten ihren _eigenen_ Implementierung der Typen. Diese Annahme ist "richtig", in dem Sinne, die Mono verfügt _ein_ Implementierung des Namespaces, sondern als unter beschriebenen Punkte \#2 und \#3 mit "Details zu den drei Ebenen von Fehlern" oben die Implementierung sind zurzeit unvollständig. Damit die richtige Lösung für das Mono-Team aufgelöst werden [Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) und [Fehler 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf.
