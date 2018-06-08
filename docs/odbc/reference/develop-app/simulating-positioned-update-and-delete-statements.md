---
title: Simulation positionné instructions Update et Delete | Documents Microsoft
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
- positioned deletes [ODBC]
- data updates [ODBC], positioned update or delete
- row identifiers [ODBC]
- positioned updates [ODBC]
- updating data [ODBC], positioned update or delete
ms.assetid: b24ed59f-f25b-4646-a135-5f3596abc1a4
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1448481938ff0ef8e20ba4e6a85801b65024cbcc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="simulating-positioned-update-and-delete-statements"></a>Simulation d’instructions Delete et mise à jour positionnée
Si la source de données ne prend en charge la mise à jour positionnée et supprimer des instructions, le pilote peut simuler ces. Par exemple, la bibliothèque de curseurs ODBC simule la mise à jour positionnée et supprimer les instructions. La stratégie générale pour simuler des instructions de suppression et de mise à jour positionnée consiste à convertir des instructions positionnées à ceux recherchés. Cela est effectué en remplaçant le **WHERE CURRENT OF** clause avec une recherche **où** clause qui identifie la ligne actuelle.  
  
 Par exemple, étant donné que la colonne CustID identifie de façon unique chaque ligne dans la table Customers, positionnée de l’instruction delete  
  
```  
DELETE FROM Customers WHERE CURRENT OF CustCursor  
```  
  
 peut être converti en  
  
```  
DELETE FROM Customers WHERE (CustID = ?)  
```  
  
 Le pilote peut utiliser une de ces *d’identificateurs de lignes* dans les **où** clause :  
  
-   Colonnes dont les valeurs servent à identifier de façon unique chaque ligne dans la table. Par exemple, l’appel **SQLSpecialColumns** avec SQL_BEST_ROWID retourne la colonne ou jeu optimal de colonnes qui correspondent à cet effet.  
  
-   Pseudo-colonnes, fournies par certaines sources de données, à des fins d’identifiant de manière unique chaque ligne. Il peuvent également être récupérables en appelant **SQLSpecialColumns**.  
  
-   Un index unique, si elle est disponible.  
  
-   Toutes les colonnes du jeu de résultats.  
  
 Les colonnes exactement un pilote doit utiliser dans le **où** clause il construit dépend du pilote. Sur des données sources, la détermination d’un identificateur de ligne peuvent être coûteuses. Toutefois, il est plus rapide d’exécuter et garantit qu’une instruction simulée met à jour ou supprime une ligne au plus. Selon les fonctionnalités du SGBD sous-jacent, à l’aide d’un identificateur de ligne peut être coûteux à configurer. Toutefois, il est plus rapide d’exécuter et garantit qu’une instruction simulée sera mise à jour ou supprimer qu’une seule ligne. L’option de l’utilisation de toutes les colonnes dans le jeu de résultats est généralement beaucoup plus facile à configurer. Toutefois, il est plus lent à exécuter et, si les colonnes n’identifient pas une ligne, vous risquez de lignes en cours involontairement mis à jour ou supprimées, en particulier lorsque la liste de sélection pour le résultat de la valeur ne contient pas toutes les colonnes qui existent dans la table sous-jacente.  
  
 Laquelle des stratégies précédentes selon le pilote prend en charge, une application peut choisir quelle stratégie il veut le pilote à utiliser avec l’attribut d’instruction SQL_ATTR_SIMULATE_CURSOR. Il peut sembler étrange pour une application à courir le risque involontairement la mise à jour ou suppression d’une ligne, l’application peut supprimer ce risque en vous assurant que les colonnes du jeu de résultats identifient de façon unique chaque ligne du jeu de résultats. Cela permet d’économiser le pilote de l’effort d’avoir à procéder ainsi.  
  
 Si le pilote choisit d’utiliser un identificateur de ligne, elle intercepte le **sélectionner pour la mise à jour** instruction qui crée le jeu de résultats. Si les colonnes dans la liste select n’identifient pas efficacement une ligne, le pilote ajoute les colonnes nécessaires à la fin de la liste de sélection. Certaines sources de données ont une colonne unique qui identifie le toujours une ligne, telles que la colonne ROWID dans Oracle. Si une telle colonne n’est disponible, le pilote utilise ceci. Dans le cas contraire, le pilote appelle **SQLSpecialColumns** pour chaque table dans la **FROM** clause pour récupérer la liste des colonnes qui identifient de façon unique chaque ligne. Une restriction courants résultant de cette technique est que la simulation de curseur échoue s’il existe plusieurs tables dans le **FROM** clause.  
  
 Quelle que soit la façon dont le pilote identifie les lignes, il supprime généralement le **FOR UPDATE OF** clause hors tension le **sélectionner pour la mise à jour** instruction avant de l’envoyer à la source de données. Le **pour mettre à jour de** clause est utilisée uniquement avec positionné mise à jour et supprimer les instructions. Sources de données ne prenant pas en charge positionné mise à jour et les instructions delete généralement ne gèrent pas il.  
  
 Lorsque l’application envoie une mise à jour positionnée ou une instruction delete pour l’exécution, le pilote remplace le **WHERE CURRENT OF** clause avec un **où** clause contenant l’identificateur de ligne. Les valeurs de ces colonnes sont récupérées à partir d’un cache géré par le pilote pour chaque colonne qu’il utilise dans les **où** clause. Une fois que le pilote a remplacé le **où** clause, il envoie l’instruction à la source de données pour l’exécution.  
  
 Par exemple, supposons que l’application soumet l’instruction suivante pour créer un jeu de résultats :  
  
```  
SELECT Name, Address, Phone FROM Customers FOR UPDATE OF Phone, Address  
```  
  
 Si l’application a la valeur SQL_ATTR_SIMULATE_CURSOR pour demander une garantie d’unicité et si la source de données ne fournit pas une pseudo-colonne qui identifie le toujours une ligne, le pilote appelle **SQLSpecialColumns** pour la table Customers, découvre que CustID est la clé à la table Customers et ajoute à la liste de sélection et supprime la **FOR UPDATE OF** clause :  
  
```  
SELECT Name, Address, Phone, CustID FROM Customers  
```  
  
 Si l’application n’a pas demandé une garantie d’unicité, le pilote supprime uniquement le **FOR UPDATE OF** clause :  
  
```  
SELECT Name, Address, Phone FROM Customers  
```  
  
 Supposons que l’application fait défile le jeu de résultats et envoie l’instruction de mise à jour positionnée suivante pour l’exécution, où le client est le nom du curseur sur le jeu de résultats :  
  
```  
UPDATE Customers SET Address = ?, Phone = ? WHERE CURRENT OF Cust  
```  
  
 Si l’application n’a pas demandé une garantie d’unicité, le pilote remplace le **où** clause et lie le paramètre CustID à la variable dans son cache :  
  
```  
UPDATE Customers SET Address = ?, Phone = ? WHERE (CustID = ?)  
```  
  
 Si l’application n’a pas demandé une garantie d’unicité, le pilote remplace le **où** clause et lie les paramètres de nom, adresse et téléphone dans cette clause aux variables dans son cache :  
  
```  
UPDATE Customers SET Address = ?, Phone = ?  
   WHERE (Name = ?) AND (Address = ?) AND (Phone = ?)  
```
