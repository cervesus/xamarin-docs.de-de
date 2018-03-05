---
title: mtouch
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b1b61e7ce1bae413f132cfe1e6c051a53b786f98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="mtouch"></a>mtouch


iPhone-Anwendungen werden als Anwendungspakete zur Verfügung gestellt. Hierbei handelt es sich um Verzeichnisse mit der Erweiterung `.app`, die Ihren Code, Daten, Konfigurationsdateien sowie ein Manifest enthalten, welche vom iPhone verwendet werden, um Informationen zu Ihrer Anwendung zu sammeln.

Der Prozess zum Konvertieren einer ausführbaren .NET-Datei in eine Anwendung wird in der Regel vom Befehl `mtouch` gesteuert. Dabei handelt es sich um ein Tool, dass viele der zum Aktivieren der Anwendung in einem Bündel erforderlichen Schritte enthält. Dieses Tool wird auch verwendet, um Ihre Anwendung im Simulator zu starten und die Software auf einem echten iPhone oder iPod Touch bereitzustellen.


## <a name="detailed-instructions"></a>Ausführliche Anweisungen

Sehen Sie sich die Seite [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) des Leitfadens mit allen möglichen Einsatzbereichen des mtouch-Tools an.


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
