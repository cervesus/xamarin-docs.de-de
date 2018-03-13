---
title: Alternative Ressourcen
ms.topic: article
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7ebbf2a9215c8472ae2f286728cb2f819e8331cb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="alternate-resources"></a>Alternative Ressourcen

Alternative Ressourcen sind, die auf ein bestimmtes Gerät oder eine Laufzeitkonfiguration wie die aktuelle Sprache, die bestimmte Bildschirmgröße und die Pixeldichte abzielen. Wenn Android eine Ressource, die spezifisch für ein bestimmtes Gerät oder die Konfiguration als die Standardressource ist übereinstimmen kann, wird stattdessen diese Ressource verwendet werden. Wenn es keine alternative Ressource, die die aktuelle Konfiguration entspricht findet, werden die Standardressourcen geladen werden. Wie Android entscheidet, wird welche Ressourcen von einer Anwendung verwendet werden ausführlicher weiter unten im Abschnitt Ressourcenspeicherort abgedeckt werden

Alternative Ressourcen sind als ein Unterverzeichnis im Ordner Ressourcen entsprechend den Ressourcentyp, genau wie Standardressourcen organisiert. Der Name des Unterverzeichnisses alternativen Ressource wird in der Form: _ResourceType_-_Qualifizierer_

*Qualifizierer* ist ein Name, der eine bestimmte Gerätekonfiguration identifiziert.
Es gibt möglicherweise mehrere Qualifizierer in einen Namen ein, jeweils getrennt durch einen Bindestrich. Der folgende Screenshot zeigt z. B. ein einfaches Projekt, das andere Ressourcen für verschiedene Konfigurationen wie z. B. das Gebietsschema, Bildschirm Dichte Bildschirmgröße und Ausrichtung aufweist:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternative Ressourcen](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Alternative Ressourcen](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Die folgenden Regeln gelten, wenn Sie einen Ressourcentyp Qualifizierer hinzufügen:

1. Es gibt möglicherweise mehr als ein Qualifizierer, die mit jeder Qualifizierer, die durch einen Bindestrich getrennt.

2. Der Qualifizierer ist möglicherweise nur einmal angegeben werden.

3. Qualifizierer muss in der Reihenfolge, die sie in der folgenden Tabelle angezeigt werden.

Die möglichen Qualifizierer sind zu Referenzzwecken im folgenden aufgeführt:

- **MCC und MNC** &ndash; der [mobile Ländercode](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) und optional die [Mobilfunknetz Code](http://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). Die SIM-Karte gebe den MCC, während das Netzwerk, mit dem das Gerät verbunden ist die MNC bereitstellt. Obwohl es möglich, Ziel Gebietsschemas, verwenden die mobile Landeskennzahl ist, ist die empfohlene Vorgehensweise verwenden Sie den unten angegebenen sprachqualifizierer. Um beispielsweise Zielressourcen in Deutschland, wäre der Qualifizierer `mcc262`. Um Zielressourcen für T-Mobile-Geräte in den USA der Qualifizierer ist `mcc310-mnc026`.
  Eine vollständige Liste der mobilen Landeskennzahlen und Mobilfunknetz Codes finden Sie unter <http://mcclist.com/>.

- **Sprache** &ndash; die zweibuchstabige [Code nach ISO 639-1-Sprache](http://en.wikipedia.org/wiki/ISO_639-1) und optional gefolgt von der zweibuchstabige [ISO 3166 Alpha 2 Landes-/Regionscode](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Wenn beide Qualifizierer angegeben werden, sind diese getrennt durch ein `-r`. Um z. B. auf Ziel französischsprachige Gebietsschemas wird der Qualifizierer der `fr` verwendet wird. Kanadischen Französisch Gebietsschemas als Ziel der `fr-rCA` verwendet werden. Eine vollständige Liste von Sprachcodes und Landes-/ Regionscodes, finden Sie unter [Codes für die Darstellung der Namen von Sprachen](http://www.loc.gov/standards/iso639-2/php/English_list.php) und [Ländernamen und Codeelemente](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Kleinste Stärke** &ndash; gibt die kleinste Bildschirmbreite die Anwendung auf ausgeführt werden sollen. Ausführlich in [erstellen Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar in API-Ebene 13 (Android 3.2) und höher. Z. B. der Qualifizierer `sw320dp` wird verwendet, um die Zielgeräte, dessen Höhe und Breite ist mindestens, 320dp.

- **Verfügbare Breite** &ndash; die minimale Breite des Bildschirms in der Format-w*N*dp, wobei *N* ist die Breite in Dichte geräteunabhängigen Pixeln.
  Dieser Wert ändert sich möglicherweise, wie der Benutzer das Gerät dreht. Ausführlich in [erstellen Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar in in API-Ebene 13 (Android 3.2) und höher. Beispiel: die Qualifizierer w720dp dient für Geräte, die eine Breite von mindestens 720dp aufweisen.

- **Verfügbaren Höhe** &ndash; die minimale Höhe des Bildschirms in der Format-h*N*dp, wobei *N* bleibt die Höhe in dp. Dieser Wert ändert sich möglicherweise, wie der Benutzer das Gerät dreht. Ausführlich in [erstellen Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar in in API-Ebene 13 (Android 3.2) und höher. Beispielsweise wird der Qualifizierer h720dp verwendet, um Geräte abzielen, die eine Höhe von mindestens 720dp haben

- **Displaygröße** &ndash; dieses Qualifizierers ist eine Generalisierung der Größe des Bildschirms, die diese Ressourcen sind. Es wird ausführlich in [erstellen Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Mögliche Werte sind `small`, `normal`, `large` und `xlarge`. In der API-Ebene 9 (Android 2.3/Android 2.3.1/Android 2.3.2) hinzugefügt

- **Bildschirm Aspekt** &ndash; dieser Vorgang basiert auf das Seitenverhältnis, nicht die Ausrichtung des Bildschirms. Ein lange Bildschirm ist breiter. In der API-Ebene 4 (Android 1.6) hinzugefügt. Mögliche Werte sind lang und Notlong.

- **Bildschirm Ausrichtung** &ndash; hoch- oder Querformat bildschirmausrichtung. Dies kann während der Lebensdauer einer Anwendung ändern.
  Mögliche Werte sind `port` und `land`.

- **Andocken Modus** &ndash; für Geräte im Auto andocken oder Andocken eines Schalters. In der API-Ebene 8 (Android 2.2.x) hinzugefügt. Mögliche Werte sind `car` und `desk`.

- **Nacht Modus** &ndash; , ob die Anwendung in der Nacht oder des Tages ausgeführt wird. Dies kann sich während der Lebensdauer einer Anwendung ändern, um Produktfeedback um Entwicklern ermöglichen, je dunkler Versionen einer Schnittstelle in der Nacht zu verwenden. In der API-Ebene 8 (Android 2.2.x) hinzugefügt. Mögliche Werte sind `night` und `notnight`.

- **Bildschirm Pixeldichte (dpi)** &ndash; die Anzahl der Pixel in einem angegebenen Bereich auf dem physischen Bildschirm. In der Regel als Punkte pro Zoll (dpi) ausgedrückt. Dabei sind folgende Werte möglich:

    - `ldpi` &ndash; Geringe Dichte Bildschirme.

    - `mdpi` &ndash; Mittlere Dichte Bildschirme

    - `hdpi` &ndash; Hohe Dichte Bildschirme

    - `xhdpi` &ndash; Sehr hohe Dichte Bildschirme

    - `nodpi` &ndash; Ressourcen, die nicht skaliert werden

    - `tvdpi` &ndash; In der API-Ebene 13 (Android 3.2) für Bildschirme zwischen Mdpi und Hdpi eingeführt.

- **Touchscreen Typ** &ndash; gibt den Typ der Touchscreen dar, die ein Gerät weist möglicherweise. Mögliche Werte sind `notouch` (kein Touchscreen) `stylus` (Energieversorgungssystems Touchscreen für einen Tablettstift geeignet), und `finger` (einem Touchscreen).

- **Tastatur Verfügbarkeit** &ndash; gibt an, welche Art von Tastatur verfügbar ist. Dies kann während der Lebensdauer einer Anwendung ändern &ndash; z. B. wenn ein Benutzer öffnet eine Hardwaretastatur. Dabei sind folgende Werte möglich:

    - `keysexposed` &ndash; Das Gerät hat eine Tastatur zur Verfügung. Ist keine Software-Tastatur aktiviert, ist dies nur verwendet, wenn die Hardwaretastatur geöffnet wird.

    - `keyshidden` &ndash; Das Gerät muss auf einer Hardwaretastatur, aber es ausgeblendet ist und keine Software-Tastatur aktiviert wird.

    - `keyssoft` &ndash; das Gerät hat eine Software-Tastatur aktiviert.

- **Primäre Text Eingabemethode** &ndash; verwenden, um anzugeben, welche Arten von Hardwareschlüssel für die Eingabe verfügbar sind. Dabei sind folgende Werte möglich:

    - `nokeys` &ndash; Es gibt keine Hardwareschlüssel für die Eingabe.

    - `qwerty` &ndash; Es ist eine qwerty Tastatur verfügbar.

    - `12key` &ndash; Es ist eine 12-Schlüssel Hardwaretastatur


- **Navigation Schlüssel Verfügbarkeit** &ndash; wenn 5-Wege- oder Steuerkreuz (direktionale Auffüllzeichen) Navigation verfügbar ist. Dies kann während der Lebensdauer der Anwendung ändern. Dabei sind folgende Werte möglich:

    - `navexposed` &ndash; die Navigation Schlüssel sind für den Benutzer verfügbar

    - `navhidden` &ndash; die Navigationstasten sind nicht verfügbar.

-  **Nicht-Touch-Navigation Hauptmethode** &ndash; die Art der Navigation auf dem Gerät verfügbar. Dabei sind folgende Werte möglich:

    - `nonav` &ndash; nur Navigation werden verfügbar mit dem Touchscreen

    - `dpad` &ndash; Steuerkreuz (direktionale Auffüllzeichen) steht für die navigation

    - `trackball` &ndash; das Gerät hat einen Trackball für die navigation

    - `wheel` &ndash; die ungewöhnliches Szenario, in denen mindestens eine oder mehrere direktionale Rädern verfügbar

-  **Die Version der Plattform (API-Ebene)** &ndash; die API-Ebene, die vom Gerät im Format V unterstützten*N*, wobei *N* ist die API-Ebene, die vorgesehen ist. V11 wird beispielsweise eine API-Ebene 11 (Android 3.0) abzielen Gerät.


Weitere Informationen zu Ressourcen Qualifizierer finden Sie unter [Ressourcen bereitstellen](http://developer.android.com/guide/topics/resources/providing-resources.html) auf der Android-Entwickler-Website.


## <a name="how-android-determines-what-resources-to-use"></a>Wie Android bestimmt, auf welche Ressourcen zu verwendenden

Es ist möglich, und es wahrscheinlich, dass eine Android-Anwendung viele Ressourcen enthält. Es ist wichtig zu verstehen, wie Android auswählen die Ressourcen für eine Anwendung bei der Ausführung auf einem Gerät.

Android bestimmt die Ressourcen, die Basis, indem der folgende Test des Regeln durchlaufen:

- **Eliminieren widersprüchlich Qualifizierer** &ndash; z. B. wenn die Ausrichtung im Hochformat ist, klicken Sie dann alle Ressourcenverzeichnisse auf Querformat zurückgewiesen.

- **Ignorieren Sie die Qualifizierer, die nicht unterstützt** &ndash; sind nicht alle Qualifizierer für alle API-Ebenen verfügbar. Wenn ein Ressourcenverzeichnis einen Qualifizierer, der von dem Gerät nicht unterstützt wird, enthält und klicken Sie dann diese Ressourcenverzeichnis werden ignoriert.

- **Identifizieren Sie den nächsten höchste Priorität Qualifizierer** &ndash; verweisen auf der obigen Tabelle wählen Sie den nächsten höchsten Priorität-Qualifizierer (von oben nach unten).

- **Behalten Sie alle Ressourcenverzeichnisse für den Qualifizierer für** &ndash; treten Ressourcenverzeichnisse auf, die den Qualifizierer, der obigen Tabelle entsprechen, wählen Sie den weiter höchste Priorität-Qualifizierer (von oben nach unten).

Diese Regeln sind auch im folgenden Flussdiagramm dargestellt:

[![Ressourcen-Flussdiagramm](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Wenn das System nach Dichte-spezifischen Ressourcen sucht und nicht werden gefunden kann, wird versucht, andere Dichte bestimmte Ressourcen suchen und zu skalieren. Android sind nicht unbedingt die Standardressourcen verwenden.
Beispielsweise bei der Suche nach einer Ressource mit geringer Dichte nicht verfügbar ist, kann Android HD-Version der Ressource für die Standardinstanz oder eine mittlere Dichte Ressourcen auswählen. Es liegt daran, dass die HD-Ressource mit einem Faktor von 0,5 skaliert werden kann nach unten zu weniger Sichtbarkeit Probleme führt als Abwärtsskalierung aktuell eine mittlere Dichte-Ressource, um den Faktor 0,75 müsste.

Betrachten Sie beispielsweise eine Anwendung mit den folgenden zeichenbaren Ressourcenverzeichnisse aus:

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Und nun die Anwendung ausgeführt wird, auf einem Gerät mit der folgenden Konfiguration:

- **Gebietsschema** &ndash; En-GB
- **Ausrichtung** &ndash; Port
- **Bildschirm Dichte** &ndash; Hdpi
- **Touchscreen Typ** &ndash; Notouch
- **Primäre Eingabemethode** &ndash; 12key

Zunächst werden die französischen Ressourcen entfernt, wie sie mit dem Gebietsschema des in Konflikt `en-GB`, lassen uns mit:

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Als Nächstes das erste Qualifizierer Qualifizierer obiger Tabelle ausgewählt ist: MCC und MNC. Es gibt keine Ressourcenverzeichnisse auf, die diese Qualifizierer enthalten, sodass der Code MCC/MNC ignoriert wird.

Der nächste Qualifizierer ist ausgewählt, dies entspricht Sprache. Es sind Ressourcen, die den Sprachcode entsprechen. Alle Ressourcenverzeichnisse auf, die nicht den Sprachcode der entsprechen `en` werden zurückgewiesen, sodass nun die Liste der Ressourcen ist:

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Der nächste Qualifizierer, die vorhanden ist ist für die Ausrichtung des Bildschirms, d. h. alle Ressourcenverzeichnisse auf, die nicht von der bildschirmausrichtung entsprechen `port` werden entfernt:

    drawable-en-port
    drawable-en-port-ldpi

Als Nächstes wird der Qualifizierer für den Bildschirm Dichte `ldpi`, was dazu führt, in der Ausschlussliste einer weitere Ressourcenverzeichnis:

    drawable-en-port-ldpi

Als Ergebnis dieses Vorgangs wird Android zeichenbaren Ressourcen verwenden, in das Ressourcenverzeichnis `drawable-en-port-ldpi` für das Gerät.

> [!NOTE]
> Der Bildschirm Größe Qualifizierer bieten eine Ausnahme zu dieser Auswahlprozesses verwendet werden. Es ist möglich, für Android Ressourcen auswählen, die für einen kleineren Bildschirm als welche das aktuelle Gerät stellt vorgesehen sind. Z. B. einem großen Bildschirmgerät möglicherweise verwenden Sie die Ressourcen für einen normal großen Bildschirm bereitzustellen. Jedoch das Gegenteil dieses nicht erfüllt ist: das gleiche großen Bildschirmgerät wird nicht für einen Bildschirm Xlarge bereitgestellten Ressourcen verwenden. Android einen Ressourcensatz gefunden, der eine bestimmte Bildschirmgröße entspricht, stürzt die Anwendung.
