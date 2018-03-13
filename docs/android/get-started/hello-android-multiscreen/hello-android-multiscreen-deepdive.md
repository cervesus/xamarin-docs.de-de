---
title: "Hallo, Android-Multiscreen: Ausführliche Erläuterungen"
description: "In diesem zweiteiligen Leitfaden wird die grundlegende Phoneword-Anwendung erweitert, die Sie im Leitfaden „Hallo, Android“ erstellt haben, um einen zweiten Bildschirm behandeln zu können. Dabei werden die grundlegenden Bausteine für die Android-Anwendung eingeführt. Es ist ein tieferer Einblick in die Android-Architektur enthalten, damit Sie sich ein besseres Bild der Android-Anwendungsstruktur und -Funktionen machen können."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: acced081daa9416c5c8dcf90f769aaacd584ec9a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="hello-android-multiscreen-deep-dive"></a>Hallo, Android-Multiscreen: Ausführliche Erläuterungen

_In diesem zweiteiligen Leitfaden wird die grundlegende Phoneword-Anwendung erweitert, die Sie im Leitfaden „Hallo, Android“ erstellt haben, um einen zweiten Bildschirm behandeln zu können. Dabei werden die grundlegenden Bausteine für die Android-Anwendung eingeführt. Es ist ein tieferer Einblick in die Android-Architektur enthalten, damit Sie sich ein besseres Bild der Android-Anwendungsstruktur und -Funktionen machen können._

## <a name="hello-android-multiscreen-deep-dive"></a>Hallo, Android-Multiscreen: Ausführliche Erläuterungen

Im [Schnellstart des Hallo, Android-Multiscreens](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md) haben Sie Ihre erste Xamarin.Android-Multiscreen-Anwendung erstellt und ausgeführt.
Nun ist es an der Zeit, ein tieferes Verständnis für die Navigation und Architektur von Android-Anwendungen zu entwickeln, sodass Sie komplexere Anwendungen erstellen können.

In diesem Handbuch werden Sie mit der Einführung der *Anwendungsbausteine* von Android die erweiterte Android-Architektur kennenlernen. Die Android-Navigation mit *Intents* wird erläutert und Android-Hardware-Navigationsoptionen werden untersucht. Neue Erweiterungen der Phoneword-Anwendung werden zerlegt, wenn Sie eine ganzheitliche Sicht der Anwendungsbeziehung mit dem Betriebssystem und anderen Anwendungen entwickeln.


## <a name="android-architecture-basics"></a>Grundlagen der Android-Architektur

In [Hallo, Android: Ausführliche Erläuterungen](~/android/get-started/hello-android/hello-android-deepdive.md) haben Sie gelernt, dass Android-Anwendungen eindeutige Programme sind, weil sie nicht über einen einzigen Einstiegspunkt verfügen. Stattdessen startet das Betriebssystem (oder eine andere Anwendung) eine der registrierten Aktivitäten für die Anwendung, wodurch wiederum der Prozess für die Anwendung gestartet wird. Diese ausführlichen Erläuterungen der Android-Architektur erweitert Ihr Verständnis der Bauweise von Android-Anwendungen durch die Einführung der Android-Anwendungsbausteine und deren Funktionen.


### <a name="android-application-blocks"></a>Android-Anwendungsbausteine

Eine Android-Anwendung besteht aus einer Auflistung von speziellen Android-Klassen namens *Anwendungsblöcke*, die mit einer beliebigen Anzahl von App-Ressourcen – Bilder, Themen, Hilfsklassen usw. &ndash; gebündelt werden. Diese werden von einer XML-Datei namens *Android-Manifest* koordiniert.

Anwendungsblöcke bilden das Rückgrat von Android-Anwendungen, da mit ihnen Aktionen ausgeführt werden können, die Sie normalerweise mit einer normalen Klasse nicht bewältigen könnten. Die beiden wichtigsten sind _Aktivitäten_ und _Dienste_:

-   **Aktivität** &ndash; Eine Aktivität entspricht einem Bildschirm mit einer Benutzeroberfläche und gleicht konzeptionell einer Webseite in einer Webanwendung. Beispielsweise wäre der Anmeldebildschirm in einer Newsfeedanwendung die erste Aktivität, die scrollbare Liste der Nachrichtenelemente wäre eine andere Aktivität und die Detailseite für jedes Element wäre eine dritte. Weitere Informationen zu Aktivitäten finden Sie im Handbuch [Activity Lifecycle (Aktivitätslebenszyklus)](~/android/app-fundamentals/activity-lifecycle/index.md).

-   **Dienst** &ndash; Android-Dienste unterstützen die Aktivitäten, indem sie Aufgaben mit langer Laufzeit übernehmen und diese im Hintergrund ausführen. Dienste verfügen nicht über eine Benutzeroberfläche und werden verwendet, um Aufgaben zu behandeln, die nicht an Bildschirme gekoppelt sind &ndash; z.B. die Wiedergabe eines Musiktitels im Hintergrund oder das Hochladen von Fotos auf einen Server. Weitere Informationen zu Diensten finden Sie in den Handbüchern [Creating Services (Erstellen von Diensten)](~/android/app-fundamentals/services/index.md) und [Android Services (Android-Dienste)](~/android/app-fundamentals/services/index.md).


Eine Android-Anwendung verwendet möglicherweise nicht alle Typen von Bausteinen und verfügt oft über mehrere Bausteine eines bestimmten Typs. Die Phoneword-Anwendung aus dem [Hallo, Android-Schnellstart](~/android/get-started/hello-android/hello-android-quickstart.md) besteht beispielsweise aus nur einer Aktivität (Bildschirm) und einigen Ressourcendateien. Eine einfache Musik-Player-App verfügt möglicherweise über mehrere Aktivitäten und einen Dienst für die Wiedergabe von Musik, wenn die App im Hintergrund ausgeführt wird.

### <a name="intents"></a>Intents

Ein anderes grundlegendes Konzept in Android-Anwendungen nennt sich *Intent*.
Android basiert auf dem *Prinzip der minimalen Rechtegewährung* &ndash; Anwendungen haben nur Zugriff auf die Bausteine, die sie zur Arbeit benötigen, und beschränkten Zugriff auf die Blöcke, die Teil des Betriebssystems oder anderer Anwendungen sind. Bausteine sind auf ähnliche Weise *lose verbunden* &ndash; sie sind dafür entwickelt, nur wenig über andere Bausteine zu wissen und eingeschränkten Zugriff auf sie zu haben (auch Bausteine, die Teil derselben Anwendung sind).

Anwendungsbausteine senden zur Kommunikation asynchrone Nachrichten, so genannte *Intents*, hin und her. Intents enthalten Informationen zum empfangenden Block und manchmal einige Daten. Ein Intent, der von einer App-Komponente gesendet wird, führt dazu, das etwas in einer anderen App-Komponente geschieht, wodurch die beiden Komponenten der App verbunden werden, und es ihnen ermöglicht wird, zu kommunizieren. Durch das Hin-und-her-Senden von Intents können Bausteine komplexe Aktionen koordinieren. Dazu gehören beispielsweise der Start der Kamera-App zum Erstellen und Speichern, die Erfassung von Speicherortinformationen oder die Navigation von einem Bildschirm zum nächsten.


### <a name="androidmanifestxml"></a>AndroidManifest.XML

Beim Hinzufügen eines Bausteins zur Anwendung wird er mit einer speziellen XML-Datei namens **Android-Manifest** registriert. Das Manifest verfolgt alle Anwendungsbausteine in einer Anwendung sowie die Versionsanforderungen, Berechtigungen und verknüpfte Bibliotheken &ndash; alles, was das Betriebssystem zum Ausführen Ihrer Anwendung kennen muss. Das **Android-Manifest** arbeitet auch mit Aktivitäten und Intents zusammen, um zu steuern, welche Aktionen für eine bestimmte Aktivität geeignet sind. Die erweiterten Funktionen des Android-Manifests finden Sie im Handbuch [Working with the Android Manifest (Arbeiten mit dem Android-Manifest)](~/android/platform/android-manifest.md).

In der Version für einen Bildschirm der Phoneword-Anwendung wurden nur eine Aktivität, ein Intent, und die `AndroidManifest.xml`, zusammen mit zusätzlichen Ressourcen wie Symbolen, verwendet. In der Version mit mehreren Bildschirm von Phoneword wurde eine zusätzliche Aktivität hinzugefügt. Sie wurde von der ersten Aktivität mit einer Intent-Absicht gestartet. Im nächsten Abschnitt wird erklärt, wie Intents bei der Erstellung der Navigation in Android-Anwendungen helfen.

## <a name="android-navigation"></a>Android-Navigation

Intents wurden für die Navigation zwischen den Bildschirmen verwendet. Es ist Zeit, sich diesen Code anzusehen, um herauszufinden, wie Intents arbeiten und ihrer Rolle in der Android-Navigation zu verstehen.


### <a name="launching-a-second-activity-with-an-intent"></a>Starten einer zweiten Aktivität mit Intent

In der Phoneword-Anwendung wurde ein Intent verwendet, um einen zweiten Bildschirm (Aktivität) zu starten. Starten Sie mit dem Erstellen eines Intents, übergeben Sie den aktuellen *Kontext* (`this`, bezieht sich auf den aktuellen **Kontext**) und den Typ der Anwendungsbausteine, den Sie suchen (`TranslationHistoryActivity`):

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

Der **Kontext** ist eine Schnittstelle für globale Informationen über die Anwendungsumgebung &ndash; Er lässt neu erstellte Objekte wissen, was mit der Anwendung geschieht. Wenn Sie sich einen Intent als Nachricht vorstellen, stellen Sie den Namen des Empfängers der Nachricht (`TranslationHistoryActivity`) und die Adresse des Empfängers (`Context`) bereit.

Android bietet eine Option zum Anfügen einfacher Daten an einen Intent (komplexe Daten werden anders behandelt). Im Phoneword-Beispiel wird `PutStringArrayExtra` verwendet, um eine Liste von Telefonnummern zum Intent hinzuzufügen, und `StartActivity` wird auf dem Empfänger des Intent aufgerufen. Der fertige Code sieht wie folgt aus:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```


## <a name="additional-concepts-introduced-in-phoneword"></a>Zusätzliche in Phoneword eingeführte Konzepte

Die Phoneword-Anwendung enthält weitere Konzepte, die jedoch nicht in diesem Leitfaden behandelt werden. Dies sind z.B. folgende Konzepte:

**Zeichenfolgenressourcen**: In der Phoneword-Anwendung wurde der `TranslationHistoryButton`-Text auf `"@string/translationHistory"` festgelegt. Die `@string`-Syntax bedeutet, dass der Wert der Zeichenfolge, in der _Zeichenfolgenressourcendatei_ **Strings.xml** gespeichert ist. Der folgende Wert für die `translationHistory`-Zeichenfolge wurde zu **Strings.xml** hinzugefügt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

Weitere Informationen zu den Zeichenfolgenressourcen und anderen Android-Ressourcen finden Sie im [Android-Ressourcenhandbuch](~/android/app-fundamentals/resources-in-android/index.md).

**Listenansichten und Adapter**: _ListView_ ist eine UI-Komponente, die eine einfache Möglichkeit zum Darstellen einer scrollbaren Liste mit Zeilen bereitstellt. Eine `ListView`-Instanz erfordert einen _Adapter_, um sie mit in Zeilenansichten enthaltenen Daten zu füllen. Die folgende Codezeile wurde verwendet, um die Benutzeroberfläche von `TranslationHistoryActivity` aufzufüllen:

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

Listenansichten und Adapter sind nicht Gegenstand dieses Dokuments, aber sie werden sehr umfassend im Handbuch [ListViews and Adapters (ListViews und Adapter)](~/android/user-interface/layouts/list-view/index.md) behandelt.
[Auffüllen von ListView mit Daten](~/android/user-interface/layouts/list-view/populating.md) behandelt insbesondere die integrierten `ListActivity`- und `ArrayAdapter`-Klassen zum Erstellen und Auffüllen einer `ListView` ohne die Definition eines benutzerdefinierten Layouts, wie im Beispiel Phoneword erfolgt.


## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch, Sie haben die erste Android-Multiscreen-Anwendung fertiggestellt! In diesem Handbuch wurden *Android-Anwendungsbausteine* und *Intents* eingeführt, und sie wurden zum Erstellen einer Android-Multiscreen-Anwendung verwendet. Mit diesem soliden Grundwissen können Sie jetzt mit der Entwicklung Ihrer eigenen Xamarin.Android-Anwendungen beginnen.

Als Nächstes lernen Sie das Erstellen von plattformübergreifenden Anwendungen mit Xamarin im Handbuch [Building Cross-Platform Applications guides (Erstellen von plattformübergreifenden Anwendungen)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
