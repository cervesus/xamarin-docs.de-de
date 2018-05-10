---
title: Mithilfe von CCTextureCache Textur-Caching
description: Der CocosSharp CCTextureCache Klasse bietet ein gängiges Verfahren zum Organisieren, Cache, und entfernen Inhalt an. Er ist besonders nützlich für große Spiele, die nicht vollständig in den Arbeitsspeicher, vereinfacht den Prozess der Gruppierung und Freigeben von Texturen anpassen können.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bb75efea0914827f1d59a8e0943584597f91803a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="texture-caching-using-cctexturecache"></a>Textur Zwischenspeichern mithilfe von CCTextureCache

_Der CocosSharp CCTextureCache Klasse bietet ein gängiges Verfahren zum Organisieren, Cache, und entfernen Inhalt an. Er ist besonders nützlich für große Spiele, die nicht vollständig in den Arbeitsspeicher, vereinfacht den Prozess der Gruppierung und Freigeben von Texturen anpassen können._

Die `CCTextureCache` Klasse ist ein wesentlicher Bestandteil des CocosSharp 3D-Spielentwicklung. Die meisten CocosSharp Spiele verwenden die `CCTextureCache` Objekt, auch wenn nicht explizit so viele CocosSharp Methoden intern verwenden eine *freigegebenen* texture-Cache.

Dieser Leitfaden behandelt die `CCTextureCache` und daher ist es wichtig, dass 3D-Spielentwicklung bleiben. Insbesondere werden sie behandelt:

 - Warum texture-caching von Bedeutung
 - Texture-Lebensdauer
 - Verwenden von SharedTextureCache
 - Lazy loading-im Vergleich zu vor Laden mit AddImage
 - Freigeben von Texturen


## <a name="why-texture-caching-matters"></a>Warum texture-caching von Bedeutung

Zwischenspeichern der Textur ist ein wichtiger Bestandteil der Entwicklung von Textur laden ist ein zeitaufwendiger Vorgang und Texturen erfordern eine beträchtliche Menge an Arbeitsspeicher zur Laufzeit.

Wie bei jeder Dateivorgang möglich Texturen von einem Datenträger laden ein kostspieliger Vorgang. Textur laden kann zusätzliche Zeit in Anspruch nehmen, wenn die geladene Datei erfordert die Verarbeitung, z. B. dekomprimiert werden, (wie bei Png oder Jpg-Bilder der Fall ist). Textur Zwischenspeichern reduzieren die Anzahl der Male, dass die Anwendung Dateien von der Festplatte geladen werden muss.

Wie bereits erwähnt, belegen Texturen auch eine große Menge an Arbeitsspeicher für Common Language Runtime. Ein Hintergrundbild auf die der Auflösung des iPhone 6 (1344 x 750) würde z. B. 4 MB RAM – belegen, selbst wenn die PNG-Datei nur einige Kilobyte groß ist. Textur caching bietet eine Möglichkeit, Textur-Verweise in einer app freizugeben, sowie eine einfache Möglichkeit, alle Inhalte zu entladen, wenn zwischen unterschiedlichen Spiel Status übergeht.


## <a name="texture-lifespan"></a>Texture-Lebensdauer

CocosSharp Texturen können für die gesamte Dauer der Ausführung einer app im Speicher gehalten werden, oder als kurzlebiges. Verwendung einer app sollten von Texturen, die nicht mehr benötigten freigeben, um Speicher zu minimieren. Natürlich, bedeutet dies, dass Texturen verworfen und neu zu einem späteren Zeitpunkt, die Ladezeiten zu erhöhen oder die Leistung während der lädt beeinträchtigen kann geladen werden können. 

Textur laden häufig erfordert einen Kompromiss zwischen Arbeitsspeicher nutzungs- und Zeiten / Leistung zur Laufzeit. Spiele, die eine kleine Menge an Textur Arbeitsspeicher verwenden können alle Strukturen im Arbeitsspeicher nach Bedarf beibehalten, aber größere Spiele müssen möglicherweise Entladen von Texturen, um Speicherplatz freizugeben.

Das folgende Diagramm zeigt ein einfaches Spiel lädt Texturen nach Bedarf, und behält diese für die gesamte Dauer der Ausführung im Arbeitsspeicher:

![](texture-cache-images/image1.png "Die folgende Abbildung zeigt ein einfaches Spiel lädt Texturen nach Bedarf, und behält diese für die gesamte Dauer der Ausführung im Arbeitsspeicher")

Die ersten beiden Balken darstellen Texturen, die sofort nach der Ausführung des Spiels jedoch benötigt werden. Die folgenden drei Balken darstellen, Texturen für jede Ebene, die nach Bedarf geladen wird.

Wenn das Spiel groß ist lädt genug es schließlich genügend Texturen, um alle von dem Gerät und dem Betriebssystem bereitgestellten RAM zu füllen. Um dieses Problem zu beheben, kann ein Spiel texturdaten entladen, wenn er nicht mehr benötigt wird. Das folgende Diagramm zeigt z. B. ein Spiel, das die Level1Texture entladen wird, wenn es nicht mehr benötigt wird, dann Level2Texture für die nächste Ebene lädt. Das Endergebnis ist, dass nur drei Texturen zu einem beliebigen Zeitpunkt im Speicher gehalten werden: 

![](texture-cache-images/image2.png "Das Endergebnis ist, dass nur drei Texturen zu einem beliebigen Zeitpunkt im Speicher gehalten werden")


Im oben gezeigte Diagramm gibt an, dass Textur speicherauslastung kann durch das Entladen reduziert werden, aber dies erfordert möglicherweise zusätzliche Ladezeiten, wenn ein Player eine Ebene wiedergeben möchte. Es ist auch Folgendes zu beachten, dass die UITexture und MainCharacter Texturen geladen und nie entladen werden. Dies bedeutet, dass diese Texturen in allen Ebenen benötigt werden, damit sie immer im Arbeitsspeicher beibehalten werden. 


## <a name="using-sharedtexturecache"></a>Verwenden von SharedTextureCache

CocosSharp automatisch Texturen zwischengespeichert, wenn sie über das Laden der `CCSprite` Konstruktor. Im folgenden Code wird z. B. nur eine Instanz der Textur erstellt:


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp automatisch zwischengespeichert der `star.png` Textur vermeiden Sie die aufwändige Alternative zum Erstellen von zahlreichen identische `CCTexture2D` Instanzen. Dies wird erreicht, indem `AddImage` aufgerufenen auf eine freigegebene `CCTextureCache` -Instanz erfolgt, insbesondere `CCTextureCache.SharedTextureCache.Shared`. Um zu verstehen, wie die `SharedTextureCache` wird verwendet, können wir sehen Sie sich den folgenden Code der ist funktionell identisch zum Aufrufen der `CCSprite` Konstruktor mit einem Zeichenfolgenparameter:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` überprüft, ob der Argumentdatei (in diesem Fall `star.png`) wurde bereits geladen. Wenn dies der Fall ist, wird die zwischengespeicherte Instanz zurückgegeben. Wenn dies nicht anschließend, aus dem Dateisystem erfolgt und ein Verweis auf die Textur intern, für gespeichert wird nachfolgende `AddImage` aufrufen. In anderen Worten: die `star.png` -Image nur einmal geladen wird, und nachfolgende Aufrufe erfordern keine zusätzliche Datenträgerzugriff oder zusätzliche Textur Arbeitsspeicher.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Lazy loading-im Vergleich zu vor Laden mit AddImage

`AddImage` ermöglicht es Code, dem geschrieben werden soll, ob die angeforderte Textur oder nicht bereits geladen ist. Das bedeutet, dass der Inhalt wird nicht geladen werden, bis diese benötigt wird; Allerdings kann dies auch zur Laufzeit aufgrund unvorhersehbare Laden von Inhalt Leistungsprobleme auftreten.

Betrachten Sie ein Spiel, in dem der Spieler Waffe aktualisiert werden kann. Wenn aktualisiert, ändert die Waffe Projektile sichtbar und führt neue Texturen, die verwendet wird. Ist der Inhalt verzögerte geladen wird werden dann die Texturen aktualisierten Waffen zugeordnet nicht zu Beginn sondern zu einem späteren Zeitpunkt geladen werden Fall, wenn der Spieler die Upgrades erwirbt. 

Dieser Blogbeitrag Spielverlauf laden kann dazu führen, dass das Spiel *pop*, dies ist eine kurze, jedoch bemerkbar Einfrieren bei Ausführung. Um dies zu verhindern, kann der Code Vorhersagen möglicherweise voraus, und sie vorab laden, welche Strukturen erforderlich. Beispielsweise kann die folgenden verwendet werden um Texturen vorab zu laden:


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Diese vorab laden kann dazu führen, verwendeter Speicher und Startzeit erhöhen kann. Der Spieler weichen z. B. tatsächlich nie eine dargestellte Einschalten der `powerup3.png` Textur, damit sie unnötig geladen wird. Natürlich kann das erforderlichen Kosten für einen potenziellen Pop im Spielverlauf, vermeiden Sie daher, Vorabladen von Inhalt in der Regel am besten, wenn er im Arbeitsspeicher passt zu Zahlen sein.


## <a name="disposing-textures"></a>Freigeben von Texturen

Wenn Sie ein Spiel, das keine erfordert mehr Textur Arbeitsspeicher als auf dem minimum Spec-Gerät verfügbar ist, dann Texturen nicht verworfen werden müssen. Andererseits, möglicherweise größer Spiele Textur Arbeitsspeicher, um Platz für neue Inhalte freizugeben. Beispielsweise kann ein Spiel, das eine große Menge an Arbeitsspeicher, die Speichern von Texturen für eine Umgebung zurückgreifen. Wenn die Umgebung nur in einer bestimmten Ebene verwendet wird sollte dann sie entladen werden nach Beendigung die Ebene.


### <a name="disposing-a-single-texture"></a>Freigeben einer einzelnen Struktur

Entfernen eine einzelne Struktur zuerst erfordert das Aufrufen der `Dispose` -Methode, und klicken Sie dann auf Manuelles Entfernen aus der `CCTextureCache`.

Im folgenden wird gezeigt, wie ein Hintergrund Sprite zusammen mit seiner Struktur vollständig zu entfernen:


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Direkt disposing Texturen kann effektiv sein, wenn der Umgang mit einer kleinen Anzahl von Texturen, aber dies beim Umgang mit größeren Textur Mengen fehleranfällig werden kann.

Texturen gruppiert werden können (nicht freigegebenen) benutzerdefinierte `CCTextureCache` -Instanzen, die Bereinigung der Textur zu vereinfachen.

Betrachten Sie beispielsweise ein Beispiel, wobei Inhalt vorab geladen wurde, verwenden eine Stufe-spezifische `CCTextureCache` Instanz. Die `CCTextureCache` Instanz kann in der Klasse definiert die Ebene definiert werden (die möglicherweise eine `CCLayer` oder `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

Die `levelTextures` Instanz kann dann verwendet, um die Level-spezifische Texturen vorab laden: 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Schließlich am Ende die Ebene, die Texturen können werden alle auf einmal durch verworfen der `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Die Dispose-Methode werden alle internen Strukturen, die gelöscht wird, des durch diese Texturen verwendeten Arbeitsspeichers freigeben. Kombinieren von `CCTextureCache.Shared` mit einer Ebene oder Spiel modusspezifische `CCTextureCache` Instanz führt einige Texturen, die über das gesamte Spiel und andere als Ebenen, ähnlich wie im Diagramm dargestellt, die am Anfang dieses Handbuchs enden entladen beibehalten: 

![](texture-cache-images/image2.png "Kombinieren von CCTextureCache.Shared mit Stufe oder Spiel modusspezifische CCTextureCache Instanz Ergebnissen in einigen Texturen, die über das gesamte Spiel und andere als Ebenen, ähnlich wie im Diagramm dargestellt, die am Anfang dieses Handbuchs enden entladen beibehalten")




## <a name="summary"></a>Zusammenfassung

Diese Anleitung zeigt, wie die `CCTextureCache` Klasse, um den Saldo Arbeitsspeicher Verwendung und die Common Language Runtime-Leistung. `CCTexturCache.SharedTextureCache` kann explizit oder implizit zu laden und Zwischenspeichern von Texturen, für die Lebensdauer der Anwendung verwendet, während er sich `CCTextureCache` Instanzen dienen zum Entladen von Texturen, um die speicherauslastung zu reduzieren.

## <a name="related-links"></a>Verwandte links

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
