---
title: Einführung in die Entwicklung mobiler Anwendungen
description: Dieser Artikel enthält eine Einführung in die Entwicklung mobiler Anwendungen, die Funktionsweisen von Xamarin und die Anwendungen, die ausgegeben werden.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: f3b1f5c11a02710de8d0ffd09741acb3017f5cb6
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780532"
---
# <a name="introduction-to-mobile-development"></a>Einführung in die Entwicklung mobiler Anwendungen

Das Erstellen mobiler Anwendungen kann so einfach sein: Öffnen Sie die IDE, schreiben Sie etwas, führen Sie einen kurzen Test durch, und senden Sie das Ergebnis dann an einen App Store. All dies ist an einem Nachmittag erledigt. Es kann jedoch sehr komplex sein, weil gründliche Vorabentwürfe, Nutzbarkeitstests, QA-Tests auf tausenden Geräten, ein vollständiger Beta-Lebenszyklus und anschließend die Bereitstellung mit einer Reihe verschiedener Methoden nötig ist.

Dieses Dokument soll Ihnen die Xamarin-Plattform näherbringen. Weitere Informationen über den *Prozess* zur Erstellung mobiler Anwendungen vom Entwurf bis zum Test finden Sie unter [Introduction to the Mobile Software Development Lifecycle (Einführung in die Entwicklung von Software für Mobilgeräte)](~/cross-platform/get-started/introduction-to-mobile-sdlc.md).

Lesen Sie sich bitte die [Systemanforderungen](~/cross-platform/get-started/requirements.md#macos-requirements) durch, um sicherzustellen, dass Sie Xamarin installieren können.

## <a name="introduction-to-xamarin"></a>Einführung in Xamarin

Wenn es um die Art der Erstellung von iOS- und Android-Anwendungen geht, denken viele Menschen, dass die nativen Sprachen, Objective-C, Swift und Java die einzigen Möglichkeiten sind. In den letzten Jahren hat sich jedoch ein komplett neues Ökosystem von Plattformen für die Erstellung mobiler Anwendungen entwickelt.

Xamarin ist in diesem Bereich einzigartig, da eine einzelne Sprache (C#, die Klassenbibliothek und die Runtime) angeboten wird, die auf den drei mobilen Plattformen iOS, Android und Windows (die native Sprache von Windows Phone ist C#) funktioniert. Dabei werden weiterhin native (nicht interpretierte) Anwendungen kompiliert, die selbst für anspruchsvolle Spiele leistungsfähig genug sind.

Jede dieser Plattformen verfügt über eine Reihe verschiedener Funktionen und jede unterscheidet sich in der Fähigkeit, native Anwendungen zu schreiben, d.h. Anwendungen, die zu einem nativen Code kompiliert werden und die problemlos mit dem zugrundeliegenden Java-Subsystem interagieren. Bei einigen Plattformen dürfen Apps z.B. in HTML und JavaScript erstellt werden, wohingegen sich andere auf niedriger Ebene befinden und nur C/C++-Code zulassen. Einige Plattformen verwenden nicht einmal das native Steuerungstoolkit.

Xamarin ist einzigartig, da in ihr alle Funktionen der nativen Plattformen kombiniert und um eine Reihe von eigenen leistungsstarken Funktionen ergänzt werden. Dazu gehören die Folgenden:

1.   **Vollständige Bindung für die zugrundeliegenden SDKs**: Xamarin enthält Bindungen für fast alle zugrundeliegenden Plattform SDKs unter iOS und Android. Zusätzlich dazu sind diese Bindungen stark typisiert. So bieten sie einfach Navigation, lassen sich leicht nutzen und sorgen während der Entwicklung für eine stabile Typüberprüfung zur Kompilierzeit und während der Entwicklung. Dies führt zu weniger Runtimefehlern und zu Anwendungen mit höherer Qualität.
1.   **Objective-C, Java, C und C++ Interop**: Xamarin stellt Funktionen zum direkten Aufrufen von Bibliotheken mit Objective-C, Java, C und C++ bereit, sodass Sie verschiedenen Drittanbietercode verwenden können, der bereits erstellt wurde. Dadurch können Sie die vorhandenen iOS- und Android-Bibliotheken verwenden, die in Objective-C, Java oder C/C++ geschrieben sind. Darüber hinaus bietet Xamarin Bindungsprojekte an, mit denen Sie ganz einfach native Objective-C- und Java-Bibliotheken binden können, indem Sie eine deklarative Syntax verwenden.
1.   **Moderne Sprachkonstrukte**: Xamarin-Anwendungen werden in C# geschrieben, eine modernen Sprache, die gegenüber Objective-C und Java bedeutende Verbesserungen beinhaltet, z.B. *dynamische Sprachfunktionen*, *funktionale Konstrukte* wie *Lambdas* und *LINQ*, Funktionen für die *parallele Programmierung*, anspruchsvolle *Generika* und viele mehr.
1.   //**Überzeugende Basisklassenbibliothek (Base Class Library, BCL)**: Xamarin-Anwendungen verwenden die .NET-BCL, eine große Sammlung von Klassen, die über umfassende und optimierte Funktionen verfügen, wie das leistungsstarke XML, Datenbanken, Serialisierung, E/A, die Zeichenfolge und die Netzwerkunterstützung, um nur ein paar zu nennen. Darüber hinaus kann der C#-Code für die Verwendung in einer Anwendung kompiliert werden, wodurch Sie Zugriff auf tausende Bibliotheken erhalten, mit denen Sie Aufgaben ausführen können, die in der BCL nicht vorhanden sind.
1.   **Moderne integrierte Entwicklungsumgebung (IDE)**: Xamarin verwendet Visual Studio für Mac unter Mac OS X und Visual Studio unter Windows. Beides sind moderne IDEs, die unter anderem folgende Funktionen enthalten: automatische Codevervollständigung, ein anspruchsvolles Projekt- und Projektmappenverwaltungssystem, eine umfassende Projektvorlagenbibliothek, eine integrierte Quellcodeverwaltung und viele mehr.
1.   **Mobiler plattformübergreifender Support**: Xamarin bietet einen ausgeklügelten, plattformübergreifenden Support für die drei mobilen Hauptplattformen iOS, Android und Windows Phone an. Bis zu 90 % des Codes einer Anwendung können wiederverwendet werden, und unsere Xamarin.Mobile-Bibliothek bietet eine einheitliche API, um von allen drei Plattformen aus auf allgemeine Ressourcen zuzugreifen. Dadurch können Entwickler, die für die drei beliebtesten mobilen Plattformen entwickeln, signifikant Entwicklungskosten einsparen und die Anwendungen schneller auf den Markt bringen.


Durch die leistungsstarken und umfassenden Funktionen von Xamarin wird eine Lücke für Anwendungsentwickler ausgefüllt, die eine moderne Sprache und Plattform verwenden möchten, um plattformübergreifende mobile Anwendungen zu entwickeln.


> [!NOTE]
> In der Reihe „Erste Schritte“ werden die ersten Schritte beim Erstellen von iOS- und Android-Anwendungen beleuchtet. Microsoft stellt Informationen zur [UWP-Entwicklung (Universelle Windows-Plattform)](https://docs.microsoft.com/windows/uwp/develop/) für Tablets und Desktop-PCs zur Verfügung. Weitere Informationen zur plattformübergreifenden Entwicklung mit Xamarin (einschließlich UWP-Anwendungen für Windows) finden Sie in der Anleitung zum [Erstellen plattformübergreifender Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).



## <a name="how-does-xamarin-work"></a>Wie funktioniert Xamarin?

Xamarin bietet zwei kommerzielle Produkte an: Xamarin.iOS und Xamarin.Android Beide bauen auf *Mono* auf, einer Open Source-Version von .NET Framework basierend auf den veröffentlichten Standards von .NET ECMA International. Mono existiert bereits fast so lange wie das .NET Framework und kann auf nahezu jeder erdenklichen Plattform ausgeführt werden, einschließlich Linux, Unix, FreeBSD und Mac OS X.

Unter iOS kompiliert der *Ahead-of-Time-Compiler* (*AOT*) die Xamarin.iOS-Anwendungen direkt in nativen ARM-Assemblycode. Unter Android kompiliert der Compiler von Xamarin in die *Zwischensprache* (*IL*), welche dann beim Start der Anwendung *Just-In-Time* (*JIT*) in eine native Assembly kompiliert wird.

In beiden Fällen verwenden die Xamarin-Anwendungen eine Runtime, die automatisch Aufgaben handhabt, wie die Speicherbelegung, die automatische Speicherbereinigung, die zugrundeliegende Technologie der Plattform-Interop usw.



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll und Mono.Android.dll

Xamarin-Anwendungen werden mit einem Teil der .NET-BCL erstellt, die als das mobile Xamarin-Profil bekannt ist. Dieses Profil wurde speziell für mobile Anwendungen erstellt und in „MonoTouch.dll“ und „Mono.Android.dll“ (jeweils für iOS und Android) verpackt. Dies entspricht der Art, wie Silverlight- und Moonlight-Anwendungen mit dem Silverlight/Moonlight .NET-Profil erstellt werden. In der Tat gleicht das mobile Xamarin-Profil dem Profil von Silverlight 4.0. Zahlreiche BCL-Klassen wurden wieder hinzugefügt.

Eine vollständige Lister der verfügbaren Assemblys und Klassen finden Sie unter [Xamarin.iOS-Assemblyliste](~/cross-platform/internals/available-assemblies.md) und [Xamarin.Android-Assemblyliste](~/cross-platform/internals/available-assemblies.md).

Zusätzlich zur BCL enthalten diese .dlls Wrapper für nahezu das gesamte iOS SDK und Android SDK, sodass die zugrundeliegenden SDK-APIs direkt aus C# aufgerufen werden können.



### <a name="application-output"></a>Anwendungsausgabe

Wenn Xamarin-Anwendungen kompiliert werden, entsteht ein Anwendungspaket – entweder eine APP-Datei unter iOS oder eine APK-Datei unter Android. Diese Dateien lassen sich nicht von den Anwendungspaketen unterscheiden, die mit den Standard-IDEs der Plattform erstellt wurden, und sie können auf die gleiche Art und Weise bereitgestellt werden.



## <a name="getting-started"></a>Erste Schritte

Nun, da Sie ein wenig über die Funktionsweise von Xamarin wissen, wird es Zeit für weitere Schritte.

Beginnen Sie als Nächstes mithilfe einer dieser Anleitungen mit der Erstellung einer App:

* [**Hallo, iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Hallo, iOS")


* [**Hallo, Android**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hallo, Android")


* [**Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>Zusammenfassung

Dieses Dokument dient lediglich der Einführung in die Xamarin-Plattform. Der Spaß beginnt erst richtig, wenn Sie Ihre erste App erfolgreich erstellt haben. Sehen Sie sich dazu die Anleitungen [Hallo, iOS](~/ios/get-started/hello-ios/index.md), [Hallo, Android](~/android/get-started/hello-android/index.md) und [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) an.


## <a name="related-links"></a>Verwandte Links

- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hallo, Android](~/android/get-started/hello-android/index.md)
