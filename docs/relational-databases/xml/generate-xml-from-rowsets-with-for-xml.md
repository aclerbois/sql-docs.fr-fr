---
title: Générer du code XML à partir d’ensembles de lignes avec FOR XML | Microsoft Docs
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
- FOR XML clause, generating XML from rowsets
ms.assetid: d061c0f1-3de9-4ad1-bbca-ce45d064b6c8
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6ebe3216692f8a42bdb805c01bbc20f5255f78c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-xml-from-rowsets-with-for-xml"></a>Générer du code XML à partir d'ensembles de lignes avec FOR XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Vous pouvez générer une instance de type **xml** à partir d’un ensemble de lignes en utilisant FOR XML avec la nouvelle directive **TYPE** .  
  
 Le résultat peut être assigné à une colonne, une variable ou un paramètre de type **xml** . De plus, il est possible d'imbriquer des clauses FOR XML pour générer une structure hiérarchique. Les clauses FOR XML imbriquées sont plus faciles à écrire que la clause FOR XML EXPLICIT, mais elles ne s'avèrent pas aussi performantes pour les hiérarchies profondes. FOR XML introduit aussi un nouveau mode PATH qui spécifie le chemin de l'arborescence XML où apparaît la valeur d'une colonne.  
  
 La nouvelle directive **FOR XML TYPE** permet de créer, avec une syntaxe SQL, des vues XML en lecture seule des données relationnelles. La vue peut être interrogée par des instructions SQL et le langage XQuery intégré, comme le montre l'exemple suivant. Vous pouvez également faire référence à ces vues SQL dans les procédures stockées.  
  
## <a name="example-sql-view-returning-generated-xml-data-type"></a>Exemple : vue SQL renvoyant un type de données XML généré  
 La définition de la vue SQL suivante crée une vue XML d'une colonne relationnelle, pk, et extrait les auteurs des livres d'une colonne XML :  
  
```  
CREATE VIEW V (xmlVal) AS  
SELECT pk, xCol.query('/book/author')  
FROM   T  
FOR XML AUTO, TYPE  
```  
  
 La vue V contient une seule ligne avec une seule colonne columnxmlVal de type XML`.` Elle peut faire l’objet d’une requête comme tout autre instance standard de type **xml** . Par exemple, la requête suivante renvoie l'auteur dont le prénom est « David » :  
  
```  
SELECT xmlVal.query('//author[first-name = "David"]')  
FROM   V  
```  
  
 Les définitions de vue SQL s'apparentent un peu aux vues XML créées à l'aide des schémas annotés, bien qu'il y ait des différences de taille. La définition d'une vue SQL est en lecture seule et doit être manipulée avec le langage XQuery intégré. Les vues XML sont créées à l'aide d'un schéma annoté. De plus, la vue SQL matérialise le résultat XML avant l'application de l'expression XQuery tandis que les requêtes XPath sur les vues XML évaluent les requêtes SQL portant sur les tables sous-jacentes.  
  
## <a name="see-also"></a> Voir aussi  
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
