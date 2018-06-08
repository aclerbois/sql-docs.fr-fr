---
title: 'Exemple : configurer la mise en miroir de base de données à l’aide de l’authentification Windows (T-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.component: database-mirroring
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- Windows authentication [SQL Server]
- authentication [SQL Server], database mirroring
- database mirroring [SQL Server], security
ms.assetid: 35800769-aede-4aac-b077-0e0e487e302f
caps.latest.revision: 41
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: bafd280f2154a4163bfedaf26596d9667660a7d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="example-setting-up-database-mirroring-using-windows-authentication-transact-sql"></a>Exemple : configurer la mise en miroir de bases de données à l'aide de l'authentification Windows (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cet exemple illustre toutes les étapes nécessaires à la création d'une session de mise en miroir de base de données avec un serveur témoin à l'aide de l'authentification Windows. Les exemples de cette rubrique utilisent [!INCLUDE[tsql](../../includes/tsql-md.md)]. Notez qu'au lieu d'utiliser les étapes [!INCLUDE[tsql](../../includes/tsql-md.md)], vous pouvez utiliser l'Assistant Sécurité de mise en miroir de bases de données pour configurer la mise en miroir de base de données. Pour plus d’informations, consultez [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
## <a name="prerequisite"></a>Condition préalable  
 L'exemple utilise la base de données d'exemple **AdventureWorks** , qui emploie par défaut le mode de récupération simple. Pour utiliser la mise en miroir avec cette base de données, vous devez la modifier pour qu'elle utilise le mode de restauration complète. Pour obtenir ce résultat dans [!INCLUDE[tsql](../../includes/tsql-md.md)], utilisez l'instruction ALTER DATABASE comme suit :  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks   
SET RECOVERY FULL;  
GO  
```  
  
 Pour plus d’informations sur la modification du modèle de récupération dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur la base de données et l'autorisation CREATE ENDPOINT, ou l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="example"></a> Exemple  
 Dans cet exemple, les deux partenaires et le témoin sont les instances de serveur par défaut réparties sur trois systèmes informatiques. Les trois instances de serveur utilisent le même domaine Windows, mais le compte d'utilisateur (utilisé comme compte de service au démarrage) est différent pour l'instance du témoin de l'exemple.  
  
 Le tableau ci-dessous récapitule les valeurs utilisées dans cet exemple.  
  
|Rôle initial de la mise en miroir|Système hôte|Compte d'utilisateur de domaine|  
|----------------------------|-----------------|-------------------------|  
|Principal|PARTNERHOST1|*\<Mon_domaine>\\<nom_utilisateur_dbo\>*|  
|Miroir|PARTNERHOST5|*\<Mon_domaine>\\<nom_utilisateur_dbo\>*|  
|Témoin|WITNESSHOST4|*\<Un_domaine>\\<utilisateur_témoin\>*|  
  
1.  Créez un point de terminaison dans l'instance du serveur principal (instance par défaut sur PARTNERHOST1).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=PARTNER)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    -- Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
2.  Créez un point de terminaison dans l'instance du serveur miroir (instance par défaut sur PARTNERHOST5).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL)  
    GO  
    --Partners under same domain user; login already exists in master.  
    --Create a login for the witness server instance,  
    --which is running as Somedomain\witnessuser:  
    USE master ;  
    GO  
    CREATE LOGIN [Somedomain\witnessuser] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of witness.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Somedomain\witnessuser];  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
3.  Créez un point de terminaison dans l'instance du serveur témoin (instance par défaut sur WITNESSHOST4).  
  
    ```  
    CREATE ENDPOINT Endpoint_Mirroring  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=WITNESS)  
    GO  
    --Create a login for the partner server instances,  
    --which are both running as Mydomain\dbousername:  
    USE master ;  
    GO  
    CREATE LOGIN [Mydomain\dbousername] FROM WINDOWS ;  
    GO  
    --Grant connect permissions on endpoint to login account of partners.  
    GRANT CONNECT ON ENDPOINT::Endpoint_Mirroring TO [Mydomain\dbousername];  
    GO  
    ```  
  
4.  Création de la base de données miroir Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
5.  Dans l'instance du serveur miroir sur PARTNERHOST5, définissez l'instance de serveur sur PARTNERHOST1 comme partenaire (ce qui en fait l'instance initiale du serveur principal).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER =   
        'TCP://PARTNERHOST1.COM:7022'  
    GO  
    ```  
  
6.  Dans l'instance du serveur principal sur PARTNERHOST1, définissez l'instance de serveur sur PARTNERHOST5 comme partenaire (ce qui en fait l'instance initiale du serveur miroir).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET PARTNER = 'TCP://PARTNERHOST5.COM:7022'  
    GO  
    ```  
  
7.  Sur le serveur principal, définissez le témoin (qui se trouve sur WITNESSHOST4).  
  
    ```  
    ALTER DATABASE AdventureWorks   
        SET WITNESS =   
        'TCP://WITNESSHOST4.COM:7022'  
    GO  
    ```  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
-   [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Exemple : configuration de la mise en miroir de bases de données à l’aide de certificats &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-certificates-transact-sql.md)  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Centre de sécurité pour le moteur de base de données SQL Server et la base de données SQL Azure](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
