---
title: Limitations d’instruction INSERT | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC SQL grammar, INSERT statement limitations
- INSERT statement limitations [ODBC]
- truncation of data [ODBC]
ms.assetid: dea05698-527a-41ab-8729-bbed85556185
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f15f7c8a45593b86f50ac4da3dc254dca507aae7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="insert-statement-limitations"></a>Insérez l’instruction
Les données insérées sont tronquées à droite sans avertissement si elle est trop longue pour tenir dans la colonne.  
  
 Tentative d’insertion d’une valeur qui est en dehors de la plage du type de données d’une colonne entraîne une valeur NULL à insérer dans la colonne.  
  
 Lorsqu’un fichier dBASE, Microsoft Excel, Paradox ou Textdriver est utilisé, l’insertion d’une chaîne de longueur zéro dans une colonne insère en fait une valeur NULL à la place.  
  
 Lorsque le pilote Microsoft Excel est utilisé, si une chaîne vide est insérée dans une colonne, une chaîne vide est convertie en une valeur NULL ; une instruction SELECT recherche qui est exécutée avec une chaîne vide dans la clause WHERE ne réussira pas sur cette colonne.  
  
 Une table n’est pas mis à jour par le pilote Paradox sous deux conditions :  
  
-   Quand un index unique n’est pas défini sur la table. Cela n’est pas vrai pour une table vide, ce qui peut être mis à jour avec une seule ligne même si un index unique n’est pas défini sur la table. Si une seule ligne est insérée dans une table vide qui ne dispose pas d’un index unique, une application ne peut pas créer un index unique ou insérer des données supplémentaires après l’insertion de la ligne.  
  
-   Si le moteur de base de données Borland n’est pas implémenté, uniquement lire et ajoutez les instructions sont autorisées sur la table de Corel.  
  
 Lorsque le pilote de texte est utilisé, les valeurs NULL sont représentées par une chaîne blancs dans les fichiers de longueur fixe, mais sont représentées par aucune des espaces dans les fichiers délimités. Par exemple, dans la ligne suivante qui contient trois champs, le deuxième champ est une valeur NULL :  
  
```  
"Smith:,, 123  
```  
  
 Lorsque le pilote de texte est utilisé, toutes les valeurs de colonne peuvent être remplis avec des espaces de début. La longueur de n’importe quelle ligne doit être inférieur ou égal à 65,543 octets.
