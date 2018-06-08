---
title: Les curseurs dynamiques ODBC | Documents Microsoft
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
- cursors [ODBC], dynamic
- dynamic cursors [ODBC]
ms.assetid: de709fd3-9eb2-44e1-a2f0-786e2b9602a6
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 706bf3a92122e025977d092c06668fc646f2b6e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-dynamic-cursors"></a>Curseurs dynamiques ODBC
Un curseur dynamique est rien de plus : dynamique. Il peut détecter les modifications apportées à l’appartenance, order et les valeurs du jeu de résultats après l’ouverture du curseur. Par exemple, un curseur dynamique extrait deux lignes et une autre application, puis met à jour l’une de ces lignes et supprime l’autre. Si le curseur dynamique tente d’extraire ces lignes, elle ne trouvera pas la ligne supprimée mais retournera les nouvelles valeurs de la ligne mise à jour.  
  
 Les curseurs dynamiques détectent toutes les mises à jour, supprime et insère les deux leur propre et celles effectuées par d’autres. (Ceci est soumis à l’isolation au niveau de la transaction, tel que défini par l’attribut de connexion SQL_ATTR_TXN_ISOLATION.) Le tableau d’état de ligne spécifié par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR reflète ces modifications et peut contenir SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO, SQL_ROW_ERROR, SQL_ROW_UPDATED et SQL_ROW_ADDED. Il ne peut pas retourner SQL_ROW_DELETED, car un curseur dynamique ne retourne pas de lignes supprimées à l’extérieur de l’ensemble de lignes et par conséquent ne reconnaît pas l’existence de la ligne supprimée du jeu de résultats ou son élément correspondant dans le tableau d’état de ligne. SQL_ROW_ADDED est retourné uniquement lorsqu’une ligne est mise à jour par un appel à **SQLSetPos**, pas lorsqu’il est mis à jour par un autre curseur.  
  
 Un moyen d’implémenter des curseurs dynamiques dans la base de données est en créant un index sélectif qui définit l’appartenance et de classement du résultat de la définir. Étant donné que l’index est mis à jour lorsque d’autres apportent des modifications, un curseur basé sur un tel index est sensible à toutes les modifications. Sélection supplémentaire dans le jeu de résultats défini par cet index est possible par le traitement le long de l’index.  
  
 Les curseurs dynamiques peuvent être simulés en exigeant que le jeu de résultats pour être classés par une clé unique. Avec une restriction de ce type, les extractions sont effectuées en exécutant un **sélectionnez** instruction chaque fois que le curseur extrait des lignes. Par exemple, supposons que le jeu de résultats est défini par cette instruction :  
  
```  
SELECT * FROM Customers ORDER BY Name, CustID  
```  
  
 Pour extraire l’ensemble de lignes suivant dans le jeu de résultats, le curseur simulé définit les paramètres dans l’exemple suivant **sélectionnez** instruction aux valeurs dans la dernière ligne de l’ensemble de lignes en cours, puis l’exécute :  
  
```  
SELECT * FROM Customers WHERE (Name > ?) AND (CustID > ?)  
   ORDER BY Name, CustID  
```  
  
 Cette instruction crée un deuxième jeu de résultats, le premier ensemble de lignes qui est l’ensemble de lignes suivant dans le jeu de résultats d’origine : dans ce cas, l’ensemble de lignes dans la table Customers. Le curseur retourne cet ensemble de lignes à l’application.  
  
 Il est intéressant de noter qu’un curseur dynamique est implémenté de cette manière crée plusieurs jeux de résultats, qui permet de détecter des modifications au jeu de résultats d’origine. L’application jamais apprend l’existence de ces jeux de résultats auxiliaire ; Il s’affiche simplement comme si le curseur est en mesure de détecter les modifications apportées à l’ensemble de résultats d’origine.
