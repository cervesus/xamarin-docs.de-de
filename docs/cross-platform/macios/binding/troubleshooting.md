---
title: Bindungs Problembehandlung
description: In diesem Leitfaden wird beschrieben, was Sie tun sollten, wenn Sie Schwierigkeiten beim Binden einer Ziel-C-Bibliothek haben. Insbesondere werden fehlende Bindungen, Argument Ausnahmen bei der Übergabe von NULL an eine Bindung und das Melden von Fehlern erläutert.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 8194c369aa0e4f8bb17a1a162354b4f72c6aaa41
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725332"
---
# <a name="binding-troubleshooting"></a>Bindungs Problembehandlung

Einige Tipps zur Problembehandlung von Bindungen an macOS-APIs (früher als OS X bezeichnet) in xamarin. Mac.

## <a name="missing-bindings"></a>Fehlende Bindungen

Obwohl xamarin. Mac einen Großteil der Apple-APIs abdeckt, muss möglicherweise eine Apple-API aufgerufen werden, die noch keine Bindung besitzt. In anderen Fällen müssen Sie den Drittanbieter c/Ziel-c anrufen, der außerhalb des Bereichs der xamarin. Mac-Bindungen liegt.

Wenn Sie mit einer Apple-API arbeiten, besteht der erste Schritt darin, dass xamarin weiß, dass Sie einen Abschnitt der API erreichen, für den wir noch keine Abdeckung haben. Melden Sie [einen Fehler](#reporting-bugs) , der die fehlende API beachtet. Wir verwenden Berichte von Kunden, um zu priorisieren, bei welchen APIs wir als nächstes arbeiten. Wenn Sie darüber hinaus über eine Geschäfts-oder Unternehmenslizenz verfügen und diese fehlende Bindung ihren Fortschritt blockiert, befolgen Sie die Anweisungen [unter Support](https://visualstudio.microsoft.com/vs/support/) , um ein Ticket zu melden. Wir können keine Bindung Zusagen, aber in einigen Fällen können wir Ihnen eine Problem Umgehung zukommen lassen.

Nachdem Sie xamarin (falls zutreffend) über die fehlende Bindung benachrichtigt haben, müssen Sie den nächsten Schritt in Erwägung ziehen, ihn selbst zu binden. [Hier](~/cross-platform/macios/binding/overview.md) finden Sie eine vollständige Anleitung und eine inoffizielle [Dokumentation,](https://brendanzagaeski.appspot.com/xamarin/0002.html) mit der Sie Ziel-C-Bindungen per Hand Umpacken können. Wenn Sie eine C-API aufrufen, können Sie den C#P/Call-Mechanismus von verwenden. die Dokumentation finden Sie [hier](https://www.mono-project.com/docs/advanced/pinvoke/).

Wenn Sie sich selbst entscheiden, an der Bindung zu arbeiten, sollten Sie Bedenken, dass Fehler in der Bindung alle möglichen Abstürze in der nativen Laufzeit erzeugen können. Achten Sie insbesondere darauf, dass Ihre Signatur in C# mit der systemeigenen Signatur in der Anzahl von Argumenten und der Größe der einzelnen Argumente übereinstimmt. Wenn dies nicht der Fall ist, kann der Arbeitsspeicher und/oder der Stapel beschädigt werden, und Sie können sofort oder an einem beliebigen Punkt in der Zukunft abstürzen oder beschädigte Daten beschädigen.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Argument Ausnahmen beim Übergeben von NULL an eine Bindung

Obwohl xamarin funktioniert, um hochwertige und gut getestete Bindungen für die Apple-APIs bereitzustellen, können Fehler und Fehler manchmal behoben werden. Das häufigste Problem, das Sie bei weitem auftreten können, ist eine API, die `ArgumentNullException` auslöst, wenn Sie NULL übergeben, wenn die zugrunde liegende API `nil`akzeptiert. Die systemeigenen Header Dateien, die die API definieren, bieten oft nicht genügend Informationen darüber, welche APIs Nil akzeptieren und welche abstürzen, wenn Sie Sie übergeben.

Wenn in einem Fall, in dem die Übergabe von `null` eine `ArgumentNullException` auslöst, Sie jedoch den Eindruck haben, dass Sie funktionieren sollte, führen Sie die folgenden Schritte aus:

1. Überprüfen Sie in der Apple-Dokumentation und/oder den Beispielen, ob Sie sicherstellen können, dass die `nil`akzeptiert werden. Wenn Sie mit "Ziel-C" vertraut sind, können Sie ein kleines Testprogramm schreiben, um es zu überprüfen.
2. [Einen Fehler melden](#reporting-bugs).
3. Können Sie den Fehler umgehen? Wenn Sie die API nicht mit `null`aufrufen können, kann eine einfache NULL-Überprüfung der Aufrufe eine einfache Problem Umgehung sein.
4. Einige APIs erfordern jedoch das Übergeben von NULL, um einige Features zu deaktivieren oder zu deaktivieren. In diesen Fällen können Sie das Problem umgehen, indem Sie den assemblybrowser aufrufen (Weitere Informationen finden Sie unter [Suchen des C# Members für eine bestimmte Auswahl](~/mac/app-fundamentals/mac-apis.md#finding_selector)), Kopieren der Bindung und Entfernen der NULL-Überprüfung. Stellen Sie sicher, dass Sie einen Fehler melden (Schritt 2), da die kopierte Bindung keine Updates und Korrekturen erhält, die wir in xamarin. Mac erstellen, und dies sollte als kurzfristige Umgehung angesehen werden.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Melden von Fehlern

Ihr Feedback ist uns sehr wichtig. Wenn Sie Probleme mit xamarin. Mac finden:

- Überprüfen Sie die [Xamarin.Mac-Foren](https://forums.xamarin.com/categories/xamarin-mac).
- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues).
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden.

Fügen Sie möglichst viele der folgenden Informationen hinzu:

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich.
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt.
