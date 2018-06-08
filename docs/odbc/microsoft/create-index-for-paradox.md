---
title: CRÉER des INDEX pour Paradox | Documents Microsoft
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
- CREATE INDEX [ODBC]
- Paradox driver [ODBC], create index
ms.assetid: 6472bd69-b931-4bc2-a9bf-f1873ed4cdfe
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 473c8d73e33504e7cc22d3b02aaca789d9c7a620
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-index-for-paradox"></a>CRÉER des INDEX pour Paradox
La syntaxe de l’instruction CREATE INDEX pour le pilote ODBC Paradox est :  
  
 **CRÉER** [**UNIQUE**] **INDEX** *-nom de l’index*  
  
 **ON** *-nom de la table*  
  
 **(** *identificateur de la colonne* [**ASC**]  
  
 [**,** *identificateur de la colonne* [**ASC**]...] **)**  
  
 Le pilote ODBC Paradox ne prend pas en charge la **DESC** mot clé dans la grammaire SQL ODBC pour l’instruction CREATE INDEX. Le *-nom de la table* argument peut spécifier le chemin d’accès complet de la table.  
  
 Si le mot clé **UNIQUE** est spécifié, le pilote ODBC Paradox créera un index unique. Le premier index unique est créé en tant qu’un index principal. Il s’agit d’un fichier de clé primaire Paradox est nommé *-nom de la table*. PX. Index primaires sont soumis aux restrictions suivantes :  
  
-   L’index principal doit être créé avant que toutes les lignes sont ajoutées à la table.  
  
-   Un index principal doit être défini sur les premières colonnes « n » dans une table.  
  
-   Un seul index primaire est autorisé par table.  
  
-   Une table ne peut pas être mis à jour par le pilote Paradox si un index principal n’est pas défini sur la table. (Notez que cela n’est pas vrai pour une table vide, ce qui peut être mise à jour même si un index unique n’est pas défini sur la table).  
  
-   Le *-nom de l’index* argument pour un index principal doit être le même que le nom de base de la table, comme requis par Paradox.  
  
 Si le mot clé **UNIQUE** est omis, le pilote ODBC Paradox créera un index non unique. Cela se compose de deux fichiers d’index secondaire Paradox nommés *-nom de la table*. X*nn* et *-nom de la table*. Y*nn*, où *nn* est le numéro de la colonne dans la table. Index non uniques sont soumis aux restrictions suivantes :  
  
-   Avant de pouvoir créer un index non unique pour une table, un index principal doit exister pour que la table.  
  
-   Pour Paradox 3. *x*, le *-nom de l’index* argument pour un index autre que d’un index primaire (unique ou non unique) doit être le même que le nom de colonne. Pour Paradox 4. *x* et 5. *x*, le nom de cet index peut être, mais ne doit pas nécessairement être le même que le nom de colonne.  
  
-   Une seule colonne peut être spécifiée pour un index non unique.  
  
 Impossible d’ajouter les colonnes une fois qu’un index a été défini sur une table. Si la première colonne de la liste d’arguments d’une instruction CREATE TABLE crée un index, une deuxième colonne ne peut pas être incluse dans la liste d’arguments.  
  
 Par exemple, pour utiliser le numéro de commande et les colonnes de numéro de ligne en tant que l’index unique sur la table SO_LINES, utilisez l’instruction :  
  
```  
CREATE UNIQUE INDEX SO_LINES  
 ON SO_LINES (SONum, LineNum)  
```  
  
 Pour utiliser la colonne de numéro de partie comme un index non unique sur la table SO_LINES, utilisez l’instruction :  
  
```  
CREATE INDEX PartNum  
 ON SO_LINES (PartNum)  
```  
  
 Notez que lorsque les deux instructions CREATE INDEX sont effectuées, la première instruction crée toujours un index primaire portant le même nom que la table et la deuxième instruction créera toujours un index non unique avec le même nom que la colonne. Ces index est nommés ainsi même si des noms différents sont entrés dans les instructions CREATE INDEX et que l’index porte le nom UNIQUE dans la deuxième instruction CREATE INDEX.  
  
> [!NOTE]  
>  Lorsque vous utilisez le pilote Paradox sans implémenter le moteur de base de données Borland, en lecture seule et ajoutez les instructions sont autorisées.
