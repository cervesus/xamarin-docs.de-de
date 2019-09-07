---
title: Verwalten von Fragmenten
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: f5baf8b46571c9b528fcc666a3f1f4530f18ee07
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761507"
---
# <a name="managing-fragments"></a>Verwalten von Fragmenten

Zur Unterstützung bei der Verwaltung von Fragmenten stellt `FragmentManager` Android die-Klasse bereit. Jede Aktivität verfügt über eine Instanz `Android.App.FragmentManager` von, mit der die Fragmente gefunden oder dynamisch geändert werden. Jeder Satz dieser Änderungen wird als *Transaktion*bezeichnet und wird mithilfe einer der in der-Klasse `Android.App.FragmentTransation`enthaltenen APIs ausgeführt, die von verwaltet `FragmentManager`wird. Eine Aktivität kann eine Transaktion wie die folgende starten:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Diese Änderungen an den Fragmenten werden mithilfe von Methoden `FragmentTransaction` `Add()`wie in der-Instanz ausgeführt, `Remove(),` und `Replace().` die Änderungen werden dann mithilfe `Commit()`von angewendet. Die Änderungen in einer Transaktion werden nicht sofort ausgeführt.
Stattdessen wird geplant, dass Sie so bald wie möglich im UI-Thread der Aktivität ausgeführt werden.

Im folgenden Beispiel wird gezeigt, wie ein Fragment einem vorhandenen Container hinzugefügt wird:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Wenn ein Commit für eine Transaktion `Activity.OnSaveInstanceState()` ausgeführt wird, nachdem aufgerufen wurde, wird eine Ausnahme ausgelöst. Dies liegt daran, dass Android den Status aller gehosteten Fragmente speichert, wenn der Zustand der Aktivität gespeichert wird. Wenn nach diesem Punkt ein Commit für eine fragmenttransaktion ausgeführt wird, geht der Status dieser Transaktionen verloren, wenn die Aktivität wieder hergestellt wird.

Es ist möglich, die fragmenttransaktionen im [BackStack](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) der Aktivität zu speichern, indem Sie aufrufen `FragmentTransaction.AddToBackStack()`. Dadurch kann der Benutzer rückwärts durch fragmentänderungen navigieren, wenn die Schaltfläche " **zurück** " gedrückt wird. Ohne einen aufzurufenden Vorgang werden Fragmente gelöscht, die entfernt werden und nicht verfügbar sind, wenn der Benutzer durch die Aktivität zurück navigiert.

Im folgenden Beispiel wird gezeigt, wie die `AddToBackStack` -Methode `FragmentTransaction` eines verwendet wird, um ein Fragment zu ersetzen, während der Zustand des ersten Fragments im BackStack beibehalten wird:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```

## <a name="communicating-with-fragments"></a>Kommunizieren mit Fragmenten

Der *fragmentmanager* kennt alle Fragmente, die an eine Aktivität angefügt sind, und stellt zwei Methoden zur Verfügung, mit denen Sie diese Fragmente finden können:

- **Findfragmentbyid** &ndash; Diese Methode findet ein Fragment, indem die ID verwendet wird, die in der Layoutdatei angegeben wurde, oder die Container-ID, als das Fragment als Teil einer Transaktion hinzugefügt wurde.

- **Findfragmentbytag** &ndash; Diese Methode wird verwendet, um ein Fragment mit einem Tag zu suchen, das in der Layoutdatei bereitgestellt wurde oder das in einer Transaktion hinzugefügt wurde.

Sowohl Fragmente als auch Aktivitäten verweisen `FragmentManager`auf das, sodass dieselben Techniken verwendet werden, um zwischen Ihnen zu kommunizieren. Eine Anwendung findet möglicherweise ein Verweis Fragment mithilfe einer dieser beiden Methoden, wandelt den Verweis in den entsprechenden Typ um und ruft dann Methoden für das Fragment direkt auf. Der folgende Code Ausschnitt enthält ein Beispiel:

Es ist auch möglich, dass die-Aktivität das `FragmentManager` verwendet, um Fragmente zu finden:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>Kommunizieren mit der Aktivität

Ein Fragment kann die `Fragment.Activity` -Eigenschaft verwenden, um auf den Host zu verweisen. Durch Umwandeln der-Aktivität in einen spezifischeren Typ kann eine Aktivität Methoden und Eigenschaften auf Ihrem Host aufzurufen, wie im folgenden Beispiel gezeigt:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
