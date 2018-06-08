---
title: Supprimer des tables (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- table deletions [SQL Server]
- deleting tables
- removing tables
- dropping tables
ms.assetid: ca6aa3e9-9885-44c3-bafc-aec441fd97ec
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: dc3a8fe6fe7577a0256490cbd412333aeaa1d37a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="delete-tables-database-engine"></a>Supprimer des tables (moteur de base de données)
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Vous pouvez supprimer une table de votre base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!CAUTION]  
>  Ne supprimez une table qu'après mûre réflexion. En effet, s'il existe des requêtes, des vues, des fonctions définies par l'utilisateur, des procédures stockées ou des programmes qui font référence à cette table, la suppression rend tous ces objets non valides.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer une table à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez pas supprimer une table qui est référencée par une contrainte FOREIGN KEY. Vous devez au préalable supprimer la contrainte FOREIGN KEY ou la table qui la référence. Si la table de référence et la table qui contient la clé primaire sont supprimées dans la même instruction DROP TABLE, la table de référence doit figurer en premier dans la liste.  
  
-   Lorsqu'une table est supprimée, les règles et les valeurs par défaut liées à celle-ci sont dissociées et toutes les contraintes et les déclencheurs qui lui sont associés sont automatiquement supprimés. Si vous recréez la table, vous devez réassocier les règles et valeurs par défaut appropriées, recréer les déclencheurs et ajouter les toutes les contraintes nécessaires.  
  
-   Si vous supprimez une table qui contient une colonne **varbinary (max)** avec l’attribut FILESTREAM, aucune donnée stockée dans le système de fichiers n’est supprimée.  
  
-   DROP TABLE et CREATE TABLE ne doivent pas être exécutés sur la même table dans le même lot. Sinon, une erreur inattendue risque de se produire.  
  
-   Toute vue ou procédure stockée faisant référence à la table supprimée doit être supprimée ou modifiée explicitement pour supprimer la référence à la table.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l’autorisation ALTER sur le schéma auquel appartient la table, l’autorisation CONTROL sur la table ou l’appartenance au rôle de base de données fixe **db_ddladmin** .  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-delete-a-table-from-the-database"></a>Pour supprimer une table de la base de données  
  
1.  Dans l'Explorateur d'objets, sélectionnez la table à supprimer.  
  
2.  Cliquez avec le bouton droit sur la table puis, dans le menu contextuel, cliquez sur **Supprimer** .  
  
3.  Un message vous demande de confirmer la suppression. Cliquez sur **Oui**.  
  
    > [!NOTE]  
    >  La suppression d'une table entraîne automatiquement celle de toutes les relations qu'elle entretient.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-delete-a-table-in-query-editor"></a>Pour supprimer une table dans l'éditeur de requête  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    DROP TABLE dbo.PurchaseOrderDetail;  
  
    ```  
  
 Pour plus d’informations, consultez [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md).  
  
  
