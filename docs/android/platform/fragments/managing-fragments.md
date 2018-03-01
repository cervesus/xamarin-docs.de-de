---
title: Verwalten von Fragmenten
ms.topic: article
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 33f8b44a1213df4a8a1c75e2df66cc0b007a5749
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="managing-fragments"></a>Verwalten von Fragmenten

Zur Unterstützung bei der Verwaltung von Fragmenten Android umfasst die `FragmentManager` Klasse. Jede Aktivität verfügt über eine Instanz von `Android.App.FragmentManager` , suchen bzw. die Fragmenten dynamisch zu ändern. Jeder Satz dieser Änderungen wird als bezeichnet eine *Transaktion*, und erfolgt über eine der APIs, die in der Klasse enthaltenen `Android.App.FragmentTransation`, wobei für verwaltete der `FragmentManager`. Eine Aktivität startet eine Transaktion wie folgt:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Diese Änderungen an die Fragmente werden ausgeführt, der `FragmentTransaction` Instanz mithilfe von Methoden wie z. B. `Add()`, `Remove(),` und `Replace().` die Änderungen werden dann mit übernommen `Commit()`. Die Änderungen in einer Transaktion werden nicht sofort ausgeführt.
Stattdessen werden ihre Ausführung geplant, auf die Aktivität UI-Thread so bald wie möglich.

Das folgende Beispiel zeigt, wie ein Fragment zu einem vorhandenen Container hinzufügen:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Wenn eine Transaktion ein Commit ausgeführt wurde, nach dem `Activity.OnSaveInstanceState()` wird aufgerufen, wird eine Ausnahme ausgelöst. Dies liegt daran, dass wenn die Aktivität den Status speichert, Android auch den Status der gehosteten Fragmente speichert. Wenn nach diesem Zeitpunkt Fragment Transaktionen ein Commit ausgeführt werden, verloren der Status von diesen Transaktionen, wenn die Aktivität wiederhergestellt wird.

Es ist möglich, das Fragment Transaktionen an die Aktivität zu speichern [rückwärts-](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) von einem Aufruf an `FragmentTransaction.AddToBackStack()`. Dadurch wird den Benutzer über rückwärts navigieren Fragment wird geändert, wenn die **wieder** gedrückt wird. Ohne einen Aufruf dieser Methode Fragmente, die entfernt werden, werden zerstört und nicht verfügbar, wenn der Benutzer über die Aktivität zurück navigiert.

Das folgende Beispiel zeigt, wie Sie die `AddToBackStack` Methode von einer `FragmentTransaction` ein Fragment, ersetzen während der Beibehaltung des Status, der das erste Fragment auf dem Stapel zurück:

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

<a name="Communicating_with_Fragments" />

## <a name="communicating-with-fragments"></a>Bei der Kommunikation mit Fragmenten

Die *FragmentManager* kennt alle Fragmente, die an eine Aktivität angefügt werden und bietet zwei Methoden, um diese Fragmente zu finden:

-   **FindFragmentById** &ndash; diese Methode wird ein Fragment suchen, durch die ID, die in der Layoutdatei angegeben wurde oder die Container-ID verwenden, wenn das Fragment im Rahmen einer Transaktion hinzugefügt wurde.

-   **FindFragmentByTag** &ndash; diese Methode wird verwendet, um ein Fragment zu suchen, die enthält ein Tag, die in der Layoutdatei bereitgestellt wurde, bzw. in einer Transaktion hinzugefügt wurde.

Referenz zu Fragmente und Aktivitäten das `FragmentManager`, sodass die gleichen Techniken hin-und Kommunikation zwischen ihnen verwendet werden. Eine Anwendung kann das Suchen nach einem Verweis Fragment mit einem dieser beiden Methoden, diesen Verweis in den entsprechenden Typ umgewandelt und direkt aufrufen Methoden im Fragment. Der folgende Codeausschnitt enthält ein Beispiel:

Es ist auch möglich, für die Aktivität mithilfe der `FragmentManager` Fragmente gefunden:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

<a name="Communicating_with_the_Activity" />

### <a name="communicating-with-the-activity"></a>Bei der Kommunikation mit der Aktivität

Es ist möglich, dass ein Fragment verwenden die `Fragment.Activity` -Eigenschaft auf seinem Host zu verweisen. Durch Umwandlung der Aktivität in einen spezifischeren Typ, ist es möglich, dass eine Aktivität zum Aufrufen von Methoden und Eigenschaften auf dem Host, wie im folgenden Beispiel gezeigt:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
