---
title: Utilisation des Types de données | Documents Microsoft
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: smo
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- DataType object
- SQL Server Management Objects, data types
- data types [SMO]
- SMO [SQL Server], data types
ms.assetid: 1e0e736a-c709-4d89-aeb2-b32dcfd641fa
caps.latest.revision: 45
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 317ff944bf477d8eff05d91d8bbf8776b2f8a1e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-data-types"></a>Utilisation des types de données
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Il existe des données de nombreux types et tailles différents, telles qu'une chaîne qui a une longueur définie, un nombre qui a une précision spécifique ou un type de données défini par l'utilisateur qui est un autre objet ayant son propre ensemble de règles. Le <xref:Microsoft.SqlServer.Management.Smo.DataType> objet classe du type de données pour qu’elle puisse être gérée correctement par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'objet <xref:Microsoft.SqlServer.Management.Smo.DataType> est associé aux objets qui acceptent des données. Les éléments suivants [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les objets Management Objects (SMO) acceptent des données qui doivent être définies par un <xref:Microsoft.SqlServer.Management.Smo.DataType> propriété d’objet :  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Column>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunctionParameter>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.StoredProcedureParameter>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunctionParameter>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedAggregateParameter>  
  
 La propriété **DataType** pour les objets qui acceptent des données peut être définie de plusieurs façons.  
  
-   Utilisez le constructeur par défaut et spécifiez <xref:Microsoft.SqlServer.Management.Smo.DataType> propriétés de l’objet explicitement  
  
-   Utilisez un constructeur surchargé et spécifiez le <xref:Microsoft.SqlServer.Management.Smo.DataType> propriétés comme paramètres.  
  
-   Spécifiez le <xref:Microsoft.SqlServer.Management.Smo.DataType> inline dans le constructeur d’objet.  
  
-   Utilisez un des membres statiques de la <xref:Microsoft.SqlServer.Management.Smo.DataType> de classe, par exemple **Int**. Cela retourne en fait une instance d'un objet <xref:Microsoft.SqlServer.Management.Smo.DataType>.  
  
 L'objet <xref:Microsoft.SqlServer.Management.Smo.DataType> a plusieurs propriétés qui définissent le type de données. Par exemple, la propriété <xref:Microsoft.SqlServer.Management.Smo.SqlDataType> spécifie le type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les valeurs constantes qui représentent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des types de données sont répertoriées dans le <xref:Microsoft.SqlServer.Management.Smo.SqlDataType> énumération. Cela fait référence à des types de données tels que **varchar**, **nchar**, **currency**, **integer**, **float**et **datetime**.  
  
 Lorsque le type de données est établi, des propriétés spécifiques doivent être définies pour les données. Par exemple, s'il s'agit d'un type **nchar** , la longueur des données de chaîne doit être définie dans la propriété **Length** . La même règle s'applique aux valeurs numériques, pour lesquelles il vous faudrait spécifier la précision et l'échelle.  
  
 Les types de données <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> et <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType> font référence à des objets qui contiennent la définition du type de données défini par l'utilisateur. Le type <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> est basé sur les types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de l'énumération <xref:Microsoft.SqlServer.Management.Smo.SqlDataType>. Le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType> est basée sur [!INCLUDE[msCoName](../../../includes/msconame-md.md)] les types de données .NET. En général, ces types représentent des données d'un type spécifique qui est fréquemment réutilisé par la base de données à cause de règles d'entreprise définies par l'organisation. Par exemple, un type de données qui stocke une somme d'argent et un dénominateur de devise serait utile dans une société qui négocie dans plusieurs devises.  
  
 Le <xref:Microsoft.SqlServer.Management.Smo.SqlDataType> énumération contient une liste de tous les [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-prise en charge des types de données.  
  
## <a name="examples"></a>Exemples  
Pour utiliser un exemple de code fourni, vous devrez sélectionner l'environnement, le modèle et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
  
## <a name="constructing-a-datatype-object-with-the-specification-in-the-constructor-in-visual-basic"></a>Construction d'un objet DataType avec la spécification dans le constructeur en Visual Basic  
 Cet exemple de code montre comment utiliser le constructeur pour créer des instances de types de données basées sur différents types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Les types <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>, <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> et XML requièrent tous une valeur de nom afin d'identifier l'objet.  
  
```VBNET
'Declare a DataType object variable and define the data type in the constructor.
Dim dt As DataType
'For the decimal data type the following two arguements specify precision, and scale.
dt = New DataType(SqlDataType.Decimal, 10, 2)
``` 
  
## <a name="constructing-a-datatype-object-with-the-specification-in-the-constructor-in-visual-c"></a>Construction d'un objet DataType avec la spécification dans le constructeur en Visual C#  
 Cet exemple de code montre comment utiliser le constructeur pour créer des instances de types de données basées sur différents types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Les types <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>, <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> et XML requièrent tous une valeur de nom afin d'identifier l'objet.  
  
```csharp  
{   
//Declare a DataType object variable and define the data type in the constructor.   
DataType dt;   
//For the decimal data type the following two arguements specify precision, and scale.   
dt = new DataType(SqlDataType.Decimal, 10, 2);   
}  
```  
  
## <a name="constructing-a-datatype-object-by-using-the-default-constructor-in-visual-basic"></a>Construction d'un objet DataType à l'aide du constructeur par défaut en Visual Basic  
 Cet exemple de code montre comment utiliser le constructeur par défaut pour créer des instances de types de données basées sur différents types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Les propriétés sont ensuite utilisées pour spécifier le type de données.  
  
 **Remarque** le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>, <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType>, et tous les types XML requièrent une valeur de nom pour identifier l’objet.  
  
```VBNET
'Declare and create a DataType object variable.
Dim dt As DataType
dt = New DataType
'Define the data type by setting the SqlDataType property.
dt.SqlDataType = SqlDataType.VarChar
'The VarChar data type requires a value for the MaximumLength property.
dt.MaximumLength = 100
```
  
## <a name="constructing-a-datatype-object-by-using-the-default-constructor-in-visual-c"></a>Construction d'un objet DataType à l'aide du constructeur par défaut en Visual C#  
 Cet exemple de code montre comment utiliser le constructeur par défaut pour créer des instances de types de données basées sur différents types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Les propriétés sont ensuite utilisées pour spécifier le type de données.  
  
 **Remarque** le <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>, <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType>, et tous les types XML requièrent une valeur de nom pour identifier l’objet.  
  
```csharp  
{   
//Declare and create a DataType object variable.   
DataType dt;   
dt = new DataType();   
//Define the data type by setting the SqlDataType property.   
dt.SqlDataType = SqlDataType.VarChar;   
//The VarChar data type requires a value for the MaximumLength property.   
dt.MaximumLength = 100;   
}  
```  
  
  
