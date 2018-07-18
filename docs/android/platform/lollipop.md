---
title: Lollipop-Funktionen
description: Dieser Artikel enthält eine grobe Übersicht der neuen Funktionen in Android 5.0.x (Lollipop) eingeführt. Zu diesen Funktionen gehören, eine neue Benutzer-schnittstellenstil Design "Material" sowie neue unterstützende Funktionen, z. B. Animationen anzeigen Schatten und zeichenbaren Farben aufgerufen wird. Android 5.0 schließt auch erweiterte Benachrichtigungen, zwei neue UI-Widgets, ein neues Auftragsplaner und eine Handvoll neuer APIs, um Speicher-, Netzwerk-, Konnektivität und multimedia-Funktionen zu verbessern.
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: cdef611525abbe4f066959c0ac56380b1c617747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773614"
---
# <a name="lollipop-features"></a>Lollipop-Funktionen

_Dieser Artikel enthält eine grobe Übersicht der neuen Funktionen in Android 5.0.x (Lollipop) eingeführt. Zu diesen Funktionen gehören, eine neue Benutzer-schnittstellenstil Design "Material" sowie neue unterstützende Funktionen, z. B. Animationen anzeigen Schatten und zeichenbaren Farben aufgerufen wird. Android 5.0 schließt auch erweiterte Benachrichtigungen, zwei neue UI-Widgets, ein neues Auftragsplaner und eine Handvoll neuer APIs, um Speicher-, Netzwerk-, Konnektivität und multimedia-Funktionen zu verbessern._

## <a name="lollipop-overview"></a>Lollipop-Übersicht

Android 5.0 (Lollipop) führt eine neue Sprache für Entwurf *Material Entwurf*, damit ein unterstützender umgewandelt werden neue Funktionen zur apps einfacher und intuitiver verwenden können. Mit Material Entwurf bietet Android 5.0 Android-Smartphones nicht nur einen überarbeiteten; Darüber hinaus einen neuen Satz von Regeln für Android-basierte Tablets, Desktopcomputer, Überwachungen und intelligenten Fernsehgeräte. Diese Regeln zum Hervorheben, Einfachheit und Minimalism beim treffen der vertrauten praktisch Attribute (z. B. realistische Fläche und Edge-Cues) zum Unterstützen von Benutzern schnell und intuitiv zu verstehen, die Schnittstelle verwenden.

*Design "Material"* ist die verkörperung diese Benutzeroberfläche Entwurfsprinzipien Android. In diesem Artikel beginnt durch abdecken Material-Design unterstützenden Funktionen:

-   **Animationen** &ndash; *berühren Feedback* Animationen, *Aktivitätsübergang* Animationen, *anzeigen Statusübergang* Animationen und eine *Offenlegen Auswirkungen*.

-   **Shadows und Erhöhung der Rechte anzeigen** &ndash; Ansichten verfügen jetzt über eine `elevation` Eigenschaft; Sichten mit höheren `elevation` Werte größer Schatten im Hintergrund.

-   **Farbe Funktionen** &ndash; *zeichenbaren Farben* ermöglicht es Ihnen Bildanlagen wiederverwenden, indem Sie ihre Farbe ändern und *deutliche Farbe Extraktion* hilft Ihnen dynamisch Design Ihrer app Grundlage Farben in einem Bild an.

Viele Material-Design, das Funktionen bereits integriert sind die Benutzeroberfläche für Android 5.0 auftreten, während andere explizit apps hinzugefügt werden müssen. In einigen Standardansichten (etwa Schaltflächen) bereits Touch-Feedback-Animationen, während apps aktivieren, die meisten Ansicht Schatten können enthalten können.

Zusätzlich zu den UI-Verbesserungen über Design "Material" gebracht umfasst Android 5.0 auch mehrere andere neuen Funktionen, die behandelt werden in diesem Artikel:

-   **Erweiterte Benachrichtigungen** &ndash; Benachrichtigungen in Android 5.0 erheblich mit ein neues Erscheinungsbild, Unterstützung für Benachrichtigungen für den sperrbildschirmhintergrund und ein neues aktualisiert wurden *Heads-Up* Presentation-benachrichtigungsformat.

-   **Neue UI-Widgets** &ndash; neuen `RecyclerView` Widget erleichtert es apps, um große Datasets und komplexe Informationen und die neue vermitteln `CardView` Widget enthält ein vereinfachten Darstellung der Karte-ähnlichen Format zur Anzeige von Text und Bilder.

-   **Neue APIs** &ndash; Android 5.0 fügt neue APIs für die Netzwerkunterstützung für mehrere Bluetooth-Konnektivität, einfacher Speicherverwaltung und flexiblere Steuerung der Player und Kamerageräte verbessert. Ein neuer Auftrag planen Feature steht auszuführende Aufgaben asynchron zu geplanten Zeiten. Diese Funktion wird Akkulaufzeit, verbessert z. B. Planen von Tasks zum erfolgen soll, wenn das Gerät angeschlossen ist und geladen.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Android 5.0-Funktionen in Xamarin-basierten apps zu verwenden:

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-   **Android SDK** &ndash; Android 5.0.x (API 21) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin.Android requires [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) or later if you are developing for API level 24 or greater (JDK 1.8 also supports API levels earlier than 24, including Lollipop). Die 64-Bit-Version des JDK 1.8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder das Vorschauprogramm Forms verwenden.

Sie können weiterhin [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Domänenmodus speziell für API-Ebene 23 entwickeln oder eine frühere Version.


## <a name="setting-up-an-android-50-project"></a>Einrichten eines Android 5.0-Projekts

Um ein Android 5.0-Projekt zu erstellen, müssen Sie die neuesten Tools und SDK-Pakete installieren. Verwenden Sie die folgenden Schritte aus, um ein Xamarin.Android-Projekt, dessen Ziel Android 5.0 einrichten:

1. Installieren Sie Xamarin.Android Tools, und aktivieren Sie die Xamarin-Lizenz. Finden Sie unter [Einrichtung und Installation](~/android/get-started/installation/index.md) für Weitere Informationen zum Installieren von Xamarin.Android.

2. Bei Verwendung von Visual Studio für Mac installieren Sie die neuesten Updates für Android 5.0.

3. Starten Sie den Android SDK-Manager (in Visual Studio für Mac verwenden **Tools &gt; öffnen Android SDK Manager&hellip;**), und Installieren von Android SDK-Tools 23.0.5 oder höher:

    [![Android SDK-Tools in den Android SDK-Manager auswählen.](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   Zudem installieren Sie die neueste Android 5.0-SDK-Paketen (API 21 oder höher):

    [![Installieren von Android 5.0 SDK-Paketen im Android SDK Manager](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Weitere Informationen zur Verwendung von Android SDK Manager finden Sie unter [SDK-Manager](http://developer.android.com/tools/help/sdk-manager.html).

4. Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Android-Projekte. Wenn Sie ein Android-Projekt erstellen, achten Sie darauf, dass Sie die versionseinstellungen für Android 5.0 konfigurieren.
   Wechseln Sie in Visual Studio für Mac zu **Projektoptionen &gt; erstellen &gt; allgemeine** und legen Sie **Zielframework** auf **Android 5.0.x (Lollipop)** oder weiter unten:

    ![Der Ziel-Framwework festlegen auf Android 5.0 Lollipop](lollipop-images/target-framework.png)

   Klicken Sie unter **Projektoptionen &gt; erstellen &gt; Android-Anwendung**, minimale festgelegt und Android-Version als Ziel **Automatic - Framework-Zielversion verwenden**:

    ![Die Mindest- und Ziel-Android-Versionen festlegen auf automatisch](lollipop-images/minimum-android-version.png)

5. Konfigurieren Sie einen Emulator oder einem Android-Gerät zum Testen Ihrer app. Wenn Sie einen Emulator verwenden, finden Sie unter [Android-Emulator-Setup](~/android/get-started/installation/android-emulator/index.md) Informationen zum Konfigurieren von Android-Emulator für die Verwendung mit Xamarin Studio oder Visual Studio. Wenn Sie ein Android-Gerät verwenden, finden Sie unter [Einstellung von der Preview-SDKS](https://developer.android.com/preview/setup-sdk.html) zu erfahren, wie Sie Ihr Gerät für Android 5.0 zu aktualisieren. Um Ihr Android-Gerät zum Ausführen und Debuggen von Xamarin.Android konfigurieren zu können, finden Sie unter [Festlegen von Geräten für die Entwicklung](~/android/get-started/installation/set-up-device-for-development.md).

Hinweis: Wenn Sie ein vorhandenes Android-Projekt aktualisieren, die die Vorschau für Android L Zielgruppenadressierung wurde, müssen Sie aktualisieren die **Zielframework** und **Android-Version** auf die oben beschriebenen Werte.

## <a name="important-changes"></a>Wichtige Änderungen

Zuvor konnte veröffentlichten Android-apps von Änderungen in Android 5.0 nicht beeinträchtigt. Insbesondere wird Android 5.0 verwendet, eine neue Laufzeit und -benachrichtigungsformat für einen erheblich geändert.

### <a name="android-runtime"></a>Android-Laufzeit

Android 5.0 wird die neue Android Common Language Runtime (Grafik) als standardruntime anstelle von Dalvik verwendet. Grafiken implementiert einige der wichtigsten neuen Features:

-   **Ahead-des-Time (AOT)-Kompilierung** &ndash; AOT kann app-Leistung verbessern, indem Sie die app-Code kompilieren, bevor die app zuerst gestartet wird. Wenn eine app installiert ist, generiert ART eine kompilierte app, die ausführbare Datei für das Zielgerät.

-   **Verbesserte Garbagecollection (GC)** &ndash; GC-Verbesserungen in Grafiken können auch die app-Leistung verbessern. Garbagecollection jetzt eine GC anhalten statt zwei verwendet und parallele GC-Operationen zu beenden, mehr rechtzeitig verarbeitet.

-   **Verbesserte Anwendungsdebuggen** &ndash; ART bietet diagnostische genauer, um Hilfe bei der Analyse von Ausnahmen und Absturzberichte.

Vorhandene apps sollte funktionieren ohne Änderung unter ART &ndash; mit Ausnahme von apps, die Techniken, die in der vorherigen Dalvik-Runtime eindeutige auszunutzen, die funktionieren möglicherweise nicht unter ART. Weitere Informationen zu diesen Änderungen finden Sie unter [Überprüfen von App-Verhalten auf Android Common Language Runtime (Grafik)](http://developer.android.com/guide/practices/verifying-apps-art.html).


### <a name="notification-changes"></a>Benachrichtigung ändert

Benachrichtigungen wurden in Android 5.0 erheblich geändert:

-   **Sounds und Vibrationen anders behandelt** &ndash; akustische und Vibrationen jetzt von behandelt `Notification.Builder` anstelle von `Ringtone`, `MediaPlayer`, und `Vibrator`.

-   **Neue Farbschema** &ndash; in Übereinstimmung mit Material Design Benachrichtigungen mit dunkel Text über weiß oder sehr Hintergründe gerendert werden. Darüber hinaus können alpha-Kanal in Benachrichtigungssymbole von Android zur Koordinierung mit Farbschemas System geändert werden. 

-   **Für den Sperrbildschirmhintergrund Benachrichtigungen** &ndash; Benachrichtigungen können jetzt auf dem Gerät für den sperrbildschirmhintergrund angezeigt werden.

-   **Heads-Up** &ndash; hoher Priorität Benachrichtigungen jetzt angezeigt werden, in einem kleinen schwebenden Fenster (Heads-Up-Benachrichtigung) Wenn das Gerät ist nicht gesperrt und der Bildschirm aktiviert ist.

In den meisten Fällen erfordert Portieren auf Android 5.0 vorhandenen app-Benachrichtigung-Funktionalität die folgenden Schritte aus:

1.  Konvertieren Sie den Code so `Notification.Builder` (oder `NotificationsCompat.Builder`) zum Erstellen von Benachrichtigungen. 

2.  Stellen Sie sicher, dass Ihre vorhandene Benachrichtigung Medienobjekte in das neue Design "Material" Farbschema angezeigt werden.

3.  Entscheiden Sie, welche Sichtbarkeit Ihrer Benachrichtigungen verfügen sollen, auf die für den sperrbildschirmhintergrund gegeben werden. Wenn eine Benachrichtigung nicht öffentlich ist, sollte auf die für den sperrbildschirmhintergrund welche Inhalte angezeigt?

4.  Die Kategorie der Benachrichtigungen festgelegt, damit sie ordnungsgemäß, in die neue Android 5.0 behandelt werden *nicht stören* Modus.

Wenn Ihre Benachrichtigungen Steuerelemente, Anzeige Medien Wiedergabe Status vorhanden ist, verwenden Sie `RemoteControlClient`, oder rufen Sie `ActivityManager.GetRecentTasks`, finden Sie unter [wichtigen Verhaltensänderungen](http://developer.android.com/preview/api-overview.html#Behaviors) für Weitere Informationen zum Aktualisieren der Benachrichtigungen für Android 5.0.

Informationen zum Erstellen von Benachrichtigungen in Android finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md). Die [Kompatibilität](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) Abschnitt dieses Artikels wird erläutert, wie Benachrichtigungen zu erstellen, sind nach unten weisenden kompatibel, mit früheren Versionen von Android.


## <a name="material-theme"></a>Design "Material"

Das neue Android 5.0 Material Design setzt umgebungsänderungen Änderungen auf das Aussehen und Verhalten der Android-Benutzeroberfläche. Visuelle Elemente verwenden jetzt praktisch Oberflächen, die auf den fett formatierten Grafiken, Typografie und helle Farben druckbasierten Entwurfs akzeptieren. Beispiele für Design "Material" werden in den folgenden Screenshots dargestellt:

[![Screenshots der Startbildschirm des Materials Design, Bildschirm "Apps" und Einstellung Bildschirm](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0.x begrüßt Sie mit dem Startbildschirm auf der linken Seite angezeigt. Ein Screenshot, der Center ist die erste Seite des app-Liste und die Screenshot, der auf der rechten Seite ist die **Einstellungen** Bildschirm. Google [Material Entwurf](https://material.io/guidelines/material-design/introduction.html) Spezifikation erläutert die Regeln zum zugrunde liegenden hinter das neue Design "Material"-Konzept.

Design "Material" enthält drei integrierte Typen, die Sie in Ihrer app verwenden können: die `Theme.Material` Design "dunkel" (Standard), die `Theme.Material.Light` Design, und die `Theme.Material.Light.DarkActionBar` Design: 

[![Screenshots dunkle, helle und DarkActionBar Designs](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Weitere Informationen zum Verwenden von Material Design-Funktionen in Xamarin.Android-apps finden Sie unter [Design "Material"](~/android/user-interface/material-theme.md).


## <a name="animations"></a>Animationen

Android 5.0 bietet Touch-Feedback-Animationen, Aktivität Übergangsanimationen und Ansicht Zustand Übergangsanimationen app Schnittstellen intuitivere Verwendung vornehmen. Darüber hinaus können Android 5.0-apps *Offenlegen Auswirkungen* Ansichten anzeigen oder Ausblenden von Fenstern. Sie können *gekrümmt, während des Verschiebens* Einstellungen zum Konfigurieren des wie schnell oder langsam Animationen werden gerendert.


### <a name="touch-feedback-animations"></a>Touch-Feedback-Animationen

Touch-Feedback-Animationen bieten Benutzern visuelles Feedback, wenn eine Sicht verwendet wurde. Z. B. Schaltflächen nun anzeigen eine sich allmählich ausbreitenden Wirkung berührt werden &ndash; Dies ist die Animation für die Bewertung von Touch Android 5.0. Ripple Animation wird implementiert, nach dem neuen `RippleDrawable` Klasse. Die sich allmählich ausbreitenden Wirkung kann an die Grenzen der Sicht beenden oder zu erweitern, die außerhalb des zulässigen Bereichs der Sicht konfiguriert werden. Die folgende Sequenz von Screenshots veranschaulicht beispielsweise die Welleneffekt in eine Schaltfläche während Touch Animation:

![Bild von Frame Screenshots Ripple Animation auf eine Schaltfläche](lollipop-images/touch-animation.png)

Anfängliche Touch Kontakt mit der Schaltfläche "" tritt auf, in der ersten Abbildung auf der linken Seite während die verbleibenden Sequenz (von links nach rechts) wird veranschaulicht, wie sich allmählich ausbreitenden Wirkung der an den Rand der Schaltfläche ausbreitet. Wenn die Animation Ripple beendet wird, gibt die Sicht mit der ursprünglichen Darstellung zurück. Die Standardeinstellung Ripple Animation findet in einem Bruchteil einer Sekunde, aber die Länge der Animation für längere oder kürzere längere Zeit angepasst werden.

Weitere Informationen über die Feedback-Animationen in Android 5.0 berühren, finden Sie unter [berühren Feedback anpassen](http://developer.android.com/training/material/animations.html#Touch).


### <a name="activity-transition-animations"></a>Aktivität Übergangsanimationen

Aktivität Übergangsanimationen ermöglichen Benutzern einen Eindruck von visuelle Kontinuität, sobald eine Aktivität in einen anderen übergeht. Apps können drei Arten von übergangsanimation angeben:

-   **Geben Sie ein Übergang** &ndash; für, wenn eine Aktivität die Szene eingibt.

-   **Beenden der Übergang** &ndash; für, wenn eine Aktivität die Szene beendet wird.

-   **Freigegebene Element Übergang** &ndash; für eine Sicht, die zwei Aktivitäten gemein sind als die erste Aktivität Übergänge zum nächsten Wenn ändert.

Die folgende Sequenz von Screenshots veranschaulicht beispielsweise einen Übergang freigegebene Element:

[![Bild von Frame Screenshots einer freigegebenen Element Übergang Animation](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Ein freigegebene Element (ein Foto des eine Caterpillar) ist eine von mehreren Ansichten in der ersten Aktivität; Es wird die einzige Ansicht in der zweiten Aktivität als die erste Aktivität Übergänge an den zweiten dahingehend vergrößert.

#### <a name="enter-transition-animation-types"></a>Geben Sie ein Übergang Animationstypen

Geben Sie Übergänge bietet Android 5.0 drei Typen von Animationen:

-   **Explodieren Animation** &ndash; eine Sicht aus der Mitte der Szene vergrößert.

-   **Schieben Sie Animation** &ndash; verschiebt eine Sicht in eine von den Rändern des themawechsel.

-   **Animation ausblenden** &ndash; ausgeblendet wird, eine Sicht in der Szene haben.

#### <a name="exit-transition-animation-types"></a>Exit-Übergang-Animationstypen

Exit-Übergänge bietet Android 5.0 drei Typen von Animationen:

-   **Explodieren Animation** &ndash; eine Sicht in die Mitte der Szene verkleinert.

-   **Schieben Sie Animation** &ndash; verschiebt eine Sicht auf einen der Ränder einer Szene.

-   **Animation ausblenden** &ndash; ausgeblendet wird, eine Sicht aus der Szene haben.

#### <a name="shared-element-transition-animation-types"></a>Freigegebene Übergang Animation-Elementtypen

Freigegebene Element Übergänge unterstützen mehrere Arten von Animationen, wie z. B.:

-   Ändern das Layout und Clip Grenzen einer Sicht.

-   Ändern der Skalierung und Drehung einer Sicht.

-   Ändern der Größe und Skalierung für eine Sicht.

Weitere Informationen zur Aktivität Übergangsanimationen Android 5.0 finden Sie unter [anpassen Aktivität Übergänge](http://developer.android.com/training/material/animations.html#Transitions).


### <a name="view-state-transition-animations"></a>Ansichtszustand Übergangsanimationen

Android 5.0.x ermöglicht Animationen ausführen, wenn der Status einer Sicht ändert. Sie können die Ansicht Statusübergänge animieren, mithilfe einer der folgenden Methoden:

-   Erstellen Sie Drawables, die animiert Zustandsänderungen, die einer bestimmten Ansicht zugeordnet. Die neue `AnimatedStateListDrawable` -Klasse können Sie die Drawables zu erstellen, die zwischen Ansichtszustandsänderungen Animationen anzeigen.

-   Definieren Sie die Animationsfunktionen, die ausgeführt wird, wenn der Status einer Sicht ändert. Die neue `StateListAnimator` -Klasse können Sie einen Animator definieren, die ausgeführt wird, wenn der Status einer Sicht ändert.

Weitere Informationen darüber anzeigen Zustand Übergangsanimationen Android 5.0, finden Sie unter [Ansichtszustandsänderungen animieren](http://developer.android.com/training/material/animations.html#ViewState).


### <a name="reveal-effect"></a>Anzeigen der Auswirkungen

Die *Offenlegen Auswirkungen* ist ein Kreis Clipping diese Änderungen Radius zum Anzeigen oder Ausblenden einer Ansicht. Dieser Effekt können Sie steuern, indem Sie den ersten und endgültigen Radius des Kreises Clipping festlegen. Die folgende Sequenz von Screenshots veranschaulicht eine Animation für den Effekt anzeigen aus der Mitte des Bildschirms:

[![Bild von Frame Screenshots der Animation anzeigen](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

Die nächste Sequenz veranschaulicht eine anzeigen Auswirkungen Animation an, die aus der unteren linken Ecke des Bildschirms stattfindet:

[![Bild von Frame Screenshots Clipping Animation](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Offenlegen Sie, dass Animationen umgekehrt werden können; d. h. kann der Kreis Clipping verkleinert die Ansicht anstatt vergrößern, um die Ansicht anzuzeigen.

Weitere Informationen zu den Auswirkungen des Android 5.0-anzeigen in finden Sie unter [verwenden Sie die Auswirkungen Offenlegen](http://developer.android.com/training/material/animations.html#Reveal).


### <a name="curved-motion"></a>Gekrümmte Motion

Zusätzlich zu diesen Animationsfeatures bietet Android 5.0 auch neue APIs, die Ihnen die Angabe der Uhrzeit und während des Verschiebens Kurven von Animationen zu ermöglichen. Android 5.0 verwendet diese Kurven zum Verschieben von temporalen und räumliche während Animationen zu interpolieren. Android 5.0 sind drei Kurven definiert:

-   **Schnelle\_out\_lineare\_in** &ndash; schnell beschleunigt und weiterhin bis zum Ende der Animation zu beschleunigen.

-   **Schnelle\_out\_langsam\_in** &ndash; Accelerates schnell und langsam gegen Ende der Animation verlangsamt wird.

-   **Lineare\_out\_langsam\_in** &ndash; beginnt mit der Geschwindigkeit Haupt- und langsam bis zum Ende der Animation verlangsamt.

Sie können die neue `PathInterpolator` Klasse, um anzugeben, wie während des Verschiebens Interpolation stattfindet. `PathInterpolator` ist einer Interpolators, der die Animation Pfade entsprechend den angegebenen Kontrollpunkten und während des Verschiebens Kurven durchläuft. Weitere Informationen zum Angeben von gekrümmten Bewegung Einstellungen Android 5.0 finden Sie unter [verwenden gekrümmte Motion](http://developer.android.com/training/material/animations.html#CurvedMotion).


## <a name="view-shadows--elevation"></a>Ansicht Schatten & Erhöhung der Rechte

Android 5.0 können Sie angeben der *rechteerweiterung* einer Ansicht durch Festlegen einer neuen `Z` Eigenschaft. Eine größere `Z` Wert bewirkt, dass die Ansicht, um eine größere Schatten im Hintergrund, machen die Ansicht "float" über dem Hintergrund höher angezeigt werden. Sie können die anfängliche Höhe einer Sicht festlegen, indem Sie konfigurieren die `elevation` Attribut im Layout.

Das folgende Beispiel veranschaulicht die Schatten durch eine leere `TextView` steuern, wenn dessen Attribut Erhöhung bzw. 2dp, 4dp und 6dp, festgelegt ist:

[![Screenshots von Progessively größere Ansicht Schatten](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Schatten ansichtseinstellungen können statisch, (siehe oben) sein, oder sie können Animationen, stellen eine Ansicht angezeigt werden, über die Ansicht Hintergrund vorübergehend steigen verwendet werden. Sie können die `ViewPropertyAnimator` Klasse, um die Höhe einer Sicht zu animieren. Die Höhe einer Sicht ist die Summe des Layouts `elevation` Einstellung zuzüglich einer `translationZ` -Eigenschaft, die Sie durch festlegen können eine `ViewPropertyAnimator` -Methodenaufruf.

Weitere Informationen zum Anzeigen von Schatten im Android 5.0, finden Sie unter [Schatten definieren und Ansichten Clippings](http://developer.android.com/training/material/shadows-clipping.html).


## <a name="color-features"></a>Farbe-Funktionen

Android 5.0 enthält zwei neue Funktionen für die Verwaltung von Farben in apps:

-   *Zeichenbaren Farben* können Sie die Farben der Bildanlagen verändern, indem ein Layoutattribut.

-   *Deutliche Farbe Extraktion* ermöglicht es Ihrer app Farbschema zur Koordinierung mit der Farbpalette eines angezeigten Bilds dynamisch anpassen.


### <a name="drawable-tinting"></a>Zeichenbaren Farben

Android 5.0 Layouts Erkennen eines neuen `tint` -Attribut, das Sie verwenden können, um die Farbe der Drawables festlegen, ohne mehrere Versionen dieser Ressourcen auf unterschiedliche Farben angezeigt erstellen zu müssen. Um dieses Feature verwenden zu können, definieren Sie eine Bitmap als alpha Maske und Verwenden der `tint` -Attribut zum Definieren der Farbe des Medienobjekts. Dadurch möglich, Bestand einmal erstellen und die Farbe für diese in dem Layout Ihr Design übereinstimmen.

Im folgenden Beispiel, eine einzelne imagemedienobjekt &ndash; ein weißes Logo mit einem transparenten Hintergrund &ndash; wird verwendet, um den Farbton Variationen zu erstellen:

![Xamarin-Logo mit transparentem Hintergrund weiß](lollipop-images/xamarin-logo-white.png)

Dieses Logo wird über einen blauen zirkuläre Hintergrund angezeigt, wie in den folgenden Beispielen gezeigt. Das Bild auf der linken Seite ist wie das Logo angezeigt wird, ohne eine `tint` Einstellung. In der Mitte, das Logo des Bilds `tint` -Attribut auf einen dunkelgrau festgelegt ist. In der Abbildung auf der rechten Seite `tint` auf eine hellgrau festgelegt ist:

![Beispiele für die oben genannten Logo mit verschiedenen Farbton-Einstellungen](lollipop-images/drawable-tinting.png)

Weitere Informationen zu zeichenbaren Farben Android 5.0 finden Sie unter [zeichenbaren Farben](http://developer.android.com/training/material/drawables.html#DrawableTint).


### <a name="prominent-color-extraction"></a>Deutliche Farbe extrahieren

Die neue Android 5.0 `Palette` -Klasse können Sie die Farben aus einem Image extrahieren, sodass Sie sie auf einer benutzerdefinierten Farbpalette dynamisch anwenden können. Die `Palette` Klasse sechs Farben aus einem Image extrahiert und diese Farben entsprechend ihrer relativen konvergenzebenen Farbe Sättigung und Helligkeit "Bezeichnungen":

-   Vibrant

-   Dynamisches dunkel

-   Dynamisches Licht

-   Stumm geschaltet

-   Stumm geschalteter dunkel

-   Stumm geschalteter Licht

Beispielsweise wird in den folgenden Screenshots ein ein Foto, das Anzeigen von app extrahiert die deutliche Farben aus dem Image auf Anzeige und verwendet diese Farben das Farbschema der app das Bild entsprechend angepasst werden kann:

[![Screenshots Grün, rosa und blaue Design Farbe Extraktionen](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

In den oben genannten Screenshots die Aktionsleiste auf die extrahierte "lebendige Light" festgelegt ist, Farbe und Hintergrund extrahierten "lebendige dunkel" festgelegt ist Farbe. In jedem Beispiel ist eine Zeile mit kleinen Farbe Quadrate eingeschlossen, um die Palettenfarben zu erläutern, die aus dem Abbild extrahiert wurden.

Weitere Informationen zum Extrahieren von Farbe in Android 5.0, finden Sie unter [extrahieren deutliche Farben aus einem Image](http://developer.android.com/training/material/drawables.html#ColorExtract).


## <a name="new-ui-widgets"></a>Neue UI-Widgets

Android 5.0 führt zwei neue UI-Widgets:

-   `RecyclerView` &ndash; Eine Gruppe "Ansicht", in dem eine Liste von bildlauffähigen Elementen angezeigt.

-   `CardView` &ndash; Eine grundlegende Layout mit abgerundeten Ecken.

Beide Widgets sind integrierten Unterstützung für Material Design-Funktionen: z. B. `RecyclerView` verwendet Sie zum Hinzufügen und Entfernen von Ansichten, Animationen und `CardView` verwendet anzeigen Schatten um jeder Karte angezeigt werden, über den Hintergrund float zu machen. Beispiele für diese neue Widgets sind in den folgenden Screenshots aufgeführt:

[![Screenshots der apps, die mit RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Der Screenshot, der auf der linken Seite ist ein Beispiel für `RecyclerView` wie in einer e-Mail-app und der Screenshot auf das Recht ist ein Beispiel für `CardView` wie in einer app des Reise-Reservierung verwendet.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` ähnelt dem `ListView,` , aber sie eignet sich besser für große Mengen von Sichten oder Listen mit Elementen, die sich dynamisch ändern. Wie `ListView,` Geben Sie einen Adapter für den Zugriff auf das zugrunde liegende DataSet. Anders als bei `ListView,` Sie verwenden eine *Layout-Manager* , positionieren Sie Elemente in `RecyclerView`. Der Layout-Manager übernimmt außerdem Ansicht Wiederverwendung; Er verwaltet die Wiederverwendung des Item-Sichten, die nicht mehr für den Benutzer sichtbar sind.

Bei Verwendung von einer `RecyclerView` Widget "", geben Sie eine `LayoutManager` und ein Adapter. In dieser Abbildung gezeigten `LayoutManager` der Vermittler zwischen dem Adapter und der `RecyclerView`:

![Diagramm der RecyclerView mit unterstützenden LayoutManager, Adapter und Dataset](lollipop-images/recyclerview-diagram.png)

Den folgenden Screenshots veranschaulicht eine `RecyclerView` 100 Elemente enthält (jedes Element besteht aus einer `ImageView` und ein `TextView`):

[![Screenshots Scrollen durch Bilder RecyclerView-App](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` behandelt diese große DataSet problemlos &ndash; Durchführen eines Bildlaufs vom Anfang der Liste, um das Ende der Liste der in diesem Beispiel app nur wenige Sekunden benötigt. `RecyclerView` unterstützt auch Animationen; Tatsächlich sind Animationen zum Hinzufügen und Entfernen von Elementen in der Standardeinstellung aktiviert. Wenn ein Element hinzugefügt wird, um eine `RecyclerView`, es ausgeblendet darüber, wie in der folgenden Reihenfolge Screenshots gezeigt wird:

[![Durch die Frame-Screenshot, der ein Foto Element ein-oder ausblenden im Frame](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Weitere Informationen zu den `RecyclerView`, finden Sie unter [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


### <a name="cardview"></a>CardView

`CardView` ist eine einfache Sicht, die eine Gleitkommazahl Karte mit abgerundeten Ecken simuliert. Da `CardView` verfügt über integrierte Ansicht Shadows, bietet eine einfache Möglichkeit für Sie visuelle Tiefe Ihrer app hinzufügen. Die folgenden Screenshots zeigen drei textorientierte Beispiele `CardView`:

[![Beispiel-Screenshots von apps mithilfe von RecyclerView mit CardView basierende Elemente](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Jede Karten im obigen Beispiel enthält eine `TextView`; die Farbe des Hintergrunds festgelegt ist, über die `cardBackgroundColor` Attribut.

Weitere Informationen zu den `CardView`, finden Sie unter [CardView](~/android/user-interface/controls/card-view.md).


## <a name="enhanced-notifications"></a>Verbesserte Benachrichtigungen

Das Benachrichtigungssystem Android 5.0 wurde erheblich mit einem neuen visuellen Format und neuen Funktionen aktualisiert. Benachrichtigungen haben ein neues Erscheinungsbild Android 5.0. Benachrichtigungen in Android 5.0 verwenden z. B. jetzt dunkel Text über einen heller Hintergrund:

![Beispiel für eine nicht erweiterten Android 5.0-Benachrichtigung](lollipop-images/expanded-notification-contracted.png)

Wenn ein großes Symbol in einer Benachrichtigung (wie im obigen Beispiel gezeigt) angezeigt wird, stellt Android 5.0 des kleinen Symbols als ein Signal über die "große Symbole" an. 

Android 5.0 können Benachrichtigungen auch auf dem Gerät für den sperrbildschirmhintergrund angezeigt.
Hier ist z. B. ein Beispiel-Screenshot, der eine für den sperrbildschirmhintergrund mit eine einzige Benachrichtigung:

[![Screenshot der Benachrichtigung angezeigt werden, auf dem Sperrbildschirm](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Benutzer können Doppeltippen Sie auf eine Benachrichtigung auf der für den sperrbildschirmhintergrund zum Entsperren des Geräts und springen Sie zu der app, die diese Benachrichtigung stammt oder Streifen verwerfen die Benachrichtigung. Benachrichtigungen haben ein neues *Sichtbarkeit* festlegen, die bestimmt, wie viel Inhalt auf den für den sperrbildschirmhintergrund angezeigt werden kann. Benutzer können auswählen, ob vertraulichem Inhalt für den sperrbildschirmhintergrund Benachrichtigungen angezeigt werden können.

Android 5.0 führt eine neue Benachrichtigung mit hoher Priorität Präsentationsformat aufgerufen *Heads-Up*. Heads-Up-Benachrichtigungen vom oberen Rand des Bildschirms für einige Sekunden nach unten schieben, und klicken Sie dann zurück an die Benachrichtigung Schattierung am oberen Rand des Bildschirms zurückzukehren. Heads-Up-Benachrichtigungen ermöglichen es dem System UI, wichtige Informationen vor der Benutzer ohne Unterbrechung der momentan ausgeführte Aktivität eingefügt werden soll. Das folgende Beispiel veranschaulicht eine einfache Heads-Up-Benachrichtigung, die zusätzlich zu einer app anzeigt:

[![Beispiel einer heads-up Benachrichtigung](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Heads-Up-Benachrichtigungen werden in der Regel für die folgenden Ereignisse verwendet:

-   Eine neue Nachricht weiter

-   Einen eingehenden Telefonanruf

-   Angabe niedriger Akkukapazität

-   Ein alarm

Android 5.0 wird eine Benachrichtigung im Heads-Up-Format angezeigt, nur, wenn es eine Einstellung hohe oder max Priorität hat.

Android 5.0 können Sie Metadaten von abfragebenachrichtigungen Android sortieren und Anzeigen von Benachrichtigungen intelligenter helfen bereitstellen. Android 5.0.x werden Benachrichtigungen entsprechend der Priorität, Sichtbarkeit und Kategorie organisiert.
Benachrichtigungskategorien dienen zum Filtern der Benachrichtigungen über angezeigt werden, können Wenn das Gerät in *nicht stören* Modus.

Ausführliche Informationen zum Erstellen und starten die Benachrichtigungen mit den neuesten Android 5.0-Funktionen finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).


## <a name="new-apis"></a>Neue APIs

Zusätzlich zu den oben beschriebenen neue Aussehen und Verhalten-Standardfunktionen Android 5.0 neue APIs hinzugefügt, die die Funktionen von vorhandenen Multimedia, Speicher- und Wireless-Konnektivität Funktionalität zu erweitern. Android 5.0 enthält zudem neue APIs, die eine neue auftragsplanungsfunktion unterstützen.

### <a name="camera"></a>Kamera

Android 5.0 bietet mehrere neue APIs für erweiterte Kamera Funktionen. Die neue `Android.Hardware.Camera2` Namespace enthält Funktionen, für den Zugriff auf einzelne Kamerageräte mit einem Android-Gerät verbunden. Darüber hinaus `Android.Hardware.Camera2` jedes Kameragerät als Pipeline modelliert: akzeptiert einen erfassungsanforderung, Erfassung des Abbilds und gibt dann das Ergebnis. Dieser Ansatz ermöglicht es apps, um mehrere Capture-Anforderungen an einer Kamera in die Warteschlange.

Die folgenden APIs ermöglichen diese neuen Features:

-   `CameraManager.GetCameraIdList` &ndash; Hilft Ihnen dabei, Kamerageräte programmgesteuert zugreifen; Verwenden Sie `CameraManager.OpenCamera` für die Verbindung mit einem bestimmten Kameragerät.

-   `CameraCaptureSession` &ndash; Erfasst oder Bilder aus der Kamera gestreamt. Sie implementieren eine `CameraCaptureSession.CaptureListener` Schnittstelle, um das neue Image erfassen Ereignisse zu behandeln.

-   `CaptureRequest` &ndash; Capture Parameter definiert.

-   `CaptureResult` &ndash; Das Ergebnis eines Vorgangs Capture Image vorgestellt.

Weitere Informationen über die neue Kamera-APIs in Android 5.0 finden Sie unter [Medien](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Audiowiedergabe

Android 5.0 Updates der `AudioTrack` Klasse für eine bessere Audiowiedergabe:

-   `ENCODING_PCM_FLOAT` &ndash; Konfiguriert die `AudioTrack` Audiodaten im Gleitkommaformat für eine bessere dynamischen Bereichs, größer Toleranzbereich und höherer Qualität (unser Dank gilt eine höhere Genauigkeit) zu akzeptieren. Darüber hinaus hilft Ihnen Gleitkommaformat um audio Clipping zu vermeiden.

-   `ByteBuffer` &ndash; Sie können jetzt angeben, Audiodaten, `AudioTrack` als Bytearray.

-   `WRITE_NON_BLOCKING` &ndash; Diese Option vereinfacht die Pufferung und bei einigen apps multithreading.

Weitere Informationen zu den `AudioTrack` Android 5.0 steigen [Medien](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Media Playback Control

Android 5.0 führt die neue `Android.Media.MediaController` Klasse, welche ersetzt `RemoteControlClient`. `Android.Media.MediaController` bietet vereinfachten Transportkontrolle APIs und Thread-sichere Steuerelement der Wiedergabe außerhalb des Kontexts UI bietet. Die folgenden neuen APIs behandeln Transport-Steuerelement:

-   `Android.Media.Session.MediaSession` &ndash; Eine Media-Steuerelement-Sitzung, die mehrere Controller behandelt. Rufen Sie `MediaSession.GetSessionToken` zum Anfordern eines Tokens, die Ihre app verwendet wird, für die Interaktion mit der Sitzung.

-   `MediaController.TransportControls` &ndash; Handles transport Befehle wie z. B. **wiedergeben**, **beenden**, und **Skip**.

Darüber hinaus können Sie die neue `Android.App.Notification.MediaStyle` Klasse umfangreiche Benachrichtigungsinhalt (z. B. extrahieren, und Anzeigen von Album Art) eine Sitzung Medien zugeordnet werden soll.

Weitere Informationen zu den neuen Medien Wiedergabe Steuerelement Features Android 5.0 finden Sie unter [Medien](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Speicher

Android 5.0.x aktualisiert das Framework Speicher Zugriff, um Anwendungen zum Arbeiten mit Verzeichnissen und Dateien zu vereinfachen:

-   Zum Auswählen einer Verzeichnisunterstruktur erstellen und senden eine `Android.Intent.Action.OPEN_DOCUMENT_TREE` Absicht. Diese Absicht bewirkt, dass das System für alle Anbieterinstanzen anzuzeigen, die Auswahl der Unterstruktur zu unterstützen. der Benutzer ist dann durchsucht und wählt ein Verzeichnis.

-   Zum Erstellen und verwalten neue Dokumente oder Verzeichnisse an einer beliebigen Stelle unter einer Unterstruktur, verwenden Sie die neue `CreateDocument`, `RenameDocument`, und `DeleteDocument` Methoden der `DocumentsContract`.

-   Um Pfade zu den Medien Verzeichnisse auf alle freigegebenen speichervorrichtungen abzurufen, rufen Sie die neue `Android.Content.Context.GetExternalMediaDirs` Methode.

Weitere Informationen zu neuem Speicher-APIs in Android 5.0 finden Sie unter [Speicher](http://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Wireless & Konnektivität

Android 5.0 fügt die folgenden API-Erweiterungen für WLAN- und Konnektivität:

-   Neue *mit mehreren Netzwerk* APIs, die für apps suchen und Auswählen von Netzwerken mit spezifischen Funktionen vor dem Herstellen einer Verbindung ermöglichen.

-   Bluetooth-Broadcasting Funktionen, die ein Android 5.0-Gerät als ein niedriger Energie Bluetooth Peripheriegerät fungieren.

-   NFC-Erweiterungen, die die Verwendung von near-Field Communications-Funktionen für die Freigabe von Daten mit anderen Geräten zu vereinfachen.

Weitere Informationen zu den neuen drahtlosen und Konnektivität-APIs in Android 5.0 finden Sie unter [Wireless und Konnektivität](http://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Auftragsplanung

Android 5.0 führt einen neuen `JobScheduler` API, die Benutzern helfen minimieren Akku ausgleichen, indem Sie Planung bestimmte Aufgaben ausgeführt werden, nur, wenn das Gerät angeschlossen ist und geladen. Diese auftragsplanungsfunktion kann auch verwendet werden, für die Planung einer Aufgabe ausgeführt wird, wenn Bedingungen sind besser geeignet, um diese Aufgabe, z. B. eine große Datei herunterladen, wenn das Gerät über ein WLAN-Netzwerk anstelle einer getakteten Netzwerken verbunden ist.

Weitere Informationen zu den neuen Auftrag planen-APIs in Android 5.0 finden Sie unter [Planen von Aufträgen](http://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel einen Überblick über wichtige neue Features in Android 5.0 für Xamarin.Android app-Entwickler zur Verfügung:

-   Design "Material"

-   Animationen

-   Ansicht Schatten und Erhöhung der Rechte

-   Farbe-Funktionen, z. B. zeichenbaren Farbtönen und deutliche Farbe extrahieren

-   Die neue `RecyclerView` und `CardView` Widgets

-   Erweiterungen der Benachrichtigung

-   Neue APIs für die Kamera, Audiowiedergabe Media-Steuerelement, Speicher, Wireless-Konnektivität und Planung von Aufträgen

Wenn Sie Xamarin Android-Entwicklung vertraut sind, lesen Sie [Einrichtung und Installation](~/android/get-started/installation/index.md) lassen sich mit Xamarin.Android zu beginnen.
[Hello, Android](~/android/get-started/hello-android/index.md) ist eine hervorragende Einführung zum Erlernen der Android-Projekte erstellen.



## <a name="related-links"></a>Verwandte Links

- [Entwicklervorschau für Android L](http://developer.android.com/preview/index.html)
- [Abrufen der Android-SDK](https://developer.android.com/sdk/index.html#Other)
- [Material Entwurf](http://developer.android.com/preview/material/index.html)
- [Material Entwurfsprinzipien](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
