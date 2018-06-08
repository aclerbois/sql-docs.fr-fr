---
title: Exemple de mise en forme des données | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], about data shaping
ms.assetid: 1bfdcad4-52e1-45bc-ad21-783657ef0a44
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3ead77ee3f08e2a4ffe9e385b32d289a77a55a92
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-shaping-example"></a>Exemple de mise en forme des données
Les données suivantes, mise en forme de la commande montre comment générer une liste hiérarchique **Recordset** à partir de la **clients** et **commandes** tables dans la base de données Northwind.  
  
```  
SHAPE {SELECT CustomerID, ContactName FROM Customers}   
APPEND ({SELECT OrderID, OrderDate, CustomerID FROM Orders} AS chapOrders   
RELATE customerID TO customerID)   
```  
  
 Lorsque cette commande est utilisée pour ouvrir un **Recordset** objet (tel qu’indiqué dans [Visual Basic exemple de données mise en forme](../../../ado/guide/data/visual-basic-example-of-data-shaping.md)), il crée un chapitre (**chapOrders**) pour chaque enregistrement retourné à partir de la **clients** table. Ce chapitre se compose d’un sous-ensemble de la **Recordset** retourné à partir de la **commandes** table. Le **chapOrders** chapitre contient toutes les informations demandées sur les commandes passées par le client donné. Dans cet exemple, le chapitre se compose de trois colonnes : **OrderID**, **OrderDate**, et **CustomerID**.  
  
 Les deux premières entrées de la résultante en forme **Recordset** sont les suivantes :  
  
|CustomerID|ContactName|OrderID|OrderDate|CustomerID|  
|----------------|-----------------|-------------|---------------|----------------|  
|ALFKI|Maria Ander|10643<br /><br /> 10692<br /><br /> 10702<br /><br /> 10835<br /><br /> 10952<br /><br /> 11011|1997-08-25<br /><br /> 1997-10-03<br /><br /> 1997-10-13<br /><br /> 1998-01-15<br /><br /> 1998-03-16<br /><br /> 1998-04-09|ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI|  
|ANATR|Ana Trujillo|10308<br /><br /> 10625<br /><br /> 10759<br /><br /> 10926|1996-09-18<br /><br /> 1997-08-08<br /><br /> 1997-11-28<br /><br /> 1998-03-04|ANATR<br /><br /> ANATR<br /><br /> ANATR<br /><br /> ANATR|  
  
 Dans une commande de la forme, ajouter permet de créer un enfant **Recordset** liés au parent **Recordset** (tel qu’il est retourné à partir de la commande spécifique au fournisseur immédiatement après le mot clé de forme qui a été indiqué plus haut) par la clause RELATE. Le parent et enfant ont en général au moins une colonne en commun : la valeur de la colonne dans une ligne du parent est identique à la valeur de la colonne de toutes les lignes de l’enfant.  
  
 Il existe un deuxième moyen d’utiliser les commandes de la forme : à savoir, pour générer un parent **Recordset** à partir d’un enfant **Recordset**. Les enregistrements de l’enfant **Recordset** sont regroupées, généralement à l’aide de la clause BY et une ligne est ajoutée au parent **Recordset** pour chaque groupe résultant de l’enfant. Si la clause BY est omise, l’enfant **Recordset** sera le formulaire d’un seul groupe et le parent **Recordset** contient exactement une ligne. Cela est utile pour calculer des agrégats de « totaux » sur l’intégralité de l’enfant **Recordset**.  
  
 La construction de la commande forme vous permet également de créer par programmation une forme **Recordset**. Vous pouvez ensuite accéder les composants de la **Recordset** par programme ou via un contrôle visuel approprié. Une commande de la forme est émise comme tout autre texte de commande ADO. Pour plus d’informations, consultez [en général les commandes Shape](../../../ado/guide/data/shape-commands-in-general.md).  
  
 Quelle que soit la façon dont le parent **Recordset** est construit, il contient une colonne de chapitre est utilisée pour établir une relation à un enfant **Recordset**. Si vous le souhaitez, le parent **Recordset** peut également posséder des colonnes contenant des agrégats (SUM, MIN, MAX, etc.) sur les lignes enfants. Le parent et l’enfant **Recordset** peuvent comporter des colonnes qui contiennent une expression sur la ligne dans le **Recordset**, ainsi que les colonnes qui sont nouveaux et initialement vides.  
  
 Cette section se poursuit avec la rubrique suivante.  
  
-   [Exemple Visual Basic de mise en forme des données](../../../ado/guide/data/visual-basic-example-of-data-shaping.md)
