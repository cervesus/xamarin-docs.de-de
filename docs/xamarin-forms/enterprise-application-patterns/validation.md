---
title: Validierung in Unternehmens-apps
description: In diesem Kapitel wird erläutert, wie die eshoponcontainers-Mobile App die Validierung von Benutzereingaben durchführt. Dies umfasst das Angeben von Validierungsregeln, das Auslösen der Validierung und das Anzeigen von Validierungs Fehlern.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: de5728710a408b8e0c7c68dc89c7e6484cbcc3ce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760165"
---
# <a name="validation-in-enterprise-apps"></a>Validierung in Unternehmens-apps

Jede APP, die Eingaben von Benutzern akzeptiert, sollte sicherstellen, dass die Eingabe gültig ist. Eine APP könnte z. b. auf Eingaben überprüfen, die nur Zeichen in einem bestimmten Bereich enthalten, eine bestimmte Länge aufweisen oder einem bestimmten Format entsprechen. Ohne Validierung kann ein Benutzerdaten bereitstellen, die dazu führen, dass die APP fehlschlägt. Die Validierung erzwingt Geschäftsregeln und verhindert, dass ein Angreifer schädliche Daten eingibt.

Im Zusammenhang mit dem Model-View-ViewModel (MVVM)-Muster ist häufig ein Ansichts Modell oder-Modell erforderlich, um die Datenüberprüfung auszuführen und Validierungs Fehler an die Sicht zu senden, damit Sie vom Benutzer korrigiert werden kann. Der eshoponcontainers-Mobile App führt eine synchrone Client seitige Validierung der Ansichts Modell Eigenschaften aus und benachrichtigt den Benutzer über Validierungs Fehler, indem er das Steuerelement, das die ungültigen Daten enthält, hervorhebt und Fehlermeldungen anzeigt, die den Benutzer informieren. der Gründe, warum die Daten ungültig sind. Abbildung 6-1 zeigt die Klassen, die zum Ausführen der Validierung in den eshoponcontainers-Mobile App beteiligt sind.

[Validierungs Klassen in der eshoponcontainers-Mobile App ![(validation-images/validation.png " ")]] (validation-images/validation-large.png#lightbox "Validierungs Klassen in der eshoponcontainers-Mobile App")

**Abbildung 6-1**: Validierungs Klassen in der eshoponcontainers-Mobile App

Die Anzeige von Modell Eigenschaften, die eine `ValidatableObject<T>` `Validations` Überprüfung erfordern, `ValidatableObject<T>` ist vom Typ, und jeder Instanz werden Validierungsregeln hinzugefügt. Die Validierung wird vom Ansichts Modell aufgerufen, indem `Validate` die-Methode `ValidatableObject<T>` der-Instanz aufgerufen wird, die die Validierungsregeln abruft und `ValidatableObject<T>` Sie für die `Value` -Eigenschaft ausführt. Alle Validierungs `Errors` Fehler werden in der `ValidatableObject<T>` `IsValid` -Eigenschaft der-Instanz platziert, und die-Eigenschaft der- Instanzwirdaktualisiert,umanzugeben,obdieÜberprüfungerfolgreichwaroderfehlgeschlagenist.`ValidatableObject<T>`

Die Benachrichtigung über die Eigenschafts Änderung `ExtendedBindableObject` wird von der-Klasse [`Entry`](xref:Xamarin.Forms.Entry) bereitgestellt. Daher kann `IsValid` ein Steuer `ValidatableObject<T>` Element an die-Eigenschaft der-Instanz in der Ansichts Modell Klasse binden, um darüber informiert zu werden, ob die eingegebenen Daten gültig sind.

## <a name="specifying-validation-rules"></a>Angeben von Validierungsregeln

Validierungsregeln werden angegeben, indem eine Klasse erstellt wird, die `IValidationRule<T>` von der-Schnittstelle abgeleitet wird. Dies wird im folgenden Codebeispiel gezeigt:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Diese Schnittstelle gibt an, dass eine Validierungs Regelklasse `boolean` eine `Check` Methode bereitstellen muss, die zum Ausführen der erforderlichen Validierung `ValidationMessage` verwendet wird, und eine-Eigenschaft, deren Wert die Validierungs Fehlermeldung ist, die angezeigt wird, wenn die Validierung schlägt fehl.

Das folgende Codebeispiel zeigt die `IsNotNullOrEmptyRule<T>` Validierungs Regel, die zum Überprüfen des Benutzernamens und des Kennworts verwendet wird, die der Benutzer `LoginView` in verwendet, wenn Mock-Dienste im Mobile App eshoponcontainers verwendet werden:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

Die `Check` -Methode gibt `boolean` einen zurück, der angibt, `null`ob das Wert Argument, leer ist oder nur aus Leerzeichen besteht.

Obwohl es nicht von der eshoponcontainers-Mobile App verwendet wird, zeigt das folgende Codebeispiel eine Validierungs Regel zum Überprüfen von e-Mail-Adressen an:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

Die `Check` Methode gibt einen `boolean` Wert zurück, der angibt, ob das value-Argument eine gültige e-Mail-Adresse ist. Dies wird erreicht, indem das Wert-Argument nach dem ersten Vorkommen des Musters für reguläre Ausdrücke durchsucht `Regex` wird, das im Konstruktor angegeben ist. Ob das Muster für `Match` reguläre Ausdrücke in der Eingabe Zeichenfolge gefunden wurde, kann durch Überprüfen des Werts der- `Success` Eigenschaft des-Objekts ermittelt werden.

> [!NOTE]
> Die Eigenschaften Validierung kann manchmal abhängige Eigenschaften einschließen. Ein Beispiel für abhängige Eigenschaften ist, wenn der Satz gültiger Werte für Eigenschaft A von dem bestimmten Wert abhängt, der in Eigenschaft B festgelegt wurde. Um zu überprüfen, ob der Wert von Eigenschaft a einem der zulässigen Werte entspricht, würde den Wert der Eigenschaft B abrufen. Wenn sich der Wert der Eigenschaft B ändert, muss die Eigenschaft A erneut überprüft werden.

## <a name="adding-validation-rules-to-a-property"></a>Hinzufügen von Validierungsregeln zu einer Eigenschaft

Im eshoponcontainers-Mobile App werden Modell Eigenschaften, die eine Validierung erfordern, als Typ `ValidatableObject<T>`deklariert, wobei `T` der Typ der zu validierenden Daten ist. Das folgende Codebeispiel zeigt ein Beispiel mit zwei Eigenschaften dieser Art:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Damit die Validierung ausgeführt werden kann, müssen der `Validations` Auflistung der einzelnen `ValidatableObject<T>` Instanzen Validierungsregeln hinzugefügt werden, wie im folgenden Codebeispiel gezeigt:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Mit dieser Methode wird `IsNotNullOrEmptyRule<T>` die Validierungs Regel der-Auflistung jeder `ValidatableObject<T>` Instanz hinzugefügt. dabei werden Werte für die `ValidationMessage` -Eigenschaft der Validierungs Regel angegeben, die die Validierungs Fehlermeldung angibt, die angezeigt wird, wenn `Validations` die Validierung schlägt fehl.

## <a name="triggering-validation"></a>Auslösen der Validierung

Der Überprüfungs Ansatz, der in den eshoponcontainers-Mobile App verwendet wird, kann die Validierung einer Eigenschaft manuell auslöst und die Validierung automatisch auslöst, wenn sich eine Eigenschaft ändert.

### <a name="triggering-validation-manually"></a>Manuelles Auslösen der Validierung

Die Validierung kann für eine Ansichts Modell Eigenschaft manuell ausgelöst werden. Dies tritt z. b. in den eshoponcontainers-Mobile App auf, wenn der Benutzer auf die `LoginView`Anmelde Schaltfläche auf dem tippt, wenn Mock-Dienste verwendet werden. Der Befehls Delegat ruft `MockSignInAsync` die-Methode `LoginViewModel`in der auf, die die Validierung `Validate` aufruft, indem die-Methode ausgeführt wird. Dies wird im folgenden Codebeispiel gezeigt:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

Die `Validate` -Methode führt die Validierung des Benutzernamens und des Kennworts durch, `LoginView`die vom Benutzer auf dem eingegeben wurden, `ValidatableObject<T>` indem die Validate-Methode für jede Instanz aufgerufen wird. Das folgende Codebeispiel zeigt die Validate-Methode aus `ValidatableObject<T>` der-Klasse:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Diese Methode löscht die `Errors` -Auflistung und ruft dann alle Validierungsregeln ab, die der- `Validations` Auflistung des-Objekts hinzugefügt wurden. Die `Check` -Methode für jede abgerufene Validierungs Regel wird ausgeführt, `ValidationMessage` und der-Eigenschafts Wert für jede Validierungs Regel, die die Daten nicht über `Errors` prüfen kann, `ValidatableObject<T>` wird der-Auflistung der-Instanz hinzugefügt. Schließlich wird die `IsValid` -Eigenschaft festgelegt, und ihr Wert wird an die Aufruf Methode zurückgegeben, die angibt, ob die Überprüfung erfolgreich war oder fehlgeschlagen ist.

### <a name="triggering-validation-when-properties-change"></a>Auslösen der Validierung beim Ändern von Eigenschaften

Die Validierung kann auch ausgelöst werden, wenn eine gebundene Eigenschaft geändert wird. Wenn z. b. eine bidirektionale Bindung `LoginView` in der `UserName` die `Password` -Eigenschaft oder die-Eigenschaft festlegt, wird die Validierung ausgelöst. Im folgenden Codebeispiel wird veranschaulicht, wie dies geschieht:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

Das [`Entry`](xref:Xamarin.Forms.Entry) -Steuerelement wird `UserName.Value` an die- `ValidatableObject<T>` Eigenschaft der-Instanz gebunden, `Behaviors` und die- `EventToCommandBehavior` Auflistung des Steuer Elements verfügt über eine-Instanz. Dieses Verhalten führt den `ValidateUserNameCommand` in Reaktion auf das [`TextChanged` `Entry`]-Ereignis aus, das in ausgelöst wird, das ausgelöst wird, `Entry` wenn sich der Text in der ändert. Der `ValidateUserNameCommand` Delegat führt wiederum die `ValidateUserName` -Methode aus, die die `Validate` -Methode für `ValidatableObject<T>` die-Instanz ausführt. Daher wird jedes Mal, wenn der Benutzer ein Zeichen im `Entry` -Steuerelement für den Benutzernamen eingibt, die Überprüfung der eingegebenen Daten durchgeführt.

Weitere Informationen zu Verhalten finden Sie unter [Implementieren von Verhalten](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Anzeigen von Validierungs Fehlern

Der eshoponcontainers-Mobile App benachrichtigt den Benutzer über Validierungs Fehler, indem das Steuerelement, das die ungültigen Daten enthält, mit einer roten Zeile hervorgehoben wird, und zeigt eine Fehlermeldung an, die den Benutzer darüber informiert, warum die Daten unter dem Steuerelement mit dem Ungültige Daten. Wenn die ungültigen Daten korrigiert werden, wird die Zeile in Schwarz geändert, und die Fehlermeldung wird entfernt. In Abbildung 6-2 wird die LoginView in den eshoponcontainers-Mobile App angezeigt, wenn Validierungs Fehler vorhanden sind.

![](validation-images/validation-login.png "Anzeigen von Validierungs Fehlern während der Anmeldung")

**Abbildung 6-2:** Anzeigen von Validierungs Fehlern während der Anmeldung

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Markieren eines Steuer Elements, das ungültige Daten enthält

Das `LineColorBehavior` angefügte Verhalten wird verwendet, [`Entry`](xref:Xamarin.Forms.Entry) um Steuerelemente hervorzuheben, bei denen Validierungs Fehler aufgetreten sind. Im folgenden Codebeispiel wird gezeigt, `LineColorBehavior` wie das angefügte Verhalten an `Entry` ein-Steuerelement angefügt wird:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

Das [`Entry`](xref:Xamarin.Forms.Entry) -Steuerelement verwendet einen expliziten Stil, der im folgenden Codebeispiel gezeigt wird:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Dieser Stil legt die `ApplyLineColor` angefügten Eigenschaften und `LineColor` des `LineColorBehavior` angefügten Verhaltens für [`Entry`](xref:Xamarin.Forms.Entry) das Steuerelement fest. Weitere Informationen zu Formatvorlagen finden Sie unter [Styles (Formatvorlagen)](~/xamarin-forms/user-interface/styles/index.md).

Wenn der Wert der `ApplyLineColor` angefügten-Eigenschaft festgelegt oder geändert wird, führt das `LineColorBehavior` angefügte `OnApplyLineColorChanged` Verhalten die-Methode aus, die im folgenden Codebeispiel gezeigt wird:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Mit den Parametern für diese Methode wird die Instanz des-Steuer Elements bereitgestellt, an die das Verhalten angefügt ist, sowie `ApplyLineColor` die alten und neuen Werte der angefügten-Eigenschaft. Die `EntryLineColorEffect` -Klasse wird der- [`Effects`](xref:Xamarin.Forms.Element.Effects) Auflistung des-Steuer Elements `ApplyLineColor` hinzugefügt, `true`wenn die angefügte-Eigenschaft ist, andernfalls `Effects` wird Sie aus der-Auflistung des Steuer Elements entfernt. Weitere Informationen zu Verhalten finden Sie unter [Implementieren von Verhalten](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

Die `EntryLineColorEffect` Unterklassen der [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) -Klasse und werden im folgenden Codebeispiel gezeigt:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

Die [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) -Klasse stellt einen plattformunabhängigen Effekt dar, der einen inneren Effekt umschließt, der plattformspezifisch ist. Dadurch wird das Entfernen eines Effekts vereinfacht, da während der Kompilierzeit nicht auf die Typinformationen für einen plattformspezifischen Effekt zugegriffen werden kann. Der `EntryLineColorEffect` Ruft den Basisklassenkonstruktor auf und übergibt einen Parameter, der aus einer Verkettung des Auflösungs Gruppennamens besteht, sowie die eindeutige ID, die für jede plattformspezifische Effekt Klasse angegeben wird.

Das folgende Codebeispiel zeigt die `eShopOnContainers.EntryLineColorEffect` Implementierung für ios:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

Die `OnAttached` -Methode ruft das Native-Steuerelement für das xamarin [`Entry`](xref:Xamarin.Forms.Entry) . Forms-Steuerelement ab und aktualisiert die Linien `UpdateLineColor` Farbe durch Aufrufen der-Methode. Die `OnElementPropertyChanged` `LineColor` - [`Height`](xref:Xamarin.Forms.VisualElement.Height) Überschreibung antwortet auf Änderungen der bindbaren Eigenschaften des- `Entry` SteuerElements,indemdieLinienFarbeaktualisiertwird,wennsichdieangefügte-Eigenschaftändert,oderdie-EigenschaftderÄnderungen.`Entry` Weitere Informationen zu Effekten finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

Wenn im [`Entry`](xref:Xamarin.Forms.Entry) Steuerelement gültige Daten eingegeben werden, wird am unteren Rand des Steuer Elements eine schwarze Linie angewendet, um anzugeben, dass kein Validierungs Fehler vorliegt. In Abbildung 6-3 wird ein Beispiel dafür gezeigt.

![](validation-images/validation-blackline.png "Schwarze Zeile, die keinen Validierungs Fehler anzeigt.")

**Abbildung 6-3**: Schwarze Zeile, die keinen Validierungs Fehler anzeigt.

Dem [`Entry`](xref:Xamarin.Forms.Entry) -Steuerelement wird [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) auch ein hinzu [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) gefügt. Das folgende Codebeispiel zeigt das `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Dadurch [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) `UserName.IsValid` `false` `LineColor` wirddie`LineColorBehavior` -Eigenschaft überwacht, und wenn der Wert ist, wird der ausgeführt [,derdieangefügte-EigenschaftdesangefügtenVerhaltensin"Red"ändert.`Setter`](xref:Xamarin.Forms.Setter) In Abbildung 6-4 wird ein Beispiel dafür gezeigt.

![](validation-images/validation-redline.png "Rote Linie zur Angabe eines Validierungs Fehlers")

**Abbildung 6-4**: Rote Linie zur Angabe eines Validierungs Fehlers

Die Zeile im [`Entry`](xref:Xamarin.Forms.Entry) Steuerelement bleibt rot, während die eingegebenen Daten ungültig sind. andernfalls wird Sie in Schwarz geändert, um anzugeben, dass die eingegebenen Daten gültig sind.

Weitere Informationen zu Triggern finden Sie unter [Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Anzeigen von Fehlermeldungen

Die Benutzeroberfläche zeigt Validierungs Fehlermeldungen in Bezeichnung-Steuerelementen unterhalb jedes Steuer Elements an, dessen Daten nicht überprüft werden konnten Das folgende Codebeispiel zeigt [`Label`](xref:Xamarin.Forms.Label) , wie eine Validierungs Fehlermeldung angezeigt wird, wenn der Benutzer keinen gültigen Benutzernamen eingegeben hat:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Jeder [`Label`](xref:Xamarin.Forms.Label) bindet an die `Errors` -Eigenschaft des Ansichts Modell Objekts, das überprüft wird. Die `Errors` -Eigenschaft wird von der `ValidatableObject<T>` -Klasse bereitgestellt und ist `List<string>`vom Typ. Da die `Errors` -Eigenschaft mehrere Validierungs Fehler enthalten kann `FirstValidationErrorConverter` , wird die-Instanz verwendet, um den ersten Fehler aus der-Auflistung für die Anzeige abzurufen.

## <a name="summary"></a>Zusammenfassung

Der eshoponcontainers-Mobile App führt eine synchrone Client seitige Validierung der Ansichts Modell Eigenschaften aus und benachrichtigt den Benutzer über Validierungs Fehler, indem er das Steuerelement, das die ungültigen Daten enthält, hervorhebt und Fehlermeldungen anzeigt, die den Benutzer informieren. Warum die Daten ungültig sind.

Die Anzeige von Modell Eigenschaften, die eine `ValidatableObject<T>` `Validations` Überprüfung erfordern, `ValidatableObject<T>` ist vom Typ, und jeder Instanz werden Validierungsregeln hinzugefügt. Die Validierung wird vom Ansichts Modell aufgerufen, indem `Validate` die-Methode `ValidatableObject<T>` der-Instanz aufgerufen wird, die die Validierungsregeln abruft und `ValidatableObject<T>` Sie für die `Value` -Eigenschaft ausführt. Alle Validierungs `Errors` Fehler werden in der `ValidatableObject<T>` `IsValid` -Eigenschaft der-Instanz platziert, und die-Eigenschaft der- Instanzwirdaktualisiert,umanzugeben,obdieÜberprüfungerfolgreichwaroderfehlgeschlagenist.`ValidatableObject<T>`

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
