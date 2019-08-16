---
title: Lollipop-Features
description: Dieser Artikel bietet einen Überblick über die neuen Features, die in Android 5,0 (Lollipop) eingeführt wurden. Diese Features enthalten einen neuen Stil für die Benutzeroberfläche, der als Material Design bezeichnet wird, sowie neue unterstützende Funktionen wie Animationen, Anzeige Schatten und drawable-Tinting. Android 5,0 umfasst auch erweiterte Benachrichtigungen, zwei neue UI-Widgets, einen neuen Auftrags Planer und einige neue APIs zur Verbesserung der Speicher-, Netzwerk-, Konnektivitäts-und Multimedia-Funktionen.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7cc7eeabebbfbcd4b264202235307638b88842d8
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524211"
---
# <a name="lollipop-features"></a>Lollipop-Features

_Dieser Artikel bietet einen Überblick über die neuen Features, die in Android 5,0 (Lollipop) eingeführt wurden. Diese Features enthalten einen neuen Stil für die Benutzeroberfläche, der als Material Design bezeichnet wird, sowie neue unterstützende Funktionen wie Animationen, Anzeige Schatten und drawable-Tinting. Android 5,0 umfasst auch erweiterte Benachrichtigungen, zwei neue UI-Widgets, einen neuen Auftrags Planer und einige neue APIs zur Verbesserung der Speicher-, Netzwerk-, Konnektivitäts-und Multimedia-Funktionen._

## <a name="lollipop-overview"></a>Lollipop (Übersicht)

Android 5,0 (Lollipop) führt eine neue Entwurfs Sprache, den *Material Entwurf*und eine unterstützende Umwandlung neuer Features ein, damit apps einfacher und intuitiver zu verwenden sind. Mit dem Material Design bietet Android 5,0 nicht nur ein Facelift für Android-Telefone. Außerdem wird ein neuer Satz von Entwurfs Regeln für Android-basierte Tablets, Desktop Computer, Überwachungen und smarttvs bereitstellt. Diese Entwurfs Regeln betonen Einfachheit und minimalität, während Sie vertraute, nicht verwendete, unrealistische Attribute (z. b. realistische Oberflächen-und Edge-Cues) verwenden, um Benutzern das schnelle und intuitive Verständnis der Schnittstelle

*Material Theme* ist die Verkörperung dieser Entwurfs Prinzipien für die Benutzeroberfläche in Android. In diesem Artikel werden die unterstützenden Features der Material Themen behandelt:

- **Animationen** Touchfeedback-Animationen, Aktivitäts Übergangs Animationen, Ansichts Zustands Übergangs-Animationen und ein offen ungs Effekt. &ndash;

- **Schatten und** Rechte Erweiterung anzeigen Sichten verfügen jetzt über `elevation` eine-Eigenschaft. &ndash;   Sichten mit höheren `elevation` Werten wandeln größere Schatten in den Hintergrund.

- **Farb Features** Mithilfe von *drawable-Farben können* Sie bildassets wieder verwenden, indem Sie Ihre Farbe ändern. mit der hervor *ragenden Farb Extraktion* können Sie Ihre APP auf der Grundlage von Farben in einem Bild dynamisch überprüfen. &ndash;

Viele Material Design Features sind bereits in der Benutzeroberfläche von Android 5,0 integriert, während andere explizit zu apps hinzugefügt werden müssen. Beispielsweise enthalten einige Standard Sichten (z. b. Schaltflächen) bereits touchfeedback-Animationen, während Apps die meisten Sicht Schatten aktivieren müssen.

Zusätzlich zu den Verbesserungen der Benutzeroberfläche, die über das Material Design eingeführt werden, umfasst Android 5,0 auch mehrere weitere neue Features, die in diesem Artikel behandelt werden:

- **Erweiterte Benachrichtigungen** Die Benachrichtigungen in Android 5,0 wurden mit einem neuen Erscheinungsbild, Unterstützung für Sperrbildschirm-Benachrichtigungen und einem neuen Format für die Benachrichtigungs Darstellung von Heads deutlich aktualisiert. &ndash;

- **Neue UI-Widgets** Das neue `RecyclerView` Widget erleichtert es apps, große Datasets und komplexe Informationen zu vermitteln, und das neue `CardView` Widget bietet ein vereinfachtes Karten ähnliches Präsentationsformat zum Anzeigen von Text und Bildern. &ndash;

- **Neue APIs** &ndash; Android 5,0 fügt neue APIs für die Unterstützung mehrerer Netzwerke, eine verbesserte Bluetooth-Konnektivität, eine einfachere Speicherverwaltung und eine flexiblere Steuerung von Multimedia-Playern und Kamera Geräten hinzu. Ein neues Auftrags Planungs Feature ist zum asynchronen Ausführen von Aufgaben zu geplanten Zeiten verfügbar. Mit dieser Funktion können Sie die Akku Lebensdauer verbessern, indem Sie z. b. Aufgaben planen, die durchgeführt werden, wenn das Gerät angeschlossen und berechnet wird.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Android 5,0-Features in xamarin-basierten apps zu verwenden:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden. 

- **Android SDK** &ndash; Android 5,0 (API 21) oder höher muss über den Android SDK-Manager installiert werden.

- **Java Developer Kit** Xamarin. Android erfordert [JDK 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für API-Ebene 24 oder höher entwickeln (JDK 1,8 unterstützt auch API-Ebenen vor 24, einschließlich Lollipop). &ndash; Die 64-Bit-Version von JDK 1,8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder den Formular-Previewer verwenden.

Sie können weiterhin [JDK 1,7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie speziell für API-Ebene 23 oder früher entwickeln.


## <a name="setting-up-an-android-50-project"></a>Einrichten eines Android 5,0-Projekts

Um ein Android 5,0-Projekt zu erstellen, müssen Sie die neuesten Tools und SDK-Pakete installieren. Führen Sie die folgenden Schritte aus, um ein xamarin. Android-Projekt einzurichten, das Android 5,0 als Ziel verwendet:

1. Installieren Sie xamarin. Android-Tools, und aktivieren Sie Ihre xamarin-Lizenz. Weitere Informationen zum Installieren von xamarin. Android finden Sie unter [Setup und Installation](~/android/get-started/installation/index.md) .

2. Wenn Sie Visual Studio für Mac verwenden, installieren Sie die neuesten Android 5,0-Updates.

3. Starten Sie den Android SDK-Manager (verwenden Sie in Visual Studio für Mac **Tools &gt; Open&hellip;Android SDK Manager**), und installieren Sie Android SDK Tools 23.0.5 oder höher:

    [![Auswählen von Android SDK Tools im Android SDK Manager](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   Installieren Sie außerdem die neuesten Android 5,0 SDK-Pakete (API 21 oder höher):

    [![Installieren von Android 5,0 SDK-Paketen im Android SDK-Manager](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Weitere Informationen zur Verwendung des Android SDK-Managers finden Sie unter [SDK-Manager](https://developer.android.com/tools/help/sdk-manager.html).

4. Erstellen Sie ein neues xamarin. Android-Projekt. Wenn Sie mit der Android-Entwicklung mit xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Weitere Informationen zum Erstellen von Android-Projekten. Wenn Sie ein Android-Projekt erstellen, stellen Sie sicher, dass Sie die Versions Einstellungen für Android 5,0 konfigurieren.
   Navigieren Sie in Visual Studio für Mac zu **Projektoptionen &gt; Build &gt; allgemein** , und legen Sie **Ziel Framework** auf **Android 5,0 (Lollipop)** oder höher fest:

    ![Festlegen der Ziel-framwework auf Android 5,0 Lollipop](lollipop-images/target-framework.png)

   Legen Sie unter **Projekt &gt; Optionen &gt; Android-Anwendung erstellen**die Mindestversion und die Android-Zielversion auf **automatische Verwendung der Ziel Framework-Version**fest:

    ![Festlegen der Mindest-und Ziel-Android-Versionen auf "automatisch"](lollipop-images/minimum-android-version.png)

5. Konfigurieren Sie einen Emulator oder ein Android-Gerät, um Ihre APP zu testen. Wenn Sie einen Emulator verwenden, finden Sie unter [Android-Emulator-Setup](~/android/get-started/installation/android-emulator/index.md) Informationen zum Konfigurieren eines Android-Emulators für die Verwendung mit Xamarin Studio oder Visual Studio. Wenn Sie ein Android-Gerät verwenden, finden Sie weitere Informationen unter [Einrichten des Vorschau-SDK](https://developer.android.com/preview/setup-sdk.html) , um zu erfahren, wie Sie Ihr Gerät für Android 5,0 aktualisieren. Informationen zum Konfigurieren Ihres Android-Geräts für das Ausführen und Debuggen von xamarin. Android-Anwendungen finden Sie unter [Einrichten des Geräts für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md).

Hinweis: Wenn Sie ein vorhandenes Android-Projekt aktualisieren, das auf die Android L-Vorschau ausgerichtet war, müssen Sie das **Ziel Framework** und die **Android-Version** auf die oben beschriebenen Werte aktualisieren.

## <a name="important-changes"></a>Wichtige Änderungen

Bereits veröffentlichte Android-Apps können von Änderungen in Android 5,0 betroffen sein. Insbesondere verwendet Android 5,0 eine neue Laufzeit und ein erheblich geändertes Benachrichtigungs Format.

### <a name="android-runtime"></a>Android-Laufzeit

Android 5,0 verwendet die neue Android-Runtime (Art) als Standardlaufzeit anstelle von Dalvik. Kunst implementiert verschiedene wichtige neue Features:

- Vorab **Kompilierung (AOT)** &ndash; AOT kann die APP-Leistung verbessern, indem app-Code kompiliert wird, bevor die APP erstmals gestartet wird. Wenn eine APP installiert wird, generiert Art eine kompilierte ausführbare App für das Zielgerät.

- **Verbessertes Garbage Collection (GC)** &ndash; GC-Verbesserungen in Kunst können auch die Leistung der APP steigern. Bei der Garbage Collection wird jetzt eine GC-Pause anstelle von zwei verwendet, und gleichzeitige GC-Vorgänge werden rechtzeitig durchgeführt.

- **Verbessertes App-Debugging** &ndash; Die Grafik bietet weitere Diagnose Details zum Analysieren von Ausnahmen und Absturzberichten.

Vorhandene apps sollten ohne Änderung in der Art &ndash; funktionieren, außer für apps, die die für die vorherige Dalvik-Laufzeit eindeutigen Verfahren ausnutzen, die möglicherweise nicht in der Kunst funktionieren. Weitere Informationen zu diesen Änderungen finden Sie unter [Überprüfen des App-Verhaltens auf der Android-Laufzeit (Art)](https://developer.android.com/guide/practices/verifying-apps-art.html).


### <a name="notification-changes"></a>Benachrichtigungs Änderungen

Die Benachrichtigungen wurden in Android 5,0 erheblich geändert:

- **Sounds und Vibrationen werden unterschiedlich behandelt** . `Ringtone` `Notification.Builder` `Vibrator` `MediaPlayer`Benachrichtigungs Klänge und-Vibrationen werden nun von anstelle von, und behandelt. &ndash;

- **Neues Farbschema** &ndash; In Übereinstimmung mit dem Material Design werden Benachrichtigungen mit einem dunklen Text über weißem oder sehr hellen Hintergründen gerendert. Außerdem können Alphakanäle in Benachrichtigungs Symbolen von Android geändert werden, damit Sie mit System Farbschemas koordiniert werden. 

- **Lockscreen-Benachrichtigungen** &ndash; Benachrichtigungen können jetzt auf dem Sperrbildschirm des Geräts angezeigt werden.

- **Heads-up** &ndash; Benachrichtigungen mit hoher Priorität werden jetzt in einem kleinen gleitenden Fenster (Heads-up-Benachrichtigung) angezeigt, wenn das Gerät entsperrt und der Bildschirm eingeschaltet ist.

In den meisten Fällen sind zum Portieren vorhandener App-Benachrichtigungsfunktionen auf Android 5,0 die folgenden Schritte erforderlich:

1. Konvertieren Sie Ihren Code, `Notification.Builder` um ( `NotificationsCompat.Builder`oder) zum Erstellen von Benachrichtigungen zu verwenden. 

2. Vergewissern Sie sich, dass Ihre vorhandenen Benachrichtigungs Ressourcen im neuen Material Design-Farbschema angezeigt werden können.

3. Entscheiden Sie, welche Sichtbarkeit Ihre Benachrichtigungen erhalten sollen, wenn Sie auf dem Sperrbildschirm angezeigt werden. Wenn eine Benachrichtigung nicht öffentlich ist, sollte der Inhalt auf dem Sperrbildschirm angezeigt werden?

4. Legen Sie die Kategorie Ihrer Benachrichtigungen fest, damit Sie im neuen Android 5,0-Modus " *nicht stören* " ordnungsgemäß verarbeitet werden.

Wenn Ihre Benachrichtigungen Transport Steuerelemente enthalten, den Status der Medienwiedergabe `RemoteControlClient`anzeigen, verwenden `ActivityManager.GetRecentTasks`oder aufrufen, finden Sie unter [wichtige Verhaltensänderungen](https://developer.android.com/preview/api-overview.html#Behaviors) Weitere Informationen zum Aktualisieren Ihrer Benachrichtigungen für Android 5,0.

Weitere Informationen zum Erstellen von Benachrichtigungen in Android finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

## <a name="material-theme"></a>Materialdesign

Das neue Android 5,0-Material Design führt zu tiefgreifenden Änderungen am Aussehen und Gefühl der Android-Benutzeroberfläche. Visuelle Elemente verwenden jetzt taktier Oberflächen, die die fett formatierten Grafiken, typografiefarben und hellen Farben des Druck basierten Entwurfs verwenden. Beispiele für Material Design werden in den folgenden Screenshots dargestellt:

[![Screenshots von Material Design-Startbildschirm, Apps-Bildschirm und Einstellungsbildschirm](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5,0 empfängt Sie mit dem auf der linken Seite angezeigten Startbildschirm. Der Screenshot "Mittelpunkt" ist der erste Bildschirm der APP-Liste, und der Screenshot auf der rechten Seite ist der Bildschirm " **Einstellungen** ". Die [Material Design](https://material.io/guidelines/material-design/introduction.html) -Spezifikation von Google erläutert die zugrunde liegenden Entwurfs Regeln hinter dem neuen Material Design-Konzept.

Das Material Design enthält drei integrierte Varianten, die Sie in Ihrer APP verwenden können: das `Theme.Material` dunkle Design (Standard), das `Theme.Material.Light` Design und das `Theme.Material.Light.DarkActionBar` Design: 

[![Screenshots von dunklen, hellen und dunklen Aktionsleisten Designs](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Weitere Informationen zur Verwendung von Material Design Features in xamarin. Android-Apps finden Sie unter [Material Theme (Material](~/android/user-interface/material-theme.md)Design).


## <a name="animations"></a>Animationen

Android 5,0 bietet touchfeedback-Animationen, Aktivitäts Übergangs Animationen und Ansichts Zustands Übergangs-Animationen, um die Verwendung von App-Schnittstellen intuitiver zu gestalten. Android 5,0-Apps können auch die Anzeige *Effekt* -Animationen verwenden, um Sichten auszublenden oder anzuzeigen. Mithilfe der *gekrümmten Bewegungs* Einstellungen können Sie konfigurieren, wie schnell oder langsam Animationen gerendert werden.


### <a name="touch-feedback-animations"></a>Touchfeedback-Animationen

Touchfeedback-Animationen bieten Benutzern visuelles Feedback, wenn eine Ansicht berührt wurde. Beispielsweise zeigen Schaltflächen nun einen Ripple-Effekt an, wenn &ndash; Sie berührt werden. Dies ist die standardmäßige touchfeedback-Animation in Android 5,0. Die Ripple-Animation wird von der `RippleDrawable` neuen-Klasse implementiert. Der Ripple-Effekt kann so konfiguriert werden, dass er an den Begrenzungen der Ansicht endet oder die Begrenzungen der Ansicht überschreitet. Beispielsweise zeigt die folgende Sequenz von Screenshots den Ripple-Effekt in einer Schaltfläche während der Touchscreen-Animation:

![Frame-by-Frame-Screenshots der Ripple-Animation auf einer Schaltfläche](lollipop-images/touch-animation.png)

Der erste Berührungs Kontakt mit der Schaltfläche wird im ersten Bild auf der linken Seite angezeigt, während die verbleibende Sequenz (von links nach rechts) veranschaulicht, wie sich der Ripple-Effekt auf den Rand der Schaltfläche ausbreitet. Wenn die Ripple-Animation endet, wird die Ansicht wieder in die ursprüngliche Darstellung zurückgegeben. Die standardmäßige Ripple-Animation findet in einem Bruchteil einer Sekunde statt, aber die Länge der Animation kann länger oder kürzer angepasst werden.

Weitere Informationen zu touchfeedback-Animationen in Android 5,0 finden Sie unter [Anpassen des touchfeedbacks](https://developer.android.com/training/material/animations.html#Touch).


### <a name="activity-transition-animations"></a>Aktivitäts Übergangs Animationen

Aktivitäts Übergangs Animationen vermitteln Benutzern einen Sinn der visuellen Kontinuität, wenn eine Aktivität zu einer anderen übergeht. Apps können drei Typen von Übergangs Animationen angeben:

- **Übergang eintreten** &ndash; Für den Fall, dass eine Aktivität in die Szene gelangt.

- **Übergang beenden** &ndash; Für den Fall, dass eine Aktivität die Szene verlässt.

- **Übergang des freigegebenen Elements** &ndash; Für den Fall, dass sich eine Sicht, die zwei Aktivitäten gemeinsam ist, ändert, während die erste Aktivität zum nächsten übergeht.

Die folgende Sequenz von Screenshots veranschaulicht z. b. den Übergang eines freigegebenen Elements:

[![Bild-auf-Frame-Screenshots einer Animation für das freigegebene Element Übergang](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Ein frei gegebenes Element (Photo of a Caterpillar) ist eine von mehreren Ansichten in der ersten Aktivität. Sie wird so vergrößert, dass Sie zur einzigen Ansicht in der zweiten Aktivität wird, wenn die erste Aktivität in die zweite übergeht.

#### <a name="enter-transition-animation-types"></a>Übergangs Animations Typen eingeben

Für Enter-Übergänge bietet Android 5,0 drei Arten von Animationen:

- **Animation explodieren** &ndash; Vergrößert eine Ansicht aus der Mitte der Szene.

- **Folien Animation** &ndash; Verschiebt eine Ansicht an einem der Ränder einer Szene.

- **Animation ausblenden** &ndash; Blendet eine Ansicht in der Szene aus.

#### <a name="exit-transition-animation-types"></a>Übergangs Animations Typen beenden

Für Exit-Übergänge bietet Android 5,0 drei Arten von Animationen:

- **Animation explodieren** &ndash; Verkleinert eine Ansicht auf den Mittelpunkt der Szene.

- **Folien Animation** &ndash; Verschiebt eine Ansicht an einen der Ränder einer Szene.

- **Animation ausblenden** &ndash; Blendet eine Ansicht aus der Szene aus.

#### <a name="shared-element-transition-animation-types"></a>Animations Typen für das gemeinsame Element Übergang

Freigegebene Element Übergänge unterstützen mehrere Animations Typen, z. b.:

- Ändern der layoutgrenzen einer Ansicht.

- Ändern der Skalierung und Drehung einer Ansicht.

- Ändern der Größe und des Skalierungs Typs für eine Ansicht.

Weitere Informationen zu Aktivitäts Übergangs Animationen in Android 5,0 finden Sie unter [Anpassen von Aktivitäts Übergängen](https://developer.android.com/training/material/animations.html#Transitions).


### <a name="view-state-transition-animations"></a>Anzeigen von Zustands Übergangs Animationen

Android 5,0 ermöglicht das Ausführen von Animationen, wenn sich der Zustand einer Ansicht ändert. Sie können Ansichts Zustandsübergänge animieren, indem Sie eine der folgenden Methoden verwenden:

- Erstellen Sie drawables, die Zustandsänderungen animieren, die einer bestimmten Ansicht zugeordnet sind. Mit der `AnimatedStateListDrawable` neuen-Klasse können Sie drawables erstellen, die Animationen zwischen Ansichts Zustandsänderungen anzeigen.

- Definieren von Animations Funktionen, die ausgeführt werden, wenn sich der Zustand einer Ansicht ändert. Mit der `StateListAnimator` neuen-Klasse können Sie einen Animator definieren, der ausgeführt wird, wenn sich der Zustand einer Ansicht ändert.

Weitere Informationen über Ansichts Zustandsübergänge-Animationen in Android 5,0 finden Sie unter [Animieren von Ansichts Zustandsänderungen](https://developer.android.com/training/material/animations.html#ViewState).


### <a name="reveal-effect"></a>Effekt aufdecken

Der Anzeige *Effekt* ist ein clippingkreis, mit dem der RADIUS geändert wird, um eine Ansicht anzuzeigen oder auszublenden. Sie können diesen Effekt steuern, indem Sie den anfänglichen und den letzten Radius des clippingkreises festlegen. Die folgende Sequenz von Screenshots veranschaulicht eine Darstellung des Effekts im Mittelpunkt des Bildschirms:

[![Bild-auf-Frame-Screenshots von "Animation anzeigen"](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

Die nächste Sequenz veranschaulicht eine Darstellung der Ergebnis Animation, die in der unteren linken Ecke des Bildschirms stattfindet:

[![Bild-auf-Frame-Screenshots der Clipping-Animation](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Das Anzeigen von Animationen kann rückgängig gemacht werden. Das heißt, der clippingkreis kann verkleinert werden, um die Ansicht auszublenden, anstatt Sie zu vergrößern, um die Ansicht anzuzeigen.

Weitere Informationen zu den Auswirkungen von Android 5,0 in finden Sie unter [Verwenden des auszeige Effekts](https://developer.android.com/training/material/animations.html#Reveal).


### <a name="curved-motion"></a>Gekrümmte Bewegung

Zusätzlich zu diesen Animations Features bietet Android 5,0 auch neue APIs, mit denen Sie die Zeit-und Bewegungs Kurven von Animationen angeben können. Android 5,0 verwendet diese Kurven, um temporale und räumliche Verschiebungen während Animationen zu interpolieren. In Android 5,0 sind drei Kurven definiert:

- **SchnelleshochskaliereninbeschleunigtschnellundbeschleunigtbiszumEndederAnimation.\_\_\_** &ndash;

- **Schnelles\_durch\_laufeninbeschleunigtschnellundlangsamverlangsamtsichamEndederAnimation.\_** &ndash;

- **Die\_langsamelineare\_Ausgabe in beginnt mit einer Spitzengeschwindigkeit und verlangsamt langsam bis zum Ende der Animation.\_** &ndash;

Sie können die neue `PathInterpolator` Klasse verwenden, um anzugeben, wie die Bewegungs interpolung stattfindet. `PathInterpolator`ist ein interpolators, der Animations Pfade gemäß den angegebenen Kontrollpunkten und Bewegungs Kurven durchläuft. Weitere Informationen zum Angeben von gekrümmten Bewegungs Einstellungen in Android 5,0 finden Sie unter [Verwenden von gekrümmter Bewegung](https://developer.android.com/training/material/animations.html#CurvedMotion).


## <a name="view-shadows--elevation"></a>Schatten & Erhöhung anzeigen

In Android 5,0 können Sie die *Höhe* einer Ansicht angeben, indem Sie eine neue `Z` Eigenschaft festlegen. Ein größerer `Z` Wert bewirkt, dass die Ansicht einen größeren Schatten in den Hintergrund wandelt, sodass die Ansicht auf eine höhere Position über dem Hintergrund zeigt. Sie können die anfängliche Erhöhung einer Ansicht festlegen, indem Sie das `elevation` zugehörige-Attribut im Layout konfigurieren.

Das folgende Beispiel veranschaulicht die durch ein leeres `TextView` -Steuerelement Umwandlungs Schatten Umwandlungen, wenn das Attribut für die Rechte Erweiterung auf 2DP, 4DP bzw. 6DP festgelegt ist:

[![Screenshots von progessively größeren Anzeige Schatten](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Anzeige Schatten Einstellungen können statisch sein (wie oben gezeigt), oder Sie können in Animationen verwendet werden, damit eine Ansicht vorübergehend oberhalb des Hintergrunds der Ansicht angezeigt wird. Sie können die `ViewPropertyAnimator` -Klasse verwenden, um die Höhe einer Ansicht zu animieren. Die Höhe einer Ansicht ist die Summe der Layouteinstellungen `elevation` zuzüglich einer `translationZ` Eigenschaft, die Sie mithilfe eines `ViewPropertyAnimator` Methoden Aufrufes festlegen können.

Weitere Informationen zu Anzeige Schatten in Android 5,0 finden Sie unter [Definieren von Schatten-und clippingansichten](https://developer.android.com/training/material/shadows-clipping.html).


## <a name="color-features"></a>Farb Features

Android 5,0 bietet zwei neue Features zum Verwalten von Farben in apps:

- Mithilfe von *drawable-Farben* können Sie die Farben von Bild Objekten durch Ändern eines Layoutattributs ändern.

- Mit der hervor *ragenden Farb Extraktion* können Sie das Farbschema Ihrer APP dynamisch anpassen, sodass Sie mit der Farbpalette eines angezeigten Bilds koordiniert wird.


### <a name="drawable-tinting"></a>Drawable-Tinting

Android 5,0-Layouts erkennen ein `tint` neues Attribut, das Sie verwenden können, um die Farbe der drawables festzulegen, ohne mehrere Versionen dieser Assets erstellen zu müssen, um unterschiedliche Farben anzuzeigen. Um dieses Feature zu verwenden, definieren Sie eine Bitmap als Alpha Maske und verwenden das `tint` -Attribut, um die Farbe des Assets zu definieren. Dies ermöglicht es Ihnen, Assets einmalig zu erstellen und Sie im Layout zu färben, damit Sie dem Design entsprechen.

Im folgenden Beispiel wird ein einzelnes Bildmedien Objekt &ndash; ein weißes Logo mit transparentem Hintergrund &ndash; verwendet, um Tönungs-Variationen zu erstellen:

![Weißes xamarin-Logo mit transparentem Hintergrund](lollipop-images/xamarin-logo-white.png)

Dieses Logo wird über einem blauen Zirkel Hintergrund angezeigt, wie in den folgenden Beispielen gezeigt. Das Bild auf der linken Seite zeigt, wie das Logo ohne `tint` eine Einstellung angezeigt wird. Im mittleren Bild wird das- `tint` Attribut des Logos auf einen dunkelgrauen Wert festgelegt. Im Bild auf der rechten Seite `tint` ist hellgrau festgelegt:

![Beispiele für das obige Logo mit unterschiedlichen Tönungs-Einstellungen](lollipop-images/drawable-tinting.png)

Weitere Informationen zu drawable Farben in Android 5,0 finden Sie unter [drawable Farben](https://developer.android.com/training/material/drawables.html#DrawableTint).


### <a name="prominent-color-extraction"></a>Bedeutende Farb Extraktion

Die neue Android 5,0 `Palette` -Klasse ermöglicht das Extrahieren von Farben aus einem Bild, sodass Sie Sie dynamisch auf eine benutzerdefinierte Farbpalette anwenden können. Die `Palette` -Klasse extrahiert sechs Farben aus einem Bild und bezeichnet diese Farben entsprechend ihren relativen Ebenen von Farbsättigung und Helligkeit:

- Reisende

- Dynamisches dunkel

- Dynamisches Licht

- Gedeckten

- Mutiert dunkel

- Gedämpftes Licht

In den folgenden Screenshots extrahiert z. b. eine APP zum Anzeigen von Fotos die markanten Farben aus dem Bild auf der Anzeige und verwendet diese Farben, um das Farbschema der APP an das Bild anzupassen:

[![Screenshots von grünen, Rosa und blauen Design Farb Extraktionen](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

In den obigen Screenshots wird die Aktionsleiste auf die extrahierte Farbe "lebendig hell" festgelegt, und der Hintergrund wird auf die extrahierte Farbe "dynamisch dunkel" festgelegt. In den obigen Beispielen ist eine Zeile mit kleinen Farbquadraten enthalten, um die Palettenfarben zu veranschaulichen, die aus dem Bild extrahiert wurden.

Weitere Informationen zur Farb Extraktion in Android 5,0 finden Sie unter [Extrahieren von markanten Farben aus einem Bild](https://developer.android.com/training/material/drawables.html#ColorExtract).


## <a name="new-ui-widgets"></a>Neue UI-Widgets

In Android 5,0 werden zwei neue UI-Widgets eingeführt:

- `RecyclerView`&ndash; Eine Ansichts Gruppe, die eine Liste der scrollbaren Elemente anzeigt.

- `CardView`&ndash; Ein grundlegendes Layout mit abgerundeten Ecken.

Beide Widgets enthalten die Unterstützung für Material Design Features. beispielsweise `RecyclerView` verwendet Animationen zum Hinzufügen und Entfernen von Sichten und `CardView` verwendet Sicht Schatten, damit jede Karte über dem Hintergrund schwebt. Beispiele für diese neuen Widgets sind in den folgenden Screenshots dargestellt:

[![Screenshots von mit "recyclerview" erstellten apps](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Der Screenshot auf der linken Seite ist ein Beispiel `RecyclerView` für die Verwendung in einer e-Mail-APP, und der Screenshot auf der rechten `CardView` Seite ist ein Beispiel für die Verwendung in einer Reise Reservierungs-app.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView`ähnelt, `ListView,` eignet sich jedoch besser für große Gruppen von Sichten oder Listen mit Elementen, die sich dynamisch ändern. Ebenso `ListView,` wie Sie einen Adapter für den Zugriff auf das zugrunde liegende Dataset angeben. Im Gegensatz `ListView,` dazu verwenden Sie jedoch einen *Layout-Manager* , um `RecyclerView`Elemente innerhalb von zu positionieren. Der LayoutManager kümmert sich auch um die Wiederverwendung der Ansicht. die Wiederverwendung von Element Sichten, die für den Benutzer nicht mehr sichtbar sind, wird verwaltet.

Wenn Sie ein `RecyclerView` Widget verwenden, müssen Sie einen `LayoutManager` und einen Adapter angeben. Wie in dieser Abbildung dargestellt, `LayoutManager` ist der Vermittler zwischen dem Adapter und dem `RecyclerView`:

![Diagramm von "recyclerview" mit unterstützender Layoutmanager, Adapter und DataSet](lollipop-images/recyclerview-diagram.png)

Die folgenden Screenshots veranschaulichen einen `RecyclerView` , der 100 Elemente enthält (jedes Element besteht aus `ImageView` einem und `TextView`einem):

[![Screenshots einer recyclerview-APP, die durch Bilder Scrollen](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView`behandelt dieses große DataSet mit &ndash; einfacheres Scrollen vom Anfang der Liste bis zum Ende der Liste in dieser Beispiel-APP dauert nur ein paar Sekunden. `RecyclerView`unterstützt auch Animationen; Tatsächlich sind Animationen zum Hinzufügen und Entfernen von Elementen standardmäßig aktiviert. Wenn ein Element zu einem `RecyclerView`hinzugefügt wird, wird es wie in dieser Sequenz von Screenshots angezeigt:

[![Screenshot "Frame nach Frame" eines Foto Elements, das ausgeblendet wird](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Weitere Informationen zu `RecyclerView`finden Sie unter [recyclerview](~/android/user-interface/layouts/recycler-view/index.md).


### <a name="cardview"></a>CardView

`CardView`ist eine einfache Ansicht, die eine Gleit Komma Zahl mit abgerundeten Ecken simuliert. Da `CardView` über integrierte Sicht Schatten verfügt, bietet es eine einfache Möglichkeit, der APP visuelle Tiefe hinzuzufügen. Die folgenden Screenshots zeigen drei textorientierte Beispiele für `CardView`:

[![Beispiel-Screenshots von apps mit "recyclerview" und CardView-basierten Elementen](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Jede der Karten im obigen Beispiel enthält eine `TextView`; die Hintergrundfarbe wird über das `cardBackgroundColor` -Attribut festgelegt.

Weitere Informationen zu `CardView`finden Sie unter [CardView](~/android/user-interface/controls/card-view.md).


## <a name="enhanced-notifications"></a>Erweiterte Benachrichtigungen

Das Benachrichtigungssystem in Android 5,0 wurde mit einem neuen visuellen Format und neuen Features erheblich aktualisiert. Benachrichtigungen haben einen neuen Einblick in Android 5,0. Beispielsweise verwenden Benachrichtigungen in Android 5,0 jetzt den dunklen Text über einen hellen Hintergrund:

![Beispiel für eine nicht erweiterte Android 5,0-Benachrichtigung](lollipop-images/expanded-notification-contracted.png)

Wenn in einer Benachrichtigung ein großes Symbol angezeigt wird (wie im obigen Beispiel gezeigt), zeigt Android 5,0 das kleine Symbol als einen Badge über dem großen Symbol an. 

In Android 5,0 können Benachrichtigungen auch auf dem Sperrbildschirm des Geräts angezeigt werden.
Hier sehen Sie beispielsweise einen Screenshot eines Sperr Bildschirms mit einer einzelnen Benachrichtigung:

[![Screenshot der Benachrichtigung, die auf dem Sperrbildschirm angezeigt wird](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Benutzer können auf dem Sperrbildschirm auf eine Benachrichtigung Doppel tippen, um das Gerät zu entsperren und zu der APP zu springen, die diese Benachrichtigung ausgelöst hat, oder die Benachrichtigung zu verwerfen. Benachrichtigungen verfügen über eine neue *Sichtbarkeits* Einstellung, die festlegt, wie viel Inhalt auf dem Sperrbildschirm angezeigt werden kann. Benutzer können auswählen, ob sensible Inhalte in Sperrbildschirm Benachrichtigungen angezeigt werden sollen.

Android 5,0 führt ein neues Format für die Benachrichtigungs Präsentation mit hoher Priorität als *Heads-up*ein. Heads-up-Benachrichtigungen werden für einige Sekunden vom oberen Bildschirmrand nach unten und dann wieder in den Benachrichtigungs Schatten oben auf dem Bildschirm zurückgezogen. Heads-up-Benachrichtigungen ermöglichen es der Benutzeroberfläche des Systems, wichtige Informationen vor dem Benutzer zu platzieren, ohne die aktuell laufende Aktivität zu stören. Das folgende Beispiel veranschaulicht eine einfache Heads-up-Benachrichtigung, die oberhalb einer App angezeigt wird:

[![Beispiel für eine Heads-up-Benachrichtigung](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Heads-up-Benachrichtigungen werden normalerweise für die folgenden Ereignisse verwendet:

- Eine neue nächste Nachricht

- Eingehender Telefonanruf

- Anzeige niedriger Akkukapazität

- Ein Alarm

Android 5,0 zeigt eine Benachrichtigung nur im Heads-up-Format an, wenn eine hohe oder maximale Prioritäts Einstellung vorliegt.

In Android 5,0 können Sie Benachrichtigungs Metadaten bereitstellen, um die intelligente Sortierung und Anzeige von Benachrichtigungen zu unterstützen. Android 5,0 organisiert Benachrichtigungen gemäß Priorität, Sichtbarkeit und Kategorie.
Benachrichtigungs Kategorien werden verwendet, um zu filtern, welche Benachrichtigungen angezeigt werden können, wenn sich das Gerät im Modus " *nicht stören* " befindet.

Ausführliche Informationen zum Erstellen und starten von Benachrichtigungen mit den neuesten Android 5,0-Features finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).


## <a name="new-apis"></a>Neue APIs

Zusätzlich zu den oben beschriebenen neuen Funktionen für das Aussehen und fühlen werden von Android 5,0 neue APIs hinzugefügt, mit denen die Funktionen vorhandener Multimedia-, Speicher-und drahtlos-und Konnektivitätsfunktionen erweitert werden. Außerdem enthält Android 5,0 neue APIs, die Unterstützung für ein neues Auftrags Planer-Feature bieten.

### <a name="camera"></a>Camera

Android 5,0 bietet mehrere neue APIs für erweiterte Kamerafunktionen. Der neue `Android.Hardware.Camera2` Namespace umfasst Funktionen für den Zugriff auf einzelne Kamerageräte, die mit einem Android-Gerät verbunden sind. `Android.Hardware.Camera2` Außerdem modelliert jedes Kamera Gerät als Pipeline: es akzeptiert eine Aufzeichnungs Anforderung, erfasst das Abbild und gibt dann das Ergebnis aus. Diese Vorgehensweise ermöglicht es apps, mehrere Aufzeichnungs Anforderungen in eine Warteschlange zu stellen.

Die folgenden APIs ermöglichen die folgenden neuen Features:

- `CameraManager.GetCameraIdList`Unterstützt Sie beim programmgesteuerten Zugriff auf Kamerageräte. `CameraManager.OpenCamera` verwenden Sie, um eine Verbindung mit einem bestimmten Kamera Gerät herzustellen. &ndash;

- `CameraCaptureSession`&ndash; Hiermit werden Bilder vom Kamera Gerät erfasst oder gestreamt. Sie implementieren eine `CameraCaptureSession.CaptureListener` -Schnittstelle, um neue Abbild Aufzeichnungs Ereignisse zu verarbeiten.

- `CaptureRequest`&ndash; Definiert Aufzeichnungsparameter.

- `CaptureResult`&ndash; Stellt die Ergebnisse eines Bild Erfassungs Vorgangs bereit.

Weitere Informationen zu den neuen Kamera-APIs in Android 5,0 finden Sie unter [Medien](https://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Audiowiedergabe

Android 5,0 aktualisiert die `AudioTrack` -Klasse für eine bessere Audiowiedergabe:

- `ENCODING_PCM_FLOAT`&ndash; Konfiguriert,dassAudiodatenimGleitKommaFormatfüreinenbesserendynamischenBereich,größerenSpielraumundhöhereQualität(Dankerhöhter`AudioTrack` Genauigkeit) akzeptiert werden. Außerdem hilft das Gleit Komma Format dabei, das AudioClipping zu vermeiden.

- `ByteBuffer`Sie können jetzt Audiodaten als Bytearray an `AudioTrack` bereitstellen. &ndash;

- `WRITE_NON_BLOCKING`&ndash; Diese Option vereinfacht die Pufferung und das Multithreading für einige apps.

Weitere Informationen zu `AudioTrack` Verbesserungen in Android 5,0 finden Sie unter [Medien](https://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Steuerelement zur Medienwiedergabe

In Android 5,0 wird die `Android.Media.MediaController` neue-Klasse eingeführt `RemoteControlClient`, die ersetzt. `Android.Media.MediaController`bietet vereinfachte Transport Steuerungs-APIs und bietet eine Thread sichere Kontrolle der Wiedergabe außerhalb des UI-Kontexts. Die folgenden neuen APIs behandeln die Transportsteuerung:

- `Android.Media.Session.MediaSession`&ndash; Eine Medien Steuerungs Sitzung, die mehrere Controller verarbeitet. Sie fordern `MediaSession.GetSessionToken` an, ein Token anzufordern, das Ihre APP verwendet, um mit der Sitzung zu interagieren.

- `MediaController.TransportControls`Verarbeitet transportbefehle wie z. b. wieder **Gabe**, **Beendigung**und **Skip.** &ndash;

Außerdem können Sie die neue `Android.App.Notification.MediaStyle` -Klasse verwenden, um eine Medien Sitzung mit umfangreichem Benachrichtigungs Inhalt (z. b. extrahieren und zeigen von albumgrafiken) zuzuordnen.

Weitere Informationen zu den neuen Funktionen für die Medienwiedergabe Steuerung in Android 5,0 finden Sie unter [Medien](https://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Speicher

Android 5,0 aktualisiert das Speicherzugriffs Framework, um Anwendungen das Arbeiten mit Verzeichnissen und Dokumenten zu vereinfachen:

- Zum Auswählen einer Verzeichnis Unterstruktur können Sie eine `Android.Intent.Action.OPEN_DOCUMENT_TREE` Absicht erstellen und senden. Diese Absicht bewirkt, dass das System alle Anbieter Instanzen anzeigt, die die Unterstruktur Auswahl unterstützen. der Benutzer durchsucht dann ein Verzeichnis und wählt es aus.

- Zum Erstellen und Verwalten neuer Dokumente oder Verzeichnisse an beliebiger Stelle in einer Unterstruktur verwenden Sie `CreateDocument`die `RenameDocument`neuen Methoden `DeleteDocument` , und `DocumentsContract`von.

- Zum Abrufen von Pfaden zu Medien Verzeichnissen auf allen freigegebenen Speichergeräten wird die neue `Android.Content.Context.GetExternalMediaDirs` -Methode aufgerufen.

Weitere Informationen zu neuen Storage-APIs in Android 5,0 finden Sie unter [Storage](https://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Drahtlose & Konnektivität

Android 5,0 fügt die folgenden API-Erweiterungen für Drahtlos Verbindungen und Konnektivität hinzu:

- Neue *Multi-Network-* APIs, mit denen apps Netzwerke mit bestimmten Funktionen suchen und auswählen können, bevor Sie eine Verbindung herstellen.

- Bluetooth-Übertragungsfunktionen, die einem Android 5,0-Gerät ermöglichen, als Low-Energy-Bluetooth-Peripherie zu fungieren.

- NFC-Erweiterungen, die die Verwendung von Near-Field-Kommunikationsfunktionen für die gemeinsame Nutzung von Daten für andere Geräte vereinfachen.

Weitere Informationen zu den neuen drahtlos-und konnektivitätsapis in Android 5,0 finden Sie unter [drahtlos und Konnektivität](https://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Auftragsplanung

In Android 5,0 wird eine `JobScheduler` neue API eingeführt, mit der Benutzer die Akku Ableitung minimieren können, indem bestimmte Aufgaben nur dann ausgeführt werden, wenn das Gerät angeschlossen ist und Gebühren berechnet werden. Diese Funktion für die Auftragsplanung kann auch zum Planen einer Aufgabe verwendet werden, die ausgeführt wird, wenn die Bedingungen für diese Aufgabe besser geeignet sind, z. b. das Herunterladen einer großen Datei, wenn das Gerät über ein WLAN anstelle eines gemessenen Netzwerks verbunden ist.

Weitere Informationen zu den neuen Auftrags Planungs-APIs in Android 5,0 finden Sie unter [Planen von Aufträgen](https://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel bietet einen Überblick über wichtige neue Features in Android 5,0 für xamarin. Android-App-Entwickler:

- Materialdesign

- Animationen

- Schatten und Rechte Erweiterung anzeigen

- Farb Features, z. b. drawable Farben und markante Farb Extraktion

- Die neuen `RecyclerView` - `CardView` und-Widgets

- Benachrichtigungs Erweiterungen

- Neue APIs für Kamera, Audiowiedergabe, Mediensteuerung, Speicher, drahtlos/Konnektivität und Auftragsplanung

Wenn Sie mit der xamarin Android-Entwicklung noch nicht vertraut sind, lesen Sie [Setup und Installation](~/android/get-started/installation/index.md) , um Ihnen den Einstieg in xamarin. Android zu erleichtern.
[Hello, Android](~/android/get-started/hello-android/index.md) ist eine ausgezeichnete Einführung, um zu erfahren, wie Sie Android-Projekte erstellen.



## <a name="related-links"></a>Verwandte Links

- [Android L Developer Preview](https://developer.android.com/preview/index.html)
- [Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Material Design](https://developer.android.com/preview/material/index.html)
- [Grundlagen des Material Entwurfs](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
