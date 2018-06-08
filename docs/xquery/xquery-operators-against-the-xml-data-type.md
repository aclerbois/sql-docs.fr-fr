---
title: Opérateurs XQuery sur le Type de données xml | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
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
- XQuery, operators
- operators [XQuery]
- xml data type [SQL Server], XQuery
ms.assetid: 39ca3d2e-e928-4333-872b-75c4ccde8e79
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 62c4875c74d6ff67e8d1760a29ac48672fc7765a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xquery-operators-against-the-xml-data-type"></a>Opérateurs XQuery sur le type de données xml
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  XQuery prend en charge les opérateurs suivants :  
  
-   Opérateurs numériques (+, -, *, div, mod)  
  
-   Opérateurs de comparaison de valeurs (eq, ne, lt, gt, le, ge)  
  
-   Opérateurs de comparaison générale (=, ! =, \<, >, \<=, > =)  
  
 Pour plus d’informations sur ces opérateurs, consultez [Expressions de comparaison &#40;XQuery&#41;](../xquery/comparison-expressions-xquery.md)  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-general-operators"></a>A. Utilisation d'opérateurs généraux  
 La requête illustre l'utilisation d'opérateurs généraux qui s'appliquent à des séquences et qui comparent également des séquences. La requête récupère une séquence de numéros de téléphone pour chaque client à partir de la **AdditionalContactInfo** colonne de la **Contact** table. Cette séquence est ensuite comparée à la séquence de deux numéros de téléphone ("111-111-1111", "222-2222").  
  
 La requête utilise le **=** opérateur de comparaison. Chaque nœud de la séquence situé à droite de la **=** opérateur est comparée à chaque nœud de la séquence situé à gauche. Si les nœuds correspondent, la comparaison de nœud est **TRUE**. Elle est ensuite convertie en int et comparée à 1, puis la requête retourne l'ID de client.  
  
```  
WITH XMLNAMESPACES (  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS ACI,  
'http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS ACT)  
SELECT ContactID   
FROM   Person.Contact  
WHERE  AdditionalContactInfo.value('  
      //ACI:AdditionalContactInfo//ACT:telephoneNumber/ACT:number =   
          ("111-111-1111", "222-2222")',  
      'bit')= cast(1 as bit)  
```  
  
 Il existe un autre moyen d’observer le fonctionne de la requête précédente : chaque valeur numéro de téléphone téléphone récupérée à partir du **AdditionalContactInfo** colonne est comparée à l’ensemble de deux numéros de téléphone. Si la valeur se trouve dans le jeu, le client correspondant est retourné dans le résultat.  
  
### <a name="b-using-a-numeric-operator"></a>B. Utilisation d'un opérateur numérique  
 L'opérateur + de cette requête est un opérateur de valeur, car il s'applique à un seul élément. Par exemple, la valeur 1 est ajoutée à une taille de lot retournée par la requête :  
  
```  
SELECT ProductModelID, Instructions.query('  
     declare namespace   
 AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     for $i in (/AWMI:root/AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"  
                   LotSize  = "{  number($i/@LotSize) }"  
                   LotSize2 = "{ number($i/@LotSize) + 1 }"  
                   LotSize3 = "{ number($i/@LotSize) + 2 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
### <a name="c-using-a-value-operator"></a>C. Utilisation d'un opérateur de valeur  
 La requête suivante récupère les éléments <`Picture`> pour un modèle de produit pour lequel la taille d'image est « small » :  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $P in /PD:ProductDescription/PD:Picture[PD:Size eq "small"]  
     return  
           $P  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Étant donné que les opérandes de le **eq** opérateur sont des valeurs atomiques, l’opérateur de valeur est utilisée dans la requête. Vous pouvez écrire la même requête en utilisant l’opérateur de comparaison générale ( **=** ).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le Type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
