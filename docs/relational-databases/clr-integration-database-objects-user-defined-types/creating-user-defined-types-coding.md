---
title: Codage de Types définis par l’utilisateur | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- validation [CLR integration]
- UDTs [CLR integration], coding
- UserDefined serialization format [CLR integration]
- Point UDT
- Parse method
- null values [CLR integration]
- ToString method
- ValidatePoint method
- attributes [CLR integration]
- coding user-defined types [CLR integration]
- Read method
- Write method
- nullability [CLR integration]
- user-defined types [CLR integration], coding
- Currency UDT
- validating UDT values
- exposing UDT properties [CLR integration]
ms.assetid: 1e5b43b3-4971-45ee-a591-3f535e2ac722
caps.latest.revision: 37
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 69a07f7537b1b5e35e1ff576ba47993ada7a4ce7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-user-defined-types---coding"></a>Création de Types définis par l’utilisateur - codage
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lorsque vous codez votre définition de type défini par l'utilisateur (UDT, User-Defined Type), vous devez implémenter différentes fonctionnalités, selon que vous implémentez le type défini par l'utilisateur comme classe ou comme structure, et selon les options de format et de sérialisation que vous avez choisies.  
  
 L’exemple de cette section illustre l’implémentation d’un **Point** UDT comme un **struct** (ou **Structure** en Visual Basic). Le **Point** UDT se compose de X et Y coordonnées implémentés en tant que procédures de propriété.  
  
 Les espaces de noms suivants sont requis lors de la définition d'un type défini par l'utilisateur :  
  
```vb  
Imports System  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
```  
  
```csharp  
using System;  
using System.Data.SqlTypes;  
using Microsoft.SqlServer.Server;  
```  
  
 Le **Microsoft.SqlServer.Server** espace de noms contient les objets requis pour divers attributs de votre UDT et le **System.Data.SqlTypes** espace de noms contient les classes qui représentent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données natifs disponibles pour l’assembly. Il se peut bien sûr que le bon fonctionnement de votre assembly requiert des espaces de noms supplémentaires. Le **Point** UDT utilise également le **System.Text** espace de noms pour l’utilisation de chaînes.  
  
> [!NOTE]  
>  Objets de base de données Visual C++ telles que les types UDT, compilés avec **/CLR : pure** ne sont pas pris en charge pour l’exécution.  
  
## <a name="specifying-attributes"></a>Spécification d'attributs  
 Les attributs déterminent la façon dont la sérialisation est utilisée pour construire la représentation de stockage des types définis par l'utilisateur et pour transmettre des types définis par l'utilisateur par valeur au client.  
  
 Le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** est requis. Le **Serializable** attribut est facultatif. Vous pouvez également spécifier le **Microsoft.SqlServer.Server.SqlFacetAttribute** pour fournir des informations sur le type de retour d’un UDT. Pour plus d’informations, consultez [Attributs personnalisés pour les routines CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md)  
  
### <a name="point-udt-attributes"></a>Attributs du type défini par l'utilisateur Point  
 Le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** définit le format de stockage pour le **Point** UDT **natif**. **IsByteOrdered** a la valeur **true**, ce qui garantit que les résultats des comparaisons sont les mêmes dans SQL Server comme si les comparaisons avaient eu lieu dans le code managé. L’UDT implémente la **System.Data.SqlTypes.INullable** interface pour signaler la valeur null de type UDT.  
  
 Le fragment de code suivant présente les attributs de la **Point** UDT.  
  
```vb  
<Serializable(), SqlUserDefinedTypeAttribute(Format.Native, _  
  IsByteOrdered:=True)> _  
  Public Structure Point  
    Implements INullable  
```  
  
```csharp  
[Serializable]  
[Microsoft.SqlServer.Server.SqlUserDefinedType(Format.Native,  
  IsByteOrdered=true)]  
public struct Point : INullable  
{  
```  
  
## <a name="implementing-nullability"></a>Implémentation de la possibilité de valeur NULL  
 En plus de spécifier correctement les attributs pour vos assemblys, votre type défini par l'utilisateur doit également prendre en charge la possibilité de valeur Null. Les UDT chargés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont null prenant en charge, mais pour que l’UDT puisse reconnaître une valeur null, l’UDT doit implémenter la **System.Data.SqlTypes.INullable** interface.  
  
 Vous devez créer une propriété nommée **IsNull**, qui est nécessaire pour déterminer si une valeur est null dans le code CLR. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trouve une instance Null d'un type défini par l'utilisateur, celui-ci est rendu persistant à l'aide de méthodes de gestion de valeur Null ordinaires. Le serveur ne perd pas de temps à sérialiser ou désérialiser le type défini par l'utilisateur si cela n'est pas nécessaire et il ne gaspille pas d'espace pour stocker un type défini par l'utilisateur Null. Ce contrôle des valeurs Null est effectué chaque fois qu'un type défini par l'utilisateur est rapporté du CLR, ce qui signifie que l'utilisation de la construction [!INCLUDE[tsql](../../includes/tsql-md.md)] IS NULL pour vérifier le caractère Null des types définis par l'utilisateur doit toujours fonctionner. Le **IsNull** propriété est également utilisée par le serveur pour tester si une instance est null. Une fois que le serveur a détermine que le type défini par l'utilisateur est Null, il peut utiliser sa gestion Null native.  
  
 Le **get()** méthode **IsNull** n’est pas de cas spéciaux en aucune façon. Si un **Point** variable **@p** est **Null**, puis **@p.IsNull** , par défaut, prendra la valeur « NULL », pas « 1 ». C’est parce que le **SqlMethod(OnNullCall)** attribut de la **IsNull get()** méthode false par défaut. Étant donné que l’objet est **Null**, lorsque la propriété est requise, l’objet n’est pas désérialisé, la méthode n’est pas appelée et une valeur par défaut de « NULL » est retournée.  
  
### <a name="example"></a>Exemple  
 Dans l'exemple suivant, la variable `is_Null` est privée et contient l'état de Null pour l'instance du type défini par l'utilisateur. Votre code doit maintenir une valeur appropriée pour `is_Null`. L’UDT doit également avoir une propriété statique nommée **Null** qui retourne une instance de valeur null de l’UDT. Cela permet au type défini par l'utilisateur de renvoyer une valeur Null si l'instance est en effet Null dans la base de données.  
  
```vb  
Private is_Null As Boolean  
  
Public ReadOnly Property IsNull() As Boolean _  
   Implements INullable.IsNull  
    Get  
        Return (is_Null)  
    End Get  
End Property  
  
Public Shared ReadOnly Property Null() As Point  
    Get  
        Dim pt As New Point  
        pt.is_Null = True  
        Return (pt)  
    End Get  
End Property  
```  
  
```csharp  
private bool is_Null;  
  
public bool IsNull  
{  
    get  
    {  
        return (is_Null);  
    }  
}  
  
public static Point Null  
{  
    get  
    {  
        Point pt = new Point();  
        pt.is_Null = true;  
        return pt;  
    }  
}  
```  
  
### <a name="is-null-vs-isnull"></a>Comparaison de IS NULL et de IsNull  
 Supposons une table qui contient le schéma Points (id int, Point d’emplacement), où **Point** est un UDT CLR et les requêtes suivantes :  
  
```  
--Query 1  
SELECT ID  
FROM Points  
WHERE NOT (location IS NULL) -- Or, WHERE location IS NOT NULL;  
```  
  
```  
--Query 2:  
SELECT ID  
FROM Points  
WHERE location.IsNull = 0;  
```  
  
 Les deux requêtes retournent les ID de points non**Null** emplacements. Dans la Requête 1, la gestion de Null normale est utilisée et là aucune désérialisation du type défini par l'utilisateur n'est requise. Requête 2, quant à lui, doit désérialiser chaque non -**Null** et appelez le CLR pour obtenir la valeur de la **IsNull** propriété. Clairement, à l’aide de **IS NULL** présentera de meilleures performances et il devrait jamais y avoir une raison de lire le **IsNull** propriété d’un UDT de [!INCLUDE[tsql](../../includes/tsql-md.md)] code.  
  
 Par conséquent, ce qui est l’utilisation de la **IsNull** propriété ? Tout d’abord, il est nécessaire pour déterminer si une valeur est **Null** code à partir de CLR. En second lieu, le serveur a besoin d’un moyen de tester si une instance est **Null**, de sorte que cette propriété est utilisée par le serveur. Après avoir déterminé qu’il est **Null**, il peut utiliser sa gestion null native pour la gérer.  
  
## <a name="implementing-the-parse-method"></a>Implémentation de la méthode Parse  
 Le **analyser** et **ToString** méthodes autorisent les conversions vers et à partir de représentations sous forme de chaîne de l’UDT. Le **analyser** méthode autorise une chaîne à convertir en un type UDT. Il doit être déclaré en tant que **statique** (ou **Shared** en Visual Basic) et accepter un paramètre de type **System.Data.SqlTypes.SqlString**.  
  
 Le code suivant implémente la **analyser** méthode pour le **Point** UDT, qui sépare les coordonnées X et Y. Le **analyser** méthode possède un seul argument de type **System.Data.SqlTypes.SqlString**et suppose que les valeurs X et Y sont fournies sous forme de chaîne délimitée par des virgules. Définissant le **Microsoft.SqlServer.Server.SqlMethodAttribute.OnNullCall** attribut **false** empêche le **analyser** méthode d’être appelée à partir d’une instance null de Point.  
  
```vb  
<SqlMethod(OnNullCall:=False)> _  
Public Shared Function Parse(ByVal s As SqlString) As Point  
    If s.IsNull Then  
        Return Null  
    End If  
  
    ' Parse input string here to separate out points.  
    Dim pt As New Point()  
    Dim xy() As String = s.Value.Split(",".ToCharArray())  
    pt.X = Int32.Parse(xy(0))  
    pt.Y = Int32.Parse(xy(1))  
    Return pt  
End Function  
```  
  
```csharp  
[SqlMethod(OnNullCall = false)]  
public static Point Parse(SqlString s)  
{  
    if (s.IsNull)  
        return Null;  
  
    // Parse input string to separate out points.  
    Point pt = new Point();  
    string[] xy = s.Value.Split(",".ToCharArray());  
    pt.X = Int32.Parse(xy[0]);  
    pt.Y = Int32.Parse(xy[1]);  
    return pt;  
}  
```  
  
## <a name="implementing-the-tostring-method"></a>Implémentation de la méthode ToString  
 Le **ToString** méthode convertit le **Point** UDT en valeur de chaîne. Dans ce cas, la chaîne « NULL » est retournée pour une instance Null de la **Point** type. Le **ToString** inverse de la méthode la **analyser** méthode à l’aide un **System.Text.StringBuilder** pour renvoyer un CSV **System.String** comprenant les valeurs des coordonnées X et Y. Étant donné que **InvokeIfReceiverIsNull** valeur par défaut est false, la vérification pour une instance null de **Point** n’est pas nécessaire.  
  
```vb  
Private _x As Int32  
Private _y As Int32  
  
Public Overrides Function ToString() As String  
    If Me.IsNull Then  
        Return "NULL"  
    Else  
        Dim builder As StringBuilder = New StringBuilder  
        builder.Append(_x)  
        builder.Append(",")  
        builder.Append(_y)  
        Return builder.ToString  
    End If  
End Function  
```  
  
```csharp  
private Int32 _x;  
private Int32 _y;  
  
public override string ToString()  
{  
    if (this.IsNull)  
        return "NULL";  
    else  
    {  
        StringBuilder builder = new StringBuilder();  
        builder.Append(_x);  
        builder.Append(",");  
        builder.Append(_y);  
        return builder.ToString();  
    }  
}  
```  
  
## <a name="exposing-udt-properties"></a>Exposition de propriétés de type défini par l'utilisateur  
 Le **Point** UDT expose des coordonnées X et Y qui sont implémentées en tant que propriétés en lecture-écriture publiques de type **System.Int32**.  
  
```vb  
Public Property X() As Int32  
    Get  
        Return (Me._x)  
    End Get  
  
    Set(ByVal Value As Int32)  
        _x = Value  
    End Set  
End Property  
  
Public Property Y() As Int32  
    Get  
        Return (Me._y)  
    End Get  
  
    Set(ByVal Value As Int32)  
        _y = Value  
    End Set  
End Property  
```  
  
```csharp  
public Int32 X  
{  
    get  
    {  
        return this._x;  
    }  
    set   
    {  
        _x = value;  
    }  
}  
  
public Int32 Y  
{  
    get  
    {  
        return this._y;  
    }  
    set  
    {  
        _y = value;  
    }  
}  
```  
  
## <a name="validating-udt-values"></a>Validation de valeurs de type défini par l'utilisateur  
 Lors de l'utilisation de données de type défini par l'utilisateur, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] convertit automatiquement les valeurs binaires en valeurs de type défini par l'utilisateur. Ce processus de conversion nécessite de vérifier que les valeurs sont adaptées au format de sérialisation du type et de s'assurer que la valeur peut être désérialisée correctement. Cela garantit que la valeur peut être convertie au format binaire. Dans le cas des types définis par l'utilisateur ordonnés par octet, cela permet de s'assurer également que la valeur binaire résultante correspond à la valeur binaire d'origine. Cela empêche des valeurs non valides d'être rendues persistantes dans la base de données. Dans certains cas, ce niveau de contrôle peut être inadéquat. Une validation supplémentaire peut être requise lorsque les valeurs de type défini par l'utilisateur doivent se trouver dans un domaine ou une plage attendu(e). Par exemple, un type défini par l'utilisateur qui implémente une date peut exiger que la valeur de jour soit un nombre positif compris dans une certaine plage de valeurs valides.  
  
 Le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute.ValidationMethodName** propriété de la **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** vous permet de fournir le nom d’une méthode de validation exécutée par le serveur lorsque des données sont affectées à un type UDT ou converties en un type UDT. **ValidationMethodName** est également appelé pendant l’exécution de l’utilitaire bcp, BULK INSERT, DBCC CHECKDB, DBCC CHECKFILEGROUP, DBCC CHECKTABLE, requête distribuée et données tabulaires stream (TDS) procédure distante (RPC) fonctionnement d’appels. La valeur par défaut **ValidationMethodName** a la valeur null, indiquant qu’il n’existe aucune méthode de validation.  
  
### <a name="example"></a>Exemple  
 Le fragment de code suivant illustre la déclaration de la **Point** (classe), qui spécifie un **ValidationMethodName** de **ValidatePoint**.  
  
```vb  
<Serializable(), SqlUserDefinedTypeAttribute(Format.Native, _  
  IsByteOrdered:=True, _  
  ValidationMethodName:="ValidatePoint")> _  
  Public Structure Point  
```  
  
```csharp  
[Serializable]  
[Microsoft.SqlServer.Server.SqlUserDefinedType(Format.Native,  
  IsByteOrdered=true,   
  ValidationMethodName = "ValidatePoint")]  
public struct Point : INullable  
{  
```  
  
 Si une méthode de validation est spécifiée, elle doit avoir une signature qui ressemble au fragment de code suivant.  
  
```vb  
Private Function ValidationFunction() As Boolean  
    If (validation logic here) Then  
        Return True  
    Else  
        Return False  
    End If  
End Function  
```  
  
```csharp  
private bool ValidationFunction()  
{  
    if (validation logic here)  
    {  
        return true;  
    }  
    else  
    {  
        return false;  
    }  
}  
```  
  
 La méthode de validation peut avoir une portée quelconque et doit retourner **true** si la valeur est valide, et **false** dans le cas contraire. Si la méthode retourne **false** ou lève une exception, la valeur est considérée comme non valide et qu’une erreur se produit.  
  
 Dans l'exemple ci-dessous, le code autorise uniquement des valeurs de zéro ou plus pour les coordonnées X et Y.  
  
```vb  
Private Function ValidatePoint() As Boolean  
    If (_x >= 0) And (_y >= 0) Then  
        Return True  
    Else  
        Return False  
    End If  
End Function  
```  
  
```csharp  
private bool ValidatePoint()  
{  
    if ((_x >= 0) && (_y >= 0))  
    {  
        return true;  
    }  
    else  
    {  
        return false;  
    }  
}  
```  
  
### <a name="validation-method-limitations"></a>Limitations de méthode de validation  
 Le serveur appelle la méthode de validation lorsqu'il effectue des conversions, et non lorsque des données sont insérées en définissant des propriétés individuelles ou à l'aide d'une instruction INSERT [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Vous devez appeler explicitement la méthode de validation à partir d’accesseurs Set de propriété et la **analyser** méthode si vous souhaitez que la méthode de validation à exécuter dans toutes les situations. Cela n'est pas obligatoire, et dans certains cas peut ne pas être souhaitable.  
  
### <a name="parse-validation-example"></a>Exemple de validation Parse  
 Pour vous assurer que le **ValidatePoint** méthode est appelée dans le **Point** (classe), vous devez l’appeler à partir de la **analyser** (méthode) et à partir de la propriété des valeurs de coordonnées procédures qui définissent des X et Y. Le fragment de code suivant montre comment appeler le **ValidatePoint** méthode de validation à partir de la **analyser** (fonction).  
  
```vb  
<SqlMethod(OnNullCall:=False)> _  
Public Shared Function Parse(ByVal s As SqlString) As Point  
    If s.IsNull Then  
        Return Null  
    End If  
  
    ' Parse input string here to separate out points.  
    Dim pt As New Point()  
    Dim xy() As String = s.Value.Split(",".ToCharArray())  
    pt.X = Int32.Parse(xy(0))  
    pt.Y = Int32.Parse(xy(1))  
  
    ' Call ValidatePoint to enforce validation  
    ' for string conversions.  
    If Not pt.ValidatePoint() Then  
        Throw New ArgumentException("Invalid XY coordinate values.")  
    End If  
    Return pt  
End Function  
```  
  
```csharp  
[SqlMethod(OnNullCall = false)]  
public static Point Parse(SqlString s)  
{  
    if (s.IsNull)  
        return Null;  
  
    // Parse input string to separate out points.  
    Point pt = new Point();  
    string[] xy = s.Value.Split(",".ToCharArray());  
    pt.X = Int32.Parse(xy[0]);  
    pt.Y = Int32.Parse(xy[1]);  
  
    // Call ValidatePoint to enforce validation  
    // for string conversions.  
    if (!pt.ValidatePoint())   
        throw new ArgumentException("Invalid XY coordinate values.");  
    return pt;  
}  
```  
  
### <a name="property-validation-example"></a>Exemple de validation de propriété  
 Le fragment de code suivant montre comment appeler le **ValidatePoint** méthode de validation à partir de procédures de propriété définir les coordonnées X et Y.  
  
```vb  
Public Property X() As Int32  
    Get  
        Return (Me._x)  
    End Get  
  
    Set(ByVal Value As Int32)  
        Dim temp As Int32 = _x  
        _x = Value  
        If Not ValidatePoint() Then  
            _x = temp  
            Throw New ArgumentException("Invalid X coordinate value.")  
        End If  
    End Set  
End Property  
  
Public Property Y() As Int32  
    Get  
        Return (Me._y)  
    End Get  
  
    Set(ByVal Value As Int32)  
        Dim temp As Int32 = _y  
        _y = Value  
        If Not ValidatePoint() Then  
            _y = temp  
            Throw New ArgumentException("Invalid Y coordinate value.")  
        End If  
    End Set  
End Property  
```  
  
```csharp  
    public Int32 X  
{  
    get  
    {  
        return this._x;  
    }  
    // Call ValidatePoint to ensure valid range of Point values.  
    set   
    {  
        Int32 temp = _x;  
        _x = value;  
        if (!ValidatePoint())  
        {  
            _x = temp;  
            throw new ArgumentException("Invalid X coordinate value.");  
        }  
    }  
}  
  
public Int32 Y  
{  
    get  
    {  
        return this._y;  
    }  
    set  
    {  
        Int32 temp = _y;  
        _y = value;  
        if (!ValidatePoint())  
        {  
            _y = temp;  
            throw new ArgumentException("Invalid Y coordinate value.");  
        }  
    }  
}  
```  
  
## <a name="coding-udt-methods"></a>Codage de méthodes UDT  
 Lors du codage de méthodes UDT, considérez si l'algorithme utilisé pourrait changer avec le temps. Si c'est le cas, vous pourriez envisager de créer une classe séparée pour les méthodes utilisées par votre type défini par l'utilisateur. Si l'algorithme change, vous pouvez recompiler la classe avec le nouveau code et charger l'assembly dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans affecter le type défini par l'utilisateur. Dans de nombreux cas, les types définis par l'utilisateur peuvent être rechargés à l'aide de l'instruction  [!INCLUDE[tsql](../../includes/tsql-md.md)] ALTER ASSEMBLY, mais cela pourrait provoquer des problèmes avec les données existantes. Par exemple, le **devise** UDT inclus avec le **AdventureWorks** exemple de base de données utilise un **ConvertCurrency** de fonction pour convertir des valeurs monétaires, implémentée dans une classe distincte. Il est possible que les algorithmes de conversion puissent changer de manière imprévisible dans le futur, ou que de nouvelles fonctionnalités soient requises. Séparer le **ConvertCurrency** fonction à partir de la **devise** implémentation d’UDT offre davantage de flexibilité lors de la planification des modifications futures.  
  
### <a name="example"></a>Exemple  
 Le **Point** classe contient trois méthodes simples pour calculer la distance : **Distance**, **DistanceFrom** et **DistanceFromXY**. Chacune retourne un **double** calcule la distance de **Point** à zéro, la distance à partir d’un point spécifié à **Point**, et la distance spécifiée des coordonnées X et Y à **Point**. **Distance** et **DistanceFrom** chaque appel **DistanceFromXY**et montrent comment utiliser différents arguments pour chaque méthode.  
  
```vb  
' Distance from 0 to Point.  
<SqlMethod(OnNullCall:=False)> _  
Public Function Distance() As Double  
    Return DistanceFromXY(0, 0)  
End Function  
  
' Distance from Point to the specified point.  
<SqlMethod(OnNullCall:=False)> _  
Public Function DistanceFrom(ByVal pFrom As Point) As Double  
    Return DistanceFromXY(pFrom.X, pFrom.Y)  
End Function  
  
' Distance from Point to the specified x and y values.  
<SqlMethod(OnNullCall:=False)> _  
Public Function DistanceFromXY(ByVal ix As Int32, ByVal iy As Int32) _  
    As Double  
    Return Math.Sqrt(Math.Pow(ix - _x, 2.0) + Math.Pow(iy - _y, 2.0))  
End Function  
```  
  
```csharp  
// Distance from 0 to Point.  
[SqlMethod(OnNullCall = false)]  
public Double Distance()  
{  
    return DistanceFromXY(0, 0);  
}  
  
// Distance from Point to the specified point.  
[SqlMethod(OnNullCall = false)]  
public Double DistanceFrom(Point pFrom)  
{  
    return DistanceFromXY(pFrom.X, pFrom.Y);  
}  
  
// Distance from Point to the specified x and y values.  
[SqlMethod(OnNullCall = false)]  
public Double DistanceFromXY(Int32 iX, Int32 iY)  
{  
    return Math.Sqrt(Math.Pow(iX - _x, 2.0) + Math.Pow(iY - _y, 2.0));  
}  
```  
  
### <a name="using-sqlmethod-attributes"></a>Utilisation d'attributs SqlMethod  
 Le **Microsoft.SqlServer.Server.SqlMethodAttribute** classe fournit des attributs personnalisés qui peuvent être utilisés pour marquer des définitions de méthode afin de spécifier le déterminisme, sur le comportement d’appel null et pour spécifier si une méthode est un mutateur. Les valeurs par défaut de ces propriétés sont assumées et l'attribut personnalisé est utilisé uniquement lorsqu'une valeur non définie par défaut est exigée.  
  
> [!NOTE]  
>  Le **SqlMethodAttribute** classe hérite de la **SqlFunctionAttribute** classe, **SqlMethodAttribute** hérite le **FillRowMethodName** et **TableDefinition** des champs **SqlFunctionAttribute**. Cela implique qu'il est possible d'écrire une méthode table, ce qui n'est pas le cas. La méthode compile et déploie l’assembly, mais une erreur sur le **IEnumerable** retourner le type est déclenché lors de l’exécution avec le message suivant : « méthode, propriété ou champ '\<nom >' dans la classe\<classe >' dans l’assembly '\<assembly >' a un type de retour non valide. »  
  
 Le tableau suivant décrit quelques-unes de la **Microsoft.SqlServer.Server.SqlMethodAttribute** propriétés qui peuvent être utilisées dans les méthodes UDT et répertorie leurs valeurs par défaut.  
  
 DataAccess  
 Indique si la fonction implique l'accès aux données utilisateur stockées dans l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Valeur par défaut est **DataAccessKind**. **Aucun**.  
  
 IsDeterministic  
 Indique si la fonction produit les mêmes valeurs de sortie étant donné les mêmes valeurs d'entrée et le même état de la base de données. Valeur par défaut est **false**.  
  
 IsMutator  
 Indique si la méthode provoque une modification d'état dans l'instance de type défini par l'utilisateur. Valeur par défaut est **false**.  
  
 IsPrecise  
 Indique si la fonction implique des calculs imprécis, tels que des opérations à virgule flottante. Valeur par défaut est **false**.  
  
 OnNullCall  
 Indique si la méthode est appelée lorsque des arguments d'entrée de référence nulle sont spécifiés. Valeur par défaut est **true**.  
  
### <a name="example"></a>Exemple  
 Le **Microsoft.SqlServer.Server.SqlMethodAttribute.IsMutator** propriété vous permet de marquer une méthode qui autorise une modification de l’état d’une instance d’un type UDT. [!INCLUDE[tsql](../../includes/tsql-md.md)] ne vous permet pas de définir deux propriétés de type défini par l'utilisateur dans la clause SET d'une instruction UPDATE. Toutefois, vous pouvez avoir une méthode marquée comme mutateur qui modifie les deux membres.  
  
> [!NOTE]  
>  Les méthodes mutateurs ne sont pas autorisés dans les requêtes. Elles peuvent être appelées uniquement dans les instructions d'assignation ou les instructions de modification de données. Si une méthode marquée comme mutateur ne retourne pas **void** (ou n’est pas un **Sub** en Visual Basic), CREATE TYPE échoue avec une erreur.  
  
 L’instruction suivante suppose l’existence d’un **Triangles** UDT qui a un **pivoter** (méthode). Les éléments suivants [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction update appelle le **pivoter** méthode :  
  
```  
UPDATE Triangles SET t.RotateY(0.6) WHERE id=5  
```  
  
 Le **pivoter** est décorée avec le **SqlMethod** paramètre d’attribut **IsMutator** à **true** afin que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse marquer la méthode comme méthode de mutateur. Le code définit également **OnNullCall** à **false**, ce qui indique au serveur que la méthode retourne une référence null (**rien** en Visual Basic) si un des paramètres d’entrée sont des références null.  
  
```vb  
<SqlMethod(IsMutator:=True, OnNullCall:=False)> _  
Public Sub Rotate(ByVal anglex as Double, _  
  ByVal angley as Double, ByVal anglez As Double)   
   RotateX(anglex)  
   RotateY(angley)  
   RotateZ(anglez)  
End Sub  
```  
  
```csharp  
[SqlMethod(IsMutator = true, OnNullCall = false)]  
public void Rotate(double anglex, double angley, double anglez)   
{  
   RotateX(anglex);  
   RotateY(angley);  
   RotateZ(anglez);  
}  
```  
  
## <a name="implementing-a-udt-with-a-user-defined-format"></a>Implémentation d'un type défini par l'utilisateur avec un format défini par l'utilisateur  
 Lors de l’implémentation d’un UDT avec un format défini par l’utilisateur, vous devez implémenter **en lecture** et **écrire** méthodes qui implémentent l’interface Microsoft.SqlServer.Server.IBinarySerialize pour gérer la sérialisation et la désérialisation de données UDT. Vous devez également spécifier le **MaxByteSize** propriété de la **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute**.  
  
### <a name="the-currency-udt"></a>Le type défini par l'utilisateur Currency  
 Le **devise** UDT est inclus dans les exemples de CLR qui peuvent être installés avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en commençant par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 Le **devise** UDT prend en charge la gestion des sommes d’argent dans le système monétaire d’une culture particulière. Vous devez définir deux champs : un **chaîne** pour **CultureInfo**, ce qui indique qui a émis la devise (en-us, par exemple) et un **décimal** pour **CurrencyValue**, la somme d’argent.  
  
 Bien qu’il n’est pas utilisé par le serveur pour effectuer des comparaisons, les **devise** UDT implémente la **System.IComparable** interface, qui expose une méthode unique, **System.IComparable.CompareTo**. Celle-ci est utilisée du côté client dans les situations où il est souhaitable de comparer ou d'ordonner précisément des valeurs monétaire dans des cultures.  
  
 Le code qui s'exécute dans le CLR compare la culture séparément de la valeur monétaire. Pour le code [!INCLUDE[tsql](../../includes/tsql-md.md)], les actions suivantes déterminent la comparaison :  
  
1.  Définir le **IsByteOrdered** la valeur true, ce qui indique à l’attribut [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser la représentation binaire persistante sur disque pour les comparaisons.  
  
2.  Utilisez le **écrire** méthode pour le **devise** UDT pour déterminer comment l’UDT est conservé sur le disque et par conséquent, comment les valeurs UDT sont comparées et ordonnées pour [!INCLUDE[tsql](../../includes/tsql-md.md)] operations.  
  
3.  Enregistrer le **devise** UDT à l’aide du format binaire suivant :  
  
    1.  Enregistrez la culture en tant que chaîne encodée UTF-16 pour les octets 0-19, avec un remplissage à droite avec des caractères Null.  
  
    2.  Utilisez les octets 20 et plus pour contenir la valeur décimale de la monnaie.  
  
 L'objectif du remplissage est de s'assurer que la culture est complètement séparée de la valeur monétaire, de sorte que lorsque deux types définis par l'utilisateur sont comparés dans le code [!INCLUDE[tsql](../../includes/tsql-md.md)], les octets de culture soient comparés à des octets de culture et les valeurs d'octets de monnaie soient comparées à des valeurs d'octets de monnaie.  
  
 Pour le code complet pour le **devise** UDT, suivez les instructions d’installation du CLR des exemples dans [exemples pour le moteur de base de données SQL Server](http://msftengprodsamples.codeplex.com/).  
  
### <a name="currency-attributes"></a>Attributs Currency  
 Le **devise** UDT est défini avec les attributs suivants.  
  
```vb  
<Serializable(), Microsoft.SqlServer.Server.SqlUserDefinedType( _  
    Microsoft.SqlServer.Server.Format.UserDefined, _  
    IsByteOrdered:=True, MaxByteSize:=32), _  
    CLSCompliant(False)> _  
Public Structure Currency  
Implements INullable, IComparable, _  
Microsoft.SqlServer.Server.IBinarySerialize  
```  
  
```csharp  
[Serializable]  
[SqlUserDefinedType(Format.UserDefined,   
    IsByteOrdered = true, MaxByteSize = 32)]  
    [CLSCompliant(false)]  
    public struct Currency : INullable, IComparable, IBinarySerialize  
    {  
```  
  
## <a name="creating-read-and-write-methods-with-ibinaryserialize"></a>Création de méthodes Read et Write avec IBinarySerialize  
 Lorsque vous choisissez **UserDefined** format de sérialisation, vous devez également implémenter la **IBinarySerialize** de l’interface et créer vos propres **en lecture** et **écrire** méthodes. Les procédures suivantes à partir de la **devise** utilisation d’UDT le **System.IO.BinaryReader** et **System.IO.BinaryWriter** pour lire et écrire dans l’UDT.  
  
```vb  
' IBinarySerialize methods  
' The binary layout is as follow:  
'    Bytes 0 - 19: Culture name, padded to the right with null  
'    characters, UTF-16 encoded  
'    Bytes 20+: Decimal value of money  
' If the culture name is empty, the currency is null.  
Public Sub Write(ByVal w As System.IO.BinaryWriter) _  
  Implements Microsoft.SqlServer.Server.IBinarySerialize.Write  
    If Me.IsNull Then  
        w.Write(nullMarker)  
        w.Write(System.Convert.ToDecimal(0))  
        Return  
    End If  
  
    If cultureName.Length > cultureNameMaxSize Then  
        Throw New ApplicationException(String.Format(CultureInfo.CurrentUICulture, _  
           "{0} is an invalid culture name for currency as it is too long.", cultureNameMaxSize))  
    End If  
  
    Dim paddedName As String = cultureName.PadRight(cultureNameMaxSize, CChar(vbNullChar))  
  
    For i As Integer = 0 To cultureNameMaxSize - 1  
        w.Write(paddedName(i))  
    Next i  
  
    ' Normalize decimal value to two places  
    currencyVal = Decimal.Floor(currencyVal * 100) / 100  
    w.Write(currencyVal)  
End Sub  
  
Public Sub Read(ByVal r As System.IO.BinaryReader) _  
  Implements Microsoft.SqlServer.Server.IBinarySerialize.Read  
    Dim name As Char() = r.ReadChars(cultureNameMaxSize)  
    Dim stringEnd As Integer = Array.IndexOf(name, CChar(vbNullChar))  
  
    If stringEnd = 0 Then  
        cultureName = Nothing  
        Return  
    End If  
  
    cultureName = New String(name, 0, stringEnd)  
    currencyVal = r.ReadDecimal()  
End Sub  
  
```  
  
```csharp  
// IBinarySerialize methods  
// The binary layout is as follow:  
//    Bytes 0 - 19:Culture name, padded to the right   
//    with null characters, UTF-16 encoded  
//    Bytes 20+:Decimal value of money  
// If the culture name is empty, the currency is null.  
public void Write(System.IO.BinaryWriter w)  
{  
    if (this.IsNull)  
    {  
        w.Write(nullMarker);  
        w.Write((decimal)0);  
        return;  
    }  
  
    if (cultureName.Length > cultureNameMaxSize)  
    {  
        throw new ApplicationException(string.Format(  
            CultureInfo.InvariantCulture,   
            "{0} is an invalid culture name for currency as it is too long.",   
            cultureNameMaxSize));  
    }  
  
    String paddedName = cultureName.PadRight(cultureNameMaxSize, '\0');  
    for (int i = 0; i < cultureNameMaxSize; i++)  
    {  
        w.Write(paddedName[i]);  
    }  
  
    // Normalize decimal value to two places  
    currencyValue = Decimal.Floor(currencyValue * 100) / 100;  
    w.Write(currencyValue);  
}  
public void Read(System.IO.BinaryReader r)  
{  
    char[] name = r.ReadChars(cultureNameMaxSize);  
    int stringEnd = Array.IndexOf(name, '\0');  
  
    if (stringEnd == 0)  
    {  
        cultureName = null;  
        return;  
    }  
  
    cultureName = new String(name, 0, stringEnd);  
    currencyValue = r.ReadDecimal();  
}  
```  
  
 Pour le code complet pour le **devise** UDT, consultez [exemples pour le moteur de base de données SQL Server](http://msftengprodsamples.codeplex.com/).  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’un Type défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md)  
  
  
