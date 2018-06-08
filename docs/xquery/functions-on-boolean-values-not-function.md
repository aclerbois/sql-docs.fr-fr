---
title: pas de fonction (XQuery) | Documents Microsoft
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
- effective Boolean value [XQuery]
- fn:not function
- not function [XQuery]
- EBV
ms.assetid: 93dfc377-45f1-4384-9392-560d9331a915
caps.latest.revision: 33
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: f3da41971a8af3fe92fc8c7034fa6cd749a68ac8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-boolean-values---not-function"></a>Fonctions sur des valeurs booléennes - pas de fonction 
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne la valeur TRUE si la valeur booléenne effective de *$arg* a la valeur false et retourne FALSE si la valeur booléenne effective de *$arg* a la valeur true.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:not($arg as item()*) as xs:boolean  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence d'éléments pour lesquels existe une valeur booléenne effective.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-using-the-not-xquery-function-to-find-product-models-whose-catalog-descriptions-do-not-include-the-specifications-element"></a>A. À l’aide de la fonction XQuery not() pour rechercher les modèles de produit dont les descriptions du catalogue n’incluent pas les \<spécifications > élément.  
 La requête suivante construit le document XML qui contient les ID des modèles de produit dont la description de catalogue ne comprend pas l'élément <`Specifications`>.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
       <Product   
           ProductModelID="{ sql:column("ProductModelID") }"  
        />  
') as Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications/*)]  '  
     ) = 0  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Étant donné que le document utilise des espaces de noms, l'exemple recourt à l'instruction WITH NAMESPACES. Une autre option consiste à utiliser le **déclarer l’espace de noms** mot clé dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) pour définir le préfixe.  
  
-   La requête construit ensuite le document XML qui comprend le <`Product`> élément et ses **ProductModelID** attribut.  
  
-   La clause WHERE utilise le [méthode exist() (type de données XML)](../t-sql/xml/exist-method-xml-data-type.md) pour filtrer les lignes. Le **exist()** méthode retourne la valeur True s’il n’y \<ProductDescription > les éléments qui n’ont pas \<spécification > des éléments enfants. Notez l’utilisation de la **not()** (fonction).  
  
 Ce jeu de résultats est vide, car chaque description de catalogue de modèles de produit comprend le \<spécifications > élément.  
  
### <a name="b-using-the-not-xquery-function-to-retrieve-work-center-locations-that-do-not-have-a-machinehours-attribute"></a>B. Utilisation de la fonction XQuery not() pour extraire les sites de production dépourvus de l'attribut MachineHours  
 La requête suivante porte sur la colonne Instructions. Cette colonne stocke les instructions de fabrication des modèles de produit.  
  
 Pour un modèle de produit particulier, la requête extrait les sites de production qui ne spécifient pas l'attribut MachineHours. Autrement dit, l’attribut **MachineHours** n’est pas spécifié pour le \<emplacement > élément.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in /AWMI:root/AWMI:Location[not(@MachineHours)]  
     return  
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ $i/@LaborHours }" >  
        </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7   
```  
  
 Dans la requête précédente, notez les points suivants :  
  
-   Le **declarenamespace** dans [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit le préfixe d’espace de noms des instructions de fabrication d’Adventure Works. Il représente le même espace de noms que celui utilisé dans le document des instructions de fabrication.  
  
-   Dans la requête, le **pas (@MachineHours)** prédicat retourne la valeur True s’il existe aucune **MachineHours** attribut.  
  
 Voici le résultat obtenu :  
  
```  
ProductModelID Result   
-------------- --------------------------------------------  
7              <Location LocationID="30" LaborHrs="1"/>  
               <Location LocationID="50" LaborHrs="3"/>  
               <Location LocationID="60" LaborHrs="4"/>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **not()** fonction prend uniquement en charge les arguments de type xs : Boolean ou node() * ou la séquence vide.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
