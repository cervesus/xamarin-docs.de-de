---
title: Binden die Problembehandlung
description: Dieses Handbuch beschreibt, was zu tun, wenn Sie Probleme, die eine Bibliothek für Objective-C-Bindung haben.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: 7ea3e3802ec2e0baf0fe8355a41e806bacabc9ac
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="binding-troubleshooting"></a>Binden die Problembehandlung

Einige Tipps zur Problembehandlung bei Bindungen mit MacOS (ehemals OS X)-APIs in Xamarin.Mac.

## <a name="missing-bindings"></a>Fehlender Bindungen

Während Xamarin.Mac Großteil der Apple-APIs behandelt, in einigen Fällen müssen Sie möglicherweise eine Apple-API aufrufen, die eine Bindung nicht noch. In anderen Fällen müssen Sie zum Aufrufen von Drittanbietern C/Objective-C, dass die It außerhalb des Bereichs der Xamarin.Mac Bindungen.

Wenn Sie über eine Apple-API arbeiten, ist der erste Schritt, lassen Xamarin, die wissen, dass Sie einen Abschnitt der API aktiviert sind, die wir Coverage noch nicht. [Einen Fehler](#reporting-bugs) Beachten Sie die fehlende-API. Wir verwenden Berichte von Kunden, um die APIs priorisieren As wir auf Weiter arbeiten. Darüber hinaus, wenn Sie eine Business oder Enterprise-Lizenz verfügen, und diese mangelnde einer Bindung wird den Fortschritt blockieren, auch entsprechend den Anweisungen unter [Unterstützung](http://xamarin.com/support) in die Datei ein Ticket. Wir können nicht versprechen eine Bindung, aber in einigen Fällen wir können Sie eine Arbeitsaufgabe umgehen.

Nachdem Sie Xamarin (falls zutreffend) der fehlenden Bindung zu benachrichtigen, besteht der nächste Schritt zu berücksichtigen, binden es selbst. Wir haben eine vollständige Anleitung [hier](~/cross-platform/macios/binding/overview.md) und einige inoffiziellen Dokumentation [hier](http://brendanzagaeski.appspot.com/xamarin/0002.html) Objective-C-Bindungen von Hand einzuschließen. Wenn Sie eine C#-API aufrufen, können Sie # P/Invoke-Mechanismus, Dokumentation ist [hier](http://www.mono-project.com/docs/advanced/pinvoke/).

Wenn Sie sich dazu entschließen, arbeiten für die Bindung selbst, Bedenken Sie, dass alle möglichen interessante Abstürze (crashes) in die native Common Language Runtime Fehlern in der Bindung erstellt werden können. Insbesondere werden Sie sehr vorsichtig vor, dass Ihre Signatur in c# die systemeigene Signatur Anzahl von Argumenten und die Größe jedes Arguments übereinstimmt. Bei unterlassen kann Speicher und/oder den Stapel beschädigt und konnte stürzt ab sofort oder an einem beliebigen Punkt in der Zukunft oder Daten beschädigt werden.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Argumentausnahmen beim Übergeben von null, einer Bindung

Während Xamarin arbeitet daran, geben Sie hohe Qualität und gut getestete Bindungen für die Apple-APIs, manchmal Fehler und Fehler Slip in. Weitem der am häufigsten auftretende Problem, das möglicherweise ist eine API auslösen `ArgumentNullException` , wenn Sie übergeben auf Null, wenn die zugrunde liegenden API akzeptiert `nil`. Die systemeigene Headerdateien, definieren die API häufig sind nicht genügend Informationen bereit, auf dem APIs akzeptieren "Nil" und dem stürzt, wenn Sie ihn in übergeben.

Wenn Sie ein Fall auftreten, übergeben `null` löst ein `ArgumentNullException` denken sollten sie arbeiten, gehen Sie folgendermaßen vor:

1. Überprüfen Sie den Apple-Dokumentation bzw. der Beispiele, um festzustellen, ob Sie Proof finden können, der diesen akzeptiert `nil`. Wenn Sie mit der Objective-C vertraut sind, können Sie ein kleines Testprogramm überprüfen schreiben.
2. [Einen Fehler](#reporting-bugs).
3. Umgehen Sie können den Fehler? Wenn Sie vermeiden können Aufrufen der API mit `null`, um die Aufrufe eine einfachen null-Prüfung kann einfach umgehen.
4. Allerdings erfordern einige APIs übergeben von Null zu deaktivieren, oder deaktivieren einige Funktionen. In diesen Fällen können Sie umgehen das Problem durch das Kombinieren von den Assembly-Browser (finden Sie unter [suchen das C#-Element für einen angegebenen Selektor](~/mac/app-fundamentals/mac-apis.md#finding_selector)), kopieren die Bindung, und entfernen die null-Prüfung. Stellen Sie sicher, einen Fehler (Schritt 2) Wenn Sie dazu, wie eine kopierte Bindung wird nicht empfangen, Updates und Korrekturen, die wir im Xamarin.Mac vornehmen, und dies ein kurzfristiger umgehen berücksichtigt werden sollte.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Melden von Fehlern

Ihr Feedback ist uns wichtig. Wenn Sie Probleme bei Xamarin.Mac finden:

- Überprüfen Sie die [Xamarin.Mac-Foren](https://forums.xamarin.com/categories/mac).
- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues). 
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden. 

Fügen Sie möglichst viele der folgenden Informationen hinzu:

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich. 
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt. 
