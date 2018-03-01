---
title: Problembehandlung
ms.topic: article
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: d9859f4e705f74e490b50443870f71f049adeaf8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Problembehandlung

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS cannot resolve System.ValueTuple

Dieser Fehler tritt aufgrund einer Inkompatibilität mit Visual Studio.

- **Visual Studio 2017 Update 1** (15.1 oder eine ältere Version) ist nur mit kompatibel **System.ValueTuple NuGet 4.3.0** (oder älter).

- **Visual Studio 2017 Update 2** (Version 15.2 oder höher) ist nur kompatibel mit der **System.ValueTuple NuGet 4.3.1** oder höher.

Wählen Sie die richtige System.ValueTuple NuGet-Visual Studio-2017 Installationsumfang entspricht.


## <a name="receiving-error-retrieving-update-information-error-message"></a>Fehlermeldung "Fehler beim Abrufen von Informationen zum Update"

Beim Versuch der Aktualisierung der Software und diese Fehlermeldung angezeigt wird, wiederholen Sie dann einen Neustart der IDE und abgemeldet und wieder zurück in Ihrem Konto in der IDE.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Wie erstelle ich Steckdosen oder Aktionen mit Schnittstelle-Generator?

Mit der Einführung von der Xamarin-Designer für iOS in Visual Studio für Mac und Visual Studio können Entwickler Xamarin.iOS jetzt Erstellen einer Benutzeroberfläche über Storyboards und .xibs nutzen. Finden Sie in der [Hello, iOS](~/ios/get-started/hello-ios/index.md) Leitfäden an, um weitere Informationen finden Sie unter den Designer.

Sie können auch auf der Apple-verweisen [Nachrichtenplattform](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) und [Aktionen](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) Leitfäden an, um weitere Informationen zur Verwendung von Steckdosen und Aktionen im IB.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding throws NotSupportedException

Sie verwenden möglicherweise eine Codierung, die nicht standardmäßig hinzugefügt wird. Überprüfen Sie die [Internationalisierung](~/ios/app-fundamentals/localization/index.md) Seite, um zu erfahren, wie die Unterstützung für weitere Codierung hinzuzufügen.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (Sonstiges)

Das Element wurde wahrscheinlich durch den Linker entfernt, und daher nicht in der Assembly zur Laufzeit vorhanden.  Es gibt mehrere Lösungen dafür:

-  Hinzufügen der [[beibehalten]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) -Attribut auf den Member.  Dadurch wird verhindert, dass den Linker entfernen.
-  Beim Aufrufen von [Mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29) , verwenden Sie die **- Nolink** oder **- Linksdkonly** Optionen. -    **-Nolink** deaktiviert alle verknüpfen.
-    **-Linksdkonly** Xamarin.iOS bereitgestellten Assemblys nur z. B. Verknüpfen *monotouch.dll* oder xamarin.ios.dll.

Beachten Sie, dass Assemblys verknüpft sind, damit die resultierende ausführbare Datei kleiner ist. Daher kann durch Deaktivieren der Verknüpfung in einer größeren ausführbaren Datei als wünschenswert ist führen.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>Sie werden eine ModelNotImplementedException abrufen.

Wenn diese Ausnahme ausgelöst wird, bedeutet dies, dass es sich bei Aufrufen von Basis. Methode () für eine Klasse, die ein Modell überschreibt. Sie müssen nicht die Basismethode für Modelle in einer Klasse aufrufen (Hierbei handelt es sich um Klassen, die mit dem [Modell]-Attribut gekennzeichnet sind).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Diese Klasse kann nicht als Schlüssel-Wert-Codierung kompatibel für den Schlüssel XXXX

Wenn Sie diesen Fehler beim Laden einer NIB-Datei erhalten bedeutet, dass den Wert, den XXXX für Ihre verwaltete Klasse nicht gefunden wurde. Dies bedeutet, dass eine Deklaration wie folgt fehlen:

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

Die oben genannten Definition wird automatisch generiert von Visual Studio für Mac für alle XIB-Dateien, die Sie zu Visual Studio für Mac im Hinzufügen der `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` Datei.

Darüber hinaus muss die Typen, die den obigen Code enthält eine Unterklasse von [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).  Wenn der enthaltende Typ innerhalb eines Namespace ist, verfügt es sollte auch über eine [[registrieren]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Attribut, das Typnamen für einen ohne einen Namespace enthält (wie Schnittstelle-Generator Namespaces nicht in Typen unterstützt wird):

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Unbekannte Klasse XXXX in Benutzeroberflächen-Generator-Datei

Dieser Fehler wird generiert, wenn Sie eine Klasse in Ihrer Schnittstelle-Generator-Dateien definieren, aber Sie nicht die tatsächliche Implementierung dafür im C#-Code bieten.

Sie müssen Code wie folgt hinzufügen:

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: Es ist kein Konstruktor für Foo.Bar::ctor(System.IntPtr) gefunden.

Dieser Fehler wird zur Laufzeit ausgelöst, wenn der Code versucht, eine Instanz der Klassen zu instanziieren, die aus Ihrer Schnittstelle-Generator-Datei, auf die verwiesen wird. Das heißt, Sie haben vergessen, einen Konstruktor hinzufügen, der einen einzelnen IntPtr als Parameter akzeptiert.

Der Konstruktor mit eines IntPtr-Handles wird verwendet, um verwaltete Objekte mit ihren nicht verwalteten Darstellungen zu binden.

Um dieses Problem zu beheben, fügen Sie der Klasse Foo.Bar die folgende Codezeile hinzu:

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>Typ {Foo} enthält keine Definition für `GetNativeField' and no extension method `GetNativeField "des Typs {" Foo "} gefunden

Wenn Sie diese Fehlermeldung, in den Designer generierten Dateien erhalten (*. xib.designer.cs), es bedeutet, dass zwei Dinge:

 **(1) fehlt der partiellen Klasse oder Basisklasse**

Partiellen Klassen-Designer generierter benötigen entsprechende partielle Klassen im Benutzercode, die von einigen Unterklasse von erben `NSObject`, häufig `UIViewController`. Stellen Sie sicher, dass Sie eine solche Klasse für den Typ verfügen, die den Fehler bereitstellt.

 **(2) Standardnamespaces geändert**

Die Designer-Dateien werden mithilfe von Namespace-Standardeinstellungen Ihres Projekts generiert. Wenn Sie diese Einstellungen geändert oder das Projekt umbenannt haben, möglicherweise generierten partiellen Klassen nicht mehr im selben Namespace wie ihre Benutzercode-Gegenstücke.

Namespace-Einstellungen können im Dialogfeld Projektoptionen gefunden werden. Der Standardnamespace befindet sich der **Allgemein -> Main Einstellungen** Abschnitt. Wenn es leer ist, wird der Name des Projekts als Standardwert verwendet. Erweiterte Namespaceeinstellungen finden Sie in der **Quellcode -> .NET Naming Policies** Abschnitt.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Warnung für Aktionen: die private Methode "Foo" wird nie verwendet. (CS0169)

Aktionen für Schnittstelle-Generator-Dateien sind mit den Widgets mittels Reflektion zur Laufzeit verbunden, damit diese Warnung erwartet wird.

Verwenden Sie "#pragma Warning deaktivieren 0169" "#pragma Warning aktivieren 0169", um Ihre Aktionen sollten Sie diese Warnung nur für diese Methoden zu unterdrücken oder 0169 in das Feld "Warnungen ignorieren" Compileroptionen hinzufügen, wenn es für das gesamte Projekt (nicht deaktiviert werden soll empfohlen).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>Mtouch Fehler mit folgender Meldung: Assembly kann nicht geöffnet werden "/ path/to/yourproject.exe"

Wenn Sie diese Fehlermeldung angezeigt wird, ist im Allgemeinen das Problem der absolute Pfad zu Ihrem Projekt ein Leerzeichen enthält. Dies wird in einer zukünftigen Version von Xamarin.iOS behoben werden, aber Sie können das Problem umgehen, durch das Projekt in einen Ordner ohne Leerzeichen zu verschieben.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Ihre sqlite3-Version sind veraltet: Aktualisieren Sie auf mindestens v3.5.0!

Dies geschieht, wenn Sie Folgendes vornehmen:

1.  Verwenden Sie Mono.Data.Sqlite
1.  Verwenden Sie die Mac OS X Leopard (10.5)
1.  Führen Sie Ihre app im Simulator an.


Das Problem besteht darin, dass der Verlagerung Mono Einrichten der OS X `libsqlite3.dylib`, nicht die iPhoneSimulator des `libsqlite3.dylib` Datei. Ihre app *wird* für das Gerät, jedoch nicht nur Ihre Simulator.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Bereitstellen für Gerät schlägt fehl mit System.Exception: AMDeviceInstallApplication zurückgegebene 3892346901

Dieser Fehler weist darauf hin, dass die Konfiguration Signieren von Code für die Zertifikat-Bündel-Id nicht das Bereitstellungsprofil auf Ihrem Gerät installierten übereinstimmt.  Bestätigen Sie das entsprechende Zertifikat in den Projekteigenschaften ausgewählt Bundle Signieren iPhone -> haben und das richtige Paket-Id, die in den Projekteigenschaften angegebenen -> iPhone Anwendung

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Codevervollständigung ist nicht in Visual Studio für Mac arbeiten.

Stellen Sie sicher, dass Sie die neueste Version von Visual Studio für Mac und Xamarin.iOS verwenden

Wenn das Problem weiterhin vorhanden ist, wenden [einen Fehler](http://monodevelop.com/Developers#Reporting_Bugs), das Anfügen der **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{Zeitstempel} .log**, und **Komponenten-{Zeitstempel} .log** Protokolldateien.

Wenn alle Versuche fehlschlagen, können Sie versuchen, entfernen den Abschluss Codecache, damit sie neu erstellt wird:

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Achten Sie darauf, dass Sie mit diesem Befehl ordnungsgemäß eingeben. Alternativ können Sie wichtige Dateien versehentlich entfernen.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Visual Studio für Mac stürzt ab, wenn Sie Text kopieren

Die gängige Mac-Hilfsprogramme QuickSilver, Google-Symbolleiste und Startbereich haben Zwischenablage-Funktionen, die Visual Studio für Mac Computer Speicher beschädigt. In ihren Optionen können Sie Visual Studio für Mac als Prozess auflisten, denen sie nicht beeinträchtigen sollte.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio für Mac meldet zu Mono 2.4 erforderlich

Wenn Sie Visual Studio für Mac aufgrund einer aktuellen Update aktualisiert, und wenn Sie versuchen, starten Sie ihn erneut er meldet zu Mono 2.4, die nicht vorhanden, Sie müssen, lediglich [upgrade der Installation Mono 2.4](http://www.go-mono.com/mono-downloads/download.html).  

Mono-/ 2.4.2.3_6 behebt einige wichtige Probleme, die verhindert, dass Visual Studio für Mac zuverlässig, manchmal nicht reagierende Visual Studio für Mac beim Start ausgeführt wird oder verhindert, dass die Code-Abschluss-Datenbank generiert wird.

Nach der Installation der neuen Mono startet Visual Studio für Mac wie erwartet.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Die Assertion an... /.. /.. /.. /Mono/Metadata/Generic-Sharing.c:704 Bedingung "Oti" ist nicht erfüllt.

Wenn Sie die folgenden stapelüberwachung empfängt:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Dies bedeutet, dass Sie eine statische Bibliothek mit Thumb-Code in das Projekt kompiliert verknüpfen. IPhone-SDK-Version 3.1 (oder höher zum Zeitpunkt der Erstellung dieses Dokuments) Apple beim Verknüpfen von nicht-Thumb-Code (Xamarin.iOS) mit Thumb-Code (die statische Bibliothek), einen Fehler in ihren Linker eingeführt. Sie müssen für die Verbindung mit einer nicht-Thumb-Version von Ihrer statischen Bibliothek aus, um dieses Problem zu minimieren.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException: Versuch JIT Compile-Methode (Wrapper verwaltet zu verwaltet) Foo[]:System.Collections.Generic.ICollection'1.get_Count)

Das Suffix [] Gibt an, dass Sie oder der Klassenbibliothek sind Aufrufen einer Methode für ein Array über eine generische Auflistung, z. B. IEnumerable <>, ICollection <> oder IList <>. Dieses Problem zu umgehen können Sie explizit erzwingen des AOT-Compilers, um eine solche Methode durch Aufrufen der Methode selbst einzuschließen, und durch sicherstellen, dass dieser Code ausgeführt wird, vor dem Aufruf, der die Ausnahme ausgelöst hat. In diesem Fall könnten Sie Folgendes schreiben:

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Die erzwingt des Compilers AOT Get_Count-Methode enthalten.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio für Mac-Quellen-Editor ist extrem langsam

In einigen Fällen wird die Visual Studio für Mac-Quellen-Editor extrem langsam angezeigt werden, einige Sekunden zwischen der Eingabe von Zeichen nicht mehr reagiert.

Dieses Problem tritt sehr selten und nur sehr schwer zu reproduzieren: es in der Regel kann nicht reproduziert werden auf dem gleichen Computer nach dem Neustart von Visual Studio für Mac. Aus diesem Grund würden wir es uns über könnten Sie verschiedene Debuggen Schritte ausführen, bevor Sie Visual Studio für Mac neu zu starten und die Ergebnisse an uns senden.

1.  Versuchen Sie die Registerkarte "Editor" zu schließen und erneut öffnen. Dauert es ein wenig bearbeiten, oder Verschieben der Einfügemarke erst die Verlangsamung wieder auftritt?
1.  Deaktivieren Sie "Balken Sync" mithilfe des "Quartz Debug" Developer-Tools (das Sie zugreifen können mit Spotlight), und überprüfen Sie, ob die Quelle Editor Leistung normal wiederhergestellt werden.
1.  Wiederholen Sie Schritt (1) mit Balken Synchronisierung noch deaktiviert.
1.  Wenn der Editor für mehr als ein paar Sekunden stürzt ab, führen Sie aus "Killall-beenden [Visual Studio für Mac]" in einem abschließenden, während er nicht reagiert wird. Es möglicherweise schwierig, die Uhrzeit des Kill-Befehls durchgeführt werden soll, während der Editor hängen geblieben ist, aber es ist wichtig, dies tun, da der Befehl zwingt Mono auftretenden Fehlern stapelverfolgungen aller Threads in das Protokoll MD schreiben wir verwenden können, welche Serverstatusermittlungs-Threads im sind, während die XS hängen geblieben ist.



Schließen Sie die Protokolle XS **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{Zeitstempel} .log**, und ** Komponenten-{Zeitstempel} .log ** (in früheren Versionen von XS / MonoDevelop, senden, sondern **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **Hinweis: Das oben angegebene Problem wurde in XS 2.2 abschließende behoben.**

## <a name="compiled-application-is-very-large"></a>Kompilierte Anwendung ist sehr groß.

Um Debuggen zu unterstützen, enthalten Debugbuilds wird zusätzlichen Code. Im Releasemodus erstellte Projekte sind einem Bruchteil der Größe.

As of Xamarin.iOS 1.3 Debug enthalten Builds debugging-Unterstützung für jede einzelne Komponente Mono (in jeder Klasse die Frameworks every-Methode).  

Mit Xamarin.iOS 1.4 eine feiner feine Methode für das Debuggen eingeführt wird, ist die Standardeinstellung zum Debuggen Instrumentation nur für den Code und Bibliotheken bereitstellen, und führen dies nicht für alle der [optimierter Mono-Assemblys](~/cross-platform/internals/available-assemblies.md) (Dies wird weiter möglich, aber es wird auf opt-in für diese Assemblys debuggen müssen).

## <a name="installation-hangs"></a>Installation Systemstillstand

Mono und Xamarin.iOS Installationsprogramme nicht reagieren, wenn stehen Ihnen die iPhone-Simulator ausgeführt wird. Dieses Problem nicht auf das Mono oder Xamarin.iOS beschränkt ist, handelt es sich ein permanentes Problem für jegliche Software, die versucht, die Software auf MacOS Schneefall Leopard installiert, wenn das iPhone Simulator Zeitpunkt der Installation ausgeführt wird.

Stellen Sie sicher, dass Sie die iPhone-Simulator zu beenden, und wiederholen Sie die Installation.

<a name="trampolines">

## <a name="ran-out-of-trampolines-of-type-0"></a>Nicht genügend Trampolines vom Typ 0

Wenn Sie diese Meldung erhalten, während der Ausführung des Geräts, können Sie weitere Typ erstellen 0 Trampolines (type-spezifisch) ändern Ihr Projekt Optionen "iPhone Build"-Abschnitt.  Sie möchten das Hinzufügen eines zusätzlichen Argumente für das Gerät Ziele zu erstellen:

 `-aot "ntrampolines=2048"`

Die Standardanzahl von Trampolines ist 1024.  Wiederholen Sie diese Anzahl erhöhen, bis Sie genug für Ihre Anwendung verfügen.

## <a name="ran-out-of-trampolines-of-type-1"></a>Nicht genügend Trampolines vom Typ 1

Wenn Sie starke Nutzung von rekursiven Generika vornehmen, erhalten Sie möglicherweise diese Meldung auf Gerät.  Sie können weitere Typ 1 Trampolines (Typ RGCTX) erstellen, indem Sie im Abschnitt "iPhone Build" Ihre des Projekts-Optionen ändern.  Sie möchten das Hinzufügen eines zusätzlichen Argumente für das Gerät Ziele zu erstellen:

 `-aot "nrgctx-trampolines=2048"`

Die Standardanzahl von Trampolines ist 1024.  Versuchen Sie, bis hoch genug für Ihre Nutzung von Generika besitzt die Erhöhung dieser Anzahl.

## <a name="ran-out-of-trampolines-of-type-2"></a>Nicht genügend Trampolines vom Typ 2

Wenn Sie vornehmen, stark Schnittstellen verwenden, erhalten Sie möglicherweise diese Meldung auf Gerät.
Sie können weitere Typ 2 Trampolines (Typ IMT Thunks) erstellen, indem Sie im Abschnitt "iPhone Build" Ihre des Projekts-Optionen ändern.  Sie möchten das Hinzufügen eines zusätzlichen Argumente für das Gerät Ziele zu erstellen:

 `-aot "nimt-trampolines=512"`

Die Standardanzahl von IMT Thunk Trampolines ist 128.  Wiederholen Sie diese Anzahl erhöhen, bis Sie hoch genug für die Verwendung der Schnittstellen verfügen.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Debugger kann keine Verbindung mit dem Gerät herstellen

Beim Starten des Debuggens einer Gerätekonfiguration, sehen Sie den Debugger, anzeigen, ein Dialogfeld gibt an, dass es an die Anwendung eine Verbindung herstellen möchte. Es gibt verschiedene Gründe dafür, die der Debugger möglicherweise nicht an die Anwendung eine Verbindung herstellen können, je nach dem Sie die Verbindung (USB- oder WiFi) verwenden.

 **Wenn das Gerät und dem Debuggerhost in unterschiedlichen Netzwerken sind**, eine Firewall oder privates Netzwerk möglicherweise verhindern die Anwendung eine Verbindung mit dem Debuggerhost im WiFi-Modus herstellen.

 **Visual Studio für Mac möglicherweise nicht die richtige IP-Adresse des Hosts Abfragen**. In WiFi Visual Studio für Mac-Modus ermöglicht es einer Anwendung alle die IP-Adressen gefunden werden kann von dem Host, und die Anwendung versucht, alle um festzustellen, ob er diese Verbindung zu Visual Studio für Mac verwenden können

 **Ein anderes Gerät ist mit einem USB-Anschluss auf dem Host verbunden.** In einigen Fällen, die andere Geräte mit USB verbunden wurden Ports auf dem Host als irgendwie beim Debuggen in USB-Modus beeinträchtigen.

Wenn entweder "WiFi" oder "USB-Modus nicht ordnungsgemäß funktioniert, können Sie problemlos versuchen die andere: in Visual Studio für Mac, öffnen Sie die Systemeinstellungen, wechseln Sie zur Seite" Einstellungen/Debugger/iPhone-Debugger "und das Kontrollkästchen"Debug iOS-Geräte über USB über WiFi anstelle von"umschalten.   Wenn keiner der beiden verwendet werden kann, sehen Sie weitere Informationen über den Fehler in der Konsole des Geräts im ausführlichen Modus (durch Hinzufügen von aktiviert "-V - V - V" auf die zusätzlichen Mtouch Argumente in den Optionen des Projekts).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>134 Mtouch Fehler mit folgender Meldung:

Dieser Fehler kann ausgelöst werden, wenn Sie versuchen, die mit - Nolink an die Xamarin.iOS 1.4-Stil der Versionen erstellen. Sie können diesen Fehler umgehen, indem Sie zusätzliche Argumente in die Projektkonfiguration Monodevelop angeben.

Fügen Sie das argument

 `-nosymbolstrip`

und das Problem sollte gelöst werden.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Verteilung Identität ist nicht im Visual Studio für Mac-Projekt Signaturoptionen dargestellt.

Visual Studio für Mac 2.2 hat einen Fehler, der bewirkt, er keine Zertifikate für Verteilungspunkte zu erkennen dass, die ein Komma enthalten. Aktualisieren Sie zu Visual Studio für Mac 2.2.1.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Fehler "AFCFileRefWrite zurückgegeben: 1" beim Hochladen

Beim Hochladen einer app auf Ihrem Gerät erhalten Sie möglicherweise eine Fehlermeldung "AFCFileRefWrite zurückgegeben: 1". Dies kann geschehen, wenn Sie eine Datei mit der Länge Null haben.

## <a name="error-mtouch-failed-with-no-output"></a>Fehler "Mtouch bei der keine Ausgabe"

Die aktuelle Version von Xamarin.iOS und Visual Studio für Mac fehl, wenn der Projektname oder das Verzeichnis, in dem die Projektmappe oder das Projekt gespeichert werden, Leerzeichen enthalten.
Um dieses Problem zu beheben:


-  Stellen Sie sicher, dass weder das Projekt oder das Verzeichnis, in dem sie gespeichert ist, ein Leerzeichen enthält.
-  Stellen Sie in Ihrem Projekt "Main-Einstellungen" sicher, dass der Projektname keine Leerzeichen enthält.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>Fehler "war die Binärdatei, die Sie hochgeladen haben ungültig. Eine Vorabversion Beta-Version des SDK wurde verwendet, um die Anwendung erstellen"

Dieser Fehler wird normalerweise verursacht ein Projekt aus, die in der iPad-Entwicklung gestartet wurde, bevor Xamarin.iOS 2.0.0 freigegeben wurde, erhalten Sie wahrscheinlich einige Schlüssel in der Datei "Info.plist" z. B.:

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Dieses Schlüsselpaars sollten entfernt werden, da Visual Studio für Mac für Sie automatisch verarbeitet.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Fehler "eine Vorabversion Beta-Version des SDK zum Erstellen der app verwendet wurde"

(Bereitgestellt durch Ed Anuff)

Führen Sie folgende Schritte aus:

-  Ändern der SDK-Version in der iPhone-Build auf der Version 3.2 oder iTunes verbinden es beim Upload abgelehnt, da sie eine kompatible iPad-app mit einer SDK-Version, die kleiner als 3.2 erstellte sehen
-  Erstellen Sie eine benutzerdefinierte Datei "Info.plist" für das Projekt, und 3.0 darin festlegen Sie MinimumOSVersion explizit.   Dadurch werden durch Xamarin.iOS festgelegten MinimumOSVersion 3.2-Wert überschrieben.   Wenn Sie dies nicht tun, kann die app nicht auf einem iPhone ausgeführt werden.
-  Rebuild, Zip und Upload an iTunes verbinden.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>Nicht behandelte Ausnahme: System.Exception: Fehler beim Suchen des Selektor SomeSelector: auf {Type}

Diese Ausnahme wird durch eines von drei Dingen verursacht:


1.  Sie haben einen Selektor für die Objective-C-Laufzeit angegeben, ohne die entsprechende [Export]-Attribut an eine Methode anwenden
1.  Sie haben die vollständige Verknüpfung aktiviert und nicht das Attribut [beibehalten] [Exportieren] Ed-Methode angewendet.
1.  Sie haben das [Export]-Attribut an eine private Methode in einem geerbten Typ angewendet.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs-Datei wird nicht aktualisiert.

Xamarin Studio 2.4, die nicht mit der Datei MainWindow.xib.designer in neuen Projekten MainWindow.xib Dateigruppen hat ist ein Fehler aufgetreten. Dies bedeutet, dass den Designer-Code für diese Datei nicht aktualisiert werden würde.

Dieses Problem wurde behoben, in der Version von Visual Studio für Mac, die in der integrierten Updater verfügbar ist, überprüfen Sie die neuere Version verwenden.

Sie können vorhandene Projekte beheben, indem entfernen (nicht gelöscht wird) die Xib und die Designer-Datei, Sichern sie hinzufügen. Dies sollte der Dateien ordnungsgemäß erneut gruppiert werden.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView oder UIActionSheet verschwinden nach dem Erstellen

Wenn Sie Code wie haben:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

das Objekt "ActionSheet" als eine temporäre Variable in der Funktion aktiv ist und als die Funktion beendet wird, das Objekt ist für die Garbagecollection, damit es am Ende der Garbage collection.

Um dieses Problem zu beheben, müssen Sie einen Verweis auf "ActionSheet" außerhalb der Methode an einer beliebigen Stelle, die außerhalb der Methode live wird beibehalten.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Projekt immer ausgeführt wird, in der iPad-Simulator

IPhone SDK 4.0 Installer installiert 2 SDKs - dem 3.2 SDK zum Erstellen von nur iPad-apps und die 4.0 SDK für Bulding iPhone und universelle apps verwendet. Wird es installiert außerdem eine 3.2-Simulator, der nur auf einem iPad simuliert, und eine 4.0-Simulator, der iPad- oder iPhone 4 simuliert. Alle älteren SDKs und Simulatoren werden entfernt.

Visual Studio für Mac iPhone-Projektoptionen für Build beinhalten eine Einstellung für die SDK-Version, die verwendet werden, bei der Erstellung der app. Mit dieser Einstellung finden Sie in **Build-Projektoptionen > -> iPhone Build**.

Neue Projekte in Visual Studio für Mac verwenden Sie das älteste installierten SDK als ihre Standardeinstellung für das SDK, und wenn der angegebene SDK nicht vorhanden ist, Visual Studio für Mac verwenden die nächstgelegene, die zur Erstellung einer app gefunden werden kann. Dies wurde vorgenommen, damit Projekte nicht immer das neueste SDK-Requre würde. Allerdings derzeit dadurch 3.2 SDK wird verwendete: was dazu führt, in der iPad-Simulator verwendet wird.

Wechseln Sie zur Beseitigung dieses Problems mithilfe des SDK 4.0 auf **Build-Projektoptionen > -> iPhone Build**> und ändern Sie den SDK-Wert, den Wert "4.0" im Dropdownfeld. Sie müssen dies für jede Konfiguration und Plattform zusammen mit den Dropdownlisten am Anfang des Bereichs zugegriffen.

Die SDK-Version sollte nicht mit der Einstellung "Minimale Betriebssystemversion" verwechselt werden.
Dieser Wert muss es sich nicht in der SDK-Version-Wert entsprechen: er wirkt sich auf die Mindestversion des Betriebssystems installieren Ihre app auf, die älter sind als das SDK sein kann, solange Sie nur die APIs, die auf dem älteren Betriebssystem vorhanden verwenden, oder als die Verwendung neuer Features, die mit der Common Language Runtime BS-Version auf Schutz KS. Sie sollten es auf die älteste Version des Betriebssystems festgelegt, auf denen die app zu testen.

Beachten Sie auch, dass die **iPhone-Simulator-Ziel-Projekts >**> im Menü kann verwendet werden, um den Simulator auswählen, die standardmäßig verwendet wird, wenn ein Projekt ausführen/Debuggen. Darüber hinaus die **ausführen Ausführen mit ->**> im Menü kann verwendet werden, um einen bestimmten Simulator mit der Ausführung auswählen

## <a name="ibtool-returns-error-133"></a>Ibtool zurück Fehler 133

Dies bedeutet, dass Sie XCode 4 ist installiert.   In XCode 4 das Tool Ibtool entfernt wurde, ist es nicht mehr möglich, Ihre XIB-Dateien mit einem eigenständigen Tool zu bearbeiten.

Wenn Sie Benutzeroberflächen-Generator verwenden möchten, installieren Sie [XCode Reihe 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), von der Apple-Website verfügbar.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Anzeige-Bindung für den MIME-Typ kann nicht erstellt werden: application/vnd.apple -<wbr/>Schnittstelle-Builder"

Dieser Fehler tritt auf, wenn Sie versuchen, ein iPhone-Benutzeroberfläche aus einem nicht-iPhone-Projekt zu erstellen. Stellen Sie sicher, dass Sie mit einer iPhone/iPad-Lösung beginnen, es ist nicht möglich, iPhone-UI-Elemente auf einem iPhone/iPad-Projekt lediglich zu addieren.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>Start-Abstürze bei der Ausführung innerhalb der iOS-simulator

Wenn Sie einen Absturz der Common Language Runtime (SIGSEGV) innerhalb der Simulator sowie eine stapelüberwachung, die wie folgt aussieht erhalten:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. ...und haben Sie wahrscheinlich eine (oder mehr) eine veraltete Assembly im Anwendungsverzeichnis Simulator. Solche Assemblys ist möglicherweise vorhanden, da Apple iOS-Simulator wird hinzugefügt und Dateien, aber nie gelöscht. Klicken Sie dann die einfachste Lösung hierfür ist, ist im Simulator "Zurücksetzen und Inhalte und Einstellungen..." auswählen.   

**Warnung:** Dies entfernt alle Dateien, Anwendungen und Daten im Simulator.   Beim nächsten die Anwendung ausführen Visual Studio für Mac wird in der Simulator bereitstellen und keine alten, veraltete Assembly den Absturz verursacht werden.

## <a name="simulator-hangs-during-application-installation"></a>Simulator Systemstillstand während der Anwendungsinstallation

Dies kann auftreten, wenn die Anwendungsnamen enthalten einen "." (Punkt) im Namen.
Dies wird als den Namen der ausführbaren Datei in CFBundleExecutable - ist unzulässig, auch wenn dies möglich ist funktioniert in vielen anderen Fällen (z. B. Geräte).

 * "Sollte der Wert keine Erweiterungen auf den Namen enthalten." - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Fehler: "Typ des benutzerdefinierten Attributs 0x43 wird nicht unterstützt" beim Doppelklicken auf .xib-Dateien

Dies wird verursacht, wird versucht, .xib Dateien geöffnet werden, wenn Umgebungsvariablen nicht ordnungsgemäß festgelegt sind. Dies sollte nicht der Fall sein mit normaler Verwendung von Visual Studio Mac/Xamarin.iOS und erneutes Öffnen des Visual Studio für Mac aus/Programme sollte das Problem behoben.

Beim Aktualisieren der Software und diese Fehlermeldung angezeigt wird, geben-e-Mail *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Anwendung-Simulator ausgeführt wird, aber ein Fehler auftritt, auf dem Gerät

Dieses Problem kann in mehreren Formularen Anwendungsmanifest und keine immer einen konsistente Fehler erzeugt wird. Wenn die Anwendung eine .xib enthält, stellen Sie sicher, den **Buildvorgang** für die .xib festgelegt, um **SchnittstelleDefinition**. Dies ist die Standardaktion für .xibs aus Build.

Um den Buildvorgang zu überprüfen, klicken Sie mit der rechten Maustaste auf die .xib-Datei, und wählen Sie **Buildvorgang**.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: Keine Daten steht für die Codierung 437

Wenn 3rd Party-Bibliotheken in Ihrer app Xamarin.iOS einschließen möchten, erhalten Sie möglicherweise einen Fehler in der Form "System.NotSupportedException: keine Daten für die Codierung 437 verfügbar ist" beim Versuch, kompilieren und Ausführen der app. Z. B. Bibliotheken, wie z. B. `Ionic.Zip.ZipFile`, während der Operation-Ausnahme auslösen können.

Dadurch gelöst werden kann, indem Sie die Optionen für das Xamarin.iOS-Projekt, und navigieren Sie zu öffnen **iOS-Build** > **Internationalisierung** und Überprüfung der **West** Internationalisierung.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Kann nicht auf Indie/Business-Testversion Konto aktualisieren

Wenn Sie kürzlich Xamarin.iOS erworben und zuvor eine Testversion Xamarin.iOS begonnen hat, müssen Sie möglicherweise die folgenden Schritte zum Abrufen dieser Lizenz Änderung von Visual Studio für Mac oder Visual Studio übernommen.

-  Schließen Sie Visual Studio für Mac/Visual Studio
-  Entfernen Sie alle Dateien aus ~/Library/MonoTouch auf Mac oder %PROGRAMDATA%\MonoTouch\License\ für Windows
-  Öffnen Sie Visual Studio erneut für Mac/Visual Studio, und beim Erstellen eines Projekts Xamarin.iOS


## <a name="receiving-activation-incomplete-error-message"></a>Fehlermeldung "Aktivierung unvollständig"

Dieses Problem kann auftreten, wenn Xamarin.iOS für Visual Studio verwenden. Um dieses Problem zu beheben, senden Sie die Protokolle aus den folgenden Ort hinzu [ contact@xamarin.com ](mailto:contact@xamarin.com).

-  Protokollspeicherort: %LocalAppData%/Xamarin/Logs
