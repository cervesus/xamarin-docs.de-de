---
title: Tipps zur Problembehandlung für xamarin. IOS
description: Dieses Dokument enthält verschiedene Tipps, die für die Problembehandlung bei der Entwicklung von xamarin. IOS-Anwendungen nützlich sind. Außerdem werden bestimmte Fehlermeldungen sowie andere potenzielle Probleme beschrieben.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2018
ms.openlocfilehash: d26f8f68b2cf4eca2d28a365c921b533e657c64b
ms.sourcegitcommit: 3434624a36a369986b6aeed7959dae60f7112a14
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69629610"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Tipps zur Problembehandlung für xamarin. IOS 

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin. IOS kann System. valuetuple nicht auflösen.

Dieser Fehler tritt aufgrund einer Inkompatibilität mit Visual Studio auf.

- **Visual Studio 2017 Update 1** (Version 15,1 oder älter) ist nur mit **System. valuetuple nuget 4.3.0** (oder älter) kompatibel.

- **Visual Studio 2017 Update 2** (Version 15,2 oder höher) ist nur mit dem **System. valuetuple-nuget-4.3.1** oder höher kompatibel.

Wählen Sie das richtige System. valuetuple-nuget aus, das Ihrer Visual Studio 2017-Installation entspricht.


## <a name="receiving-error-retrieving-update-information-error-message"></a>Empfangene Fehlermeldung "Fehler beim Abrufen der Update Informationen"

Wenn Sie versuchen, die Software zu aktualisieren, und diese Fehlermeldung angezeigt wird, starten Sie den Neustart Ihrer IDE, und melden Sie sich dann wieder bei Ihrem Konto in der IDE an.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Gewusst wie Sie mit Interface Builder Outlets oder Aktionen erstellen?

Mit der Einführung der Xamarin Designer für IOS in Visual Studio für Mac und Visual Studio können xamarin. IOS-Entwickler nun die Vorteile der Erstellung einer Benutzeroberfläche über Storyboards und xisb nutzen. Weitere Informationen zur Verwendung des Designers finden Sie in den Handbüchern zu [Hello, IOS](~/ios/get-started/hello-ios/index.md) .

Weitere Informationen zur Verwendung von Outlets und Aktionen in IB finden Sie auch in den Handbüchern zu den [Outlet](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) -und [Aktions](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) Handbüchern von Apple.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System. Text. Encoding. GetEncoding löst NotSupportedException aus.

Möglicherweise verwenden Sie eine Codierung, die nicht standardmäßig hinzugefügt wird. Auf der Seite [Internationalisierung](~/ios/app-fundamentals/localization/index.md) finden Sie Informationen zum Hinzufügen von Unterstützung für weitere Codierungen.

## <a name="systemmissingmethodexception-anything-else"></a>System. MissingMethodException (alles andere)

Der Member wurde wahrscheinlich vom Linker entfernt und ist daher zur Laufzeit nicht in der Assembly vorhanden.  Hierfür gibt es mehrere Lösungen:

- Fügen Sie [`[Preserve]`](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) dem-Element das-Attribut hinzu.  Dies hindert den Linker daran, ihn zu entfernen.
- Wenn Sie [**Mink**](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29)aufrufen, verwenden Sie die Optionen " **-nolink** " oder " **-linksdkonly** ":
  - **-nolink** deaktiviert alle Verknüpfungen.
  - **-linksdkonly** verknüpft nur von xamarin. IOS bereitgestellte Assemblys, z. b. **xamarin. IOS. dll**, während alle Typen in vom Benutzer erstellten Assemblys (d. a. ihren App-Projekten) beibehalten werden.

Beachten Sie, dass Assemblys verknüpft sind, sodass die resultierende ausführbare Datei kleiner ist. Folglich kann das Deaktivieren der Verknüpfung zu einer größeren ausführbaren Datei führen als erwünscht.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>Sie erhalten eine modelnotimplementedexception.

Wenn Sie diese Ausnahme erhalten, bedeutet dies, dass Sie die Basis aufrufen. Methode () für eine Klasse, die ein Modell überschreibt. Die Basis Methode muss nicht in einer Klasse für Modelle aufgerufen werden (Hierbei handelt es sich um Klassen, die mit dem [Model]-Attribut gekennzeichnet sind).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Diese Klasse ist für den Schlüssel xxxx nicht codierkonform mit Schlüsselwert.

Wenn Sie diese Fehlermeldung erhalten, wenn Sie eine NIB-Datei laden, bedeutet dies, dass der Wert xxxx nicht in der verwalteten Klasse gefunden wurde. Dies bedeutet, dass eine Deklaration wie die folgende fehlt:

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

Die obige Definition wird automatisch durch Visual Studio für Mac für alle XIb-Dateien generiert, die Sie Visual Studio für Mac in der `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` Datei hinzufügen.

Außerdem müssen die Typen, die den obigen Code enthalten, eine Unterklasse von [NSObject](xref:Foundation.NSObject)sein.  Wenn sich der enthaltende Typ innerhalb eines Namespace befindet, sollte er auch über ein [[Register]](xref:Foundation.RegisterAttribute) -Attribut verfügen, das einen Typnamen ohne Namespace bereitstellt (da Interface Builder Namespaces in Typen nicht unterstützt):

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Unbekannte Klasse XXXX in Interface Builder Datei

Dieser Fehler wird generiert, wenn Sie eine Klasse in den Schnittstellen Generator-Dateien definieren, aber die tatsächliche Implementierung nicht im C# Code bereitstellen.

Sie müssen Code wie den folgenden hinzufügen:

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System. MissingMethodException: Es wurde kein Konstruktor für foo. Bar:: ctor (System. IntPtr) gefunden.

Dieser Fehler wird zur Laufzeit erzeugt, wenn der Code versucht, eine Instanz der Klassen zu instanziieren, auf die Sie in ihrer Interface Builder Datei verwiesen haben. Dies bedeutet, dass Sie vergessen haben, einen Konstruktor hinzuzufügen, der einen einzelnen IntPtr als Parameter annimmt.

Der Konstruktor mit einem IntPtr-Handle wird verwendet, um verwaltete Objekte mit ihren nicht verwalteten Darstellungen zu binden.

Um dieses Problem zu beheben, fügen Sie der Klasse foo. Bar die folgende Codezeile hinzu:

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>Der Typ "{foo}" enthält keine Definition `GetNativeField` für, und es `GetNativeField` wurde keine Erweiterungsmethode vom Typ "{foo}" gefunden.

Wenn Sie diesen Fehler in den vom Designer generierten Dateien (*. XIb.Designer.cs) erhalten, bedeutet dies Folgendes:

 **1) fehlende partielle Klasse oder Basisklasse**

Die vom Designer generierten partiellen Klassen müssen über entsprechende partielle Klassen in Benutzercode verfügen, die häufig `NSObject` `UIViewController`von einer Unterklasse von erben. Stellen Sie sicher, dass Sie über eine solche Klasse für den Typ verfügen, der den Fehler ausgegeben hat.

 **2) Standardnamespaces geändert**

Die Designer Dateien werden mithilfe der standardmäßigen Namespace Einstellungen Ihres Projekts generiert. Wenn Sie diese Einstellungen geändert oder das Projekt umbenannt haben, befinden sich die generierten partiellen Klassen möglicherweise nicht mehr im gleichen Namespace wie Ihre Benutzercode Entsprechungen.

Namespace Einstellungen finden Sie im Dialogfeld "Projektoptionen". Der Standard Namespace finden Sie im Abschnitt **Allgemein-> Haupteinstellungen** . Wenn die Datei leer ist, wird der Name des Projekts als Standard verwendet. Erweiterte Namespace Einstellungen finden Sie im Abschnitt **Quell Code > .net-Benennungs Richtlinien** .

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Warnung für Aktionen: Die private Methode "foo" wird nie verwendet. (CS0169)

Aktionen für Interface Builder-Dateien werden durch Reflektion zur Laufzeit mit den Widgets verbunden, sodass diese Warnung erwartet wird.

Wenn Sie diese Warnung nur für diese Methoden unterdrücken möchten, können Sie die Option "#Pragma Warnung deaktivieren 0169" "#Pragma Warnung" 0169 aktivieren "verwenden, wenn Sie diese Warnung nur für diese Methoden unterdrücken möchten. Sie können auch 0169 zum Feld" Warnungen ignorieren "in den Compileroptionen verwenden, wenn Sie es für das gesamte Projekt deaktivieren möchten empfohlen).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>Fehler bei mtouchscreen mit folgender Meldung: Die Assembly "/path/to/yourproject.exe" kann nicht geöffnet werden.

Wenn diese Fehlermeldung angezeigt wird, ist das Problem in der Regel der absolute Pfad zu Ihrem Projekt, das ein Leerzeichen enthält. Dies wird in einer zukünftigen Version von xamarin. IOS behoben, aber Sie können das Problem umgehen, indem Sie das Projekt in einen Ordner ohne Leerzeichen verschieben.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Ihre sqlite3-Version ist veraltet. führen Sie ein Upgrade auf mindestens v 3.5.0 durch!

Dies geschieht, wenn Sie alle folgenden Aktionen ausführen:

1. Verwenden von Mono. Data. sqlite
1. Verwenden von Mac OS X Leopard (10,5)
1. Führen Sie die APP im Simulator aus.


Das Problem ist, dass Mono das Betriebssystem X `libsqlite3.dylib`und nicht die `libsqlite3.dylib` Datei "iphonesimulator" aufnimmt. Ihre APP funktioniert auf dem Gerät, aber nicht auf dem Simulator.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Fehler bei der Bereitstellung auf dem Gerät mit System. Exception: Aminviceingestallapplication hat 3892346901 zurückgegeben.

Dieser Fehler weist darauf hin, dass die Code Signierungs Konfiguration für Ihre Zertifikat-/Bundle-ID nicht mit dem Bereitstellungs Profil identisch ist, das auf Ihrem Gerät installiert ist.  Vergewissern Sie sich, dass das entsprechende Zertifikat in den Projektoptionen > iPhone-Bundle-Signierung und die richtige Bündel-ID in den Projektoptionen > iPhone-Anwendung ausgewählt ist.

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Code Vervollständigung funktioniert nicht in Visual Studio für Mac

Stellen Sie sicher, dass Sie die neueste Version von Visual Studio für Mac und xamarin. IOS verwenden.

Wenn das Problem weiterhin besteht, melden Sie [einen Fehler](http://monodevelop.com/Developers#Reporting_Bugs), und fügen Sie die Protokolldateien " **~/Library/Logs/XamarinStudio-{Version}/IDE-{Timestamp}.log**", " **androidtools-{Timestamp}. log**" und " **Components-{Timestamp}. log** " an.

Wenn alle anderen Fehler nicht angezeigt werden, können Sie versuchen, den Code Vervollständigungs Cache zu entfernen, sodass er neu generiert wird:

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Achten Sie darauf, dass Sie diesen Befehl ordnungsgemäß eingeben oder wichtige Dateien versehentlich entfernen.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Visual Studio für Mac stürzt ab, wenn Sie Text kopieren.

Die beliebten Mac-Hilfsprogramme Quicksilver, Google Toolbar und Launchbar verfügen über Zwischenablage Features, die Visual Studio für Mac Arbeitsspeicher beschädigen. In den Optionen können Sie Visual Studio für Mac als Prozess auflisten, der nicht beeinträchtigt werden sollte.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio für Mac über die erforderlichen mono-2,4-Informationen

Wenn Sie Visual Studio für Mac aufgrund eines aktuellen Updates aktualisiert haben, und wenn Sie versuchen, es erneut zu starten, werden Sie sich darüber beschweren, dass mono 2,4 nicht vorhanden ist. Sie müssen lediglich [die mono 2,4-Installation aktualisieren](http://www.go-mono.com/mono-downloads/download.html).  

Mono 2.4.2.3 _6 korrigiert einige wichtige Probleme, die eine zuverlässige Ausführung von Visual Studio für Mac verhindert haben, manchmal Visual Studio für Mac beim Start nicht reagiert oder das Generieren der Code Vervollständigungs Datenbank verhindert.

Nachdem Sie die neue Mono-Installation installiert haben, wird Visual Studio für Mac erwartungsgemäß gestartet.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Assert-unter.. /.. /.. /.. /Mono/Metadata/Generic-Sharing.c: 704, Bedingung ' Oti ' nicht erfüllt

Wenn Sie die folgende Stapel Überwachung erhalten:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Dies bedeutet, dass Sie eine mit Thumb-Code kompilierte statische Bibliothek in Ihr Projekt verknüpfen. Seit der iPhone SDK-Version 3,1 (oder höher zum Zeitpunkt der Erstellung dieses Dokuments) hat Apple einen Fehler in seinen Linker eingeführt, als nicht Thumb-Code (xamarin. IOS) mit Thumb-Code (Ihre statische Bibliothek) zu verknüpfen. Sie müssen eine Verknüpfung mit einer nicht-Thumb-Version ihrer statischen Bibliothek herstellen, um dieses Problem zu beheben.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1get_count-"></a>System.ExecutionEngineException: Versuch der JIT-Kompilierungs Methode (Wrapper von verwaltetem zu verwaltetem) foo []: System. Collections. Generic. icollection' 1. get_Count ()

Das Suffix [] gibt an, dass Sie oder die Klassenbibliothek eine Methode für ein Array über eine generische Auflistung aufrufen, z. b. IEnumerable < >, ICollection < > oder IList < >. Um dieses Problem zu umgehen, können Sie explizit erzwingen, dass der AOT-Compiler diese Methode einschließt, indem Sie die Methode selbst aufrufen und sicherstellen, dass dieser Code vor dem Aufruf ausgeführt wird, der die Ausnahme ausgelöst hat. In diesem Fall können Sie Folgendes schreiben:

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Dadurch wird der AOT-Compiler gezwungen, die get_Count-Methode einzubeziehen.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio für Mac Quellen-Editor ist äußerst langsam.

Manchmal wird der Quellen-Editor für Visual Studio für Mac extrem langsam, und er scheint mehrere Sekunden lang zwischen Eingabezeichen zu hängen.

Dieses Problem ist sehr selten und extrem schwer zu reproduzieren. es ist normalerweise nicht möglich, nach dem Neustart Visual Studio für Mac auf dem gleichen Computer reproduziert zu werden. Aus diesem Grund würden wir uns freuen, wenn Sie mehrere Debugschritte ausführen könnten, bevor Sie Visual Studio für Mac neu starten, und die Ergebnisse an uns senden.

1. Schließen Sie die Registerkarte Editor, und öffnen Sie Sie erneut. Dauert es etwas, bis die Einfügemarke bearbeitet oder verschoben wird, bis die Verlangsamung wieder auftritt?
1. Deaktivieren Sie "Balken Synchronisierung" mit dem Entwickler Tool "Quartz Debug" (das Sie mithilfe von Spotlight suchen können), und überprüfen Sie, ob die Leistung des Quell-Editors in normaler Weise wieder hergestellt wird.
1. Wiederholen Sie den Schritt (1), bei dem die Balken Synchronisierung deaktiviert ist.
1. Wenn der Editor seit mehr als wenigen Sekunden nicht mehr verfügbar ist, versuchen Sie, "killall-quit [Visual Studio für Mac]" in einem Terminal auszuführen, während es nicht reagiert. Es ist möglicherweise schwierig, den Kill-Befehl zu warten, während der Editor nicht reagiert, aber es ist von entscheidender Bedeutung, da der Befehl Mono erzwingt, Stapel Überwachungen aller Threads in das MD-Protokoll zu schreiben, mit dem wir ermitteln können, in welchem Zustand sich die Threads befinden, während der XS-Dienst nicht reagiert.



Fügen Sie die XS-Protokolle, **~/Library/Logs/XamarinStudio-{Version}/IDE-{Timestamp}.log**, **androidtools-{Timestamp}. log**und **Components-{Timestamp}. log** (in älteren Versionen von XS/monodevelop, Just send **~/Library/Logs /MonoDevelop-(3.0 | 2.8 | 2.6)/monodevelop.log**).

> [!NOTE]
> Das oben beschriebene Problem wurde in XS 2,2 Final * * behoben.

## <a name="compiled-application-is-very-large"></a>Kompilierte Anwendung sehr groß

Zum unterstützen des Debuggens enthalten Debugbuilds zusätzlichen Code. Im Releasemodus integrierte Projekte sind ein Bruchteil der Größe.

Ab xamarin. IOS 1,3 enthielt die Debug-Builds Debuggingunterstützung für jede einzelne Komponente von Mono (jede Methode in jeder Klasse des Frameworks).  

Mit xamarin. IOS 1,4 wird eine präzisere Methode für das Debuggen eingeführt. Standardmäßig wird nur die Debuginstrumentation für Ihren Code und Ihre Bibliotheken bereitgestellt. Dies geschieht nicht für alle [Mono](~/cross-platform/internals/available-assemblies.md) -Assemblys (Dies ist dennoch möglich, aber Sie muss sich für das Debuggen dieser Assemblys entscheiden.)

## <a name="installation-hangs"></a>Installation ist nicht mehr

Mono-und xamarin. IOS-Installationsprogramme hängen nicht aus, wenn der iPhone-Simulator ausgeführt wird. Dieses Problem ist nicht auf Mono oder xamarin. IOS beschränkt. Dies ist ein konsistentes Problem für jede Software, die versucht, Software auf MacOS snowleopard zu installieren, wenn der iPhone-Simulator zum Installations Zeitpunkt ausgeführt wird.

Stellen Sie sicher, dass Sie den iPhone-Simulator beenden, und wiederholen Sie die Installation.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>Nicht genügend neusprung des Typs 0

Wenn Sie diese Meldung erhalten, während das Gerät ausgeführt wird, können Sie weitere Typ 0-Trampolines (typspezifisch) erstellen, indem Sie den Abschnitt "iPhone Build" der Projektoptionen ändern.  Sie möchten zusätzliche Argumente für die gerätebuildziele hinzufügen:

 `-aot "ntrampolines=2048"`

Die Standard Anzahl der Trampoline ist 1024. Versuchen Sie, diese Zahl zu erhöhen, bis Sie für Ihre Anwendung ausreichend sind.

## <a name="ran-out-of-trampolines-of-type-1"></a>Nicht genügend neusprung des Typs 1.

Wenn Sie rekursive Generika stark verwenden, erhalten Sie diese Meldung möglicherweise auf dem Gerät.  Sie können weitere Type 1-neuteschlangen (Typ rgctx) erstellen, indem Sie den Abschnitt "iPhone Build" der Projektoptionen ändern.  Sie möchten zusätzliche Argumente für die gerätebuildziele hinzufügen:

 `-aot "nrgctx-trampolines=2048"`

Die Standard Anzahl der Trampoline ist 1024. Versuchen Sie, diese Zahl zu erhöhen, bis Sie für die Verwendung von Generika ausreichend sind.

## <a name="ran-out-of-trampolines-of-type-2"></a>Aus dem Typ 2 wurden keine neuwerte entfernt.

Wenn Sie hohe Verwendungs Schnittstellen verwenden, erhalten Sie diese Meldung möglicherweise auf dem Gerät.
Durch Ändern der Projektoptionen "iPhone Build" können Sie weitere Typ-2-Trampolines (IMT-thunkttente) erstellen.  Sie möchten zusätzliche Argumente für die gerätebuildziele hinzufügen:

 `-aot "nimt-trampolines=512"`

Die Standard Anzahl von IMT-Thunk-Trampoline ist 128. Versuchen Sie, diese Zahl zu erhöhen, bis Sie für die Verwendung von Schnittstellen ausreichend sind.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Der Debugger kann keine Verbindung mit dem Gerät herstellen.

Wenn Sie mit dem Debuggen einer Gerätekonfiguration beginnen, sehen Sie, dass der Debugger einen Dialog anzeigt, der anzeigt, dass er versucht, eine Verbindung mit der Anwendung herzustellen. Es gibt mehrere Gründe, warum der Debugger möglicherweise nicht in der Lage ist, eine Verbindung mit der Anwendung herzustellen, abhängig vom Modus, den Sie für die Verbindung verwenden (USB oder WiFi).

 **Wenn sich das Gerät und der Debuggerhost in unterschiedlichen Netzwerken befinden**, verhindert eine Firewall oder ein privates Netzwerk möglicherweise, dass die Anwendung im WiFi-Modus eine Verbindung mit dem Debugger-Host herstellt.

 **Visual Studio für Mac ist möglicherweise nicht in der Lage, die richtige IP-Adresse des Hosts abzufragen**. Im WiFi-Modus Visual Studio für Mac die Anwendung alle IPS erhält, die Sie für den Host finden kann, und die Anwendung versucht, Sie zu überprüfen, ob Sie für die Verbindung mit Visual Studio für Mac verwendet werden kann.

 **Ein anderes Gerät ist mit einem USB-Anschluss auf dem Host verbunden.** In einigen Fällen sind andere Geräte, die mit den USB-Ports auf dem Host verbunden sind, mit dem Debuggen im USB-Modus beeinträchtigt.

Wenn der WiFi-oder USB-Modus nicht funktioniert, können Sie den anderen problemlos ausprobieren: Öffnen Sie in Visual Studio für Mac die Einstellungen, wechseln Sie zur Seite Preferences/Debugger/iPhone Debugger, und schalten Sie das Kontrollkästchen "IOS-Geräte über WiFi anstatt über USB Debuggen".   Wenn keines von beiden funktioniert, können Sie weitere Informationen zu dem Fehler in der Geräte Konsole im ausführlichen Modus anzeigen (dieser wird durch Hinzufügen von "-v-v-v" zu den zusätzlichen mberührungs-Argumenten in den Projektoptionen aktiviert).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>Fehler 134: Fehler bei mtouchscreen mit folgender Meldung:

Dieser Fehler kann ausgelöst werden, wenn Sie versuchen, mit-nolink im xamarin. IOS 1,4-Stil von Releases zu erstellen. Sie können diesen Fehler umgehen, indem Sie in der monodevelop-Projekt Konfiguration zusätzliche Argumente angeben.

Argument hinzufügen

 `-nosymbolstrip`

das Problem sollte behoben werden.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Die Verteilungs Identität wird in Visual Studio für Mac Projekt Signierungs Optionen nicht angezeigt.

Visual Studio für Mac 2,2 weist einen Fehler auf, der bewirkt, dass keine Verteilungs Zertifikate erkannt werden, die ein Komma enthalten. Aktualisieren Sie auf Visual Studio für Mac 2.2.1.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Fehler "afcfilerefwrite zurückgegeben: 1 "während des Uploads

Beim Hochladen einer APP auf Ihr Gerät erhalten Sie möglicherweise die Fehlermeldung "afcfilerefwrite hat Folgendes zurückgegeben: 1". Dies kann vorkommen, wenn Sie eine Datei mit der Länge 0 (null) haben.

## <a name="error-mtouch-failed-with-no-output"></a>Fehler "mfinger Fehler ohne Ausgabe"

Die aktuelle Version von xamarin. IOS und Visual Studio für Mac schlägt fehl, wenn der Projektname oder das Verzeichnis, in dem die Projekt Mappe oder das Projekt gespeichert ist, Leerzeichen enthält.
Gehen Sie wie folgt vor, um das Problem zu beheben:


- Stellen Sie sicher, dass weder das Projekt noch das Verzeichnis, in dem es gespeichert ist, ein Leerzeichen enthält.
- Stellen Sie sicher, dass der Projekt Name im Projekt "Haupteinstellungen" keine Leerzeichen enthält.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>Fehler "die von Ihnen hochgeladene Binärdatei war ungültig. Eine Vorabversion der Beta Version des SDK wurde verwendet, um die Anwendung zu erstellen.

Dieser Fehler wird normalerweise durch ein Projekt verursacht, das in der iPad-Entwicklung vor der Veröffentlichung von xamarin. IOS 2.0.0 gestartet wurde. wahrscheinlich haben Sie einige Schlüssel in der Datei "Info. plist" wie folgt:

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Dieses Schlüsselpaar sollte entfernt werden, Visual Studio für Mac es automatisch verarbeitet.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Fehler "eine Vorabversion der Beta Version des SDK wurde verwendet, um die APP zu erstellen"

(Beigetragen von Ed Anuff)

Führen Sie folgende Schritte aus:

- Ändern Sie die SDK-Version in iPhone Build in 3,2, oder iTunes Connect lehnt Sie beim Upload ab, da eine iPad-kompatible App mit einer SDK-Version kleiner als 3,2 erstellt wird.
- Erstellen Sie eine benutzerdefinierte Datei "Info. plist" für das Projekt, und legen Sie minimumosversion explizit auf 3,0 fest.   Dadurch wird der von xamarin. IOS festgelegte minimumosversion 3,2-Wert überschrieben.   Wenn Sie dies nicht tun, kann die APP nicht auf einem iPhone ausgeführt werden.
- Neu erstellen, zip und Hochladen in iTunes Connect.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>Ausnahmefehler: System.Exception: Fehler beim Suchen der Auswahl "someselector": on {Type}

Diese Ausnahme wird durch eine der folgenden drei Punkte verursacht:


1. Sie haben einen Selektor für die Ziel-C-Laufzeit bereitgestellt, ohne das entsprechende [Export]-Attribut auf eine Methode anzuwenden.
1. Sie haben das vollständige verknüpfen aktiviert und das Attribut [preserve] nicht auf die [Export] Ed-Methode angewendet.
1. Sie haben das [Export]-Attribut auf eine private Methode in einem geerbten Typ angewendet.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>Die MainWindow.XIb.Designer.cs-Datei wird nicht aktualisiert.

In Xamarin Studio 2,4 ist ein Fehler aufgetreten, der bewirkt hat, dass die Datei "MainWindow. XIb" nicht mit der Datei "MainWindow. XIb. Designer" in neuen Projekten gruppiert wurde. Dies bedeutete, dass der Designer Code für die jeweilige Datei nicht aktualisiert werden konnte.

Dieses Problem wurde in der Version von Visual Studio für Mac behoben, die im integrierten Updater verfügbar ist. Stellen Sie daher sicher, dass Sie die neuere Version verwenden.

Sie können vorhandene Projekte reparieren, indem Sie den XIb und seine Designer Datei entfernen (nicht löschen) und dann wieder hinzufügen. Dies sollte die Dateien ordnungsgemäß erneut gruppieren.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView oder uiaktionsheet wird nach dem Erstellen nicht mehr angezeigt.

Wenn Sie über Code verfügen, der wie folgt aussieht:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

Das "Aktions Blatt"-Objekt ist als temporäre Variable in der Funktion verfügbar, und sobald die Funktion beendet wird, ist das Objekt für Garbage Collection qualifiziert, sodass es am Ende der Garbage Collection entspricht.

Um dieses Problem zu beheben, müssen Sie einen Verweis auf "Aktions Blatt" außerhalb ihrer Methode aufbewahren, da Sie an einer Stelle, an der Sie sich außerhalb ihrer Methode befinden.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Das Projekt wird immer im iPad-Simulator ausgeführt.

Der iPhone SDK 4,0-Installer installiert 2 SDKs: das 3,2 SDK zum Entwickeln von reinen iPad-apps und das 4,0 SDK, das zum Entwickeln von iPhone-und universellen Apps verwendet wird. Außerdem wird ein 3,2-Simulator installiert, der nur ein iPad simuliert, und ein 4,0-Simulator, der iPhone oder iPhone 4 simuliert. Alle älteren sdche und Simulatoren werden entfernt.

Visual Studio für Mac iPhone-projektbuildoptionen enthalten eine Einstellung für die SDK-Version, die beim Erstellen der APP verwendet wird. Diese Einstellung finden Sie unter **Project Options-> Build-> iPhone Build**.

Neue Projekte in Visual Studio für Mac das älteste installierte SDK als standardmäßige SDK-Einstellung verwenden, und wenn das angegebene SDK nicht vorhanden ist, verwendet Visual Studio für Mac den nächstgelegenen, der zum Erstellen der APP gefunden werden kann. Dies wurde erreicht, damit Projekte nicht immer das neueste SDK benötigen. Dies führt jedoch derzeit dazu, dass das 3,2 SDK verwendet wird. Dies führt dazu, dass der iPad-Simulator verwendet wird.

Um dies mithilfe des 4,0 SDK zu beheben, wechseln Sie zu **Projektoptionen-> Build-> iPhone Build**>, und ändern Sie den SDK-Wert im Dropdown Feld in "4,0". Dies muss für jede Konfiguration und Platt Form Kombination erfolgen, auf die Sie über die Dropdown Listen oben im Panel zugreifen können.

Die SDK-Version sollte nicht mit der Einstellung "minimale Betriebssystemversion" verwechselt werden.
Dieser Wert muss nicht mit dem SDK-Versions Wert identisch sein. er wirkt sich auf die Mindestversion des Betriebssystems aus, auf dem Ihre APP installiert wird, das älter als das SDK ist, solange Sie nur APIs verwenden, die im älteren Betriebssystem vorhanden sind, oder die Verwendung von neueren Features mithilfe der Laufzeit-Betriebssystemversion (Auschec) schützen. KS. Sie sollten diese auf die älteste Betriebssystemversion festlegen, auf der Sie Ihre APP testen.

Beachten Sie auch, dass das **Projekt > iPhone-Simulator**> Menü verwendet werden kann, um den Simulator auszuwählen, der beim Ausführen/Debuggen eines Projekts standardmäßig verwendet wird. Darüber hinaus kann das Menü Run **-> Run with**> verwendet werden, um einen bestimmten Simulator auszuwählen, mit dem ausgeführt werden soll.

## <a name="ibtool-returns-error-133"></a>ibtool gibt den Fehler 133 zurück.

Dies bedeutet, dass Xcode 4 installiert ist.   In Xcode 4 wurde das Tool ibtool entfernt. es ist nicht mehr möglich, ihre XIb-Dateien mit einem eigenständigen Tool zu bearbeiten.

Wenn Sie Interface Builder verwenden möchten, installieren Sie die [Xcode-Serie 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), die auf der Apple-Website verfügbar ist.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-interface-builder"></a>"Die Anzeige Bindung für den MIME-Typ kann nicht erstellt werden: application/vnd. Apple-Interface-Builder"

Dieser Fehler tritt auf, wenn Sie versuchen, eine iPhone-Benutzeroberfläche von einem nicht-iPhone-Projekt aus zu erstellen. Stellen Sie sicher, dass Sie mit einer iPhone/iPad-Lösung beginnen. es ist nicht möglich, iPhone-UI-Elemente einfach einem nicht-iPhone/iPad-Projekt hinzuzufügen.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>Start Absturz bei Ausführung innerhalb des IOS-Simulators

Wenn Sie einen Lauf Zeit Absturz (SIGSEGV) im Simulator zusammen mit einer Stapel Überwachung erhalten, die wie folgt aussieht:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
... Anschließend verfügen Sie wahrscheinlich über eine (oder mehrere) veraltete Assembly in Ihrem simulatoranwendungsverzeichnis. Solche Assemblys können vorhanden sein, da der Apple IOS-Simulator Dateien hinzufügt und aktualisiert, Sie aber nie löscht. Wenn dies der Fall ist, besteht die einfachste Lösung darin, "zurücksetzen und Inhalt und Einstellungen..." auszuwählen. im Menü Simulator.   

> [!WARNING]
> Dadurch werden alle Dateien, Anwendungen und Daten aus dem Simulator entfernt.   Wenn Sie die Anwendung das nächste Mal ausführen, wird Visual Studio für Mac im Simulator bereitgestellt, und es gibt keine alte, veraltete Assembly, die den Absturz verursacht.

## <a name="simulator-hangs-during-application-installation"></a>Simulator hängt bei der Anwendungs Installation

Dies kann vorkommen, wenn Anwendungsnamen ein "." enthalten. (Punkt) im Namen.
Dies ist unzulässig als Name der ausführbaren Datei in cfbundleausführ Bare Datei, auch wenn Sie in vielen anderen Fällen (z. b. Geräte) funktionieren kann.

 \* "Der Wert darf keine Erweiterung für den Namen enthalten." -[https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Fehler: Der benutzerdefinierte Attributtyp 0x43 wird beim Doppelklicken auf XIb-Dateien nicht unterstützt.

Dies wird durch den Versuch verursacht, XIb-Dateien zu öffnen, wenn Umgebungsvariablen falsch festgelegt sind. Dies sollte nicht bei der normalen Verwendung von Visual Studio für Mac/xamarin. IOS vorkommen, und durch das erneute Öffnen Visual Studio für Mac von/Anwendungen sollte das Problem behoben werden.

Wenn Sie versuchen, die Software zu aktualisieren, und diese Fehlermeldung angezeigt wird, senden Sie eine e-Mail *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Anwendung wird im Simulator ausgeführt, schlägt jedoch auf dem Gerät fehl

Dieses Problem kann sich in mehreren Formularen manifestieren und führt nicht immer zu einem konsistenten Fehler. Wenn die Anwendung eine XIb-Datei enthält, stellen Sie sicher, dass die Buildaktion auf der XIb-Datei auf **interfacedefinition**festgelegt ist. Dies ist die Standard-Buildaktion für. xisb.

Um die Buildaktion zu überprüfen, klicken Sie mit der rechten Maustaste aufdie XIb-Datei, und wählen Sie die Option


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: Es sind keine Daten für die Codierung 437 verfügbar.

Wenn Sie Drittanbieterbibliotheken in Ihre xamarin. IOS-App einschließen, erhalten Sie möglicherweise eine Fehlermeldung in der Form "System. NotSupportedException: Bei dem Versuch, die APP zu kompilieren und auszuführen, sind keine Daten für die Codierung 437 verfügbar. Beispielsweise können Bibliotheken, z `Ionic.Zip.ZipFile`. b., diese Ausnahme während des Vorgangs auslösen.

Dies kann behoben werden, indem Sie die Optionen für das xamarin. IOS-Projekt öffnen, zu der **IOS** > -buildinternationalisierung navigieren und die **West** Internationalisierung überprüfen.
