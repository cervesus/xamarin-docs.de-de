---
title: Buildprozess
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: 806ed841ec4db037a063bb458e1eed13226e08bd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
ms.locfileid: "32019710"
---
# <a name="build-process"></a>Buildprozess


## <a name="overview"></a>Übersicht

Der Xamarin.Android-Buildprozess ist dafür verantwortlich, alles zusammenzufügen: [Generieren von `Resource.designer.cs`](~/android/internals/api-design.md), Unterstützen von `AndroidAsset`, `AndroidResource` und anderen [Buildaktionen](#Build_Actions), Generieren von [Wrappern, die von Android aufgerufen werden können](~/android/platform/java-integration/android-callable-wrappers.md), und Generieren einer `.apk`-Datei für die Ausführung auf Android-Geräten.


## <a name="application-packages"></a>Anwendungspakete

Im Großen und Ganzen gibt es zwei Arten von Android-Anwendungspaketen (`.apk`-Dateien), die das Xamarin.Android-Buildsystem generieren kann: 

-   **Release**builds, die vollständig in sich geschlossen sind und keine zusätzlichen Pakete benötigen, um ausgeführt werden zu können. Dies sind die Pakete, die einem App Store zur Verfügung gestellt werden. 

-   **Debug**builds, auf die dies nicht zutrifft. 

Nicht zufällig stimmen diese mit der MSBuild-`Configuration` überein, die das Paket generiert.

### <a name="shared-runtime"></a>Shared Runtime

Die *Shared Runtime* besteht aus zwei zusätzlichen Android-Paketen, die die Basisklassenbibliothek (`mscorlib.dll` usw.) und die Android-Bindungsbibliothek (`Mono.Android.dll` usw.) bereitstellen. Debugbuilds verwenden die Shared Runtime, anstatt die Basisklassenbibliothek und Bindungsassemblys in das Android-Anwendungspaket einzubinden, wodurch das Debugpaket kleiner wird.

Die Shared Runtime kann in Debugbuilds deaktiviert werden, indem die `$(AndroidUseSharedRuntime)`-Eigenschaft auf `False` festgelegt wird. 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Schnelle Bereitstellung

Die *schnelle Bereitstellung* arbeitet mit der Shared Runtime zusammen, um die Größe des Android-Anwendungspakets weiter zu verringern. Dies wird dadurch erreicht, dass die Assemblys der App nicht innerhalb des Pakets gebündelt werden. Stattdessen werden sie über `adb push` in das Ziel kopiert. Dieser Prozess beschleunigt den Build-/Bereitstellungs-/Debugzyklus, denn wenn *nur* Assemblys geändert werden, wird das Paket nicht erneut installiert. Stattdessen werden nur die aktualisierten Assemblys mit dem Zielgerät erneut synchronisiert. 

Es ist bekannt, dass die schnelle Bereitstellung auf Geräten fehlschlägt, die die Synchronisierung von `adb` mit dem Verzeichnis `/data/data/@PACKAGE_NAME@/files/.__override__` blockieren. 

Die schnelle Bereitstellung ist standardmäßig aktiviert und kann in Debugbuilds deaktiviert werden, indem die `$(EmbedAssembliesIntoApk)`-Eigenschaft auf `True` festgelegt wird.


## <a name="msbuild-projects"></a>MSBuild-Projekte

Der Xamarin.Android-Buildprozess basiert auf MSBuild, dem Projektdateiformat, das auch von Visual Studio für Mac und Visual Studio verwendet wird.
Normalerweise müssen die Benutzer die MSBuild-Dateien nicht von Hand bearbeiten &ndash; die IDE erstellt voll funktionsfähige Projekte und aktualisiert sie mit allen vorgenommenen Änderungen und ruft bei Bedarf automatisch Build-Ziele auf. 

Fortgeschrittene Benutzer möchten vielleicht Aktionen ausführen, die nicht von der GUI der IDE unterstützt werden, sodass der Buildprozess durch direktes Bearbeiten der Projektdatei angepasst werden kann. Diese Seite dokumentiert nur die für Xamarin.Android spezifischen Features und Anpassungen &ndash; viele weitere Aktionen sind mit den normalen MSBuild-Elementen, -Eigenschaften und -Zielen möglich. 

<a name="Build_Targets" />

## <a name="build-targets"></a>Buildziele

Die folgenden Buildziele sind für Xamarin.Android-Projekte definiert:

-   **Build** &ndash; Erstellt das Paket.

-   **Clean** &ndash; Entfernt alle Dateien, die vom Buildprozess generiert wurden.

-   **Install** &ndash; Installiert das Paket auf dem Standardgerät oder virtuellen Gerät.

-   **Uninstall** &ndash; Deinstalliert das Paket vom Standardgerät oder virtuellen Gerät.

-   **SignAndroidPackage** &ndash; Erstellt und signiert das Paket (`.apk`). Verwenden Sie diese Option mit `/p:Configuration=Release`, um eigenständige „Releasepakete“ zu generieren.

-   **UpdateAndroidResources** &ndash; Aktualisiert die `Resource.designer.cs`-Datei. Dieses Ziel wird normalerweise von der IDE aufgerufen, wenn dem Projekt neue Ressourcen hinzugefügt werden.


## <a name="build-properties"></a>Buildeigenschaften

MSBuild-Eigenschaften steuern das Verhalten der Ziele. Sie werden in der Projektdatei (z.B. **MyApp.csproj**) in einem [PropertyGroup-Element von MSBuild](http://msdn.microsoft.com/en-us/library/t4w159bs.aspx) angegeben. 

-   **Konfiguration** &ndash; Gibt die zu verwendende Buildkonfiguration an, z.B. „Debug“ oder „Release“. Die Configuration-Eigenschaft wird verwendet, um Standardwerte für andere Eigenschaften zu ermitteln, die das Zielverhalten bestimmen. In der IDE können ggf. zusätzliche Konfigurationen erstellt werden.

    *Standardmäßig* wird die `Debug`-Konfiguration dazu führen, dass die `Install`- und `SignAndroidPackage`-Ziele ein kleineres Android-Paket erstellen, das das Vorhandensein anderer Dateien und Pakete erfordert, um funktionsfähig zu sein.

    Die `Release`-Standardkonfiguration führt dazu, dass die `Install`- und `SignAndroidPackage`-Ziele ein Android-Paket erstellen, das *eigenständig* ist und ohne Installation anderer Pakete oder Dateien verwendet werden kann.

-   **DebugSymbols** &ndash; Ein boolescher Wert, der bestimmt, ob das Android-Paket *debuggt* werden kann (in Kombination mit der `$(DebugType)`-Eigenschaft). Ein Paket, das debuggt werden kann, enthält Debugsymbole, legt das `//application/@android:debuggable`-Attribut auf `true` fest und fügt automatisch die Berechtigung `INTERNET` hinzu, sodass ein Debugger an den Prozess angefügt werden kann. Eine Anwendung kann debuggt werden, wenn `DebugSymbols` den Wert `True` aufweist *und* `DebugType` entweder die leere Zeichenfolge oder `Full` ist.

-   **DebugType** &ndash; Gibt den [Typ von Debugsymbolen](http://msdn.microsoft.com/en-us/library/s5c8athz.aspx) an, die als Teil des Builds generiert werden sollen. Dies wirkt sich ebenfalls darauf aus, ob die Anwendung debuggt werden kann. Mögliche Werte:

    - **Full**: Vollständige Symbole werden generiert. Wenn die MSBuild-Eigenschaft `DebugSymbols` ebenfalls den Wert `True` aufweist, kann das Anwendungspaket debuggt werden.

    - **PdbOnly**: Es werden PDB-Symbole generiert. Das Anwendungspaket kann *nicht* debuggt werden.

    Wenn `DebugType` nicht festgelegt wurde oder die leere Zeichenfolge ist, steuert die `DebugSymbols`-Eigenschaft, ob die Anwendung debuggt werden kann.


### <a name="install-properties"></a>Installationseigenschaften

Installationseigenschaften steuern das Verhalten der `Install`- und `Uninstall`-Ziele.

-   **AdbTarget** &ndash; Gibt das Android-Zielgerät an, auf dem das Android-Paket installiert oder entfernt werden soll. Der Wert dieser Eigenschaft ist identisch mit der [-`adb`Zielgerätoption](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Paketeigenschaften

Paketeigenschaften steuern die Erstellung des Android-Pakets und werden von den `Install`- und `SignAndroidPackage`-Zielen verwendet.
Die [Signatureigenschaften](#Signing_Properties) sind auch für die Pakete von Releaseanwendungen relevant.


-   **AndroidApkSigningAlgorithm**: Ein Zeichenfolgenwert, der den mit `jarsigner -sigalg` zu verwendenden Signaturalgorithmus angibt.

    Der Standardwert ist `md5withRSA`.

    Hinzugefügt in Xamarin.Android 8.2.

-   **AndroidApplication** &ndash; Ein boolescher Wert, der angibt, ob das Projekt für eine Android-Anwendung (`True`) oder für ein Android-Bibliotheksprojekt (`False` oder nicht vorhanden) vorgesehen ist.

    Es darf nur ein Projekt mit `<AndroidApplication>True</AndroidApplication>` in einem Android-Paket vorhanden sein. (Leider wurde dies noch nicht bestätigt, was zu subtilen und bizarren Fehlern in Bezug auf Android-Ressourcen führen kann.)

-   **AndroidBuildApplicationPackage** &ndash; Ein boolescher Wert, der angibt, ob das Paket erstellt und signiert werden soll (APK-Datei). Wenn Sie diesen Wert auf `True` festlegen, entspricht dies der Verwendung des [SignAndroidPackage](#Build_Targets)-Buildziels.

    Unterstützung für diese Eigenschaft wurde nach Xamarin.Android 7.1 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `False`.

-   **AndroidEnableMultiDex** &ndash; Eine boolesche Eigenschaft, die bestimmt, ob Multi-Dex-Unterstützung in der endgültigen `.apk`-Datei verwendet wird.

    Unterstützung für diese Eigenschaft wurde in Xamarin.Android 5.1 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `False`.

-   **AndroidEnableSGenConcurrent** &ndash; Eine boolesche Eigenschaft, die bestimmt, ob der [gleichzeitige GC-Collector](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) von Mono verwendet wird.

    Unterstützung für diese Eigenschaft wurde in Xamarin.Android 7.2 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `False`.

-   **AndroidErrorOnCustomJavaObject**: Eine boolesche Eigenschaft, die bestimmt, ob Typen `Android.Runtime.IJavaObject`
    *ohne* gleichzeitiges Erben von `Java.Lang.Object` oder `Java.Lang.Throwable` implementieren können:

    ```csharp
    class BadType : IJavaObject {
        public IntPtr Handle {
            get {return IntPtr.Zero;}
        }

        public void Dispose()
        {
        }
    }
    ```

    Wenn diese Eigenschaft „true“ lautet, erzeugen solche Typen einen XA4212-Fehler. Andernfalls wird eine XA4212-Warnung generiert.

    Unterstützung für diese Eigenschaft wurde in Xamarin.Android 8.1 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `True`.

-   **AndroidFastDeploymentType** &ndash; Eine durch Doppelpunkte (`:`) getrennte Liste von Werten zum Steuern, welche Typen im [Verzeichnis für schnelle Bereitstellung](#Fast_Deployment) auf dem Zielgerät bereitgestellt werden können, wenn die MSBuild-Eigenschaft `$(EmbedAssembliesIntoApk)` den Wert `False` aufweist. Wenn eine Ressource schnell bereitgestellt wird, ist sie *nicht* in die generierte `.apk`-Datei eingebettet, was die Bereitstellungszeiten beschleunigen kann. (Je mehr schneller bereitgestellt wird, desto seltener muss die `.apk`-Datei neu erstellt werden, und der Installationsprozess kann schneller ablaufen.) Gültige Werte sind:

    - `Assemblies`: Bereitstellen von Anwendungsassemblys.

    - `Dexes`: Bereitstellen von `.dex`-Dateien, Android-Ressourcen und Android-Objekten. **Dieser Wert kann *nur* auf Geräten mit Android 4.4 oder höher (API-19) verwendet werden.**

    Der Standardwert ist `Assemblies`.

    **Experimentell**. In Xamarin.Android 6.1 hinzugefügt.

-   **AndroidApplicationJavaClass** &ndash; Der vollständige Java-Klassenname, der anstelle von `android.app.Application` verwendet werden soll, wenn eine Klasse von [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) erbt.

    Diese Eigenschaft wird im Allgemeinen durch *andere* Eigenschaften festgelegt, z.B. durch die MSBuild-Eigenschaft `$(AndroidEnableMultiDex)`.

    In Xamarin.Android 6.1 hinzugefügt.

-   **AndroidHttpClientHandlerType**: Steuert die standardmäßige Implementierung von `System.Net.Http.HttpMessageHandler`, die vom `System.Net.Http.HttpClient`-Standardkonstruktor verwendet wird. Der Wert ist ein Name mit Assemblyqualifikation einer `HttpMessageHandler`-Unterklasse und eignet sich für die Verwendung mit [`System.Type.GetType(string)`](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_).

    Der Standardwert ist `System.Net.Http.HttpClientHandler, System.Net.Http`.

    Der Wert kann mit `Xamarin.Android.Net.AndroidClientHandler` überschrieben werden, der die Android Java-APIs verwendet, um Netzwerkanforderungen auszuführen. Dies ermöglicht den Zugriff auf TLS 1.2-URLs, wenn die zugrunde liegende Android-Version TLS 1.2 unterstützt.  
    Nur Android 5.0 und höhere Versionen stellen zuverlässige TLS 1.2-Unterstützung durch Java bereit.

    *Hinweis*: Wenn TLS 1.2-Unterstützung in Android-Versionen vor 5.0 erforderlich ist, *oder* wenn TLS 1.2-Unterstützung mit dem `System.Net.WebClient` und zugehörigen APIs benötigt wird, sollte `$(AndroidTlsProvider)` verwendet werden.

    *Hinweis*: Die Unterstützung für diese Eigenschaft funktioniert durch Festlegen der [`XA_HTTP_CLIENT_HANDLER_TYPE`-Umgebungsvariable](~/android/deploy-test/environment.md).
    Wenn in einer Datei ein `$XA_HTTP_CLIENT_HANDLER_TYPE`-Wert mit einer `@(AndroidEnvironment)`-Buildaktion gefunden wird, hat dieser Wert Vorrang.

    In Xamarin.Android 6.1 hinzugefügt.

-   **AndroidTlsProvider** &ndash; Ein Zeichenfolgenwert, der angibt, welcher TLS-Anbieter in einer Anwendung verwendet werden soll. Dabei sind folgende Werte möglich:

    - `btls`: Verwendet [BoringSSL](https://boringssl.googlesource.com/boringssl) für die TLS-Kommunikation mit [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      Dies ermöglicht die Verwendung von TLS 1.2 in allen Android-Versionen.

    - `legacy`: Verwendet die historisch verwaltete SSL-Implementierung für die Netzwerkinteraktion. Dies unterstützt TLS 1.2 *nicht*.

    - `default`: Gestatten Sie *Mono* die Auswahl des TLS-Standardanbieters.
      Dies entspricht `legacy`, auch in Xamarin.Android 7.3.  
      *Hinweis*: Dieser Wert wird wahrscheinlich nicht in `.csproj`-Werten angezeigt, da der „Standardwert“ der IDE zu einer*Entfernung* der `$(AndroidTlsProvider)`-Eigenschaft führt.

    - Nicht festgelegt oder leere Zeichenfolge: In Xamarin.Android 7.1 entspricht dies `legacy`.  
      In Xamarin.Android 7.3 entspricht dies `btls`.

    Der Standardwert ist die leere Zeichenfolge.

    In Xamarin.Android 7.1 hinzugefügt.

-   **AndroidLinkMode** &ndash; Gibt an, welcher Typ von [Verknüpfung](~/android/deploy-test/linker.md) für Assemblys ausgeführt werden soll, die im Android-Paket enthalten sind. Wird nur in Android-Anwendungsprojekten verwendet. Der Standardwert ist *SdkOnly*. Gültige Werte sind:

    - **Keiner**: Es wird keine Verknüpfung versucht.

    - **SdkOnly**: Verknüpfung wird nur für die Basisklassenbibliotheken ausgeführt, nicht für die Assemblys des Benutzers.

    - **Full**: Verknüpfung wird für die Basisklassenbibliotheken und die Assemblys des Benutzers ausgeführt. **Hinweis:** Die Verwendung eines `AndroidLinkMode`-Werts von *Full* führt häufig zu fehlerhaften Apps, insbesondere wenn Reflektion verwendet wird. Vermeiden Sie diesen Wert (es sei denn, Sie wissen *wirklich*, was Sie tun).

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; Gibt eine durch Semikolons (`;`) getrennte Liste von Assemblynamen (ohne Dateierweiterungen) von Assemblys an, die nicht verknüpft werden sollen. Wird nur in Android-Anwendungsprojekten verwendet.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; Eine boolesche Eigenschaft, die steuert, ob Sequenzpunkte generiert werden, sodass Dateinamen- und Zeilennummerinformationen aus `Release`-Stapelüberwachungen extrahiert werden können.

    In Xamarin.Android 6.1 hinzugefügt.

-   **AndroidManifest** &ndash; Gibt einen Dateinamen an, der als Vorlage für die Datei [`AndroidManifest.xml`](~/android/platform/android-manifest.md) der App verwendet werden soll.
    Während des Builds werden alle anderen notwendigen Werte gemergt, um die eigentliche Datei `AndroidManifest.xml` zu generieren.
    `$(AndroidManifest)` muss den Paketnamen im `/manifest/@package`-Attribut enthalten.

-   **AndroidSdkBuildToolsVersion** &ndash; Das build-tools-Paket des Android SDK enthält unter anderem die Tools **aapt** und **zipalign**. Mehrere verschiedene Versionen des build-tools-Pakets können gleichzeitig installiert werden. Das build-tools-Paket, das für die Paketerstellung ausgewählt wird, wird durch Überprüfen und Verwenden einer „bevorzugten“ build-tools-Version ermittelt, wenn diese vorhanden ist. Wenn die „bevorzugte“ Version *nicht* vorhanden ist, wird das Paket mit der höchsten installierten build-tools-Version verwendet.

    Die MSBuild-Eigenschaft `$(AndroidSdkBuildToolsVersion)` enthält die bevorzugte build-tools-Version. Das Xamarin.Android-Buildsystem stellt einen Standardwert in `Xamarin.Android.Common.targets` zur Verfügung, und der Standardwert kann in Ihrer Projektdatei überschrieben werden, um eine alternative build-tools-Version auszuwählen, wenn (zum Beispiel) die letzte Version von aapt abstürzt, während eine frühere aapt-Version bekanntermaßen funktioniert.

-   **AndroidSupportedAbis** &ndash; Eine Zeichenfolgeneigenschaft, die eine durch Semikolons (`;`) getrennte Liste von ABIs enthält, die in die `.apk`-Datei aufgenommen werden sollen.

    Unterstützte Werte sind:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Erfordert Xamarin.Android 5.1 oder höher.
    -   `x86_64`: Erfordert Xamarin.Android 5.1 oder höher.

-   **AndroidUseSharedRuntime** &ndash; Eine boolesche Eigenschaft, die bestimmt, ob die *Shared Runtime-Pakete* erforderlich sind, um die Anwendung auf dem Zielgerät auszuführen. Durch die Verwendung der Shared Runtime-Pakete kann das Anwendungspaket kleiner werden, wodurch der Vorgang der Paketerstellung und -bereitstellung beschleunigt wird, was zu einem schnelleren Verarbeitungszyklus für Build/Bereitstellung/Debuggen führt.

    Diese Eigenschaft sollte `True` für Debugbuilds und `False` für Releaseprojekte sein.

-   **AotAssemblies** &ndash; Eine boolesche Eigenschaft, die festlegt, ob Assemblys vorzeitig in nativen Code kompiliert und in die `.apk`-Datei eingebunden werden.

    Unterstützung für diese Eigenschaft wurde in Xamarin.Android 5.1 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `False`.

-   **EmbedAssembliesIntoApk** &ndash; Eine boolesche Eigenschaft, die bestimmt, ob die Assemblys der App in das Anwendungspaket eingebettet werden sollen.

    Diese Eigenschaft sollte `True` für Releasebuilds und `False` für Debugbuilds sein. Sie *muss* ggf. `True` in Debugbuilds sein, wenn die schnelle Bereitstellung das Zielgerät nicht unterstützt.

    Wenn diese Eigenschaft `False` ist, steuert die MSBuild-Eigenschaft `$(AndroidFastDeploymentType)` auch, was in die `.apk`-Datei eingebettet wird. Dies wirkt sich auf die Bereitstellungszeit und die Zeit für erneute Buildprozesse aus.

-   **EnableLLVM** &ndash; Eine boolesche Eigenschaft, die bestimmt, ob LLVM verwendet wird, wenn Assemblys vorab in nativen Code kompiliert werden.

    Unterstützung für diese Eigenschaft wurde in Xamarin.Android 5.1 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `False`.

    Diese Eigenschaft wird ignoriert, wenn die MSBuild-Eigenschaft `$(AotAssemblies)` nicht den Wert `True` aufweist.

-   **EnableProguard** &ndash; Eine boolesche Eigenschaft, die bestimmt, ob [proguard](http://developer.android.com/tools/help/proguard.html) als Teil des Paketerstellungsprozesses ausgeführt wird, um Java-Code zu verknüpfen.

    Unterstützung für diese Eigenschaft wurde in Xamarin.Android 5.1 hinzugefügt.

    Standardmäßig ist diese Eigenschaft `False`.

    Wenn `True`, werden [ProguardConfiguration](#ProguardConfiguration)-Dateien verwendet, um die `proguard`-Ausführung zu steuern.

-   **JavaMaximumHeapSize** &ndash; Gibt den Wert des **java**
    `-Xmx`-Parameters an, der beim Erstellen der `.dex`-Datei als Teil des Paketerstellungsprozesses verwendet werden soll. Wenn keine Angabe erfolgt, wird die Option `-Xmx` nicht an **java** übergeben.

    Die Angabe dieser Eigenschaft ist erforderlich, wenn das [`_CompileDex`-Ziel einen `java.lang.OutOfMemoryError` auslöst](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; Gibt zusätzliche Befehlszeilenoptionen an, die beim Erstellen der `.dex`-Datei an **java** übergeben werden.

-   **MandroidI18n** &ndash; Gibt die Internationalisierungsunterstützung an, die in der Anwendung enthalten ist, wie z.B. Sortierung und Sortieren von Tabellen. Der Wert ist eine durch Kommas oder Semikolons getrennte Liste von mindestens einem der folgenden Werte, für den nicht zwischen Groß-/Kleinschreibung unterschieden wird:

    -   **Keiner**: Keine zusätzliche Codierung einbeziehen.

    -   **All**: Alle verfügbaren Codierungen einschließen.

    -   **CJK**: Chinesische, japanische und koreanische Codierungen einschließen, z.B. *Japanisch (EUC)* \[enc-jp, CP51932\], *Japanisch (Shift-JIS)* \[iso-2022-jp, shift\_jis, CP932\], *Japanisch (JIS)* \[CP50220\], *Chinesisch vereinfacht (GB2312)* \[gb2312, CP936\], *Koreanisch (UHC)* \[ks\_c\_5601-1987, CP949\], *Koreanisch (EUC)* \[euc-kr, CP51949\], *Chinesisch traditionell (Big5)* \[big5, CP950\] und *Chinesisch vereinfacht (GB18030)* \[GB18030, CP54936\].

    -   **MidEast**: Codierungen des Nahen Ostens einschließen, z.B. *Türkisch (Windows)* \[iso-8859-9, CP1254\], *Hebräisch (Windows)* \[windows-1255, CP1255\], *Arabisch (Windows)* \[windows-1256, CP1256\], *Arabisch (ISO)* \[iso-8859-6, CP28596\], *Hebräisch (ISO)* \[iso-8859-8, CP28598\], *Lateinisch 5 (ISO)* \[iso-8859-9, CP28599\] und *Hebräisch (Iso Alternative)* \[iso-8859-8, CP38598\].

    -   **Other**: Andere Codierungen einschließen, z.B. *Kyrillisch (Windows)* \[CP1251\], *Baltisch (Windows)* \[iso-8859-4, CP1257\], *Vietnamesisch (Windows)* \[CP1258\], *Kyrillisch (KOI8-R)* \[koi8-r, CP1251\], *Ukrainisch (KOI8-U)* \[koi8-u, CP1251\], *Baltisch (ISO)* \[iso-8859-4, CP1257\], *Kyrillisch (ISO)* \[iso-8859-5, CP1251\], *ISCII Davenagari* \[x-iscii-de, CP57002\], *ISCII Bangla* \[x-iscii-be, CP57003\], *ISCII Tamil* \[x-iscii-ta, CP57004\], *ISCII Telegu* \[x-iscii-te, CP57005\], *ISCII Assamesisch* \[x-iscii-as, CP57006\], *ISCII Odia* \[x-iscii-or, CP57007\], *ISCII Kannada* \[x-iscii-ka, CP57008\], *ISCII Malayalam* \[x-iscii-ma, CP57009\], *ISCII Gujarati* \[x-iscii-gu, CP57010\], *ISCII Punjabi* \[x-iscii-pa, CP57011\] und *Thailändisch (Windows)* \[CP874\].

    -   **Rare**: Seltene Codierungen einschließen, z.B. *IBM EBCDIC (Türkisch)* \[CP1026\], *IBM EBCDIC (Open Systems Lateinisch 1)* \[CP1047\], *IBM EBCDIC (USA/Kanada mit Euro)* \[CP1140\], *IBM EBCDIC (Deutschland mit Euro)* \[CP1141\], *IBM EBCDIC (Dänemark/Norwegen mit Euro)* \[CP1142\], *IBM EBCDIC (Finnland/Schweden mit Euro)* \[CP1143\], *IBM EBCDIC (Italien mit Euro)* \[CP1144\], *IBM EBCDIC (Lateinamerika/Spanien mit Euro)* \[CP1145\], *IBM EBCDIC (Vereinigtes Königreich mit Euro)* \[CP1146\], *IBM EBCDIC (Frankreich mit Euro)* \[CP1147\], *IBM EBCDIC (International mit Euro)* \[CP1148\], *IBM EBCDIC (Isländisch mit Euro)* \[CP1149\], *IBM EBCDIC (Deutschland)* \[CP20273\], *IBM EBCDIC (Dänemark/Norwegen)* \[CP20277\], *IBM EBCDIC (Finnland/Schweden)* \[CP20278\], *IBM EBCDIC (Italien)* \[CP20280\], *IBM EBCDIC (Lateinamerika/Spanien)* \[CP20284\], *IBM EBCDIC (Vereinigtes Königreich)* \[CP20285\], *IBM EBCDIC (Japanisch, erweitertes Katakana)* \[CP20290\], *IBM EBCDIC (Frankreich)* \[CP20297\], *IBM EBCDIC (Arabisch)* \[CP20420\], *IBM EBCDIC (Hebräisch)* \[CP20424\], *IBM EBCDIC (Isländisch)* \[CP20871\], *IBM EBCDIC (Kyrillisch: Serbisch, Bulgarisch)* \[CP21025\], *IBM EBCDIC (USA/Kanada)* \[CP37\], *IBM EBCDIC (International)* \[CP500\], *Arabisch (ASMO 708)* \[CP708\], *Zentraleuropäisch(DOS)* \[CP852\]*, Kyrillisch (DOS)* \[CP855\], *Türkisch (DOS)* \[CP857\], *Westeuropäisch (DOS mit Euro)* \[CP858\], *Hebräisch (DOS)* \[CP862\], *Arabisch (DOS)* \[CP864\], *Russisch (DOS)* \[CP866\], *Griechisch (DOS)* \[CP869\], *IBM EBCDIC (Lateinisch 2)* \[CP870\] und *IBM EBCDIC (Griechisch)* \[CP875\].

    -   **West**: Westliche Codierungen einschließen, z.B. *Westeuropäisch (Mac)* \[macintosh, CP10000\], *Isländisch (Mac)* \[x-mac-icelandic, CP10079\], *Zentraleuropäisch (Windows)* \[iso-8859-2, CP1250\], *Westeuropäisch (Windows)* \[iso-8859-1, CP1252\], *Griechisch (Windows)* \[iso-8859-7, CP1253\], *Zentraleuropäisch (ISO)* \[iso-8859-2, CP28592\], *Lateinisch 3 (ISO)* \[iso-8859-3, CP28593\], *Griechisch (ISO)* \[iso-8859-7, CP28597\], *Lateinisch 9 (ISO)* \[iso-8859-15, CP28605\], *OEM USA* \[CP437\], *Westeuropäisch (DOS)* \[CP850\], *Portugiesisch (DOS)* \[CP860\], *Isländisch (DOS)* \[CP861\], *Französisch, Kanada (DOS)* \[CP863\] und *Nordisch (DOS)* \[CP865\].


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchiv** &ndash; Eine boolesche Eigenschaft, die steuert, ob `.mSYM`-Artefakte für die spätere Verwendung mit `mono-symbolicate` generiert werden, um &ldquo;echte&rdquo; Dateinamen- und Zeilennummerinformationen aus Releasestapelüberwachungen zu extrahieren.

    Dies ist standardmäßig TRUE für &ldquo;Release&rdquo;-Apps mit aktivierten Debugsymbolen: `$(EmbedAssembliesIntoApk)` ist TRUE, `$(DebugSymbols)` ist TRUE, und `$(Optimize)` ist TRUE.

    In Xamarin.Android 7.1 hinzugefügt.

-   **AndroidVersionCodePattern** &ndash; Eine Zeichenfolgeneigenschaft, die es dem Entwickler ermöglicht, den `versionCode` im Manifest anzupassen.
    Informationen zur Entscheidung für einen `versionCode`finden Sie unter [Erstellen des Versionscodes für das APK](~/android/deploy-test/building-apps/abi-specific-apks.md).
    
    Einige Beispiele: Wenn `abi` `armeabi` ist und `versionCode` im Manifest `123` ist, generiert `{abi}{versionCode}` einen versionCode von `1123`, wenn `$(AndroidCreatePackagePerAbi)` TRUE ist, andernfalls wird ein Wert von 123 generiert.
    Wenn `abi` `x86_64` ist und `versionCode` im Manifest `44` ist: Dies generiert `544`, wenn `$(AndroidCreatePackagePerAbi)` TRUE ist, andernfalls wird ein Wert von `44` generiert.

    Wenn wir eine Formatzeichenfolge für Auffüllung links `{abi}{versionCode:0000}` einschließen, wird `50044` generiert, weil wir `versionCode` links mit `0` auffüllen. Alternativ können Sie z.B. die Dezimalauffüllung wie `{abi}{versionCode:D4}` verwenden, bei der das gleiche wie im vorherigen Beispiel geschieht.

    Nur die Formatzeichenfolgen „0“ und „Dx“ für Auffüllung werden unterstützt, da der Wert eine ganze Zahl sein MUSS.
    
    Vordefinierte Schlüsselelemente

    -   **abi** &ndash; Fügt die Ziel-ABI für die App ein.
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; Fügt den mindestens unterstützten SDK-Wert aus `AndroidManifest.xml` oder `11` ein, wenn kein solcher definiert ist.

    -   **versionCode** &ndash; Verwendet den Versionscode direkt aus `Properties\AndroidManifest.xml`. 

    Sie können mithilfe der `$(AndroidVersionCodeProperties)`-Eigenschaft (definiert als Nächstes) benutzerdefinierte Elemente definieren.

    Standardmäßig ist der Wert auf `{abi}{versionCode:D6}` festgelegt. Wenn Sie das vorherige Verhalten beibehalten möchten, können Sie die Standardeinstellung überschreiben, indem Sie die `$(AndroidUseLegacyVersionCode)`-Eigenschaft auf `true` festlegen.

    Hinzugefügt in Xamarin.Android 7.2.

-   **AndroidVersionCodeProperties** &ndash; Eine Zeichenfolgeneigenschaft, die es dem Entwickler erlaubt, benutzerdefinierte Elemente für die Verwendung mit `AndroidVersionCodePattern` zu definieren. Diese liegen in Form eines `key=value`-Paares vor. Alle Elemente im `value` sollten ganzzahlige Werte sein. Beispiel: `screen=23;target=$(_SupportedApiLevel)`. Wie Sie sehen können, können Sie vorhandene oder benutzerdefinierte MSBuild-Eigenschaften in der Zeichenfolge verwenden.

    Hinzugefügt in Xamarin.Android 7.2.

-   **AndroidUseLegacyVersionCode** &ndash; Eine boolesche Eigenschaft, die es dem Entwickler ermöglicht, die versionCode-Berechnung auf das frühere Verhalten vor Xamarin.Android 8.2 zurückzusetzen. Dies sollte NUR für Entwickler verwendet werden, die an im Google Play Store vorhandenen Anwendungen arbeiten. Es wird dringend empfohlen, die neue `$(AndroidVersionCodePattern)`-Eigenschaft zu verwenden.

    Hinzugefügt in Xamarin.Android 8.2.

-  **AndroidUseManagedDesignTimeResourceGenerator**: Eine boolesche Eigenschaft, die die Entwurfszeitbuilds umschaltet, damit diese anstelle von `aapt` den Parser für verwaltete Ressourcen verwenden.

    Hinzugefügt in Xamarin.Android 8.1.

-  **AndroidUseApkSigner**: Eine boolesche Eigenschaft, die es dem Entwickler ermöglicht, anstelle von `jarsigner` das `apksigner`-Tool zu verwenden.

    Hinzugefügt in Xamarin.Android 8.2.

-  **AndroidApkSignerAdditionalArguments**: Eine Zeichenfolgeneigenschaft, mit deren Hilfe der Entwickler im `apksigner`-Tool zusätzliche Argumente angeben kann.

    Hinzugefügt in Xamarin.Android 8.2.

### <a name="binding-project-build-properties"></a>Binden von Projektbuildeigenschaften

Die folgenden MSBuild-Eigenschaften werden mit [Bindungsprojekten](~/android/platform/binding-java-library/index.md) verwendet:

-   **AndroidClassParser** &ndash; Eine Zeichenfolgeneigenschaft, die steuert, wie `.jar`-Dateien analysiert werden. Mögliche Werte:

    - **class-parse**: Verwendet `class-parse.exe`, um Java-Bytecode direkt (ohne Unterstützung durch eine JVM) zu analysieren. Dieser Wert ist experimentell. 


    - **jar2xml**: Verwendet `jar2xml.jar`, um Java-Reflektion zu verwenden, um Typen und Member aus einer `.jar`-Datei zu extrahieren.

    `class-parse` besitzt die folgenden Vorteile im Vergleich zu `jar2xml`:

    - `class-parse` ist in der Lage, Parameternamen aus Java-Bytecode zu extrahieren, der *Debugsymbole* enthält (z.B. mit `javac -g` kompilierter Bytecode).

    - `class-parse` „überspringt“ keine Klassen, die von Membern nicht auflösbarer Typen erben oder diese enthalten.

    **Experimentell**. Hinzugefügt in Xamarin.Android 6.0.

    Der Standardwert ist `jar2xml`.

    Der Standardwert wird sich in einem zukünftigen Release ändern.

-   **AndroidCodegenTarget** &ndash; Eine Zeichenfolgeneigenschaft, die die Codegenerierungsziel-ABI steuert. Mögliche Werte:

    - **XamarinAndroid**: Verwendet die JNI-Bindungs-API, die seit Mono für Android 1.0 vorhanden ist. Bindungsassemblys, die mit Xamarin.Android 5.0 oder höher erstellt wurden, können nur unter Xamarin.Android 5.0 oder höher (API-/ABI-Erweiterungen) ausgeführt werden, aber die *Quelle* ist kompatibel mit früheren Produktversionen.

    - **XAJavaInterop1**: Verwendet Java.Interop für JNI-Aufrufe. Bindungsassemblys, die `XAJavaInterop1` verwenden, können nur mit Xamarin.Android 6.1 oder höher erstellt und ausgeführt werden. Xamarin.Android 6.1 oder höher bindet `Mono.Android.dll` mit diesem Wert.

      Die Vorteile von `XAJavaInterop1` umfassen Folgendes:

      - Kleinere Assemblys.

      - `jmethodID`-Zwischenspeicherung für `base` Methodenaufrufe, solange alle anderen Bindungstypen in der Vererbungshierarchie mit `XAJavaInterop1` oder höher erstellt werden.

      - `jmethodID`-Zwischenspeicherung von durch Java aufrufbare Wrapperkonstruktoren für verwaltete Unterklassen.

    Der Standardwert ist `XamarinAndroid`.

    Der Standardwert wird sich in einem zukünftigen Release ändern.


### <a name="resource-properties"></a>Ressourceneigenschaften

Ressourceneigenschaften steuern die Generierung der `Resource.designer.cs`-Datei, die Zugriff auf Android-Ressourcen ermöglicht. 

-   **AndroidResgenExtraArgs** &ndash; Gibt zusätzliche Befehlszeilenoptionen an, die an den Befehl **aapt** übergeben werden, wenn Android-Objekte und -Ressourcen verarbeitet werden.

-   **AndroidResgenFile** &ndash; Gibt den Namen der zu generierenden Ressourcendatei an. Die Standardvorlage legt diese Option auf `Resource.designer.cs` fest.

-   **MonoAndroidResourcePrefix** &ndash; Gibt ein *Pfadpräfix* an, das am Anfang von Dateinamen mit einer Buildaktion von `AndroidResource` entfernt wird. Damit soll ermöglicht werden, den Speicherort von Ressourcen zu ändern.

    Der Standardwert ist `Resources`. Ändern Sie diese Option für die Java-Projektstruktur in `res`.

-   **AndroidExplicitCrunch** &ndash; Wenn Sie eine App mit einer sehr großen Anzahl lokaler zeichenbarer Ressourcen erstellen, kann ein anfänglicher Buildprozess (oder erneuter Buildprozess) mehrere Minuten dauern. Um den Buildprozess zu beschleunigen, versuchen Sie, diese Eigenschaft einzuschließen und auf `True` festzulegen. Wenn diese Eigenschaft festgelegt ist, verarbeitet der Buildprozess PNG-Dateien vorab.

    **Experimentell**. Hinzugefügt in Xamarin.Android 7.0.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>Signatureigenschaften

Signatureigenschaften steuern, wie das Anwendungspaket signiert wird, damit es auf einem Android-Gerät installiert werden kann. Um eine schnellere Builditeration zu ermöglichen, signieren die Xamarin.Android-Tasks während des Buildprozesses keine Pakete, da das Signieren recht langsam ist. Stattdessen werden sie (wenn erforderlich) vor der Installation oder während des Exports von der IDE oder dem Buildziel *Install* signiert. Wenn Sie das Ziel *SignAndroidPackage* aufrufen, wird ein Paket mit dem Suffix `-Signed.apk` im Ausgabeverzeichnis generiert.

Standardmäßig generiert das Signaturziel bei Bedarf einen neuen Debugsignaturschlüssel. Wenn Sie einen bestimmten Schlüssel verwenden möchten (z.B. auf einem Buildserver), können die folgenden MSBuild-Eigenschaften verwendet werden:

-   **AndroidKeyStore** &ndash; Ein boolescher Wert, der angibt, ob benutzerdefinierte Signaturinformationen verwendet werden sollen. Der Standardwert ist `False`. Dies bedeutet, dass der Debugsignatur-Standardschlüssel verwendet wird, um Pakete zu signieren.

-   **AndroidSigningKeyAlias** &ndash; Gibt den Alias für den Schlüssel im Keystore an. Dies ist der Wert **keytool -alias**, der beim Erstellen des Keystore verwendet wird. 

-   **AndroidSigningKeyPass** &ndash; Gibt das Kennwort des Schlüssels in der Keystoredatei an. Dies ist der Wert, der eingegeben wird, wenn `keytool` die folgende Aufforderung ausgibt: **Enter key password for $(AndroidSigningKeyAlias)**.

-   **AndroidSigningKeyStore** &ndash; Gibt den Dateinamen der von `keytool` erstellten Keystoredatei an. Dies entspricht dem Wert, der der Option **keytool -keystore** zur Verfügung gestellt wird.

-   **AndroidSigningStorePass** &ndash; Gibt das Kennwort für `$(AndroidSigningKeyStore)` an. Dies ist der Wert, der `keytool` beim Erstellen der Keystoredatei zur Verfügung gestellt wird, wenn die folgende Aufforderung ausgegeben wird: **Enter keystore password:**. 

Betrachten Sie zum Beispiel den folgenden `keytool`-Aufruf:

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

Um den oben generierten Keystore zu verwenden, verwenden Sie die Eigenschaftengruppe:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm**: Gib den Standardalgorithmus zur Verwendung mit dem `debug.keystore` an. Sie wird standardmäßig auf `RSA` festgelegt.

-   **AndroidDebugKeyValidity**: Gibt den Standardwert für die Gültigkeit an, die für den `debug.keystore` verwendet werden soll. Die Eigenschaft wird standardmäßig auf `10950` oder `30 * 365` oder `30 years` festgelegt.

<a name="Build_Actions" />

## <a name="build-actions"></a>Buildvorgänge

*Buildaktionen* werden [auf Dateien](http://msdn.microsoft.com/en-us/library/bb629388.aspx) im Projekt angewendet und steuern, wie die Datei verarbeitet wird. 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Dateien mit einer Buildaktion von `AndroidEnvironment` werden verwendet, um [Umgebungsvariablen und Systemeigenschaften während des Prozessstarts zu initialisieren](~/android/deploy-test/environment.md).
Die Buildaktion `AndroidEnvironment` kann auf mehrere Dateien angewendet werden. Diese werden in keiner bestimmten Reihenfolge ausgewertet (geben Sie daher nicht die gleiche Umgebungsvariable oder Systemeigenschaft in mehreren Dateien an).


### <a name="androidjavasource"></a>AndroidJavaSource

Dateien mit einer Buildaktion von `AndroidJavaSource` sind Java-Quellcode, der im endgültigen Android-Paket enthalten sein wird.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Dateien mit einer Buildaktion von `AndroidJavaLibrary` sind Java-Archive (`.jar`-Dateien), die im endgültigen Android-Paket enthalten sein werden.


### <a name="androidresource"></a>AndroidResource

Alle Dateien mit einer *AndroidResource*-Buildaktion werden während des Buildprozesses in Android-Ressourcen kompiliert und über `$(AndroidResgenFile)` zugänglich gemacht.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Fortgeschrittene Benutzer wünschen sich vielleicht, dass verschiedene Ressourcen in verschiedenen Konfigurationen, aber mit demselben effektiven Pfad verwendet werden. Dies kann erreicht werden, indem mehrere Ressourcenverzeichnisse und Dateien mit den gleichen relativen Pfaden innerhalb dieser verschiedenen Verzeichnisse vorhanden sind und MSBuild-Bedingungen verwendet werden, um bedingt verschiedene Dateien in verschiedenen Konfigurationen einzubinden. Zum Beispiel:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; Gibt den Ressourcenpfad explizit an. Ermöglicht das &ldquo;Aliasing&rdquo; von Dateien, damit diese als mehrere unterschiedliche Ressourcennamen zur Verfügung stehen.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```


### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[Native Bibliotheken](~/android/platform/native-libraries.md) werden dem Build hinzugefügt, indem ihre Buildaktion auf `AndroidNativeLibrary` festgelegt wird.

Beachten Sie Folgendes: Da Android mehrere ABIs (Application Binary Interfaces) unterstützt, muss das Buildsystem wissen, für welche ABI die native Bibliothek erstellt wird. Es gibt zwei Möglichkeiten, dies zu erreichen:

1.  Pfadermittlung.
2.  Verwenden des `Abi`-Elementattributs.

Bei der Pfadermittlung wird der Name des übergeordneten Verzeichnisses der nativen Bibliothek verwendet, um die ABI anzugeben, die die Bibliothek als Ziel verwendet. Wenn Sie dem Build also `lib/armeabi/libfoo.so` hinzufügen, wird die ABI als `armeabi` „ermittelt“. 


#### <a name="item-attribute-name"></a>Elementattributname

**Abi** &ndash; Gibt die ABI der nativen Bibliothek an.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

Die Buildaktion von `AndroidAarLibrary` sollte verwendet werden, um direkt auf AAR-Dateien zu verweisen. Diese Buildaktion wird am häufigsten von Xamarin-Komponenten verwendet. Sie wird insbesondere verwendet, um Verweise auf AAR-Dateien einzuschließen, die erforderlich sind, damit Google Play und andere Dienste funktionieren.

Dateien mit dieser Buildaktion werden ähnlich behandelt wie die eingebetteten Ressourcen in Bibliotheksprojekten. Die AAR-Datei wird in das Zwischenverzeichnis extrahiert. Danach werden Objekte, Ressourcen und JAR-Dateien in die entsprechenden Elementgruppen eingefügt.  

### <a name="content"></a>Inhalt

Die normale `Content`-Buildaktion wird nicht unterstützt (da wir noch nicht herausgefunden haben, wie wir sie ohne einen möglicherweise kostspieligen ersten Ausführungsschritt unterstützen können).

Ab Xamarin.Android 5.1 führt der Versuch, die `@(Content)`-Buildaktion zu verwenden, zu einer `XA0101`-Warnung.

### <a name="linkdescription"></a>LinkDescription

Dateien mit einer *LinkDescription*-Buildaktion werden verwendet, um das [Verhalten des Linkers zu steuern](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

Dateien mit einer *ProguardConfiguration*-Buildaktion enthalten Optionen, mit denen das Verhalten von `proguard` gesteuert werden kann. Weitere Informationen zu dieser Buildaktion finden Sie unter [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Diese Dateien wird ignoriert, wenn die MSBuild-Eigenschaft `$(EnableProguard)` nicht den Wert `True` aufweist.


## <a name="target-definitions"></a>Zieldefinitionen

Die für Xamarin.Android spezifischen Teile des Buildprozesses werden in `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets` definiert, aber auch normale sprachspezifische Ziele wie *Microsoft.CSharp.targets* sind erforderlich, um die Assembly zu erstellen.

Die folgenden Buildeigenschaften müssen vor dem Importieren von Sprachzielen festgelegt werden:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Alle diese Ziele und Eigenschaften können für C# durch Importieren von *Xamarin.Android.CSharp.targets* eingebunden werden: 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Diese Datei kann problemlos für andere Sprachen angepasst werden.
