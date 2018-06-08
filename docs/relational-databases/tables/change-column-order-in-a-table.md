---
title: Modifier l’ordre des colonnes dans une table | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], change order in a table
- column order, change
ms.assetid: cd99ef56-9085-431a-a0fc-58e7add5399f
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d011463176762dfae5f0ff417b5122ebdca7777c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-column-order-in-a-table"></a>Modifier l'ordre des colonnes dans une table
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Vous pouvez changer l'ordre des colonnes dans le Concepteur de tables dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!CAUTION]  
>  Changer l'ordre des colonnes d'une table peut avoir une incidence sur le code et les applications qui dépendent de l'ordre spécifique des colonnes. Il peut s'agir de requêtes, de vues, de procédures stockées, de fonctions définies par l'utilisateur et d'applications clientes. Prenez toutes les précautions nécessaires avant de changer l'ordre des colonnes. La recommandation est de spécifier l'ordre dans lequel les colonnes sont renvoyées au niveau de l'application et de la requête. Vous ne pouvez pas compter sur l'utilisation de SELECT * pour retourner toutes les colonnes dans une commande prévue d'après l'ordre dans lequel elles sont définies dans la table. Spécifiez toujours le nom des colonnes dans vos requêtes et applications dans l'ordre dans lequel vous souhaitez qu'elles apparaissent.  
  
 **Dans cette rubrique**  
  
-   **Pour changer l'ordre des colonnes à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-change-the-column-order"></a>Pour changer l'ordre des colonnes  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table dont vous souhaitez réorganiser les colonnes et cliquez sur **Conception**.  
  
2.  Sélectionnez la case à gauche du nom de la colonne à réordonner.  
  
3.  Faites glisser la colonne vers un autre emplacement dans la table.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour changer l'ordre des colonnes**  
  
 Cette tâche ne peut pas être effectuée à l'aide d'instructions Transact-SQL.  
  
###  <a name="TsqlExample"></a>  
