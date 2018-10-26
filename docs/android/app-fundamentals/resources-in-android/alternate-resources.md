---
title: Alternative Ressourcen
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 0384d96ddc96f8d0b16a42f691305f26ea25881d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108755"
---
# <a name="alternate-resources"></a>Alternative Ressourcen

Alternative Ressourcen sind Ressourcen, die ein bestimmtes Gerät oder einen Laufzeit-Konfiguration, z. B. die aktuelle Sprache, die bestimmten Bildschirmgröße und die Pixeldichte. Wenn Android eine Ressource werden, die für ein bestimmtes Gerät oder der Konfiguration spezifischer als der Standardressource ist verglichen kann, wird diese Ressource stattdessen verwendet werden. Wenn sie nicht mit eine andere Ressource, die die aktuelle Konfiguration entspricht findet, werden die Standardressourcen geladen werden. Wie Android entscheidet, wird welche Ressourcen von einer Anwendung verwendet werden ausführlicher unten im Abschnitt Ressourcenspeicherort abgedeckt werden

Alternative Ressourcen sind als ein Unterverzeichnis im Ordner "Ressourcen" gemäß den Ressourcentyp, genau wie Standardressourcen organisiert. Der Name des Unterverzeichnisses alternativen Ressource wird in der Form: _ResourceType_-_Qualifizierer_

*Qualifizierer* ist ein Name, der eine bestimmte Gerätekonfiguration identifiziert.
Es gibt möglicherweise mehr als einem Qualifizierer in einen Namen ein, jeweils getrennt durch einen Bindestrich. Der folgende Screenshot zeigt beispielsweise ein einfaches Projekt, das alternative Ressourcen für verschiedene Konfigurationen wie z. B. Gebietsschema, Bildschirm Dichte, Bildschirmgröße und-Ausrichtung sind:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Alternative Ressourcen](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Alternative Ressourcen](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Wenn Sie einen Ressourcentyp Qualifizierer hinzufügen, gelten die folgenden Regeln:

1. Es gibt möglicherweise mehr als einem Qualifizierer, die mit jeder Qualifizierer, die durch einen Bindestrich getrennt.

2. Die Qualifizierer, die vielleicht nur einmal angegeben werden.

3. Qualifizierer muss in der Reihenfolge, die sie in der folgenden Tabelle angezeigt werden.

Die möglichen Qualifizierer sind zu Referenzzwecken im folgenden aufgeführt:

- **MCC und MNC** &ndash; der [Ländercode für Mobilgerät](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) und optional die [Netzwerkcode](http://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). Die SIM-Karte bietet die MCC, während das Netzwerk aus, dem das Gerät, um verbunden ist die MNC bereitstellt. Obwohl es möglich, Ziel-Gebietsschemas, verwenden den Ländercode für Mobilgerät ist, ist der empfohlene Ansatz, den unten angegebenen sprachqualifizierer verwenden. Um beispielsweise Zielressourcen in Deutschland, wäre der Qualifizierer `mcc262`. Um die Zielressourcen für T-Mobile-Geräte in den USA der Qualifizierer ist `mcc310-mnc026`.
  Eine vollständige Liste der mobilen Ländercodes und mobiles Netzwerk-Fehlercodes finden Sie unter <http://mcc-mnc.com/>.

- **Sprache** &ndash; kleingeschriebener zweibuchstabiger [Code nach ISO 639-1-Sprache](http://en.wikipedia.org/wiki/ISO_639-1) und optional gefolgt von den zwei Buchstaben bestehenden [ISO-3166-Alpha-2-Regionscode](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Wenn beide Qualifizierer werden bereitgestellt, sie werden getrennt von einem `-r`. Z. B. zum Ziel französischsprachige Gebietsschemas wird der Qualifizierer der `fr` verwendet wird. Kanadischen Französisch Gebietsschemas als Ziel der `fr-rCA` eingesetzt werden. Eine vollständige Liste von Sprachcodes und Landes-/Regionscodes, finden Sie unter [Codes für die Darstellung der Namen von Sprachen](http://www.loc.gov/standards/iso639-2/php/English_list.php) und [Ländernamen und Codeelemente](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Kleinste Stärke** &ndash; gibt die kleinste Bildschirmbreite, die die Anwendung zum Ausführen dient auf. Ausführlicher behandelt [Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  In API-Ebene 13 (Android 3.2) und höher verfügbar. Zum Beispiel der Qualifizierer `sw320dp` wird verwendet, um Geräte, deren Höhe und Breite ist mindestens, 320dp.

- **Verfügbare Breite** &ndash; die minimale Breite des Bildschirms in der Format-w*N*dp, wobei *N* ist die Breite Dichte in geräteunabhängigen Pixeln.
  Dieser Wert kann sich ändern, wie der Benutzer das Gerät dreht. Ausführlicher behandelt [Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  In in-API-Ebene 13 (Android 3.2) und höher verfügbar. Beispiel: die Qualifizierer w720dp dient, Geräte zu verwenden, die eine Breite von mindestens 720dp aufweisen.

- **Verfügbare Höhe** &ndash; die minimale Höhe des Bildschirms in der Format-h*N*dp, wobei *N* ist die Höhe in dp. Dieser Wert kann sich ändern, wie der Benutzer das Gerät dreht. Ausführlicher behandelt [Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  In in-API-Ebene 13 (Android 3.2) und höher verfügbar. Beispielsweise wird der Qualifizierer h720dp verwendet, um Geräte zu verwenden, die eine von mindestens 720dp Höhe

- **Bildschirmgröße** &ndash; dieser Qualifizierer ist eine Generalisierung der Größe des Bildschirms, die für diese Ressourcen sind. Es wird ausführlicher behandelt [Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Mögliche Werte sind `small`, `normal`, `large` und `xlarge`. In API-Ebene 9 (Android 2.3 oder Android 2.3.1/Android 2.3.2) hinzugefügt

- **Bildschirm Aspekt** &ndash; basiert auf das Seitenverhältnis beibehalten, nicht die bildschirmausrichtung. Es ist ein langer Bildschirm breiter. In API-Ebene-4 (Android 1.6) hinzugefügt. Mögliche Werte sind lang und Notlong.

- **Bildschirm Ausrichtung** &ndash; hoch- oder Querformat bildschirmausrichtung. Dies kann während der Lebensdauer einer Anwendung ändern.
  Mögliche Werte sind `port` und `land`.

- **Andocken Modus** &ndash; für Geräte in einem Auto andocken oder einen Helpdesk andocken. In API-Ebene 8 (Android 2.2.x) hinzugefügt. Mögliche Werte sind `car` und `desk`.

- **Nachtmodus** &ndash; angibt, ob die Anwendung bei Nacht oder am Tag ausgeführt wird. Dies kann sich während der Lebensdauer einer Anwendung ändern und um Entwicklern die Möglichkeit, je dunkler Versionen einer Schnittstelle in der Nacht verwenden soll. In API-Ebene 8 (Android 2.2.x) hinzugefügt. Mögliche Werte sind `night` und `notnight`.

- **Bildschirm Pixeldichte (dpi)** &ndash; die Anzahl der Pixel in einem bestimmten Gebiet auf dem physischen Bildschirm. In der Regel als Punkte pro Zoll (dpi) ausgedrückt. Dabei sind folgende Werte möglich:

    - `ldpi` &ndash; Geringe Dichte Bildschirme.

    - `mdpi` &ndash; Mittlere Dichte Bildschirme

    - `hdpi` &ndash; Bildschirme mit hoher Dichte

    - `xhdpi` &ndash; Bildschirme mit sehr hoher Dichte

    - `nodpi` &ndash; Ressourcen, die nicht skaliert werden

    - `tvdpi` &ndash; In API-Ebene 13 (Android 3.2) für Bildschirme zwischen Mdpi und Hdpi eingeführt.

- **Typ der Touchscreen** &ndash; gibt den Typ der Touchscreen, die ein Gerät haben kann. Mögliche Werte sind `notouch` (ohne Touchscreen), `stylus` (ein magnetisierungsresistent Touchscreen für einen Tablettstift geeignet ist), und `finger` (ein Touchscreen).

- **Verfügbarkeit der Tastatur** &ndash; gibt an, welche Art von Tastatur verfügbar ist. Dies kann während der Lebensdauer einer Anwendung ändern &ndash; z. B. wenn ein Benutzer öffnet eine Hardwaretastatur. Dabei sind folgende Werte möglich:

    - `keysexposed` &ndash; Das Gerät verfügt über eine Tastatur zur Verfügung. Liegt keine Software-Tastatur aktiviert, wird dies nur verwendet, wenn die Hardwaretastatur geöffnet wird.

    - `keyshidden` &ndash; Das Gerät verfügt über eine Hardwaretastatur, aber es ausgeblendet ist und keine Tastatur Software aktiviert ist.

    - `keyssoft` &ndash; das Gerät besitzt, eine Software-Tastatur aktiviert wird.

- **Primäre Texteingabemethode** &ndash; verwenden, um anzugeben, welche Arten von Hardwareschlüssel für die Eingabe zur Verfügung stehen. Dabei sind folgende Werte möglich:

    - `nokeys` &ndash; Es gibt keine Hardwareschlüssel für die Eingabe.

    - `qwerty` &ndash; Es ist eine qwerty-Tastatur verfügbar.

    - `12key` &ndash; Es ist eine mit 12 Tasten-Hardwaretastatur


- **Navigation Schlüssel Verfügbarkeit** &ndash; bei 5-Wege- oder Steuerkreuz (direktionale-Pad) Navigation verfügbar ist. Dies kann während der Lebensdauer der Anwendung ändern. Dabei sind folgende Werte möglich:

    - `navexposed` &ndash; die Standardnavigations-Schlüssel sind für den Benutzer verfügbar

    - `navhidden` &ndash; die Navigationstasten sind nicht verfügbar.

-  **Nicht-Touch-Navigation Hauptmethode** &ndash; die Art der Navigation auf dem Gerät verfügbar. Dabei sind folgende Werte möglich:

    - `nonav` &ndash; die einzige Navigationsfunktion verfügbar ist, den Touchscreen

    - `dpad` &ndash; ein Steuerkreuz (direktionale-Pad) steht für die navigation

    - `trackball` &ndash; das Gerät verfügt über einen Trackball für die navigation

    - `wheel` &ndash; die ungewöhnliches Szenario, in denen mindestens ein oder mehrere unidirektionale Rädern verfügbar

-  **Plattformversion (API-Ebene)** &ndash; die API-Ebene, die vom Gerät im Format V unterstützten*N*, wobei *N* ist die API-Ebene, die als Ziel festgelegt ist. Z. B. v11 wird ein API-Zielebene 11 (Android 3.0) Geräte.


Weitere Informationen zur Ressource Qualifizierer finden Sie unter [Bereitstellen von Ressourcen](http://developer.android.com/guide/topics/resources/providing-resources.html) auf der Android-Entwickler-Website.


## <a name="how-android-determines-what-resources-to-use"></a>Wie Android bestimmt, auf welche Ressourcen zum Verwenden

Es ist sehr möglich, sondern sogar wahrscheinlich, dass eine Android-Anwendung viele Ressourcen enthält. Es ist wichtig zu verstehen, wie Android auswählen die Ressourcen für eine Anwendung bei der Ausführung auf einem Gerät.

Android bestimmt die grundlegenden Ressourcen durch Iteration über den folgenden Test von Regeln:

- **Beseitigen widersprüchlich Qualifizierer** &ndash; z. B. wenn die Ausrichtung auf Hochformat ist, klicken Sie dann alle Querformat Ressourcenverzeichnisse zurückgewiesen.

- **Ignorieren Sie die Qualifizierer, die nicht unterstützt** &ndash; nicht alle Qualifizierer sind für alle API-Ebenen verfügbar. Wenn Sie ein Ressourcenverzeichnis einen Qualifizierer enthält, der nicht vom Gerät unterstützt wird, werden diese Ressourcenverzeichnis ignoriert.

- **Identifizieren Sie den nächsten höchste Priorität Qualifizierer** &ndash; verweisen auf der obigen Tabelle wählen Sie den nächsten höchste Priorität-Qualifizierer (von oben nach unten).

- **Behalten Sie alle Ressourcenverzeichnisse für den Qualifizierer für** &ndash; treten Ressourcenverzeichnisse, die den Qualifizierer, der in der obigen Tabelle entsprechen, wählen Sie den nächsten höchste Priorität-Qualifizierer (von oben nach unten).

Diese Regeln sind auch im folgenden Flussdiagramm dargestellt:

[![Ressourcen-Flussdiagramm](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Wenn das System nach Dichte-spezifischen Ressourcen sucht und nicht werden gefunden, wird versucht, suchen Sie nach anderen bestimmten Dichte-Ressourcen und skalieren. Android sind nicht unbedingt die Standardressourcen verwenden.
Z. B. bei der Suche nach einer Ressource mit geringer Dichte nicht verfügbar ist, kann Android mit hoher Dichte Version der Ressource für die Standardinstanz oder mittlere Dichte Ressourcen auswählen. Es liegt daran, dass die Ressource mit hoher Dichte um den Faktor 0,5 skaliert werden kann nach unten bei weniger Sichtbarkeit als eine mittlere Dichte-Ressource, die einen Faktor von 0,75 müssten Herunterskalieren führen.

Betrachten Sie beispielsweise eine Anwendung mit den folgenden drawable Ressourcenverzeichnisse aus:

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Und nun die Anwendung ausgeführt wird, auf einem Gerät mit der folgenden Konfiguration:

- **Gebietsschema** &ndash; En-GB-
- **Ausrichtung** &ndash; Port
- **Bildschirm Dichte** &ndash; Hdpi
- **Typ der Touchscreen** &ndash; Notouch
- **Primäre Eingabemethode** &ndash; 12key

Zunächst werden die französischen Ressourcen entfernt, wie sie mit dem Gebietsschema des Konflikt `en-GB`, lassen uns:

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Als Nächstes der ersten Qualifizierer ausgewählt ist, aus der obigen Tabelle Qualifizierer: MCC und MNC. Es gibt keine Ressourcenverzeichnisse aus, die dieser Qualifizierer enthalten, sodass der Code MCC/MNC ignoriert wird.

Der nächste Qualifizierer ist ausgewählt, die Sprache ist. Es sind Ressourcen, die den Sprachcode entsprechen. Alle Ressourcenverzeichnisse, die nicht den Sprachcode des entsprechen `en` abgelehnt werden, damit, dass die Liste der Ressourcen ist:

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Der nächste Qualifizierer, die vorhanden ist wird für die Ausrichtung des Bildschirms, sodass alle Ressourcenverzeichnisse, die nicht die bildschirmausrichtung des entsprechen `port` werden entfernt:

    drawable-en-port
    drawable-en-port-ldpi

Als Nächstes wird der Qualifizierer für den Bildschirm Dichte `ldpi`, was dazu führt, in der Ausschluss eines in das Verzeichnis für weitere:

    drawable-en-port-ldpi

Durch diesen Prozess wird Android nutzt die zeichenbaren Ressourcen im Ressourcenverzeichnis `drawable-en-port-ldpi` für das Gerät.

> [!NOTE]
> Der Bildschirm Größe Qualifizierer Geben Sie eine Ausnahme für diesen Auswahlprozess. Es ist möglich, für Android, Ressourcen auswählen, die für kleinere Bildschirme als welche das aktuelle Gerät stellt entwickelt wurden. Z. B. einem großen Bildschirm Gerät können Sie die Ressourcen für einen normal großen Bildschirm angeben. Jedoch das Gegenteil davon nicht erfüllt ist: das gleiche Gerät mit großen Bildschirmen verwendet nicht die Ressourcen bereitgestellt, die für einen Bildschirm sehr groß. Wenn Android einen Ressourcensatz, der eine bestimmte Bildschirmgröße entspricht nicht finden kann, stürzt die Anwendung.
