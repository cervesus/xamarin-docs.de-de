---
Title: "Warum wird meine Xamarin.Forms . Fehler beim Maps-Android-Projekt mit einem unerwarteten Fehler auf oberster Ebene in compiletedalvik? "
ms. Topic: Problembehandlung bei MS. Prod: xamarin ms. assetid: C0251EB1-F509-47AD-98D6-846AF46425E5 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 04/25/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Warum ist mein Xamarin.Forms . Fehler beim Maps-Android-Projekt mit einem unerwarteten Fehler der obersten Ebene in compiletedalvik?

Dieser Fehler wird möglicherweise im Fehler Fenster von Visual Studio für Mac oder im Fenster "Buildausgabe" von Visual Studio angezeigt. in Android-Projekten, die verwenden Xamarin.Forms . Maps.

Dies wird am häufigsten durch Erhöhen der Java-Heapgröße für Ihr xamarin. Android-Projekt gelöst. Führen Sie diese Schritte aus, um die Heapgröße zu erhöhen:

## <a name="visual-studio"></a>Visual Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt & die Projektoptionen zu öffnen.
2. Gehe zu **Android-Optionen-> erweitert**
3. Geben Sie im Textfeld Java-Heap Größe den Wert 1 g ein.
4. Erstellen Sie das Projekt neu.

![Screenshot der Visual Studio-Projektoptionen](maps-compiletodalvik-error-images/vsjavaheap.png "Android-Buildoptionen in Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt & die Projektoptionen zu öffnen.
2. Wechseln Sie zu **Build-> Android-Build > erweitert** .
3. Geben Sie im Textfeld Java-Heap Größe den Wert 1 g ein.
4. Erstellen Sie das Projekt neu.  

![Screenshot der Optionen des Visual Studio für Mac Projekts](maps-compiletodalvik-error-images/xsjavaheap.png "Android-Buildoptionen in Visual Studio für Mac")
