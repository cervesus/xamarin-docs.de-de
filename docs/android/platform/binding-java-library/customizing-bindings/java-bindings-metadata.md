---
title: Metadaten für Java-Bindungen
description: C#Code in Xamarin.Android ruft Java-Bibliotheken über Bindungen, die einen Mechanismus, der die Details auf niedriger Ebene abstrahiert, die in Java Native Interface (JNI) angegeben werden. Xamarin.Android bietet ein Tool, das diese Bindungen generiert werden. Dieser Tools können die Developer-Steuerelement wie eine Bindung erstellt wird, mithilfe von Metadaten, die Prozeduren, z. B. das Ändern von Namespaces und Umbenennen von Elementen ermöglicht. In diesem Dokument wird erläutert, wie Metadaten funktioniert, werden die Attribute aufgeführt, dass Metadaten unterstützt, und erläutert, wie Bindungsprobleme zu beheben, indem Sie diese Metadaten zu ändern.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: ce9bf0293b846299cc7cd06773ce936f725715fa
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669894"
---
# <a name="java-bindings-metadata"></a>Metadaten für Java-Bindungen

_C#Code in Xamarin.Android ruft Java-Bibliotheken über Bindungen, die einen Mechanismus, der die Details auf niedriger Ebene abstrahiert, die in Java Native Interface (JNI) angegeben werden. Xamarin.Android bietet ein Tool, das diese Bindungen generiert werden. Dieser Tools können die Developer-Steuerelement wie eine Bindung erstellt wird, mithilfe von Metadaten, die Prozeduren, z. B. das Ändern von Namespaces und Umbenennen von Elementen ermöglicht. In diesem Dokument wird erläutert, wie Metadaten funktioniert, werden die Attribute aufgeführt, dass Metadaten unterstützt, und erläutert, wie Bindungsprobleme zu beheben, indem Sie diese Metadaten zu ändern._


## <a name="overview"></a>Übersicht

Eine xamarin.Android-Anwendung **Java Bindungsbibliothek** versucht, den Großteil der Arbeit erforderlich, für die Bindung einer vorhandenen Android-Bibliothek mithilfe eines Tools, auch bezeichnet als automatisieren die _Bindungen Generator_. Beim Binden einer Java-Bibliothek Xamarin.Android überprüfen Sie die Java-Klassen und generiert eine Liste mit allen Paketen, Typen und Member werden die gebunden werden soll. Diese Liste der APIs befindet sich in einer XML-Datei, die finden Sie unter  **\{Projekt directory}\obj\Release\api.xml** für eine **Version** erstellen und zur  **\{Projekt Directory}\obj\Debug\api.XML** für eine **DEBUGGEN** erstellen.

![Speicherort der Datei im Ordner "Obj/Debug" api.xml](java-bindings-metadata-images/java-bindings-metadata-01.png)

Der Generator Bindungen verwenden die **api.xml** Datei als Richtlinie zum Generieren von erforderlichen C# Wrapperklassen. Der Inhalt dieser XML-Datei ist eine Variation des Google _Android Open Source-Projekt_ Format.
Der folgende Codeausschnitt zeigt ein Beispiel für den Inhalt der **api.xml**:

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

In diesem Beispiel **api.xml** deklariert eine Klasse in der `android` Paket mit dem Namen `Manifest` , reicht die `java.lang.Object`.

In vielen Fällen ist die menschliche Unterstützung erforderlich, um die Java-API können Sie weitere ".NET wie" machen oder um Probleme zu beheben, die verhindern, die bindungsassembly kompilieren dass. Beispielsweise ist es möglicherweise notwendig, Java-Paketnamen .NET Namespaces, benennen Sie eine Klasse, oder ändern den Rückgabetyp einer Methode.

Diese Änderungen werden nicht erreicht, indem **api.xml** direkt.
Stattdessen werden die Änderungen in bestimmten XML-Dateien aufgezeichnet, die von der Vorlage binden von Java-Bibliothek bereitgestellt werden. Beim Kompilieren der Assembly der Xamarin.Android-Bindung wird der Bindings-Generator von diesen Zuordnungsdateien beeinflusst werden beim Erstellen der bindungsassembly

Diese XML-Zuordnungsdateien finden Sie unter den **transformiert** -Ordner des Projekts:

-   **MetaData.xml** &ndash; ermöglicht Änderungen an der endgültigen-API, wie z. B. das Ändern des Namespaces der generierten Bindung hergestellt werden. 

-   **EnumFields.xml** &ndash; enthält die Zuordnung zwischen Java `int` Konstanten und C# `enums` . 

-   **EnumMethods.xml** &ndash; ermöglicht das Ändern von Methodenparametern und Rückgabetypen von Java `int` Konstanten C# `enums` . 

Die **MetaData.xml** Datei sind die meisten Importieren dieser Dateien allgemeine Änderungen an der Bindung, z. B.:

-   Namespaces, Klassen, Methoden oder Felder umbenannt werden, damit sie .NET-Konventionen beachten. 

-   Entfernen von Namespaces, Klassen, Methoden oder Felder, die nicht benötigt werden. 

-   Verschieben von Klassen zu unterschiedlichen Namespaces. 

-   Hinzufügen zusätzlicher Support-Klassen, stellen den Entwurf der Bindung führen Sie die .NET Framework-Muster. 

Ermöglicht, fahren Sie mit diskutieren **Metadata.xml** im Detail.


## <a name="metadataxml-transform-file"></a>Metadata.XML-Transform-Datei

Wie wir haben bereits gesehen haben, die Datei **Metadata.xml** von den Bindungen-Generator verwendet, um die Erstellung der bindungsassembly beeinflussen.
Im Metadaten-Format [XPath](https://www.w3.org/TR/xpath/) Syntax und ist nahezu identisch mit der *GAPI Metadaten* beschriebenen [GAPI Metadaten](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) Guide. Diese Implementierung ist beinahe eine vollständige Implementierung von XPath 1.0 und unterstützt daher in der standard 1.0-Elemente. Diese Datei ist ein leistungsstarker Mechanismus für XPath-Basis zu ändern, hinzufügen, ausblenden oder Verschieben jedes Element oder Attribut in der API-Datei. Alle Elemente der Regel in der Spezifikation für die Metadaten enthalten ein Path-Attribut, um den Knoten zu identifizieren, für den die Regel angewendet wird. Die Regeln werden in der folgenden Reihenfolge angewendet:

* **Hinzufügen von Knoten** &ndash; Fügt einen untergeordneten Knoten auf den Knoten, die durch den Path-Attribut angegeben.
* **Attr** &ndash; legt den Wert eines Attributs für das durch den Path-Attribut angegebene Element fest.
* **Entfernen von Knoten** &ndash; entfernt einen angegebenen XPath-Ausdruck übereinstimmenden Knoten.

Folgendes ist ein Beispiel für eine **Metadata.xml** Datei:

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

Die folgende Liste enthält einige der häufiger verwendeten XPath-Elemente für die Java-API:

-   `interface` &ndash; Zum Suchen einer Java-Schnittstelle verwendet. z. B. `/interface[@name='AuthListener']`.

-   `class` &ndash; Verwendet, um eine Klasse nicht gefunden. z. B. `/class[@name='MapView']`.

-   `method` &ndash; Verwendet, um eine Methode für eine Java-Klasse oder Schnittstelle zu suchen. z. B. `/class[@name='MapView']/method[@name='setTitleSource']`.

-   `parameter` &ndash; Identifizieren Sie einen Parameter für eine Methode an. Beispiel: `/parameter[@name='p0']`



### <a name="adding-types"></a>Hinzufügen von Typen

Die `add-node` Element informiert das Xamarin.Android-Bindung-Projekt hinzufügen zu eine neuen Wrapperklasse **api.xml**. Im folgende Codeausschnitt wird beispielsweise der Binding-Generator zum Erstellen einer Klasse mit einem Konstruktor und ein einzelnes Feld leiten:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```


### <a name="removing-types"></a>Entfernen von Typen

Es ist möglich, weisen Sie den Xamarin.Android-Bindungen-Generator einen Java-Typ ignoriert und nicht gebunden. Dies erfolgt durch Hinzufügen einer `remove-node` zu XML-Element der **metadata.xml** Datei:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Umbenennen von Elementen

Umbenennen von Elementen kann nicht durchgeführt werden, durch direktes Bearbeiten der **api.xml** -Datei, da Xamarin.Android die ursprünglichen Namen der Java Native Interface (JNI) ist erforderlich. Aus diesem Grund die `//class/@name` Attribut kann nicht geändert werden, wenn es sich handelt, die Bindung funktioniert nicht.

Betrachten Sie den Fall, wollen wir einen Typ umbenennen `android.Manifest`.
Zu diesem Zweck können wir versuchen, direkt zu bearbeiten **api.xml** und benennen Sie die Klasse wie folgt:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Dies führt zu den Bindungen-Generator, erstellen die folgenden C# Code für die Wrapperklasse:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Beachten Sie, die die Wrapperklasse um umbenannt wurde `NewName`, während die ursprüngliche Java-Typ noch `Manifest`. Es ist nicht mehr möglich, dass die Xamarin.Android-Binding-Klasse zum Zugriff auf alle Methoden in `android.Manifest`; die Wrapperklasse in einen nicht vorhandenen Java-Typ gebunden ist.

Um der verwaltete Name der umschlossenen Typs (oder einer Methode) ordnungsgemäß zu ändern, es ist notwendig, legen Sie die `managedName` -Attribut wie im folgenden Beispiel gezeigt:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>Umbenennen von `EventArg` Wrapperklassen

Bei der Xamarin.Android-Bindung Generator identifiziert eine `onXXX` Setter-Methode für eine _Listenertyp_, C# Ereignis und `EventArgs` Unterklasse generiert werden Zusatz Sie zur Unterstützung von .NET API für die Java-basierte Listener-Muster. Betrachten Sie beispielsweise die folgende Java-Klasse und Methode ein:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android löscht das Präfix `on` aus der Settermethode und verwenden Sie stattdessen `2DSignNextManuever` als Grundlage für den Namen der `EventArgs` Unterklasse. Die Unterklasse wird etwas Ähnliches wie in benannt werden:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Dies ist nicht zulässigen C# Klassenname. Um dieses Problem zu beheben, muss der Autor der Bindung verwenden die `argsType` Attribut, und geben Sie einen gültigen C# für die `EventArgs` Unterklasse:
 
```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

 

## <a name="supported-attributes"></a>Unterstützte Attribute

In den folgenden Abschnitten werden einige der Attribute für das Transformieren von Java-APIs beschrieben.

### <a name="argstype"></a>argsType

Dieses Attribut befindet sich auf die Setter-Methoden zum Benennen der `EventArg` Unterklasse, die zur Unterstützung von Java-Listener generiert werden. Hierzu finden Sie weiter unten ausführlicher im Abschnitt [EventArg-Wrapperklassen umbenennen](#Renaming_EventArg_Wrapper_Classes) später in diesem Handbuch.

### <a name="eventname"></a>eventName

Gibt einen Namen für ein Ereignis. Wenn die Sequenz leer ist, unterdrückt sie ereignisgenerierung.
Dies wird in den Abschnittstiteln ausführlicher beschrieben [EventArg-Wrapperklassen umbenennen](#Renaming_EventArg_Wrapper_Classes).

### <a name="managedname"></a>managedName

Dies wird verwendet, um den Namen des Pakets, Klasse, Methode oder Parameter zu ändern. Z. B. der Java-Klasse umbenennen `MyClass` zu `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

Das folgende Beispiel veranschaulicht einen XPath-Ausdruck für die Methode umbenennen `java.lang.object.toString` zu `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` wird verwendet, um den Rückgabetyp einer Methode ändern. In einigen Situationen folgert den Generator Bindungen nicht ordnungsgemäß den Rückgabetyp einer Java-Methode, was zu einem Fehler zur Kompilierzeit führt. Eine mögliche Lösung in diesem Fall ist den Rückgabetyp der Methode ändern.

Der Generator Bindungen beispielsweise davon ausgeht, die die Java-Methode `de.neom.neoreadersdk.resolution.compareTo()` zurückgeben sollte ein `int`, was dazu führt, in der Fehlermeldung **Fehler CS0535: ' DE. Neom.Neoreadersdk.Resolution' ist nicht implementiert den Schnittstellenmember "Java.Lang.IComparable.CompareTo(Java.Lang.Object)"**. Der folgende Codeausschnitt zeigt, wie Sie den Rückgabetyp der generierten ändern C# Methode aus einem `int` auf eine `Java.Lang.Object`: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]"name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Ändert sich den Rückgabetyp einer Methode. Dies ändert die return-Attribut nicht (wie Änderungen zurückgegeben, dass Attribute auf die JNI-Signatur nicht kompatiblen Änderungen führen können). Im folgenden Beispiel ist der Rückgabetyp von den `append` Methode von geändert wird `SpannableStringBuilder` zu `IAppendable` (zur Erinnerung: C# kovariante Rückgabetypen nicht unterstützt):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>verborgen

Tools, mit denen Verschleiern von Java-Bibliotheken der Generator für die Xamarin.Android-Bindung und die Möglichkeit zum Generieren beeinträchtigen C# Wrapperklassen. Merkmale der verborgenen Klassen enthalten: * enthält den Namen der Klasse eine **$**, d. h. **eine$ .class** * den Namen der Klasse von Kleinbuchstaben, d. h. vollständig gefährdet  **a.Class**

Dieser Codeausschnitt ist ein Beispiel zum Generieren einer "nicht verborgenen" C# Typ:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Dieses Attribut kann verwendet werden, um den Namen einer verwalteten Eigenschaft ändern.

Ein spezialisierter Fall der Verwendung von `propertyName` umfasst die Situation, in denen eine Java-Klasse nur eine Getter-Methode für ein Feld hat. In diesem Fall möchten den Generator Bindung eine Nur-Schreiben-Eigenschaft, etwas zu erstellen, die in .NET wird nicht empfohlen. Der folgende Codeausschnitt zeigt die Eigenschaften von .NET durch Festlegen von "entfernen" die `propertyName` auf eine leere Zeichenfolge:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Beachten Sie, dass die Setter- und Getter-Methoden immer noch durch den Generator Bindungen erstellt werden.

### <a name="sender"></a>sender

Gibt an, welche Parameter einer Methode sein, sollte die `sender` Parameter an, wenn die Methode nicht auf ein Ereignis zugeordnet ist. Der Wert kann sein `true` oder `false`. Zum Beispiel:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>Sichtbarkeit

Dieses Attribut wird verwendet, um die Sichtbarkeit der Klasse, Methode oder Eigenschaft zu ändern. Angenommen, sie müssen ggf. Förderung einer `protected` Java-Methode so, dass die It entsprechende C# Wrapper `public`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml und EnumMethods.xml

Es gibt Fälle, in dem Android-Bibliotheken ganzzahlige Konstanten verwenden, um Zustände darzustellen, die auf Eigenschaften oder Methoden der Clientbibliotheken übergeben werden. In vielen Fällen ist es sinnvoll, diese ganzzahlige Konstanten, Enumerationen in binden C#. Um diese Zuordnung zu erleichtern, verwenden Sie die **EnumFields.xml** und **EnumMethods.xml** Dateien in Ihrem bindungsprojekt. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Definieren einer Enumeration mit EnumFields.xml

Die **EnumFields.xml** -Datei enthält die Zuordnung zwischen Java `int` Konstanten und C# `enums`. Im folgende Beispiel sehen wir uns ein C# Enumeration, die für einen Satz von zu erstellenden `int` Konstanten: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Hier habe ich die Java-Klasse `SKRealReachSettings` und definiert eine C# Enumeration namens `SKMeasurementUnit` im Namespace `Skobbler.Ngx.Map.RealReach`. Die `field` Einträge definiert den Namen der Java-Konstante (Beispiel `UNIT_SECOND`), den Namen der Enum-Eintrags (Beispiel `Second`), und der ganzzahlige Wert durch beide Entitäten dargestellt (Beispiel `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Definieren mithilfe von EnumMethods.xml Getter/Setter-Methoden

Die **EnumMethods.xml** Datei ermöglicht das Ändern von Methodenparametern und Rückgabetypen von Java `int` Konstanten C# `enums`. Das heißt, es zugeordnet, das Lesen und Schreiben von C# Enumerationen (definiert der **EnumFields.xml** Datei) zu Java `int` Konstanten `get` und `set` Methoden.

Erhält die `SKRealReachSettings` -Enumeration definiert, über die folgenden **EnumMethods.xml** Datei würde die Getter/Setter für diese Enumeration definieren:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

Die erste `method` Zeile zugeordnet, den Rückgabewert von der Java `getMeasurementUnit` Methode, um die `SKMeasurementUnit` Enum. Die zweite `method` Zeile zugeordnet, den ersten Parameter der `setMeasurementUnit` der gleichen-Enumeration.

Alle diese Änderungen vorgenommen, Sie können den folgenden Code in Xamarin.Android Festlegen der `MeasurementUnit`: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```


## <a name="summary"></a>Zusammenfassung

In diesem Artikel beschrieben, wie Xamarin.Android zum Transformieren einer API-Definition von Metadaten verwendet die *Google* *AOSP Format*. Verwenden Sie nach der Behandlung von Änderungen, die möglich sind *Metadata.xml*, er überprüft die Einschränkungen, die auftreten, wenn Sie Mitglieder umbenennen und es angezeigt, dass die Liste der unterstützten XML-Attribute, die beschreiben, wann die einzelnen Attribute verwendet werden sollten.



## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [GAPI-Metadaten](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
