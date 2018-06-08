---
title: En spécifiant un chemin d’accès d’emplacement (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- absolute location path
- node-set [SQLXML]
- XPath queries [SQLXML], location paths
- relative location path [SQLXML]
- location path for XPath query
ms.assetid: a23a2b75-bc69-49f0-99db-05e14dc15bc0
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2c2cad3730cd0948f94adc8ad5b877fd2e921bc3
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708737"
---
# <a name="specifying-a-location-path-sqlxml-40"></a>Spécification d'un chemin d'accès d'emplacement (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Les requêtes XPath sont spécifiées sous la forme d'une expression. Il existe divers types d'expressions. Un chemin d'accès d'emplacement est une expression qui sélectionne un ensemble de nœuds associés au nœud de contexte. L'évaluation d'un chemin d'accès d'emplacement doit aboutir à un élément node-set.  
  
## <a name="types-of-location-paths"></a>Types de chemin d'accès d'emplacement  
 Un chemin d'accès d'emplacement peut adopter l'une ou l'autre des formes suivantes :  
  
-   **Chemin d’accès d’emplacement absolu**  
  
     Un chemin d'accès d'emplacement absolu démarre au nœud racine du document. Il se compose d'une barre oblique (/) suivie éventuellement d'un chemin d'accès relatif. La barre oblique (/) sélectionne le nœud racine du document.  
  
-   **Chemin d’accès relatif**  
  
     Un chemin d'accès relatif de l'emplacement démarre au nœud de contexte dans le document. Un chemin d'accès d'emplacement consiste en une séquence d'une ou plusieurs étapes d'emplacement séparées par une barre oblique (/). Chaque étape sélectionne un ensemble de nœuds associés au nœud de contexte. La première séquence d'étapes sélectionne un ensemble de nœuds associés à un nœud de contexte. Chaque nœud dans cet ensemble est utilisé comme un nœud de contexte pour l'étape suivante. Les ensembles de nœuds identifiés par cette étape sont joints. Par exemple, **child::Order/child::OrderDetail** sélectionne le  **\<OrderDetail >** éléments enfants de la  **\<ordre >** éléments enfants du nœud de contexte.  
  
    > [!NOTE]  
    >  Dans l'implémentation SQLXML 4.0 de XPath, chaque requête XPath démarre au contexte racine, même si la requête XPath n'est pas explicitement absolue. Par exemple, une requête XPath commençant par « Customer » (Client) est traitée comme « /Customer ». Dans la requête XPath **Customer [Order]**, Customer démarre au contexte racine mais Order démarre au contexte Customer. Pour plus d’informations, consultez [Introduction à l’aide de requêtes XPath &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/introduction-to-using-xpath-queries-sqlxml-4-0.md).  
  
## <a name="location-steps"></a>Étapes d'emplacement  
 Un chemin d'accès d'emplacement (absolu ou relatif) est composé d'étapes d'emplacement contenant trois parties :  
  
-   **Axis**  
  
     L'axe spécifie la relation d'arborescence entre les nœuds sélectionnés par l'étape d'emplacement et le nœud de contexte. Le **parent**, **enfant**, **attribut**, et **self** axes sont pris en charge. Si un **enfant** axe est spécifié dans le chemin d’accès de l’emplacement, tous les nœuds sélectionnés par la requête sont les enfants du nœud de contexte. Si un **parent** axe est spécifié, le nœud sélectionné est le nœud parent du nœud de contexte. Si un **attribut** axe est spécifié, les nœuds sélectionnés sont les attributs du nœud de contexte.  
  
-   **Test de nœud**  
  
     Un test de nœud spécifie le type de nœud sélectionné par le niveau d'emplacement. Chaque axe (**enfant**, **parent**, **attribut**, et **self**) a un type de nœud principal. Pour le **attribut** axe, le type de nœud principal est  **\<attribut >**. Pour le **parent**, **enfant**, et **self** axes, le type de nœud principal est  **\<élément >**.  
  
     Par exemple, si le chemin d’accès d’emplacement spécifie **child::Customer**, le  **\<client >** éléments enfants du nœud de contexte sont sélectionnés. Étant donné que la **enfant** axe a  **\<élément >** comme type de nœud principal, le test de nœud, Customer, a la valeur TRUE si le client est un  **\<élément >** nœud.  
  
-   **Prédicats de sélection (zéro ou plus)**  
  
     Un prédicat permet de filtrer un élément node-set par rapport à un axe. La définition de prédicats de sélection dans une expression XPath équivaut à spécifier une clause WHERE dans une instruction SELECT. Le prédicat est spécifié entre crochets. L'application du test spécifié dans les prédicats de sélection permet de filtrer les nœuds retournés par le test de nœud. Pour chaque nœud de l'élément node-set à filtrer, l'expression de prédicat est évaluée avec ce nœud en tant que nœud de contexte et avec le nombre de nœuds de l'élément node-set en tant que taille de contexte. Si l'expression de prédicat prend la valeur TRUE pour ce nœud, ce dernier est inclus dans l'élément node-set obtenu.  
  
     La syntaxe d'une étape d'emplacement se compose du nom de l'axe et du test de nœud séparé par deux signes deux-points (::), suivis d'aucune ou plusieurs expressions, chacune entre crochets. Par exemple, l’expression XPath (chemin d’accès d’emplacement) **child::Customer [@CustomerID= 'ALFKI']** sélectionne tous les  **\<client >** éléments enfants du nœud de contexte. Le test dans le prédicat est alors appliqué à l’élément node-set, qui retourne uniquement les  **\<client >** valeur de nœuds d’élément avec l’attribut 'ALFKI' pour son **CustomerID** attribut.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Spécification d’un axe &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/location-path/specifying-an-axis-sqlxml-4-0.md)  
 Fournit des exemples de spécification d'un axe.  
  
 [Spécification d’un Test de nœud dans le chemin d’accès d’emplacement &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/location-path/specifying-a-node-test-in-the-location-path-sqlxml-4-0.md)  
 Fournit des exemples de spécification d'un test de nœud.  
  
 [Spécification de sélection de prédicats dans le chemin d’accès de l’emplacement &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/location-path/specifying-selection-predicates-in-the-location-path-sqlxml-4-0.md)  
 Fournit des exemples de spécification de prédicats de sélection.  
  
  
