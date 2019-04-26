---
title: Zwischenspeichern von Texturen mit CCTextureCache
description: CocosSharp CCTextureCache-Klasse bietet es sich um eine standardisierte Möglichkeit zum Organisieren, Cache, und Entfernen von Inhalt. Es ist besonders nützlich für große Spiele, die nicht vollständig in den Arbeitsspeicher, vereinfacht den Prozess der Gruppierung und Freigeben von Texturen entsprechen können.
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 232363d6ce1cb93499716b2c1247c48403cf6cea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61302334"
---
# <a name="texture-caching-using-cctexturecache"></a>Zwischenspeichern von Texturen mit CCTextureCache

_CocosSharp CCTextureCache-Klasse bietet es sich um eine standardisierte Möglichkeit zum Organisieren, Cache, und Entfernen von Inhalt. Es ist besonders nützlich für große Spiele nicht vollständig in den Arbeitsspeicher, vereinfacht den Prozess der Gruppierung und Freigeben von Texturen anpassen können._

Die `CCTextureCache` Klasse ist ein wesentlicher Bestandteil der Entwicklung von Spielen CocosSharp. Die meisten CocosSharp Spiele verwenden die `CCTextureCache` Objekt, auch wenn nicht explizit, wie viele CocosSharp-Methoden intern verwenden eine *freigegebenen* texture-Cache.

Dieser Leitfaden behandelt die `CCTextureCache` und aus welchem Grund es wichtig, für die Entwicklung von Spielen ist. Er behandelt insbesondere folgende Aktionen ausführen:

 - Warum texture-caching von Bedeutung
 - Textur-Lebensdauer
 - Verwenden von SharedTextureCache
 - Lazy loading-im Vergleich zu vorabladens mit AddImage
 - Verwerfen von Texturen


## <a name="why-texture-caching-matters"></a>Warum texture-caching von Bedeutung

Zwischenspeichern von Texturen ist ein wichtiger Aspekt bei der Entwicklung von Spielen, wie die Textur laden ist ein zeitaufwändiger Vorgang und Texturen erfordert eine beträchtliche Menge an Arbeitsspeicher zur Laufzeit.

Wie bei jedem Dateivorgang kann die Texturen von einem Datenträger laden ein kostspieliger Vorgang sein. Laden der Textur kann zusätzliche Zeit in Anspruch nehmen, wenn die zu ladende Datei verarbeitungsquellen erfordert, z.B. wird dekomprimiert, (wie bei Png und Jpg-Bildern der Fall ist). Zwischenspeichern von Texturen kann die weniger häufig, dass die Anwendung Dateien vom Datenträger geladen werden muss.

Wie bereits erwähnt, nehmen Texturen auch eine große Menge an Arbeitsspeicher für Common Language Runtime. Ein Hintergrundbild, die die Auflösung von einem iPhone 6 (1344 x 750) angepasst würde z. B. 4 MB des Arbeitsspeichers – belegen, auch wenn die PNG-Datei nur einige Kilobyte groß ist. Zwischenspeichern von Texturen bietet eine Möglichkeit, Textur-Verweise in eine app freizugeben, sowie eine einfache Möglichkeit, alle Inhalte zu entladen, wenn der Übergang zwischen Zuständen für verschiedene Spiele.


## <a name="texture-lifespan"></a>Textur-Lebensdauer

CocosSharp-Texturen für die gesamte Dauer der app-Ausführung im Arbeitsspeicher beibehalten werden können, oder sie können kurzlebig sein. Verwendung einer app sollten von Texturen, wenn nicht mehr benötigt freigeben, um Speicher zu minimieren. Natürlich bedeutet dies, dass Texturen möglicherweise gelöscht und neu geladen wird, zu einem späteren Zeitpunkt an, die Ladezeiten zu erhöhen oder die Leistung bei Lasten beeinträchtigen kann. 

Textur loading häufig erfordert einen Kompromiss zwischen Arbeitsspeicher nutzungs- und / Leistung zur Laufzeit. Verwenden Sie eine kleine Datenmenge Texturspeicher Spiele können alle Texturen im Speicher nach Bedarf beibehalten, größere Spiele müssen jedoch möglicherweise Entladen von Texturen, um Speicherplatz freizugeben.

Das folgende Diagramm zeigt ein einfaches Spiel die Texturen lädt, je nach Bedarf, und bleiben im Arbeitsspeicher für die gesamte Dauer der Ausführung:

![](texture-cache-images/image1.png "Dieses Diagramm zeigt ein einfaches Spiel die Texturen lädt, je nach Bedarf, und bleiben im Arbeitsspeicher für die gesamte Dauer der Ausführung")

Die ersten beiden Balken darstellen, Texturen, die sofort nach der Ausführung für das Spiel benötigt werden. Die folgenden drei Balken darstellen, Texturen für jede Ebene, die nach Bedarf geladen wird.

Wenn das Spiel groß war würde genug es schließlich genügend Texturen, um alle von dem Gerät und dem Betriebssystem bereitgestellten RAM zu füllen geladen werden. Um dieses Problem zu beheben, kann ein Spiel die Daten zu entladen, wenn es nicht mehr benötigt wird. Das folgende Diagramm zeigt z.B. ein Spiel die Level1Texture entladen wird, wenn es nicht mehr benötigt und lädt dann Level2Texture für die nächste Ebene. Das Endergebnis ist, dass nur drei Texturen im Speicher, zu jedem Zeitpunkt gehalten werden: 

![](texture-cache-images/image2.png "Das Endergebnis ist, dass nur drei Texturen zu jedem Zeitpunkt im Speicher gehalten werden")


Oben gezeigte Diagramm gibt an, dass die speicherauslastung für die Textur kann reduziert werden, indem entladen, aber dies erfordert möglicherweise zusätzliche Ladezeiten, wenn ein Player zum Wiedergeben einer Ebene entscheidet. Es ist auch erwähnenswert, dass die Texturen UITexture und MainCharacter geladen und niemals entladen werden. Dies bedeutet, dass diese Texturen auf sämtlichen Ebenen benötigt werden, damit sie immer im Arbeitsspeicher beibehalten werden. 


## <a name="using-sharedtexturecache"></a>Verwenden von SharedTextureCache

CocosSharp enthält Automatisches Caching Texturen beim Laden über die `CCSprite` Konstruktor. Der folgende Code erstellt z. B. nur eine Textur-Instanz:


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp enthält Automatisches Caching von der `star.png` Textur vermeiden Sie die teure Alternative zum Erstellen zahlreicher identisch `CCTexture2D` Instanzen. Dies erfolgt durch `AddImage` aufgerufenen auf eine freigegebene `CCTextureCache` Instanz, insbesondere `CCTextureCache.SharedTextureCache.Shared`. Um zu verstehen wie das `SharedTextureCache` wird verwendet, sehen wir den folgenden Code dem zum Aufruf funktional identisch ist die `CCSprite` Konstruktor mit einem Zeichenfolgenparameter:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` überprüft, ob der Argumentdatei (in diesem Fall `star.png`) wurde bereits geladen. Wenn dies der Fall ist, wird die zwischengespeicherte Instanz zurückgegeben. Wenn keine dann ist es aus dem Dateisystem geladen, und ein Verweis auf die Textur intern, für gespeichert wird nachfolgende `AddImage` aufrufen. Anders ausgedrückt: die `star.png` -Image nur einmal geladen wird und nachfolgende Aufrufe erfordern keine zusätzliche Datenträger zuzugreifen oder zusätzliche Texturspeicher.


## <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Lazy loading-im Vergleich zu vorabladens mit AddImage

`AddImage` ermöglicht es Code, der gleich geschrieben werden soll, ob die angeforderte Textur oder nicht bereits geladen ist. Dies bedeutet, dass Inhalt wird nicht geladen werden, bis diese benötigt wird; Allerdings kann dies auch zur Laufzeit aufgrund unvorhersehbare Inhalt laden Leistungsprobleme.

Nehmen Sie beispielsweise ein Spiel, in denen des Spielers Waffe aktualisiert werden können. Wenn aktualisiert, werden die sorgfältig und Projektile sichtbar geändert, führt neue Texturen, die verwendet wird. Wenn der Inhalt lazy geladen ist werden klicken Sie dann die Texturen, die aktualisierten Waffen zugeordnet nicht zunächst sondern zu einem späteren Zeitpunkt geladen werden Wenn der Spieler die Upgrades bereitstellt. 

Dieser Ladevorgang mid-Spiel kann dazu führen, dass das Spiel zu *pop*, dies ist eine kurze, aber wichtigen Einfrieren bei Ausführung. Um dies zu verhindern, kann der Code Vorhersagen, welche Strukturen ggf. voraus, und sie vorab zu laden. Beispielsweise kann der folgende verwendet werden zum Vorabladen von Texturen:


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Dieses Vorabladen von kann zu verschwendetem Speicherplatz und kann die Dauer des verbindungsstarts erhöhen. Der Player kann nie erhalten beispielsweise eine verhältnisschwellenwert-Power-Up der `powerup3.png` Struktur, damit sie unnötigerweise geladen wird. Natürlich kann dies sein erforderlichen Kosten für das bezahlen, um einen potenziellen Pop in Gaming, zu vermeiden, damit es sich zum Inhalt der vorab laden, empfiehlt Wenn es in den Arbeitsspeicher passen.


## <a name="disposing-textures"></a>Verwerfen von Texturen

Wenn ein Spiel keine weitere Texturspeicher erfordert als auf die Spezifikation mindesteinrichtung des Geräts verfügbar ist, dann Texturen nicht verworfen werden müssen. Auf der anderen Seite müssen größere Spiele Texturspeicher, um Platz für neue Inhalte freizugeben. Ein Spiel kann z. B. eine große Menge an Arbeitsspeicher, die Speichern von Texturen für eine Umgebung verwenden. Wenn die Umgebung nur in einer bestimmten Ebene verwendet wird sollte dann sie entladen werden nach dem Ende der Ebene.


### <a name="disposing-a-single-texture"></a>Löschen einer einzelnen Struktur

Entfernen einer einzelnen Struktur zuerst erfordert das Aufrufen der `Dispose` -Methode, und klicken Sie dann auf manuelle Entfernung aus der `CCTextureCache`.

Der folgende Code zeigt ein Sprite Hintergrund sowie dessen Struktur vollständig zu entfernen:


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Direkt disposing Texturen kann effektiv sein, beim Umgang mit einer kleinen Anzahl von Texturen, aber dies beim Umgang mit größeren Mengen der Textur fehleranfälligen werden kann.

Texturen können in benutzerdefinierten (nicht freigegebenen) gruppiert werden `CCTextureCache` Instanzen zum Vereinfachen der Bereinigung der Textur.

Betrachten Sie beispielsweise ein Beispiel, Inhalt vorab geladen wurde, über eine Level-spezifische `CCTextureCache` Instanz. Die `CCTextureCache` Instanz in der Klasse definieren Sie die Ebene definiert werden kann (die möglicherweise eine `CCLayer` oder `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

Die `levelTextures` Instanz kann dann verwendet, um die Ebenen spezifische Texturen vorab zu laden: 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Schließlich am Ende die Ebene die Texturen können werden alle freigegebenen gleichzeitig über die `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Die Dispose-Methode gibt alle internen Texturen, die gelöscht wird, die von diesen Texturen verwendeten Arbeitsspeicher frei. Kombinieren von `CCTextureCache.Shared` mit einer Ebene oder Spiele modusspezifische `CCTextureCache` Instanz führt einige Texturen, die Beibehaltung des gesamten Spiels und einige entladen wird, wie Ebenen, ähnlich wie im Diagramm angezeigt, die zu Beginn dieses Handbuchs zu beenden: 

![](texture-cache-images/image2.png "Kombinieren von CCTextureCache.Shared mit einer Ebene oder Spiele modusspezifische CCTextureCache führt einige Texturen, die Beibehaltung des gesamten Spiels und einige entladen wird, wie Ebenen, ähnlich wie im Diagramm angezeigt, die zu Beginn dieses Handbuchs beenden")




## <a name="summary"></a>Zusammenfassung

Dieses Handbuch veranschaulicht, wie die `CCTextureCache` Klasse, um das ideale Verhältnis zwischen Arbeitsspeicher Nutzung und der Common Language Runtime-Leistung. `CCTexturCache.SharedTextureCache` kann explizit oder implizit zum Laden und Zwischenspeichern von Texturen für die Lebensdauer der Anwendung, während er sich `CCTextureCache` Instanzen können verwendet werden, um das Entladen von Texturen, um die speicherauslastung zu reduzieren.

## <a name="related-links"></a>Verwandte Links

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
