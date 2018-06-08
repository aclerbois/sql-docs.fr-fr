---
title: RESTORE DATABASE (Parallel Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d915bfc1-e392-4a3a-9d94-08682cf3c864
caps.latest.revision: 8
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ed7e6aeb0630a20ee39d512fc17dfe24040737f2
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="restore-database-parallel-data-warehouse"></a>RESTORE DATABASE (Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

  Restaure une base de données utilisateur [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] à partir d’une sauvegarde de base de données dans une appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. La base de données est restaurée à partir d’une sauvegarde créée précédemment par la commande [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] [BACKUP DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/backup-database-parallel-data-warehouse.md). Les opérations de sauvegarde et de restauration vous permettent d’établir un plan de récupération d’urgence ou de déplacer des bases de données d’une appliance vers une autre.  
  
> [!NOTE]  
>  La restauration de la base de données MASTER inclut la restauration des informations de connexion d’appliance. Pour restaurer la base de données MASTER, accédez à la page [Restaurer la base de données MASTER &#40;Transact-SQL&#41; ](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md) dans l’outil **Gestionnaire de configuration**. Un administrateur ayant accès au nœud de contrôle peut effectuer cette opération.  
  
 Pour plus d’informations sur les sauvegardes de bases de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez la section relative à la sauvegarde et à la restauration dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
      Restore the master database  
-- Use the Configuration Manager tool.  
  
Restore a full user database backup.  
RESTORE DATABASE database_name   
    FROM DISK = '\\UNC_path\full_backup_directory'  
[;]  
  
Restore a full user database backup and then a differential backup.  
RESTORE DATABASE database_name  
    FROM DISK = '\\UNC_path\differential_backup_directory'   
    WITH [ ( ] BASE = '\\UNC_path\full_backup_directory' [ ) ]   
[;]  
  
Restore header information for a full or differential user database backup.  
RESTORE HEADERONLY   
    FROM DISK = '\\UNC_path\backup_directory'  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 RESTORE DATABASE *database_name*  
 Spécifie la restauration d’une base de données utilisateur dans une base de données appelée *database_name*. La base de données restaurée peut avoir un nom différent du nom de la base de données source qui a été sauvegardée. *database_name* ne peut pas déjà exister comme base de données dans l’appliance de destination. Pour plus d’informations sur les noms de base de données autorisés, consultez les règles de nommage d’objets dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 La restauration d’une base de données utilisateur a pour effet de restaurer une sauvegarde complète de base de données et éventuellement de restaurer une sauvegarde différentielle dans l’appliance. La restauration d’une base de données utilisateur inclut la restauration des utilisateurs de la base de données, ainsi que les rôles de base de données.  
  
 FROM DISK = '\\\\*UNC_path*\\*backup_directory*'  
 Chemin réseau et répertoire à partir duquel [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] restaure les fichiers de sauvegarde. Par exemple, FROM DISK = « \\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup» .  
  
 *backup_directory*  
 Spécifie le nom d’un répertoire qui contient la sauvegarde complète ou différentielle. Par exemple, vous pouvez effectuer une opération RESTORE HEADERONLY sur une sauvegarde complète ou différentielle.  
  
 *full_backup_directory*  
 Spécifie le nom d’un répertoire qui contient la sauvegarde complète.  
  
 *differential_backup_directory*  
 Spécifie le nom du répertoire qui contient la sauvegarde différentielle.  
  
-   Le chemin et le répertoire de sauvegarde doivent déjà exister et être spécifiés comme chemin UNC (Universal Naming Convention).  
  
-   Le chemin du répertoire de sauvegarde ne peut pas être un chemin local ni un emplacement sur un nœud d’appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
-   La longueur maximale du chemin UNC et du nom du répertoire de sauvegarde est de 200 caractères.  
  
-   Le serveur ou l’hôte doivent être spécifiés comme une adresse IP.  
  
 RESTORE HEADERONLY  
 Spécifie le retour des seules informations d’en-tête d’une sauvegarde de base de données utilisateur unique. L’en-tête comprend différents champs, notamment le texte descriptif de la sauvegarde et le nom de la sauvegarde. Le nom de la sauvegarde ne doit pas nécessairement être le même que celui du répertoire où sont stockés les fichiers de sauvegarde.  
  
 Les résultats de RESTORE HEADERONLY sont modélisés d’après les résultats de RESTORE HEADERONLY de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les résultats comportent plus de 50 colonnes, qui ne sont pas toutes utilisées par [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Pour une description des colonnes présentes dans les résultats de RESTORE HEADERONLY de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **CREATE ANY DATABASE**.  
  
 Nécessite un compte Windows doté d’un droit d’accès et de lecture à partir du répertoire de sauvegarde. Vous devez aussi stocker le nom de compte et le mot de passe Windows dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
1.  Pour vérifier si les informations d’identification s’y trouvent déjà, utilisez [sys.dm_pdw_network_credentials &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).  
  
2.  Pour ajouter ou mettre à jour les informations d’identification, utilisez [sp_pdw_add_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).  
  
3.  Pour supprimer les informations d’identification de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez [sp_pdw_remove_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).  
  
## <a name="error-handling"></a>Gestion des erreurs  
 La commande RESTORE DATABASE génère des erreurs dans les cas suivants :  
  
-   Le nom de la base de données à restaurer existe déjà dans l’appliance cible. Pour éviter cela, choisissez un nom de base de données unique ou supprimez la base de données existante avant d’exécuter la restauration.  
  
-   Le répertoire de sauvegarde contient un ensemble de fichiers de sauvegarde non valide.  
  
-   Les autorisations de connexion ne sont pas suffisantes pour restaurer une base de données.  
  
-   [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne dispose pas des autorisations appropriées pour accéder à l’emplacement réseau où se trouvent les fichiers de sauvegarde.  
  
-   L’emplacement réseau du répertoire de sauvegarde n’existe pas ou n’est pas disponible.  
  
-   Les nœuds de calcul ou le nœud de contrôle n’ont pas suffisamment d’espace disque. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne confirme pas que l’appliance dispose d’une quantité d’espace disque suffisante avant le lancement de la restauration. De ce fait, il est possible qu’une erreur d’espace disque insuffisant soit générée pendant l’exécution de l’instruction RESTORE DATABASE. Quand l’espace disque est insuffisant, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] annule la restauration.  
  
-   L’appliance cible dans laquelle la base de données est restaurée compte moins de nœuds de calcul que l’appliance source à partir de laquelle la base de données a été sauvegardée.  
  
-   La tentative de restauration de la base de données a pour cadre une transaction.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] suit la bonne réalisation des restaurations de base de données. Avant de restaurer une sauvegarde différentielle de base de données, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] vérifie que la restauration complète de base de données a abouti correctement.  
  
 Après une restauration, la base de données utilisateur présente le niveau de compatibilité de base de données 120. Cela vaut pour toutes les bases de données, quel que soit leur niveau de compatibilité d’origine.  
  
 **Restauration dans une appliance dotée d’un plus grand nombre de nœuds de calcul**  
Exécutez [DBCC SHRINKLOG (Azure SQL Data Warehouse)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) après avoir restauré une base de données d’une appliance vers une autre de plus grande taille, car la redistribution fera croître le journal des transactions.  

Le fait de restaurer une sauvegarde dans une appliance dotée d’un plus grand nombre de nœuds de calcul a pour effet d’accroître la taille de la base de données allouée de façon proportionnelle au nombre de nœuds de calcul.  
  
Par exemple, si vous restaurez une base de données de 60 Go d’une appliance à 2 nœuds (30 Go par nœud) vers une appliance à 6 nœuds, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée une base de données de 180 Go (6 nœuds à raison de 30 Go par nœud) dans l’appliance à 6 nœuds. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] commence par restaurer la base de données sur 2 nœuds pour être en adéquation avec la configuration source, puis redistribue les données aux 6 nœuds.  
  
 À l’issue de la redistribution, chaque nœud de calcul contient moins de données réelles et plus d’espace libre que chaque nœud de calcul de l’appliance source de plus petite taille. Profitez de l’espace supplémentaire pour ajouter davantage de données à la base de données. Si la taille de la base de données restaurée est plus grande qu’il ne faut, vous pouvez utiliser [ALTER DATABASE &#40;Parallel Data Warehouse&#41; ](../../t-sql/statements/alter-database-parallel-data-warehouse.md) pour réduire la taille des fichiers de base de données.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Dans le cadre de ces limitations et restrictions, l’appliance source est celle à partir de laquelle la sauvegarde de base de données a été créée, tandis que l’appliance cible est celle sur laquelle la base de données est restaurée.  
  
 À l’occasion d’une restauration de base de données, les statistiques ne sont pas régénérées automatiquement.  
  
 Seule une instruction RESTORE DATABASE ou BACKUP DATABASE peut s’exécuter dans l’appliance à un moment donné. Si plusieurs instructions de sauvegarde et de restauration sont envoyées simultanément, l’appliance les met en file d’attente et les traite l’une après l’autre.  
  
 Vous ne pouvez restaurer une sauvegarde de base de données que dans une appliance cible [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] qui compte au moins le même nombre de nœuds de calcul que l’appliance source. L’appliance cible ne peut pas contenir moins de nœuds de calcul que l’appliance source.  
  
 Une sauvegarde qui a été créée dans une appliance basée sur du matériel SQL Server 2012 PDW ne pas être restaurée dans une appliance basée sur du matériel SQL Server 2008 R2. Cela est vrai même si l’appliance a été acquise à l’origine avec du matériel SQL Server 2008 R2 PDW et exécute maintenant un logiciel SQL Server 2012 PDW.  
  
## <a name="locking"></a>Verrouillage  
 Applique un verrou exclusif sur l’objet DATABASE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-restore-examples"></a>A. Exemples simples RESTORE  
 L’exemple suivant restaure une sauvegarde complète dans la base de données `SalesInvoices2013`. Les fichiers de sauvegarde sont stockés dans le répertoire \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full. La base de données SalesInvoices2013 ne doit pas déjà se trouver dans l’appliance cible, car cette commande échouerait avec une erreur.  
  
```  
RESTORE DATABASE SalesInvoices2013  
FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';  
```  
  
### <a name="b-restore-a-full-and-differential-backup"></a>B. Restaurer une sauvegarde complète et différentielle  
 L’exemple suivant restaure une sauvegarde complète, puis une sauvegarde différentielle dans la base de données SalesInvoices2013.  
  
 La sauvegarde complète de la base de données est restaurée à partir de la sauvegarde complète, qui est stockée dans le répertoire « \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full ». Si la restauration aboutit, la sauvegarde différentielle est restaurée dans la base de données SalesInvoices2013.  La sauvegarde différentielle est stockée dans le répertoire « \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff ».  
  
```  
RESTORE DATABASE SalesInvoices2013  
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'  
    WITH BASE = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'  
[;]  
  
```  
  
### <a name="c-restoring-the-backup-header"></a>C. Restauration de l’en-tête de sauvegarde  
 Cet exemple restaure les informations d’en-tête pour la sauvegarde de base de données « \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full ». La commande génère une ligne d’informations pour la sauvegarde Invoices2013Full.  
  
```  
RESTORE HEADERONLY  
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'  
[;]  
```  
  
 Vous pouvez vous servir des informations d’en-tête pour vérifier le contenu d’une sauvegarde ou pour vous assurer que l’appliance de restauration cible est compatible avec l’appliance de sauvegarde source avant d’entreprendre la restauration de la sauvegarde.  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/backup-database-parallel-data-warehouse.md)  
  
  
