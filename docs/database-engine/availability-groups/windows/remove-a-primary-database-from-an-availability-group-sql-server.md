---
title: Supprimer une base de données primaire d’un groupe de disponibilité (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.removeprimarydb.f1
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 6d4ca31e-ddf0-44bf-be5e-a5da060bf096
caps.latest.revision: 28
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: a7855aedfc5b2b9c40a60e7ee3deb77fe6468ebf
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770255"
---
# <a name="remove-a-primary-database-from-an-availability-group-sql-server"></a>Supprimer une base de données primaire d'un groupe de disponibilité (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment supprimer la base de données primaire et la ou les bases de données secondaires correspondantes d’un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
-   **Avant de commencer :**  
  
     [Conditions préalables requises et restrictions](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer une base de données de disponibilité, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
-   **Suivi :**  [Après la suppression d'une base de données de disponibilité dans un groupe de disponibilité](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables requises et restrictions  
  
-   Cette tâche est prise en charge uniquement sur les réplicas principaux. Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour supprimer une base de données de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal de la ou des bases de données à supprimer et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Sélectionnez le groupe de disponibilité, puis développez le nœud **Bases de données de disponibilité** .  
  
4.  Cette étape varie selon que vous souhaitez supprimer plusieurs groupes de bases de données ou une seule base de données, comme suit :  
  
    -   Pour supprimer plusieurs bases de données, utilisez le volet **Détails de l'Explorateur d'objets** pour afficher et sélectionner toutes les bases de données que vous souhaitez supprimer. Pour plus d’informations, consultez [Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Pour supprimer une seule base de données, sélectionnez-la dans le volet **Explorateur d'objets** ou le volet **Détails de l'Explorateur d'objets** .  
  
5.  Cliquez avec le bouton droit sur la ou les bases de données sélectionnées, puis sélectionnez **Supprimer la base de données du groupe de disponibilité** dans le menu de commande.  
  
6.  Dans la boîte de dialogue **Supprimer les bases de données du groupe de disponibilité** , pour supprimer toutes les bases de données répertoriées, cliquez sur **OK**. Si vous ne souhaitez pas toutes les supprimer, cliquez sur **Annuler**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour supprimer une base de données de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe* REMOVE DATABASE *nom_base_de_données_de_disponibilité*  
  
     où *nom_groupe* est le nom du groupe de disponibilité et *nom_base_de_données* est le nom de la base de données à supprimer.  
  
     L'exemple suivant supprime une bases de données nommée `Db6` du groupe de disponibilité `MyAG` .  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG REMOVE DATABASE Db6;  
    ```  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour supprimer une base de données de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Remove-SqlAvailabilityDatabase** , en spécifiant le nom de la base de données de disponibilité à supprimer du groupe de disponibilité. Lorsque vous êtes connecté à l'instance de serveur qui héberge le réplica principal, la base de données primaire et ses bases de données secondaires correspondantes sont toutes supprimées du groupe de disponibilité.  
  
     Par exemple, la commande suivante supprime la base de données de disponibilité `MyDb9` du groupe de disponibilité nommé `MyAg`. Dans la mesure où la commande est exécutée sur l'instance de serveur qui héberge le réplica principal, la base de données primaire et toutes ses bases de données secondaires correspondantes sont supprimées du groupe de disponibilité. La synchronisation de données ne se produira plus pour cette base de données sur aucun réplica secondaire.  
  
    ```  
    Remove-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\Sql\PrimaryComputer\InstanceName\AvailabilityGroups\MyAg\AvailabilityDatabases\MyDb9
    ```  
  
    > [!NOTE]  
    >  Pour afficher la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../relational-databases/scripting/get-help-sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
##  <a name="FollowUp"></a> Suivi : Après la suppression d'une base de données de disponibilité dans un groupe de disponibilité  
 La suppression d'une base de données de disponibilité de son groupe de disponibilité met fin à la synchronisation des données entre l'ancienne base de données primaire et les bases de données secondaires correspondantes. L'ancienne base de données primaire reste en ligne. Chaque base de données secondaire correspondante est placée dans l'état RESTORING.  
  
 À ce stade, il existe d'autres méthodes pour traiter une base de données secondaire supprimée :  
  
-   Si vous n'avez plus besoin d'une base de données secondaire particulière, vous pouvez la supprimer.  
  
     Pour plus d’informations, consultez [Supprimer une base de données](../../../relational-databases/databases/delete-a-database.md).  
  
-   Si vous souhaitez accéder à une base de données secondaire supprimée après sa suppression du groupe de disponibilité, vous pouvez récupérer la base de données. Toutefois, si vous récupérez une base de données secondaire supprimée, deux bases de données divergentes distinctes portant le même nom se trouvent alors en ligne. Vous devez vous assurer que les clients ne peuvent accéder qu'à une seule d'entre elles, généralement la base de données primaire la plus récente.  
  
     Pour plus d’informations, consultez [Récupérer une base de données sans restauration des données &#40;Transact-SQL&#41;](../../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Supprimer une base de données secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
  
