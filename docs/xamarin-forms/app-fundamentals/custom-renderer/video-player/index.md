---
title: Implementieren einen video player
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CE9BEE7-4F81-4A00-B9B3-5E2535CD3050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a889be5ee31f667117d2c36859e667980f0e6610
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="implementing-a-video-player"></a>Implementieren einen video player

Manchmal ist es wünschenswert sein, die in einer Anwendung Xamarin.Forms Videodateien wiedergeben. Diese Reihe von Artikeln wird erläutert, wie zum Schreiben von benutzerdefinierten Renderer für iOS, Android und die universelle Windows-Plattform (UWP) für eine Xamarin.Forms-Klasse, die mit dem Namen `VideoPlayer`.

In der [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Stichprobe, die Dateien, die zu implementieren und unterstützen `VideoPlayer` befinden sich im Ordner mit den Namen `FormsVideoLibrary` und mit dem Namespace identifiziert `FormsVideoLibrary` oder Namespaces beginnen Sie, die `FormsVideoLibrary`. Diese Organisation und Benennung sollten erleichtern die Videoplayer-Dateien in Ihren eigenen Xamarin.Forms-Projektmappe kopieren.

`VideoPlayer` Videodateien aus drei Arten von Quellen können wiedergegeben werden:

- Das Internet mithilfe einer URL
- Eine Ressource eingebettet in der Plattform-Anwendung
- Das Gerät Videobibliothek

Video-Player erfordern *Transportsteuerelemente*, die Schaltflächen zum Wiedergeben und Anhalten des Videos sind und eine Positionierung Balken-, wird der Fortschritt durch das Video und ermöglicht es dem Benutzer schnell mit einem anderen Speicherort zu fortfahren. `VideoPlayer` können Sie entweder die Transport-Steuerelemente und Positionierung Leiste bereitgestellt, die von der Plattform (wie unten gezeigt), oder Sie können angeben, benutzerdefinierte Steuerelemente und Positionierung Balken. Hier ist das Programm, die unter iOS, Android und universellen Windows-Plattform ausgeführt wird:

[![Web-Video abspielen](web-videos-images/playwebvideo-small.png "Web Video abspielen")](web-videos-images/playwebvideo-large.png#lightbox "Web Video abspielen")

Natürlich können Sie das Telefon seitwärts für eine größere Ansicht deaktivieren.

Ein komplexer Videoplayer müsste einige zusätzlichen Funktionen, z. B. Lautstärkeregelung und einen Mechanismus, um das Video zu unterbrechen, wenn ein Telefonanruf über geht eine Möglichkeit, den Bildschirm aktiv halten, während der Wiedergabe.

Die folgende Reihe von Artikeln wird progressiv gezeigt, wie die Plattform-Renderer und unterstützende Klassen erstellt werden:

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[Erstellen der Plattform-video-Player](player-creation.md)

Jede Plattform erfordert eine `VideoPlayerRenderer` -Klasse, die erstellt und verwaltet ein video Player-Steuerelement, das von der Plattform unterstützt. In diesem Artikel wird die Struktur des Renderers, Klassen und wie der Player erstellt werden.

## <a name="playing-a-web-videoweb-videosmd"></a>[Wiedergeben eines Web-Videos](web-videos.md)

Wahrscheinlich ist die häufigste Ursache für ein video Player Videos über das Internet. In diesem Artikel wird beschrieben, wie ein Web-Video auf die verwiesen wird und als Quelle für den Videoplayer verwendet werden kann.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[Binden von Videoquellen an den player](source-bindings.md)

In diesem Artikel verwendet eine `ListView` auf eine Auflistung von Videos, die Wiedergabe vorhanden. Ein Programm zeigt, wie die Code-Behind-Datei den Videoplayer Videoquelle festgelegt, aber ein zweites Programm veranschaulicht die Verwendung von Daten, die Bindung zwischen der `ListView` und den Videoplayer.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[Laden die Anwendung Ressourcen videos](loading-resources.md)

Videos können als Ressourcen in die Plattformprojekte eingebettet werden. Dieser Artikel zeigt, wie diese Ressourcen zu speichern und Laden Sie diese später in das Programm von der Videoplayer wiedergegeben werden soll.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[Zugreifen auf die Videobibliothek des Geräts](accessing-library.md)

Wenn ein Video mit der Kamera des Geräts erstellt wird, wird die Videodatei in der Bildbibliothek des Geräts gespeichert. Dieser Artikel zeigt, wie auf das Gerät Image Auswahl, um das Video auswählen und dann mithilfe des Videoplayers wiedergeben.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[Benutzerdefinierte video Transport-Steuerelemente](custom-transport.md)

Obwohl der Videoplayer auf jeder Plattform über eigene Steuerungen Transport in Form von Schaltflächen zum Bereitstellen **wiedergeben** und **anhalten**, können Sie unterdrückt die Anzeige von über diese Schaltflächen und Ihre eigenen angeben. In diesem Artikel erfahren Sie, wie.

## <a name="custom-video-positioningcustom-positioningmd"></a>[Benutzerdefinierte video Positionierung](custom-positioning.md)

Jede der Videoplayer Plattform verfügt über eine Position-Leiste, die wird der Fortschritt des Videos und bietet die Möglichkeit, ist mein Vorsprung oder wieder zu einer bestimmten Position zu wechseln. In diesem Artikel wird veranschaulicht, wie Sie diese Leiste Position durch ein benutzerdefiniertes Steuerelement ersetzen können.





## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
