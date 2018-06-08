---
title: Données retournées par les fonctions de catalogue | Documents Microsoft
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
- catalog functions [ODBC], result sets
- functions [ODBC], catalog functions
ms.assetid: 399e1a64-8766-4c44-81ff-445399b7a1de
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d27d395913ce64d263798205521a3d1136460105
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-returned-by-catalog-functions"></a>Données retournées par les fonctions de catalogue
Chaque fonction de catalogue retourne des données dans un jeu de résultats. Ce jeu de résultats n’est pas différent de tout autre ensemble de résultats. Il est généralement généré par prédéfini, paramétrables **sélectionnez** instruction qui est codé en dur dans le pilote ou stocké dans une procédure dans la source de données. Pour plus d’informations sur la façon de récupérer des données à partir d’un jeu de résultats, consultez [a un résultat défini créé ?](../../../odbc/reference/develop-app/was-a-result-set-created.md).  
  
 Le jeu de résultats pour chaque fonction de catalogue sont décrite dans l’entrée de référence pour cette fonction. Outre les colonnes répertoriées, le jeu de résultats peut contenir des colonnes spécifiques aux pilotes après la dernière colonne prédéfinie. Ces colonnes (le cas échéant) sont décrits dans la documentation du pilote.  
  
 Les applications doivent lier les colonnes spécifiques au pilote par rapport à la fin du jeu de résultats. Autrement dit, ils doivent calculer le nombre d’une colonne spécifique au pilote que le nombre de la dernière colonne — récupérés avec **SQLNumResultCols** , moins le nombre de colonnes qui se produisent après la colonne requise. Cela permet de gagner à la modification de l’application lorsque de nouvelles colonnes sont ajoutées au résultat défini dans les futures versions d’ODBC ou le pilote. Pour ce schéma fonctionne, les pilotes doivent ajouter de nouvelles colonnes de spécifiques au pilote avant les anciennes colonnes spécifiques au pilote afin que les numéros de colonne ne modifiez pas par rapport à la fin du jeu de résultats.  
  
 Les identificateurs qui sont retournées dans le jeu de résultats ne sont pas cités, même s’ils contiennent des caractères spéciaux. Par exemple, supposons que l’identificateur de guillemet (qui est spécifique au pilote et retournés via **SQLGetInfo**) est un guillemet double (") et de la table Fournisseurs contient une colonne nommée Customer Name. Dans la ligne retournée par **SQLColumns** pour cette colonne, la valeur de la colonne de la table TABLE_NAME est paies, pas « comptabilité », et la valeur de la colonne COLUMN_NAME n’est pas « Customer Name », nom du client. Pour récupérer les noms des clients dans la table fournisseurs, l’application serait devis ces noms :  
  
```  
SELECT "Customer Name" FROM "Accounts Payable"  
```  
  
 Pour plus d’informations, consultez [identificateurs entre guillemets](../../../odbc/reference/develop-app/quoted-identifiers.md).  
  
 Les fonctions de catalogue sont basées sur un modèle d’autorisation de type SQL dans laquelle une connexion est effectuée en fonction d’un nom d’utilisateur et un mot de passe, et uniquement les données pour laquelle l’utilisateur a un privilège qui sont retournées. Mot de passe de fichiers individuels, qui ne tient pas dans ce modèle, est définie par le pilote.  
  
 Les jeux de résultats retournés par les fonctions de catalogue sont presque jamais être mise à jour, et les applications ne doivent pas s’attendre à être en mesure de modifier la structure de la base de données en modifiant les données dans ces jeux de résultats.
