---
title: BACKUP DATABASE (Parallel Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 73c8d465-b36b-4727-b9f3-368e98677c64
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1dfce92ca664b934f5dec7085934eb4997df8e56
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="backup-database-parallel-data-warehouse"></a>BACKUP DATABASE (Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

  Crée une sauvegarde d’une base de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] et stocke la sauvegarde de l’appliance dans un emplacement réseau spécifié par l’utilisateur. Utilisez cette instruction avec [RESTORE DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/restore-database-parallel-data-warehouse.md) pour la récupération d’urgence, ou pour copier une base de données d’une appliance vers une autre.  
  
 **Avant de commencer**, lisez la section relative à l’acquisition et à la configuration d’un serveur de sauvegarde dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 Deux types de sauvegardes sont possibles dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Une *sauvegarde complète de base de données* correspond à la sauvegarde de l’intégralité d’une base de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Une *sauvegarde différentielle de base de données* contient uniquement les modifications effectuées depuis la dernière sauvegarde complète. Une sauvegarde de base de données utilisateur comprend les utilisateurs de la base de données, ainsi que ses rôles. La sauvegarde de la base de données MASTER comprend les connexions.  
  
 Pour plus d’informations sur les sauvegardes de bases de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez la section relative à la sauvegarde et à la restauration dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
      Create a full backup of a user database or the master database.  
BACKUP DATABASE database_name  
    TO DISK = '\\UNC_path\backup_directory'  
    [ WITH [ ( ]  <with_options> [ ,...n ]  [ ) ] ]  
[;]  
  
Create a differential backup of a user database.  
BACKUP DATABASE database_name  
    TO DISK = '\\UNC_path\backup_directory'  
    WITH [ ( ] DIFFERENTIAL   
    [ , <with_options> [ ,...n ] [ ) ]  
[;]  
  
<with_options> ::=  
    DESCRIPTION = 'text'  
    | NAME = 'backup_name'  
  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données pour laquelle créer une sauvegarde. Il peut s’agir de la base de données MASTER ou d’une base de données utilisateur.  
  
 TO DISK = '\\\\*UNC_path*\\*backup_directory*'  
 Chemin réseau et répertoire dans lesquels [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] doit écrire les fichiers de sauvegarde. Par exemple, « \\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup ».  
  
-   Le chemin où se trouve le nom du répertoire de sauvegarde doit déjà exister et être spécifié comme un chemin UNC complet.  
  
-   Le répertoire de sauvegarde (*backup_directory*) ne doit pas exister avant l’exécution de la commande de sauvegarde. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée le répertoire de sauvegarde.  
  
-   Le chemin du répertoire de sauvegarde ne peut pas être un chemin local ni un emplacement sur un nœud d’appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
-   La longueur maximale du chemin UNC et du nom du répertoire de sauvegarde est de 200 caractères.  
  
-   Le serveur ou l’hôte doivent être spécifiés comme une adresse IP.  Vous ne pouvez pas le spécifier comme le nom de l’hôte ou du serveur.  
  
 DESCRIPTION = **'***text***'**  
 Spécifie une description textuelle de la sauvegarde. La longueur maximale du texte est de 255 caractères.  
  
 La description est stockée dans les métadonnées et s’affiche lorsque l’en-tête de sauvegarde est restauré avec RESTORE HEADERONLY.  
  
 NAME = **'***backup _name***'**  
 Spécifie le nom de la sauvegarde. Le nom de la sauvegarde peut être différent de celui de la base de données.  
  
-   Les noms peuvent contenir jusqu'à 128 caractères.  
  
-   Ne peut pas inclure un chemin.  
  
-   Doit commencer par une lettre, un numéro ou un trait de soulignement (_). Les caractères spéciaux autorisés sont le trait de soulignement (\_), le trait d’union (-) ou l’espace ( ). Les noms de sauvegarde ne peuvent pas se terminer par un espace.  
  
-   L’instruction échoue si *backup_name* existe déjà à l’emplacement spécifié.  
  
 Le nom est stocké dans les métadonnées et s’affiche lorsque l’en-tête de sauvegarde est restauré avec RESTORE HEADERONLY.  
  
 DIFFERENTIAL  
 Spécifie l’exécution d’une sauvegarde différentielle pour une base de données utilisateur. Si vous ne spécifiez pas de valeur, une sauvegarde complète est exécutée par défaut. Le nom de la sauvegarde différentielle ne doit pas nécessairement être le même que celui de la sauvegarde complète. Pour effectuer le suivi de la sauvegarde différentielle et de la sauvegarde complète correspondante, il est conseillé d’utiliser le même nom suivi de « complète » et de « diff ».  
  
 Exemple :  
  
 `BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerFull';`  
  
 `BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerDiff' WITH DIFFERENTIAL;`  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **BACKUP DATABASE** ou l’appartenance au rôle de base de données fixe **db_backupoperator**. La base de données MASTER peut uniquement être sauvegardée par un utilisateur standard ayant reçu le rôle de base de données fixe **db_backupoperator**. La base de données MASTER peut uniquement être sauvegardée par un **administrateur système**, un administrateur d’infrastructure ou un membre du rôle serveur fixe **sysadmin**.  
  
 Nécessite un compte Windows doté d’un droit d’accès, de création et d’écriture sur le répertoire de sauvegarde. Vous devez aussi stocker le nom de compte et le mot de passe Windows dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Pour ajouter ces informations d’identification réseau à [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez la procédure stockée [sp_pdw_add_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).  
  
 Pour plus d’informations sur la gestion des informations d’identification dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez la section [Sécurité](#Security).  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Des erreurs BACKUP DATABASE se produisent dans les conditions suivantes :  
  
-   Les autorisations de l’utilisateur ne sont pas suffisantes pour effectuer une sauvegarde.  
  
-   [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne dispose pas des autorisations nécessaires pour accéder à l’emplacement réseau où est stockée la sauvegarde.  
  
-   La base de données n’existe pas.  
  
-   Le répertoire cible existe déjà sur le partage réseau.  
  
-   Le partage réseau cible n’est pas disponible.  
  
-   Le partage réseau cible n’a pas suffisamment d’espace pour la sauvegarde. La commande BACKUP DATABASE ne vérifie pas la présence d’un espace disque suffisant avant de lancer la sauvegarde, ce qui peut entraîner une erreur d’espace disque insuffisant lors de l’exécution de BACKUP DATABASE. Quand l’espace disque est insuffisant, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] annule la commande BACKUP DATABASE. Pour réduire la taille de votre base de données, exécutez [DBCC SHRINKLOG (Azure SQL Data Warehouse)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md).  
  
-   Un lancement de sauvegarde a été tenté dans une transaction.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Avant d’effectuer une sauvegarde de base de données, utilisez [DBCC SHRINKLOG (Azure SQL Data Warehouse)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) pour réduire la taille de votre base de données. 
 
 Une sauvegarde [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] est stockée sous la forme d’un ensemble de fichiers regroupés dans un même répertoire.  
  
 Généralement, une sauvegarde différentielle prend moins de temps qu’une sauvegarde complète et peut être effectuée plus souvent. Quand plusieurs sauvegardes différentielles sont basées sur une même sauvegarde complète, chaque sauvegarde différentielle inclut l’ensemble des modifications de la dernière sauvegarde différentielle.  
  
 Si vous annulez une commande BACKUP, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] supprime le répertoire cible et tous les fichiers créés pour la sauvegarde. Si [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] perd la connexion au partage réseau, l’annulation ne peut pas être effectuée.  
  
 Les sauvegardes complètes et les sauvegardes différentielles sont stockées dans des répertoires différents. Les conventions de nommage ne sont pas appliquées lorsque vous associez une sauvegarde complète et une sauvegarde différentielle. Vous pouvez effectuer ce suivi à l’aide de vos propres conventions de nommage. Vous pouvez également effectuer ce suivi à l’aide de l’option WITH DESCRIPTION pour ajouter une description, puis à l’aide de l’instruction RESTORE HEADERONLY pour récupérer la description.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Vous ne pouvez pas effectuer de sauvegarde différentielle pour la base de données MASTER. Seules les sauvegardes complètes sont possibles avec les bases de données MASTER.  
  
 Les fichiers de sauvegarde sont stockés dans un format qui est adapté uniquement à la restauration de la sauvegarde vers une appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] à l’aide de l’instruction [RESTORE DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/restore-database-parallel-data-warehouse.md).  
  
 La sauvegarde réalisée à l’aide de l’instruction BACKUP DATABASE ne peut pas être utilisée pour transférer des données ou des informations utilisateur vers des bases de données SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour cela, vous pouvez utiliser la fonctionnalité de copie de la table distante. Pour plus d’informations, consultez la section relative à la copie de données à partir d’une table distante dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] utilise la technologie de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour sauvegarder et restaurer des bases de données. Les options de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont préconfigurées pour utiliser la compression de sauvegarde. Vous ne pouvez pas définir des options de sauvegarde comme la compression, la somme de contrôle, la taille des blocs ou le nombre de tampons.  
  
 Vous ne pouvez exécuter qu’une seule sauvegarde ou restauration de base de données à la fois sur une même appliance. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] met en file d’attente les commandes BACKUP et RESTORE tant que la commande BACKUP ou RESTORE en cours n’est pas terminée.  
  
 L’appliance cible pour la restauration de la sauvegarde doit comprendre au moins autant de nœuds de calcul que l’appliance source. L’appliance cible peut avoir plus de nœuds de calcul que l’appliance source, mais pas moins.  
  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] n’effectue pas le suivi de l’emplacement et des noms des sauvegardes, puisque celles-ci ne sont pas stockées sur l’appliance.  
  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] effectue le suivi des sauvegardes de base de données ayant réussi et ayant échoué.  
  
 Une sauvegarde différentielle est uniquement autorisée si la dernière sauvegarde complète a réussi. Par exemple, supposons que lundi vous ayez créé une sauvegarde complète de la base de données Ventes et que la sauvegarde ait réussi. Supposons ensuite que mardi, vous ayez créé une sauvegarde complète de la base de données Ventes et que celle-ci ait échoué. Après cet échec, vous ne pourrez pas créer une sauvegarde différentielle basée sur la sauvegarde complète de lundi. Pour créer une sauvegarde différentielle, une sauvegarde complète réussie est nécessaire.  
  
## <a name="metadata"></a>Métadonnées  
 Ces vues de gestion dynamique contiennent des informations sur toutes les opérations de sauvegarde, de restauration et de chargement. Ces informations sont conservées après le redémarrage du système.  
  
-   [sys.pdw_loader_backup_runs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md)  
  
-   [sys.pdw_loader_backup_run_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql.md)  
  
-   [sys.pdw_loader_run_stages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-loader-run-stages-transact-sql.md)  
  
## <a name="performance"></a>Performances  
 Pour effectuer une sauvegarde, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] sauvegarde d’abord les métadonnées, puis effectue une sauvegarde parallèle des données de base de données qui sont stockées dans les nœuds de calcul. Les données sont copiées de chaque nœud de calcul vers le répertoire de sauvegarde. Pour obtenir de meilleures performances quand vous déplacez des données des nœuds de calcul vers le répertoire de sauvegarde, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] permet de contrôler le nombre de nœuds de calcul qui copient des données simultanément.  
  
## <a name="locking"></a>Verrouillage  
 Applique un verrou ExclusiveUpdate sur l’objet DATABASE.  
  
##  <a name="Security"></a> Sécurité  
 Les sauvegardes [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne sont pas stockées sur l’appliance. Il revient donc à votre équipe informatique de gérer tous les aspects liés à la sécurité de la sauvegarde. Cela comprend, par exemple, la gestion de la sécurité des données de sauvegarde, de la sécurité du serveur utilisé pour stocker les sauvegardes et de la sécurité de l’infrastructure réseau qui connecte le serveur de sauvegarde à l’appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 **Gérer les informations d’identification réseau**  
  
 L’accès réseau au répertoire de sauvegarde est basé sur la sécurité standard des partages de fichiers Windows. Avant d’effectuer une sauvegarde, vous devez créer ou désigner un compte Windows qui sera utilisé pour l’authentification de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] auprès du répertoire de sauvegarde. Ce compte Windows doit être doté d’un droit d’accès, de création et d’écriture sur le répertoire de sauvegarde.  
  
> [!IMPORTANT]  
>  Pour réduire les risques de sécurité liés à vos données, il vous est conseillé de désigner un compte Windows qui servira uniquement aux opérations de sauvegarde et de restauration. Autorisez ce compte à accéder à l’emplacement de sauvegarde et à aucun autre emplacement.  
  
 Vous devez stocker le nom d’utilisateur et le mot de passe dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] en exécutant la procédure stockée [sp_pdw_add_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md). [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] utilise le Gestionnaire d’informations d’identification Windows pour stocker et chiffrer les noms d’utilisateur et les mots de passe sur le nœud de contrôle et les nœuds de calcul. Les informations d’identification ne sont pas sauvegardées avec la commande BACKUP DATABASE.  
  
 Pour supprimer les informations d’identification réseau de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez [sp_pdw_remove_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).  
  
 Pour répertorier toutes les informations d’identification réseau stockées dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez la vue de gestion dynamique [sys.dm_pdw_network_credentials &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-add-network-credentials-for-the-backup-location"></a>A. Ajouter des informations d’identification réseau pour l’emplacement de sauvegarde  
 Pour créer une sauvegarde, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] doit disposer d’autorisations de lecture/écriture pour le répertoire de sauvegarde. L’exemple suivant montre comment ajouter les informations d’identification d’un utilisateur. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] va stocker ces informations d’identification et les utiliser pour les opérations de sauvegarde et de restauration.  
  
> [!IMPORTANT]  
>  Pour des raisons de sécurité, nous vous recommandons de créer un compte de domaine qui servira uniquement pour les sauvegardes.  
  
```  
EXEC sp_pdw_add_network_credentials 'xxx.xxx.xxx.xxx', 'domain1\backupuser', '*****';  
```  
  
### <a name="b-remove-network-credentials-for-the-backup-location"></a>B. Supprimer les informations d’identification réseau pour l’emplacement de sauvegarde  
 L’exemple suivant montre comment supprimer de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] les informations d’identification d’un utilisateur de domaine.  
  
```  
EXEC sp_pdw_remove_network_credentials 'xxx.xxx.xxx.xxx';  
```  
  
### <a name="c-create-a-full-backup-of-a-user-database"></a>C. Créer une sauvegarde complète d’une base de données utilisateur  
 L’exemple suivant crée une sauvegarde complète de la base de données utilisateur Invoices. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée le répertoire Invoices2013 et enregistre les fichiers de sauvegarde dans le répertoire \\\10.192.63.147\backups\yearly\Invoices2013Full.  
  
```  
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';  
```  
  
### <a name="d-create-a-differential-backup-of-a-user-database"></a>D. Créer une sauvegarde différentielle d’une base de données utilisateur  
 L’exemple suivant crée une sauvegarde différentielle, qui inclut toutes les modifications apportées depuis la dernière sauvegarde complète de la base de données Invoices. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée le répertoire \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff dans lequel il stocke les fichiers. La description « Invoices 2013 differential backup » est stockée avec les informations d’en-tête de la sauvegarde.  
  
 La sauvegarde différentielle ne peut être effectuée que si la dernière sauvegarde complète d’Invoices a réussi.  
  
```  
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'  
    WITH DIFFERENTIAL,   
    DESCRIPTION = 'Invoices 2013 differential backup';  
```  
  
### <a name="e-create-a-full-backup-of-the-master-database"></a>E. Créer une sauvegarde complète de la base de données MASTER  
 L’exemple suivant crée une sauvegarde complète de la base de données MASTER et la stocke dans le répertoire « \\\10.192.63.147\backups\2013\daily\20130722\master ».  
  
```  
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master';  
```  
  
### <a name="f-create-a-backup-of-appliance-login-information"></a>F. Créer une sauvegarde des informations de connexion de l’appliance  
 La base de données MASTER stocke les informations de connexion de l’appliance. Pour sauvegarder les informations de connexion de l’appliance, vous devez effectuer une sauvegarde de la base de données MASTER.  
  
 L’exemple suivant crée une sauvegarde complète de la base de données MASTER.  
  
```  
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master'  
WITH (   
    DESCRIPTION = 'Master Backup 20130722',  
    NAME = 'login-backup'  
)  
;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [RESTORE DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/restore-database-parallel-data-warehouse.md)  
  
  
