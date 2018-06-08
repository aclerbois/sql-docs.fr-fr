---
title: Fonction SUM (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- sum function [XQuery]
- fn:sum function
ms.assetid: 12288f37-b54c-4237-b75e-eedc5fe8f96d
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 071394e1c2889c94c65a8e42179fd1bdbf5cf383
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="aggregate-functions---sum"></a>Fonctions d’agrégation - sum
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne la somme d'une série de nombres.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:sum($arg as xdt:anyAtomicType*) as xdt:anyAtomicType  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Série de valeurs atomiques dont la somme est à calculer.  
  
## <a name="remarks"></a>Notes  
 Tous les types de valeurs atomisées transmises à **sum()** doivent être des sous-types du même type de base. Les types de base acceptés sont les trois types de base numériques intégrés ou xdt:untypedAtomic. Les valeurs de type xdt:untypedAtomic sont converties en xs:double. S’il existe un mélange de ces types, ou si d’autres valeurs d’autres types sont passés, une erreur statique est déclenchée.  
  
 Le résultat de **sum()** reçoit le type de base des types transmis tel que xs : double dans le cas de xdt : untypedAtomic, même si l’entrée est éventuellement la séquence vide. Si l'entrée est vide statiquement, le résultat est 0 avec le type statique et dynamique de xs:integer.  
  
 Le **sum()** fonction renvoie la somme des valeurs numériques. Si une valeur xdt : untypedAtomic ne peut pas être convertie en xs : double, la valeur est ignorée dans la séquence d’entrée, *$arg*. Si l'entrée est une séquence vide calculée dynamiquement, la valeur 0 du type de base utilisé est retournée.  
  
 La fonction retourne une erreur d'exécution en cas d'exception de dépassement de capacité ou de valeur hors limite.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** type des colonnes dans le [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
### <a name="a-using-the-sum-xquery-function-to-find-the-total-combined-number-of-labor-hours-for-all-work-center-locations-in-the-manufacturing-process"></a>A. Utilisation de la fonction XQuery sum() pour rechercher le nombre total d'heures de travail pour tous les ateliers inclus dans le processus de fabrication  
 La requête ci-dessous permet de trouver le nombre total d'heures de travail pour tous les ateliers inclus dans le processus de fabrication de tous les modèles de produits pour lesquels des instructions de fabrication sont stockées.  
  
```  
SELECT Instructions.query('         
   declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
  <ProductModel PMID= "{ sql:column("Production.ProductModel.ProductModelID") }"         
  ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >         
   <TotalLaborHrs>         
     { sum(//AWMI:Location/@LaborHours) }         
   </TotalLaborHrs>         
 </ProductModel>         
    ') as Result         
FROM Production.ProductModel         
WHERE Instructions is not NULL         
```  
  
 Le résultat partiel est le suivant.  
  
```  
<ProductModel PMID="7" ProductModelName="HL Touring Frame">  
   <TotalLaborHrs>12.75</TotalLaborHrs>  
</ProductModel>  
<ProductModel PMID="10" ProductModelName="LL Touring Frame">  
  <TotalLaborHrs>13</TotalLaborHrs>  
</ProductModel>  
...  
```  
  
 Au lieu de retourner le résultat au format XML, vous pouvez écrire la requête pour générer des résultats relationnels, comme l'illustre la requête suivante :  
  
```  
SELECT ProductModelID,         
        Name,         
        Instructions.value('declare namespace   
      AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
    sum(//AWMI:Location/@LaborHours)', 'float') as TotalLaborHours         
FROM Production.ProductModel         
WHERE Instructions is not NULL          
```  
  
 Voici un extrait du résultat :  
  
```  
ProductModelID Name                 TotalLaborHours         
-------------- -------------------------------------------------  
7              HL Touring Frame           12.75                   
10             LL Touring Frame           13                      
43             Touring Rear Wheel         3                       
...  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Seule la version de l’argument unique de **sum()** est pris en charge.  
  
-   Si l'entrée est une séquence vide calculée dynamiquement, la valeur 0 du type de base utilisé est retournée à la place du type xs:integer.  
  
-   Le **sum()** fonction mappe tous les entiers à xs : decimal.  
  
-   Le **sum()** fonction sur des valeurs de type xs : Duration n’est pas pris en charge.  
  
-   Les séquences faisant intervenir plusieurs types dérivés de différents types de base ne sont pas prises en charge.  
  
-   L'expression sum((xs:double(“INF”), xs:double(“-INF”))) génère une erreur de domaine.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
