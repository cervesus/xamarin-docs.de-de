---
title: Features von Lollipop
description: Dieser Artikel bietet einen allgemeinen Überblick über die in Android 5.0 (Lollipop) eingeführten neuen Features. Zu diesen Features gehören ein neuer Benutzeroberflächenstil, das sogenannte „Material Theme“, sowie neue unterstützende Funktionen wie etwa Animationen, Ansichtsschatten und das Einfärben von zeichenbaren Ressourcen. Android 5.0 bietet darüber hinaus verbesserte Benachrichtigungen, zwei neue Benutzeroberflächenwidgets, einen neuen Auftragsplaner und eine Handvoll neuer APIs zur Verbesserung der Speicher-, Netzwerk-, Konnektivitäts- und Multimediafunktionen.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 297c7806ce8a880d65c38ef0e4672e41fee5acfe
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724442"
---
# <a name="lollipop-features"></a>Features von Lollipop

_Dieser Artikel bietet einen allgemeinen Überblick über die in Android 5.0 (Lollipop) eingeführten neuen Features. Zu diesen Features gehören ein neuer Benutzeroberflächenstil, das sogenannte „Material Theme“, sowie neue unterstützende Funktionen wie etwa Animationen, Ansichtsschatten und das Einfärben von zeichenbaren Ressourcen. Android 5.0 bietet darüber hinaus verbesserte Benachrichtigungen, zwei neue Benutzeroberflächenwidgets, einen neuen Auftragsplaner und eine Handvoll neuer APIs zur Verbesserung der Speicher-, Netzwerk-, Konnektivitäts- und Multimediafunktionen._

## <a name="lollipop-overview"></a>Übersicht über Lollipop

Android 5.0 (Lollipop) führt eine neue Designsprache – das *Material Design* – ein, und mit ihr verschiedene neue unterstützende Features, die Apps einfacher und intuitiver bedienbar machen. Mit dem Material Design von Android 5.0 erhalten Android-Smartphones nicht nur ein Facelift, darüber hinaus werden auch neue Entwurfsregeln für Android-basierte Tablets, Desktopcomputer, Uhren und Smart-TVs bereitgestellt. Diese Entwurfsregeln rücken Einfachheit und Minimalismus in den Vordergrund und nutzen gleichzeitig vertraute taktile Attribute (beispielsweise realistische Oberflächen- und Kantenmerkmale), um dem Benutzer ein schnelles und intuitives Verständnis der Benutzeroberfläche zu ermöglichen.

Das *Material Theme* ist die Umsetzung dieser Entwurfsregeln für das Benutzeroberflächendesign in Android. Im vorliegenden Artikel werden zunächst die folgenden unterstützenden Features für das Material Theme erläutert:

- **Animationen** &ndash; *Touchfeedback*-Animationen, Animationen für *Aktivitätsübergänge*, Animationen für *Anzeigestatusübergänge* und einen *Enthüllungseffekt*.

- **Ansichtsschatten und Anzeigeebenen** &ndash; Ansichten umfassen ab sofort eine Eigenschaft `elevation`, Ansichten mit höheren `elevation`-Werten werfen größere Schatten auf den Hintergrund.

- **Farbfeatures** &ndash; Mit dem Feature zum *Einfärben von zeichenbaren Ressourcen* ist es möglich, Bildressourcen durch Änderung ihrer Farbe wiederzuverwenden, und über das Feature zum *Extrahieren markanter Farben* können Sie das Design Ihrer App basierend auf den Farben in einem Bild dynamisch gestalten.

Zahlreiche Features des Material Theme sind bereits in die Android 5.0-Benutzeroberfläche integriert, andere müssen den Apps explizit hinzugefügt werden. Beispielsweise umfassen einige Standardansichten (und Schaltflächen) bereits Touchfeedback-Animationen, während in Apps die meisten Ansichtsschatten erst aktiviert werden müssen.

Zusätzlich zu den Benutzeroberflächenverbesserungen durch das Material Theme umfasst Android 5.0 verschiedene weitere neue Features, die in diesem Artikel abgedeckt werden:

- **Verbesserte Benachrichtigungen** &ndash; Die Benachrichtigungen in Android 5.0 wurden durch ein neues Aussehen, die Unterstützung von Benachrichtigungen auf dem Sperrbildschirm und eine neues *Heads-up*-Darstellungsformat für Benachrichtigungen erheblich verbessert.

- **Neue Benutzeroberflächenwidgets** &ndash; Durch das neue `RecyclerView`-Widget können Apps große Datensätze und komplexe Informationen besser auf den Bildschirm übertragen, und das neue `CardView`-Widget bietet ein vereinfachtes Darstellungsformat im Kartenstil für die Anzeige von Text und Bildern.

- **Neue APIs** &ndash; Android 5.0 stellt neue APIs für umfangreiche Netzwerkunterstützung, verbesserte Bluetooth-Konnektivität, einfachere Speicherverwaltung und eine flexiblere Steuerung von Multimediaplayern und Kamerageräten bereit. Ferner steht ein neues Feature für die Auftragsplanung zur Verfügung, mit dem Aufgaben zu geplanten Zeiten asynchron ausgeführt werden können. Dieses Feature verbessert die Akkulaufzeit, indem Aufgaben beispielsweise dann ausgeführt werden, wenn das Gerät an das Stromnetz angeschlossen ist und aufgeladen wird.

## <a name="requirements"></a>Anforderungen

Für die Verwendung von Android 5.0-Features in Xamarin-basierten Apps gelten die folgenden Voraussetzungen:

- **Xamarin.Android** &ndash; Xamarin.Android 4.20 oder höher muss entweder mit Visual Studio oder Visual Studio für Mac installiert und konfiguriert sein.

- **Android SDK** &ndash; Android 5.0 (API 21) oder höher muss über den Android-SDK-Manager installiert werden.

- **Java Developer Kit** &ndash; Xamarin.Android erfordert [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 1.8 unterstützt auch niedrigere API-Ebenen als Ebene 24, Lollipop eingeschlossen). Die 64-Bit-Version von JDK 1.8 wird benötigt, wenn Sie benutzerdefinierte Steuerelemente oder die Forms-Vorschau verwenden.

Sie können weiterhin [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie speziell für API-Ebene 23 oder früher entwickeln.

## <a name="setting-up-an-android-50-project"></a>Einrichten eines Android 5.0-Projekts

Um ein Android 5.0-Projekt zu erstellen, müssen Sie die neuesten Tools und SDK-Pakete installieren. Führen Sie die folgenden Schritte aus, um ein Xamarin.Android-Projekt für Android 5.0 einzurichten:

1. Installieren Sie die Xamarin.Android-Tools, und aktivieren Sie Ihre Xamarin-Lizenz. Weitere Informationen zum Installieren von Xamarin.Android finden Sie unter [Setup und Installation](~/android/get-started/installation/index.md).

2. Wenn Sie Visual Studio für Mac verwenden, installieren Sie die neuesten Android 5.0-Updates.

3. Starten Sie den Android-SDK-Manager (verwenden Sie in Visual Studio für Mac **Extras &gt; Android-SDK-Manager öffnen&hellip;** ), und installieren Sie die Android SDK Tools 23.0.5 oder höher:

    [![Auswählen der Android SDK Tools im Android-SDK-Manager](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   Installieren Sie außerdem die neuesten Android 5.0 SDK-Pakete (API 21 oder höher):

    [![Installieren der Android 5.0 SDK-Pakete im Android-SDK-Manager](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Weitere Informationen zur Verwendung des Android-SDK-Managers finden Sie im Artikel zum [SDK-Manager](https://developer.android.com/tools/help/sdk-manager.html).

4. Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie mit der Android-Entwicklung mit Xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Android-Projekten. Wenn Sie ein Android-Projekt erstellen, müssen Sie darauf achten, die Versionseinstellungen für Android 5.0 zu konfigurieren.
   Navigieren Sie in Visual Studio für Mac zu **Projektoptionen &gt; Erstellen &gt; Allgemein**, und legen Sie das **Zielframework** auf **Android 5.0 (Lollipop)** oder höher fest:

    ![Festlegen des Zielframeworks auf Android 5.0 (Lollipop)](lollipop-images/target-framework.png)

   Legen Sie unter **Projektoptionen &gt; Erstellen &gt; Android-Anwendung** die Mindestversion und die Android-Zielversion auf **Automatisch – Zielframeworkversion verwenden** fest:

    ![Festlegen von Mindestversion und Android-Zielversion auf „Automatisch“](lollipop-images/minimum-android-version.png)

5. Konfigurieren Sie einen Emulator auf einem Android-Gerät, um Ihre App zu testen. Wenn Sie einen Emulator verwenden, finden Sie unter [Setup von Android-Emulator](~/android/get-started/installation/android-emulator/index.md) Informationen zum Konfigurieren eines Android-Emulators zur Verwendung mit Xamarin Studio oder Visual Studio. Wenn Sie ein Android-Gerät verwenden, finden Sie im Artikel zum [Einrichten des Preview SDK](https://developer.android.com/preview/setup-sdk.html) Informationen zum Aktualisieren Ihres Geräts für Android 5.0. Informationen zum Konfigurieren Ihres Android-Geräts für das Ausführen und Debuggen von Xamarin.Android-Anwendungen finden Sie unter [Einrichten eines Geräts für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md).

Hinweis: Wenn Sie ein vorhandenes Android-Projekt aktualisieren, das für „Android L Preview“ konfiguriert war, müssen Sie das **Zielframework** und die **Android-Version** auf die oben beschriebenen Werte aktualisieren.

## <a name="important-changes"></a>Wichtige Änderungen

Bereits veröffentlichte Apps können von Änderungen in Android 5.0 betroffen sein. Insbesondere verwendet Android 5.0 eine neue Runtime und ein erheblich geändertes Benachrichtigungsformat.

### <a name="android-runtime"></a>Android-Runtime

Android 5.0 verwendet anstelle von Dalvik die neue Android-Runtime (ART) als Standardruntime. ART implementiert einige wichtige neue Features:

- **AOT-Kompilierung (Ahead-of-time)** &ndash; AOT kann die App-Leistung steigern, indem der App-Code vor dem ersten Starten der App kompiliert wird. Wenn eine App installiert wird, generiert die Android-Runtime eine kompilierte ausführbare App-Datei für das Zielgerät.

- **Verbesserte Garbage Collection (GC)** &ndash; Die GC-Verbesserungen in der Android-Runtime können auch die App-Leistung verbessern. Die Garbage Collection verwendet jetzt eine GC-Pausen statt zwei, und parallele GC-Vorgänge werden jetzt schneller abgeschlossen.

- **Verbessertes App-Debuggen** &ndash; Die Android-Runtime stellt mehr Diagnosedetails bereit, um die Analyse von Ausnahmen und Absturzberichten zu unterstützen.

Vorhandene Apps sollten ohne Änderungen in der Android-Runtime funktionieren &ndash; mit Ausnahme von Apps, die spezifische Techniken der vorherigen Dalvik-Runtime nutzen, die in der Android-Runtime möglicherweise nicht funktionieren. Weitere Informationen zu diesen Änderungen finden Sie unter [Verifying App Behavior on the Android Runtime (ART)](https://developer.android.com/guide/practices/verifying-apps-art.html) (Überprüfen des App-Verhaltens in der Android-Runtime).

### <a name="notification-changes"></a>Änderungen an Benachrichtigungen

An den Benachrichtigungen wurden in Android 5.0 erhebliche Änderungen vorgenommen:

- **Andere Verarbeitung von Sounds und Vibrationen** &ndash; Benachrichtigungssounds und Vibrationen werden jetzt von `Notification.Builder` und nicht mehr von `Ringtone`, `MediaPlayer` und `Vibrator` verarbeitet.

- **Neues Farbschema** &ndash; In Abstimmung auf das Material Theme werden Benachrichtigungen mit dunklem Text auf einem weißen oder sehr hellen Hintergrund gerendert. Außerdem können Alphakanäle in Benachrichtigungssymbolen zur Anpassung an ein Systemfarbschema durch Android geändert werden.

- **Benachrichtigungen auf dem Sperrbildschirm** &ndash; Ab sofort können Benachrichtigungen auf dem Sperrbildschirm angezeigt werden.

- **Heads-up**-Benachrichtigungen &ndash; Benachrichtigungen mit hoher Priorität werden ab sofort in einem kleinen unverankerten Fenster (Heads-up-Benachrichtigung) angezeigt, wenn das Gerät entsperrt und das Display aktiviert wird.

In den meisten Fällen müssen sind Portieren der vorhandenen App-Benachrichtigungsfunktionalität zu Android 5.0 die folgenden Schritte erforderlich:

1. Konvertieren Sie Ihren Code zur Verwendung von `Notification.Builder` (oder `NotificationsCompat.Builder`) für das Erstellen von Benachrichtigungen.

2. Stellen Sie sicher, dass Ihre vorhandenen Benachrichtigungsressourcen im neuen Farbschema des Material Theme angezeigt werden können.

3. Legen Sie die Sichtbarkeit für Ihre Benachrichtigungen fest, wenn diese auf dem Sperrbildschirm angezeigt werden. Wenn eine Benachrichtigung nicht öffentlich ist, welcher Inhalt sollte auf dem Sperrbildschirm angezeigt werden?

4. Legen Sie die Kategorie Ihrer Benachrichtigungen fest, damit sie im neuen *Nicht stören*-Modus von Android 5.0 richtig verarbeitet werden.

Wenn Ihre Benachrichtigungen Transportsteuerelemente enthalten, den Status der Medienwiedergabe anzeigen, `RemoteControlClient` verwenden oder `ActivityManager.GetRecentTasks` aufrufen, finden Sie unter [Important Behavior Changes](https://developer.android.com/preview/api-overview.html#Behaviors) (Wichtige Verhaltensänderungen) weitere Informationen zur Aktualisierung Ihrer Benachrichtigungen für Android 5.0.

Weitere Informationen zum Erstellen von Benachrichtigungen in Android finden Sie unter [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

## <a name="material-theme"></a>Materialdesign

Das neue Material Theme von Android 5.0 umfasst weitreichende Änderungen in Bezug auf das Aussehen und Verhalten der Benutzeroberfläche. Visuelle Elemente verwenden ab sofort taktile Oberflächen, die die plakativen Grafiken, die Typografie und die leuchtenden Farben des druckbasierten Designs übernehmen. Die folgenden Screenshots zeigen Beispiele aus dem Material Theme:

[![Screenshots von Startbildschirm, App-Bildschirm und Bildschirm mit Einstellungen im Material Theme](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 begrüßt Sie mit dem links abgebildeten Startbildschirm. Der Screenshot in der Mitte zeigt den ersten Bildschirm der App-Liste, der Screenshot rechts zeigt den Bildschirm **Einstellungen**. In der Google-Spezifikation für das [Material Design](https://material.io/guidelines/material-design/introduction.html) werden die Entwurfsregeln erläutert, die dem neuen Konzept des Material Theme zugrunde liegen.

Das Material Theme umfasst drei integrierte Varianten, die Sie in Ihrer App verwenden können: das dunkle `Theme.Material`-Design (die Standardeinstellung), das `Theme.Material.Light`-Design und das `Theme.Material.Light.DarkActionBar`-Design:

[![Screenshots der Dark-, Light- und DarkActionBar-Designs](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Weitere Informationen zur Verwendung der Features des Material Theme in Xamarin.Android-Apps finden Sie unter [Material Theme](~/android/user-interface/material-theme.md).

## <a name="animations"></a>Animationen

Android 5.0 bietet Touchfeedback-Animationen, Animationen für Aktivitätsübergänge und Animationen für Ansichtsstatusübergänge, um die Bedienung von App-Benutzeroberflächen intuitiver zu gestalten. Darüber hinaus können Android 5.0-Apps Animationen mit einem *Enthüllungseffekt* verwenden, um Ansichten ein- oder auszublenden. Mit den Einstellungen für *Bewegungskurven* können Sie konfigurieren, wie schnell oder langsam Animationen wiedergegeben werden.

### <a name="touch-feedback-animations"></a>Touchfeedback-Animationen

Touchfeedback-Animationen bieten Benutzern ein visuelles Feedback, wenn auf eine Ansicht getippt wurde. Beispielsweise zeigen Schaltflächen jetzt einen Welleneffekt, wenn sie berührt werden &ndash; dies ist die standardmäßige Touchfeedback-Animation in Android 5.0. Die Wellenanimation wird durch die neue `RippleDrawable`-Klasse implementiert. Der Welleneffekt kann so konfiguriert werden, dass er an den Rändern der Ansicht endet oder sich darüber hinaus erstreckt. Die folgenden Screenshots veranschaulichen den Welleneffekt für eine Schaltfläche während einer Touchfeedback-Animation:

![Screenshots der einzelnen Frames einer Wellenanimation für eine Schaltfläche](lollipop-images/touch-animation.png)

Der erste Berührungskontakt mit der Schaltfläche erfolgt im ersten Bild links, während die verbleibende Sequenz (von links nach rechts) veranschaulicht, wie sich der Welleneffekt bis zum Rand der Taste ausbreitet. Nach dem Ende der Wellenanimation kehrt die Ansicht zu ihrem ursprünglichen Aussehen zurück. Der standardmäßige Welleneffekt findet im Bruchteil einer Sekunde statt, aber die Dauer der Animation kann verlängert oder verkürzt werden.

Weitere Informationen zu Touchfeedback-Animationen in Android 5.0 finden Sie unter [Customize Touch Feedback](https://developer.android.com/training/material/animations.html#Touch) (Anpassen des Touchfeedbacks).

### <a name="activity-transition-animations"></a>Animationen für Aktivitätsübergänge

Animationen für Aktivitätsübergänge geben dem Benutzer ein Gefühl der visuellen Kontinuität, wenn eine Aktivität in eine andere übergeht. Apps können die folgenden drei Arten von Übergangsanimationen angeben:

- **Übergang bei Eintritt** &ndash; Findet statt, wenn eine Aktivität gestartet wird.

- **Übergang bei Austritt** &ndash; Findet statt, wenn eine Aktivität beendet wird.

- **Übergang für gemeinsames Element** &ndash; Findet statt, wenn eine zwei Aktivitäten gemeinsame Ansicht sich beim Übergang zur nächsten Aktivität ändert.

Die nachstehende Folge von Screenshots veranschaulicht einen Übergang für ein gemeinsames Element:

[![Screenshots der einzelnen Frames einer Übergangsanimation für ein gemeinsames Element](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Ein gemeinsames Element (ein Foto einer Raupe) ist eine von mehreren Ansichten in der ersten Aktivität. Das Foto wird zur einzigen Ansicht vergrößert, wenn die erste Aktivität in die zweite übergeht.

#### <a name="enter-transition-animation-types"></a>Animationen vom Typ „Übergang bei Eintritt“

Für Eintrittsanimationen stellt Android 5.0 drei Arten von Animationen bereit:

- **Explosionsanimation** &ndash; Vergrößert eine Ansicht aus dem Zentrum der Szene.

- **Gleitanimation** &ndash; Bewegt eine Ansicht von einer Seite in eine Szene hinein.

- **Überblendanimation** &ndash; Blendet eine Ansicht in eine Szene ein.

#### <a name="exit-transition-animation-types"></a>Animationen vom Typ „Übergang bei Austritt“

Für Austrittsübergänge stellt Android 5.0 drei Arten von Animationen bereit:

- **Explosionsanimation** &ndash; Verkleinert eine Ansicht in das Zentrum der Szene.

- **Gleitanimation** &ndash; Bewegt eine Ansicht an einer Seite aus einer Szene hinaus.

- **Überblendanimation** &ndash; Blendet eine Ansicht aus einer Szene aus.

#### <a name="shared-element-transition-animation-types"></a>Animationen vom Typ „Übergang für gemeinsames Element“

Übergangsanimationen für ein gemeinsames Element unterstützen verschiedene Arten von Animationen, darunter diese:

- Änderung der Layout- oder Ausschnittbegrenzungen einer Ansicht

- Änderung von Skalierung und Rotation einer Ansicht

- Änderung von Größe und Skalierungstyp für eine Ansicht

Weitere Informationen zu Animationen für Aktivitätsübergänge in Android 5.0 finden Sie unter [Customize Activity Transitions](https://developer.android.com/training/material/animations.html#Transitions) (Anpassen von Aktivitätsübergängen).

### <a name="view-state-transition-animations"></a>Animationen für Ansichtsstatusübergänge

Android 5.0 ermöglicht das Ausführen von Animationen, wenn sich der Status einer Ansicht ändert. Sie können Änderungen des Ansichtsstatus mithilfe einer der folgenden Methoden animieren:

- Erstellen Sie zeichenbare Ressourcen, die einer bestimmten Ansicht zugeordnete Statusänderungen animieren. Die neue `AnimatedStateListDrawable`-Klasse ermöglicht Ihnen das Erstellen von zeichenbaren Ressourcen, die Animationen zwischen Ansichtsstatusänderungen anzeigen.

- Definieren Sie Animationsfunktionalität, die ausgeführt wird, wenn sich der Status einer Ansicht ändert. Mit der neuen `StateListAnimator`-Klasse können Sie einen Animator definieren, der ausgeführt wird, wenn sich der Status einer Ansicht ändert.

Weitere Informationen zu Animationen für Ansichtsstatusübergänge in Android 5.0 finden Sie unter [Animate View State Changes](https://developer.android.com/training/material/animations.html#ViewState) (Animieren von Ansichtsstatusänderungen).

### <a name="reveal-effect"></a>Enthüllungseffekt

Der *Enthüllungseffekt* ist ein kreisförmiger Ausschnitt, dessen Radius sich ändert, um eine Ansicht anzuzeigen oder zu verbergen. Sie können diesen Effekt steuern, indem Sie den Anfangs- und Endradius des kreisförmigen Ausschnitts festlegen. Die nachstehende Folge von Screenshots veranschaulicht ein Animation mit Enthüllungseffekt aus der Mitte des Bildschirms:

[![Screenshots der einzelnen Frames einer Animation mit Enthüllungseffekt](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

Die nächste Sequenz veranschaulicht eine Animation mit Enthüllungseffekt von der linken unteren Ecke des Bildschirms aus:

[![Screenshots der einzelnen Frames einer Animation mit Kreisausschnitt](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Animationen mit Enthüllungseffekt können umgekehrt werden, d. h. der Kreisausschnitt kann sich verkleinern, um die Ansicht zu verbergen, anstatt die Ansicht zur Einblendung zu vergrößern.

Weitere Informationen zum Einblendeffekt in Android 5.0 finden Sie unter [Use the Reveal Effect](https://developer.android.com/training/material/animations.html#Reveal) (Verwenden des Enthüllungseffekts).

### <a name="curved-motion"></a>Bewegungskurven

Zusätzlich zu den beschriebenen Animationsfeatures bietet Android 5.0 neue APIs, mit denen Sie Zeit- und Bewegungskurven für Animationen angeben können. Android 5.0 verwendet diese Kurven, um zeitliche und räumliche Bewegungen während einer Animation zu interpolieren. In Android 5.0 sind drei Kurven definiert:

- **Fast\_out\_linear\_in** &ndash; Schnelle Anfangsbeschleunigung, Fortsetzung der Beschleunigung bis zum Ende der Animation.

- **Fast\_out\_slow\_in** &ndash; Schnelle Anfangsbeschleunigung, Verlangsamung gegen Ende der Animation.

- **Linear\_out\_slow\_in** &ndash; Beginnt mit einem Höchstwert für die Beschleunigung, Verlangsamung gegen Ende der Animation.

Sie können die neue `PathInterpolator`-Klasse verwenden, um die Art der Bewegungsinterpolation anzugeben. `PathInterpolator` ist ein Interpolator, der Animationspfade gemäß angegebener Kontrollpunkte und Bewegungskurven durchläuft. Weitere Informationen zum Angeben von Einstellungen für Bewegungskurven in Android 5.0 finden Sie unter [Use Curved Motion](https://developer.android.com/training/material/animations.html#CurvedMotion) (Verwenden von Bewegungskurven).

## <a name="view-shadows--elevation"></a>Ansichtsschatten und Anzeigeebenen

In Android 5.0 können Sie die *Anzeigeebene* einer Ansicht angeben, indem Sie die neue `Z`-Eigenschaft festlegen. Ein höherer `Z`-Wert bewirkt, dass die Ansicht einen größeren Schatten auf den Hintergrund wirft, sodass die Ansicht höher über dem Hintergrund zu schweben scheint. Sie können die anfängliche Anzeigeebene einer Ansicht festlegen, indem Sie das zugehörige `elevation`-Attribut im Layout konfigurieren.

Das folgende Beispiel veranschaulicht den Schattenwurf durch ein leeres `TextView`-Steuerelement, bei dem das Attribut für die Anzeigeebene respektive auf 2dp, 4dp und 6dp festgelegt ist:

[![Screenshots von progressiv vergrößerten Ansichtsschatten](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Die Einstellungen für Ansichtsschatten können statisch sein (wie oben gezeigt) oder in Animationen verwendet werden, um eine Ansicht so aussehen zu lassen, als würde sie sich vorübergehend über den Hintergrund der Ansicht erheben. Sie können die Anzeigeebene einer Ansicht mithilfe der `ViewPropertyAnimator`-Klasse animieren. Die Anzeigeebene einer Ansicht setzt sich aus der `elevation`-Einstellung ihres Layouts und einer `translationZ`-Eigenschaft zusammen, die Sie über einen `ViewPropertyAnimator`- Methodenaufruf festlegen können.

Weitere Informationen zu Ansichtsschatten in Android 5.0 finden Sie unter [Defining Shadows and Clipping Views](https://developer.android.com/training/material/shadows-clipping.html) (Definieren von Schatten und Ausschnittansichten).

## <a name="color-features"></a>Farbfeatures

Android 5.0 stellt zwei neue Features zur Farbverwaltung in Apps bereit:

- Durch das *Einfärben von zeichenbaren Ressourcen* können Sie die Farben von Bildressourcen ändern, indem Sie ein Layoutattribut ändern.

- Durch das *Extrahieren markanter Farben* können Sie das Farbdesign Ihrer App dynamisch anpassen, um es auf die Farbpalette eines angezeigten Bilds abzustimmen.

### <a name="drawable-tinting"></a>Einfärben von zeichenbaren Ressourcen

Android 5.0-Layouts erkennen ein neues `tint`-Attribut, mit dem Sie die Farbe von zeichenbaren Ressourcen festlegen können, ohne mehrere Versionen dieser Ressourcen zur Anzeige verschiedener Farben erstellen zu müssen. Um dieses Feature zu verwenden, definieren Sie eine Bitmaske als Alphamaske und verwenden das `tint`-Attribut zum Definieren der Farbe der Ressource. Auf diese Weise können Sie Ressourcen einmalig erstellen und sie in Ihrem Layout abgestimmt auf Ihr Design einfärben.

Im folgenden Beispiel wird eine einzelne Bildressource &ndash; ein weißes Logo mit transparentem Hintergrund &ndash; zum Erstellen verschiedener Farbvarianten verwendet:

![Weißes Xamarin-Logo mit transparentem Hintergrund](lollipop-images/xamarin-logo-white.png)

Dieses Logo wird über einem blauen, kreisförmigen Hintergrund angezeigt, wie in den folgenden Beispielen gezeigt. Das Bild links zeigt das Logo ohne `tint`-Einstellung. Im Bild in der Mitte ist das `tint`-Attribut des Logos auf ein dunkles Grau festgelegt. Im Bild rechts ist `tint` auf ein helles Grau festgelegt:

![Beispiele des obigen Logos mit verschiedenen tint-Einstellungen](lollipop-images/drawable-tinting.png)

Weitere Informationen zum Einfärben von zeichenbaren Ressourcen in Android 5.0 finden Sie unter [Drawable Tinting](https://developer.android.com/training/material/drawables.html#DrawableTint) (Einfärben von zeichenbaren Ressourcen).

### <a name="prominent-color-extraction"></a>Extrahieren markanter Farben

Die neue Android 5.0-Klasse `Palette` ermöglicht Ihnen das Extrahieren von Farben aus einem Bild, sodass Sie diese dynamisch auf eine benutzerdefinierte Farbpalette anwenden können. Die `Palette`-Klasse extrahiert sechs Farben aus einem Bild und kennzeichnet diese Farben entsprechend ihrer relativen Farbsättigung und Helligkeit:

- Vibrant

- Vibrant dark

- Vibrant light

- Muted

- Muted dark

- Muted light

In den folgenden Screenshots extrahiert beispielsweise eine App zur Fotoanzeige die markanten Farben aus dem angezeigten Bild und verwendet diese Farben, um das Farbschema der App an das Bild anzupassen:

[![Screenshots der Farbextraktionen für ein grünes, ein pinkfarbenes und ein blaues Design](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

In den oben gezeigten Screenshots ist die Aktionsleiste auf die extrahierte Farbe „vibrant light“ und der Hintergrund auf die extrahierte Farbe „vibrant dark“ festgelegt. In jedem der obigen Beispiele ist eine Reihe kleiner Farbquadrate enthalten, um die aus dem Bild extrahierten Palettenfarben zu veranschaulichen.

Weitere Informationen zur Farbextraktion in Android 5.0 finden Sie unter [Extracting Prominent Colors from an Image](https://developer.android.com/training/material/drawables.html#ColorExtract) (Extrahieren markanter Farben aus einem Bild).

## <a name="new-ui-widgets"></a>Neue Benutzeroberflächenwidgets

In Android 5.0 werden zwei neue Benutzeroberflächenwidgets eingeführt:

- `RecyclerView` &ndash; Eine Ansichtsgruppe, die eine Liste scrollbarer Elemente anzeigt.

- `CardView` &ndash; Ein Basislayout mit abgerundeten Ecken.

Beide Widgets bieten eine integrierte Unterstützung für die Material Theme-Features, beispielsweise verwendet `RecyclerView` Animationen zum Hinzufügen und Entfernen von Ansichten, und `CardView` verwendet Ansichtsschatten, um jede Karte über dem Hintergrund schweben zu lassen. Beispiele dieser neuen Widgets werden in den folgenden Screenshots gezeigt:

[![Screenshots von Apps mit RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Der Screenshot auf der linken Seite ist ein Beispiel für die Verwendung von `RecyclerView` in einer E-Mail-Anwendung, der Screenshot rechts zeigt ein Beispiel für die Verwendung von `CardView` in einer App zum Buchen von Reisen.

### <a name="recyclerview"></a>RecyclerView

`RecyclerView` ähnelt `ListView,`, eignet sich aber besser für umfangreiche Ansichtssätze oder für Listen mit Elementen, die sich dynamisch ändern. Wie bei `ListView,` geben Sie einen Adapter zum Zugriff auf das zugrunde liegende Dataset an. Im Gegensatz zu `ListView,` verwenden Sie jedoch einen *Layout-Manager*, um Elemente innerhalb der `RecyclerView` zu positionieren. Der Layout-Manager kümmert sich auch um das Ansichtsrecycling. Er verwaltet die Wiederverwendung von Elementansichten, die nicht länger für den Benutzer sichtbar sind.

Wenn Sie ein `RecyclerView`-Widget verwenden, müssen Sie einen `LayoutManager` und einen Adapter angeben. Wie in dieser Abbildung gezeigt, ist `LayoutManager` der Vermittler zwischen dem Adapter und der `RecyclerView`:

![Diagramm der RecyclerView mit Unterstützung durch LayoutManager, Adapter und Dataset](lollipop-images/recyclerview-diagram.png)

Die folgenden Screenshots veranschaulichen eine `RecyclerView` mit 100 Elementen (jedes Element setzt sich aus einer `ImageView` und einer `TextView` zusammen):

[![Screenshots einer RecyclerView-App zum Scrollen durch verschiedene Bilder](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` verarbeitet dieses große Dataset problemlos &ndash; das Scrollen vom Anfang bis zum Ende der Liste dauert in dieser Beispiel-App nur wenige Sekunden. `RecyclerView` bietet außerdem Unterstützung für Animationen. Tatsächlich sind Animationen für das Hinzufügen und Entfernen von Elementen standardmäßig aktiviert. Wird einer `RecyclerView` ein Element hinzugefügt, wird es wie in dieser Folge von Screenshots gezeigt eingeblendet:

[![Screenshots der einzelnen Frames der Einblendung eines Fotoelements](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Weitere Informationen zu `RecyclerView` finden Sie unter [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).

### <a name="cardview"></a>CardView

`CardView` ist eine einfache Ansicht, die eine schwebende Karte mit abgerundeten Ecken simuliert. Da `CardView` über integrierte Ansichtsschatten verfügt, können Sie Ihrer App auf einfache Weise visuelle Tiefe verleihen. Die folgenden Screenshots zeigen drei textorientierte Beispiele für `CardView`:

[![Beispielscreenshots von Apps mit Verwendung der RecyclerView und CardView-basierten Elementen](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Jede der Karten im obigen Beispiel enthält eine `TextView`. Die Hintergrundfarbe wird über das `cardBackgroundColor`-Attribut festgelegt.

Weitere Informationen zu `CardView` finden Sie unter [CardView](~/android/user-interface/controls/card-view.md).

## <a name="enhanced-notifications"></a>Verbesserte Benachrichtigungen

Das Benachrichtigungssystem in Android 5.0 wurde mit einem neuen visuellen Format und neuen Funktionen umfangreich aktualisiert. Benachrichtigungen haben in Android 5.0 ein neues Aussehen. Beispielsweise verwenden Benachrichtigungen in Android 5.0 jetzt dunklen Text auf einem hellen Hintergrund:

![Beispiel einer nicht erweiterten Android 5.0-Benachrichtigung](lollipop-images/expanded-notification-contracted.png)

Wenn ein großes Symbol in einer Benachrichtigung angezeigt wird (wie im obigen Beispiel), stellt Android 5.0 das kleine Symbol als Badge über dem großen Symbol dar.

In Android 5.0 können Benachrichtigungen auch auf dem Sperrbildschirm angezeigt werden.
Nachfolgend sehen Sie einen Beispielscreenshot eines Sperrbildschirms mit einer einzigen Benachrichtigung:

[![Screenshot einer auf dem Sperrbildschirm angezeigten Benachrichtigung](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Benutzer können auf eine Benachrichtigung auf dem Sperrbildschirm doppeltippen, um das Gerät zu entsperren und zu der App zu wechseln, von der diese Benachrichtigung stammt. Alternativ kann die Benachrichtigung durch ein Wischen verworfen werden. Benachrichtigungen verfügen über eine neue Einstellung für die *Sichtbarkeit*, die bestimmt, wie viel Inhalt auf dem Sperrbildschirm angezeigt werden kann. Benutzer können auswählen, ob die Anzeige vertraulicher Inhalte in Benachrichtigungen auf dem Sperrbildschirm zugelassen wird.

Android 5.0 führt ein neues Format für die Darstellung von Benachrichtigungen mit hoher Priorität ein, bezeichnet als *Heads-up*-Benachrichtigungen. Heads-up-Benachrichtigungen gleiten vom oberen Bildschirmrand einige Sekunden lang nach unten und dann in die Benachrichtigungsleiste am oberen Bildschirmrand zurück. Mithilfe von Heads-Up-Benachrichtigungen kann die Systembenutzeroberfläche dem Benutzer wichtige Informationen anzeigen, ohne die gerade ausgeführte Aktivität zu unterbrechen.
Das folgende Beispiel veranschaulicht eine einfache Heads-up-Benachrichtigung, die oberhalb einer App angezeigt wird:

[![Beispiel einer Heads-up-Benachrichtigung](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Heads-up-Benachrichtigungen werden typischerweise für die folgenden Ereignisse verwendet:

- Neue Textnachricht

- Eingehender Telefonanruf

- Niedriger Akkustand

- Alarm

Android 5.0 zeigt nur dann eine Benachrichtigung im Heads-up-Format an, wenn eine hohe oder maximale Priorität für die Benachrichtigung festgelegt wurde.

In Android 5.0 können Sie Benachrichtigungsmetadaten bereitstellen, um Android dabei zu unterstützen, Benachrichtigungen intelligenter zu sortieren und anzuzeigen. Android 5.0 organisiert Benachrichtigungen nach Priorität, Sichtbarkeit und Kategorie.
Mithilfe von Benachrichtigungskategorien kann gefiltert werden, welche Benachrichtigungen im *Nicht stören*-Gerätemodus angezeigt werden können.

Ausführliche Informationen zum Erstellen und Auslösen von Benachrichtigungen mit den neuesten Android 5.0-Features finden Sie unter [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

## <a name="new-apis"></a>Neue APIs

Zusätzlich zu den oben beschriebenen neuen Features in Bezug auf das Aussehen und Verhalten stellt Android 5.0 neue APIs bereit, mit denen die vorhandene Multimedia-, Speicher- und Drahtlos-/Konnektivitätsfunktionalität erweitert wird. Darüber hinaus umfasst Android 5.0 neue APIs, die Unterstützung für ein neues Feature zur Auftragsplanung bieten.

### <a name="camera"></a>Camera

Android 5.0 stellt mehrere neue APIs für erweiterte Kamerafunktionen bereit. Der neue `Android.Hardware.Camera2`-Namespace enthält Funktionen für den Zugriff auf einzelne Kamerageräte, die an ein Android-Gerät angeschlossen sind. Außerdem modelliert `Android.Hardware.Camera2` jedes Kameragerät als Pipeline: Es akzeptiert eine Aufnahmeanforderung, nimmt das Bild auf und gibt dann das Ergebnis aus. Dieser Ansatz ermöglicht es Apps, mehrere Aufnahmeanforderungen an ein Kameragerät in eine Warteschlange zu stellen.

Die folgenden APIs ermöglichen diese neuen Features:

- `CameraManager.GetCameraIdList` &ndash; Hilft Ihnen, programmgesteuert auf Kamerageräte zuzugreifen. Sie verwenden `CameraManager.OpenCamera`, um eine Verbindung mit einem bestimmten Kameragerät herzustellen.

- `CameraCaptureSession` &ndash; Nimmt Bilder vom Kameragerät auf oder streamt Bilder von der Kamera. Sie implementieren eine `CameraCaptureSession.CaptureListener`-Schnittstelle zum Verarbeiten neuer Bildaufnahmeereignisse.

- `CaptureRequest` &ndash; Definiert Aufnahmeparameter.

- `CaptureResult` &ndash; Stellt die Ergebnisse eines Bildaufnahmevorgangs bereit.

Weitere Informationen zu den neuen Kamera-APIs in Android 5.0 finden Sie unter [Media](https://developer.android.com/about/versions/android-5.0.html#Media) (Medien).

### <a name="audio-playback"></a>Audiowiedergabe

Android 5.0 aktualisiert die `AudioTrack`-Klasse, um eine bessere Audiowiedergabe zu erzielen:

- `ENCODING_PCM_FLOAT` &ndash; Konfiguriert `AudioTrack` zum Akzeptieren von Audiodaten im Gleitkommaformat für einen besseren Dynamikbereich, einen größeren Toleranzbereich und höhere Qualität (dank erhöhter Genauigkeit). Darüber hinaus trägt das Gleitkommaformat dazu bei, das Abschneiden von Audiodaten zu vermeiden.

- `ByteBuffer` &ndash; Sie können Audiodaten jetzt als Bytearray für `AudioTrack` bereitstellen.

- `WRITE_NON_BLOCKING` &ndash; Durch diese Option werden Pufferung und Multithreading für einige Apps vereinfacht.

Weitere Informationen zu `AudioTrack`-Verbesserungen in Android 5.0 finden Sie unter [Media](https://developer.android.com/about/versions/android-5.0.html#Media) (Medien).

### <a name="media-playback-control"></a>Steuern der Medienwiedergabe

Android 5.0 führt die neue Klasse `Android.Media.MediaController` ein, die `RemoteControlClient` ersetzt. `Android.Media.MediaController` bietet vereinfachte APIs für die Transportsteuerung und eine threadsichere Steuerung der Wiedergabe außerhalb des Benutzeroberflächenkontextes. Die Transportsteuerung wird über die folgenden neuen APIs gesteuert:

- `Android.Media.Session.MediaSession` &ndash; Eine Mediensteuerungssitzung, die mehrere Controller verarbeitet. Sie rufen `MediaSession.GetSessionToken` auf, um ein Token anzufordern, das Ihrer App die Interaktion mit der Sitzung ermöglicht.

- `MediaController.TransportControls` &ndash; Verarbeitet Transportbefehle wie **Wiedergabe**, **Beenden** und **Überspringen**.

Außerdem können Sie mit der neuen Klasse `Android.App.Notification.MediaStyle` einer Mediensitzung umfangreiche Benachrichtigungsinhalte zuordnen (z. B. durch Extrahieren und Anzeigen von Albumcovern).

Weitere Informationen zu den neuen Features für die Medienwiedergabesteuerung in Android 5.0 finden Sie unter [Media](https://developer.android.com/about/versions/android-5.0.html#Media) (Medien).

### <a name="storage"></a>Speicher

Android 5.0 aktualisiert das Storage Access Framework, um Anwendungen die Arbeit mit Verzeichnissen und Dokumenten zu erleichtern:

- Zur Auswahl einer Verzeichnisunterstruktur können Sie eine `Android.Intent.Action.OPEN_DOCUMENT_TREE`-Absicht erstellen und senden. Diese Absicht veranlasst das System, alle Anbieterinstanzen anzuzeigen, die die Auswahl von Unterstrukturen unterstützen. Der Benutzer durchsucht dann diese Unterstruktur und wählt ein Verzeichnis aus.

- Um neue Dokumente oder Verzeichnisse an einer beliebigen Position in einer Unterstruktur zu erstellen und zu verwalten, verwenden Sie die neuen Methoden `CreateDocument`, `RenameDocument` und `DeleteDocument` von `DocumentsContract`.

- Zum Abrufen von Pfaden zu Medienverzeichnissen auf allen freigegebenen Speichergeräten rufen Sie die neue `Android.Content.Context.GetExternalMediaDirs`-Methode auf.

Weitere Informationen zu neuen Speicher-APIs in Android 5.0 finden Sie unter [Storage](https://developer.android.com/preview/api-overview.html#Storage) (Speicher).

### <a name="wireless--connectivity"></a>Drahtlosfunktionen und Konnektivität

Android 5.0 fügt die folgenden API-Verbesserungen für Drahtlosfunktionen und Konnektivität hinzu:

- Neue *Multi-Netzwerk*-APIs, mit denen Apps vor der Verbindungsherstellung Netzwerke mit bestimmten Funktionen ermitteln und auswählen können.

- Bluetooth-Broadcastingfunktionalität, durch die ein Android 5.0-Gerät als Bluetooth-Peripheriegerät mit geringem Stromverbrauch fungieren kann.

- NFC-Verbesserungen, die die Nutzung von Funktionen für die Nahbereichskommunikation zur Freigabe von Daten für andere Geräte erleichtern.

Weitere Informationen zu den neuen Drahtlos- und Konnektivitäts-APIs in Android 5.0 finden Sie unter [Wireless and Connectivity](https://developer.android.com/preview/api-overview.html#Wireless) (Drahtlosfunktionen und Konnektivität).

### <a name="job-scheduling"></a>Auftragsplanung

Android 5.0 führt eine neue `JobScheduler`-API ein, mit denen Benutzer den Akkuverbrauch minimieren können, indem bestimmte Aufgaben so geplant werden, dass sie nur ausgeführt werden, wenn das Gerät angeschlossen ist und aufgeladen wird. Mit diesem Feature für die Auftragsplanung können Aufgaben außerdem so geplant werden, dass sie nur bei günstigen Bedingungen für diese Aufgabe ausgeführt werden. Beispielsweise wird eine große Datei nur dann heruntergeladen, wenn das Gerät mit einem WLAN und nicht mit einem Netzwerk mit nutzungsbezogenen Gebühren verbunden ist.

Weitere Informationen zu den neuen APIs für die Auftragsplanung in Android 5.0 finden Sie unter [Scheduling Jobs](https://developer.android.com/preview/api-overview.html#JobScheduler) (Planen von Aufträgen).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die wichtigen neuen Features in Android 5.0 für Xamarin.Android-App-Entwickler vorgestellt:

- Materialdesign

- Animationen

- Ansichtsschatten und Anzeigeebenen

- Farbfeatures, wie z. B. das Einfärben von zeichenbaren Ressourcen und die Extrahierung markanter Farben

- Die neuen `RecyclerView`- und `CardView`-Widgets

- Verbesserungen an Benachrichtigungen

- Neue APIs für Kamera, Audiowiedergabe, Mediensteuerung, Speicher, Drahtlosfunktionen/Konnektivität und Auftragsplanung

Wenn die Xamarin Android-Entwicklung noch neu für Sie ist, lesen Sie zum einfacheren Einstieg in Xamarin.Android den Artikel [Setup und Installation](~/android/get-started/installation/index.md).
[Hello, Android (Hallo, Android)](~/android/get-started/hello-android/index.md) bietet eine hervorragende Einführung in die Erstellung von Android-Projekten.

## <a name="related-links"></a>Verwandte Links

- [Android L Developer Preview](https://developer.android.com/preview/index.html)
- [Herunterladen des Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Material Design](https://developer.android.com/preview/material/index.html)
