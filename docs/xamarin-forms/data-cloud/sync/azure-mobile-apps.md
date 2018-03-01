---
title: Synchronisieren von Offline-Daten mit Azure Mobile Apps
description: "Offline Sync ermöglicht Benutzern, für die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Netzwerkverbindung vorhanden ist. Änderungen in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen mit der Azure-Mobile Apps-Instanz synchronisiert. In diesem Artikel wird erläutert, wie offline Sync-Funktionen zu einer Xamarin.Forms-Anwendung hinzugefügt."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: b7756c63901d3b4fbfea70587b3fdf8e5cf9df72
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Synchronisieren von Offline-Daten mit Azure Mobile Apps

_Offline Sync ermöglicht Benutzern, für die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Netzwerkverbindung vorhanden ist. Änderungen in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen mit der Azure-Mobile Apps-Instanz synchronisiert. In diesem Artikel wird erläutert, wie offline Sync-Funktionen zu einer Xamarin.Forms-Anwendung hinzugefügt._

## <a name="overview"></a>Übersicht

Die [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) bietet die `IMobileServiceTable` -Schnittstelle, die die Vorgänge darstellt, die für Tabellen in der Instanz von Azure Mobile Apps ausgeführt werden können. Diese Vorgänge direkt an die Azure-Mobile Apps-Instanz verbinden und schlägt fehl, wenn das mobile Gerät nicht über eine Netzwerkverbindung verfügt.

Zur Unterstützung von offline-Synchronisierung unterstützt das Azure Mobile Client-SDK *Tabellen synchronisieren*, die im Lieferumfang von der `IMobileServiceSyncTable` Schnittstelle. Diese Schnittstelle bietet die gleichen erstellen, lesen, aktualisieren, löschen (CRUD)-Vorgänge als die `IMobileServiceTable` -Schnittstelle, aber die Vorgänge lesen oder Schreiben in einen lokalen Speicher. Lokale Speicher ist nicht mit neuen Daten aus der Azure-Mobile Apps-Instanz aufgefüllt, bis es aufgerufen wird, *Pull* Daten. Auf ähnliche Weise Daten ist nicht erst gesendet, mit der Azure-Mobile Apps-Instanz erfolgt ein Aufruf zum *Push* lokale Änderungen.

Offline Sync gehört auch die Unterstützung zum Erkennen von Konflikten bei der gleiche Datensatz, im lokalen Speicher und geändert hat in der Azure-Mobile Apps-Instanz, und klicken Sie auf eine benutzerdefinierte konfliktlösung. Konflikte können entweder im lokalen Speicher oder in der Azure-Mobile Apps-Instanz verarbeitet werden.

Weitere Informationen zu offline Sync, finden Sie unter [Offline-Datensynchronisierung in Azure Mobile Apps](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) und [aktivieren offline Synchronisierung für Ihre mobile app mit Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Setup

Der Prozess für die Integration von offline-Synchronisierung zwischen einer Xamarin.Forms-Anwendung und einer Instanz von Azure Mobile Apps ist wie folgt:

1. Erstellen Sie eine Azure Mobile Apps-Instanz. Weitere Informationen finden Sie unter [einer Azure-Mobile-App nutzen](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Hinzufügen der [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) NuGet-Paket für alle Projekte in Xamarin.Forms-Projektmappe.
1. (Optional) Aktivieren der Authentifizierung in der Azure-Mobile Apps-Instanz und die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Der folgende Abschnitt enthält zusätzliche setupanweisungen für das Konfigurieren von Projekten für universelle Windows-Plattform (UWP) das Microsoft.Azure.Mobile.Client.SQLiteStore NuGet-Paket verwenden. Ohne zusätzliche Einrichtung ist erforderlich, um das Microsoft.Azure.Mobile.Client.SQLiteStore NuGet-Paket für iOS und Android verwenden.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Um SQLite auf die universelle Windows-Plattform (UWP) zu verwenden, gehen Sie folgendermaßen vor:

1. Installieren der [SQLite für die universelle Windows-Plattform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) Visual Studio-Erweiterung in der Entwicklungsumgebung.
1. Klicken Sie im uwp-Projekt in Visual Studio, mit der rechten Maustaste auf **Verweise > Verweis hinzufügen**, navigieren Sie zu **Erweiterungen** und Hinzufügen der **SQLite für universelle Windows-Plattform** und  **Visual C++ 2015-Laufzeit für universelle Windows-Plattform-Apps** Erweiterungen für das uwp-Projekt.

## <a name="initializing-the-local-store"></a>Initialisieren den lokalen Speicher

Der lokale Speicher muss initialisiert werden, damit alle Tabellenvorgänge Sync ausgeführt werden können. Dies erfolgt im Projekt Portable Klassenbibliothek (PCL) mit Xamarin.Forms-Projektmappe:

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

Eine neue lokale SQLite-Datenbank wird erstellt, indem die `MobileServiceSQLiteStore` -Klasse, vorausgesetzt, dass er nicht bereits vorhanden. Anschließend wird die `DefineTable<T>` Methode erstellt eine Tabelle im lokalen Speicher, der die Felder im entspricht der `TodoItem` eingeben, vorausgesetzt, dass er nicht bereits vorhanden.

Ein *Sync Kontext* zugeordnet ist eine `MobileServiceClient` -Instanz und verfolgt Änderungen, die mit Sync Tabellen vorgenommen werden. Das Sync-Kontext verwaltet eine Warteschlange, die einer geordneten Liste von Vorgängen für erstellen, aktualisieren und löschen (CRUD), und dabei mit der Azure-Mobile Apps-Instanz später gesendet wird. Die `IMobileServiceSyncContext.InitializeAsync()` Methode wird verwendet, um den Kontext für die Synchronisierung im lokalen Speicher zuordnen.

Die `todoTable` Feld ist ein `IMobileServiceSyncTable`, weshalb alle CRUD-Operationen mit dem lokalen Speicher.

## <a name="performing-synchronization"></a>Ausführen einer Synchronisierung

Der lokale Speicher mit den Azure-Mobile Apps synchronisiert Instanz beim die `SyncAsync` Methode aufgerufen wird:

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

Die `IMobileServiceSyncTable.PushAsync` Methode arbeitet mit den Synchronisierung-Kontext, anstatt eine bestimmte Tabelle und alle CUD Änderungen seit der letzten Push sendet.

Pull erfolgt durch die `IMobileServiceSyncTable.PullAsync` Methode für eine einzelne Tabelle. Der erste Parameter für die `PullAsync` Methode wird einen Abfragenamen ein, die nur auf dem mobilen Gerät verwendet wird. Mit einer Abfrage ungleich Null Name-Ergebnissen bei der Ausführung von Azure Mobile Client-SDK ein *inkrementelle Synchronisierung*, jedes Mal eine Pullvorgang Ergebnisse, die neuesten zurück `updatedAt` Zeitstempel aus den Ergebnissen in der lokalen gespeichert ist Systemtabellen. Nachfolgende Pull-Vorgängen abrufen dann nur Datensätze nach diesem Zeitstempel. Alternativ können Sie *vollständige Synchronisierung* kann erreicht werden, indem Sie übergeben `null` als Name der Abfrage, was dazu führt, in allen Datensätzen, die auf jeden Pullvorgang abgerufen wird. Nach jeder Synchronisierungsvorgang werden die empfangene Daten in den lokalen Speicher eingefügt.

> [!NOTE]
> **Hinweis**: Wenn ein Pullabonnement für eine Tabelle, die lokale Updates ausstehen ausgeführt wird, führen die Pull einen Push zuerst im Kontext der Synchronisierung. Dies minimiert die Konflikte zwischen Änderungen, die bereits in der Warteschlange befinden und neue Daten aus der Azure-Mobile Apps-Instanz.

Die `SyncAsync` -Methode enthält auch eine grundlegende Implementierung für die Behandlung von Konflikten, wenn derselbe Datensatz im lokalen Speicher und in Azure Mobile Apps-Instanz geändert wurde. Wenn der Konflikt wird, dass Daten sowohl im lokalen Speicher und in der Azure-Mobile Apps-Instanz aktualisiert wurde die `SyncAsync` Methode aktualisiert die Daten im lokalen Speicher aus den Daten in der Azure-Mobile Apps-Instanz gespeichert. Wenn alle anderen Konflikt auftritt, die `SyncAsync` -Methode werden die lokale Änderung verworfen. Verarbeitet das Szenario vorhanden ist, in denen eine Änderung der lokale für Daten, die aus der Azure Mobile Apps-Instanz gelöscht worden ist.

In einer produktionsanwendung sollten Entwickler schreiben eine benutzerdefinierten `IMobileServiceSyncHandler` Konfliktbehandlung Implementierung, die auf ihre Anwendungsfall geeignet ist. Weitere Informationen finden Sie unter [vollständige Parallelität für die konfliktlösung](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) auf Azure-Portal und [Deep beschäftigen, der offline-Unterstützung in den verwalteten Client SDK](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) auf MSDN-Blogs.

## <a name="purging-data"></a>Löschen von Daten

Tabellen in den lokalen Speicher können gelöscht werden, der Daten mit der `IMobileServiceSyncTable.PurgeAsync` Methode. Diese Methode unterstützt Szenarien, z. B. das Entfernen von veralteten Daten, die eine Anwendung nicht mehr benötigt. Zeigt z. B. die beispielanwendung nur `TodoItem` Instanzen, die nicht abgeschlossen sind. Aus diesem Grund müssen abgeschlossene Elemente nicht mehr lokal gespeichert werden. Löschen abgeschlossene Elemente aus dem lokalen Speicher kann wie folgt erfolgen:

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Ein Aufruf von `PurgeAsync` auch löst einen Push-Vorgang. Aus diesem Grund werden alle Elemente, die lokal als abgeschlossen markiert sind mit der Azure-Mobile Apps-Instanz gesendet werden, aus dem lokalen Speicher entfernt. Allerdings Vorgänge ausstehend Synchronisierung mit der Azure-Mobile Apps-Instanz vorhanden sind, die Bereinigung wird auszulösen, wenn ein `InvalidOperationException` , sofern die `force` Parameter auf festgelegt ist `true`. Eine alternative Strategie besteht darin, überprüfen Sie die `IMobileServiceSyncContext.PendingOperations` Eigenschaft, die die Anzahl der ausstehenden Vorgänge, die noch nicht mit der Azure-Mobile Apps-Instanz ein Push ausgeführt wurde zurückgibt, und nur die Bereinigung durchführen, wenn die Eigenschaft auf 0 (null) ist.

> [!NOTE]
> **Hinweis**: Aufrufen von `PurgeAsync` mit der `force` Parametersatz auf `true` verlieren alle ausstehenden Änderungen.

## <a name="initiating-synchronization"></a>Initiieren der Synchronisierung

In der beispielanwendung der `SyncAsync` Methode wird aufgerufen, durch die `TodoList.OnAppearing` Methode:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

Dies bedeutet, dass die Anwendung beim Start mit der Azure-Mobile Apps-Instanz zu synchronisieren versucht.

Synchronisierung kann darüber hinaus in iOS und Android initiiert werden, mithilfe von Pull, aktualisieren Sie auf die Liste der Daten, und klicken Sie auf der Windows-Plattformen mithilfe der **Sync** Schaltfläche auf der Benutzeroberfläche. Weitere Informationen finden Sie unter [Aktualisierung Pull](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie eine Xamarin.Forms-Anwendung offline Sync Funktionen hinzu. Offline Sync ermöglicht Benutzern, für die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Netzwerkverbindung vorhanden ist. Änderungen in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen mit der Azure-Mobile Apps-Instanz synchronisiert.


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuthOfflineSync (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Verwenden einer mobilen Anwendung für Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
