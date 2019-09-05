---
title: Einstieg in ios
description: In diesem Dokument werden die ersten Schritte bei der Verwendung der .net-Einbettung mit IOS beschrieben. In diesem Thema werden die Anforderungen erläutert und eine Beispiel-App vorgestellt, um zu veranschaulichen, wie eine verwaltete Assembly gebunden und die Ausgabe in einem Xcode-Projekt verwendet wird.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: d5bde89ed90e55724fbc25fc473e265affa9ce2f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292946"
---
# <a name="getting-started-with-ios"></a>Einstieg in ios

## <a name="requirements"></a>Anforderungen

Zusätzlich zu den Anforderungen des Handbuchs " [Getting Started with Ziel-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) " benötigen Sie auch Folgendes:

* [Xamarin. IOS 10,11](https://visualstudio.microsoft.com/xamarin/) oder höher

## <a name="hello-world"></a>Hello World

Erstellen Sie zuerst ein einfaches Hello World-Beispiel C#in.

### <a name="create-c-sample"></a>Beispiel C# erstellen

Öffnen Sie Visual Studio für Mac, erstellen Sie ein neues IOS-Klassen Bibliotheksprojekt, benennen Sie es mit **Hello-from-CSharp**, und speichern Sie es in **~/projects/Hello-from-CSharp**.

Ersetzen Sie den Code in der Datei **MyClass.cs** durch den folgenden Code Ausschnitt:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Erstellen Sie das Projekt, und die resultierende Assembly wird als **~/projects/Hello-from-CSharp/Hello-from-CSharp/bin/debug/Hello-from-CSharp.dll**gespeichert.

### <a name="bind-the-managed-assembly"></a>Binden der verwalteten Assembly

Sobald Sie über eine verwaltete Assembly verfügen, binden Sie Sie durch Aufrufen der .net-Einbettung.

Wie im [Installations](~/tools/dotnet-embedding/get-started/install/install.md) Handbuch beschrieben, kann dies als Postbuildschritt in Ihrem Projekt mit einem benutzerdefinierten MSBuild-Ziel oder manuell erfolgen:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Das Framework wird in **~/projects/Hello-from-CSharp/Output/Hello-from-CSharp.Framework**platziert.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Verwenden der generierten Ausgabe in einem Xcode-Projekt

Öffnen Sie Xcode, erstellen Sie eine neue IOS Single View-Anwendung, benennen Sie Sie mit **Hello-from-CSharp**, und wählen Sie die Sprache **Ziel-C** aus.

Öffnen Sie das Verzeichnis **~/projects/Hello-from-CSharp/Output** im Finder, wählen Sie **Hello-from-CSharp. Framework aus**, ziehen Sie es in das Xcode-Projekt, und legen Sie es direkt oberhalb des " **Hello-from-CSharp** "-Ordners im Projekt ab.

![Drag & Drop-Framework](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Stellen Sie sicher, dass bei **Bedarf Elemente kopieren** im angezeigten Dialogfeld aktiviert ist, und klicken Sie auf **Fertig**stellen.

![Elemente bei Bedarf kopieren](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Wählen Sie das **Hello-from-CSharp-Projekt aus** , und navigieren Sie zur **Registerkarte Allgemein auf der Registerkarte**" **Hello-from-CSharp** ". Fügen Sie im Abschnitt **eingebettete Binärdatei** den Wert **Hello-from-CSharp. Framework**hinzu.

![Eingebettete Binärdateien](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Öffnen Sie **ViewController. m**, und ersetzen Sie den Inhalt durch:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Das Einbetten von .NET unterstützt derzeit keinen Bitcode unter IOS, der für einige Xcode-Projektvorlagen aktiviert ist. 

Deaktivieren Sie Sie in den Projekteinstellungen:

![Bitcode-Option](../../images/ios-bitcode-option.png)

Schließlich führen Sie das Xcode-Projekt aus, und in etwa wird Folgendes angezeigt:

![Hello from C# Sample-Ausführung im Simulator](ios-images/hello-from-csharp-ios.png)
