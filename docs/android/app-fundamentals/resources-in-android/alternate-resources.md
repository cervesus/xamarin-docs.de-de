---
title: Alternative Ressourcen
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 05c3816d0cc01beb3ed99994788b58e5f187171a
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70225779"
---
# <a name="alternate-resources"></a>Alternative Ressourcen

Alternative Ressourcen sind Ressourcen, die auf ein bestimmtes Gerät oder eine Laufzeitkonfiguration abzielen, z. b. die aktuelle Sprache, bestimmte Bildschirmgröße oder Pixeldichte. Wenn Android eine Ressource finden kann, die für ein bestimmtes Gerät oder eine bestimmte Konfiguration spezifischer ist als die Standardressource, wird stattdessen diese Ressource verwendet. Wenn keine Alternative Ressource gefunden wird, die mit der aktuellen Konfiguration übereinstimmt, werden die Standard Ressourcen geladen. Wie Android entscheidet, welche Ressourcen von einer Anwendung verwendet werden, wird im Abschnitt Ressourcen Speicherort ausführlicher behandelt.

Alternative Ressourcen sind entsprechend dem Ressourcentyp, wie Standard Ressourcen, als Unterverzeichnis im Ressourcen Ordner angeordnet. Der Name des alternativen Unterverzeichnisses der Ressource hat folgendes Format: _ResourceType_--Qualifizierer

Der Qualifizierer ist ein Name, der eine bestimmte Gerätekonfiguration identifiziert.
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

- **MCC und MNC** Der Mobile [Country Code](https://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) und optional der [Mobile Network Code](https://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). &ndash; Der MCC wird von der SIM-Karte bereitgestellt, während das Netzwerk, mit dem das Gerät verbunden ist, das MNC bereitstellt. Obwohl es möglich ist, Gebiets Schemas mithilfe des Codes für das Mobile Land zu verwenden, empfiehlt es sich, den unten angegebenen sprach Qualifizierer zu verwenden. Um z `mcc262`. b. auf Ressourcen in Deutschland zu abzielen, wäre der Qualifizierer. Der Qualifizierer ist `mcc310-mnc026`der Qualifizierer, um Ressourcen für T-Mobile in den USA zu Ziel.
  Eine umfassende Liste der mobilen Ländercodes und mobilen Netzwerk Codes finden <http://mcc-mnc.com/>Sie unter.

- **Sprache** Der aus zwei Buchstaben bestehende [ISO 639-1-Sprachcode](https://en.wikipedia.org/wiki/ISO_639-1) und optional der aus zwei Buchstaben bestehende [ISO-3166-Alpha-2-Regions Code.](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) &ndash; 
  Wenn beide Qualifizierer angegeben werden, werden Sie durch eine `-r`getrennt. Wenn Sie z. b. auf Französisch sprechende Gebiets Schemas `fr` abzielen, wird der Qualifizierer von verwendet. Zum Ziel von französisch-kanadischen `fr-rCA` Gebiets Schemas wird verwendet. Eine umfassende Liste der Sprachen Codes und Regions Codes finden Sie unter [Codes für die Darstellung von Namen von Sprachen](http://www.loc.gov/standards/iso639-2/php/English_list.php) und [Ländernamen und Code Elementen](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Kleinste Breite** &ndash; Gibt die kleinste Bildschirmbreite an, auf der die Anwendung ausgeführt werden soll. Ausführliche Informationen [zum Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar auf API-Ebene 13 (Android 3,2) und höher. Der Qualifizierer `sw320dp` wird z. b. für Geräte verwendet, deren Höhe und Breite mindestens 320dp ist.

- **Verfügbare Breite** Die minimale Breite des Bildschirms im Format w*N*DP, wobei N die Breite in Dichte unabhängigen Pixeln ist. &ndash;
  Dieser Wert kann sich ändern, wenn der Benutzer das Gerät dreht. Ausführliche Informationen [zum Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar auf API-Ebene 13 (Android 3,2) und höher. Beispiel: der Qualifizierer w720dp wird für Geräte mit einer Breite von mindestens 720DP verwendet.

- **Verfügbare Höhe** Die minimale Höhe des Bildschirms im Format h*N*DP, wobei N die Höhe in DP ist. &ndash; Dieser Wert kann sich ändern, wenn der Benutzer das Gerät dreht. Ausführliche Informationen [zum Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Verfügbar auf API-Ebene 13 (Android 3,2) und höher. Beispielsweise wird der Qualifizierer h720dp für Geräte mit einer Höhe von mindestens 720DP verwendet.

- **Bildschirmgröße** &ndash; Dieser Qualifizierer ist eine Generalisierung der Bildschirmgröße, für die diese Ressourcen gelten. Weitere Informationen finden Sie unter [Erstellen von Ressourcen für unterschiedliche Bildschirme](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Mögliche Werte sind `small`, `normal`, `large` und `xlarge`. In API-Ebene 9 hinzugefügt (Android 2.3/Android 2.3.1/Android 2.3.2)

- **Bildschirm Aspekt** &ndash; Dies basiert auf dem Seitenverhältnis, nicht auf der Bildschirm Ausrichtung. Ein langer Bildschirm ist breiter. In API-Ebene 4 (Android 1,6) hinzugefügt. Mögliche Werte sind Long und notlong.

- **Bildschirm Ausrichtung** &ndash; Bildschirm Ausrichtung für Hochformat oder Querformat. Dies kann sich während der Lebensdauer einer Anwendung ändern.
  Mögliche Werte sind `port` und `land`.

- **Andock Modus** &ndash; Für Geräte in einem Fahrzeug-oder Desk-Dock. In API-Ebene 8 (Android 2.2. x) hinzugefügt. Mögliche Werte sind `car` und `desk`.

- **Nachtmodus** &ndash; Gibt an, ob die Anwendung nachts oder täglich ausgeführt wird. Dies kann sich während der Lebensdauer einer Anwendung ändern und soll Entwicklern die Möglichkeit geben, die dunkleren Versionen einer Schnittstelle zu Nacht zu verwenden. In API-Ebene 8 (Android 2.2. x) hinzugefügt. Mögliche Werte sind `night` und `notnight`.

- **Bildschirm Pixel Dichte (dpi)** &ndash; Die Anzahl der Pixel in einem angegebenen Bereich auf dem physischen Bildschirm. Wird normalerweise als dpi (dpi) ausgedrückt. Dabei sind folgende Werte möglich:

  - `ldpi`&ndash; Bildschirme mit geringer Dichte.

  - `mdpi`&ndash; Bildschirme mit mittlerer Dichte

  - `hdpi`&ndash; Bildschirme mit hoher Dichte

  - `xhdpi`&ndash; Bildschirme mit hoher Dichte

  - `nodpi`&ndash; Ressourcen, die nicht skaliert werden sollen

  - `tvdpi`&ndash; Eingeführt in API-Ebene 13 (Android 3,2) für Bildschirme zwischen MDPI und hdpi.

- **Touchscreen-Typ** &ndash; Gibt den Typ des Touchscreen an, den ein Gerät haben kann. Mögliche Werte sind `notouch` (kein Touchscreen), `stylus` (ein für einen Tablettstift geeignetes Bildschirm für die resistischtaste) und `finger` (ein Touchscreen).

- **Tastatur Verfügbarkeit** &ndash; Gibt an, welche Art von Tastatur verfügbar ist. Dies kann sich während der Lebensdauer einer Anwendung &ndash; ändern, z. b. Wenn ein Benutzer eine Hardware Tastatur öffnet. Dabei sind folgende Werte möglich:

  - `keysexposed`&ndash; Auf dem Gerät ist eine Tastatur verfügbar. Wenn keine Software Tastatur aktiviert ist, wird diese nur beim Öffnen der Hardware Tastatur verwendet.

  - `keyshidden`&ndash; Das Gerät verfügt über eine Hardware Tastatur, aber es ist ausgeblendet, und es ist keine Software Tastatur aktiviert.

  - `keyssoft`&ndash; auf dem Gerät ist eine Software Tastatur aktiviert.

- **Primäre Text Eingabemethode** &ndash; Verwenden Sie, um anzugeben, welche Arten von Hardware Schlüsseln für die Eingabe verfügbar sind. Dabei sind folgende Werte möglich:

  - `nokeys`&ndash; Es sind keine Hardwareschlüssel für die Eingabe vorhanden.

  - `qwerty`&ndash; Eine QWERTY-Tastatur ist verfügbar.

  - `12key`&ndash; Es ist eine Hardware Tastatur mit 12 Schlüsseln vorhanden.


- **Verfügbarkeit von Navigations Schlüsseln** &ndash; Für den Fall, dass die Navigation mit dem 5-Wege-oder d-Pad (direktionaler Pad) verfügbar ist Dies kann sich während der Lebensdauer der Anwendung ändern. Dabei sind folgende Werte möglich:

  - `navexposed`&ndash; die Navigationsschlüssel sind für den Benutzer verfügbar.

  - `navhidden`&ndash; die Navigationsschlüssel sind nicht verfügbar.

- **Primäre Non-Touchscreen-Navigations Methode** &ndash; Die Art der auf dem Gerät verfügbaren Navigation. Dabei sind folgende Werte möglich:

  - `nonav`&ndash; die einzige verfügbare Navigationsfunktion ist der Touchscreen.

  - `dpad`&ndash; ein d-Pad (direktionaler Pad) ist für die Navigation verfügbar.

  - `trackball`&ndash; das Gerät verfügt über einen trackballunterstützung für die Navigation.

  - `wheel`&ndash; das ungewöhnliche Szenario, in dem ein oder mehrere direktionale Räder verfügbar sind

- **Platt Form Version (API-Ebene)** Die API-Ebene, die vom Gerät im Format v*N*unterstützt wird, wobei N für die API-Ebene steht, die Ziel ist. &ndash; Beispielsweise wird für v11 ein Gerät auf API Level 11 (Android 3,0) als Ziel verwendet.


Ausführlichere Informationen zu Ressourcen Qualifizierern finden Sie unter [Bereitstellen von Ressourcen](https://developer.android.com/guide/topics/resources/providing-resources.html) auf der Android-Entwickler Website.


## <a name="how-android-determines-what-resources-to-use"></a>Bestimmen der zu verwendenden Ressourcen durch Android

Es ist sehr möglich und wahrscheinlich, dass eine Android-Anwendung viele Ressourcen enthält. Es ist wichtig zu verstehen, wie Android die Ressourcen für eine Anwendung auswählt, wenn diese auf einem Gerät ausgeführt wird.

Android bestimmt die Ressourcenbasis durch Iteration des folgenden Regel Tests:

- **Widersprüchliche Qualifizierer ausschließen** &ndash; Wenn z. b. die Geräte Ausrichtung Hochformat ist, werden alle quer Ressourcen Verzeichnisse zurückgewiesen.

- **Qualifizierer ignorieren nicht unterstützt** &ndash; Nicht alle Qualifizierer sind für alle API-Ebenen verfügbar. Wenn ein Ressourcenverzeichnis einen Qualifizierer enthält, der vom Gerät nicht unterstützt wird, wird dieses Ressourcenverzeichnis ignoriert.

- **Bestimmen des Qualifizierers mit der höchsten Priorität** &ndash; Wenn Sie auf die obige Tabelle verweisen, wählen Sie den Qualifizierer mit der höchsten Priorität (von oben nach unten).

- **Alle Ressourcen Verzeichnisse für Qualifizierer beibehalten** &ndash; wenn Ressourcen Verzeichnisse vorhanden sind, die mit dem Qualifizierer der obigen Tabelle identisch sind, wählen Sie den Qualifizierer mit der höchsten Priorität aus (von oben nach unten).

Diese Regeln werden auch im folgenden Flussdiagramm veranschaulicht:

[![Flussdiagramm für Ressourcen](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

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

- Gebiets Schema &ndash; en-GB
- **Ausrichtung** &ndash; Port
- **Bildschirm Dichte** &ndash; hdpi
- **Touchscreen-Typ** &ndash; NoTouch
- **Primäre Eingabemethode** &ndash; 12key

Zunächst werden die französischen Ressourcen eliminiert, da Sie mit dem Gebiets Schema von `en-GB`in Konflikt stehen, und wir haben Folgendes:

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

Der nächste Qualifizierer ist ausgewählt (Sprache). Es gibt Ressourcen, die dem Sprachcode entsprechen. Alle Ressourcen Verzeichnisse, die nicht dem Sprachcode von `en` entsprechen, werden abgelehnt, sodass die Liste der Ressourcen jetzt lautet:

```
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
```

Der nächste Qualifizierer, der vorhanden ist, ist für die Bildschirm Ausrichtung vorgesehen, sodass alle Ressourcen Verzeichnisse, die `port` nicht mit der Bildschirm Ausrichtung von identisch sind, gelöscht werden:

```
drawable-en-port
drawable-en-port-ldpi
```

Als nächstes ist der Qualifizierer für `ldpi`die Bildschirm Dichte,, der zum Ausschluss eines weiteren Ressourcen Verzeichnisses führt:

```
drawable-en-port-ldpi
```

Im Ergebnis dieses Prozesses verwendet Android die drawable-Ressourcen im Ressourcenverzeichnis `drawable-en-port-ldpi` für das Gerät.

> [!NOTE]
> Die Qualifizierer für die Bildschirmgröße stellen eine Ausnahme für diesen Auswahlprozess dar. Android kann Ressourcen auswählen, die für einen kleineren Bildschirm entwickelt wurden, als das aktuelle Gerät bereitstellt. Beispielsweise kann ein großes Bildschirm die Ressourcen verwenden, die für einen Bildschirm mit normaler Größe bereitgestellt werden. Dies trifft jedoch nicht zu: das gleiche Gerät für große Bildschirme verwendet nicht die Ressourcen, die für einen XLarge-Bildschirm bereitgestellt werden. Wenn Android keinen Ressourcen Satz finden kann, der mit einer bestimmten Bildschirmgröße übereinstimmt, stürzt die Anwendung ab.
