---
title: Preparing an Application for Release (Vorbereiten einer Anwendung auf die Veröffentlichung)
ms.prod: xamarin
ms.assetid: 9C8145B3-FCF1-4649-8C6A-49672DDA4159
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/21/2018
ms.openlocfilehash: 4ddae1ae4f49c01220b2f5ce78dc19122b3015a0
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525272"
---
# <a name="preparing-an-application-for-release"></a>Preparing an Application for Release (Vorbereiten einer Anwendung auf die Veröffentlichung)

Nachdem eine Anwendung codiert und getestet wurde, ist es erforderlich, ein Paket zur Verteilung vorzubereiten. Die erste Aufgabe bei der Vorbereitung dieses Pakets liegt darin, die Anwendung für die Veröffentlichung zu erstellen, was hauptsächlich mit dem Festlegen einiger Anwendungsattribute verbunden ist.

Führen Sie die folgenden Schritte aus, um die App zur Veröffentlichung zu erstellen:

- **[Angeben des Anwendungssymbols](#Specify_the_Application_Icon)** &ndash; Für jede Xamarin.Android-Anwendung muss ein Anwendungssymbol angegeben werden. Dies ist bei einigen Märkten wie Google Play erforderlich, obwohl keine technische Notwendigkeit besteht.

- **[Versionieren der Anwendung](#Versioning)** : In diesem Schritt werden die Versionsinformationen initialisiert oder aktualisiert. Dies ist wichtig für zukünftige Anwendungsupdates und um sicherzustellen, dass die Benutzer wissen, welche Version der Anwendung sie installiert haben.

- **[Verkleinern des APK](#shrink_apk)** &ndash; Die Größe des endgültigen APKs kann mithilfe des Linkers Xamarin.Android für den verwalteten Code sowie von ProGuard für den Java-Bytecode beträchtlich verringert werden.

- **[Schützen der Anwendung](#protect_app)** &ndash; Hindern Sie Benutzer oder Angreifer daran, die Anwendung zu debuggen, zu manipulieren oder zurückzuentwickeln, indem Sie Debugging deaktivieren, den verwalteten Code verbergen, Anti-Debug- und Anti-Manipulationsmaßnahmen hinzufügen und native Kompilierung verwenden.

- **[Festlegen von Paketeigenschaften](#Set_Packaging_Properties)** : Mit Paketeigenschaften wird die Erstellung des Android-Anwendungspakets (APK) gesteuert. Dieser Schritt optimiert das APK, schützt dessen Objekte und modularisiert das Paket nach Bedarf.

- **[Kompilieren](#Compile)** &ndash; In diesem Schritt werden Code und Objekte kompiliert, um das Erstellen im Releasemodus zu bestätigen.

- **[Archivieren zur Veröffentlichung](#archive)** &ndash; In diesem Schritt wird die App erstellt und zum Signieren und Veröffentlichen in einem Archiv platziert.

Jeder dieser Schritte wird unten genauer beschrieben.

<a name="Specify_the_Application_Icon" />

## <a name="specify-the-application-icon"></a>Angeben des Anwendungssymbols

Es wird dringend empfohlen, für jede Xamarin.Android-Anwendung ein Anwendungssymbol festzulegen. Einige Anwendungs-Marktplaces werden eine Veröffentlichung einer Android-Anwendung ohne ein solches Symbol nicht genehmigen. Die Eigenschaft `Icon` für das `Application`-Attribut (Anwendungsattribut) wird zur Festlegung eines Anwendungssymbols für ein Xamarin.Android-Projekt verwendet.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Geben Sie in Visual Studio 2017 und höher wie im folgenden Screenshot gezeigt das Anwendungssymbol über den Bereich **Android-Manifest** der **Eigenschaften** des Projekts an:

[![Anwendungssymbol festlegen](images/vs/01-application-icon-sml.png)](images/vs/01-application-icon.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

In Visual Studio für Mac ist es auch möglich, das Anwendungssymbol über den **Projektoptionen**-Bereich **Android-Anwendung** anzugeben, wie im folgenden Screenshot gezeigt:

[![Anwendungssymbol festlegen](images/xs/01-application-icon-sml.png)](images/xs/01-application-icon.png#lightbox)

-----

In diesen Beispielen bezieht sich `@drawable/icon` auf eine Symboldatei, die sich in **Resources/drawable/icon.png** befindet (Beachten Sie, dass die **.PNG**-Erweiterung nicht im Ressourcennamen enthalten ist). Das Attribut kann auch in der Datei **Properties\AssemblyInfo.cs** deklariert werden, wie im folgenden Beispiel gezeigt wird:

```csharp
[assembly: Application(Icon = "@drawable/icon")]
```

In der Regel wird `using Android.App` am oberen Rand von **AssemblyInfo.cs** deklariert (der Namespace des `Application`-Attributs ist `Android.App`). Dennoch müssen Sie diese `using`-Anweisung hinzufügen, falls sie noch nicht vorhanden ist.

<a name="Versioning" />

## <a name="version-the-application"></a>Versionieren der Anwendung

Die Versionsverwaltung ist wichtig für die Wartung und Verteilung von Android-Anwendungen. Ohne die Versionsverwaltung ist es schwierig zu bestimmen, ob oder wie eine Anwendung aktualisiert werden soll. Um Sie bei der Versionsverwaltung zu unterstützen, erkennt Android zwei verschiedene Arten von Informationen: 

- **Die Versionsnummer** &ndash; Ein ganzzahliger Wert, der intern von Android und der Anwendung verwendet wird und für die Version der Anwendung steht. In den meisten Anwendungen ist der Wert zunächst auf 1 festgelegt und wird mit jedem weiteren Build inkrementiert. Dieser Wert steht in keiner Verbindung oder Affinität zum Attribut des Versionsnamens (siehe unten). Anwendungen und Veröffentlichungsdienste sollten Benutzern diesen Wert nicht anzeigen. Dieser Wert wird in der Datei **AndroidManifest.xml** als `android:versionCode` gespeichert. 

- **Der Versionsname** &ndash; Eine Zeichenfolge, deren einziger Zweck es ist, dem Benutzer Informationen zur Version der Anwendung auf einem spezifischen Gerät bereitzustellen. Der Versionsname soll Benutzern angezeigt werden, z.B. in Google Play. Diese Zeichenfolge wird nicht intern von Android verwendet. Der Versionsname kann jeder beliebige Zeichenfolgenwert sein, anhand dessen ein Benutzer den Build bestimmen kann, der auf seinem Gerät installiert ist. Dieser Wert wird in der Datei **AndroidManifest.xml** als `android:versionName` gespeichert. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

In Visual Studio können diese Werte wie im folgenden Screenshot gezeigt über den Bereich **Android-Manifest** der **Eigenschaften** des Projekts festgelegt werden:

[![Versionsnummer festlegen](images/vs/02-versioning-sml.png)](images/vs/02-versioning.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Diese Werte können über den Bereich **Erstellen > Android-Anwendung** der **Projektoptionen** festgelegt werden wie im folgenden Screenshot gezeigt:

[![Versionsnummer festlegen](images/xs/02-versioning-sml.png)](images/xs/02-versioning.png#lightbox)

-----

<a name="shrink_apk" />

## <a name="shrink-the-apk"></a>Verkleinern der APK

Xamarin.Android-APKs können durch eine Kombination des Linkers Xamarin.Android zum Entfernen von unnötigem *verwalteten* Code sowie des Tools *ProGuard* aus dem Android SDK zum Entfernen von nicht verwendetem *Java-Bytecode* verkleinert werden. Vom Erstellungsprozess wird zuerst der Linker Xamarin.Android zum Optimieren der App auf Ebene des verwalteten Codes (C#) und anschließend ProGuard (sofern aktiviert) zum Optimieren der APK auf Java-Bytecode-Ebene verwendet.


### <a name="configure-the-linker"></a>Konfigurieren des Linkers

Der Releasemodus deaktiviert die freigegebene Laufzeit und aktiviert die Verknüpfung, sodass die Anwendung nur die Teile von Xamarin.Android enthält, die zur Laufzeit erforderlich sind. Der *Linker* in Xamarin.Android bestimmt mithilfe von statischen Analysen, welche Assemblys, Typen und Typmember von einer Xamarin.Android-Anwendung verwendet oder referenziert werden. Anschließend verwirft der Linker alle ungenutzten Assemblys, Typen und Member, die nicht verwendet (oder referenziert) werden. Dies kann zu einer erheblichen Verringerung der Paketgröße führen. Betrachten Sie beispielsweise das [HalloWelt](~/android/deploy-test/linker.md)-Beispiel, bei dem sich die endgültige Größe des zugehörigen Android-Anwendungspakets um 83% verringert: 

- Konfiguration: Keine &ndash; Xamarin.Android 4.2.5 Größe = 17,4 MB.

- Konfiguration: Nur SDK-Assemblys &ndash; Xamarin.Android 4.2.5 Größe = 3,0 MB.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Legen Sie die Optionen des Linkers über den Bereich **Android-Optionen** der **Eigenschaften** des Projekts fest:

[![Linkeroptionen](images/vs/03-linking-sml.png)](images/vs/03-linking.png#lightbox)

Das Pulldownmenü **Verknüpfen** enthält die folgenden Optionen zum Steuern des Linkers:

- **Keine** &ndash; Der Linker wird deaktiviert, und es erfolgt keine Verknüpfung.

- **Nur SDK-Assemblys**: Es werden nur die Assemblys verknüpft, die [von Xamarin.Android benötigt werden](~/cross-platform/internals/available-assemblies.md). 
    Andere Assemblys werden nicht verknüpft.

- **SDK und Benutzerassemblys** &ndash; Es werden alle Assemblys verknüpft, die von der Anwendung benötigt werden, nicht nur diejenigen, die von Xamarin.Android benötigt werden.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Legen Sie die Optionen des Linkers wie im folgenden Screenshot gezeigt über die Registerkarte **Linker** des Bereichs **Android-Build** der **Projektoptionen** fest:

[![Linkeroptionen](images/xs/03-linking-sml.png)](images/xs/03-linking.png#lightbox)

Es stehen folgende Optionen zum Steuern des Linkers zur Verfügung:

- **Keine Verknüpfung** &ndash; Der Linker wird deaktiviert, und es erfolgt keine Verknüpfung.

- **Nur SDK-Assemblys verknüpfen**: Es werden nur die Assemblys verknüpft, die [von Xamarin.Android benötigt werden](~/cross-platform/internals/available-assemblies.md). Andere Assemblys werden nicht verknüpft.

- **Alle Assemblys verknüpfen** &ndash; Es werden alle Assemblys verknüpft, die von der Anwendung benötigt werden, nicht nur diejenigen, die von Xamarin.Android benötigt werden.

-----

Durch das Verknüpfen können unerwartete Nebenwirkungen auftreten, daher ist es wichtig, dass eine Anwendung im Releasemodus auf einem physischen Gerät erneut getestet wird.

### <a name="proguard"></a>ProGuard

*ProGuard* ist ein Android SDK-Tool, das Java-Code verknüpft und verschleiert. ProGuard wird normalerweise verwendet, um kleinere Anwendungen zu erstellen, indem der Speicherbedarf für große enthaltene Bibliotheken (wie Google Play Services) in Ihrem APK reduziert wird. ProGuard entfernt nicht verwendeten Java-Bytecode, wodurch die resultierende App kleiner wird. Beispielsweise wird durch die Verwendung von ProGuard bei kleinen Xamarin.Android-Apps für gewöhnlich eine Größenreduzierung um etwa 24 % erzielt &ndash; bei größeren Apps mit mehreren Bibliotheksabhängigkeiten wird die Größe durch die Verwendung von ProGuard sogar noch weiter reduziert. 

ProGuard ist keine Alternative zum Xamarin.Android-Linker. Der Xamarin.Android-Linker verknüpft *verwalteten* Code, während ProGuard Java-Bytecode verknüpft. Der Buildprozess verwendet zunächst den Xamarin.Android-Linker, um den verwalteten (C#) Code in der App zu optimieren. Anschließend nutzt er ProGuard (wenn es aktiviert ist), um das APK auf der Java-Bytecode-Ebene zu optimieren. 

Wenn **Enable ProGuard** (ProGuard aktivieren) ausgewählt ist, führt Xamarin.Android das ProGuard-Tool auf dem resultierenden APK aus. Eine ProGuard-Konfigurationsdatei wird generiert und von ProGuard bei der Erstellung verwendet. Xamarin.Android unterstützt außerdem benutzerdefinierte *ProguardConfiguration*-Buildvorgänge. Sie können Ihrem Projekt eine benutzerdefinierte ProGuard-Konfigurationsdatei hinzufügen, mit der rechten Maustaste darauf klicken und sie als einen Buildvorgang auswählen, wie im folgenden Beispiel gezeigt wird: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Proguard-Buildvorgang](images/vs/05-proguard-build-action-sml.png)](images/vs/05-proguard-build-action.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Proguard-Buildvorgang](images/xs/05-proguard-build-action-sml.png)](images/xs/05-proguard-build-action.png#lightbox)

-----

ProGuard ist standardmäßig deaktiviert. Die Option **ProGuard aktivieren** ist nur dann verfügbar, wenn sich das Projekt im Modus **Release** befindet. Alle Buildaktionen von ProGuard werden ignoriert, wenn **ProGuard** deaktiviert ist. Die ProGuard-Konfiguration „Xamarin.Android“ verschleiert das APK nicht. Zudem ist es nicht möglich, die Obfuskation zu aktivieren, auch nicht mit benutzerdefinierten Konfigurationsdateien. Wenn Sie von der Obfuskation Gebrauch machen möchten, finden Sie unter [Application Protection with Dotfuscator (Anwendungsschutz mit Dotfuscator)](~/android/deploy-test/release-prep/index.md#dotfuscator) mehr Informationen. 

Ausführlichere Informationen zur Verwendung des ProGuard-Tools finden Sie unter [ProGuard](~/android/deploy-test/release-prep/proguard.md).

<a name="protect_app" />

## <a name="protect-the-application"></a>Schützen der Anwendung

<a name="Disable_Debugging" />

### <a name="disable-debugging"></a>Deaktivieren des Debuggens

Während der Entwicklung einer Android-Anwendung wird das Debuggen mit dem *Java Debug Wire Protocol* (JDWP) ausgeführt. Dies ist eine Technologie, die es Tools wie **adb** ermöglicht, für das Debuggen mit einer JVM zu kommunizieren. JDWP ist für die Debugbuilds einer Xamarin.Android-Anwendung standardmäßig aktiviert. Obwohl JDWP während der Entwicklung wichtig ist, kann es ein Sicherheitsproblem für freigegebene Anwendungen darstellen. 

> [!IMPORTANT]
> Deaktivieren Sie den Debugzustand in einer freigegebenen Anwendung nach Möglichkeit immer (über JDWP), um vollständigen Zugriff auf den Java-Prozess zu erhalten und beliebigen Code im Kontext der Anwendung auszuführen, wenn der Debugzustand nicht deaktiviert ist.

Das Android-Manifest enthält das `android:debuggable`-Attribut, das steuert, ob die Anwendung gedebuggt werden kann oder nicht. Es wird empfohlen, das `android:debuggable`-Attribut auf `false` festzulegen. Der einfachste Weg ist das Hinzufügen einer bedingten Kompilierungsanweisung in **AssemblyInfo.cs**: 

```csharp
#if DEBUG
[assembly: Application(Debuggable=true)]
#else
[assembly: Application(Debuggable=false)]
#endif
```

Beachten Sie, dass Debugbuilds automatisch manche Berechtigungen festlegen, um das Debuggen zu erleichtern (z.B. **Internet** und **ReadExternalStorage**). Releasebuilds verwenden jedoch nur die Berechtigungen, die sie explizit konfigurieren. Wenn Sie feststellen, dass Ihre App durch den Wechsel zum Releasebuild eine Berechtigung verliert, die im Debugbuild verfügbar war, überprüfen Sie, dass Sie diese Berechtigung wie in [Berechtigungen](~/android/app-fundamentals/permissions.md) beschrieben explizit in der Liste **Erforderliche Berechtigungen** aktiviert haben. 

<a name="dotfuscator" id="dotfuscator" />

### <a name="application-protection-with-dotfuscator"></a>Schützen der Anwendung mit Dotfuscator

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Auch mit der Einstellung [Debugging deaktiviert](#Disable_Debugging) ist es möglich, dass Angreifer Anwendungen erneut verpacken oder Konfigurationsoptionen bzw. -berechtigungen hinzufügen oder entfernen. Auf diese Weise können sie Anwendungen zurückentwickeln, debuggen oder manipulieren.
Sie können [Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) verwenden, um verwalteten Code zu verbergen und Laufzeitcode zur Erkennung des Sicherheitsstatus zur Buildzeit in eine Xamarin.Android-App einzufügen, um zu entdecken, ob die App auf einem Gerät mit entfernten Nutzungsbeschränkungen ausgeführt wird und darauf zu antworten.

Dotfuscator CE ist in Visual Studio 2017 enthalten.
Wenn Sie Dotfuscator verwenden möchten, klicken Sie auf **Tools > Präemptiver Schutz - Dotfuscator**.

Informationen zum Konfigurieren von Dotfuscator CE finden Sie unter [Using Dotfuscator Community Edition with Xamarin (Verwenden von Dotfuscator Community Edition mit Xamarin)](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Nach der Konfiguration schützt Dotfuscator CE automatisch jeden erstellten Build.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Auch mit der Einstellung [Debugging deaktiviert](#Disable_Debugging) ist es möglich, dass Angreifer Anwendungen erneut verpacken oder Konfigurationsoptionen bzw. -berechtigungen hinzufügen oder entfernen. Auf diese Weise können sie Anwendungen zurückentwickeln, debuggen oder manipulieren.
Sie können [Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) mit Visual Studio verwenden, um verwalteten Code zu verbergen und Laufzeitcode zur Erkennung des Sicherheitsstatus zur Buildzeit in eine Xamarin.Android-App einzufügen, um zu entdecken, ob die App auf einem Gerät mit entfernten Nutzungsbeschränkungen ausgeführt wird und darauf zu antworten, obwohl Visual Studio für Mac nicht unterstützt wird.

Informationen zum Konfigurieren von Dotfuscator CE finden Sie unter [Using Dotfuscator Community Edition with Xamarin (Verwenden von Dotfuscator Community Edition mit Xamarin)](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Nach der Konfiguration schützt Dotfuscator CE automatisch jeden erstellten Build.

-----

<a name="bundle" />

### <a name="bundle-assemblies-into-native-code"></a>Bündeln von Assemblys in nativem Code

Wenn diese Option aktiviert ist, werden Assemblys in einer nativen freigegebenen Bibliothek gebündelt. Durch diese Option bleibt der Code sicher. Sie schützt verwaltete Assemblys, indem sie diese in native Binärdateien einbettet.

Diese Option erfordert eine Unternehmenslizenz und ist nur verfügbar, wenn **Fast Deployment verwenden** deaktiviert ist. **Assemblys in nativem Code bündeln** ist standardmäßig deaktiviert.

Beachten Sie, dass die Option **Assemblys in nativem Code bündeln** *nicht* bedeutet, dass die Assemblys in nativen Code kompiliert werden. Es ist nicht möglich, die [**AOT-Kompilierung**](#aot) zu verwenden, um Assemblys in nativen Code zu kompilieren (derzeit nur eine experimentelle Funktion und nicht für die Verwendung in der Produktion geeignet).

<a name="aot" />

### <a name="aot-compilation"></a>AOT-Kompilierung

Die Option **AOT Kompilierung** (unter [Packaging Properties (Paketeigenschaften)](#Set_Packaging_Properties)) aktiviert die Ahead-of-Time-Kompilierung von Assemblys. Wenn diese Option aktiviert ist, wird der Mehraufwand eines Just-In-Time-Starts minimiert, indem Assemblys vor dem Ausführen vorkompiliert werden. Der native Code, der sich daraus ergibt, ist genauso wie die nicht kompilierten Assemblys im APK enthalten. Dadurch wird zwar einerseits die Startzeit verringert, andererseits sind jedoch die APK etwas größer.

Um die Option **AOT-Kompilierung** ausführen zu können, müssen Sie mindestens über eine Enterprise-Lizenz verfügen. Die **AOT Kompilierung** ist nur verfügbar, wenn das Projekt für den Releasemodus konfiguriert ist. Sie ist standardmäßig deaktiviert. Weitere Informationen über die AOT-Kompilierung finden Sie unter [AOT](https://www.mono-project.com/docs/advanced/aot/).

#### <a name="llvm-optimizing-compiler"></a>LLVM-Optimierungscompiler

Der _LLVM-Optimierungscompiler_ erstellt kürzeren und schneller kompilierbaren Code und konvertiert AOT-kompatible Assemblys in nativen Code. Dies führt jedoch zu erhöhten Buildzeiten. Der LLVM-Compiler ist standardmäßig deaktiviert. Um den LLVM-Compiler nutzen zu können, muss zunächst auf der Seite [Verpackungseigenschaften](#Set_Packaging_Properties) die Option **AOT-Kompilierung** aktiviert werden.


> [!NOTE]
> Für die Option **LLVM-Optimierungscompiler** ist eine Enterprise-Lizenz erforderlich.  

<a name="Set_Packaging_Properties" />

## <a name="set-packaging-properties"></a>Festlegen von Paketeigenschaften

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Paketeigenschaften können über den Bereich **Android-Optionen** der **Eigenschaften** des Projekts festgelegt werden wie im folgenden Screenshot gezeigt:

[![Paketeigenschaften](images/vs/04-packaging-sml.png)](images/vs/04-packaging.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Paketeigenschaften können wie im folgenden Screenshot gezeigt in den **Projektoptionen** festgelegt werden:

[![Paketeigenschaften](images/xs/04-packaging-sml.png)](images/xs/04-packaging.png#lightbox)

-----

Viele dieser Eigenschaften, wie **Shared Runtime verwenden** und **Fast Deployment verwenden**, sind für den Debugmodus konzipiert. Wenn die Anwendung jedoch für den Releasemodus konfiguriert ist, bestimmen andere Einstellungen, wie die App [für Größe und Ausführungsgeschwindigkeit optimiert wird](#shrink_apk), [wie sie vor Manipulationen geschützt wird](#protect_app) und wie sie verpackt werden kann, damit verschiedene Architekturen und Größeneinschränkungen unterstützt werden.

### <a name="specify-supported-architectures"></a>Angeben unterstützter Architekturen

Wenn Sie eine Xamarin.Android-App auf die Veröffentlichung vorbereiten, müssen Sie unbedingt die unterstützten CPU-Architekturen angeben. Ein einzelnes APK kann Computercode enthalten, der mehrere unterschiedliche Architekturen unterstützt. Weitere Angaben zur Unterstützung mehrere CPU-Architekturen finden Sie unter [CPU-Architekturen](~/android/app-fundamentals/cpu-architectures.md).

### <a name="generate-one-package-apk-per-selected-abi"></a>Generieren eines Pakets (.APK) pro ausgewählter ABI

Wenn diese Option aktiviert ist, wird ein APK für jede der unterstützten ABIs erstellt (wird in der **Erweitert**-Registerkarte ausgewählt, wie in [CPU Architectures (CPU-Architekturen)](~/android/app-fundamentals/cpu-architectures.md) beschrieben), anstatt eines einzigen, großen APK für alle unterstützten ABIs. Diese Option steht nur zur Verfügung, wenn das Projekt für den Releasemodus konfiguriert ist, und sie ist standardmäßig deaktiviert.

### <a name="multi-dex"></a>Multi-Dex

Wenn die Option **Multi-Dex aktivieren** aktiviert ist, werden die Android SDK Tools dazu verwendet, das 65K-Limit für Methoden des Dateiformats **.dex** zu umgehen. Das 65K-Limit für Methoden basiert auf der Anzahl von Java-Methoden, auf die eine App _verweist_ (einschließlich derer in Bibliotheken, von denen die App abhängt) &ndash; es hängt nicht von der Anzahl von Methoden ab, die _im Quellcode geschrieben_ sind. Wenn eine Anwendung nur wenige Methoden definiert, jedoch viele (oder große Bibliotheken) verwendet, ist es möglich, dass das 65K-Limit erweitert wird.

Es ist möglich, dass eine App nicht jede Methode in jeder Bibliothek verwendet, auf die verwiesen wird. Daher ist es möglich, dass ein Tool wie ProGuard (siehe oben) die nicht verwendeten Methoden aus dem Code entfernen kann. Die bewährte Methode ist, die Option **Multi-Dex aktivieren** nur zu aktivieren, wenn dies unbedingt notwendig ist. D.h., dass die App selbst nach der Verwendung von ProGuard noch auf mehr als 65K Java-Methoden verweist.

Weitere Informationen zu Multi-Dex finden Sie unter [Configure Apps with Over 64K Methods (Konfigurieren von Apps, die mehr als 65535 Methoden enthalten)](https://developer.android.com/tools/building/multidex.html).

<a name="Compile" />

## <a name="compile"></a>Compile

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Nachdem alle oben genannten Schritte abgeschlossen wurden, ist die App bereit zur Kompilierung. Wählen Sie **Erstellen > Projektmappe neu erstellen** aus, damit sie erfolgreich im Releasemodus erstellt wird. Beachten Sie, dass in diesem Schritt noch kein APK erstellt wird.

Unter [Signing the App Package (Signieren des App-Pakets)](~/android/deploy-test/signing/index.md) werden der Packvorgang und die Anmeldung detaillierter beschrieben.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Nachdem alle oben aufgeführten Schritte abgeschlossen wurden, kompilieren Sie die Anwendung (wählen Sie **Erstellen > Alle erstellen** aus), damit sie erfolgreich im Releasemodus erstellt wird. Beachten Sie, dass in diesem Schritt noch kein APK erstellt wird.

-----

<a name="archive" />

## <a name="archive-for-publishing"></a>Archivieren zur Veröffentlichung

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Klicken Sie mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer**, und wählen Sie das Kontextmenüelement **Archiv ...**  aus, um mit der Veröffentlichung zu beginnen:

[![Archiv-App](images/vs/07-archive-for-publishing-sml.png)](images/vs/07-archive-for-publishing.png#lightbox)

Mit **Archiv...** wird der **Archiv-Manager** geöffnet und der Archivierungsprozess der App-Bündel gestartet wie in diesem Screenshot gezeigt:

[![Archiv-Manager](images/vs/08-archive-manager-sml.png)](images/vs/08-archive-manager.png#lightbox)

Eine andere Möglichkeit, ein Archiv zu erstellen, besteht darin, mit der rechten Maustaste auf die Projektmappe im **Projektmappen-Explorer** zu klicken und **Alle archivieren...** auszuwählen. Dadurch wird die Projektmappe erstellt, und alle Xamarin-Projekte, aus denen ein Archiv generiert werden kann, werden archiviert:

[![Alle archivieren](images/vs/09-archive-all-sml.png)](images/vs/09-archive-all.png#lightbox)

Sowohl mit **Archivieren** als auch mit **Alle archivieren** wird der **Archiv-Manager** automatisch geöffnet. Zum direkten Starten des **Archiv-Managers** klicken Sie auf das Menüelement **Tools > Archiv-Manager ...** :

[![Archiv-Manager starten](images/vs/10-launch-archive-manager-sml.png)](images/vs/10-launch-archive-manager.png#lightbox)

Sie können die Archive der Projektmappen jederzeit ansehen, indem Sie mit der rechten Maustaste auf den Knoten **Projektmappe** klicken und **Archivansicht** auswählen:

[![Archivansicht](images/vs/11-view-archives-sml.png)](images/vs/11-view-archives.png#lightbox)

### <a name="the-archive-manager"></a>Der Archiv-Manager

Der **Archiv-Manager** besteht aus den Bereichen **Projektmappenliste**, **Archivliste** und **Details**:

[![Archiv-Manager-Bereiche](images/vs/12-archive-manager-detail-sml.png)](images/vs/12-archive-manager-detail.png#lightbox)

In der **Projektmappenliste** werden alle Projektmappen mit mindestens einem archivierten Projekt angezeigt. Die **Projektmappenliste** enthält die folgenden Bereiche:

* **Aktuelle Projektmappe**: Die aktuelle Projektmappe wird angezeigt. Beachten Sie, dass dieser Bereich möglicherweise leer ist, wenn die aktuelle Projektmappe kein vorhandenes Archiv aufweist.
* **Alle Projektmappen**: Es werden alle Projektmappen angezeigt, die ein Archiv aufweisen.
* **Suchen** Textfeld (oben): Die in der Liste **Alle Projektmappen** aufgeführten Projektmappen werden entsprechend der Suchzeichenfolge gefiltert, die in das Textfeld eingegeben wurde.

In der **Archivliste** werden alle Archive der ausgewählten Projektmappe angezeigt. Die **Archivliste** enthält die folgenden Bereiche:

* **Name der ausgewählten Projektmappe**: Es wird der Name der Projektmappe angezeigt, die in der **Projektmappenliste** ausgewählt wurde. Alle Informationen in der **Archivliste** beziehen sich auf diese ausgewählte Projektmappe.
* **Plattformfilter**: In diesem Feld können Archive nach Plattformtyp (z.B. iOS oder Android) gefiltert werden.
* **Elemente archivieren**: Es wird eine Liste der Archive der ausgewählten Projektmappe angezeigt. Jedes Element in dieser Liste enthält Projektnamen, Erstellungsdatum und Plattform. Es können auch zusätzliche Informationen wie der Fortschritt beim Archivieren oder Veröffentlichen eines Elements angezeigt werden.

Im **Bereich „Details“** werden zusätzliche Informationen zu jedem Archiv angezeigt. Hier können Benutzer auch den Workflow für die Verteilung starten oder den Ordner öffnen, in dem die Verteilung erstellt wurde. Im Bereich **Kommentare erstellen** können Build-Kommentare in das Archiv eingeschlossen werden.

### <a name="distribution"></a>Verteilung

Wenn eine archivierte Version der Anwendung zur Veröffentlichung bereit ist, wählen Sie das Archiv im **Archiv-Manager** aus und klicken auf die Schaltfläche **Verteilen...** :

[![Schaltfläche „Verteilen“](images/vs/13-distribute-sml.png)](images/vs/13-distribute.png#lightbox)

Im Dialogfeld **Vertriebskanal** werden Informationen zur App, zum Fortschritt des Verteilungs-Workflows sowie eine Vertriebskanal-Auswahl angezeigt. Beim ersten Ausführen werden zwei Optionen angezeigt:

[![Vertriebskanal auswählen](images/vs/14-distribution-channel-sml.png)](images/vs/14-distribution-channel.png#lightbox)

Es kann einer der folgenden Vertriebskanäle ausgewählt werden:

* **Ad-Hoc**: Es wird ein signiertes APK auf dem Datenträger gespeichert, das auf Android-Geräte quergeladen werden kann. Fahren Sie mit dem Artikel [Signing the App Package (Signieren des App-Pakets)](~/android/deploy-test/signing/index.md) fort, um zu erfahren, wie eine Android-Signierungsidentität und ein neues Signaturzertifikat für Android-Anwendungen erstellt und eine _Ad-Hoc_-Version der App auf dem Datenträger veröffentlicht werden kann. Dies ist eine gute Möglichkeit, ein APK für Testzwecke zu erstellen.

* **Google Play**: veröffentlicht ein signiertes APK bei Google Play. Fahren Sie mit dem Artikel [Publishing to Google Play (Veröffentlichen in Google Play)](~/android/deploy-test/publishing/publishing-to-google-play/index.md) fort, um zu erfahren, wie Sie ein APK im Google Play Store signieren und veröffentlichen können.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wählen Sie **Erstellen > Archivieren zur Veröffentlichung** aus, um den Veröffentlichungsprozess zu starten:

[![Archivieren zur Veröffentlichung](images/xs/07-archive-for-publishing-sml.png)](images/xs/07-archive-for-publishing.png#lightbox)

Mit **Archivieren zur Veröffentlichung** wird das Projekt erstellt und in einer Archivdatei gebündelt. Mit der Menüoption **Alle archivieren** werden alle Projekte in der Projektmappe archiviert, die archiviert werden können. Bei beiden Optionen wird automatisch der **Archiv-Manager** geöffnet, wenn das Erstellen und Bündeln abgeschlossen ist:

[![Archivansicht](images/xs/08-archives-view-sml.png)](images/xs/08-archives-view.png#lightbox)

In diesem Beispiel wird im **Archiv-Manager** nur eine archivierte Anwendung aufgeführt: **Meine App**. Beachten Sie, dass im Kommentarfeld ein kurzer Kommentar zum Archiv gespeichert werden kann. Um eine archivierte Version einer Xamarin.Android-Anwendung zu veröffentlichen, wählen Sie die App im **Archiv-Manager** aus und klicken auf **Anmelden und Verteilen ...**  wie oben gezeigt. Im daraufhin geöffneten Dialogfeld **Anmelden und Verteilen** haben Sie zwei Auswahlmöglichkeiten:

[![Signieren und Verteilen](images/xs/09-sign-and-distribute-sml.png)](images/xs/09-sign-and-distribute.png#lightbox)

Hier können Sie den Vertriebskanal auswählen:

- **Ad-Hoc** &ndash; Es wird ein signiertes APK auf dem Datenträger gespeichert, damit es auf Android-Geräte quergeladen werden kann. Fahren Sie mit dem Artikel [Signing the App Package (Signieren des App-Pakets)](~/android/deploy-test/signing/index.md) fort, um zu erfahren, wie eine Android-Signierungsidentität und ein neues Signaturzertifikat für Android-Anwendungen erstellt und eine &ldquo;Ad-Hoc&rdquo;-Version der App auf dem Datenträger veröffentlicht werden kann. Dies ist eine gute Möglichkeit, ein APK für Testzwecke zu erstellen.


- **Google Play**: veröffentlicht ein signiertes APK bei Google Play.
    Fahren Sie mit dem Artikel [Publishing to Google Play (Veröffentlichen in Google Play)](~/android/deploy-test/publishing/publishing-to-google-play/index.md) fort, um zu erfahren, wie Sie ein APK im Google Play Store signieren und veröffentlichen können.

-----

## <a name="related-links"></a>Verwandte Links

- [Geräte mit mehreren Kernen und Xamarin.Android](~/android/deploy-test/multicore-devices.md)
- [CPU-Architekturen](~/android/app-fundamentals/cpu-architectures.md)
- [AOT](https://www.mono-project.com/docs/advanced/aot/)
- [Verkleinern des Codes und der Ressourcen](https://developer.android.com/tools/help/proguard.html)
- [Konfigurieren von Apps, die mehr als 65535 Methoden enthalten](https://developer.android.com/tools/building/multidex.html)
