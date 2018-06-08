---
title: Expressions de requête et noms URN | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: scripting
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- query expressions
- unique resource names
- URN
ms.assetid: e0d30dbe-7daf-47eb-8412-1b96792b6fb9
caps.latest.revision: 14
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: cf241911ce9befc7b42e8c84fd6fddb7dcd10374
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="query-expressions-and-uniform-resource-names"></a>Expressions de requête et noms URN
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Les modèles SMO ( [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Objects) et les composants logiciels enfichables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell utilisent deux types de chaînes d’expression semblables aux expressions XPath. Les expressions de requête sont des chaînes qui spécifient un jeu de critères permettant d'énumérer un ou plusieurs objets dans une hiérarchie de modèle objet. Un nom de ressource unique (URN) est un type spécifique de chaîne d'expression de requête qui identifie de façon unique un objet particulier.  

> [!NOTE]
> Il existe deux modules SQL Server PowerShell : **SqlServer** et **SQLPS**. Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour. Le module PowerShell le plus récent est **SqlServer**. Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS**, ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.  
> Des versions précédentes du module **SqlServer** *étaient* fournies avec SQL Server Management Studio (SSMS), mais uniquement avec les versions 16.x de SSMS. Pour utiliser PowerShell avec SSMS 17.0 et ultérieur, vous devez installer le module **SqlServer** à partir de PowerShell Gallery.
> Pour installer le module **SqlServer**, consultez [Installer SQL Server PowerShell](download-sql-server-ps-module.md).

  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Object1[<FilterExpression1>]/ ... /ObjectN[<FilterExpressionN>]  
  
<FilterExpression>::=  
<PropertyExpression> [and <PropertyExpression>][...n]  
  
<PropertyExpression>::=  
      @BooleanPropertyName=true()  
 | @BooleanPropertyName=false()  
 | contains(@StringPropertyName, 'PatternString')  
  | @StringPropertyName='String'  
 | @DatePropertyName=datetime('DateString')  
 | is_null(@PropertyName)  
 | not(<PropertyExpression>)  
  
```  
  
## <a name="arguments"></a>Arguments  
 *Objet*  
 Spécifie le type d'objet qui est représenté au niveau de ce nœud par la chaîne d'expression. Chaque objet représente une classe de collection à partir de ces espaces de noms du modèle objet SMO :  
  
 <xref:Microsoft.SqlServer.Management.Smo>  
  
 <xref:Microsoft.SqlServer.Management.Smo.Agent>  
  
 <xref:Microsoft.SqlServer.Management.Smo.Broker>  
  
 <xref:Microsoft.SqlServer.Management.Smo.Mail>  
  
 <xref:Microsoft.SqlServer.Management.Dmf>  
  
 <xref:Microsoft.SqlServer.Management.Facets>  
  
 <xref:Microsoft.SqlServer.Management.RegisteredServers>  
  
 <xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>  
  
 Par exemple, spécifiez Server pour la classe **ServerCollection** et Database pour la classe **DatabaseCollection** .  
  
 @*PropertyName*  
 Spécifie le nom de l’une des propriétés de la classe associée à l’objet spécifié dans *Object*. Le nom de la propriété doit avoir pour préfixe le caractère @. Par exemple, spécifiez @IsAnsiNull pour la propriété de classe **Database** **IsAnsiNull**.  
  
 @*BooleanPropertyName*=true()  
 Énumère tous les objets où la propriété booléenne spécifiée a la valeur TRUE.  
  
 @*BooleanPropertyName*=false()  
 Énumère tous les objets où la propriété booléenne spécifiée a la valeur FALSE.  
  
 contains(@*StringPropertyName*, '*PatternString*')  
 Énumère tous les objets où la propriété de chaîne spécifiée contient au moins une occurrence du jeu de caractères spécifié dans '*PatternString*'.  
  
 @*StringPropertyName*='*PatternString*'  
 Énumère tous les objets où la valeur de la propriété de chaîne spécifiée est identique au modèle de caractère spécifié dans '*PatternString*'.  
  
 @*DatePropertyName*= datetime('*DateString*')  
 Énumère tous les objets où la valeur de la propriété de date spécifiée correspond à la date spécifiée dans '*DateString*'. *DateString* doit être au format aaaa-mm-jj hh:mi:ss.mmm.  
  
|||  
|-|-|  
|aaaa|Année à quatre chiffres.|  
|MM|Mois à deux chiffres (01 à 12).|  
|jj|Date à deux chiffres (01 à 31).|  
|hh|Heure à deux chiffres au format 24 heures (01 à 23).|  
|mi|Minutes à deux chiffres (01 à 59).|  
|ss|Secondes à deux chiffres (01 à 59).|  
|mmm|Nombre de millisecondes (001 à 999).|  
  
 Les dates spécifiées dans ce format peuvent être évaluées par rapport à tout format de date stocké dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 is_null(@*PropertyName*)  
 Énumère tous les objets où la propriété spécifiée a la valeur NULL.  
  
 not(\<*PropertyExpression*>)  
 Inverse la valeur d’évaluation de *PropertyExpression*, énumérant tous les objets qui ne correspondent pas à la condition spécifiée dans *PropertyExpression*. Par exemple, not(contains(@Name, ’xyz’)) énumère tous les objets dont le nom ne contient pas la chaîne xyz.  
  
## <a name="remarks"></a>Notes   
 Les expressions de requête sont des chaînes qui énumèrent les nœuds dans une hiérarchie de modèle SMO. Chaque nœud possède une expression de filtre qui spécifie les critères pour déterminer les objets qui sont énumérés au niveau de ce nœud. Les expressions de requête sont modélisées sur le langage d'expression XPath. Les expressions de requête implémentent un petit sous-ensemble des expressions qui sont prises en charge par XPath, et possèdent également quelques extensions qui ne sont pas présentes dans XPath. Les expressions XPath sont des chaînes qui spécifient un jeu de critères utilisé pour énumérer une ou plusieurs balises dans un document XML. Pour plus d'informations sur XPath, consultez [W3C XPath Language](http://www.w3.org/TR/xpath20/)(en anglais).  
  
 Les expressions de requête doivent commencer par une référence absolue à l'objet serveur. Les expressions relatives avec une barre oblique (/) de début ne sont pas autorisées. La séquence des objets spécifiés dans une expression de requête doit respecter la hiérarchie des objets de collection dans le modèle objet associé. Par exemple, une expression de requête qui fait référence à des objets dans l'espace de noms Microsoft.SqlServer.Management.Smo doit commencer par un nœud Server, suivi d'un nœud Database, etc.  
  
 Si un *\<FilterExpression>* n’est pas spécifié pour un objet, tous les objets au niveau de ce nœud sont énumérés.  
  
## <a name="uniform-resource-names-urn"></a>URN (Uniform Resource Name)  
 Les noms URN sont un sous-ensemble d'expressions de requête. Chaque nom URN forme une référence complète à un objet unique. Un nom URN type utilise la propriété Name pour identifier un objet unique au niveau de chaque nœud. Par exemple, ce nom URN fait référence à une colonne spécifique :  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012']/Table[@Name='SalesPerson' and @Schema='Sales']/Column[@Name='SalesPersonID']  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enumerating-objects-using-false"></a>A. Énumération d'objets à l'aide de false()  
 Cette expression de requête énumère toutes les bases de données dont l’attribut **AutoClose** a la valeur false dans l’instance par défaut sur **MyComputer**.  
  
```  
Server[@Name='MYCOMPUTER']/Database[@AutoClose=false()]  
```  
  
### <a name="b-enumerating-objects-using-contains"></a>B. Énumération d'objets à l'aide de contains  
 Cette expression de requête énumère toutes les bases de données qui ne respectent pas la casse et dont le nom comporte le caractère « m ».  
  
```  
Server[@Name='MYCOMPUTER']/Database[@CaseSensitive=false() and contains(@Name, 'm')]   
```  
  
### <a name="c-enumerating-objects-using-not"></a>C. Énumération d'objets à l'aide de not  
 Cette expression de requête énumère toutes les tables [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] qui ne figurent pas dans le schéma **Production** et qui contiennent le mot History dans le nom de la table :  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012']/Table[not(@Schema='Production') and contains(@Name, 'History')]  
```  
  
### <a name="d-not-supplying-a-filter-expression-for-the-final-node"></a>D. Non-spécification d'une expression de filtre pour le dernier nœud  
 Cette expression de requête énumère toutes les colonnes dans la table **AdventureWorks2012.Sales.SalesPerson** :  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[@Schema='Sales' and @Name='SalesPerson']/Columns  
```  
  
### <a name="e-enumerating-objects-using-datetime"></a>E. Énumération d'objets à l'aide de datetime  
 Cette expression de requête énumère toutes les tables créées dans la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] à une heure spécifique :  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[@CreateDate=datetime('2008-03-21 19:49:32.647')]  
```  
  
### <a name="f-enumerating-objects-using-isnull"></a>F. Énumération d'objets à l'aide de is_null  
 Cette expression de requête énumère toutes les tables dans la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] dont la propriété de date de dernière modification n'a pas la valeur NULL :  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[Not(is_null(@DateLastModified))]  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Invoke-PolicyEvaluation (applet de commande)](invoke-policyevaluation-cmdlet.md)   
 [SQL Server Audit &#40moteur de base de données&#41;](../relational-databases/security/auditing/sql-server-audit-database-engine.md)  
  
  
