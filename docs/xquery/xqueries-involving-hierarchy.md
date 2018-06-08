---
title: Requêtes XQuery impliquant une hiérarchie | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
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
- hierarchies [XQuery]
- XQuery, hierarchies
ms.assetid: 6953d8b7-bad8-4b64-bf7b-12fa4f10f65c
caps.latest.revision: 25
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: dd9e93969bd8677311edc22ae61f314c8b89c5d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xqueries-involving-hierarchy"></a>Requêtes XQuery impliquant une hiérarchie
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  La plupart des **xml** type des colonnes dans le **AdventureWorks** base de données sont des documents semi-structurés. Par conséquent, les documents stockés dans chaque ligne peuvent avoir un aspect différent. Les exemples de requêtes fournis dans cette rubrique montrent comment extraire des informations de ces divers documents.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-from-the-manufacturing-instructions-documents-retrieve-work-center-locations-together-with-the-first-manufacturing-step-at-those-locations"></a>A. Extraction, à partir des instructions de fabrication, des postes de travail ainsi que de la première étape de fabrication réalisée sur ces différents postes  
 Pour le modèle de produit 7, la requête construit le document XML qui comprend le <`ManuInstr`> élément, avec **ProductModelID** et **ProductModelName** attributs et un ou plusieurs <`Location`> des éléments enfants.  
  
 Chaque élément <`Location`> dispose de son propre ensemble d'attributs et d'un élément enfant <`step`>. Cet élément enfant <`step`> représente la première étape de fabrication réalisée sur le poste de travail.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   \<ManuInstr  ProdModelID = "{sql:column("Production.ProductModel.ProductModelID") }"   
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
            {   
              for $wc in //AWMI:root/AWMI:Location  
              return  
                <Location>  
                 {$wc/@* }  
                 <step1> { string( ($wc//AWMI:step)[1] ) } </step1>  
                </Location>  
            }  
          </ManuInstr>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le **espace de noms** mot clé dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit un préfixe d’espace de noms. Ce préfixe est utilisé ultérieurement dans le corps de la requête.  
  
-   Les jetons de basculement de contexte, {) et (}, sont utilisés pour faire passer la requête de la construction XML à sa propre évaluation.  
  
-   Le **SQL :Column()** est utilisé pour inclure une valeur relationnelle dans le code XML qui est en cours de construction.  
  
-   Lors de la construction de l'élément <`Location`>, $wc/@* récupère tous les attributs des postes de travail.  
  
-   Le **string()** fonction retourne la valeur de chaîne dans le <`step`> élément.  
  
 Voici un extrait du résultat :  
  
```  
<ManuInstr ProdModelID="7" ProductModelName="HL Touring Frame">  
   <Location LocationID="10" SetupHours="0.5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
     <step1>Insert aluminum sheet MS-2341 into the T-85A   
             framing tool.</step1>  
   </Location>  
   <Location LocationID="20" SetupHours="0.15"   
            MachineHours="2" LaborHours="1.75" LotSize="1">  
      <step1>Assemble all frame components following   
             blueprint 1299.</step1>  
   </Location>  
...  
</ManuInstr>   
```  
  
### <a name="b-find-all-telephone-numbers-in-the-additionalcontactinfo-column"></a>B. Recherche de tous les numéros de téléphone de la colonne AdditionalContactInfo  
 La requête suivante récupère tous les numéros de téléphone supplémentaires définis pour un contact client spécifique en recherchant l'élément <`telephoneNumber`> dans l'ensemble de la hiérarchie. Dans la mesure où l'élément <`telephoneNumber`> peut apparaître n'importe où dans la hiérarchie, la requête utilise l'opérateur descendant-and-self (//) dans la recherche.  
  
```  
SELECT AdditionalContactInfo.query('  
 declare namespace ci="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
 declare namespace act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
for $ph in /ci:AdditionalContactInfo//act:telephoneNumber  
   return  
      $ph/act:number  
') as x  
FROM  Person.Contact  
WHERE ContactID = 1  
```  
  
 Voici le résultat obtenu :  
  
```  
\<act:number   
  xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  112-111-1111  
\</act:number>  
```  
  
 Pour récupérer uniquement les numéros de téléphone de premier niveau, et plus particulièrement les éléments enfants <`telephoneNumber`> de <`AdditionalContactInfo`>, l'expression FOR de la requête devient  
  
 `for $ph in /ci:AdditionalContactInfo/act:telephoneNumber`.  
  
## <a name="see-also"></a>Voir aussi  
 [Principes fondamentaux de XQuery](../xquery/xquery-basics.md)   
 [Construction XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
  
