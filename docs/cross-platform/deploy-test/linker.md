---
title: Benutzerdefinierte Linkerkonfiguration
description: In diesem Dokument wird eine XML-Datei beschrieben, die zum Konfigurieren des Linkers verwendet werden kann, um explizit sicherzustellen, dass erforderlicher Code nicht aus der verknüpften Anwendung gelöscht wird.
ms.prod: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7d9b7dfc12c27a195e3cb797167690cded348803
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "75488906"
---
# <a name="custom-linker-configuration"></a>Benutzerdefinierte Linkerkonfiguration

Wenn die Standardoptionen nicht ausreichen, können Sie den Linkerprozess mit einer XML-Datei steuern, die beschreibt, was Sie mit dem Linker tun möchten.

Sie können dem Linker zusätzliche Definitionen zur Verfügung stellen, um sicherzustellen, das der Typ, die Methoden und/oder die Felder nicht aus Ihrer Anwendung gelöscht werden. In Ihrem eigenen Code ist die empfohlene Herangehensweise das Verwenden der benutzerdefinierten `[Preserve]`-Attribute. Dies wird in den Artikeln [Linking on iOS (Verknüpfen unter iOS)](~/ios/deploy-test/linker.md) und [Linking on Android (Verknüpfen unter Android)](~/android/deploy-test/linker.md) besprochen.
Wenn Sie allerdings Definitionen vom SDK oder von Produktassemblys benötigen, ist eine XML-Datei die beste Lösung (und nicht das Hinzufügen von Code, um sicherzustellen, dass der Linker nichts Wichtiges löscht).

Dazu definieren Sie eine XML-Datei mit dem allgemeinen Element `<linker>`, die *assembly*-Knoten enthält, die wiederum *type*-Knoten enthalten, die *method*- und *field*-Knoten enthalten.

Sobald Sie über diese Linkerbeschreibungsdatei verfügen, fügen Sie sie Ihrem Projekt hinzu, und führen Sie Folgendes durch:

- **Für Android:** Legen Sie den **Buildvorgang** auf **LinkDescription** fest.
- **Für iOS:** Legen Sie den **Buildvorgang** auf **LinkDescription** fest.

Im folgenden Beispiel wird gezeigt, wie die XML-Datei aussieht:

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

Im oben stehenden Beispiel liest der Linker die Anweisungen in der `mscorlib.dll`- (wird mit Mono für Android ausgeliefert) und der `My.Own.Assembly`-Assembly (Benutzercode) und wendet diese an.

Der erste Abschnitt für `mscorlib.dll` stellt sicher, das der `System.Environment`-Typ sein Feld mit dem Namen `mono_corlib_version` und dessen `get_StackTrace`-Methode beibehält.
Beachten Sie, dass die Setter- und/oder Gettermethodennamen verwendet werden müssen, wenn der Linker mit IL arbeitet und C#-Eigenschaften nicht versteht.

Der zweite Abschnitt für `My.Own.Assembly.dll` stellt sicher, dass der `Foo`-Typ alle seine Felder (d.h. das `preserve="fields"`-Attribut) und alle seine Konstruktoren (d.h. alle Methoden mit dem Namen `.ctor` in IL) beibehält. Der `Bar`-Typ behält spezifische Signaturen (keine Namen) für einen Konstruktor (der einen einzelnen Zeichenfolgenparameter akzeptiert) und für das spezifische Zeichenfolgenfeld `_blah` bei.
Der `My.Own.Namespace`-Namespace behält alle Typen bei, die er enthält.
Jeder Typ, dessen vollständiger Name (der Namespace inbegriffen) mit dem Platzhaltermuster „My.Other\*“ übereinstimmt, behält alle seine Felder und Methoden bei. Das Platzhalterzeichen `*` kann mehrere Male im „type fullname“-Muster verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Verknüpfung unter iOS](~/ios/deploy-test/linker.md)
- [Linking on Android (Verknüpfung unter Android)](~/android/deploy-test/linker.md)
- [Linker GitHub-Repository mit Beispielen](https://github.com/mono/linker)
