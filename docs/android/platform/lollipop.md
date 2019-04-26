---
title: Lollipop-Funktionen
description: Dieser Artikel enthält eine allgemeine Übersicht über die neuen Features in Android 5.0 (Lollipop). Zu diesen Features gehören einen neue Benutzer-schnittstellenstil mit der Bezeichnung Material Design als auch neue unterstützende Funktionen wie z. B. Animationen, Ansicht Schatten und drawable Farben. Android 5.0 umfasst zudem verbesserte Benachrichtigungen, zwei neue UI-Widgets, einen neuen Auftragsplaner und eine Reihe neuer APIs zur Verbesserung der Speicher-, Netzwerk-, Konnektivität und Multimediafunktionen.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d6173e1886eaf807decd960b07acc022bb17c04d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61177668"
---
# <a name="lollipop-features"></a>Lollipop-Funktionen

_Dieser Artikel enthält eine allgemeine Übersicht über die neuen Features in Android 5.0 (Lollipop). Zu diesen Features gehören einen neue Benutzer-schnittstellenstil mit der Bezeichnung Material Design als auch neue unterstützende Funktionen wie z. B. Animationen, Ansicht Schatten und drawable Farben. Android 5.0 umfasst zudem verbesserte Benachrichtigungen, zwei neue UI-Widgets, einen neuen Auftragsplaner und eine Reihe neuer APIs zur Verbesserung der Speicher-, Netzwerk-, Konnektivität und Multimediafunktionen._

## <a name="lollipop-overview"></a>Lollipop-Übersicht

Android 5.0 (Lollipop) führt eine neue entwurfssprache *Material Design*, und damit ein unterstützender Typumwandlung eines neuen Features, die apps einfacher und intuitiver verwenden. Bei Material Design bietet Android 5.0 Android-Smartphones nicht nur einen überarbeiteten; Darüber hinaus einen neuen Satz von Regeln für Android-basierte Tablets, desktop-PCs, Überwachungen und smart-TVs. Diese Regeln hervorzuheben, Einfachheit und Minimalism bei der Erstellung vertraut sind, praktisch Attribute (z. B. Hinweise für realistische Oberfläche und Rand) verwenden, damit Benutzer schnell und intuitiv die Benutzeroberfläche zu verstehen.

*Materialdesign* die verkörperung diese Designrichtlinien für Benutzeroberflächen in Android ist. Dieser Artikel beginnt mit dem Material des Designs unterstützen Features behandelt:

-   **Animationen** &ndash; *Touch Feedback* Animationen *Aktivitätsübergang* Animationen *Statusübergang anzeigen* Animationen und einer *Auswirkung anzeigen*.

-   **Schatten und Erhöhung der Rechte anzeigen** &ndash; Ansichten verfügen nun über eine `elevation` Eigenschaft; Ansichten mit höheren `elevation` Werte umgewandelt, größere Zeichnen von Schatten im Hintergrund.

-   **Farbe, die Funktionen** &ndash; *Drawable Farben* ermöglicht es Ihnen Bildanlagen wiederverwenden, indem Sie die Farbe, ändern und *auffällige Farbe Extraktion* können Sie dynamisch Design Ihrer app anhand Farben eines Bilds an.

Viele Material-Design, das Funktionen bereits integriert sind, in der Benutzeroberfläche für Android 5.0 auftreten, während andere Apps explizit hinzugefügt werden müssen. Beispielsweise in einigen Standardansichten (z. B. Schaltflächen) bereits Touch Feedback Animationen enthalten, während apps aktivieren, die meisten Ansicht Schatten können.

Zusätzlich zu den Verbesserungen der Benutzeroberfläche eingehen, die über Material Design umfasst Android 5.0 auch mehrere andere neuen Funktionen, die behandelt werden in diesem Artikel:

-   **Verbesserte Benachrichtigungen** &ndash; Benachrichtigungen in Android 5.0 wurden erheblich aktualisiert mit der ein neues Erscheinungsbild, Unterstützung für den sperrbildschirmhintergrund Benachrichtigungen und eine neue *Heads-Up* Presentation-benachrichtigungsformat.

-   **Neue UI-Widgets** &ndash; neuen `RecyclerView` Widget erleichtert das Apps auf große Datasets und komplexe Informationen und die neue vermitteln `CardView` Widget bietet einen vereinfachten Darstellung der Karte-ähnlichen Format zum Anzeigen von Text und Bilder.

-   **Neue APIs** &ndash; Android 5.0 fügt neue APIs für die Netzwerkunterstützung mehrerer, Bluetooth-Konnektivität, einfachere Speicherverwaltung und flexiblere Steuerung der multimedia-Playern und Kamerageräte verbessert. Ein neues Feature zur auftragsplanung steht zum Ausführen von Aufgaben asynchron mit geplanten Zeiten. Mit dieser Funktion können zur Verbesserung der Akkuverbrauch gering zu halten, z. B. Planen von Aufgaben vorhanden, wenn das Gerät im Netzbetrieb befindet und Gebühren.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neue Android 5.0-Features in Xamarin-basierte apps verwenden:

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-   **Android SDK** &ndash; Android 5.0 (API 21) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin.Android erfordert [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für API-Ebene 24 entwickeln oder größer (JDK 1.8 unterstützt auch API-Ebenen älter als 24, einschließlich der Lollipop). Die 64-Bit-Version des JDK 1.8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder die Forms-Vorschau verwenden.

Sie können weiterhin verwenden [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Wenn Sie die Entwicklung von speziell für die API-Ebene 23 oder früher sind.


## <a name="setting-up-an-android-50-project"></a>Das Einrichten eines Android 5.0-Projekts

Um ein Android 5.0-Projekt zu erstellen, müssen Sie die neuesten Tools und SDK-Pakete installieren. Verwenden Sie die folgenden Schritte aus, um ein Xamarin.Android-Projekt, das als Ziel Android 5.0 einzurichten:

1. Installieren von Xamarin.Android-Tools, und aktivieren Sie Ihre Xamarin-Lizenz. Finden Sie unter [Setup und Installation](~/android/get-started/installation/index.md) für Weitere Informationen zum Installieren von Xamarin.Android.

2. Wenn Sie Visual Studio für Mac verwenden, installieren Sie die neuesten Updates für Android 5.0.

3. Starten Sie den Android SDK Manager (verwenden Sie in Visual Studio für Mac **Tools &gt; Open Android SDK Manager&hellip;**) herunter, und Installieren von Android SDK Tools 23.0.5 oder höher:

    [![Android SDK-Tools im Android SDK-Manager auswählen.](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   Außerdem installieren Sie die neuesten Android 5.0-SDK-Pakete (API 21 oder höher):

    [![Installieren von Android 5.0-SDK-Pakete im Android SDK-Manager](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Weitere Informationen zur Verwendung von Android SDK-Manager finden Sie unter [SDK-Manager](https://developer.android.com/tools/help/sdk-manager.html).

4. Erstellen eines neuen Xamarin.Android-Projekts an. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/index.md) um weitere Informationen zum Erstellen von Android-Projekte. Wenn Sie ein Android-Projekt erstellen, achten Sie darauf, dass Sie so konfigurieren Sie die versionseinstellungen für Android 5.0.
   Wechseln Sie in Visual Studio für Mac zu **Projektoptionen &gt; erstellen &gt; allgemeine** und **Zielframework** zu **Android 5.0 (Lollipop)** oder weiter unten:

    ![Die Ziel-Framwework festlegen auf Android 5.0 Lollipop](lollipop-images/target-framework.png)

   Klicken Sie unter **Projektoptionen &gt; erstellen &gt; Android-Anwendung**, legen Sie Mindest- und Android-Zielversion, **automatisch – Zielframeworkversion verwenden**:

    ![Die minimale und die Target Android Version festlegen auf automatisch](lollipop-images/minimum-android-version.png)

5. Konfigurieren Sie einen Emulator oder einem Android-Gerät zum Testen Ihrer app. Wenn Sie einen Emulator verwenden, finden Sie unter [Setup von Android-Emulator](~/android/get-started/installation/android-emulator/index.md) erfahren, wie einen Android-Emulator für die Verwendung mit Xamarin Studio oder Visual Studio konfigurieren. Wenn Sie ein Android-Gerät verwenden, finden Sie unter [Einstellung sich die Preview SDK](https://developer.android.com/preview/setup-sdk.html) erfahren, wie Sie Ihr Gerät für Android 5.0 aktualisieren. Zum Konfigurieren Ihres Android-Geräts für das Ausführen und Debuggen von Xamarin.Android-Anwendungen finden Sie unter [Set Up Device Geräts für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md).

Hinweis: Wenn Sie ein vorhandenes Android-Projekt, die die Android-L-Vorschau abzielte aktualisieren, müssen Sie aktualisieren die **Zielframework** und **Android-Version** auf die Werte, die oben beschriebenen.

## <a name="important-changes"></a>Wichtige Änderungen

Zuvor konnte veröffentlichten Android-apps durch Änderungen in Android 5.0 beeinflusst werden. Insbesondere wird Android 5.0 verwendet, eine neue Laufzeit und -benachrichtigungsformat für einen erheblich geändert.

### <a name="android-runtime"></a>Android-Laufzeit

Android 5.0 verwendet die neue Android Common Language Runtime (ART) als standardruntime statt Dalvik. ART implementiert einige der wichtigsten neuen Features:

-   **Ahead-of-Time (AOT) Kompilierung** &ndash; AOT kann app-Leistung verbessern, indem Sie die app-Code kompilieren, bevor die app zum ersten Mal gestartet wird. Wenn eine app installiert ist, generiert ART eine kompilierte ausführbare Datei für das Zielgerät-app.

-   **Verbesserter Garbagecollection (GC)** &ndash; GC-Verbesserungen bei der Kunst können auch app-Leistung verbessern. Jetzt die automatische speicherbereinigung verwendet eine GC anhalten anstelle von zwei und gleichzeitige GC-Vorgänge, die in mehr rechtzeitig abgeschlossen.

-   **Verbessertes Debuggen der app** &ndash; ART bietet mehr Diagnosedetails in helfen bei der Analyse von Ausnahmen und Absturzberichte.

Vorhandene apps sollten funktionieren, ohne Änderungen unter ART &ndash; mit Ausnahme von apps, die exploit-Verfahren nur für die frühere Dalvik-Laufzeitumgebung, die funktionieren nicht unter ART. Weitere Informationen zu diesen Änderungen finden Sie unter [überprüft das Verhalten der App auf Android Common Language Runtime (ART)](https://developer.android.com/guide/practices/verifying-apps-art.html).


### <a name="notification-changes"></a>Benachrichtigung ändert

Benachrichtigungen wurden in Android 5.0 erheblich geändert:

-   **Tönen und Vibration werden anders verarbeitet** &ndash; akustische und Vibrationen erfolgt jetzt mit `Notification.Builder` anstelle von `Ringtone`, `MediaPlayer`, und `Vibrator`.

-   **Neues Farbschema** &ndash; gemäß Material Design Benachrichtigungen mit dunkel Text über weiß oder geringe Hintergründen gerendert werden. Darüber hinaus können alpha-Kanäle in Benachrichtigungssymbole von Android zur Koordinierung mit dem System Farbschemas geändert werden. 

-   **Für den Sperrbildschirmhintergrund Benachrichtigungen** &ndash; Benachrichtigungen können jetzt auf dem Gerät für den sperrbildschirmhintergrund angezeigt werden.

-   **Heads-Up** &ndash; jetzt Benachrichtigungen mit hoher Priorität angezeigt, in einem kleinen unverankerte Fenster (Heads-Up-Benachrichtigung) Wenn das Gerät entsperrt ist und der Bildschirm eingeschaltet ist.

In den meisten Fällen erfordert Portieren vorhandenen app-Benachrichtigung-Funktionalität auf Android 5.0 die folgenden Schritte aus:

1.  Konvertieren Sie Ihren Code, um `Notification.Builder` (oder `NotificationsCompat.Builder`) zum Erstellen von Benachrichtigungen. 

2.  Stellen Sie sicher, dass Ihre vorhandenen Assets für die Benachrichtigung in der neuen Material Design-Farbskala angezeigt werden.

3.  Entscheiden Sie, welche Sichtbarkeit Ihrer Benachrichtigungen haben sollen, wenn sie auf die für den sperrbildschirmhintergrund dargestellt werden. Wenn eine Benachrichtigung nicht öffentlich ist, sollte auf die für den sperrbildschirmhintergrund Inhalte angezeigt?

4.  Legen Sie die Kategorie der Benachrichtigungen aus, damit sie ordnungsgemäß in die neue Android 5.0 verarbeitet werden *nicht stören* Modus.

Wenn Sie Ihre Benachrichtigungen Transport-Steuerelemente, Anzeige Media Playback-Status vorhanden ist, verwenden Sie `RemoteControlClient`, oder rufen Sie `ActivityManager.GetRecentTasks`, finden Sie unter [wichtigen Verhaltensänderungen](https://developer.android.com/preview/api-overview.html#Behaviors) für Weitere Informationen zum Aktualisieren Ihrer Benachrichtigungen für Android 5.0.

Weitere Informationen zum Erstellen von Benachrichtigungen unter Android, finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md). Die [Kompatibilität](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) Abschnitt dieses Artikels wird erläutert, wie Benachrichtigungen erstellen, die nach unten-kompatiblen mit früheren Versionen von Android.


## <a name="material-theme"></a>Materialdesign

Das neue Android 5.0 Material Design bringt umwälzende Änderungen, um das Aussehen und Verhalten der Android-Benutzeroberfläche. Visuelle Elemente verwenden nun praktisch Oberflächen, die auf den fett formatierten Grafiken, Typografie und hellen Farben druckbasierten Entwurfs. Beispiele für Material Design sind in den folgenden Screenshots dargestellt:

[![Screenshots von Material Design-Startseite, Bildschirm "Apps" und Einstellung-Bildschirm](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 begrüßt Sie mit der Startseite, die auf der linken Seite angezeigt. Der Center-Screenshot ist der erste Bildschirm der app-Liste, und den Screenshot, der auf der rechten Seite ist die **Einstellungen** Bildschirm. Google [Material Design](https://material.io/guidelines/material-design/introduction.html) Spezifikation erläutert die Regeln für zugrunde liegende hinter dem neuen Material Design-Konzept.

Material Design enthält drei integrierte Typen, die Sie in Ihrer app verwenden können: die `Theme.Material` Design "dunkel" (Standard), die `Theme.Material.Light` Design und die `Theme.Material.Light.DarkActionBar` Design: 

[![Screenshots der dunkle, helle und DarkActionBar Designs](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Weitere Informationen zur Verwendung von Material Design-Features in Xamarin.Android-apps, finden Sie unter [Material Design](~/android/user-interface/material-theme.md).


## <a name="animations"></a>Animationen

Android 5.0 bietet Touch-Feedback-Animationen, Aktivität Übergangsanimationen und Ansicht Zustand Übergangsanimationen, app-Schnittstellen mit intuitiver zu gestalten. Darüber hinaus können apps für Android 5.0 *Auswirkung anzeigen* Animationen auf Sichten anzeigen oder ausblenden. Sie können *gekrümmt, während der Übertragung* Einstellungen konfigurieren wie schnell oder langsam Animationen werden gerendert.


### <a name="touch-feedback-animations"></a>Tippen Sie auf Feedback-Animationen

Touch-Feedback-Animationen bieten Benutzern visuelles Feedback, wenn eine Ansicht aufgerufen wurden. Z. B. jetzt angezeigt werden sollen einen Welleneffekt, wenn sie betroffen sind &ndash; Dies ist die Standard-Touch Feedback-Animation in Android 5.0. Ripple-Animation wird implementiert, durch die neue `RippleDrawable` Klasse. Der sich allmählich ausbreitenden Wirkung können für das Ende um die Grenzen der Ansicht oder zu erweitern, die außerhalb des zulässigen Bereichs der Ansicht konfiguriert werden. Die folgende Sequenz von Screenshots veranschaulicht beispielsweise die allmählich ausbreitenden Wirkung in einer Schaltfläche, während der Touch-Animation:

![Frame Frame Screenshots der Ripple-Animation auf eine Schaltfläche](lollipop-images/touch-animation.png)

Anfängliche Touch wenden Sie sich an, mit der Schaltfläche tritt auf, in der ersten Abbildung auf der linken Seite, während die restlichen Sequenz (von links nach rechts) wird veranschaulicht, wie die sich allmählich ausbreitenden Wirkung am Rand der Schaltfläche ausbreitet. Wenn die Ripple-Animation beendet wird, gibt die Sicht auf die ursprüngliche Darstellung zurück. Der Ripple-Standardanimation findet in einem Bruchteil einer Sekunde, aber die Länge der Animation kann für längere oder kürzere Länge der Zeit angepasst werden.

Weitere Informationen zur Feedback-Animationen in Android 5.0 touch, finden Sie unter [Touch Feedback anpassen](https://developer.android.com/training/material/animations.html#Touch).


### <a name="activity-transition-animations"></a>Aktivität Übergangsanimationen

Übergangsanimationen Aktivität ermöglichen Benutzern einen Eindruck davon, visuelle Kontinuität, wenn eine Aktivität in einen anderen wechselt. Apps können auf drei Arten von Übergangsanimationen angeben:

-   **Geben Sie ein Übergang** &ndash; für wechselt eine Aktivität auf die Szene.

-   **Beenden der Übergang** &ndash; für, wenn die Szene in eine Aktivität beendet wird.

-   **Freigegebene Element Übergang** &ndash; für, wenn eine Sicht, die zwei Aktivitäten gemein sind als die erste Aktivität Übergänge zur nächsten geändert wird.

Die folgende Sequenz von Screenshots veranschaulicht z. B. einen Übergang des freigegebenen Elements:

[![Frame Frame Screenshots des freigegebenen Elements übergangsanimation](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Ein freigegebenes-Element (ein Foto von einem Caterpillar) ist eine der mehrere Ansichten in der ersten Aktivität; Es wird vergrößert, um die einzige Ansicht in der zweiten Aktivität als die erste Aktivität Übergänge für die zweite zu werden.

#### <a name="enter-transition-animation-types"></a>Geben Sie ein Übergang-Animationstypen

Geben Sie Übergänge enthält Android 5.0 drei Arten von Animationen Folgendes:

-   **Explode Animation** &ndash; vergrößert eine Ansicht in der Mitte der Szene.

-   **Schieben Sie die Animation** &ndash; verschiebt eine Ansicht in der über einen der Ränder einer Szene.

-   **Animation eingeblendet** &ndash; wird eine Ansicht in der Szene.

#### <a name="exit-transition-animation-types"></a>Exit-Übergang-Animationstypen

Exit-Übergänge enthält Android 5.0 drei Arten von Animationen Folgendes:

-   **Explode Animation** &ndash; verkleinert eine Ansicht in der Mitte des der Szene.

-   **Schieben Sie die Animation** &ndash; verschiebt eine Ansicht um einen der Ränder einer Szene.

-   **Animation eingeblendet** &ndash; wird von einer Sicht anhand der Szene.

#### <a name="shared-element-transition-animation-types"></a>Freigegebene Element Übergang-Animationstypen

Freigegebene Element. Übergänge unterstützen mehrere Typen von Animationen, z. B.:

-   Ändern die Layout- oder Clip Grenzen einer Ansicht.

-   Ändern die Skalierung und Drehung einer Ansicht.

-   Ändern des Größe und Skalierung Typs für eine Ansicht an.

Weitere Informationen zu Aktivitäten Übergangsanimationen in Android 5.0, finden Sie unter [anpassen Aktivität Übergänge](https://developer.android.com/training/material/animations.html#Transitions).


### <a name="view-state-transition-animations"></a>Ansichtszustand Übergangsanimationen

Android 5.0 ermöglicht es Animationen ausgeführt werden, wenn der Zustand einer Sicht ändert. Sie können die Statusübergänge anzeigen animieren, mit einer der folgenden Methoden:

-   Erstellen Sie zeichenbarer Ressourcen, die Zustandsänderungen im Zusammenhang mit einer bestimmten Ansicht animieren. Die neue `AnimatedStateListDrawable` Klasse können Sie die zeichenbarer Ressourcen erstellen, die Animationen zwischen Änderungen des Ansichtszustands anzeigen.

-   Definieren Sie die Animationsfunktionen, die ausgeführt wird, wenn der Zustand einer Sicht ändert. Die neue `StateListAnimator` Klasse können Sie einen Animator zu definieren, die ausgeführt wird, wenn der Zustand einer Sicht ändert.

Weitere Informationen zu Ansicht-Status-Übergang-Animationen in Android 5.0, finden Sie unter [Animieren von Änderungen des Ansichtszustands](https://developer.android.com/training/material/animations.html#ViewState).


### <a name="reveal-effect"></a>Anzeigen der Auswirkungen

Die *Auswirkung anzeigen* ist ein Clipping-Kreis, Änderungen Radius auf Sie ein- oder Ausblenden einer Ansicht. Sie können diesen Effekt steuern, durch Festlegen des anfänglichen und abschließenden Radius des Kreises Clipping. Die folgende Sequenz von Screenshots veranschaulicht eine Animation für den Effekt anzeigen aus der Mitte des Bildschirms:

[![Frame Screenshots der Frame-Animation anzeigen](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

Die nächste Sequenz zeigt eine Animation der anzeigen-Auswirkungen, die von der unteren linken Ecke des Bildschirms stattfindet:

[![Frame Frame Screenshots Clipping-Animation](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Einblenden Sie, dass Animationen umgekehrt werden können; d. h. kann der Clipping-Kreis verkleinert werden, wenn die Ansicht anstatt vergrößern, um die Ansicht anzuzeigen.

Weitere Informationen zu den Auswirkungen des Android 5.0-anzeigen in finden Sie unter [verwenden Sie die Auswirkungen anzuzeigen](https://developer.android.com/training/material/animations.html#Reveal).


### <a name="curved-motion"></a>Gekrümmte während der Übertragung

Neben diesen Animationsfeatures hinaus Android 5.0 neue APIs, die Ihnen die Angabe die Uhrzeit und während der Übertragung Kurven von Animationen ermöglichen. Android 5.0 verwendet diese Kurven temporale und räumliche Bewegung während Animationen zu interpolieren. Drei Kurven mit sind in Android 5.0 definiert:

-   **Schnelle\_out\_lineare\_in** &ndash; schnell beschleunigt wird und bis zum Ende der Animation zu beschleunigen.

-   **Schnelle\_out\_langsam\_in** &ndash; Accelerates schnell und langsam gegen Ende der Animation verlangsamt wird.

-   **Lineare\_out\_langsam\_in** &ndash; beginnt mit der eine maximale Geschwindigkeit und langsam an das Ende der Animation verlangsamt wird.

Sie können die neue `PathInterpolator` Klasse, um anzugeben, wie die Interpolation der während der Übertragung stattfindet. `PathInterpolator` ist ein Interpolators, das die Animationspfade gemäß den angegebenen Kontrollpunkten und während der Übertragung Kurven durchläuft. Weitere Informationen zum Angeben von gekrümmten Motion-Einstellungen in Android 5.0, finden Sie unter [gekrümmte Motion verwenden](https://developer.android.com/training/material/animations.html#CurvedMotion).


## <a name="view-shadows--elevation"></a>Ansicht Schatten & Erhöhung der Rechte

In Android 5.0, können Sie angeben der *Erhöhung der Rechte* einer Ansicht durch Festlegen eines neuen `Z` Eigenschaft. Eine größere `Z` Wert bewirkt, dass die Ansicht, um eine größere Schatten im Hintergrund, sodass die Ansicht "float" über dem Hintergrund höher angezeigt werden. Sie können die anfängliche Höhe einer Ansicht festlegen, indem Sie die Konfiguration der `elevation` -Attribut in das Layout.

Das folgende Beispiel veranschaulicht die Umwandlung ein leeres Schatten `TextView` steuern, wenn das Attribut für die Erhöhung der Rechte bzw. 2dp 4dp und 6dp, festgelegt ist:

[![Screenshots der Progessively größere Ansicht Schatten](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Volumeschattenkopie-Anzeigeeinstellungen können statisch (siehe oben) oder sie können zu einer Ansicht angezeigt werden, vorübergehend über der Ansicht Hintergrund steigen in Animationen verwendet werden. Sie können die `ViewPropertyAnimator` Klasse, um die Höhe einer Ansicht zu animieren. Die Höhe einer Sicht ist die Summe der Layouts `elevation` Einstellung zuzüglich einer `translationZ` -Eigenschaft, die Sie durch festlegen können eine `ViewPropertyAnimator` Methodenaufruf.

Weitere Informationen dazu anzeigen Shadows in Android 5.0, finden Sie unter [Schatten definieren und Clipping Ansichten](https://developer.android.com/training/material/shadows-clipping.html).


## <a name="color-features"></a>Color-Funktionen

Android 5.0 bietet zwei neue Features für die Verwaltung von Farbe in apps:

-   *Drawable Farben* können Sie die Farben der Bildanlagen verändern, indem ein Layoutattribut.

-   *Auffällige Farbe Extraktion* ermöglicht es Ihrer app Farbschema zur Koordinierung mit die Farbpalette der ein angezeigtes Bild dynamisch anpassen.


### <a name="drawable-tinting"></a>Drawable-Farben

Android 5.0 Layouts erkennt ein neues `tint` -Attribut, das Sie verwenden können, um die Farbe des zeichenbarer Ressourcen festgelegt werden, ohne dass mehrere Versionen dieser Ressourcen auf verschiedenen Farben erstellen. Um dieses Feature verwenden zu können, definieren Sie eine Bitmap, ein alpha maskieren und die Verwendung der `tint` Attribut, um die Farbe des Medienobjekts zu definieren. Dadurch wird es möglich, dass Sie Objekte einmal erstellen und die Farbe, die sie in Ihrem Layout Ihrem Design übereinstimmt.

Im folgenden Beispiel ein einzelnes Abbild Medienobjekt &ndash; ein weißes Logo mit transparentem Hintergrund &ndash; wird verwendet, um den Farbton Variationen erstellen:

![Xamarin-Logo mit transparentem Hintergrund weiß](lollipop-images/xamarin-logo-white.png)

Dieses Logo wird über einen blauen zirkuläre Hintergrund angezeigt, wie in den folgenden Beispielen gezeigt. Das Bild auf der linken Seite ist wie das Logo ohne angezeigt wird eine `tint` festlegen. In der Mitte Abbildung ist das Logo des `tint` -Attribut auf eine dunkelgrau festgelegt ist. In der Abbildung auf der rechten Seite `tint` auf Hellgrau festgelegt ist:

![Beispiele für die oben genannten Logo mit verschiedenen Tint-Einstellungen](lollipop-images/drawable-tinting.png)

Weitere Informationen dazu drawable Farben in Android 5.0, finden Sie unter [Drawable Farben](https://developer.android.com/training/material/drawables.html#DrawableTint).


### <a name="prominent-color-extraction"></a>Auffällige Farbe extrahieren

Die neue Android 5.0 `Palette` Klasse können Sie die Farben aus einem Image zu extrahieren, damit Sie sie auf eine benutzerdefinierte Farbpalette dynamisch anwenden können. Die `Palette` Klasse extrahiert sechs Farben aus einem Bild, und diese Farben entsprechend ihrer relativen konvergenzebenen Farbe Sättigung und Helligkeit Bezeichnungen:

-   Vibrant

-   Sehr aktives dunkel

-   Sehr aktives Licht

-   Stumm geschaltet

-   Stumm geschalteter dunkel

-   Stumm geschalteter Licht

Beispielsweise wird in den folgenden Screenshots ein ein Foto, das Anzeigen von app extrahiert die deutliche Farben aus dem Image auf und verwendet diese Farben, um das Farbschema der app das Bild entsprechend anpassen:

[![Screenshots der grüne, sondern rosa und die blaue Design Farbe Extraktionen](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

In den obigen Screenshots die Aktionsleiste festgelegt ist, auf das extrahierte "sehr aktives Licht" Farbe und Hintergrund festgelegt ist, die extrahierte "lebendige dunkel" Farbe. In jedem Beispiel oben wird eine Zeile mit kleinen Farbe Quadrate eingeschlossen, um die Palettenfarben zu veranschaulichen, die aus dem Abbild extrahiert wurden.

Weitere Informationen zum Extrahieren von Farbe in Android 5.0, finden Sie [deutliche Farben aus einem Image extrahieren](https://developer.android.com/training/material/drawables.html#ColorExtract).


## <a name="new-ui-widgets"></a>Neue UI-Widgets

Android 5.0 führt zwei neue UI-Widgets:

-   `RecyclerView` &ndash; Eine Ansichtsgruppe, die eine Liste der bildlauffähigen Elemente anzeigt.

-   `CardView` &ndash; Eine grundlegende Layout mit abgerundeten Ecken.

Beide Widgets sind integrierte Unterstützung für Material Design-Features: z. B. `RecyclerView` verwendet Sie zum Hinzufügen und Entfernen von Ansichten, Animationen und `CardView` verwendet anzeigen, Schatten, um jede Karte angezeigt werden, die über dem Hintergrund "float" zu machen. Beispiele für diese neue Widgets sind in den folgenden Screenshots dargestellt:

[![Screenshots der apps, die mit RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Im Screenshot auf der linken Seite ist ein Beispiel für `RecyclerView` verwendeten in eine e-Mail-app und im Screenshot auf der rechten Seite ist ein Beispiel des `CardView` in einer Reservierung Reise-app verwendet.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` ist vergleichbar mit `ListView,` , aber es ist besser, sich ideal für große Mengen von Ansichten oder Listen mit Elementen, die sich dynamisch ändern. Wie `ListView,` Geben Sie einen Adapter für den Zugriff auf das zugrunde liegende DataSet. Anders als bei `ListView,` Sie verwenden eine *Layout-Manager* zum Positionieren der Datenelemente `RecyclerView`. Der Layout-Manager übernimmt außerdem Ansicht wiederverwenden; Er verwaltet die Wiederverwendung von Element-Sichten, die nicht mehr für den Benutzer sichtbar sind.

Bei Verwendung einer `RecyclerView` -Widget erstellen, müssen Sie angeben einer `LayoutManager` und einen Adapter. In dieser Abbildung gezeigten `LayoutManager` der Vermittler zwischen dem Adapter und die `RecyclerView`:

![Diagramm der RecyclerView mit unterstützenden LayoutManager, Adapter und Dataset](lollipop-images/recyclerview-diagram.png)

Die folgenden Screenshots zeigen eine `RecyclerView` , 100 Elemente enthält (jedes Element besteht aus einer `ImageView` und `TextView`):

[![Screenshots der Images durchblättern RecyclerView-app](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` behandelt diese großen Datasets mit einfachen &ndash; Durchführen eines Bildlaufs vom Anfang der Liste der Enden der Liste der in diesem Beispiel-app dauert nur wenige Sekunden. `RecyclerView` unterstützt auch Animationen; Animationen für das Hinzufügen und Entfernen von Elementen sind in der Tat standardmäßig aktiviert. Wenn ein Element hinzugefügt wird, um eine `RecyclerView`, es ausgeblendet wird, als in dieser Sequenz von Screenshots gezeigt:

[![Frame Frame-Screenshot, der ein Foto Element Ausblenden von Fenstern in](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Weitere Informationen zu `RecyclerView`, finden Sie unter [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


### <a name="cardview"></a>CardView

`CardView` ist eine einfache Ansicht, die eine Gleitkommazahl Karte mit abgerundeten Ecken simuliert. Da `CardView` verfügt über integrierte Ansicht Schatten, es bietet eine einfache Möglichkeit für Sie optische Tiefe zu Ihrer app hinzufügen. Die folgenden Screenshots zeigen die drei Beispiele, die für die textorientierten `CardView`:

[![Beispielscreenshots von apps mithilfe von RecyclerView CardView-basierenden Elemente](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Jede der Karten im obigen Beispiel enthält eine `TextView`; die Hintergrundfarbe festgelegt ist, über die `cardBackgroundColor` Attribut.

Weitere Informationen zu `CardView`, finden Sie unter [CardView](~/android/user-interface/controls/card-view.md).


## <a name="enhanced-notifications"></a>Verbesserte Benachrichtigungen

Das Benachrichtigungssystem in Android 5.0 wurde mit einem neuen visuellen Format und die neuen Features erheblich aktualisiert. Benachrichtigungen über ein neues Aussehen in Android 5.0 verfügen. Benachrichtigungen in Android 5.0 verwenden z. B. jetzt dunkel Text über einen hellen Hintergrund:

![Beispiel für eine nicht erweiterte Android 5.0-Benachrichtigung](lollipop-images/expanded-notification-contracted.png)

Wenn ein großes Symbol in einer Benachrichtigung (wie im obigen Beispiel gezeigt) angezeigt wird, wird Android 5.0 des kleinen Symbols als Signal über großes Symbol angezeigt. 

In Android 5.0 können Benachrichtigungen auch auf dem Gerät für den sperrbildschirmhintergrund angezeigt werden.
Hier ist z. B. eine beispielscreenshot, der eine für den sperrbildschirmhintergrund eine einmalige Benachrichtigung:

[![Screenshot der Benachrichtigung auf dem Sperrbildschirm angezeigt werden](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Benutzer können Doppeltippen Sie auf eine Benachrichtigung auf die für den sperrbildschirmhintergrund zum Entsperren des Geräts, und fahren Sie mit der app, die diese Benachrichtigung stammt oder Wischen, um diese zu schließen. Benachrichtigungen können nun ein neues *Sichtbarkeit* festlegen, die bestimmt, wie viel Inhalt auf den für den sperrbildschirmhintergrund angezeigt werden kann. Benutzer können auswählen, ob vertraulichen Inhalt für den sperrbildschirmhintergrund Benachrichtigungen angezeigt werden können.

Android 5.0 führt ein neue Benachrichtigung mit hoher Priorität Präsentationsformat namens *Heads-Up*. Heads-Up-Benachrichtigungen schieben Sie ein paar Sekunden nach unten aus dem oberen Rand des Bildschirms, und klicken Sie dann zurück an die Schattierung der Benachrichtigungen am oberen Rand des Bildschirms zurückzukehren. Heads-Up-Benachrichtigungen können für das System UI wichtige Informationen vor dem Benutzer ohne Unterbrechung die momentan ausgeführte Aktivität einfügen. Das folgende Beispiel veranschaulicht eine einfache Heads-Up-Benachrichtigung, die zusätzlich zu einer app anzeigt:

[![Beispiel für eine heads-up-Benachrichtigung](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Heads-Up-Benachrichtigungen werden in der Regel für die folgenden Ereignisse verwendet:

-   Eine neue Nachricht für die nächsten

-   Einen eingehenden Telefonanruf

-   Angabe von niedriger Akkukapazität

-   Einen alarm

Android 5.0 zeigt eine Benachrichtigung im Heads-Up-Format, nur, wenn es sich um hoch oder die maximale Priorität hat.

In Android 5.0 können Sie angeben, der Metadaten von abfragebenachrichtigungen Android sortieren und zu Benachrichtigungen intelligenter anzeigen können. Android 5.0 werden Benachrichtigungen entsprechend der Priorität, Sichtbarkeit und Kategorie organisiert.
Benachrichtigungskategorien werden verwendet, zum Filtern der Benachrichtigungen über angezeigt werden, können Wenn das Gerät in *nicht stören* Modus.

Ausführliche Informationen zum Erstellen und starten die Benachrichtigungen mit den neuesten Android 5.0-Features finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).


## <a name="new-apis"></a>Neue APIs

Zusätzlich zu den neuen Aussehen und Verhalten-Features, die oben beschriebenen wird fügt Android 5.0 neue APIs, die die Funktionen des vorhandenen Multimedia, Speicher und WLAN-Verbindung-Funktionalität zu erweitern. Android 5.0 enthält ebenfalls neue APIs, die für eine neue Job Scheduler-Funktion Unterstützung bieten.

### <a name="camera"></a>Camera

Android 5.0 bietet mehrere neue APIs für die erweiterten Kamera-Funktionen. Die neue `Android.Hardware.Camera2` Namespace enthält Funktionen, für den Zugriff auf einzelne Kamerageräte, die mit einem Android-Gerät verbunden. Darüber hinaus `Android.Hardware.Camera2` jedes Kameragerät als Pipeline-Modellen: akzeptiert eine erfassungsanforderung, Erfassung des Abbilds und gibt dann das Ergebnis. Dieser Ansatz ermöglicht es apps für mehrere Anforderungen von Capture auf einem Kameragerät in die Warteschlange.

Die folgenden APIs können diese neuen Features:

-   `CameraManager.GetCameraIdList` &ndash; Hilft Ihnen dabei, Kamera-Geräten programmgesteuert zugreifen; Verwenden Sie `CameraManager.OpenCamera` für die Verbindung mit einem Gerät für die jeweilige Kamera.

-   `CameraCaptureSession` &ndash; Erfasst oder Bilder vom Kameragerät. Sie implementieren eine `CameraCaptureSession.CaptureListener` Schnittstelle, um das neue Image-Capture-Ereignisse zu behandeln.

-   `CaptureRequest` &ndash; Definiert die Capture-Parameter.

-   `CaptureResult` &ndash; Enthält die Ergebnisse eines Image-Capture-Vorgangs.

Weitere Informationen über die neue Kamera-APIs in Android 5.0, finden Sie [Media](https://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Audiowiedergabe

Android 5.0 Updates der `AudioTrack` -Klasse für eine bessere Audiowiedergabe:

-   `ENCODING_PCM_FLOAT` &ndash; Konfiguriert die `AudioTrack` Audiodaten im Gleitkommaformat für eine bessere dynamischen Bereichs, mehr Spielraum und höherer Qualität (durch erhöhte Genauigkeit) zu akzeptieren. Darüber hinaus hilft Ihnen Gleitkommaformat, um audio-Clipping zu vermeiden.

-   `ByteBuffer` &ndash; Sie können jetzt angeben, Audiodaten, `AudioTrack` als Bytearray.

-   `WRITE_NON_BLOCKING` &ndash; Diese Option vereinfacht die Pufferung und bei einigen apps multithreading.

Weitere Informationen zu `AudioTrack` Verbesserungen in Android 5.0 [Media](https://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Steuerelement zum Wiedergeben von Medien

Android 5.0 führt die neue `Android.Media.MediaController` Klasse ersetzt `RemoteControlClient`. `Android.Media.MediaController` stellt vereinfachte Transport Steuerelement APIs bereit, und bietet eine Thread-sichere Kontrolle der Wiedergabe nicht im UI-Kontext. Die folgenden neuen APIs verarbeiten, die Transport-Steuerelement:

-   `Android.Media.Session.MediaSession` &ndash; Eine Sitzung der Media-Steuerelement, die mehrere Controller behandelt. Rufen Sie `MediaSession.GetSessionToken` zum Anfordern eines Tokens, die Ihre app verwendet, um die Interaktion mit der Sitzung.

-   `MediaController.TransportControls` &ndash; Handles-Befehlen wie z.B. transport **spielen**, **beenden**, und **überspringen**.

Darüber hinaus können Sie die neue `Android.App.Notification.MediaStyle` -Klasse eine Media-Sitzung mit umfassenden Benachrichtigungsinhalt (z. B. extrahieren und Anzeigen von Albumcover) verbinden.

Weitere Informationen zu den neuen Media Playback-Steuerelement-Funktionen in Android 5.0, finden Sie unter [Media](https://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Speicher

Android 5.0 aktualisiert das Storage Access Framework für Anwendungen, um das Arbeiten mit Verzeichnissen und Dokumenten vereinfachen:

-   Um einer Verzeichnisunterstruktur ausgewählt haben, können Sie erstellen und senden eine `Android.Intent.Action.OPEN_DOCUMENT_TREE` Absicht. Diese Absicht bewirkt, dass das System alle Instanzen des Ressourcenanbieters anzuzeigen, die Auswahl der Unterstruktur zu unterstützen; der Benutzer wird dann durchsucht, und wählt ein Verzeichnis.

-   Zum Erstellen und verwalten neue Dokumente oder Verzeichnisse an einer beliebigen Stelle unter einer Unterstruktur, verwenden Sie die neue `CreateDocument`, `RenameDocument`, und `DeleteDocument` Methoden `DocumentsContract`.

-   Um Pfade zu den Verzeichnissen der Medien auf allen Geräten der freigegebenen Speicher zu erhalten, rufen Sie die neue `Android.Content.Context.GetExternalMediaDirs` Methode.

Weitere Informationen zu neuen Speicher-APIs in Android 5.0, finden Sie unter [Storage](https://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Drahtlose und Konnektivität

Android 5.0 fügt die folgenden API-Erweiterungen für drahtlose Netzwerke und Konnektivität hinzu:

-   Neue *Multi-Network* APIs, die es möglich, dass apps suchen und Auswählen von Netzwerken mit bestimmten Funktionen vor dem Herstellen einer Verbindung.

-   Bluetooth-Broadcasting Funktionen, die ein Android 5.0-Gerät, das als ein energiesparendes Bluetooth-Peripheriegeräte fungiert.

-   NFC-Erweiterungen, die die near-Field Communications-Funktion nutzen, für die Freigabe von Daten mit anderen Geräten zu vereinfachen.

Weitere Informationen zu den neuen drahtlosen und -APIs in Android 5.0-Konnektivität, finden Sie unter [drahtlosen und Konnektivität](https://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Auftragsplanung

Android 5.0 führt einen neuen `JobScheduler` API, die Benutzern helfen kann zu akkuentleerung durch Planen bestimmte Aufgaben ausführen, nur, wenn das Gerät im Netzbetrieb befindet, und dann damit, minimieren. Diese auftragsplanungsfunktion kann auch verwendet werden, zum Planen einer Aufgabe ausgeführt wird, wenn Bedingungen sind besser geeignet, für die jeweilige Aufgabe, z. B. eine große Datei herunterladen, wenn das Gerät über eine Wi-Fi-Netzwerk anstelle einer getakteten Netzwerken verbunden ist.

Weitere Informationen zu den neuen Auftrag planen von APIs in Android 5.0, finden Sie unter [planen Aufträge](https://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine Übersicht über wichtige neue Features in Android 5.0 für Xamarin.Android-app-Entwickler bereitgestellt:

-   Materialdesign

-   Animationen

-   Ansicht-Schatten und erhöhte Rechte

-   Color-Features, z. B. drawable Farben und deutliche Farbe extrahieren

-   Die neue `RecyclerView` und `CardView` Widgets

-   Verbesserungen der Benachrichtigung

-   Neue APIs für die Kamera, Audiowiedergabe, Media-Steuerelement, Speicher, WLAN-Verbindung und auftragsplanung

Wenn Sie die Xamarin Android-Entwicklung vertraut sind, lesen Sie [Setup und Installation](~/android/get-started/installation/index.md) können Sie den ersten Schritten mit Xamarin.Android.
[Hallo, Android](~/android/get-started/hello-android/index.md) ist eine ausgezeichnete Einführung für die lernen, wie Sie Android-Projekte zu erstellen.



## <a name="related-links"></a>Verwandte Links

- [Android-L-Entwicklervorschau](https://developer.android.com/preview/index.html)
- [Abrufen des Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Materialdesign](https://developer.android.com/preview/material/index.html)
- [Material Entwurfsprinzipien](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
