---
title: Verwenden von mtouch zum Bündeln von Xamarin.iOS-Apps
description: In diesem Dokument wird das Tool „mtouch“ beschrieben, das viele der Schritte durchführt, die für das Bündeln einer Xamarin.iOS-App, das Starten in einem Emulator und das Bereitstellen auf einem physischen Gerät erforderlich sind.
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9aaa79f929898f6765b97ab0a0c4a30a271d945a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784952"
---
# <a name="using-mtouch-to-bundle-xamarinios-apps"></a>Verwenden von mtouch zum Bündeln von Xamarin.iOS-Apps

iPhone-Anwendungen werden als Anwendungspakete zur Verfügung gestellt. Hierbei handelt es sich um Verzeichnisse mit der Erweiterung `.app`, die Ihren Code, Daten, Konfigurationsdateien sowie ein Manifest enthalten, welche vom iPhone verwendet werden, um Informationen zu Ihrer Anwendung zu sammeln.

Der Prozess zum Konvertieren einer ausführbaren .NET-Datei in eine Anwendung wird in der Regel vom Befehl `mtouch` gesteuert. Dabei handelt es sich um ein Tool, dass viele der zum Aktivieren der Anwendung in einem Bündel erforderlichen Schritte enthält. Dieses Tool wird auch verwendet, um Ihre Anwendung im Simulator zu starten und die Software auf einem echten iPhone oder iPod Touch bereitzustellen.

## <a name="detailed-instructions"></a>Ausführliche Anweisungen

Sehen Sie sich die Seite [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) des Leitfadens mit allen möglichen Einsatzbereichen des mtouch-Tools an.

## <a name="installation"></a>Installation

Auf einem Mac ist `mtouch` mit Xamarin.iOS gebündelt. Es kann im folgenden Verzeichnis gefunden werden:

**/Library/Frameworks/Xamarin.iOS-Framework/Versions/Current/bin**

Um `mtouch` sinnvoll nutzen zu können, müssen Sie dessen übergeordnetes Verzeichnis Ihrer `PATH`-Umgebungsvariable hinzufügen.  

Um dies beispielsweise in Bash tun zu können, muss dem Ende Ihrer **~/.bash_profile**-Datei folgende Zeile hinzugefügt werden:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> Um `mtouch` zu verwenden, sollten Sie sich nicht darauf verlassen, dass der symbolische Link **/Developer/MonoTouch/usr/bin**, der auf **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin** verweist, vorhanden ist. Diese symbolische Verknüpfung ist nur vorhanden, um die Kompatibilität mit älteren MonoTouch-Releases zu gewährleisten, die nicht unter **/Library/Frameworks/...** installiert wurden. Die Verknüpfung wird in zukünftigen Releases eventuell nicht mehr vorhanden sein.

## <a name="building"></a>Erstellung

Es gibt drei Möglichkeiten, Ihren Code mit dem `mtouch`-Befehl zu kompilieren:

-  Kompilieren für Simulatortests
-  Kompilieren für die Gerätebereitstellung
-  Bereitstellen Ihrer ausführbaren Datei auf dem Gerät


### <a name="building-for-the-simulator"></a>Entwickeln für den Simulator

Wenn Sie beginnen, ist das gebräuchlichste Szenario das Testen der Anwendung im Simulator, Sie werden also `mtouch -sim` verwenden, um den Code in ein Simulatorpaket zu kompilieren. Das sieht folgendermaßen aus:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Entwickeln für das Gerät

Um Software für das Gerät zu erstellen, erstellen Sie Ihre Anwendung mit der `mtouch -dev`-Option. Zusätzlich müssen Sie den Namen des Zertifikats zum Signieren Ihrer Anwendung bereitstellen. Das folgende Beispiel zeigt, wie die Anwendung für das Gerät erstellt wird:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

In diesem Fall verwenden wir das Zertifikat „iPhone Developer: Miguel de Icaza“, um die Anwendung zu signieren. Dieser Schritt ist sehr wichtig, andernfalls verweigert das physische Gerät das Laden der Anwendung.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Ausführen Ihrer Anwendung


### <a name="launching-on-the-simulator"></a>Starten auf dem Simulator

Das Starten auf dem Simulator ist sehr einfach, sobald Sie über ein Anwendungsbündel verfügen:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Wenn das `--sdkroot`-Flag nicht festgelegt ist, wird standardmäßig der Pfad „xcode-select“ festgelegt, und folgende Warnung ausgegeben:

> eg: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer 

Die Ausgabe der oben stehenden Befehlszeile sieht in etwa so aus:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Es wird dringend empfohlen, dass Sie auch ein Protokoll der Standardausgabe- und Standardfehlerdateien speichern, das Ihnen beim Debuggen hilft. Die Ausgabe von `Console.WriteLine` an `stdout` übergeben, und die Ausgabe von `Console.Error.WriteLine` sowie jede andere Laufzeitfehlermeldung wird an `stderr` übergeben.

Verwenden Sie dazu die Flags `--stdout` und `--stderr`:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Wenn Ihre Anwendung fehlschlägt, werden Ihnen die Ausgabe und der Fehler angezeigt, damit Sie das Problem ermitteln können.


### <a name="deploying-to-a-device"></a>Bereitstellen auf einem Gerät

Für eine Bereitstellung auf ein Gerät müssen Sie Ihr Gerät wie im Apple-Dokument [Managing Devices (Verwalten von Geräten)](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) beschrieben bereitstellen. Sobald Ihr Gerät ordnungsgemäß bereitgestellt wurde, können Sie den mtouch-Befehl verwenden, um eine kompilierte „.app“ auf Ihrem Gerät bereitzustellen. Dazu verwenden Sie folgenden Befehl:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Wenn das `--sdkroot`-Flag nicht festgelegt ist, wird standardmäßig der Pfad „xcode-select“ festgelegt, und folgende Warnung ausgegeben:

> eg: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer 

Diese Schritte werden in der Regel von Visual Studio für Mac ausgeführt.

## <a name="reference"></a>Referenz

Auf der Seite [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) im Leitfaden finden Sie Informationen zu den anderen Befehlszeilenoptionen.



## <a name="related-links"></a>Verwandte Links

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
