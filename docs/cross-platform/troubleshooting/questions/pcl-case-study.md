---
title: Beheben von Problemen im Zusammenhang mit System.Diagnostics.Tracing und TPL-Datenfluss
description: 'PCL-Fallstudie: wie können Sie Probleme im Zusammenhang mit System.Diagnostics.Tracing für das Microsoft TPL Dataflow-NuGet-Paket lösen?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: 1acc9ccc78ad14198a59e74d1fae845790d66b16
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL-Fallstudie: wie können Sie Probleme im Zusammenhang mit System.Diagnostics.Tracing für das Microsoft TPL Dataflow-NuGet-Paket lösen?

> [!IMPORTANT]
> Von diesem speziellen Beispiel `System.Diagnostic.Tracing` mehr Fehler erzeugt, wird standardmäßig in den neuesten Versionen von Xamarin. Während die vorgeschlagene problemumgehung weiterhin funktionieren, beachten Sie, dass einige der im Abschnitt "Ebenen von Fehlern" erwähnten Fehler behoben wurden.
> Darüber hinaus sollten Sie beachten, dass .NET Standard jetzt die bevorzugte Art der plattformübergreifenden .NET APIs implementieren.

## <a name="summary"></a>Zusammenfassung

Xamarin.iOS und Xamarin.Android implementieren nicht 100 % der jedes Profil PCL, die sie als Verweise zu ermöglichen. Praktische halber in Visual Studio für Mac und Visual Studio NuGet-Paket-Manager zulassen Xamarin Projekte die Verwendung von mehreren Profilen, die nur _unvollständige_ Implementierungen. Weder Xamarin.iOS und Xamarin.Android derzeit enthält beispielsweise eine vollständige Implementierung der Typen in der PCL "System.Diagnostics.Tracing" Namespace. Diese Einschränkung führt mit drei Ebenen von Fehlern beim Versuch, die Standardeinstellung verwenden `portable-net45+win8+wpa81` Version des Microsoft TPL Dataflow-NuGet-Pakets.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Problemumgehung: Wechseln Sie das app-Projekt zu verweisen die `portable-net45+win8+wp8+wpa81` Version die TPL-Datenflussbibliothek

(Dadurch werden alle drei Ebenen von Fehlern und funktioniert bei allen früheren Versionen von Xamarin vermieden.)

1. Öffnen Sie das Anwendungsprojekt **csproj** -Datei in einem Text-Editor.

2. Suchen Sie die Zeile, die in etwa wie folgt aussieht:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Änderung `portable-net45+win8+wpa81` auf `portable-net45+win8+wp8+wpa81` (`+wp8` hinzugefügt wird):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Erklärung

Die `portable-net45+win8+wp8+wpa81` Version der Bibliothek verweist nicht auf **System.Diagnostics.Tracing.dll** _überhaupt_, wodurch alle drei Ebenen von Problemen vollständig vermieden werden.

### <a name="limitations"></a>Einschränkungen

- Die `portable-net45+win8+wp8+wpa81` Version der Bibliothek enthalten möglicherweise nicht die Funktionalität von 100 % der `portable-net45+win8+wpa81` Version.

- Installiert das NuGet-Paket-Manager die `portable-net45+win8+wpa81` Version des PCL NuGet-Pakets standardmäßig, sodass Sie den Verweis manuell anpassen müssen.

## <a name="details-about-the-three-layers-of-errors"></a>Details zu den drei Ebenen von Fehlern

1. Die **System.Diagnostics.Tracing.dll** Fassade Assembly wird derzeit von allen Macintosh-Versionen von Xamarin.Android (nicht öffentlichen Bug 34888) fehlt und abwesende aus allen Xamarin.iOS Versionen 9.0 niedrigeren (oder niedriger ist als XamarinVS 3.11.1443 unter Windows) (in festen [Fehler 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Dieses Problem wird einer der folgenden Fehler je nach Bereitstellungsziel und der Linker Einstellungen verursacht:

    - Xamarin.Android.Common.targets: Fehler: Ausnahme beim Laden von Assemblys: System.IO.FileNotFoundException: Assembly konnte nicht geladen werden "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a". Möglicherweise ist es in der Mono für Android-Profil nicht vorhanden?

    - Datei oder Assembly 'System.Diagnostics.Tracing' oder eine ihrer Abhängigkeiten konnte nicht geladen werden. Die angegebene Datei wurde nicht gefunden.“ (System.IO.FileNotFoundException)

    - MTOUCH: Fehler MT3001: konnte keine AOT der Assembly "/ Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll"

    - MTOUCH: Fehler MT2002: Fehler beim Auflösen der Assembly: "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a"

2. Die aktuelle [Mono-Implementierung der Typen in "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) fehlen einige Überladungen der Methode ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Dieses Problems eine der folgenden Linkerfehler bewirkt, dass beim Erstellen einer Xamarin-app:

    - /Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: error : Error executing task LinkAssemblies: error XA2006: Reference to metadata item 'System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])' (defined in 'System.Threading.Tasks.Dataflow, Version=4.5.24.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a') from 'System.Threading.Tasks.Dataflow, Version=4.5.24.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' could not be resolved.

    - MTOUCH: Fehler MT2002: Fehler beim Auflösen von "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" Verweis "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = Neutral, PublicKeyToken = b03f5f7f11d50a3a"

3. Die aktuelle [Mono-Implementierung der Typen in "System.Diagnostics.Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) ist derzeit auch ein _leere_ "dummy" Implementierung ([Fehler 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Jeder Versuch, verwenden diese Methoden zur Laufzeit kann daher mit unerwarteten Ergebnissen führen. Für die _bestimmten_ Fall der TPL-Datenflussbibliothek von Microsoft, die es scheint die Aufrufe von `WriteEvent(System.Int32,System.Object[])` sind nicht wichtig für die meisten Verhalten der Bibliothek, damit die Fix für "Layer 2" ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), Hinzufügen von leeren Implementierungen) ausreichen für die meisten Microsoft TPL Datenfluss Anwendungsfälle wahrscheinlich.

## <a name="questions--answers"></a>Fragen und Antworten

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Lassen Sie die Verknüpfung mit aktiviert war die <code>portable-net45+win8+wpa81</code> Version der Bibliothek auf älteren Versionen von Xamarin.iOS oder Xamarin.Android. Wie funktionieren, die die?

#### <a name="answer"></a>Antwort

Es ist _möglich_ auf der Build abgeschlossen "erfolgreich auf" (Verknüpfung aktiviert) in älteren Versionen von Xamarin.iOS oder Xamarin.Android auf einem Mac, wenn Sie einen Verweis auf verfügen die `System.Diagnostics.Tracing.dll` _Assemblyverweisen_ \[1\] statt über das _Fassade Assembly_ \[2], was jedoch leider nicht "richtig" dieses Problem zu umgehen. Verweisassemblys werden sollten nur verwendet werden, beim Erstellen von _portable Bibliotheken_, nicht plattformspezifischen Code wie apps. Es wird versucht, _ausführen_ der Verweisassemblys (statt nur Build gegenüber diesem) enthaltene Code ist wahrscheinlich zu unerwarteten Ergebnissen führen. Die richtige Korrektur werden für den Mono / Team hinzufügen die fehlende `WriteEvent(System.Int32,System.Object[])` Überladung auf, um die [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) Typ ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Für jetzt die beste Option ist zum Wechseln der `portable-net45+win8+wp8+wpa81` Version der Microsoft TPL-Datenflussbibliothek wie im obigen Abschnitt Problemumgehung erläutert.

(Personen, die in diesem Artikel nach dem Lesen einer verwandten älteren, schneller Antwort vom StackOverflow gelesen werden kann (<http://stackoverflow.com/a/23591322/2561894>), Hinweis, der der Unterschied zwischen Verweisassemblys und Fassade Assembly war _nicht_ erwähnt vorhanden.)

**\[1\] Speicherorte "Verweisen auf Assembly"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "Fassaden" Assemblyspeicherorte**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Ist dies hilfreich, wenn ich manuell einen Verweis auf die "System.Diagnostics.Tracing" Fassade-Assembly hinzufügen?

_Insbesondere kann ich das Problem mithilfe der folgenden 2 Schritte gelöst?_

1. _Kopieren der `System.Diagnostics.Tracing.dll` Fassade-Assembly in den Projektordner der Anwendung aus einem der folgenden Speicherorte:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Fügen Sie einen Verweis auf die Assembly Fassade im Anwendungsprojekt Xamarin.iOS oder Xamarin.Android._

#### <a name="answer"></a>Antwort

Nein, wird dadurch nicht behoben.

- Für Xamarin.iOS 9.0 oder aktuelle Version der Xamarin.Android unter Windows diese problemumgehung ist streng redundant, und treten Kompilierungsfehler ähnelt "hat eine Assembly"System.Diagnostics.Tracing"mit der gleichen Identität bereits importiert wurden.".

- Xamarin.iOS 8.10 oder niedriger oder Xamarin.Android auf einem Mac, hilft diese problemumgehung funktioniert jedoch _nur_ für das "Layer 1"-Problem der fehlenden Assembly. Es wird _nicht_ Linkerfehler "layer 2" zu beheben, damit es sich nicht um eine vollständige Lösung handelt.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Kann ich verwenden die [System.Diagnostics.Tracing NuGet-Paket](https://www.nuget.org/packages/System.Diagnostics.Tracing/) zur Behebung des Problems?

#### <a name="answer"></a>Antwort

Nein, enthält das NuGet-3.0-Paket "System.Diagnostics.Tracing" nur plattformspezifische Implementierungen für "DNXCore50" und "netcore50". Es explizit _lässt_ Implementierungen für Xamarin.Android ("MonoAndroid") und Xamarin.iOS ("MonoTouch" und "Xamarinios"). Dies bedeutet, dass das Paket wird installiert _wirken sich nicht_ für Xamarin.Android und Xamarin.iOS-Projekte. Das NuGet-Paket wird davon ausgegangen, dass beide diese Plattformen bieten ihre _eigenen_ Implementierung der Typen. Diese Annahme ist "richtig", in dem Sinne, die Mono besitzt _ein_ Implementierung des Namespace, jedoch als unter diskutierten Punkte \#2 und \#3 von "Details zu den drei Ebenen von Fehlern" oben die Implementierung ist derzeit nicht vollständig. Damit die richtige Lösung für den Mono / Team aufgelöst werden [Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) und [Fehler 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Nächste Schritte

Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf.
