---
title: Metadaten für Java-Bindungen
description: C#-Code in Xamarin.Android ruft Java-Bibliotheken über Bindungen auf. Dabei handelt es sich um einen Mechanismus, der die in Java Native Interface (JNI) festgelegten untergeordneten Details abstrahiert. Xamarin.Android umfasst ein Tool, das diese Bindungen generiert. Mit diesem Tool kann der Entwickler mithilfe von Metadaten steuern, wie eine Bindung erstellt wird, um z. B. Namespaces zu ändern und Member umzubenennen. In diesem Dokument wird erläutert, wie Metadaten funktionieren. Es enthält weiterhin einer Übersicht über die von Metadaten unterstützten Attribute und erläutert, wie Bindungsprobleme durch Ändern dieser Metadaten gelöst werden.
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 2a88888b2306589930ad6386fb69bbd3b48924b7
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571375"
---
# <a name="java-bindings-metadata"></a>Metadaten für Java-Bindungen

_C#-Code in Xamarin.Android ruft Java-Bibliotheken über Bindungen auf. Dabei handelt es sich um einen Mechanismus, der die in Java Native Interface (JNI) festgelegten untergeordneten Details abstrahiert. Xamarin.Android umfasst ein Tool, das diese Bindungen generiert. Mit diesem Tool kann der Entwickler mithilfe von Metadaten steuern, wie eine Bindung erstellt wird, um z. B. Namespaces zu ändern und Member umzubenennen. In diesem Dokument wird erläutert, wie Metadaten funktionieren. Es enthält weiterhin einer Übersicht über die von Metadaten unterstützten Attribute und erläutert, wie Bindungsprobleme durch Ändern dieser Metadaten gelöst werden._

## <a name="overview"></a>Übersicht

Eine Xamarin.Android-**Java-Bindungsbibliothek** versucht, einen Großteil der erforderlichen Arbeit für das Binden einer vorhandenen Android-Bibliothek mithilfe eines Tools, das bisweilen als _Bindungsgenerator_ bezeichnet wird, zu automatisieren. Beim Binden einer Java-Bibliothek untersucht Xamarin.Android die Java-Klassen und generiert eine Liste aller Pakete, Typen und Member, die gebunden werden sollen. Diese Liste der APIs wird in einer XML-Datei gespeichert ( **\{Projektverzeichnis}\obj\Release\api.xml** für einen **RELEASE**-Build und **\{Projektverzeichnis}\obj\Debug\api.xml** für einen **DEBUG**-Build).

![Speicherort der api.xml-Datei im Ordner „obj/Debug“](java-bindings-metadata-images/java-bindings-metadata-01.png)

Der Bindungsgenerator verwendet die **api.xml**-Datei als Richtlinie zum Generieren der erforderlichen C# Wrapperklassen. Der Inhalt dieser XML-Datei ist eine Variante des _Android Open Source Project_-Formats von Google.
Der folgende Codeausschnitt ist ein Beispiel für den Inhalt von **api.xml**:

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

In diesem Beispiel deklariert **api.xml** eine Klasse im `android` Paket namens `Manifest`, die `java.lang.Object` erweitert.

In vielen Fällen ist menschliche Unterstützung erforderlich, damit die Java-API .NET-ähnlicher aussieht oder Probleme korrigiert, die eine Kompilierung der Bindungsassembly verhindern. Es kann beispielsweise erforderlich sein, Java-Paketnamen in .NET-Namespaces zu ändern, eine Klasse umzubenennen oder den Rückgabetyp einer Methode zu ändern.

Diese Änderungen können nicht durch eine direkte Änderung der **api.xml** erreicht werden.
Stattdessen werden Änderungen in speziellen XML-Dateien aufgezeichnet, die von der Java-Bindungsbibliotheksvorlage bereitgestellt werden. Beim Kompilieren der Xamarin.Android-Bindungsassembly wird der Bindungsgenerator beim Erstellen der Bindungsassembly von diesen Zuordnungsdateien beeinflusst.

Diese XML-Zuordnungsdateien befinden sich im Ordner **Transforms** des Projekts:

- **Metadata.xml** &ndash; Ermöglicht die Durchführung von Änderungen an der endgültigen API, z. B. Ändern des Namespaces der generierten Bindung. 

- **EnumFields.xml** &ndash; Enthält die Zuordnung zwischen `int`-Konstanten von Java und `enums` von C#. 

- **EnumMethods.xml** &ndash; Ermöglicht die Änderung von Methodenparametern und Rückgabetypen von `int`-Konstanten von Java in `enums` von C#. 

Die Datei **Metadata.xml** ist die wichtigste dieser Dateien, da sie allgemeine Änderungen an der Bindung ermöglicht, z. B.:

- Umbenennen von Namespaces, Klassen, Methoden oder Feldern, sodass sie den .NET-Konventionen entsprechen. 

- Entfernen von Namespaces, Klassen, Methoden oder Feldern, die nicht benötigt werden. 

- Verschieben von Klassen in andere Namespaces. 

- Hinzufügen weiterer Unterstützungsklassen, damit der Entwurf der Bindung die .NET Framework-Muster befolgt. 

Im Folgenden wird **Metadata.xml** ausführlicher erläutert.

## <a name="metadataxml-transform-file"></a>Transformationsdatei „Metadata.xml“

Wie bereits erwähnt, wird die Datei **Metadata.xml** vom Bindungsgenerator verwendet, um die Erstellung der Bindungsassembly zu beeinflussen.
Das Metadatenformat verwendet [XPath](https://www.w3.org/TR/xpath/)-Syntax und ist nahezu identisch mit den *GAPI-Metadaten*, die in der Anleitung zu [GAPI-Metadaten](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata) beschrieben werden. Diese Implementierung ist fast eine komplette Implementierung von XPath 1.0, sodass Elemente im 1.0-Standard unterstützt werden. Diese Datei ist ein leistungsfähiger XPath-basierter Mechanismus zum Ändern, Hinzufügen, Ausblenden oder Verschieben von Elementen oder Attributen in der API-Datei. Alle Regelelemente in der Metadatenspezifikation enthalten ein Pfadattribut, das den Knoten kennzeichnet, auf den die Regel angewendet werden soll. Die Regeln werden in der folgenden Reihenfolge angewendet:

- **add-node** &ndash; Fügt einen untergeordneten Knoten an den im path-Attribut angegebenen Knoten an.
- **attr** &ndash; Legt den Wert eines Attributs des Elements fest, das durch das path-Attribut angegeben wird.
- **remove-node** &ndash; Entfernt Knoten, die mit einem angegebenen XPath übereinstimmen.

Nachfolgend finden Sie ein Beispiel für eine **Metadata.xml**-Datei:

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

Nachstehend sind einige der gängigsten XPath-Elemente für die Java-APIs aufgeführt:

- `interface` &ndash; Wird verwendet, um eine Java-Schnittstelle zu finden. Beispiel: `/interface[@name='AuthListener']`.

- `class` &ndash; Wird verwendet, um eine Klasse zu finden. Beispiel: `/class[@name='MapView']`.

- `method` &ndash; Wird verwendet, um eine Methode in einer Java-Klasse oder -Schnittstelle zu finden. Beispiel: `/class[@name='MapView']/method[@name='setTitleSource']`.

- `parameter` &ndash; Kennzeichnet einen Parameter für eine Methode. Beispiel: `/parameter[@name='p0']`

### <a name="adding-types"></a>Hinzufügen von Typen

Das `add-node`-Element weist das Xamarin.Android-Bindungsprojekt an, eine neue Wrapperklasse zu **api.xml** hinzuzufügen. Der folgende Codeausschnitt weist den Bindungsgenerator beispielsweise an, eine Klasse mit einem Konstruktor und einem einzelnen Feld zu erstellen:

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>Entfernen von Typen

Es ist möglich, den Xamarin.Android-Bindungsgenerator anzuweisen, einen Java-Typ zu ignorieren und nicht zu binden. Dies erfolgt durch Hinzufügen eines `remove-node`-XML-Elements zur **Metadata.xml**-Datei:

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>Umbenennen von Membern

Das Umbenennen von Membern ist nicht durch direktes Bearbeiten der **api.xml**-Datei möglich, da Xamarin.Android die ursprünglichen JNI-Namen (Java Native Interface) benötigt. Daher kann das `//class/@name`-Attribut nicht geändert werden. Andernfalls funktioniert die Bindung nicht.

Stellen Sie sich vor, dass der Typ `android.Manifest` umbenannt werden soll.
Um dies zu erreichen, könnten Sie versuchen, die **api.xml** direkt zu bearbeiten und die Klasse wie folgt umzubenennen:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

Dies führt dazu, dass der Bindungsgenerator den folgenden C# Code für die Wrapperklasse erstellt:

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

Beachten Sie, dass die Wrapperklasse in `NewName` umbenannt wurde, während der ursprüngliche Java-Typ weiterhin `Manifest` ist. Die Xamarin.Android-Bindungsklasse kann daher mehr auf Methoden von `android.Manifest` zugreifen, da die Wrapperklasse an einen nicht vorhandenen Java-Typ gebunden ist.

Um den verwalteten Namen eines Wrappertyps (oder einer Wrappermethode) ordnungsgemäß zu ändern, müssen Sie das `managedName`-Attribut wie im folgenden Beispiel gezeigt festlegen:

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes"></a>

#### <a name="renaming-eventarg-wrapper-classes"></a>Umbenennen von `EventArg`-Wrapperklassen

Wenn der Xamarin.Android-Bindungsgenerator eine `onXXX`-Settermethode für einen _Listenertyp_ erkennt, werden ein C#-Ereignis und eine `EventArgs`-Unterklasse generiert, damit eine API im .NET-Stil für das Java-basierte Listenermuster unterstützt wird. Betrachten Sie als Beispiel die folgende Java-Klasse und -Methode:

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android löscht das Präfix `on` aus der Settermethode und verwendet stattdessen `2DSignNextManuever` als Grundlage für den Namen der `EventArgs`-Unterklasse. Der Name der Unterklasse lautet in etwa wie folgt:

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

Dies ist kein gültiger C#-Klassenname. Um dieses Problem zu beheben, muss der Bindungsersteller das `argsType`-Attribut verwenden und einen gültigen C#-Namen für die `EventArgs`-Unterklasse angeben:

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>Unterstützte Attribute

In den folgenden Abschnitten werden einige Attribute zum Transformieren von Java-APIs beschrieben.

### <a name="argstype"></a>argsType

Dieses Attribut wird in Settermethoden eingefügt, um die `EventArg`-Unterklasse zu benennen, die zur Unterstützung von Java-Listenern generiert wird. Dies wird ausführlicher im Abschnitt [Umbenennen von EventArg-Wrapperklassen](#Renaming_EventArg_Wrapper_Classes) weiter unten in dieser Anleitung beschrieben.

### <a name="eventname"></a>eventName

Gibt einen Namen für ein Ereignis an. Wenn der Name leer ist, wird die Ereignisgenerierung verhindert.
Dies wird ausführlicher im Abschnitt [Umbenennen von EventArg-Wrapperklassen](#Renaming_EventArg_Wrapper_Classes) beschrieben.

### <a name="managedname"></a>managedName

Dieses Attribut wird verwendet, um den Namen eines Pakets, einer Klasse, einer Methode oder eines Parameters zu ändern, z. B. zum Ändern des Namens der Java-Klasse `MyClass` in `NewClassName`:

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

Das nächste Beispiel veranschaulicht einen XPath-Ausdruck zum Umbenennen der Methode `java.lang.object.toString` in `Java.Lang.Object.NewManagedName`:

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` wird verwendet, um den Rückgabetyp einer Methode zu ändern. In einigen Situationen leitet der Bindungsgenerator nicht den richtigen Rückgabetyp einer Java-Methode ab, sodass es beim Kompilieren zu einem Fehler kommt. Eine mögliche Lösung in dieser Situation besteht darin, den Rückgabetyp der Methode zu ändern.

Der Bindungsgenerator geht z. B. davon aus, dass die Java-Methode `de.neom.neoreadersdk.resolution.compareTo()` einen `int`-Wert zurückgeben und `Object` als Parameter akzeptieren soll. Dies führt zu der Fehlermeldung **Fehler CS0535: 'DE.Neom.Neoreadersdk.Resolution' implementiert den Schnittstellenmember 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' nicht**. Der folgende Codeausschnitt veranschaulicht, wie der Typ des ersten Parameters der generierten C#-Methode von `DE.Neom.Neoreadersdk.Resolution` in `Java.Lang.Object`geändert wird: 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

Ändert den Rückgabetyp einer Methode. Dadurch wird das Rückgabeattribut nicht geändert (da Änderungen an Rückgabeattributen zu inkompatiblen Änderungen der JNI-Signatur führen können). Im folgenden Beispiel wird der Rückgabetyp der `append`-Methode von `SpannableStringBuilder` in `IAppendable` geändert (beachten Sie, dass C# keine kovarianten Rückgabetypen unterstützt):

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obfuscated

Tools, die Java-Bibliotheken verschleiern, können den Xamarin.Android-Bindungsgenerator und seine Fähigkeit zum Generieren von C#-Wrapperklassen beeinträchtigen. Zu den Merkmalen von verschleierten Klassen gehören: 

- Der Klassenname enthält ein **$** , z. B. **a$.class**.
- Der Klassenname besteht ausschließlich aus Kleinbuchstaben, z. B. **a.class**.

Dieser Codeausschnitt ist ein Beispiel für die Generierung eines „nicht verschleierten“ C#-Typs:

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

Dieses Attribut kann verwendet werden, um den Namen einer verwalteten Eigenschaft zu ändern.

Ein Sonderfall für die Verwendung von `propertyName` ist die Situation, in der eine Java-Klasse nur über eine Gettermethode für ein Feld verfügt. In diesem Fall möchte der Bindungsgenerator eine schreibgeschützte Eigenschaft erstellen, wovon in .NET abgeraten wird. Der folgende Codeausschnitt zeigt, wie die .NET-Eigenschaften „entfernt“ werden, indem `propertyName` auf eine leere Zeichenfolge festgelegt wird:

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

Beachten Sie, dass die Setter- und Gettermethoden weiterhin vom Bindungsgenerator erstellt werden.

### <a name="sender"></a>sender

Gibt an, welcher Parameter einer Methode der `sender`-Parameter sein soll, wenn die Methode einem Ereignis zugeordnet wird. Der Wert kann `true` oder `false` lauten. Zum Beispiel:

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>Sichtbarkeit

Dieses Attribut wird verwendet, um die Sichtbarkeit einer Klasse, Methode oder Eigenschaft zu ändern. Beispielsweise kann es erforderlich sein, eine `protected`-Java-Methode höher zu stufen, damit der zugehörige C#-Wrapper `public` lautet:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>„EnumFields.xml“ und „EnumMethods.xml“

Es gibt Fälle, in denen Android-Bibliotheken ganzzahlige Konstanten zum Darstellen von Zuständen verwenden, die an Eigenschaften oder Methoden der Bibliotheken übergeben werden. In vielen Fällen ist es hilfreich, diese ganzzahligen Konstanten in C# an Enumerationen zu binden. Um diese Zuordnung zu vereinfachen, können Sie die Dateien **EnumFields.xml** und **EnumMethods.xml** im Bindungsprojekt verwenden. 

### <a name="defining-an-enum-using-enumfieldsxml"></a>Definieren einer Enumeration mit „EnumFields.xml“

Die Datei **EnumFields.xml** enthält die Zuordnung zwischen `int`-Konstanten von Java und `enums` von C#. Betrachten Sie das folgende Beispiel für eine C# Enumeration, die für mehrere `int`-Konstanten erstellt wird: 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

Hier wird die Java-Klasse `SKRealReachSettings` verwendet, und es wird eine C# Enumeration mit dem Namen `SKMeasurementUnit` im Namespace `Skobbler.Ngx.Map.RealReach` definiert. Die `field`-Einträge definieren den Namen der Java-Konstanten (z. B. `UNIT_SECOND`), den Namen des Enumerationseintrags (z. B. `Second`) und den ganzzahligen Wert, der von beiden Entitäten dargestellt wird (z. B. `0`). 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>Definieren von Getter-/Settermethoden mit „EnumMethods.xml“

Die Datei **EnumMethods.xml** ermöglicht die Änderung von Methodenparametern und Rückgabetypen von `int`-Konstanten von Java in `enums` von C#. Das heißt, das Lesen und Schreiben von C#-Enumerationen (definiert in der Datei **EnumFields.xml**) wird in Java `int`-Konstanten sowie `get`- und `set`-Methoden zugeordnet.

Bei der oben definierten `SKRealReachSettings`-Enumeration definiert die folgende **EnumMethods.xml**-Datei den Getter/Setter für diese Enumeration:

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

In der ersten `method`-Zeile wird der Rückgabewert der Java-Methode `getMeasurementUnit` der `SKMeasurementUnit`-Enumeration zugeordnet. In der zweiten `method`-Zeile wird der erste Parameter von `setMeasurementUnit` derselben Enumeration zugeordnet.

Wenn alle diese Änderungen vorgenommen wurden, können Sie den folgenden Code in Xamarin.Android verwenden, um `MeasurementUnit` festzulegen: 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Xamarin.Android Metadaten verwendet, um eine API-Definition aus dem *Google* *AOSP-Format* zu transformieren. Nachdem die mit *Metadata.xml* möglichen Änderungen behandelt wurden, wurden die Einschränkungen beim Umbenennen von Membern untersucht. Anschließend wurden die unterstützten XML-Attribute mit einer Beschreibung aufgelistet, wann die einzelnen Attribute verwendet werden sollen.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [GAPI-Metadaten](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
