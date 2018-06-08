---
title: Désactiver la gestion de sauvegarde de SQL Server sur Microsoft Azure | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3e02187f-363f-4e69-a82f-583953592544
caps.latest.revision: 8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 1117eaebd9e4ed5c44077966bc9b3d2874548248
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="disable-sql-server-managed-backup-to-microsoft-azure"></a>Désactiver la gestion de sauvegarde de SQL Server sur Microsoft Azure
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment activer ou suspendre [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] au niveau base de données et instance.  
  
##  <a name="DatabaseDisable"></a> Désactiver la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour une base de données  
 Vous pouvez désactiver les paramètres de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] à l’aide de la procédure stockée système, [managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md). Le paramètre *@enable_backup* sert à activer et désactiver les configurations de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour une base de données spécifique ; la valeur 1 active les paramètres de configuration, et la valeur 0 les désactive.  
  
#### <a name="to-disable-includesssmartbackupincludesss-smartbackup-mdmd-for-a-specific-database"></a>Pour désactiver la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour une base de données spécifique :  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
```  
EXEC msdb.managed_backup.sp_backup_config_basic  
                @database_name = 'TestDB'   
                ,@enable_backup = 0;  
GO  
  
```  
  
##  <a name="DatabaseAllDisable"></a> Désactiver la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour toutes les bases de données sur l'instance  
 La procédure suivante désactive les paramètres de configuration de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sur toutes les bases de données où la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est actuellement activée sur l'instance.  Les paramètres de configuration tels que l’URL de stockage, la rétention et les informations d’identification SQL restent dans les métadonnées et peuvent être utilisés si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée ultérieurement pour la base de données. Si vous souhaitez simplement interrompre temporairement les services de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , vous pouvez utiliser le commutateur principal abordé dans les sections suivantes de cette rubrique.  
  
#### <a name="to-disable-includesssmartbackupincludesss-smartbackup-mdmd-for-all-the-databases"></a>Pour désactiver la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour toutes les bases de données :  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L’exemple suivant détermine si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est configurée au niveau de l’instance et identifie toutes les bases de données activées pour la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sur l’instance, puis exécute la procédure stockée système **sp_backup_config_basic** pour désactiver la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
```  
-- Create a working table to store the database names  
Declare @DBNames TABLE  
  
       (  
             RowID int IDENTITY PRIMARY KEY  
             ,DBName varchar(500)  
  
       )  
-- Define the variables  
DECLARE @rowid int  
DECLARE @dbname varchar(500)  
DECLARE @SQL varchar(2000)  
-- Get the database names from the system function  
  
INSERT INTO @DBNames (DBName)  
  
SELECT db_name  
       FROM   
  
       msdb.managed_backup.fn_backup_db_config (NULL)  
       WHERE is_managed_backup_enabled = 1 
       AND is_dropped = 0
  
       --Select DBName from @DBNames  
  
       select @rowid = min(RowID)  
       FROM @DBNames  
  
       WHILE @rowID IS NOT NULL  
       Begin  
  
             Set @dbname = (Select DBName From @DBNames Where RowID = @rowid)  
             Begin  
             Set @SQL = 'EXEC msdb.managed_backup.sp_backup_config_basic    
                @database_name= '''+'' + @dbname+ ''+''',   
                @enable_backup=0'  
  
            EXECUTE (@SQL)  
  
             END  
             Select @rowid = min(RowID)  
             From @DBNames Where RowID > @rowid  
  
       END  
```  
  
 Pour passer en revue les paramètres de configuration de toutes les bases de données sur l'instance, utilisez la requête suivante :  
  
```  
Use msdb;  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL);  
GO  
  
```  
  
##  <a name="InstanceDisable"></a> Désactiver les paramètres de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] par défaut pour l'instance  
 Les paramètres par défaut au niveau de l'instance sont appliqués à toutes les nouvelles bases de données créées sur cette instance.  Si vous n’avez plus besoin des paramètres par défaut, vous pouvez désactiver cette configuration à l’aide de la procédure stockée système **managed_backup.sp_backup_config_basic** , en affectant la valeur Null au paramètre *@database_name* . La désactivation ne supprime pas les autres paramètres de configuration, comme l'URL de stockage, le paramètre de rétention ou le nom de l'objet contenant les informations d'identification SQL. Ces paramètres seront utilisés si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée sur l'instance ultérieurement.  
  
#### <a name="to-disable-includesssmartbackupincludesss-smartbackup-mdmd-default-configuration-settings"></a>Pour désactiver les paramètres de configuration par défaut de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] :  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    EXEC msdb.managed_backup.sp_backup_config_basic  
                    @database_name = 'TestDB'   
                    ,@enable_backup = 0;  
    GO  
  
    ```  
  
##  <a name="InstancePause"></a> Interrompre la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] au niveau de l'instance  
 Dans certains cas, vous pouvez souhaiter interrompre les services de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour une courte période.  La procédure stockée système **managed_backup.sp_backup_master_switch** vous permet de désactiver le service de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] au niveau de l’instance.  La même procédure est utilisée pour reprendre la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Le paramètre @state est utilisé pour déterminer si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] doit être désactivée ou activée.  
  
#### <a name="to-pause-includesssmartbackupincludesss-smartbackup-mdmd-services-using-transact-sql"></a>Pour interrompre les services de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] à l'aide de Transact-SQL :  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go  
  
```  
  
#### <a name="to-resume-includesssmartbackupincludesss-smartbackup-mdmd-using-transact-sql"></a>Pour reprendre la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] à l'aide de Transact-SQL  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
```  
Use msdb;  
Go  
EXEC managed_backup. sp_backup_master_switch @new_state=1;  
GO  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Activation de la sauvegarde managée SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)  
  
  
