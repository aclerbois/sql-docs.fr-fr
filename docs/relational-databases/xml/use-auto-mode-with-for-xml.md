---
title: Utiliser le mode AUTO avec FOR XML | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, AUTO mode
- ELEMENTS option
- FOR XML AUTO mode
- AUTO FOR XML mode
ms.assetid: 7140d656-1d42-4f01-a533-5251429f4450
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5622f01bcfa3884391654e1b4ad3170985ccbb58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-auto-mode-with-for-xml"></a>Utiliser le mode AUTO avec FOR XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Comme décrit dans [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md), le mode AUTO retourne des résultats de requête en tant qu’éléments XML imbriqués. Cela ne permet pas de contrôler de façon précise la forme du document XML généré à partir du résultat d'une requête. Les requêtes au mode AUTO sont utiles pour générer des hiérarchies simples. Toutefois, [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md) et [Utiliser le mode PATH avec FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md) fournissent davantage de contrôle et de souplesse pour déterminer la forme du XML à partir d’un résultat de requête.  
  
 Chaque table de la clause FROM, dont au moins une colonne est répertoriée dans la clause SELECT, est représentée sous la forme d'un élément XML. Les colonnes répertoriées dans la clause SELECT sont mappées avec des attributs ou des sous-éléments si l'option ELEMENTS facultative est spécifiée dans la clause FOR XML.  
  
 La hiérarchie XML, imbrication des éléments, obtenue dans les données XML dépend de l'ordre des tables définies par les colonnes indiquées dans la clause SELECT. Par conséquent, l'ordre dans lequel les noms de colonnes sont spécifiés dans la clause SELECT est significatif. La première table identifiée, celle qui se trouve le plus à gauche, constitue l'élément du plus haut niveau dans le document XML résultant. La deuxième table la plus à gauche, identifiée par des colonnes dans l'instruction SELECT, constitue un sous-élément de l'élément de plus haut niveau et ainsi de suite.  
  
 Si un nom de colonne répertorié dans la clause SELECT provient d'une table déjà identifiée par une colonne précédemment spécifiée dans cette clause, la colonne est ajoutée en tant qu'attribut de l'élément déjà créé et aucun nouveau niveau de hiérarchie n'est ouvert. Si l'option ELEMENTS est spécifiée, la colonne est ajoutée en tant qu'attribut.  
  
 Exécutez par exemple cette requête :  
  
```  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       OrderHeader.Status,  
       Cust.CustomerType  
FROM Sales.Customer Cust, Sales.SalesOrderHeader OrderHeader  
WHERE Cust.CustomerID = OrderHeader.CustomerID  
ORDER BY Cust.CustomerID  
FOR XML AUTO  
```  
  
 Voici le résultat partiel :  
  
```  
<Cust CustomerID="1" CustomerType="S">  
  <OrderHeader CustomerID="1" SalesOrderID="43860" Status="5" />  
  <OrderHeader CustomerID="1" SalesOrderID="44501" Status="5" />  
  <OrderHeader CustomerID="1" SalesOrderID="45283" Status="5" />  
  <OrderHeader CustomerID="1" SalesOrderID="46042" Status="5" />  
</Cust>  
...  
```  
  
 Notez les points suivants relatifs à la clause SELECT :  
  
-   L'identificateur CustomerID référence la table Cust. Par conséquent, un élément <`Cust`> est créé et un attribut CustomerID y est ajouté.  
  
-   Ensuite, trois colonnes, OrderHeader.CustomerID, OrderHeader.SaleOrderID et OrderHeader.Status, référencent la table OrderHeader. Par conséquent, un élément <`OrderHeader`> est ajouté en tant que sous-élément de l'élément <`Cust`> et les trois colonnes sont ajoutées en tant qu'attributs de <`OrderHeader`>.  
  
-   Ensuite, la colonne Cust.CustomerType référence de nouveau la table Cust déjà identifiée par la colonne Cust.CustomerID. Aucun nouvel élément n'est donc créé. À la place, l'attribut CustomerType est ajouté à l'élément <`Cust`> précédemment créé.  
  
-   La requête spécifie des alias pour les noms de tables. Ces alias apparaissent sous la forme de noms d'éléments correspondants.  
  
-   La clause ORDER BY est requise pour regrouper tous les enfants situés sous un même parent.  
  
 La requête suivante est similaire à la précédente, sauf que la clause SELECT spécifie des colonnes de la table OrderHeader avant les colonnes de la table Cust. Par conséquent, un premier élément <`OrderHeader`> est créé, auquel est ensuite ajouté l'élément enfant <`Cust`>.  
  
```  
select OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       OrderHeader.Status,  
       Cust.CustomerID,   
       Cust.CustomerType  
from Sales.Customer Cust, Sales.SalesOrderHeader OrderHeader  
where Cust.CustomerID = OrderHeader.CustomerID  
for xml auto  
```  
  
 Voici le résultat partiel :  
  
```  
<OrderHeader CustomerID="1" SalesOrderID="43860" Status="5">  
  <Cust CustomerID="1" CustomerType="S" />  
</OrderHeader>  
...  
```  
  
 Si l'option ELEMENTS est ajoutée à la clause FOR XML, des données XML centrées sur l'élément sont renvoyées.  
  
```  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       OrderHeader.Status,  
       Cust.CustomerType  
FROM Sales.Customer Cust, Sales.SalesOrderHeader OrderHeader  
WHERE Cust.CustomerID = OrderHeader.CustomerID  
ORDER BY Cust.CustomerID  
FOR XML AUTO, ELEMENTS  
```  
  
 Voici le résultat partiel :  
  
```  
<Cust>  
  <CustomerID>1</CustomerID>  
  <CustomerType>S</CustomerType>  
  <OrderHeader>  
    <CustomerID>1</CustomerID>  
    <SalesOrderID>43860</SalesOrderID>  
    <Status>5</Status>  
  </OrderHeader>  
   ...  
</Cust>  
...  
```  
  
 Dans cette requête, les valeurs CustomerID sont comparées d’une ligne à l’autre lors de la création des éléments \<Cust>, car CustomerID est la clé primaire de la table. Si CustomerID n'est pas identifié en tant que clé primaire de la table, toutes les valeurs de colonnes (CustomerID et CustomerType dans cette requête) sont comparées d'une ligne à l'autre. Si les valeurs diffèrent, un nouvel élément \<Cust> est ajouté au document XML.  
  
 Lors de la comparaison de ces valeurs de colonnes, si l’une des colonnes à comparer est de type **text**, **ntext**, **image**ou **xml**, la clause FOR XML considère que les valeurs sont différentes et non comparées, même si elles peuvent être identiques. Cela est dû au fait que la comparaison des objets volumineux n'est pas prise en charge. Des éléments sont ajoutés au résultat pour chaque ligne sélectionnée. Les colonnes de type **(n)varchar(max)** et **varbinary(max)** sont comparées.  
  
 À l'image d'une colonne d'agrégation ou calculée, lorsqu'une colonne figurant dans la clause SELECT ne peut être associée à aucune des tables identifiées dans la clause FROM, elle est ajoutée au document XML dans le niveau d'imbrication le plus profond dans la liste. Si une telle colonne apparaît comme première colonne dans la clause SELECT, elle est ajoutée à l'élément supérieur.  
  
 Si le caractère générique astérisque « * » est spécifié dans la clause SELECT, l'imbrication est déterminée de la même façon que celle précédemment décrite, en fonction de l'ordre dans lequel les lignes sont renvoyées par le moteur de requête.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques suivantes fournissent des informations supplémentaires sur le mode AUTO :  
  
-   [Utiliser l'option BINARY BASE64](../../relational-databases/xml/use-the-binary-base64-option.md)  
  
-   [Heuristique du mode AUTO permettant de définir la forme des données XML renvoyées](../../relational-databases/xml/auto-mode-heuristics-in-shaping-returned-xml.md)  
  
-   [Exemples : utilisation du mode AUTO](../../relational-databases/xml/examples-using-auto-mode.md)  
  
## <a name="see-also"></a> Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
