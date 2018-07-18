---
title: Abläufe beim Erstellen von iOS-Builds
description: In diesem Leitfaden erfahren Sie, wie Sie Ihre Anwendungen zeitlich steuern und wie Sie für alle Buildkonfigurationen Methoden für schnellere Builds verwenden können.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: df84e78709b0ff16087c4bb9816c5d45f6ec33ed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772356"
---
# <a name="ios-build-mechanics"></a>Abläufe beim Erstellen von iOS-Builds

_In diesem Leitfaden erfahren Sie, wie Sie Ihre Anwendungen zeitlich steuern und wie Sie Methoden für schnellere Builds verwenden können._

Das Entwickeln großartiger Anwendungen ist mehr als das bloße Schreiben von funktionierendem Code. Eine gut geschriebene App sollte Optimierungen aufweisen, die schnellere Builds mit kleineren und schnelleren Anwendungen ermöglichen. Diese Optimierungen führen nicht nur zu einer besseren Erfahrung für den Benutzer, sondern auch für Sie bzw. alle Entwickler, die am Projekt arbeiten. Es ist wichtig sicherzustellen, dass beim Umgang mit Ihrer App alles häufig zeitlich gesteuert wird. 

Denken Sie daran, dass die Standardoptionen sicher und schnell, aber nicht für jede Situation optimal sind. Darüber hinaus können viele Optionen den Entwicklungszyklus je nach Projekt verlangsamen oder beschleunigen. Beispielsweise nimmt natives Entfernen Zeit in Anspruch, aber wenn nur eine sehr geringe Größe erreicht wird, lässt sich der Zeitaufwand für das Entfernen nicht durch eine schnellere Bereitstellung zurückgewinnen. Andererseits kann natives Entfernen die App erheblich verkleinern, wodurch die Bereitstellung schneller erfolgt. Dies variiert je nach Projekt und kann nur durch Testen festgestellt werden.

Das Tempo von Xamarin-Builds kann auch durch verschiedene Kapazitäten und Fähigkeiten eines Computers beeinflusst werden, die die Leistung beeinflussen können: Prozessorleistung, Busgeschwindigkeiten, die Größe des physischen Speichers, Festplattengeschwindigkeit, Netzwerkgeschwindigkeit. Diese Leistungseinschränkungen sind nicht Gegenstand dieses Dokuments und liegen in der Verantwortung des Entwicklers.


## <a name="timing-apps"></a>Zeitsteuerung-Apps

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio für Mac

1. Klicken Sie auf **Visual Studio für Mac > Einstellungen...**
2. Wählen Sie in der linken Struktur **Projekte > Build** aus.
3. Legen Sie im rechten Bereich das Dropdownelement „Protokollausführlichkeit“ auf **Diagnose** fest: [![](ios-build-mechanics-images/image2.png "Festlegen der Ausführlichkeit des Protokolls")](ios-build-mechanics-images/image2.png#lightbox)
4. Klicken Sie auf **OK**.
5. Starten Sie Visual Studio für Mac neu.
6. Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
7. Zeigen Sie die Diagnoseausgabe im Bereich „Fehler“ (Ansicht > Bereiche > Fehler) an, indem Sie auf die Schaltfläche „Buildausgabe“ klicken.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

So aktivieren Sie die Diagnoseausgabe von MSBuild in Visual Studio

1. Klicken Sie auf **Extras > Optionen...**.
2. Wählen Sie in der linken Struktur **Projekte und Projektmappen > Erstellen und Ausführen** aus.
3. Legen Sie im rechten Bereich das Dropdownelement *Ausführlichkeit der MSBuild-Buildausgabe* auf **Diagnose** fest: [![](ios-build-mechanics-images/image2-vs.png "Festlegen der Ausführlichkeit der MSBuild-Buildausgabe")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. Klicken Sie auf **OK**.
5. Bereinigen Sie Ihr Paket, und erstellen Sie es erneut.
6. Die Diagnoseausgabe wird im Ausgabebereich angezeigt.

-----

## <a name="timing-mtouch"></a>Zeitliche Steuerung von mtouch

Um spezifische Informationen zum mtouch-Buildprozess anzuzeigen, übergeben Sie in Ihren **Projektoptionen** `--time --time` an die mtouch-Argumente. Die Ergebnisse finden Sie in der Buildausgabe, indem Sie den Task `MTouch` suchen:

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>Herstellen einer Verbindung von Visual Studio mit dem Buildhost

Die Xamarin-Tools funktionieren technisch auf jedem Mac, auf dem OS X 10.10 Yosemite oder höher ausgeführt werden kann. Allerdings können Entwicklererfahrungen und Buildzeiten durch die Leistung des Mac beeinträchtigt werden.

Im getrennten Zustand führt Visual Studio unter Windows nur die C#-Kompilierungsphase durch und versucht nicht, eine Verknüpfung oder AOT-Kompilierung auszuführen, die Anwendung in ein _.app_-Bundle zu packen oder das App Bundle zu signieren. (Die C#-Kompilierungsphase ist selten ein Leistungsengpass.) Versuchen Sie herauszufinden, wo in der Pipeline der Build verlangsamt wird, indem Sie direkt auf dem Mac-Buildhost in Visual Studio für Mac einen Build erstellen.


Darüber hinaus ist einer der häufigsten Gründe für Langsamkeit die Netzwerkverbindung zwischen dem Windows-Computer und dem Mac-Buildhost. Dies kann durch eine physische Einschränkung im Netzwerk, eine drahtlose Verbindung oder einen überlasteten Computer (z. B. einen Mac-in-the-Cloud-Dienst) verursacht werden.

## <a name="simulator-tricks"></a>Simulatortricks

 
Bei der Entwicklung mobiler Anwendungen ist die schnelle Bereitstellung von Code unerlässlich. Aus einer Vielzahl von Gründen, darunter Geschwindigkeit und fehlende Anforderungen an die Gerätebereitstellung, entscheiden sich Entwickler häufig für die Bereitstellung auf einem vorinstallierten Simulator oder Emulator. Für Hersteller von Entwicklertools ist die Entscheidung, einen Simulator oder Emulator zur Verfügung zu stellen, ein Kompromiss zwischen Geschwindigkeit und Kompatibilität. 

Apple stellt einen Simulator für die iOS-Entwicklung zur Verfügung, der Geschwindigkeit über Kompatibilität stellt, indem er eine weniger restriktive Umgebung für die Ausführung von Code schafft. Diese weniger restriktive Umgebung erlaubt es Xamarin, den JIT-Compiler (Just In Time) für den Simulator zu verwenden (im Gegensatz zu [AOT](~/ios/internals/architecture.md) auf einem Gerät), was bedeutet, dass der Build zur Laufzeit in nativen Code kompiliert wird. Da der Mac viel schneller als ein Gerät ist, ermöglicht dies eine bessere Leistung.

Der Simulator verwendet einen gemeinsam genutzten Anwendungsstarter, sodass der Starter wiederverwendet werden kann, anstatt jedes Mal neu erstellt zu werden, wie es auf dem Gerät erforderlich ist.

Unter Berücksichtigung der obigen Informationen bietet die nachstehende Liste einige Informationen zu den Schritten, die Sie beim Erstellen und Bereitstellen Ihrer App im Simulator durchführen müssen, um die beste Leistung zu erzielen.
 
### <a name="tips"></a>Tipps

- Für Builds: 
  - Deaktivieren Sie in „Projektoptionen“ die Option **PNG-Bilder optimieren**. Diese Optimierung ist für Builds im Simulator nicht notwendig.
  - Legen Sie den Linker auf **Nicht verknüpfen** fest. Das Deaktivieren des Linkers sorgt für Beschleunigung, da die Ausführung des Linkers viel Zeit in Anspruch nimmt.
  - Das Deaktivieren des gemeinsam genutzten Anwendungsstarters durch Verwendung des Flags `--nofastsim` führt dazu, dass Simulatorbuilds wesentlich langsamer werden. Entfernen Sie dieses Flag, wenn es nicht mehr benötigt wird.
  - Die Verwendung nativer Bibliotheken ist langsamer, da die gemeinsam genutzte Hauptausführungsdatei von simlauncher in solchen Fällen nicht wiederverwendet werden kann und für jeden Build eine anwendungsspezifische ausführbare Datei kompiliert werden muss.
- Für die Bereitstellung
  - Lassen Sie den Simulator nach Möglichkeit immer in Betrieb. Der Kaltstart des Simulators kann bis zu 12 Sekunden dauern.
- Weitere Tipps
  - Ziehen Sie das Erstellen dem Neuerstellen vor, da beim Neuerstellen vor dem Erstellen des Builds eine Bereinigung erfolgt. Das Bereinigen kann sehr lange dauern, da Verweise entfernt werden, die verwendet werden könnten.
  - Nutzen Sie die Tatsache aus, dass der Simulator die Sandbox nicht erzwingt. Wenn Sie große Ressourcen wie Videos oder andere Objekte in Ihr Projekt einbinden, kann es bei jedem Start der Anwendung im Simulator zu aufwändigen Dateikopiervorgängen kommen. Vermeiden Sie diese aufwändigen Vorgänge, indem Sie diese Dateien im Basisverzeichnis ablegen und in Ihrer Anwendung über den vollständigen Dateipfad auf sie verweisen.  
  - Verwenden Sie im Zweifelsfall das Flag `–time –time`, um Ihre Änderung zu messen.

Der folgende Screenshot zeigt, wie Sie in Ihren iOS-Optionen diese Optionen für den Simulator festlegen:

[![](ios-build-mechanics-images/image3.png "Festlegen der Optionen")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>Gerätetricks

Die Bereitstellung auf dem Gerät ist vergleichbar mit der Bereitstellung auf dem Simulator, da der Simulator eine kleine Teilmenge des Builds ist, der für das iOS-Gerät verwendet wird. Das Erstellen eines Builds für ein Gerät erfordert viele weitere Schritte, hat aber den Vorteil, dass es zusätzliche Möglichkeiten zur Optimierung Ihrer App bietet.

### <a name="build-configurations"></a>Buildkonfigurationen

Es gibt eine Reihe von Buildkonfigurationen, die bei der Bereitstellung von iOS-Apps zur Verfügung stehen. Es ist wichtig, ein gutes Verständnis der einzelnen Konfigurationen zu haben, um zu wissen, wann und warum Sie optimieren sollten.

 - Debug
  - Dies ist die Hauptkonfiguration, die während der Entwicklung einer App verwendet werden sollte und daher so schnell wie möglich sein sollte.
 - Release
  - Releasebuilds sind diejenigen, die an Ihre Benutzer ausgeliefert werden. Bei ihnen liegt der Schwerpunkt auf der Leistung. Wenn Sie die Releasekonfiguration verwenden, empfiehlt es die Verwendung des LLVM-Optimierungscompilers und Optimierung von PNG-Dateien.

 
Wichtig ist es auch, die Beziehung zwischen dem Erstellen des Builds und Bereitstellen zu verstehen. Die Bereitstellungszeit richtet sich nach der Größe der Anwendung. Eine größere Anwendung benötigt mehr Zeit zum Bereitstellen. Durch Minimieren der App-Größe können Sie die Bereitstellungszeit verkürzen.

Durch Minimieren der App-Größe verkürzt sich auch die Buildzeit. Das liegt daran, dass das Entfernen von Code aus der Anwendung weniger Zeit in Anspruch nimmt, als das native Kompilieren des Codes, der nicht verwendet werden soll. Kleinere Objektdateien bedeuten eine schnellere Verknüpfung, wodurch eine kleinere ausführbare Datei mit weniger zu erzeugenden Symbolen erstellt wird. Das Einsparen von Speicherplatz zahlt sich also doppelt aus, weshalb **SDK verknüpfen** der Standard für alle Gerätebuilds ist. 

> [!NOTE]
> Die Option **SDK verknüpfen** kann je nach verwendeter IDE als „Nur Framework-SDKs verknüpfen“ oder „Nur SDK-Assemblys verknüpfen“ angezeigt werden.
 

### <a name="tips"></a>Tipps

- Build: 
  - Das Erstellen einer einzelnen Architektur (z. B. ARM64) erfolgt schneller als das einer FAT-Binärdatei (z. B. ARMv7 + ARM64).
  - Vermeiden Sie die Optimierung von PNG-Dateien beim Debuggen.
  - Erwägen Sie, alle Assemblys zu verknüpfen. Optimieren Sie jede Assembly. 
  - Deaktivieren Sie die Erstellung von Debugsymbolen mit `--dsym=false`. Sie sollten sich jedoch im Klaren sein, dass das Deaktivieren dieser Option bedeutet, dass Absturzberichte nur auf dem Computer, auf dem die Anwendung erstellt wurde, durch Symbole dargestellt werden können, und nur dann, wenn aus der Anwendung kein nicht verwendeter Code entfernt wurde.

 
Die folgenden Dinge sollten vermieden werden:

- FAT-Binärdateien (Debuggen) 
- Deaktivieren des Linkers `–nolink` 
- Deaktivieren der Entfernung von nicht verwendetem Code 
  - Symbole `--nosymbolstrip` 
  - IL (Release) `--nostrip`.  
 
Weitere Tipps 

- Ziehen Sie wie beim Simulator das Erstellen dem Neuerstellen vor. 
  - AOT kompilierte Assemblys (Objektdateien) werden zwischengespeichert. 
- Debugbuilds dauern wegen der Symbole länger, da dsymutil ausgeführt wird. Und weil sie letztendlich größer werden, dauert das Hochladen auf Geräte länger. 
- Releasebuilds wenden standardmäßig eine IL-Entfernung auf die Assemblys an. Das dauert nur kurze Zeit, die wahrscheinlich zurückgewonnen wird, weil eine kleinere App auf dem Gerät bereitgestellt wird.
- Vermeiden Sie das Bereitstellen großer statischer Dateien bei jedem Build (Debug). 
  - Verwenden Sie „UIFileSharingEnabled“ (Info.plist). 
    - Objekte können einmal hochgeladen werden. 
- Verwenden Sie im Zweifelsfall das Flag `–time –time`, um Ihre Änderung zu messen.

Der folgende Screenshot zeigt, wie Sie in Ihren iOS-Optionen diese Optionen für den Simulator festlegen:

[![](ios-build-mechanics-images/image4.png "Festlegen der Optionen")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>Verwenden des Linkers

Beim Erstellen Ihrer Anwendung verwendet mtouch einen Linker für verwalteten Code, der Code entfernt, den die Anwendung nicht verwendet. Theoretisch ergeben sich dadurch kleinere und damit schnellere Builds. Weitere Informationen zum Linker finden Sie in der Anleitung [Verknüpfung unter iOS](~/ios/deploy-test/linker.md).

Beachten Sie beim Verwenden des Linkers die folgenden Punkte:

- Die Auswahl von **Nicht verknüpfen** für einen Gerätebuild bringt einen höheren Zeitaufwand mit sich und erzeugt auch eine größere App. 
  - Apple lehnt Apps ab, die die Größenbeschränkung überschreiten. Abhängig von `MinimumOSVersion` sind maximal 60 MB möglich. 
  - Der native ausführbare Datei wird dabei eingerechnet. 
  - Bei Simulatorbuilds ist die Verwendung von „Nicht verknüpfen“ schneller, da die JIT-Kompilierung zum Einsatz kommt (im Gegensatz zu AOT auf einem Gerät).
- „SDK verknüpfen“ ist die Standardoption.
- Das Verwenden von „Alle verknüpfen“ ist möglicherweise nicht sicher, insbesondere wenn Sie mit Code arbeiten, der nicht Ihr eigener ist, wie z.B. NuGet-Pakete oder Komponenten. Wenn Sie sich dafür entscheiden, Assemblys nicht zu verknüpfen, wird der gesamte Code aus diesen Diensten in Ihre Anwendung eingebunden, wodurch möglicherweise größere Apps entstehen. 
  - Wenn Sie jedoch **Alle verknüpfen** wählen, kann die Anwendung abstürzen, insbesondere wenn externe Komponenten verwendet werden. Der Grund dafür ist, dass einige Komponenten für bestimmte Typen Reflektion verwenden.
  - Statische Analyse und Reflektion funktionieren nicht zusammen. 

Die Tools können angewiesen werden, Dinge innerhalb der Anwendung zu behalten, indem das [`[Preserve]`-Attribut](~/ios/deploy-test/linker.md) verwendet wird. 

Wenn Sie keinen Zugriff auf den Quellcode haben oder er von einem Tool generiert wird und Sie ihn nicht ändern möchten, können Sie ihn trotzdem verknüpfen, indem Sie eine XML-Datei erstellen, die alle Typen und Member beschreibt, die erhalten bleiben müssen. Sie können dann das Flag `--xml={file.name}.xml` Ihren Projektoptionen hinzufügen, wodurch Code genau so verarbeitet wird, als ob Sie Attribute verwenden würden.


### <a name="partially-linking-applications"></a>Teilweises Verknüpfen von Anwendungen 

Es ist auch möglich, Anwendungen teilweise zu verknüpfen, um die Buildzeit Ihrer Anwendung zu optimieren:

- Verwenden Sie `Link All`, und überspringen Sie einige Assemblys. 
  - Die Optimierung der Anwendungsgröße geht teilweise verloren.
  - Es ist kein Zugriff auf den Quellcode erforderlich.
  - Beispiel: `--linkall --linkskip=fieldserviceiOS`.
 
- Verwenden Sie die Option `Link SDK` und das Attribut `[LinkerSafe]` für die benötigten Assemblys. 
  - Zugriff auf den Quellcode ist erforderlich.
  - Teilt dem System mit, dass die Assembly sicher zu verknüpfen ist und wie ein Xamarin SDK verarbeitet wird.
 
### <a name="objective-c-bindings"></a>Objective-C-Bindungen 

- Durch Verwenden des `[Assembly: LinkerSafe]`-Attributs für Ihre Bindungen können Sie Zeit sparen und die Größe reduzieren.

- SmartLink 
  - Erfolgt auf nativer Seite. 
  - Verwenden Sie das Attribut `[LinkWith (SmartLink=true)]`.
  - Dies hilft dem nativen Linker, nativen Code aus der Bibliothek zu entfernen, mit der Sie eine Verknüpfung herstellen. 
  - Beachten Sie, dass die dynamische Suche nach Symbolen in diesem Fall nicht funktioniert. 

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung wurde untersucht, wie eine iOS-Anwendung zeitlich gesteuert werden kann und welche Optionen zu berücksichtigen sind, die von der Buildkonfiguration und den Optionen des Projekts abhängen. 

<!-----
# Benchmarks

## Layer 1: building again after making modifications, but _without_ cleaning should be faster 
 
The app should build a bit more quickly if you have only made changes to a subset of the libraries and you do not clean the build before re-deploying. 
 
 
 
### Clean build time 
178 seconds 
 
 
### Build again (without cleaning) after making _no changes_ 
12.5 seconds 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
3 trials: 45 seconds, 43 seconds, 43 seconds 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
 
3 trials: 45 seconds, 45 seconds, 45 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
- Sales.Native.Core.IOS.Ext/ServiceInterfaces/AlertDialog/Dialog.cs 
- Sales.Native.Core.Tools.IOS.Ext/BaseViews/BaseNavigationViewController.cs 
- View.Common/Services/DataTransferResult.cs 
 
45 seconds 
 
 
 
 
 
 
## Layer 2: "app thinning" aka "device specific builds" 
 
The idea of "app thinning" is that the IDE will only build the 1 architecture needed for the specific device that you're deploying to (rather than _both_ 32-bit and 64-bit architectures). 
 
As of the latest "Xamarin 4" builds, you can now enable "app thinning" in Visual Studio via the "Project Options -> iOS Build -> Enable device-specific builds" setting. 
 
Or if you prefer you can achieve a similar result by changing the "Project Options -> iOS Build -> Advanced [tab] -> Supported architectures" to select just _one_ architecture (for example ARM64 if you are developing on a 64-bit device). 
 
 
 
(Caveat: I ran the following builds in Visual Studio for Mac on the Mac rather than on the command line.) 
 
### Clean build time without "device specific builds" 
177 seconds 
 
 
 
### Clean build time _with_ "device specific builds"  
2 trials: 106 seconds, 98 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 31 seconds, 31 seconds 
 
 
* * * 
 
 
## Using the same strategy, but explicitly setting "Supported architectures" to select ARM64 _only_ (rather than using "device specific builds") 
 
(These builds were again run on the command line using `xbuild`.) 
 
 
 
### Clean build time with "Supported architectures" set to ARM64 _only_ 
2 trials: 80 seconds, 91 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 26 seconds, 26 seconds 
 
 
 
 
 
[1] Mac system used for testing: MacBookAir5,2 
 
- 2.0 GHz Core i7 (I7-3667U) 
 
2 Cores with hyper-threading 
 
L2 Cache (per Core): 256 KB 
L3 Cache: 4 MB 
 
- Standard MacBook soldered-in solid-state storage 
 
- 8 GB RAM 
---->


## <a name="related-links"></a>Verwandte Links

- [Blogbeitrag](https://blog.xamarin.com/xamarin-ios-build-improvements/)
- [Verknüpfung unter iOS](~/ios/deploy-test/linker.md)
- [Benutzerdefinierte Linkerkonfiguration](~/cross-platform/deploy-test/linker.md)
