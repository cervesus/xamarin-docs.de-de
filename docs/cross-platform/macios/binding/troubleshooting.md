---
title: Binden die Problembehandlung
description: Dieses Handbuch beschreibt, was zu tun, wenn Sie eine Objective-C-Bibliothek binden können. Insbesondere erläutert er fehlende Bindungen Argumentausnahmen beim Übergeben von null eine Bindung, und Melden von Fehlern.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854799"
---
# <a name="binding-troubleshooting"></a>Binden die Problembehandlung

Einige Tipps zur Problembehandlung von Bindungen in MacOS (früher als OS X)-APIs in Xamarin.Mac.

## <a name="missing-bindings"></a>Fehlender Bindungen

Während der Xamarin.Mac ein Großteil der Apple-APIs behandelt, manchmal müssen Sie möglicherweise eine Apple-API aufrufen, das eine Bindung keine noch. In anderen Fällen müssen Sie zum Aufrufen von Drittanbietern C/Objective-C, dass die It außerhalb des Bereichs der Xamarin.Mac Bindungen.

Wenn Sie mit einer Apple-API arbeiten, ist der erste Schritt, können Sie Xamarin wissen, dass Sie einen Teil der API erreichen, die wir Coverage noch keine besitzen. [Einen Fehler](#reporting-bugs) Beachten Sie die fehlenden-API. Wir verwenden die Berichte von Kunden, um welche APIs zu priorisieren, wir an weiter arbeiten. Darüber hinaus, wenn Sie eine Business oder Enterprise-Lizenz und dieses fehlen einer Bindung wird durch Ihren Fortschritt blockiert, auch anhand der Schritte unter [Unterstützung](http://xamarin.com/support) um ein Ticket. Wir können nicht versprechen, eine Bindung, wurde jedoch in einigen Fällen können Sie eine Arbeit rund um.

Nachdem Sie Xamarin (falls zutreffend) der fehlenden Bindung zu benachrichtigen, besteht der nächste Schritt, berücksichtigt er sich selbst gebunden. Wir haben eine vollständige Anleitung [hier](~/cross-platform/macios/binding/overview.md) und eine inoffizielle Dokumentation [hier](http://brendanzagaeski.appspot.com/xamarin/0002.html) für das Umschließen von Objective-C-Bindungen von Hand. Wenn Sie eine C-API aufrufen, können Sie # P/Invoke-Mechanismus, Dokumentation [hier](http://www.mono-project.com/docs/advanced/pinvoke/).

Wenn Sie sich entscheiden, funktioniert für die Bindung selbst, Bedenken Sie, dass alle möglichen interessanten Abstürzen in der nativen Runtime von Fehlern in der Bindung erstellt werden können. Insbesondere werden Sie sehr sorgfältig, dass Ihre Signatur in c# die native Signatur in Anzahl von Argumenten und die Größe der einzelnen Argumente entspricht. Bei unterlassen kann Speicher und/oder der Stapel beschädigt, und Sie stürzt ab sofort oder zu einem beliebigen Zeitpunkt in der Zukunft oder Daten beschädigt werden könnten.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Argumentausnahmen beim Übergeben von null zu einer Bindung

Xamarin funktioniert zum Bereitstellen von hoher Qualität und gut getestete Bindungen für die Apple-APIs, manchmal Programmierfehler und Fehlern Verteiler in. Bei weitem der am häufigsten auftretende Problem, die auftreten können, ist eine API auszulösen `ArgumentNullException` , wenn Sie übergeben NULL, wenn die zugrunde liegende API akzeptiert `nil`. Die native Headerdateien, definieren die API häufig sind nicht genügend Informationen bereitstellt, die APIs nil akzeptieren und die stürzt, wenn Sie ihn in übergeben.

Wenn ein Fall auftreten, übergeben `null` löst eine `ArgumentNullException` denken sollte es funktioniert, gehen Sie folgendermaßen vor:

1. Überprüfen Sie die Apple-Dokumentation und/oder Beispielen, um festzustellen, ob Sie Beweis, der diesen akzeptiert `nil`. Wenn Sie mit Objective-C vertraut sind, können Sie ein Programm kleinen Test durch, um sie zu überprüfen schreiben.
2. [Einen Fehler](#reporting-bugs).
3. Umgehen Sie können den Fehler? Wenn Sie vermeiden können, Aufrufen der API mit `null`, eine einfache null-Überprüfung, um die Aufrufe, kann eine einfache problemumgehung sein.
4. Allerdings erfordern einige APIs übergeben von Null ausschalten oder deaktivieren einige Funktionen. In diesen Fällen können Sie umgehen das Problem durch die Einbindung von der Assembly-Browser (finden Sie unter [suchen das C#-Element für einen angegebenen Selektor](~/mac/app-fundamentals/mac-apis.md#finding_selector)), kopieren die Bindung und die Überprüfung auf null löschen. Stellen Sie sicher, einen Fehler (Schritt 2) Wenn Sie dazu, wie die kopierte Bindung wird nicht empfangen, Updates und Korrekturen, die wir in Xamarin.Mac stellen, und dies eine kurzfristige problemumgehung berücksichtigt werden sollte.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Melden von Fehlern

Ihr Feedback ist uns wichtig. Wenn Sie Probleme mit Xamarin.Mac finden:

- Überprüfen Sie die [Xamarin.Mac-Foren](https://forums.xamarin.com/categories/mac).
- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues). 
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden. 

Fügen Sie möglichst viele der folgenden Informationen hinzu:

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich. 
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt. 

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen einer Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen einer Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
