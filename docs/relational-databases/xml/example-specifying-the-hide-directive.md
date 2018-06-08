---
title: 'Exemple : spécification de la directive HIDE | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- HIDE directive
ms.assetid: 87504d87-1cbd-412a-9041-47884b6efcec
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b71fa8a58aa2e18e2de3cb24da42cf8c470c40f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="example-specifying-the-hide-directive"></a>Exemple : spécification de la directive HIDE
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cet exemple illustre l'utilisation de la directive **HIDE** . Cette directive est utile quand vous souhaitez que la requête renvoie un attribut pour classer les lignes de la table universelle renvoyée par la requête, sans que cet attribut figure dans le document XML obtenu au final.  
  
 Cette requête construit le document XML suivant :  
  
```  
<ProductModel ProdModelID="19" Name="Mountain-100">  
  <Summary>  
    <SummaryDescription>  
           <Summary> element from XML stored in CatalogDescription column  
    </SummaryDescription>  
  </Summary>  
</ProductModel>  
```  
  
 Cette requête génère le document XML souhaité. Elle identifie deux groupes de colonnes possédant 1 et 2 comme valeurs Tag dans les noms de colonnes.  
  
 Cette requête utilise la [méthode query() (type de données xml)](../../t-sql/xml/query-method-xml-data-type.md) du type de données **xml** pour interroger la colonne CatalogDescription de type **xml** afin d’extraire la description résumée. Elle utilise également la [méthode value() (type de données xml)](../../t-sql/xml/value-method-xml-data-type.md) du type de données **xml** pour extraire la valeur ProductModelID de la colonne CatalogDescription. Cette valeur n'est pas requise dans le document XML obtenu, mais elle est nécessaire pour trier l'ensemble de lignes obtenu. Par conséquent, le nom de colonne, `[Summary!2!ProductModelID!HIDE]`, inclut la directive **HIDE** . Si cette colonne n'est pas incluse dans l'instruction SELECT, vous devez trier l'ensemble de lignes par `[ProductModel!1!ProdModelID]` et `[Summary!2!SummaryDescription]` , de type **xml** , et vous ne pouvez pas utiliser la colonne de type **xml** dans la clause ORDER BY. Par conséquent, la colonne `[Summary!2!ProductModelID!HIDE]` est ajoutée puis spécifiée dans la clause ORDER BY.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT  1 as Tag,  
        0 as Parent,  
        ProductModelID     as [ProductModel!1!ProdModelID],  
        Name               as [ProductModel!1!Name],  
        NULL               as [Summary!2!ProductModelID!hide],  
        NULL               as [Summary!2!SummaryDescription]  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not null  
UNION ALL  
SELECT  2 as Tag,  
        1 as Parent,  
        ProductModelID,  
        Name,  
        CatalogDescription.value('  
         declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       (/PD:ProductDescription/@ProductModelID)[1]', 'int'),  
        CatalogDescription.query('  
         declare namespace pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /pd:ProductDescription/pd:Summary')  
FROM    Production.ProductModel  
WHERE   CatalogDescription is not null  
ORDER BY [ProductModel!1!ProdModelID],[Summary!2!ProductModelID!hide]  
FOR XML EXPLICIT  
go  
```  
  
 Voici le résultat obtenu :  
  
```  
<ProductModel ProdModelID="19" Name="Mountain-100">  
  <Summary>  
    <SummaryDescription>  
      <pd:Summary xmlns:pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription" xmlns="">  
        <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">Our top-of-the-line competition mountain bike. Performance-enhancing options include the innovative HL Frame, super-smooth front suspension, and traction for all terrain. </p1:p>  
      </pd:Summary>  
    </SummaryDescription>  
  </Summary>  
</ProductModel>  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
