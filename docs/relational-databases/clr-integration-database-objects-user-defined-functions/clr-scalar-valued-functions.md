---
title: Les fonctions scalaires CLR | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
dev_langs:
- TSQL
- VB
- CSharp
helpviewer_keywords:
- SVF
- scalar-valued functions
ms.assetid: 20dcf802-c27d-4722-9cd3-206b1e77bee0
caps.latest.revision: 81
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 739a6face41bc0f99b0f21567dee6192392e13fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-scalar-valued-functions"></a>Fonctions scalaires CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une fonction scalaire (SVF) retourne une valeur unique, telle qu'une chaîne, un entier ou une valeur binaire. Vous pouvez créer des fonctions scalaires définies par l'utilisateur en code managé à l'aide de tout langage de programmation .NET Framework. Ces fonctions sont accessibles au code [!INCLUDE[tsql](../../includes/tsql-md.md)] ou autre code managé. Pour plus d’informations sur les avantages de l’intégration du CLR et en choisissant entre le code managé et [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [vue d’ensemble de l’intégration CLR](../../relational-databases/clr-integration/clr-integration-overview.md).  
  
## <a name="requirements-for-clr-scalar-valued-functions"></a>Spécifications relatives aux fonctions scalaires CLR  
 Les fonctions scalaires .NET Framework sont implémentées en tant que méthodes d'une classe dans un assembly .NET Framework. Les paramètres d’entrée et le type retourné à partir d’une fonction scalaire peuvent être un des types de données scalaires pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à l’exception de **varchar**, **char**, **rowversion**, **texte**, **ntext**, **image**, **timestamp**, **table**, ou **curseur**. Les fonctions scalaires doivent garantir une correspondance entre le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le type de données de retour de la méthode d'implémentation. Pour plus d’informations sur les conversions de type, consultez [de mappage de données de paramètre CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).  
  
 Lors de l'implémentation d'une fonction scalaire .NET Framework dans un langage .NET Framework, l'attribut personnalisé **SqlFunction** peut être spécifié de façon à inclure des informations supplémentaires à propos de la fonction. L'attribut **SqlFunction** indique si la fonction accède ou modifie des données, si elle est déterministe et si elle implique des opérations de virgule flottante.  
  
 Les fonctions scalaires définies par l'utilisateur peuvent être déterministes ou non déterministes. Une fonction déterministe retourne toujours le même résultat lorsqu'elle est appelée avec un jeu de paramètres d'entrée spécifique. Une fonction non déterministe peut retourner des résultats différents lorsqu'elle est appelée avec un jeu de paramètres d'entrée spécifique.  
  
> [!NOTE]  
>  Ne marquez pas une fonction comme étant déterministe si elle ne produit pas toujours les mêmes valeurs de sortie à partir des mêmes valeurs d'entrée et du même état de base de données. Le marquage d'une fonction comme étant déterministe alors que cette dernière ne l'est pas vraiment peut provoquer une altération des vues indexées et des colonnes calculées. Pour marquer une fonction comme déterministe, vous devez affecter la valeur « true » à la propriété **IsDeterministic** .  
  
### <a name="table-valued-parameters"></a>Paramètres table  
 Les paramètres table (types de tables définis par l'utilisateur et passés dans une procédure ou une fonction) offrent un moyen efficace pour passer plusieurs lignes de données au serveur. Ils procurent une fonctionnalité semblable aux tableaux de paramètres, mais offrent une meilleure souplesse et une intégration plus étroite à [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ils sont également susceptibles de générer de meilleures performances. Les paramètres table aident également à réduire le nombre d'allers-retours au serveur. Au lieu d'envoyer plusieurs demandes au serveur, comme avec une liste de paramètres scalaires, les données peuvent être envoyées au serveur en tant que paramètres table. Un type de table défini par l'utilisateur ne peut pas être passé en tant que paramètre table à une fonction ou à une procédure stockée managée s'exécutant dans le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ni être retourné à partir de ces dernières. Pour plus d’informations sur les paramètres table, consultez [utiliser des paramètres table & #40 ; moteur de base de données & #41 ;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
## <a name="example-of-a-clr-scalar-valued-function"></a>Exemple de fonction scalaire CLR  
 Voici une fonction scalaire simple qui accède à des données et renvoie une valeur entière :  
  
```csharp  
using Microsoft.SqlServer.Server;  
using System.Data.SqlClient;  
  
public class T  
{  
    [SqlFunction(DataAccess = DataAccessKind.Read)]  
    public static int ReturnOrderCount()  
    {  
        using (SqlConnection conn   
            = new SqlConnection("context connection=true"))  
        {  
            conn.Open();  
            SqlCommand cmd = new SqlCommand(  
                "SELECT COUNT(*) AS 'Order Count' FROM SalesOrderHeader", conn);  
            return (int)cmd.ExecuteScalar();  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Server  
Imports System.Data.SqlClient  
  
Public Class T  
    <SqlFunction(DataAccess:=DataAccessKind.Read)> _  
    Public Shared Function ReturnOrderCount() As Integer  
        Using conn As New SqlConnection("context connection=true")  
            conn.Open()  
            Dim cmd As New SqlCommand("SELECT COUNT(*) AS 'Order Count' FROM SalesOrderHeader", conn)  
            Return CType(cmd.ExecuteScalar(), Integer)  
        End Using  
    End Function  
End Class  
```  
  
 La première ligne de code fait référence à **Microsoft.SqlServer.Server** afin d'accéder à des attributs et à **System.Data.SqlClient** afin d'accéder à l'espace de noms ADO.NET. (Cet espace de noms contient **SqlClient**, le fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].)  
  
 Ensuite, la fonction reçoit l'attribut personnalisé **SqlFunction** , qui se trouve dans l'espace de noms **Microsoft.SqlServer.Server** . L'attribut personnalisé indique si la fonction définie par l'utilisateur utilise le fournisseur in-process pour lire les données sur le serveur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas les fonctions définies par l'utilisateur à mettre à jour, insérer ou supprimer des données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut optimiser l'exécution d'une fonction définie par l'utilisateur qui n'utilise pas le fournisseur in-process. Cela est indiqué en affectant la valeur **DataAccessKind** à **DataAccessKind.None**. Sur la ligne suivante, la méthode cible est une méthode statique publique (partagée dans Visual Basic .NET).  
  
 Le **SqlContext** (classe), situé dans le **Microsoft.SqlServer.Server** espace de noms, peut alors accéder un **SqlCommand** objet avec une connexion à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance est déjà configuré. Bien qu'il ne soit pas utilisé ici, le contexte de transaction actuel est également disponible par le biais de l'API **System.Transactions** .  
  
 La plupart des lignes de code dans le corps de la fonction doivent sembler familières aux développeurs ayant écrit des applications clientes qui utilisent les types de l'espace de noms **System.Data.SqlClient** .  
  
 [C#]  
  
```  
using(SqlConnection conn = new SqlConnection("context connection=true"))   
{  
   conn.Open();  
   SqlCommand cmd = new SqlCommand(  
        "SELECT COUNT(*) AS 'Order Count' FROM SalesOrderHeader", conn);  
   return (int) cmd.ExecuteScalar();  
}    
```  
  
 [Visual Basic]  
  
```  
Using conn As New SqlConnection("context connection=true")  
   conn.Open()  
   Dim cmd As New SqlCommand( _  
        "SELECT COUNT(*) AS 'Order Count' FROM SalesOrderHeader", conn)  
   Return CType(cmd.ExecuteScalar(), Integer)  
End Using  
```  
  
 Le texte de commande approprié est spécifié en initialisant l'objet **SqlCommand** . L'exemple précédent compte le nombre de lignes dans la table **SalesOrderHeader**. Ensuite, la méthode **ExecuteScalar** de l'objet **cmd** est appelée. Elle retourne une valeur de type **int** basée sur la requête. Pour finir, le nombre de commandes (« order count ») est retourné à l'appelant.  
  
 Si ce code est enregistré dans un fichier nommé FirstUdf.cs, il peut être compilé dans un assembly comme suit :  
  
 [C#]  
  
```  
csc.exe /t:library /out:FirstUdf.dll FirstUdf.cs   
```  
  
 [Visual Basic]  
  
```  
vbc.exe /t:library /out:FirstUdf.dll FirstUdf.vb  
```  
  
> [!NOTE]  
>  `/t:library` indique qu'une bibliothèque, plutôt qu'un fichier exécutable, doit être produite. Les fichiers exécutables ne peuvent pas être inscrits dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Les objets de base de données Visual C++ compilés avec **/CLR : pure** ne sont pas pris en charge pour l’exécution sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il s'agit par exemple d'objets de base de données tels que des fonctions scalaires.  
  
 Voici la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] et un exemple d'appel pour inscrire l'assembly et la fonction définie par l'utilisateur :  
  
```  
CREATE ASSEMBLY FirstUdf FROM 'FirstUdf.dll';  
GO  
  
CREATE FUNCTION CountSalesOrderHeader() RETURNS INT   
AS EXTERNAL NAME FirstUdf.T.ReturnOrderCount;   
GO  
  
SELECT dbo.CountSalesOrderHeader();  
GO  
  
```  
  
 Notez qu'il n'est pas obligatoire que le nom de fonction tel qu'exposé dans [!INCLUDE[tsql](../../includes/tsql-md.md)] corresponde au nom de la méthode statique publique cible.  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de données de paramètre CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md)   
 [Vue d’ensemble des attributs personnalisés de l’intégration de CLR](http://msdn.microsoft.com/library/ecf5c097-0972-48e2-a9c0-b695b7dd2820)   
 [Fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/user-defined-functions.md)   
 [Accès aux données à partir d'objets de base de données CLR](../../relational-databases/clr-integration/data-access/data-access-from-clr-database-objects.md)  
  
  
