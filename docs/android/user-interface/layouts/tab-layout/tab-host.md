---
title: Registerkartenlayout mit TabHost
description: Dieser Artikel bietet eine grobe Übersicht der der TabHost, eine ältere API verwendet, um in einer Anwendung Xamarin.Android im Registerkartenformat Layouts zu erstellen.
ms.prod: xamarin
ms.assetid: 77B890A4-27A6-41DF-81BA-22C6116A8FB2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: ae5b9020e08575bcd453703f3df14f63b288d2f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="tab-layout-with-tabhost"></a>Registerkartenlayout mit TabHost

_Dieser Artikel bietet eine grobe Übersicht der der TabHost, eine ältere API verwendet, um in einer Anwendung Xamarin.Android im Registerkartenformat Layouts zu erstellen._


## <a name="overview"></a>Übersicht

> [!NOTE]
> `TabHost` ist eine alte API, die von Google als veraltet markiert wurden. Entwicklern wird empfohlen, die im Registerformat mithilfe Anwendungen erstellen, die [ActionBar](~/android/user-interface/controls/action-bar.md). Die `ActionBar` in allen Android-Version verfügbar ist. Es wurde erstmals in Android 3.0 (API-Ebene 11) und portiert wurde wieder Android 2.2 (API-Ebene 8) und Android 2.3 (API-Ebene 10) in der [V7 AppCompat Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat), steht auf Xamarin.Android über die [Xamarin Android-Unterstützungsbibliothek – V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) Paket.

Die `TabHost` ist die ältere, ursprünglichen-API für den Benutzer im Registerkartenformat InterfacesIt erstellen für Xamarin.Android Anwendungen am besten geeignet, Android 2.2 und 2.3 Android unterstützt müssen und kann nicht **ActionBarSherlock**.
Die folgenden fünf Komponenten sind mit allen Beteiligten die `TabHost` API:

-  **TabActivity** &ndash; Dies ist eine spezielle Sicht, die fungiert als Container für eine `TabHost` (siehe unten).

-  **TabHost** &ndash; Dies ist ein Container für die Benutzeroberfläche im Registerkartenformat. Hostet zwei Kinder: eine Liste von registerkartenbezeichnungen und eine `FrameLayout` , die die Registerkarteninhalts anzeigt.

-  **TabWidget** &ndash; dieses Steuerelement zeigt eine Liste von registerkartenbezeichnungen, eine für jede Registerkarte in der `TabHost` . Jede Registerkarte in einem `TabHost` werden durch dargestellt eine `TabHost.TabSpec` Objekt. Dieses Objekt enthält die Metadaten für jede Registerkarte. Bei der Auswahl einer Registerkarte für die `TabHost` antwortet auf die Auswahl, indem Sie die entsprechende Registerkarte anzeigen.

-  **FrameLayout** &ndash; eine Benutzeroberfläche mit Registerkarten benötigen eine `FrameLayout` innerhalb eines enthalten eine `TabHost`. Es dient zur Anzeige der Inhalte für die Registerkarte.

-  **Aktivitäten oder Ansichten** &ndash; Wenn eine Registerkarte ausgewählt wird, zeigt er eine Aktivität oder einer Sicht in der `FrameLayout`.

Das folgende Diagramm zeigt, wie alle diese Komponenten miteinander in Beziehung stehen:

![Diagramm zur Veranschaulichung der Frame-Layout in TabWidget in TabHost](tab-host-images/image03.png)

Die Registerkarteninhalt möglicherweise Aktivitäten oder Ansichten. Ansichten sind relativ einfache und einfache, aber möglicherweise viele nicht verknüpfte Code co-Habitating in der Aktivität. Dies führt in eine schlechte Trennung von Anliegen und eine sehr großer-Klasse, die schwer zu verwalten ist. Im Gegensatz dazu Aktivitäten Systemressourcen erfordern jedoch ein modularer Ansatz mit der Logik für jede Registerkarte in seinem eigenen distinct Klasse gekapselte ermöglichen.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde erläutert, die auf hoher Ebenen Komponenten der älteren `TabHost` -API für Android und wie sie alle miteinander in Beziehung stehen.



## <a name="related-links"></a>Verwandte Links

- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [NuGet-Paket für Android, unterstützen Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 Appcompat-Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
