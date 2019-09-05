---
title: Ersten Einstieg in macOS
description: In diesem Dokument wird beschrieben, wie Sie mit dem Einbetten von .net mit macOS beginnen. Es werden die Anforderungen erläutert und eine Beispielanwendung dargestellt, um zu veranschaulichen, wie die verwaltete Assembly gebunden und die generierte Ausgabe in einem Xcode-Projekt verwendet wird.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: e7f1bac95e9e812c0e47b55b85f72f34ba76e68e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290269"
---
# <a name="getting-started-with-macos"></a>Ersten Einstieg in macOS

## <a name="what-you-will-need"></a>Was Sie benötigen

* Befolgen Sie die Anweisungen im Handbuch " [Getting Started with Ziel-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) ".

## <a name="hello-world"></a>Hello World

Erstellen Sie zuerst ein einfaches Hello World-Beispiel C#in.

### <a name="create-c-sample"></a>Beispiel C# erstellen

Öffnen Sie Visual Studio für Mac, erstellen Sie ein neues Mac-Klassen Bibliotheksprojekt mit dem Namen **Hello-from-CSharp**, und speichern Sie es in **~/projects/Hello-from-CSharp**.

Ersetzen Sie den Code in der Datei **MyClass.cs** durch den folgenden Code Ausschnitt:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Erstellen Sie das Projekt. Die resultierende Assembly wird als **~/projects/Hello-from-CSharp/Hello-from-CSharp/bin/debug/Hello-from-CSharp.dll**gespeichert.

### <a name="bind-the-managed-assembly"></a>Binden der verwalteten Assembly

Sobald Sie über eine verwaltete Assembly verfügen, binden Sie Sie durch Aufrufen der .net-Einbettung.

Wie im [Installations](~/tools/dotnet-embedding/get-started/install/install.md) Handbuch beschrieben, kann dies als Postbuildschritt in Ihrem Projekt mit einem benutzerdefinierten MSBuild-Ziel oder manuell erfolgen:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Das Framework wird in **~/projects/Hello-from-CSharp/Output/Hello-from-CSharp.Framework**platziert.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Verwenden der generierten Ausgabe in einem Xcode-Projekt

Öffnen Sie Xcode, und erstellen Sie eine neue Cocoa-Anwendung. Nennen Sie es **Hello-from-CSharp** , und wählen Sie die Sprache **Ziel-C** aus.

Öffnen Sie das Verzeichnis **~/projects/Hello-from-CSharp/Output** im Finder, wählen Sie **Hello-from-CSharp. Framework aus**, ziehen Sie es in das Xcode-Projekt, und legen Sie es direkt oberhalb des " **Hello-from-CSharp** "-Ordners im Projekt ab.

![Drag & Drop-Framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Stellen Sie sicher, dass bei **Bedarf Elemente kopieren** im angezeigten Dialogfeld aktiviert ist, und klicken Sie auf **Fertig**stellen.

![Elemente bei Bedarf kopieren](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Wählen Sie das **Hello-from-CSharp-Projekt aus** , und navigieren Sie zur Registerkarte **Allgemein** auf der Registerkarte " **Hello-from-CSharp** ". Fügen Sie im Abschnitt **eingebettete Binärdatei** den Wert **Hello-from-CSharp. Framework**hinzu.

![Eingebettete Binärdateien](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Öffnen Sie **ViewController. m**, und ersetzen Sie den Inhalt durch:

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Schließlich führen Sie das Xcode-Projekt aus, und in etwa wird Folgendes angezeigt:

![Hello from C# Sample-Ausführung im Simulator](macos-images/hello-from-csharp-mac.png)

Ein ausführlichere und besser aussehende Stichprobe [finden Sie hier](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
