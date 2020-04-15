---
title: Verwalten von Fragmenten
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3733ed37abe9604e77529db4864f601d2b473280
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027320"
---
# <a name="managing-fragments"></a>Verwalten von Fragmenten

Zur Unterstützung der Verwaltung von Fragmenten stellt Android die Klasse `FragmentManager` bereit. Jede Aktivität verfügt über eine Instanz von `Android.App.FragmentManager`, die die zugehörigen Fragmente sucht oder dynamisch ändert. Jeder Satz solcher Änderungen wird als *Transaktion* bezeichnet und mithilfe einer der APIs in der Klasse `Android.App.FragmentTransation` ausgeführt, die vom `FragmentManager` verwaltet wird. Eine Aktivität kann beispielsweise folgendermaßen eine Transaktion starten:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Diese Änderungen an den Fragmenten werden in der `FragmentTransaction`-Instanz mithilfe solcher Methoden ausgeführt: `Add()`, `Remove(),` und `Replace().` Dann werden die Änderungen mithilfe von `Commit()` angewendet. Die Änderungen in einer Transaktion werden nicht sofort ausgeführt.
Stattdessen werden sie so geplant, dass sie so schnell wie möglich im Benutzeroberflächenthread der Aktivität ausgeführt werden.

Das folgende Beispiel zeigt, wie Sie einem vorhandenen Container ein Fragment hinzufügen:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Wenn der Commit einer Transaktion nach dem Aufruf von `Activity.OnSaveInstanceState()` erfolgt, wird eine Ausnahme ausgelöst. Das passiert aus folgendem Grund: Wenn die Aktivität ihren Status speichert, speichert Android auch den Status aller gehosteten Fragmente. Wenn der Commit von Fragmenttransaktionen nach diesem Zeitpunkt erfolgt, geht der Status dieser Transaktionen verloren, wenn die Aktivität wiederhergestellt wird.

Es ist möglich, die Fragmenttransaktionen durch Aufruf von `FragmentTransaction.AddToBackStack()` im [Back Stack](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) (Stapel der Aktionen zum Rückgängigmachen) der Aktivität zu speichern. So können Benutzer rückwärts durch Fragmentänderungen navigieren, indem sie auf die Schaltfläche **Zurück** tippen. Ohne Aufruf dieser Methode werden entfernte Fragmente gelöscht und sind nicht mehr verfügbar, wenn Benutzer rückwärts durch die Aktivität navigieren.

Das folgende Beispiel zeigt die Verwendung der `AddToBackStack`-Methode einer `FragmentTransaction`, um ein Fragment zu ersetzen und gleichzeitig den Status des ersten Fragments im Back Stack beizubehalten:

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

Der *FragmentManager* kennt alle Fragmenten, die an eine Aktivität angefügt sind, und stellt zwei Methoden zum Suchen nach diesen Fragmenten bereit:

- **FindFragmentById** &ndash; bei dieser Methode erfolgt die Fragmentsuche anhand der in der Layoutdatei angegebenen ID oder der Container-ID, die beim Hinzufügen des Fragments im Rahmen einer Transaktion erstellt wurde.

- **FindFragmentByTag** &ndash; diese Methode sucht nach einem Fragment, das ein Tag aufweist, das in der Layoutdatei bereitgestellt oder in einer Transaktion hinzugefügt wurde.

Sowohl Fragmente als auch Aktivitäten verweisen auf den `FragmentManager`, sodass zur Kommunikation zwischen diesen Elementen dieselben Techniken verwendet werden. Eine Anwendung kann mithilfe einer dieser beiden Methoden nach einem Verweisfragment suchen, diesen Verweis in einen geeigneten Typ umwandeln und dann Methoden im Fragment direkt aufrufen. Der folgende Codeausschnitt zeigt ein Beispiel dafür:

Es ist auch möglich, dass eine Aktivität den `FragmentManager` verwendet, um Fragmente zu suchen:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>Kommunizieren mit der Aktivität

Ein Fragment kann die `Fragment.Activity`-Eigenschaft verwenden, um auf seinen Host zu verweisen. Durch Umwandeln einer Aktivität in einen spezifischeren Typ ist es möglich, dass die Aktivität Methoden und Eigenschaften in ihrem Host aufruft, wie im folgenden Beispiel gezeigt:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
