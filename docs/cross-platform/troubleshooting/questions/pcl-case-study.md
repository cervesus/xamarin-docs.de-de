---
title: Beheben von Problemen im Zusammenhang mit System. Diagnostics. Tracing und TPL DataFlow
description: 'PCL-Fallstudie: Wie kann ich Probleme im Zusammenhang mit System.Diagnostics.Tracing für das NuGet-Paket für den Microsoft-TPL-Datenfluss lösen?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: davidortinau
ms.author: daortin
ms.openlocfilehash: e7c0bcc7450ab718659723293f995c83b4dc517a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013574"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL-Fallstudie: Wie kann ich Probleme im Zusammenhang mit System.Diagnostics.Tracing für das NuGet-Paket für den Microsoft-TPL-Datenfluss lösen?

> [!IMPORTANT]
> In diesem speziellen Beispiel für `System.Diagnostic.Tracing` werden in den neuesten Versionen von xamarin standardmäßig keine Fehler mehr erzeugt. Obwohl die vorgeschlagene Problem Umgehung weiterhin funktioniert, beachten Sie, dass einige der im Abschnitt "Fehler Ebenen" erwähnten Fehler behoben wurden.
> Außerdem sollten Sie beachten, dass .NET Standard jetzt die bevorzugte Methode zum Implementieren von plattformübergreifenden .NET-APIs ist.

## <a name="summary"></a>Zusammenfassung

Xamarin. IOS und xamarin. Android implementieren nicht 100% jedes PCL-Profils, das Sie als Verweise zulassen. Der praktische Vorteil in Visual Studio für Mac, Visual Studio und dem nuget-Paket-Manager ermöglichen xamarin-Projekte die Verwendung mehrerer Profile, die nur _unvollständige_ Implementierungen aufweisen. Beispielsweise enthält weder xamarin. IOS noch xamarin. Android derzeit eine vollständige Implementierung der Typen im "System. Diagnostics. Tracing"-PCL-Namespace. Diese Einschränkung führt zu drei Fehler Ebenen, wenn Sie versuchen, die Standard `portable-net45+win8+wpa81` Version des Microsoft TPL DataFlow nuget-Pakets zu verwenden.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Problem Umgehung: Ändern Sie das App-Projekt, um auf die `portable-net45+win8+wp8+wpa81` Version der TPL-Datenfluss Bibliothek zu verweisen.

(Dadurch werden alle drei Fehler Ebenen vermieden und funktionieren für alle neueren Versionen von xamarin.)

1. Öffnen Sie die Datei "Application Project **. csproj** " in einem Text-Editor.

2. Suchen Sie die Zeile, die in etwa wie folgt aussieht:

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Ändern Sie `portable-net45+win8+wpa81` in `portable-net45+win8+wp8+wpa81` (`+wp8` hinzugefügt):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Erklärung

Die `portable-net45+win8+wp8+wpa81` Version der Bibliothek verweist _überhaupt_nicht auf **System. Diagnostics. Tracing. dll** , sodass alle drei Ebenen von Problemen vollständig vermieden werden.

### <a name="limitations"></a>Einschränkungen

- Die `portable-net45+win8+wp8+wpa81` Version der Bibliothek enthält möglicherweise nicht 100% der Funktionen der `portable-net45+win8+wpa81` Version.

- Der nuget-Paket-Manager installiert standardmäßig die `portable-net45+win8+wpa81` Version des PCL-nuget-Pakets, sodass Sie den Verweis von Hand anpassen müssen.

## <a name="details-about-the-three-layers-of-errors"></a>Details zu den drei Fehler Ebenen

1. Die **System. Diagnostics. Tracing. dll** -Fassaden Assembly ist derzeit nicht in allen Mac-Versionen von xamarin. Android vorhanden (nichtöffentlicher Fehler 34888) und fehlt bei allen xamarin. IOS-Versionen, die niedriger sind als 9,0 (oder niedriger als xamarinvs 3.11.1443 unter Windows) (korrigiert in [Fehler 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Dieses Problem führt je nach Bereitstellungs Ziel und den Linker-Einstellungen zu einem der folgenden Fehler:

    - Xamarin. Android. Common. Targets: Error: Ausnahme beim Laden von Assemblys: System. IO. File otfoundexception: die Assembly "System. Diagnostics. Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a" konnte nicht geladen werden. Vielleicht ist es im Mono für Android-Profil nicht vorhanden?

    - Die Datei oder Assembly ' System. Diagnostics. Tracing ' oder eine ihrer Abhängigkeiten konnte nicht geladen werden. Die angegebene Datei wurde nicht gefunden.“ (System. IO. File otfoundexception)

    - Mberührungs: Fehler MT3001: die Assembly "/users/MacUser/projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mTouch-Cache/64/Build/System.Threading.Tasks.DataFlow.dll" konnte nicht AOT werden.

    - Mtouchscreen: Error MT2002: Fehler beim Auflösen der Assembly: "System. Diagnostics. Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a"

2. In der aktuellen [Mono-Implementierung der Typen in "System. Diagnostics. Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) fehlen einige Methoden Überladungen ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Dieses Problem führt bei der Erstellung einer xamarin-APP zu einem der folgenden Linker-Fehler:

    - /Library/Frameworks/Mono.Framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.Targets: Fehler: Fehler beim Ausführen von Task linkassemblys: Fehler XA2006: Verweis auf das Metadatenelement "System. void System. Diagnostics. Tracing. eventSource:: "Write Event" (System. Int32, System. Object []) "(definiert in" System. Threading. Tasks. DataFlow, Version = 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ") von" System. Threading. Tasks. DataFlow, Version = 4.5.24.0, Culture = neutral ", PublicKeyToken = b03f5f7f11d50a3a ' konnte nicht aufgelöst werden.

    - Mtouchscreen: Error MT2002: Fehler beim Auflösen des System. void System. Diagnostics. Tracing. eventSource:: Write Event (System. Int32, System. Object [])-Verweises von "System. Diagnostics. Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a

3. Die aktuelle [Mono-Implementierung der Typen in "System. Diagnostics. Tracing"](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) ist zurzeit eine _leere_ "Dummy"-Implementierung ([Fehler 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Wenn Sie versuchen, diese Methoden zur Laufzeit zu verwenden, kann dies zu unerwarteten Ergebnissen führen. Für den _speziellen_ Fall der Microsoft TPL-Datenfluss Bibliothek sind die Aufrufe an `WriteEvent(System.Int32,System.Object[])` für das meiste Verhalten der Bibliothek nicht zwingend erforderlich. Daher ist die Korrektur für "Layer 2" ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), Hinzufügen von leeren Implementierungen) wahrscheinlich ausreichend für die meisten Microsoft TPL-Datenfluss Anwendungsfälle.

## <a name="questions--answers"></a>Fragen & Antworten

### <a name="i-was-able-to-leave-linking-enabled-with-the-portable-net45win8wpa81-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>Ich konnte die Verknüpfung mit der `portable-net45+win8+wpa81`-Version der Bibliothek in älteren Versionen von xamarin. IOS oder xamarin. Android aktivieren lassen. Wie funktioniert das?

#### <a name="answer"></a>Te

Es ist _möglich_ , den Build in älteren Versionen von xamarin. IOS oder in xamarin. Android unter Mac erfolgreich zu vervollständigen (mit aktiviertem Verknüpfungs Vorgang), wenn Sie einen Verweis auf die `System.Diagnostics.Tracing.dll` _Verweisassembly_ \[1\] anstelle der _Fassaden-Assembly_ \[2], aber leider ist dies keine "korrekte" Problem Umgehung. Verweisassemblys sind nur für die Erstellung von _portablen Bibliotheken_gedacht, nicht für plattformspezifischen Code wie apps. Der Versuch, den in Verweisassemblys enthaltenen Code _auszuführen_ (anstatt ihn einfach zu erstellen), führt wahrscheinlich zu unerwarteten Ergebnissen. Die richtige Lösung für das Mono-Team besteht darin, die fehlende `WriteEvent(System.Int32,System.Object[])` Überladung dem [`EventSource`](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) -Typ hinzuzufügen ([Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Die beste Möglichkeit ist, auf die `portable-net45+win8+wp8+wpa81` Version der Microsoft TPL-Datenfluss Bibliothek zu wechseln, wie im Abschnitt "Problem Umgehung" oben erläutert.

(Für alle Personen, die diesen Artikel lesen, nachdem Sie eine verwandte ältere, kurze Antwort von StackOverflow (<https://stackoverflow.com/a/23591322/2561894>) gesehen haben, beachten Sie, dass der Unterschied zwischen Verweisassemblys und der Fassaden Assembly hier _nicht_ erwähnt wurde.)

**\[1\] "Verweisassembly"-Speicherorte**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "Fassaden-assemblyorte"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Wird es hilfreich, wenn ich manuell einen Verweis auf die Fassaden Assembly "System. Diagnostics. Tracing" hinzufüge?

_Insbesondere kann ich das Problem mit diesen zwei Schritten lösen?_

1. _Kopieren Sie die `System.Diagnostics.Tracing.dll` Fassaden-Assembly aus einem der folgenden Speicherorte in den Anwendungsprojekt Ordner:_

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Fügen Sie im xamarin. IOS-oder xamarin. Android-Anwendungsprojekt einen Verweis auf die Fassaden-Assembly hinzu._

#### <a name="answer"></a>Te

Nein, dies wird nicht unterstützt.

- Für xamarin. IOS 9,0 oder eine neuere Version von xamarin. Android unter Windows ist diese Problem Umgehung strikt redundant und kann zu Kompilierungs Fehlern führen, ähnlich wie "eine Assembly System. Diagnostics. Tracing" mit derselben Identität wurde bereits importiert. "

- Bei xamarin. IOS 8,10 oder niedriger oder für xamarin. Android unter Mac ist diese Problem Umgehung hilfreich, aber _nur_ für das Problem "Layer 1" fehlt. Die "Layer 2"-Linker-Fehler werden _nicht_ gelöst, sodass es sich nicht um eine komplette Lösung handelt.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Kann ich das " [System. Diagnostics. Tracing"-nuget-Paket](https://www.nuget.org/packages/System.Diagnostics.Tracing/) verwenden, um das Problem zu beheben?

#### <a name="answer"></a>Te

Nein, das nuget 3,0-Paket "System. Diagnostics. Tracing" umfasst nur plattformspezifische Implementierungen für "DNXCore50" und "netcore50". Es werden explizit Implementierungen für xamarin. Android ("monoandroid") und xamarin. IOS ("MonoTouch" und "xamarinios") _ausgelassen_ . Dies bedeutet, dass die Installation des Pakets _keine Auswirkung_ auf xamarin. Android-und xamarin. IOS-Projekte hat. Das nuget-Paket geht davon aus, dass beide Plattformen eine _eigene_ Implementierung der Typen bereitstellen. Diese Annahme ist "richtig" in dem Sinne, dass Mono über _eine_ Implementierung des-Namespaces verfügt, wie jedoch Unterpunkte \#2 und \#3 der "Details zu den drei Fehler Ebenen" oben erläutert wird, ist die Implementierung derzeit unvollständig. Die richtige Lösung besteht also darin, dass das Mono-Team [Fehler 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) und [Fehler 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)auflösen kann.

## <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. .
