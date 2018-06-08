---
title: Sauvegarder des fichiers et des groupes de fichiers (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backing up filegroups [SQL Server]
- file backups [SQL Server], how-to topics
- backing up files [SQL Server]
- backups [SQL Server], creating
- filegroups [SQL Server], backing up
ms.assetid: a0d3a567-7d8b-4cfe-a505-d197b9a51f70
caps.latest.revision: 41
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: aa791223469f92033b8613a1761ea7ac32994e5f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-files-and-filegroups-sql-server"></a>Sauvegarder des fichiers et des groupes de fichiers (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment sauvegarder des fichiers et des groupes de fichiers dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou de PowerShell. Lorsque la taille de la base de données et les exigences en matière de performances rendent impraticable une sauvegarde complète de la base de données, créez une sauvegarde de fichiers. Une *sauvegarde de fichiers* contient toutes les données dans un ou plusieurs fichiers (ou groupes de fichiers). Pour plus d’informations sur les sauvegardes de fichiers, consultez [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md) et [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  

  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   L'instruction BACKUP n'est pas autorisée dans une transaction explicite ou implicite.  
  
-   En mode de récupération simple, vous devez sauvegarder conjointement tous les fichiers en lecture-écriture. Cela permet de garantir la restauration de la base de données dans un état cohérent dans le temps. Plutôt que de spécifier individuellement chaque fichier ou groupe de fichier en lecture-écriture, utilisez l'option READ_WRITE_FILEGROUPS. Cette option sauvegarde tous les groupes de fichiers en lecture-écriture dans la base de données. Une sauvegarde créée en spécifiant READ_WRITE_FILEGROUPS est ce que l’on appelle une *sauvegarde partielle*. Pour plus d’informations, consultez [Sauvegardes partielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/partial-backups-sql-server.md).  
  
-   Pour plus d'informations sur les limitations et les restrictions, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Par défaut, chaque opération de sauvegarde réussie ajoute une entrée au journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et au journal des événements système. Si vous sauvegardez très fréquemment le journal, ces messages de réussite peuvent rapidement s'accumuler, créer des journaux d'erreurs très volumineux et compliquer la recherche d'autres messages. Dans de tels cas, vous pouvez supprimer ces entrées de journal en utilisant l'indicateur de trace 3226 si aucun de vos scripts ne dépend de ces entrées. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
 
##  <a name="Permissions"></a> Permissions  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.  
  
  
## <a name="back-up-files-and-filegroups-using-ssms"></a>Sauvegarder des groupes de fichiers et des fichiers à l’aide de SSMS   
  
1.  Après vous être connecté à l'instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**. La boîte de dialogue **Sauvegarder la base de données** s'affiche.  
  
4.  Dans la liste **Base de données** , vérifiez le nom de la base de données. Vous pouvez éventuellement sélectionner une autre base de données dans la liste.  
  
5.  Dans la liste **Type de sauvegarde** , sélectionnez **Complète** ou **Différentielle**.  
  
6.  Pour l'option **Composant de sauvegarde** , cliquez sur **Fichier et groupes de fichiers**.  
  
7.  Dans la boîte de dialogue **Sélection de fichiers et de groupes de fichiers** , sélectionnez les fichiers et les groupes de fichiers que vous voulez sauvegarder. Vous pouvez sélectionner un ou plusieurs fichiers individuellement, ou vous pouvez activer la case qui permet de sélectionner automatiquement tous les fichiers d'un groupe de fichiers.  
  
8.  Acceptez le nom du jeu de sauvegarde par défaut proposé dans la zone de texte **Nom** , ou attribuez-lui un autre nom.  
  
9. Dans la zone de texte **Description** , vous avez la possibilité de saisir une description du jeu de sauvegarde.  
  
10. Indiquez quand le jeu de sauvegarde arrivera à expiration :  
  
    -   Pour que le jeu de sauvegarde expire au bout d’un certain nombre de jours, cliquez sur **Après** (option par défaut). Ensuite, entrez le nombre de jours suivant la création du jeu où le jeu expirera. Cette valeur doit être comprise entre 0 et 99999 jours ; une valeur de 0 jour signifie que le jeu de sauvegarde n'expirera jamais.  
  
         La valeur par défaut est définie dans l’option **Délai de rétention par défaut du support de sauvegarde (jours)** de la boîte de dialogue **Propriétés du serveur** (page**Paramètres de base de données** ). Pour accéder à cette option, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets et sélectionnez les propriétés ; sélectionnez ensuite la page **Paramètres de base de données** .  
  
    -   Pour que le jeu de sauvegarde expire à une date spécifique, cliquez sur **Le**et entrez la date d'expiration souhaitée.  
  
11. Choisissez le type de destination de la sauvegarde : **Disque** ou **Bande**. Pour sélectionner les chemins d'accès de 64 lecteurs de bande ou disques (maximum) contenant un support de sauvegarde unique, cliquez sur **Ajouter**. Les chemins sélectionnés sont affichés dans la liste **Sauvegarde sur** .  
  
    >**REMARQUE :** Pour supprimer une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Supprimer**. Pour afficher le contenu d'une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Sommaire**.  
  
12. Pour afficher ou sélectionner les options avancées, cliquez sur **Options** dans le volet **Sélectionner une page** .  
  
13. Sélectionnez une option **Remplacer le support** en cliquant sur un des éléments suivants :  
  
    -   **Sauvegarder sur le support de sauvegarde existant**  
  
         Pour cette option, cliquez sur **Ajouter au jeu de sauvegarde existant** ou sur **Remplacer tous les jeux de sauvegarde existants**. Pour plus d’informations sur la sauvegarde sur un jeu de supports existant, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
         Vous pouvez aussi activer la case à cocher **Vérifier le nom du support de sauvegarde et la date d'expiration du jeu de sauvegarde** pour forcer l'opération de sauvegarde à vérifier la date et l'heure de l'expiration du jeu de supports ou du jeu de sauvegarde.  
  
         Vous pouvez éventuellement entrer un nom dans la zone de texte **Nom du support de sauvegarde** . Si aucun nom n'est spécifié, un support de sauvegarde avec un nom vide est créé. Si vous spécifiez un nom pour le jeu de sauvegarde, le support (bande ou disque) est vérifié pour voir si le nom réel correspond bien au nom que vous entrez ici.  
  
         Si vous n'entrez pas de nom et que vous activez la case à cocher pour demander la vérification par rapport au support, le nom du support sera également vide sur le support.  
  
    -   **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**  
  
         Pour cette option, entrez un nom dans la zone de texte **Nouveau nom du support de sauvegarde** et décrivez éventuellement le jeu de supports dans la zone de texte **Description du nouveau support de sauvegarde** . Pour plus d’informations sur la création d’un jeu de supports existant, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
14. Dans la section **Fiabilité** , vous pouvez activer les cases à cocher :  
  
    -   **Vérifier la sauvegarde en fin d'opération**;  
  
    -   **Effectuer une somme de contrôle avant d'écrire sur le support**et éventuellement **Continuer lors d'erreurs de somme de contrôle**. Pour plus d’informations sur les sommes de contrôle, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
15. Si vous effectuez la sauvegarde sur un lecteur de bande (spécifié dans la section **Destination** de la page **Général** ), l’option **Décharger la bande après la sauvegarde** est active. Cliquer sur cette option active l'option **Rembobiner la bande avant de décharger** .  
  
    > **REMARQUE :** Les options de la section **Journal des transactions** sont inactives, à moins que vous sauvegardiez un journal des transactions (spécifié dans la section **Type de sauvegarde** de la page **Général** ).  
  
16. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et les versions ultérieures prennent en charge la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Par défaut, la compression d’une sauvegarde dépend de la valeur de l’option de configuration de serveur **Compression par défaut des sauvegardes** . Toutefois, quelle que soit la valeur par défaut actuelle au niveau du serveur, vous pouvez compresser une sauvegarde en activant **Compresser la sauvegarde**, et vous pouvez empêcher la compression en activant **Ne pas compresser la sauvegarde**.  
  
     **Pour consulter la valeur par défaut de compression de la sauvegarde actuelle**  
  
    -   [Afficher ou configurer l'option de configuration du serveur valeur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
  
## <a name="back-up-files-and-filegroups-using-t-sql"></a>Sauvegarder des groupes de fichiers et des fichiers à l’aide de T-SQL
  
1.  Pour créer une sauvegarde de fichier ou de groupe de fichiers, utilisez une instruction [BACKUP DATABASE <fichier_ou_groupe_de_fichiers>](../../t-sql/statements/backup-transact-sql.md). Au minimum, cette instruction doit spécifier les actions suivantes :  
  
    -   Nom de la base de données.  
  
    -   Clause FILE ou FILEGROUP pour chaque fichier ou groupe de fichiers, respectivement.  
  
    -   L’unité de sauvegarde où sera écrite la sauvegarde complète.  
  
     La syntaxe de base [!INCLUDE[tsql](../../includes/tsql-md.md)] d'une sauvegarde de fichiers est la suivante :  
  
     BACKUP DATABASE *database*  
  
     { FILE **=***nom_fichier_logique* | FILEGROUP **=***nom_groupe_fichiers_logique* } [ **,**...* f* ]  
  
     TO *unité_sauvegarde* [ **,**...*n* ]  
  
     [ WITH *options_with* [ **,**...*o* ] ] ;  
  
    |Option|Description|  
    |------------|-----------------|  
    |*database*|Correspond à la base de données à partir de laquelle va être opérée la sauvegarde du journal des transactions, c'est à dire la sauvegarde complète ou partielle.|  
    |FILE **=***logical_file_name*|Indique le nom logique d'un fichier à inclure dans la sauvegarde de fichiers.|  
    |FILEGROUP **=***logical_filegroup_name*|Indique le nom logique d'un groupe de fichiers à inclure dans la sauvegarde de fichiers. En mode de récupération simple, la sauvegarde d'un groupe de fichiers n'est autorisée que pour un groupe de fichiers en lecture seule.|  
    |[ **,**...*f* ]|Espace réservé indiquant qu'il est possible de spécifier plusieurs fichiers et groupes de fichiers. Le nombre de fichiers ou de groupes de fichiers est illimité.|  
    |*unité_sauvegarde* [ **,**...*n* ]|Spécifie une liste de 1 à 64 unités de sauvegarde à utiliser pour l'opération de sauvegarde. Vous pouvez spécifier une unité de sauvegarde physique ou une unité de sauvegarde logique correspondante, si celle-ci est déjà définie. Pour spécifier une unité de sauvegarde physique, utilisez l'option DISK ou TAPE :<br /><br /> { DISK &#124; TAPE } **=***physical_backup_device_name*<br /><br /> Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|  
    |WITH *options_with* [ **,**...*o* ]|Spécifie, éventuellement, une ou plusieurs options supplémentaires telles que DIFFERENTIAL.<br /><br /> Remarque : une sauvegarde différentielle de fichiers requiert une sauvegarde complète de fichiers comme base. Pour plus d’informations, consultez [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md).|  
  
2.  En mode de restauration complète, vous devez aussi sauvegarder le journal des transactions. Pour utiliser un jeu complet de sauvegardes de fichiers complètes afin de restaurer une base de données, vous devez aussi disposer de suffisamment de sauvegardes de journal pour couvrir toutes les sauvegardes de fichiers depuis la première sauvegarde de fichiers. Pour plus d’informations, consultez [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md).  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Dans les exemples suivants, vous procédez à une sauvegarde d'un ou plusieurs fichiers des groupes de fichiers secondaires de la base de données `Sales` . Cette base de données fait appel au mode de restauration complète et contient les groupes de fichiers secondaires suivants :  
  
-   Un groupe de fichiers nommé `SalesGroup1` avec les fichiers `SGrp1Fi1` et `SGrp1Fi2`.  
  
-   Un groupe de fichiers nommé `SalesGroup2` avec les fichiers `SGrp2Fi1` et `SGrp2Fi2`.  
  
#### <a name="a-create-a-file-backup-of-two-files"></a>A. Créer une sauvegarde de fichiers de deux fichiers  
 Dans l'exemple suivant, vous créez une sauvegarde de fichiers différentiel contenant seulement le fichier `SGrp1Fi2` du `SalesGroup1` et le fichier `SGrp2Fi2` du groupe de fichiers `SalesGroup2` .  
  
```sql  
--Backup the files in the SalesGroup1 secondary filegroup.  
BACKUP DATABASE Sales  
   FILE = 'SGrp1Fi2',   
   FILE = 'SGrp2Fi2'   
   TO DISK = 'G:\SQL Server Backups\Sales\SalesGroup1.bck';  
GO  
```  
  
#### <a name="b-create-a-full-file-backup-of-the-secondary-filegroups"></a>B. Créer une sauvegarde de fichiers complète des groupes de fichiers secondaires  
 L'exemple suivant crée une sauvegarde complète de tous les fichiers se trouvant dans les deux groupes de fichiers secondaires.  
  
```sql  
--Back up the files in SalesGroup1.  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'C:\MySQLServer\Backups\Sales\SalesFiles.bck';  
GO  
```  
  
#### <a name="c-create-a-differential-file-backup-of-the-secondary-filegroups"></a>C. Créer une sauvegarde de fichiers différentielle des groupes de fichiers secondaires  
 L'exemple suivant crée une sauvegarde différentielle de tous les fichiers se trouvant dans les deux groupes de fichiers secondaires.  
  
```sql  
--Back up the files in SalesGroup1.  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'C:\MySQLServer\Backups\Sales\SalesFiles.bck'  
   WITH   
      DIFFERENTIAL;  
GO  
```  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
1.  Utilisez l’applet de commande **Backup-SqlDatabase** et spécifiez **Files** comme valeur du paramètre **-BackupAction** . Spécifiez également l'un des paramètres suivants :  
  
    -   Pour sauvegarder un fichier spécifique, spécifiez le paramètre **-DatabaseFile***chaîne*, où *chaîne* représente un ou plusieurs fichiers de base de données à sauvegarder.  
  
    -   Pour sauvegarder tous les fichiers d’un groupe de fichiers donné, spécifiez le paramètre **-DatabaseFileGroup***chaîne*, où *chaîne* représente un ou plusieurs groupes de fichiers de base de données à sauvegarder.  
  
     L'exemple suivant crée une sauvegarde complète de tous les fichiers dans les groupes de fichiers secondaires 'FileGroup1' et 'FileGroup2' dans la base de données `MyDB` . Les sauvegardes sont créées à l’emplacement de sauvegarde par défaut de l’instance de serveur `Computer\Instance`.  
  
    ```sql  
    --Enter this command at the PowerShell command prompt, C:\PS>  
    Backup-SqlDatabase -ServerInstance Computer\Instance -Database MyDB -BackupAction Files -DatabaseFileGroup "FileGroup1","FileGroup2"  
    ```  
  
 **Configurer et utiliser le fournisseur PowerShell SQL Server**  
  
-   [Fournisseur SQL Server PowerShell](../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [Sauvegarder la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Restaurations de fichiers &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)  
  
  
