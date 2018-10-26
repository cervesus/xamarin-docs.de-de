---
title: Verwalten von Fragmenten
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 107877d0e92d3a46101812b78bc0b414c0fbb320
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105466"
---
# <a name="managing-fragments"></a>Verwalten von Fragmenten

Damit Sie mit der Verwaltung von Fragmenten, Android bietet die `FragmentManager` Klasse. Jede Aktivität verfügt über eine Instanz von `Android.App.FragmentManager` , finden Sie das bzw. die Fragmenten dynamisch zu ändern. Jeder Satz dieser Änderungen wird als bezeichnet ein *Transaktion*, und erfolgt über eine der APIs in der Klasse enthaltenen `Android.App.FragmentTransation`, wobei für verwaltete der `FragmentManager`. Eine Aktivität kann es sich um eine Transaktion wie folgt starten:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Diese Änderungen auf die Fragmente werden ausgeführt, der `FragmentTransaction` Instanz mithilfe von Methoden wie z. B. `Add()`, `Remove(),` und `Replace().` die Änderungen werden dann mithilfe von angewendet `Commit()`. Die Änderungen in einer Transaktion werden nicht sofort ausgeführt.
Stattdessen sind sie so bald wie möglich auf UI-Thread von der Aktivität Ausführung geplant.

Das folgende Beispiel zeigt, wie Sie ein Fragment zu einem vorhandenen Container hinzuzufügen:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Wenn Sie nach dem Commit einer Transaktion wird `Activity.OnSaveInstanceState()` wird aufgerufen, wird eine Ausnahme ausgelöst werden. Dies liegt daran, wenn die Aktivität den Zustand speichert, Android auch den Status der gehosteten Fragmente speichert. Wenn nach diesem Punkt Fragment Transaktionen ein Commit ausgeführt werden, wird der Status dieser Transaktionen verloren, wenn es sich bei die Aktivität wiederhergestellt wird.

Es ist möglich, die Fragment-Transaktionen in der Aktivitäts zu speichern [BackStack](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) von einem Aufruf an `FragmentTransaction.AddToBackStack()`. Dadurch kann der Benutzer über rückwärts navigieren Fragment wird geändert, wenn die **wieder** gedrückt wird. Fragmente, die entfernt werden, ohne einen Aufruf dieser Methode zerstört, und ist nicht verfügbar, wenn der Benutzer über die Aktivität zurück navigiert.

Das folgende Beispiel zeigt, wie Sie mit der `AddToBackStack` Methode eine `FragmentTransaction` ein Fragment, bei der Beibehaltung des Status, der das erste Fragment im Rückwärtsstapel ersetzen:

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


## <a name="communicating-with-fragments"></a>Kommunikation mit Fragmenten

Die *FragmentManager* kennt, bis alle Fragmente, die einer Aktivität zugeordnet sind, und bietet zwei Methoden, um diese Fragmente zu finden:

-   **FindFragmentById** &ndash; diese Methode wird ein Fragment finden, indem Sie die ID, die in der Layoutdatei angegeben wurde oder die Container-ID verwenden, wenn das Fragment als Teil der Transaktion hinzugefügt wurde.

-   **FindFragmentByTag** &ndash; diese Methode wird verwendet, um ein Fragment zu finden, die ein Tag, die in der Layoutdatei bereitgestellt wurde, oder, die in einer Transaktion hinzugefügt wurde.

Referenz zu sowohl Fragmente und Aktivitäten der `FragmentManager`, sodass die gleichen Techniken verwendet werden, um zwischen diesen beiden Richtungen miteinander kommunizieren. Eine Anwendung möglicherweise finden einen Verweis Fragment mit einem dieser beiden Methoden, diesen Verweis in den entsprechenden Typ umgewandelt und dann direkt rufen Methoden für das Fragment. Der folgende Codeausschnitt enthält ein Beispiel:

Es ist auch möglich, für die Aktivität mit der `FragmentManager` Fragmenten finden:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>Kommunikation mit der Aktivität

Es ist möglich, dass ein Fragment, das Verwenden der `Fragment.Activity` Eigenschaft an, auf dem Host. Durch die Aktivität, die einen spezifischeren Typ umwandeln kann für eine Aktivität zum Aufrufen von Methoden und Eigenschaften auf dem Host, wie im folgenden Beispiel gezeigt:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
