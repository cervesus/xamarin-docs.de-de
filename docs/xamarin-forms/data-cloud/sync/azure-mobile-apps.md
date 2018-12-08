---
title: Synchronisieren von Offlinedaten mit Azure Mobile Apps
description: In diesem Artikel wird erläutert, Hinzufügen von offlinesynchronisierung zu einer Xamarin.Forms-Anwendung, die ein Azure Mobile Apps-Back-End verwendet wird.
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: 6080b4dc152558d6f532399cee7424670c588c28
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058177"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Synchronisieren von Offlinedaten mit Azure Mobile Apps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)

_Die offlinesynchronisierung ermöglicht Benutzern die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn es keine Netzwerkverbindung. Änderungen werden in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen mit der Azure Mobile Apps-Instanz synchronisiert werden. In diesem Artikel wird erläutert, Hinzufügen von offlinesynchronisierung zu einer Xamarin.Forms-Anwendung._

## <a name="overview"></a>Übersicht

Die [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) bietet die `IMobileServiceTable` -Schnittstelle, die die Vorgänge darstellt, die in Tabellen gespeichert, in der Azure Mobile Apps-Instanz ausgeführt werden können. Diese Vorgänge direkt an die Azure Mobile Apps-Instanz herstellen und schlägt fehl, wenn das mobile Gerät nicht über eine Netzwerkverbindung verfügt.

Zur Unterstützung von offlinesynchronisierung unterstützt das Azure Mobile Client SDK *Tabellen synchronisieren*, die im Lieferumfang von der `IMobileServiceSyncTable` Schnittstelle. Diese Schnittstelle bietet, die dieselben Erstellungs-, Lese-, Update, Delete (CRUD) Vorgänge wie das `IMobileServiceTable` -Schnittstelle, aber die Vorgänge lesen oder Schreiben in einen lokalen Speicher. Der lokale Speicher ist nicht mit neuen Daten aus der Azure Mobile Apps-Instanz aufgefüllt, bis ein zum Aufruf *Pull* Daten. Auf ähnliche Weise wird nicht Daten mit der Azure Mobile Apps-Instanz gesendet, bis ein zum Aufruf *Push* lokale Änderungen.

Die offlinesynchronisierung unterstützt auch zum Erkennen von Konflikten bei der gleiche Datensatz, im lokalen Speicher und geändert wurde in das Azure Mobile Apps-Instanz, und klicken Sie auf benutzerdefinierte konfliktlösung. Konflikte können entweder im lokalen Speicher oder in der Azure Mobile Apps-Instanz verarbeitet werden.

Weitere Informationen zur offline-datensynchronisierung finden Sie unter [Synchronisierung von Offlinedaten in Azure Mobile Apps](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) und [Aktivieren der offlinesynchronisierung für Ihre mobile Xamarin.Forms-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Setup

Der Prozess für die Integration von offline-Synchronisierung zwischen einer Xamarin.Forms-Anwendung und einer Azure Mobile Apps-Instanz lautet wie folgt aus:

1. Erstellen Sie eine Azure Mobile Apps-Instanz. Weitere Informationen finden Sie unter [Nutzung von Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Hinzufügen der [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) NuGet-Paket für alle Projekte in der Xamarin.Forms-Projektmappe.
1. (Optional) Aktivieren der Authentifizierung in der Azure Mobile Apps-Instanz und die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Der folgende Abschnitt enthält weitere installationsanweisungen für das Konfigurieren von Projekten der universellen Windows-Plattform (UWP) das Microsoft.Azure.Mobile.Client.SQLiteStore NuGet-Paket verwenden. Ohne zusätzliche Einrichtung ist erforderlich, um das Microsoft.Azure.Mobile.Client.SQLiteStore NuGet-Paket für iOS und Android zu verwenden.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Um auf die universelle Windows-Plattform (UWP) zu SQLite verwenden, gehen Sie folgendermaßen vor:

1. Installieren Sie die [SQLite für die universelle Windows-Plattform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) Visual Studio-Erweiterung in Ihrer Entwicklungsumgebung.
1. In der UWP-Projekt in Visual Studio, mit der rechten Maustaste **Verweise > Verweis hinzufügen**, navigieren Sie zu **Erweiterungen** und Hinzufügen der **SQLite für universelle Windows-Plattform** und  **Visual C++ 2015 Runtime für Apps der universellen Windows-Plattform** -Erweiterungen für UWP-Projekt.

## <a name="initializing-the-local-store"></a>Initialisieren den lokalen Store

Bevor synchronisierungtabellenvorgänge durchgeführt werden können, muss der lokale Speicher initialisiert werden. Dies erfolgt in das Projekt "Portable Klassenbibliothek (PCL)" der Xamarin.Forms-Lösung:

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

Eine neue lokale SQLite-Datenbank wird erstellt, indem die `MobileServiceSQLiteStore` Klasse, vorausgesetzt, dass sie nicht bereits vorhanden. Anschließend wird die `DefineTable<T>` Methode erstellt eine Tabelle im lokalen Speicher, der die Felder im entspricht der `TodoItem` eingeben, vorausgesetzt, dass sie nicht bereits vorhanden.

Ein *Synchronisierungskontext* zugeordnet ist eine `MobileServiceClient` -Instanz und Änderungen in Echtzeit nachverfolgt, die mithilfe von Synchronisierungstabellen vorgenommen werden. Der Synchronisierungskontext verwaltet, dass eine Warteschlange, die einer geordneten Liste von erstellen-, aktualisieren- und löschen (CRUD) Vorgänge, die verfolgt werden später mit der Azure Mobile Apps-Instanz gesendet werden. Die `IMobileServiceSyncContext.InitializeAsync()` Methode wird verwendet, um den lokalen Speicher mit dem Synchronisierungskontext zuordnen.

Die `todoTable` Feld ist ein `IMobileServiceSyncTable`, weshalb alle CRUD-Vorgänge im lokalen Speicher verwenden.

## <a name="performing-synchronization"></a>Eine Synchronisierung

Im lokale Speicher synchronisiert wird, mit der Azure Mobile Apps-Instanz bei der `SyncAsync` -Methode wird aufgerufen:

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

Die `IMobileServiceSyncTable.PushAsync` Methode greift auf den Synchronisierungskontext, anstatt eine bestimmte Tabelle und alle CUD-Änderungen seit dem letzten Pushvorgang sendet.

Pull erfolgt durch die `IMobileServiceSyncTable.PullAsync` Methode für eine einzelne Tabelle. Der erste Parameter für die `PullAsync` Methode ist, einen Abfragenamen ein, die nur auf dem mobilen Gerät verwendet wird. Bereitstellen einer nicht-Null-Abfrage von Namen wird das Ausführen von Azure Mobile Client SDK eine *inkrementelle Synchronisierung*, in denen jedes Mal ein Pull-Vorgang Ergebnisse zu erzielen, die neuesten zurückgibt `updatedAt` Zeitstempel aus den Ergebnissen befindet sich in der lokalen Systemtabellen. Nachfolgende Pullvorgänge rufen Sie dann nur Datensätze nach dem jeweiligen Zeitstempel. Sie können auch *vollständige Synchronisierung* erzielt werden, indem übergeben `null` als den Namen der Abfrage, was dazu führt, in allen Datensätzen, die bei jedem Pullvorgang abgerufen wird. Nach jedem Synchronisierungsvorgang werden die empfangene Daten in den lokalen Speicher eingefügt.

> [!NOTE]
> Wenn ein Pullvorgang für eine Tabelle, die ausstehenden lokalen Updates ausgeführt wird, wird der Pull zuerst einen Pushvorgang im Synchronisierungskontext ausgeführt. Dies minimiert Konflikte zwischen Änderungen, die bereits in der Warteschlange werden und neue Daten aus der Azure Mobile Apps-Instanz.

Die `SyncAsync` Methode enthält außerdem eine grundlegende Implementierung für das Behandeln von Konflikten, wenn der gleiche Datensatz, im lokalen Speicher und in Azure Mobile Apps-Instanz geändert wurde. Wenn der Konflikt besteht, dass die Daten aktualisiert wurden sowohl im lokalen Speicher und in der Azure Mobile Apps-Instanz, die `SyncAsync` Methode aktualisiert die Daten im lokalen Speicher von den in der Azure Mobile Apps-Instanz gespeicherten Daten. Wenn alle anderen Konflikt auftritt, die `SyncAsync` -Methode verwirft die lokale Änderung. Diese behandelt das Szenario, in denen eine lokale Änderung vorhanden, für Daten, die von der Azure Mobile Apps-Instanz gelöscht wurde, werden.

In einer produktionsanwendung sollten Entwickler ein benutzerdefiniertes schreiben `IMobileServiceSyncHandler` Konfliktbehandlung Implementierung, die für ihren Anwendungsfall geeignet ist. Weitere Informationen finden Sie unter [verwenden der optimistischen Parallelität zur Lösung von Konflikten](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) auf Azure-Portal und [fundierte Einblicke in die offline-Unterstützung in verwalteten Client-SDK](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) auf MSDN-Blogs.

## <a name="purging-data"></a>Löschen von Daten

Tabellen im lokalen Speicher können gelöscht werden, mit der `IMobileServiceSyncTable.PurgeAsync` Methode. Diese Methode unterstützt Szenarien, z. B. das Entfernen von veralteter Daten, die eine Anwendung nicht mehr erforderlich ist. Beispielsweise zeigt die beispielanwendung nur `TodoItem` -Instanzen, die nicht abgeschlossen sind. Aus diesem Grund müssen abgeschlossene Elemente nicht mehr lokal gespeichert werden. Löschen die abgeschlossene Elemente aus dem lokalen Speicher kann wie folgt erreicht werden:

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Ein Aufruf von `PurgeAsync` auch einen Push-Vorgang ausgelöst. Aus diesem Grund werden alle Elemente, die lokal als abgeschlossen markiert sind mit der Azure Mobile Apps-Instanz gesendet werden, bevor Sie aus dem lokalen Speicher entfernt werden. Jedoch, wenn Vorgänge ausstehenden Synchronisierung mit der Azure Mobile Apps-Instanz vorhanden sind, löst das Löschen einer `InvalidOperationException` , wenn die `force` Parametersatz zu `true`. Eine alternative Strategie besteht darin, überprüfen Sie die `IMobileServiceSyncContext.PendingOperations` -Eigenschaft, die die Anzahl der ausstehenden Vorgänge, die noch nicht mit der Azure Mobile Apps-Instanz übertragen wurden zurückgibt, und führen das Löschen nur, wenn die Eigenschaft auf 0 (null) ist.

> [!NOTE]
> Aufrufen von `PurgeAsync` mit der `force` Parametersatz zu `true` verlieren alle ausstehenden Änderungen.

## <a name="initiating-synchronization"></a>Initiieren der Synchronisierung

In der beispielanwendung die `SyncAsync` -Methode wird aufgerufen, durch die `TodoList.OnAppearing` Methode:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

Dies bedeutet, dass die Anwendung versucht wird, mit der Azure Mobile Apps-Instanz synchronisiert werden soll, wenn er gestartet wird.

Synchronisierung kann darüber hinaus in iOS und Android initiiert werden, mithilfe von Pull, aktualisieren Sie in der Liste der Daten, und klicken Sie auf den Windows-Plattformen mithilfe der **Sync** Schaltfläche auf der Benutzeroberfläche. Weitere Informationen finden Sie unter [zum Aktualisieren ziehen](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert das Hinzufügen von offlinesynchronisierung zu einer Xamarin.Forms-Anwendung. Die offlinesynchronisierung ermöglicht Benutzern die Interaktion mit einer mobilen Anwendung, anzeigen, hinzufügen oder Ändern von Daten, selbst wenn es keine Netzwerkverbindung. Änderungen werden in einer lokalen Datenbank gespeichert, und sobald das Gerät online ist, können die Änderungen mit der Azure Mobile Apps-Instanz synchronisiert werden.


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuthOfflineSync (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Verwenden einer Azure-Mobile-App](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
