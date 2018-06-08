---
title: 'Exemple : spécification des directives ID et IDREFS | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- IDREFS directive
- ID directive
ms.assetid: 99b9f0d8-ecbb-4225-859f-881066c09785
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5483235964a1b10decad823313e3657809a7d34a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="example-specifying-the-id-and-idrefs-directives"></a>Exemple : spécification des directives ID et IDREFS
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Un attribut d’élément peut être spécifié comme attribut de type **ID** et l’attribut **IDREFS** peut ensuite être utilisé pour y faire référence. Cela permet de créer des liens à l'intérieur du document et est assimilable aux relations entre clés primaires et clés étrangères dans les bases de données relationnelles.  
  
 Cet exemple illustre l’utilisation des directives **ID** et **IDREFS** pour créer des attributs de types **ID** et **IDREFS** . Étant donné que les ID ne peuvent pas être des valeurs entières, dans cet exemple, leurs valeurs sont converties. En d'autres termes, ils subissent une conversion de type. Des préfixes sont utilisés pour les valeurs d'ID.  
  
 Supposons que vous souhaitez construire un document XML comme suit :  
  
```  
<Customer CustomerID="C1" SalesOrderIDList=" O11 O22 O33..." >  
    <SalesOrder SalesOrderID="O11" OrderDate="..." />  
    <SalesOrder SalesOrderID="O22" OrderDate="..." />  
    <SalesOrder SalesOrderID="O33" OrderDate="..." />  
    ...  
</Customer>  
```  
  
 L'attribut `SalesOrderIDList` de l'élément < `Customer` > est un attribut à valeurs multiples qui fait référence à l'attribut `SalesOrderID` de l'élément < `SalesOrder` >. Pour établir ce lien, vous devez déclarer l’attribut `SalesOrderID` comme étant de type `ID` et l’attribut `SalesOrderIDList` de l’élément <`Customer`> comme étant de type `IDREFS`. Comme un client peut passer plusieurs commandes, le type `IDREFS` est utilisé.  
  
 Les éléments de type **IDREFS** ont également plusieurs valeurs. Par conséquent, vous devez recourir à une clause de sélection distincte qui réutilisera les mêmes informations de colonne de balise, de parent et de clé. La clause `ORDER BY` doit faire en sorte que les lignes qui composent les valeurs **IDREFS** apparaissent regroupées sous leur élément parent.  
  
 La requête ci-après génère le document XML souhaité. La requête utilise les directives `ID` et `IDREFS` pour remplacer les types dans les noms de colonne (`SalesOrder!2!SalesOrderID!ID`, `Customer!1!SalesOrderIDList!IDREFS`).  
  
```  
USE AdventureWorks2012;  
GO  
SELECT  1 as Tag,  
        0 as Parent,  
        C.CustomerID       [Customer!1!CustomerID],  
        NULL               [Customer!1!SalesOrderIDList!IDREFS],  
        NULL               [SalesOrder!2!SalesOrderID!ID],  
        NULL               [SalesOrder!2!OrderDate]  
FROM   Sales.Customer C   
UNION ALL   
SELECT  1 as Tag,  
        0 as Parent,  
        C.CustomerID,  
        'O-'+CAST(SalesOrderID as varchar(10)),   
        NULL,  
        NULL  
FROM   Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID  
UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
        C.CustomerID,  
        NULL,  
        'O-'+CAST(SalesOrderID as varchar(10)),  
        OrderDate  
FROM   Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerIDORDER BY [Customer!1!CustomerID] ,  
         [SalesOrder!2!SalesOrderID!ID],  
         [Customer!1!SalesOrderIDList!IDREFS]  
FOR XML EXPLICIT;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
