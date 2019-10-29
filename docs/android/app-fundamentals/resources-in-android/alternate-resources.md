---
title: Alternative Ressourcen
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 644262310614874794810fd083ba1823abfd0da2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025413"
---
# <a name="alternate-resources"></a>Alternative Ressourcen

Alternative Ressourcen sind Ressourcen, die auf ein bestimmtes Gerät oder eine Laufzeitkonfiguration abzielen, z. b. die aktuelle Sprache, bestimmte Bildschirmgröße oder Pixeldichte. Wenn Android eine Ressource finden kann, die für ein bestimmtes Gerät oder eine bestimmte Konfiguration spezifischer ist als die Standardressource, wird stattdessen diese Ressource verwendet. Wenn keine Alternative Ressource gefunden wird, die mit der aktuellen Konfiguration übereinstimmt, werden die Standard Ressourcen geladen. Wie Android entscheidet, welche Ressourcen von einer Anwendung verwendet werden, wird im Abschnitt Ressourcen Speicherort ausführlicher behandelt.

Alternative Ressourcen sind entsprechend dem Ressourcentyp, wie Standard Ressourcen, als Unterverzeichnis im Ressourcen Ordner angeordnet. Der Name des alternativen Unterverzeichnisses der Ressource weist das folgende Format auf: _ResourceType_-_Qualifizierer_

Der *Qualifizierer* ist ein Name, der eine bestimmte Gerätekonfiguration identifiziert.
Es können mehrere Qualifizierer in einem Namen vorhanden sein, die jeweils durch einen Bindestrich getrennt sind. Der folgende Screenshot zeigt z. b. ein einfaches Projekt mit alternativen Ressourcen für verschiedene Konfigurationen, z. b. Gebiets Schema, Bildschirm Dichte, Bildschirmgröße und Ausrichtung:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Alternative Ressourcen](alternate-resources-images/alternate-resources-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Alternative Ressourcen](alternate-resources-images/alternate-resources-xs.png)

-----

Die folgenden Regeln gelten für das Hinzufügen von Qualifizierern zu einem Ressourcentyp:

1. Es können mehrere Qualifizierer vorhanden sein, wobei jeder Qualifizierer durch einen Bindestrich getrennt ist.

2. Die Qualifizierer sind möglicherweise nur einmal angegeben.

3. Qualifizierer müssen in der Reihenfolge angegeben werden, in der Sie in der nachfolgenden Tabelle

Die möglichen Qualifizierer sind unten aufgeführt:

- **MCC und MNC** &ndash; das [Mobile Country Code](https://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) und optional den [mobilen Netzwerkcode](https://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). Der MCC wird von der SIM-Karte bereitgestellt, während das Netzwerk, mit dem das Gerät verbunden ist, das MNC bereitstellt. Obwohl es möglich ist, Gebiets Schemas mithilfe des Codes für das Mobile Land zu verwenden, empfiehlt es sich, den unten angegebenen sprach Qualifizierer zu verwenden. Um z. b. auf Ressourcen in Deutschland zu abzielen, wäre der Qualifizierer `mcc262`. Um Ressourcen für T-Mobile in den USA als Ziel zu haben, wird der Qualifizierer `mcc310-mnc026`.
  Eine umfassende Liste der mobilen Ländercodes und mobilen Netzwerk Codes finden Sie unter <http://mcc-mnc.com/>.

- **Sprach** &ndash; den aus zwei Buchstaben bestehenden [ISO 639-1-Sprachcode](https://en.wikipedia.org/wiki/ISO_639-1) und optional den aus zwei Buchstaben bestehenden Code für die [ISO-3166-Alpha-2-Region](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Wenn beide Qualifizierer angegeben werden, werden Sie durch eine `-r`getrennt. Wenn Sie z. b. auf Französisch sprechende Gebiets Schemas abzielen, wird der Qualifizierer von `fr` verwendet. Zum Ausrichten von französisch-kanadischen Gebiets Schemas wird der `fr-rCA` verwendet. Eine umfassende Liste der Sprachen Codes und Regions Codes finden Sie unter [Codes für die Darstellung von Namen von Sprachen](https://www.loc.gov/standards/iso639-2/php/English_list.php) und [Ländernamen und Code Elementen](https://www.iso.org/iso-3166-country-codes.html).

- **Kleinste Breite** &ndash; gibt die kleinste Bildschirmbreite an, auf der die Anwendung ausgeführt werden soll. Ausführliche Informationen [zum Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar auf API-Ebene 13 (Android 3,2) und höher. Beispielsweise wird der Qualifizierer `sw320dp` zum Ziel von Geräten verwendet, deren Höhe und Breite mindestens 320dp ist.

- **Verfügbare Breite** &ndash; die Mindestbreite des Bildschirms im Format w*N*DP, wobei *N* die Breite in Dichte unabhängigen Pixeln ist.
  Dieser Wert kann sich ändern, wenn der Benutzer das Gerät dreht. Ausführliche Informationen [zum Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar auf API-Ebene 13 (Android 3,2) und höher. Beispiel: der Qualifizierer w720dp wird für Geräte mit einer Breite von mindestens 720DP verwendet.

- **Verfügbare Höhe** &ndash; die Mindesthöhe des Bildschirms im Format h*N*DP, wobei *N* die Höhe in DP ist. Dieser Wert kann sich ändern, wenn der Benutzer das Gerät dreht. Ausführliche Informationen [zum Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar auf API-Ebene 13 (Android 3,2) und höher. Beispielsweise wird der Qualifizierer h720dp für Geräte mit einer Höhe von mindestens 720DP verwendet.

- **Bildschirmgröße** &ndash; dieser Qualifizierer ist eine Generalisierung der Bildschirmgröße, für die diese Ressourcen gelten. Weitere Informationen finden Sie unter [Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Mögliche Werte sind `small`, `normal`, `large` und `xlarge`. In API-Ebene 9 hinzugefügt (Android 2.3/Android 2.3.1/Android 2.3.2)

- **Bildschirm Aspekt** &ndash; Dies basiert auf dem Seitenverhältnis, nicht auf der Bildschirm Ausrichtung. Ein langer Bildschirm ist breiter. In API-Ebene 4 (Android 1,6) hinzugefügt. Mögliche Werte sind Long und notlong.

- **Bildschirm Ausrichtung** &ndash; Bildschirm Ausrichtung für Hochformat oder Querformat. Dies kann sich während der Lebensdauer einer Anwendung ändern.
  Mögliche Werte sind `port` und `land`.

- **Andock Modus** &ndash; für Geräte in einem Fahrzeug-oder Desk-Dock. In API-Ebene 8 (Android 2.2. x) hinzugefügt. Mögliche Werte sind `car` und `desk`.

- Der **Nachtmodus** &ndash;, ob die Anwendung nachts oder täglich ausgeführt wird. Dies kann sich während der Lebensdauer einer Anwendung ändern und soll Entwicklern die Möglichkeit geben, die dunkleren Versionen einer Schnittstelle zu Nacht zu verwenden. In API-Ebene 8 (Android 2.2. x) hinzugefügt. Mögliche Werte sind `night` und `notnight`.

- Die **Bildschirm Pixeldichte (dpi)** &ndash; die Anzahl der Pixel in einem bestimmten Bereich auf dem physischen Bildschirm. Wird normalerweise als dpi (dpi) ausgedrückt. Dabei sind folgende Werte möglich:

  - `ldpi` &ndash; Bildschirme mit geringer Dichte.

  - Bildschirme für die `mdpi` &ndash; mittlere Dichte

  - `hdpi` &ndash; Bildschirme mit hoher Dichte

  - `xhdpi` &ndash; Bildschirme mit hoher Dichte

  - `nodpi` &ndash; Ressourcen, die nicht skaliert werden sollen.

  - `tvdpi` &ndash;, die auf API-Ebene 13 (Android 3,2) für Bildschirme zwischen MDPI und hdpi eingeführt wurden.

- **Touchscreen Type** &ndash; gibt den Typ des Touchscreen an, den ein Gerät haben kann. Mögliche Werte sind `notouch` (kein Touchscreen), `stylus` (ein für einen Tablettstift geeigneter resistietouchscreen) und `finger` (ein Touchscreen).

- **Tastatur Verfügbarkeit** &ndash; gibt an, welche Art von Tastatur verfügbar ist. Dies kann sich während der Lebensdauer einer Anwendung ändern &ndash; z. b. Wenn ein Benutzer eine Hardware Tastatur öffnet. Dabei sind folgende Werte möglich:

  - `keysexposed` &ndash; auf dem Gerät eine Tastatur verfügbar ist. Wenn keine Software Tastatur aktiviert ist, wird diese nur beim Öffnen der Hardware Tastatur verwendet.

  - `keyshidden` &ndash; das Gerät über eine Hardware Tastatur verfügt, aber ausgeblendet ist und keine Software Tastatur aktiviert ist.

  - `keyssoft` &ndash; auf dem Gerät eine Software Tastatur aktiviert ist.

- **Primary Text Input-Methode** &ndash; mit der angegeben wird, welche Arten von Hardware Schlüsseln für die Eingabe verfügbar sind. Dabei sind folgende Werte möglich:

  - `nokeys` &ndash; für die Eingabe keine Hardwareschlüssel vorhanden sind.

  - `qwerty` &ndash; es ist eine QWERTY-Tastatur verfügbar.

  - `12key` &ndash; eine Hardware Tastatur mit 12 Schlüsseln vorhanden ist.

- **Navigationsschlüssel-Verfügbarkeits** &ndash; für den Fall, dass die Navigation mit dem 5-Wege-oder d-Pad (direktionaler Pad) verfügbar Dies kann sich während der Lebensdauer der Anwendung ändern. Dabei sind folgende Werte möglich:

  - `navexposed` &ndash; die Navigationsschlüssel für den Benutzer verfügbar sind.

  - `navhidden` &ndash; die Navigationsschlüssel nicht verfügbar sind.

- **Primäre Non-Touchscreen-Navigations Methode** &ndash; die Art der auf dem Gerät verfügbaren Navigation. Dabei sind folgende Werte möglich:

  - `nonav` &ndash; die einzige verfügbare Navigationsfunktion der Touchscreen ist.

  - `dpad` &ndash; ein d-Pad (direktionaler Pad) für die Navigation verfügbar ist.

  - `trackball` &ndash; das Gerät über einen trackballunterstützung für die Navigation verfügt.

  - `wheel` &ndash; des ungewöhnlichen Szenarios, in dem ein oder mehrere direktionale Räder verfügbar sind

- **Platt Form Version (API-Ebene)** &ndash; die API-Ebene, die vom Gerät im Format v*N*unterstützt wird, wobei *N* für die API-Ebene steht, die Ziel ist. Beispielsweise wird für v11 ein Gerät auf API Level 11 (Android 3,0) als Ziel verwendet.

Ausführlichere Informationen zu Ressourcen Qualifizierern finden Sie unter [Bereitstellen von Ressourcen](https://developer.android.com/guide/topics/resources/providing-resources.html) auf der Android-Entwickler Website.

## <a name="how-android-determines-what-resources-to-use"></a>Bestimmen der zu verwendenden Ressourcen durch Android

Es ist sehr möglich und wahrscheinlich, dass eine Android-Anwendung viele Ressourcen enthält. Es ist wichtig zu verstehen, wie Android die Ressourcen für eine Anwendung auswählt, wenn diese auf einem Gerät ausgeführt wird.

Android bestimmt die Ressourcenbasis durch Iteration des folgenden Regel Tests:

- **Vermeiden Sie widersprüchliche Qualifizierer** &ndash; z. b. wenn die Geräte Ausrichtung Hochformat ist, werden alle quer Ressourcen Verzeichnisse zurückgewiesen.

- **Qualifizierer ignorieren werden nicht unterstützt** &ndash; nicht alle Qualifizierer sind für alle API-Ebenen verfügbar. Wenn ein Ressourcenverzeichnis einen Qualifizierer enthält, der vom Gerät nicht unterstützt wird, wird dieses Ressourcenverzeichnis ignoriert.

- **Identifizieren Sie den Qualifizierer mit der höchsten Priorität** &ndash; der sich auf die obige Tabelle bezieht, wählen Sie den Qualifizierer mit der höchsten Priorität aus

- **Behalten Sie alle Ressourcen Verzeichnisse für Qualifizierer** &ndash; wenn Ressourcen Verzeichnisse vorhanden sind, die mit dem Qualifizierer der obigen Tabelle identisch sind, wählen Sie den Qualifizierer mit der höchsten Priorität (von oben nach unten).

Diese Regeln werden auch im folgenden Flussdiagramm veranschaulicht:

[Flussdiagramm für![Ressourcen](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Wenn das System nach Dichte spezifischen Ressourcen sucht und diese nicht finden kann, wird versucht, andere Dichte spezifische Ressourcen zu finden und zu skalieren. Android verwendet möglicherweise nicht notwendigerweise die Standard Ressourcen.
Wenn Sie z. b. nach einer Ressource mit geringer Dichte suchen und diese nicht verfügbar ist, kann Android die hochdichte Version der Ressource über die Standard-oder mitteldichte Ressourcen auswählen. Dies liegt daran, dass die Ressource mit hoher Dichte um den Faktor 0,5 herunterskaliert werden kann. Dies führt zu weniger Sichtbarkeits Problemen als das Herunterskalieren einer Ressource mit mittlerer Dichte, die einen Faktor von 0,75 erfordern würde.

Stellen Sie sich als Beispiel eine Anwendung vor, die über die folgenden drawable-Ressourcen Verzeichnisse verfügt:

```
drawable
drawable-en
drawable-fr-rCA
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
drawable-port-ldpi
drawable-port-notouch-12key
```

Nun wird die Anwendung auf einem Gerät mit der folgenden Konfiguration ausgeführt:

- Gebiets **Schema &ndash; en** -GB
- **Ausrichtung** &ndash; Port
- **Bildschirm Dichte** &ndash; hdpi
- **Touchscreen-Typ** &ndash; NoTouch
- **Primäre Eingabemethode** &ndash; 12key

Zu Beginn werden die französischen Ressourcen eliminiert, da Sie mit dem Gebiets Schema von `en-GB`in Konflikt stehen, sodass wir Folgendes unterliegen:

```
drawable
drawable-en
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
drawable-port-ldpi
drawable-port-notouch-12key
```

Als nächstes wird der erste Qualifizierer aus der qualifizierertabelle oben ausgewählt: MCC und MNC. Es sind keine Ressourcen Verzeichnisse vorhanden, die diesen Qualifizierer enthalten, sodass der MCC/MNC-Code ignoriert wird.

Der nächste Qualifizierer ist ausgewählt (Sprache). Es gibt Ressourcen, die dem Sprachcode entsprechen. Alle Ressourcen Verzeichnisse, die nicht mit dem Sprachcode von `en` Stimmen, werden abgelehnt, sodass die Liste der Ressourcen jetzt lautet:

```
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
```

Der nächste Qualifizierer, der vorhanden ist, ist für die Bildschirm Ausrichtung vorgesehen, sodass alle Ressourcen Verzeichnisse, die nicht der Bildschirm Ausrichtung `port` entsprechen, entfernt werden:

```
drawable-en-port
drawable-en-port-ldpi
```

Als nächstes ist der Qualifizierer für die Bildschirm Dichte, `ldpi`, der zum Ausschluss eines weiteren Ressourcen Verzeichnisses führt:

```
drawable-en-port-ldpi
```

Im Ergebnis dieses Prozesses verwendet Android die drawable-Ressourcen im Ressourcenverzeichnis `drawable-en-port-ldpi` für das Gerät.

> [!NOTE]
> Die Qualifizierer für die Bildschirmgröße stellen eine Ausnahme für diesen Auswahlprozess dar. Android kann Ressourcen auswählen, die für einen kleineren Bildschirm entwickelt wurden, als das aktuelle Gerät bereitstellt. Beispielsweise kann ein großes Bildschirm die Ressourcen verwenden, die für einen Bildschirm mit normaler Größe bereitgestellt werden. Dies trifft jedoch nicht zu: das gleiche Gerät für große Bildschirme verwendet nicht die Ressourcen, die für einen XLarge-Bildschirm bereitgestellt werden. Wenn Android keinen Ressourcen Satz finden kann, der mit einer bestimmten Bildschirmgröße übereinstimmt, stürzt die Anwendung ab.
