---
title: Détacher une base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.detachdatabase.f1
helpviewer_keywords:
- database detaching [SQL Server]
- detaching databases [SQL Server]
ms.assetid: f63d4107-13e4-4bfe-922d-5e4f712e472d
caps.latest.revision: 36
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1d3636faa07e65f61faa08561c82a58530187f1d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="detach-a-database"></a>Détacher une base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment détacher une base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les fichiers détachés restent et peuvent être rattachés à l'aide de CREATE DATABASE avec l'option FOR ATTACH ou FOR ATTACH_REBUILD_LOG. Vous pouvez les déplacer sur un autre serveur et les y attacher.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour détacher une base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
 Pour obtenir la liste des limitations et restrictions, consultez [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-detach-a-database"></a>Pour détacher une base de données  
  
1.  Dans l'Explorateur d'objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , connectez-vous à une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez cette dernière.  
  
2.  Développez **Bases de données**, puis sélectionnez le nom de la base de données utilisateur que vous souhaitez détacher.  
  
3.  Cliquez avec le bouton droit sur le nom de la base de données, pointez sur **Tâches**, puis cliquez sur **Détacher**. La boîte de dialogue **Détacher la base de données** apparaît.  
  
     **Bases de données à détacher**  
     Répertorie les bases de données à détacher.  
  
     **Database Name**  
     Spécifie le nom de la base de données à détacher.  
  
     **Supprimer les connexions**  
     Permet de déconnecter les connexions à la base de données spécifiée.  
  
    > [!NOTE]  
    >  Vous ne pouvez pas détacher une base de données avec des connexions actives.  
  
     **Mettre à jour les statistiques**  
     Par défaut, l'opération de détachement conserve toutes les statistiques d'optimisation obsolètes avant de procéder au détachement ; pour actualiser les statistiques existantes, activez cette case à cocher.  
  
     **Conserver les catalogues de texte intégral**  
     Par défaut, l'opération de détachement conserve tous les catalogues de texte intégral associés à la base de données. Pour les supprimer, décochez la case **Conserver les catalogues de texte intégral** . Cette option s'affiche uniquement lors de la mise à niveau d'une base de données à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
     **État**  
     Affiche l’un des états suivants : **Prêt** ou **Non prêt**.  
  
     **Message**  
     La colonne **Message** peut indiquer des informations sur la base de données, comme suit :  
  
    -   Lorsqu'une base de données est impliquée dans la réplication, l' **État** est **Non prêt** et la colonne **Message** indique **Base de données répliquée**.  
  
    -   Quand une base de données a une ou plusieurs connexions actives, l’**État** est **Non prêt** et la colonne **Message** indique *<nombre_de_connexions_actives>***Connexion(s) active(s)*, par exemple, **1 connexion(s) active(s)**. Avant de détacher la base de données, vous devez déconnecter toutes les connexions actives en cliquant sur **Supprimer les connexions**.  
  
     Pour obtenir plus d'informations sur un message, cliquez sur le texte du lien hypertexte pour ouvrir le Moniteur d'activité.  
  
4.  Lorsque vous avez prêt à lancer le processus, cliquez sur **OK**.  
  
> [!NOTE]  
>  La base de données ainsi détachée reste toujours visible dans le nœud **Bases de données** de l'explorateur d'objets jusqu'à ce que la vue soit actualisée. Actualisez la vue à tout moment en cliquant dans le volet de l'explorateur d'objets et en sélectionnant, dans la barre de menus, les éléments **Affichage** puis **Actualiser**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-detach-a-database"></a>Pour détacher une base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple détache la base de données AdventureWorks2012 avec l'option skipchecks définie sur la valeur True.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
  
