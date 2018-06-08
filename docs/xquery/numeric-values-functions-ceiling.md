---
title: Fonction CEILING (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- fn:ceiling function
- ceiling function [XQuery]
ms.assetid: 594f1dd0-3c27-41b3-b809-9ce6714c5a97
caps.latest.revision: 30
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 1399fd20bf4d7af3fed85730fc397e1400347ad2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="numeric-values-functions---ceiling"></a>Fonctions de valeurs numériques - ceiling 
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Renvoie le nombre le plus petit sans portion décimale qui n'est pas inférieur à la valeur de cet argument. Si l'argument est une séquence vide, la fonction renvoie la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:ceiling ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nombre à laquelle s'applique la fonction.  
  
## <a name="remarks"></a>Notes  
 Si le type de *$arg* est un des trois types numériques de base, **xs : float**, **xs : double**, ou **xs : decimal**, le type de retour est le même que le *$arg* type.  
  
 Si le type de *$arg* est un type qui est dérivé de l’un des types numériques, le type de retour est le type numérique de base.  
  
 Si l’entrée pour les fonctions fn : Floor, fn : Ceiling ou fn : Round est **xdt : untypedAtomic**, elle est implicitement convertie en **xs : double**.  
  
 Tout autre type génère une erreur statique.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** colonnes de type dans la base de données AdventureWorks.  
  
### <a name="a-using-the-ceiling-xquery-function"></a>A. Utilisation de la fonction ceiling() de XQuery  
 Pour le modèle de produit 7, cette requête renvoie une liste des postes de travail que compte le processus de fabrication du modèle de produit. Pour chaque poste de travail, la requête renvoie l'ID, les heures de main-d'œuvre et la taille des lots, le cas échéant. La requête utilise le **plafond** fonction pour retourner les heures de main-d'œuvre sous forme de valeurs de type **décimal**.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
     for $i in /AWMI:root/AWMI:Location  
     return   
       <Location LocationID="{ $i/@LocationID }"   
                   LaborHrs="{ ceiling($i/@LaborHours) }" >  
                    {   
                      $i/@LotSize  
                    }    
       </Location>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le préfixe d'espace de noms AWMI signifie Adventure Works Manufacturing Instructions (instructions de fabrication d'Adventure Works). Ce préfixe fait référence à l'espace de noms utilisé dans le document interrogé.  
  
-   **Instructions** est un **xml** colonne de type. Par conséquent, le [méthode query() (type de données XML)](../t-sql/xml/query-method-xml-data-type.md) est utilisé pour spécifier une requête XQuery. L'instruction XQuery est spécifiée comme argument de la méthode query.  
  
-   **pour... retourner** est une construction de la boucle. Dans la requête, le **pour** boucle identifie une liste de \<emplacement > éléments. Pour chaque poste de travail, le **retourner** instruction dans le **pour** boucle décrit le code XML à générer :  
  
    -   A \<emplacement > élément possédant les attributs LocationID et LaborHrs. L'expression correspondante entre accolades ({ }) récupère les valeurs requises à partir du document.  
  
    -   Le {$i/@LotSize } expression récupère l’attribut LotSize du document, le cas échéant.  
  
    -   Voici le résultat obtenu :  
  
```  
ProductModelID Result    
-------------- ------------------------------------------------------  
7      <Location LocationID="10" LaborHrs="3" LotSize="100"/>  
       <Location LocationID="20" LaborHrs="2" LotSize="1"/>     
       <Location LocationID="30" LaborHrs="1" LotSize="1"/>     
       <Location LocationID="45" LaborHrs="1" LotSize="20"/>  
       <Location LocationID="60" LaborHrs="3" LotSize="1"/>     
       <Location LocationID="60" LaborHrs="4" LotSize="1"/>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **ceiling()** fonction mappe toutes les valeurs entières à xs : decimal.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction FLOOR &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Fonction Round &#40;XQuery&#41;](../xquery/numeric-values-functions-round.md)  
  
  
