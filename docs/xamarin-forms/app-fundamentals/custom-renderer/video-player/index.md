---
title: Implementieren eines Videoplayers
description: In diesem Artikel wird erläutert, wie eine Videoplayeranwendung mithilfe von Xamarin.Forms implementiert wird.
ms.prod: xamarin
ms.assetid: 0CE9BEE7-4F81-4A00-B9B3-5E2535CD3050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 00697ca0adf3a34abec90c2f96d9fd9c273d06bb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239783"
---
# <a name="implementing-a-video-player"></a>Implementieren eines Videoplayers

Manchmal ist es erwünscht, dass Videodateien in einer Xamarin.Forms-Anwendung wiedergegeben werden. In dieser Artikelreihe wird erläutert, wie benutzerdefinierte Renderer für iOS, Android und die universelle Windows-Plattform (UWP) für eine Xamarin.Forms-Klasse namens `VideoPlayer` geschrieben werden.

Im [**VideoPlayerDemos**](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)-Beispiel befinden sich alle Dateien, die `VideoPlayer` implementieren und unterstützen, in Ordnern mit der Bezeichnung `FormsVideoLibrary`. Sie werden zudem mit dem Namespace `FormsVideoLibrary` oder Namespaces, die mit `FormsVideoLibrary` beginnen, identifiziert. Diese Organisation und Benennung sollte es einfach machen, die Videoplayerdateien in Ihre eigene Xamarin.Forms-Projektmappe zu kopieren.

`VideoPlayer` kann Videodateien von drei Quelltypen abspielen:

- aus dem Internet über eine URL
- aus einer Ressource, die in der Plattformanwendung eingebettet ist
- aus der Videobibliothek des Geräts

Videoplayer erfordern *Transportsteuerelemente*, also Schaltflächen zum Abspielen und Pausieren des Videos, sowie einen Positionierungsbalken, der den Fortschritt des Videos anzeigt und dem Benutzer so ermöglicht, schnell zu einer anderen Position zu springen. `VideoPlayer` kann entweder die Transportsteuerelemente und den Positionierungsbalken verwenden, die von der Plattform (wie unten dargestellt) bereitgestellt werden, oder Sie können benutzerdefinierte Transportsteuerelemente und den Positionierungsbalken selbst bereitstellen. In diesem Beispiel wird das Programm unter iOS, Android und der universellen Windows-Plattform dargestellt:

[![Webvideo abspielen](web-videos-images/playwebvideo-small.png "Webvideo abspielen")](web-videos-images/playwebvideo-large.png#lightbox "Webvideo abspielen")

Natürlich können Sie das Telefon auch seitwärts drehen, um die Ansicht zu vergrößern.

Ein komplexerer Videoplayer hätte zusätzliche Funktionen wie die Lautstärkeregelung, einen Mechanismus zum Unterbrechen des Videos, wenn ein Telefonanruf eingeht, sowie eine Möglichkeit, den Bildschirm während der Wiedergabe aktiv zu halten.

In den folgenden Artikeln erhalten Sie nach und nach Informationen darüber, wie Plattformrenderer und unterstützende Klassen erstellt werden:

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[Erstellen der Videoplayer für die Plattform](player-creation.md)

Jede Plattform erfordert eine `VideoPlayerRenderer`-Klasse, die ein Videoplayersteuerelement, das von der Plattform unterstützt wird, erstellt und verwaltet. Der Artikel stellt die Struktur der Rendererklassen dar und wie die Player erstellt werden.

## <a name="playing-a-web-videoweb-videosmd"></a>[Wiedergeben eines Webvideos](web-videos.md)

Die häufigste Quelle für Videos für einen Videoplayer ist wahrscheinlich das Internet. In diesem Artikel wird beschrieben, wie auf ein Webvideo verwiesen werden kann und es als Quelle für den Videoplayer verwendet werden kann.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[Binden von Videoquellen an den Player](source-bindings.md)

Dieser Artikel verwendet eine `ListView`, um eine Videosammlung darzustellen, die abgespielt werden soll. Ein Programm zeigt, wie die CodeBehind-Datei die Videoquelle des Videoplayers festlegen kann. Im zweiten Programm wird jedoch dargestellt, wie Sie die Datenbindung zwischen `ListView` und dem Videoplayer verwenden können.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[Laden von Anwendungsressourcenvideos](loading-resources.md)

Videos können in Plattformprojekten als Ressource eingebettet werden. In diesem Artikel wird erläutert, wie diese Ressourcen gespeichert und später in das Programm geladen werden, sodass sie über den Videoplayer abgespielt werden können.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[Zugreifen auf die Videobibliothek des Geräts](accessing-library.md)

Wenn ein Video über die Kamera des Geräts erstellt wurde, wird die Videodatei in der Bildbibliothek des Geräts gespeichert. In diesem Artikel wird erläutert, wie auf die Bildauswahl des Geräts zugegriffen werden kann, um ein Video auszuwählen und dieses dann mithilfe des Videoplayers abzuspielen.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[Benutzerdefinierte Videotransport-Steuerelemente](custom-transport.md)

Obwohl Videoplayer auf jeder Plattform eigene Transportsteuerelemente in Form von Schaltflächen für **Abspielen** und **Pausieren** bieten, können Sie die Anzeige solcher Schaltflächen unterdrücken und dafür Ihre eigenen bereitstellen. Dieser Artikel zeigt Ihnen wie.

## <a name="custom-video-positioningcustom-positioningmd"></a>[Benutzerdefinierte Videopositionierung](custom-positioning.md)

Jeder Videoplayer der Plattform verfügt über einen Positionierungsbalken, der den Fortschritt des Videos anzeigt und Ihnen so ermöglicht, weiter nach vorne oder zurück zu einer bestimmten Position zu springen. In diesem Artikel wird gezeigt, wie Sie diesen Positionierungsbalken mit einem benutzerdefinierten Steuerelement ersetzen können.





## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
