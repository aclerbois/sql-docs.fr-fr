---
title: Supprimer le témoin d’une session de mise en miroir de bases de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.component: database-mirroring
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], turning off
- witness [SQL Server], removing
- database mirroring [SQL Server], witness
ms.assetid: f3ce7afc-8936-4d35-80ce-d0f8fbc318d3
caps.latest.revision: 39
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ae033cc8ab9717d119ccee3f2d10ad9f4d1daefd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="remove-the-witness-from-a-database-mirroring-session-sql-server"></a>Supprimer le témoin d'une session de mise en miroir de bases de données (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment supprimer un témoin depuis une session de mise en miroir de bases de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. À tout moment au cours d'une session de mise en miroir d'une base de données, le propriétaire de cette dernière peut désactiver le témoin pour cette session.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer le témoin, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir supprimé le témoin](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-remove-the-witness"></a>Pour supprimer le témoin  
  
1.  Connectez-vous à l'instance du serveur principal puis, dans le volet **Explorateur d'objets** , cliquez sur le nom du serveur pour développer l'arborescence.  
  
2.  Développez **Bases de données**, puis sélectionnez la base de données dont vous souhaitez supprimer le témoin.  
  
3.  Cliquez avec le bouton droit sur la base de données, sélectionnez **Tâches**, puis cliquez sur **Miroir**. La page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** s'affiche.  
  
4.  Pour supprimer le témoin, supprimez son adresse réseau de serveur du champ **Témoin** .  
  
    > [!NOTE]  
    >  Si vous passez du mode de sécurité élevée avec basculement automatique au mode haute performance, le champ **Témoin** est automatiquement supprimé.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-remove-the-witness"></a>Pour supprimer le témoin  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur l'une des instances de serveur partenaire.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Émettez l'instruction suivante :  
  
     [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md) *nom_base_de_données* SET WITNESS OFF  
  
     où *nom_base_de_données* est le nom de la base de données en miroir.  
  
     L'exemple suivant supprime le témoin de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET WITNESS OFF ;  
    ```  
  
##  <a name="FollowUp"></a> Suivi : après avoir supprimé le témoin  
 La désactivation du témoin modifie le [mode d’opération](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)conformément au paramètre de sécurité des transactions :  
  
-   Si la sécurité des transactions a la valeur FULL (valeur par défaut), la session utilise le mode synchrone haute sécurité sans basculement automatique.  
  
-   Si la sécurité des transactions a la valeur OFF (désactivée), la session agit de manière asynchrone (en mode hautes performances) sans nécessiter de quorum. Lorsque la sécurité des transactions est désactivée, il est vivement recommandé de désactiver également le témoin.  
  
> [!TIP]  
>  Le paramètre de sécurité des transactions de la base de données est enregistré pour chaque serveur partenaire dans l’affichage catalogue [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) des colonnes **mirroring_safety_level** et **mirroring_safety_level_desc** .  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Ajouter un témoin de mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)  
  
-   [Ajouter ou remplacer un témoin de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en miroir de bases de données ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Modifier la sécurité des transactions dans une session de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/change-transaction-safety-in-a-database-mirroring-session-transact-sql.md)   
 [Ajouter un témoin de mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/add-a-database-mirroring-witness-using-windows-authentication-transact-sql.md)   
 [Témoin de mise en miroir de base de données](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
