---
title: Tipps zur Problembehandlung für Xamarin.iOS
description: Dieses Dokument enthält verschiedene Tipps für die Problembehandlung bei der Entwicklung von Xamarin.iOS-Anwendungen nützlich. Es wird beschrieben, Fehlermeldungen sowie andere potenzielle Probleme.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2018
ms.openlocfilehash: 1a98cf854ffdd1d4904981f85fd8e33ad486743c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417373"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Tipps zur Problembehandlung für Xamarin.iOS 

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS kann nicht System.ValueTuple aufgelöst werden.

Dieser Fehler tritt aufgrund einer Inkompatibilität mit Visual Studio.

- **Visual Studio 2017 Update 1** (Version 15.1 oder früher) ist nur mit kompatibel **System.ValueTuple NuGet 4.3.0** (und früher).

- **Visual Studio 2017 Update 2** (Version 15.2 oder höher) ist nur kompatibel mit der **System.ValueTuple NuGet 4.3.1** oder höher.

Wählen Sie die richtige System.ValueTuple NuGet, der mit Ihrer Visual Studio 2017-Installation entspricht.


## <a name="receiving-error-retrieving-update-information-error-message"></a>Empfangen die Fehlermeldung "Fehler beim Abrufen von Informationen zu"

Beim Versuch der Aktualisierung der Software und diese Fehlermeldung angezeigt wird, versuchen Sie es neu zu starten Ihrer IDE und Abmelden und wieder zurück in Ihrem Konto in der IDE.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Wie erstelle ich Outlets oder Aktionen mit Interface Builder?

Mit der Einführung des Designers Xamarin für iOS in Visual Studio für Mac und Visual Studio können Entwickler mit Xamarin.iOS jetzt zum Erstellen einer Benutzeroberflächenautomatisierungs mithilfe von Storyboards und .xibs nutzen. Finden Sie in der [Hallo, iOS](~/ios/get-started/hello-ios/index.md) Anleitungen für die Weitere Informationen finden Sie unter den Designer.

Sie können auch im Apple-verweisen [Outlet](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) und [Aktionen](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) Anleitungen für die Weitere Informationen zur Verwendung von Outlets und Aktionen in IB.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>Löst eine NotSupportedException aus System.Text.Encoding.GetEncoding

Möglicherweise verwenden Sie eine Codierung, die nicht standardmäßig hinzugefügt wird. Überprüfen Sie die [Internationalisierung](~/ios/app-fundamentals/localization/index.md) Seite, um Informationen zum Hinzufügen von Unterstützung für weitere Codierung.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (sonstige)

Das Element wurde wahrscheinlich durch den Linker entfernt, und daher nicht in der Assembly zur Laufzeit vorhanden.  Es gibt mehrere Lösungsansätze:

- Hinzufügen der [ `[Preserve]` ](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) -Attribut auf den Member.  Dadurch wird verhindert, dass den Linker entfernt.
- Beim Aufrufen [ **Mtouch**](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29), verwenden Sie die **- Nolink** oder **- Linksdkonly** Optionen:
  - **-Nolink** deaktiviert alle Verknüpfungen.
  - **-Linksdkonly** wird nur an link Xamarin.iOS bereitgestellte Assemblys, wie z. B. **xamarin.ios.dll**, und gleichzeitig alle Typen in Assemblys benutzererstellte (ie. Ihre app-Projekte).

Beachten Sie, dass die Assemblys verknüpft sind, sodass die resultierende ausführbare Datei kleiner ist. Daher kann das Deaktivieren von Verknüpfungen zu einer größeren ausführbaren Datei als erwünscht ist, führen.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>Sie sind ein ModelNotImplementedException erhalten.

Wenn Sie diese Ausnahme erhalten, bedeutet dies, dass Sie die Basisklasse aufrufen. -Methode () für eine Klasse, die ein Modell überschreibt. Sie müssen nicht die Basismethode für Modelle in einer Klasse aufrufen (Hierbei handelt es sich um Klassen, die mit dem [Model]-Attribut gekennzeichnet sind).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Diese Klasse ist nicht als Schlüssel-Wert-Codierung kompatible für den Schlüssel XXXX

Wenn Sie diese Fehlermeldung beim Laden einer NIB-Datei bedeutet, dass den Wert, den XXXX für Ihre verwaltete Klasse nicht gefunden wurde. Dies bedeutet, dass Sie eine Deklaration wie folgt fehlt:

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

Die oben genannten Definition wird automatisch generiert von Visual Studio für Mac für alle XIB-Dateien, die Sie in Visual Studio für Mac im Hinzufügen der `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` Datei.

Darüber hinaus werden die Typen, die mit dem obigen Code müssen eine Unterklasse von [NSObject](xref:Foundation.NSObject).  Wenn der enthaltende Typ innerhalb eines Namespace ist, sollten sie außerdem besitzen eine [[registrieren]](xref:Foundation.RegisterAttribute) Attribut, das einen Namen ohne Namespace enthält (wie Interface Builder Namespaces in Typen unterstützt wird):

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Unbekannte Klasse XXXX in Interface Builder-Datei

Dieser Fehler wird generiert, wenn Sie eine Klasse in Ihren Interface Builder-Dateien definieren, aber Sie nicht die tatsächliche Implementierung für die sie in bieten Ihrem C# Code.

Sie benötigen, fügen Sie Code wie folgt hinzu:

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: Es wurde kein Konstruktor für Foo.Bar::ctor(System.IntPtr) gefunden.

Dieser Fehler wird zur Laufzeit ausgelöst, wenn der Code versucht, eine Instanz der Klassen zu instanziieren, die aus Ihrer Interface Builder-Datei, auf die verwiesen wird. Dies bedeutet, dass Sie vergessen haben, einen Konstruktor hinzufügen, der ein einzelnes IntPtr als Parameter akzeptiert.

Der Konstruktor mit einem IntPtr-Handles wird verwendet, um verwaltete Objekte mit ihren nicht verwaltete Darstellungen zu binden.

Um dieses Problem zu beheben, fügen Sie der Foo.Bar-Klasse die folgende Codezeile hinzu:

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>Typ {"Foo"} enthält keine Definition für `GetNativeField' and no extension method `GetNativeField "des Typs {" Foo "} gefunden

Wenn Sie diese Fehlermeldung erhalten, die sich in den Designer generierten Dateien (*. xib.designer.cs), es bedeutet, dass zwei Möglichkeiten:

 **(1) fehlt der partiellen Klasse oder Basisklasse**

Die vom Designer generierten partiellen Klassen müssen entsprechende partielle Klassen im Benutzercode, der eine Unterklasse von erben `NSObject`häufig `UIViewController`. Stellen Sie sicher, dass Sie eine solche Klasse für den Typ verfügen, die den Fehler erhalten.

 **(2) Standardnamespaces geändert**

Der Designer-Dateien werden unter Verwendung Ihres Projekts Standardeinstellungen-Namespace generiert. Wenn Sie diese Einstellungen geändert oder das Projekt umbenannt haben, möglicherweise die generierten partiellen Klassen nicht mehr im selben Namespace wie ihre äquivalente Benutzercode.

Namespace-Einstellungen finden Sie in das Dialogfeld "Projektoptionen". Der Standardnamespace befindet sich der **Allgemein-Einstellungen für die Main >** Abschnitt. Wenn es leer ist, ist der Name des Projekts als Standardwert verwendet. Erweiterte Namespaceeinstellungen finden Sie in der **Quellcode -> .NET Naming Policies** Abschnitt.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Warnung für Aktionen: Die private Methode "Foo" wird nie verwendet. (CS0169)

Aktionen für Interface Builder-Dateien sind mit den Widgets durch Reflektion zur Laufzeit verbunden, damit diese Warnung erwartet wird.

Sie können "#pragma-Warnung deaktivieren 0169" "#pragma-Warnung aktivieren 0169", um Ihre Aktionen sollten Sie diese Warnung nur für diese Methoden zu unterdrücken oder 0169 auf das Feld "Ignore-Warnungen" in den Compileroptionen hinzufügen, wenn es für Ihr gesamtes Projekt (nicht deaktiviert werden soll empfohlen).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>Fehler bei der Mtouch mit folgender Meldung: Assembly kann nicht geöffnet werden kann ' / path/to/yourproject.exe "

Wenn Sie diese Fehlermeldung angezeigt wird, ist im Allgemeinen das Problem, dass der absolute Pfad zu Ihrem Projekt ein Leerzeichen enthält. Dies wird in einer zukünftigen Version von Xamarin.iOS behoben werden, jedoch können Sie das Problem umgehen, indem Sie das Projekt in einen Ordner ohne Leerzeichen verschieben.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Ihre sqlite3-Version ist veraltet: Aktualisieren Sie mindestens auf v3.5.0!

Dies geschieht, wenn Sie Folgendes tun:

1.  Verwenden Sie Mono.Data.Sqlite
1.  Verwenden von Mac OS X Leopard (10.5)
1.  Führen Sie die app im Simulator an.


Das Problem besteht darin, dass Mono OS X-Datei übernommen wird `libsqlite3.dylib`, nicht die iPhoneSimulator des `libsqlite3.dylib` Datei. Ihre app *wird* arbeiten an das Gerät, aber nicht nur Ihre Simulator.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Stellen Sie bereit zum Gerät tritt der Fehler "System.Exception": AMDeviceInstallApplication returned 3892346901

Dieser Fehler weist darauf hin, dass die Code-signing-Konfiguration für Ihr Zertifikat/Bündel-Id nicht das Bereitstellungsprofil auf dem Gerät installierten übereinstimmt.  Vergewissern Sie sich, Sie haben das richtige Zertifikat ausgewählt, die in den Projektoptionen Bundle-Signierung iPhone ->, und die richtige Paket-Id, die in den Projekteigenschaften angegeben -> iPhone-Anwendung

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Vervollständigung von Code funktioniert in Visual Studio für Mac nicht

Stellen Sie sicher, dass Sie die neueste Version von Visual Studio für Mac und Xamarin.iOS verwenden

Wenn das Problem weiterhin besteht, wenden Sie [einen Fehler](http://monodevelop.com/Developers#Reporting_Bugs), Anfügen der **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{Zeitstempel} .log**, und **Komponenten – {Zeitstempel} .log** Protokolldateien.

Wenn alle anderen Versuche fehlschlagen, können Sie versuchen, entfernen den Code-Abschluss-Cache, damit sie erneut generiert wird:

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Achten Sie darauf, dass Sie mit diesem Befehl richtig eingeben. Alternativ könnten Sie wichtige Dateien versehentlich entfernen.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Visual Studio für Mac stürzt ab, wenn Sie Text kopieren

Die beliebte Mac QuickSilver, Google-Symbolleiste und Startbereich können Zwischenablage-Funktionen, die Visual Studio für Mac Speicher beschädigt. Unter "die Optionen" können Sie Visual Studio für Mac als Prozess auflisten, die, denen Sie nicht beeinträchtigen sollte.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio für Mac meldet die Mono-2.4 erforderlich

Wenn Sie Visual Studio für Mac aufgrund von einem kürzlich veröffentlichten Update aktualisiert, und wenn Sie versuchen, starten Sie ihn erneut es meldet die Mono-2.4 wird nicht vorhanden, müssen Sie, lediglich [upgrade Ihrer Installation von Mono 2.4](http://www.go-mono.com/mono-downloads/download.html).  

Mono 2.4.2.3_6 behebt einige wichtige Probleme, die verhindert, Visual Studio für Mac ausgeführt wird zuverlässig in einigen Fällen blockiert Visual Studio für Mac, beim Start oder verhindert, dass die vervollständigungsdatenbank Code generiert werden.

Wenn Sie den neue Mono installieren, wird Visual Studio für Mac gestartet, wie erwartet.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Die Assertion am... /.. /.. /.. /Mono/Metadata/Generic-Sharing.c:704, Bedingung 'Oti"nicht erfüllt

Wenn Sie die folgenden stapelüberwachung erhalten:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Es bedeutet, dass Sie eine statische Bibliothek mit Thumb-Code kompiliert werden, in Ihr Projekt verknüpft sind. IPhone SDK-Version 3.1 (oder höher zum Zeitpunkt der artikelerstellung) Apple führte einen Fehler in ihren Linker, beim Verknüpfen von nicht-Thumb-Code (Xamarin.iOS) mit Thumb-Code (die statische Bibliothek). Sie müssen zur Verknüpfung mit einer nicht-Thumb-Version Ihrer statischen Bibliothek aus, um dieses Problem zu beheben.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException: Es wird versucht, JIT-Kompilierung kompilieren (Wrapper verwaltet zu verwaltet)-Methode Foo[]:System.Collections.Generic.ICollection'1.get_Count)

Das []-Suffix gibt an, dass Sie oder der Klassenbibliothek sind Aufrufen einer Methode für ein Array über eine generische Auflistung, z. B. "IEnumerable" <>, ICollection <> oder IList <>. Dieses Problem zu umgehen können Sie explizit erzwingen, dass des AOT-Compilers eine solche Methode durch Aufrufen der Methode selbst einschließen, und sicherstellen, dass dieser Code ausgeführt wird, bevor der Aufruf, der die Ausnahme ausgelöst hat. In diesem Fall könnten Sie Folgendes schreiben:

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Die erzwingt, dass des AOT-Compilers die Get_Count-Methode enthalten.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio für Mac-Quellen-Editor ist sehr langsam.

Manchmal wird die Visual Studio für Mac-Quellen-Editor extrem langsam, nicht mehr reagiert. für einige Sekunden zwischen der Eingabe von Zeichen angezeigt werden.

Dieses Problem tritt sehr selten und extrem schwer zu reproduzieren: es in der Regel nicht reproduzieren auf demselben Computer nach dem Neustart von Visual Studio für Mac. Aus diesem Grund würden wir es uns freuen, wenn Sie könnten einige Debuggen Schritte durchführen, vor dem Neustart von Visual Studio für Mac und die Ergebnisse an uns senden.

1.  Versuchen Sie die Registerkarte "Editor" zu schließen und erneut öffnen. Dauert es ein wenig bearbeiten oder verschiebt die Einfügemarke um ein, bis die Verlangsamung erneut auftritt?
1.  Deaktivieren Sie "Balken Sync" mit dem Tool für Entwickler "Quartz Debug" (finden Sie mithilfe von Spotlight), und überprüfen Sie, ob die Quell-Editor-Leistung auf Normal zurückgesetzt wird.
1.  Versuchen Sie es wiederholen Schritt (1), mit dem Balken Synchronisierung immer noch deaktiviert.
1.  Wenn der Editor für mehr als ein paar Sekunden stürzt ab, versuchen Sie es ausführen "Killall-beenden [Visual Studio für Mac]" in einem Terminal aus, während es hängen geblieben ist. Es kann schwierig sein, Zeitpunkt den Kill-Befehl ausgeführt wird, während der Editor hängen geblieben ist, aber es ist wichtig, zu diesem Zweck, da der Befehl zwingt, Mono, um das stapelverfolgungen aller Threads in die MD-Protokoll zu schreiben, die wir verwenden können, welcher Zustand zu ermitteln, die Threads in sind, während die XS hängen geblieben ist.



Fügen Sie die Protokolle XS **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{Zeitstempel} .log**, und **Komponenten – {Zeitstempel} .log**(in älteren Versionen von XS/MonoDevelop, senden Sie einfach **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **HINWEIS: Das oben angegebene Problem wurde in XS 2.2 endgültige behoben.**

## <a name="compiled-application-is-very-large"></a>Kompilierte Anwendung ist sehr groß.

Zur Unterstützung des Debuggens, enthalten die Debug-Builds zusätzlichen Code. Projekte, die im Releasemodus erstellt, einem Bruchteil der Größe sind.

Ab Xamarin.iOS 1.3 Debug enthalten Builds die debugging-Unterstützung für jede einzelne Komponente von Mono (jede Methode in jeder Klasse Frameworks).  

Mit Xamarin.iOS 1.4 eine Einführung einer eine differenzierte Methode für das Debuggen, ist die Standardeinstellung nur Debuggen Instrumentierung für Code und Ihre Bibliotheken bereitstellen, und dies nicht für alle der [Mono-Assemblys](~/cross-platform/internals/available-assemblies.md) (dies weiterhin möglich sein, aber Sie müssen sich für diese Assemblys Debuggen).

## <a name="installation-hangs"></a>Installation hängt

Sowohl Mono und Xamarin.iOS-Installationsprogramme hängen bleiben, wenn der iPhone-Simulator ausgeführt werden. Dieses Problem ist nicht beschränkt auf Mono oder Xamarin.iOS, ein permanentes Problem handelt es sich für jede Software, die versucht, die Software auf MacOS Schnee Leopard installieren, wenn das iPhone Simulators bei der Installation ausgeführt wird.

Stellen Sie sicher, dass Sie die iPhone-Simulator zu beenden, und wiederholen Sie die Installation.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>Nicht genügend Trampolines vom Typ 0

Wenn Sie diese Meldung erhalten, während der Ausführung von Gerät, können Sie weiterer geben erstellen 0 Trampolines (Typ spezifische) ändern Ihr Projekt Optionen "iPhone Build"-Abschnitt.  Sie möchten hinzufügen, dass die zusätzlichen Argumente für das Gerät Ziele zu erstellen:

 `-aot "ntrampolines=2048"`

Die Standardanzahl von Trampolines ist 1024.  Versuchen Sie diese Zahl erhöhen, bis man genug für Ihre Anwendung.

## <a name="ran-out-of-trampolines-of-type-1"></a>Nicht genügend Trampolines vom Typ 1

Wenn Sie die häufige Verwendung rekursiver Generika vornehmen, können Sie diese Meldung auf Gerät erhalten.  Sie können weitere Typ 1 Trampolines (Typ RGCTX) erstellen, durch Ändern Ihrer Projekt-Optionen "iPhone Build"-Abschnitt.  Sie möchten hinzufügen, dass die zusätzlichen Argumente für das Gerät Ziele zu erstellen:

 `-aot "nrgctx-trampolines=2048"`

Die Standardanzahl von Trampolines ist 1024.  Versuchen Sie diese Anzahl erhöhen, bis man genug für die Verwendung von Generika.

## <a name="ran-out-of-trampolines-of-type-2"></a>Nicht genügend Trampolines vom Typ 2

Wenn Sie intensiv Schnittstellen verwenden, können Sie diese Meldung auf Gerät erhalten.
Sie können weiterer Geben Sie 2 Trampolines (Typ IMT Thunks) erstellen, indem Sie Ihr Projekt Optionen "iPhone Build" Abschnitt ändern.  Sie möchten hinzufügen, dass die zusätzlichen Argumente für das Gerät Ziele zu erstellen:

 `-aot "nimt-trampolines=512"`

Die Standardanzahl von IMT Thunk Trampolines ist 128.  Versuchen Sie diese Zahl erhöhen, bis man genug für die Verwendung von Schnittstellen.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Debugger kann keine Verbindung mit dem Gerät herstellen

Beim Starten des Debuggens einer Gerätekonfiguration, sehen Sie den Debugger angezeigt wird, ein Dialogfeld gibt an, dass es für die Anwendung eine Verbindung herzustellen versucht. Es gibt verschiedene Gründe, die der Debugger möglicherweise nicht an die Anwendung eine Verbindung herstellen können, verwenden Sie abhängig vom Modus eine Verbindung herstellen (USB oder WLAN).

 **Wenn das Gerät und dem Debuggerhost in unterschiedlichen Netzwerken befinden**, eine Firewall oder ein privates Netzwerk möglicherweise verhindert die Anwendung eine Verbindung mit dem Debuggerhost im WLAN-Modus herstellen.

 **Visual Studio für Mac ist möglicherweise nicht in der Lage, Fragen die richtige IP-Adresse des Hosts**. In WiFi Modus Visual Studio für Mac kann der Anwendung alle IP-Adressen gefunden werden kann von dem Host, und die Anwendung versucht, alle um festzustellen, ob es diese verwenden kann, um die Verbindung mit Visual Studio für Mac.

 **Ein anderes Gerät ist mit einem USB-Anschluss auf dem Host verbunden.** In einigen Fällen, die andere Geräte mit USB verbunden wurden Ports auf dem Host als irgendwie gestört, im Modus für USB-Debuggen.

Wenn entweder "WiFi" oder "USB-Modus nicht ordnungsgemäß funktioniert, können Sie einfach versuchen die andere: Öffnen Sie die Einstellungen in Visual Studio für Mac, wechseln Sie zu der Seite" Einstellungen/Debugger/iPhone-Debugger "und schalten Sie das Kontrollkästchen"Debuggen von iOS-Geräte über WLAN anstelle über USB".   Wenn keines von beiden funktioniert, finden Sie weitere Informationen über den Fehler in der Konsole des Geräts im ausführlichen Modus (durch Hinzufügen von aktiviert "-V - V - V" auf die weitere Mtouch-Argumente in den Projektoptionen).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>134 Mtouch Fehler mit folgender Meldung:

Dieser Fehler kann ausgelöst werden, wenn Sie versuchen, die mit - Nolink auf den Xamarin.iOS-1.4-Stil, der Releases zu erstellen. Sie können diesen Fehler umgehen, durch die Angabe der zusätzlichen Argumente in der Projektkonfiguration Monodevelop.

Fügen Sie das argument

 `-nosymbolstrip`

und das Problem sollte aufgelöst werden.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Distribution-Identität wird nicht in Visual Studio für Mac-Projekt, die Optionen zum codesignieren angezeigt.

Visual Studio für Mac, 2.2 hat einen Fehler, der bewirkt, dass es keine verteilungszertifikate zu erkennen, die kein Komma enthalten. Aktualisieren Sie Visual Studio für Mac ab 2.2.1.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Fehler "AFCFileRefWrite zurückgegeben: 1" beim Hochladen

Beim Hochladen einer app auf Ihrem Gerät erhalten Sie möglicherweise eine Fehlermeldung "AFCFileRefWrite zurückgegeben: 1". Dies kann vorkommen, wenn Sie eine Datei mit der Länge 0 (null) haben.

## <a name="error-mtouch-failed-with-no-output"></a>Fehler "Mtouch bei der keine Ausgabe"

Fehl, wenn die aktuelle Version von Xamarin.iOS- und Visual Studio für Mac den Namen des Projekts oder das Verzeichnis, in dem die Projektmappe oder das Projekt gespeichert werden, Leerzeichen enthalten.
Gehen Sie wie folgt vor, um das Problem zu beheben:


-  Stellen Sie sicher, dass weder das Projekt oder das Verzeichnis, wo sie gespeichert ist, ein Leerzeichen enthält.
-  Die "Main Projekteinstellungen" Stellen Sie sicher, dass der Projektname keine Leerzeichen enthält.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>"Fehler: die Binärdatei, die Sie hochgeladen haben, ungültig. Eine Vorabversion Beta-Version des SDK wurde verwendet, um die Anwendung erstellen"

Dieser Fehler tritt normalerweise mit einem Projekt, die in der iPad-Entwicklung gestartet wurde, bevor Xamarin.iOS 2.0.0 veröffentlicht wurde, liegt vermutlich einige Schlüssel in Ihrer Datei "Info.plist", z.B.:

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Dieses Schlüsselpaar sollten entfernt werden, wie in Visual Studio für Mac es für Sie automatisch behandelt.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Fehler "eine Vorabversion Beta-Version des SDK wurde verwendet, um die app erstellen"

(Die von Ed Anuff beigetragen wurden)

Führen Sie folgende Schritte aus:

-  Ändern Sie die SDK-Version in der iPhone-Build, der Version 3.2 oder iTunes connect es beim Hochladen abgelehnt, da eine kompatible iPad-app mit einer SDK-Version, die kleiner als 3.2 erstellt erkannten
-  Erstellen Sie eine benutzerdefinierte Datei "Info.plist" für das Projekt, und explizit MinimumOSVersion auf 3.0 darin festgelegt.   Dies überschreibt die MinimumOSVersion 3.2-Wert, der von Xamarin.iOS festgelegt.   Wenn Sie dies nicht tun, wird die app nicht auf einem iPhone ausgeführt werden können.
-  Neu erstellen "," Zip "und" Upload in iTunes connect.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>Ausnahmefehler: System.Exception: Fehler beim Suchen der Selektor SomeSelector: auf {Type}

Diese Ausnahme wird durch eines von drei Dingen verursacht werden:


1.  Sie haben eine Auswahl, um die Objective-C-Laufzeit bereitgestellt, ohne dass das entsprechende [Export]-Attribut an eine Methode angewendet
1.  Sie haben vollständigen Verknüpfung, und das Attribut [Preserve] nicht angewendet, für die [Export]-Ed-Methode.
1.  Sie haben das [Export]-Attribut auf eine private Methode in einem geerbten Typ angewendet.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs-Datei wird nicht aktualisiert werden.

Gab es ein Fehler in Xamarin Studio 2.4, die nicht mit der Datei MainWindow.xib.designer in neuen Projekten der MainWindow.xib Dateigruppe verursacht hat. Dies bedeutete, dass die Designer-Code für diese bestimmte Datei nicht aktualisiert werden sollen.

Dieses Problem wurde behoben, in der Version von Visual Studio für Mac, die in der integrierten Aktualisierung verfügbar ist damit stellen Sie sicher Sie die neuere Version verwenden.

Sie können vorhandene Projekte beheben, indem Sie entfernt (nicht löschen) Xib und die Designer-Datei, Sichern sie hinzufügen. Dies sollten die Dateien ordnungsgemäß erneut gruppiert werden.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>Uialertview-Element oder UIActionSheet verschwinden, nach dem Erstellen

Wenn Sie Code wie verfügen:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

das Objekt "ActionSheet" befindet sich wie eine temporäre Variable in der Funktion, und sobald die Funktion beendet wird, das Objekt ist für die Garbagecollection, damit es am Ende der Garbage collection.

Um dieses Problem zu beheben, müssen Sie einen Verweis auf "ActionSheet" außerhalb Ihrer Methode an, an einer beliebigen Stelle, die sich außerhalb der Methode befinden wird.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Projekt immer wird in der iPad-Simulator

Das iPhone SDK 4.0-Installationsprogramm installiert, 2 SDKs – das 3.2 SDK zum Erstellen von nur-iPad-apps, und das 4.0-SDK zum Erstellen von iPhone und universelle apps verwendet wird. Außerdem werden ein 3.2-Simulator, der nur auf einem iPad simuliert, und ein 4.0 Simulator, der simuliert, iPhone oder iPhone 4 installiert. Alle älteren SDKs und-Simulatoren werden entfernt.

Visual Studio für Mac Build iPhone Projektoptionen enthalten eine Einstellung für die SDK-Version, die verwendet werden, bei der Erstellung Ihrer app. Mit dieser Einstellung finden Sie in **Build-Projektoptionen > -> iPhone Build**.

Neue Projekte in Visual Studio für Mac das älteste installierte SDK als die Standardeinstellung für das SDK verwenden, und wenn das angegebene SDK nicht vorhanden ist, Visual Studio für Mac verwendet am ehesten zum Erstellen der app gefunden werden kann. Dies wurde erreicht, sodass Projekte nicht immer das neueste SDK erfordern würde. Dies führt jedoch zu derzeit die 3.2 SDK wird verwendet: was dazu führt, in der iPad-Simulator verwendet wird.

Zum Beheben dieses Problems mithilfe des SDK 4.0, wechseln Sie zu **Build-Projektoptionen > -> iPhone Build**>, und ändern Sie den SDK-Wert, den Wert "4.0" im Dropdownfeld. Sie müssen dies für jede Konfiguration und plattformkombination, die mithilfe der Dropdownlisten am Anfang des Bereichs zugegriffen.

Die SDK-Version sollte nicht mit der Einstellung "Minimum OS-Version" verwechselt werden.
Dieser Wert muss nicht mit dem Wert der SDK-Version überein – er wirkt sich auf die Mindestversion des Betriebssystems Ihrer app installieren, die älter sind als das SDK sein kann, solange Sie nur APIs, die in der älteren Betriebssystem vorhanden verwenden, oder schützen die Verwendung von neueren Features, die mit der Common Language Runtime OS Version Auschec KS. Sie sollten es auf die älteste Betriebssystemversion festlegen, auf denen die app zu testen.

Beachten Sie auch, dass die **Projekt -> iPhone-Simulatorziel**> Menü kann verwendet werden, um den Simulator auswählen, die standardmäßig verwendet wird, wenn ein Projekt ausführen/Debuggen. Darüber hinaus die **-Ausführung > Führen Sie mit**> im Menü kann verwendet werden, um einen bestimmten Simulator mit der Ausführung auswählen

## <a name="ibtool-returns-error-133"></a>Ibtool gibt Fehler 133 zurück.

Dies bedeutet, dass Sie XCode 4 installiert haben.   In XCode 4 die Tool-Ibtool wurde entfernt, da sie nicht mehr möglich ist, Ihrer XIB-Dateien mit einem eigenständigen Tool bearbeiten.

Wenn Sie die Verwendung von Interface Builder möchten, installieren Sie [XCode-Serie 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), verfügbar über den Apple Website.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Display-Bindung für den MIME-Typ kann nicht erstellt werden: application/vnd.apple -<wbr/>Schnittstelle-Builder"

Dieser Fehler tritt auf, wenn Sie versuchen, ein iPhone-Benutzeroberfläche aus einem nicht-iPhone-Projekt zu erstellen. Stellen Sie sicher, dass Sie beginnen mit einer iPhone/iPad-Lösung, es ist nicht möglich, einfach die iPhone-UI-Elemente zu einem iPhone/iPad-Projekt hinzuzufügen.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>Start-Absturz beim Ausführen in der iOS-simulator

Wenn Sie einen Absturz zur Laufzeit (SIGSEGV) innerhalb der Simulator sowie eine stapelüberwachung, die folgendermaßen aussieht erhalten:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. .then haben Sie wahrscheinlich eine (oder mehr) veraltete Assembly im Anwendungsverzeichnis Simulator. Solche Assemblys ist möglicherweise vorhanden, Apple iOS-Simulator hinzufügt und anschließend aktualisiert, Dateien, aber nie gelöscht. Klicken Sie dann die einfachste Lösung hierfür ist der Simulator im Menü "Zurückgesetzt und Inhalte und Einstellungen..." aus.   

**Warnung:** Dadurch werden alle Dateien, Anwendungen und Daten aus den Simulator entfernt.   Beim nächsten die Anwendung ausführen Visual Studio für Mac wird in der Simulator bereitstellen und es werden keine alten, veralteten-Assembly, die den Absturz verursachen.

## <a name="simulator-hangs-during-application-installation"></a>Simulator-Abstürze während der Anwendungsinstallation

Dies kann auftreten, wenn der Anwendungsnamen enthalten, eine "." (Punkt) in ihrem Namen.
Dies wird als den Namen der ausführbaren Datei in CFBundleExecutable - ist unzulässig, auch wenn es funktioniert in vielen Fällen (z. B. Geräte) können.

 * "Sollte der Wert keine Erweiterungen auf den Namen enthalten." - [https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Fehler: "0 x 43 Typ des benutzerdefinierten Attributs wird nicht unterstützt" beim Doppelklicken auf XIB-Dateien

Dies wird verursacht, versucht, eine XIB-Dateien geöffnet werden, wenn Umgebungsvariablen nicht ordnungsgemäß festgelegt sind. Dies sollte nicht der Fall mit normaler Nutzung von Visual Studio für Mac/Xamarin.iOS und erneutes Öffnen von Visual Studio für Mac unter/Programme sollte das Problem behoben.

Beim Versuch, aktualisieren Sie die Software und diese Fehlermeldung angezeigt wird, Sie-e-Mail *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Anwendung wird auf Simulator ausgeführt, aber ein Fehler auftritt, auf dem Gerät

Dieses Problem kann auf verschiedene Arten manifest und nicht immer einen konsistente Fehler erzeugen. Wenn die Anwendung eine XIB enthält, stellen Sie sicher, die **Buildvorgang** auf die XIB nastaven NA hodnotu **SchnittstelleDefinition**. Dies ist die Standard-Buildaktion für .xibs.

Um den Buildvorgang zu überprüfen, klicken Sie mit der rechten Maustaste auf die XIB-Datei, und wählen Sie **Buildvorgang**.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: Keine Daten sind für die Codierung 437 verfügbar

Wenn 3. Bibliotheken von Drittanbietern in Ihrer Xamarin.iOS-app einschließen möchten, erhalten Sie möglicherweise einen Fehler in der Form "System.NotSupportedException: Keine Daten ist für die Codierung 437" beim Versuch, kompilieren und Ausführen der app verfügbar. Z. B. Bibliotheken, wie z. B. `Ionic.Zip.ZipFile`, kann diese Ausnahme während des Betriebs.

Dies kann gelöst werden, öffnen Sie die Optionen für die Xamarin.iOS-Projekt, und möchte **iOS-Build** > **Internationalisierung** und die Überprüfung der **West** Internationalisierung.
