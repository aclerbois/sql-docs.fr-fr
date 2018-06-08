---
title: Copier des bases de données avec la sauvegarde et la restauration | Microsoft Docs
ms.custom: ''
ms.date: 07/15/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], back up and restore
- restoring databases [SQL Server], previous SQL Server versions
- database restores [SQL Server], copying databases
- restoring databases [SQL Server], onto another server instance
- restoring [SQL Server], onto another server instance
- backing up databases [SQL Server], copying databases
- database backups [SQL Server], copying databases
ms.assetid: b93e9701-72a0-408e-958c-dc196872c040
caps.latest.revision: 61
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 113095b4e1ad71b2f034c3dd7f92c02a0fc65e9d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32931094"
---
# <a name="copy-databases-with-backup-and-restore"></a>Copier des bases de données avec la sauvegarde et la restauration
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous pouvez créer une base de données en restaurant une sauvegarde d'une base de données utilisateur créée à l'aide de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure. Cependant, les sauvegardes des bases de données **master**, **model** et **msdb** créées avec une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées par [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Par ailleurs, les sauvegardes [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] ne peuvent pas être restaurées par les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
>**IMPORTANT !** SQL Server 2016 utilise un chemin d’accès par défaut différent de celui des versions précédentes. Par conséquent, pour restaurer des sauvegardes d'une base de données créée à l'emplacement par défaut de versions antérieures, vous devez utiliser l'option MOVE. Pour plus d'informations sur le nouveau chemin par défaut, consultez [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md). Pour plus d'informations sur le déplacement des fichiers d'une base de données, consultez la section « Déplacement des fichiers d'une base de données » dans les pages suivantes de cette rubrique.  
  
## <a name="general-steps-for-using-backup-and-restore-to-copy-a-database"></a>Étapes générales de l’utilisation de la sauvegarde et de la restauration pour copier une base de données  
 Lorsque vous utilisez la sauvegarde et la restauration pour copier une base de données vers une autre instance de SQL Server, les ordinateurs source et de destination peuvent correspondre à n’importe quelle plateforme qui exécute SQL Server.  
  
 Voici les étapes de base :  
  
1.  Sauvegardez la base de données source, qui peut résider sur une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure. L’ordinateur qui exécute cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] constitue **l’ordinateur source**.  
  
2.  Sur l’ordinateur de destination de la copie de la base de données ( **ordinateur de destination**), connectez-vous à l’instance de SQL Server sur laquelle vous voulez restaurer la base de données. Si nécessaire, sur l’instance de serveur de **destination** , créez les mêmes unités de sauvegarde que celles utilisées pour sauvegarder les bases de données **sources** .  
  
3.  Restaurez la sauvegarde de la base de données **source** sur l’ordinateur de **destination** . La restauration automatique de la base de données crée tous les fichiers de la base de données.  
  
D’autres points sont susceptibles d’affecter cette procédure :
  
## <a name="before-you-restore-database-files"></a>Avant la restauration des fichiers de la base de données  
 La restauration automatique d’une base de données crée les fichiers de base de données nécessaires à la base de données à restaurer. Par défaut, les fichiers créés par SQL Server pendant la restauration utilisent les mêmes noms et chemins que les fichiers de sauvegarde de la base de données d’origine sur l’ordinateur source.  
  
 Lorsque vous restaurez la base de données, vous pouvez, si vous le voulez, définir le mappage de lecteur, les noms de fichiers ou le chemin de la base de données à restaurer. 
 
 Cela peut être nécessaire dans les cas suivants :  
  
-   La structure de répertoire ou le mappage de lecteur utilisé par la base de données sur l'ordinateur d'origine n'existe pas sur l'autre ordinateur. Par exemple, il est possible que la sauvegarde contienne un fichier à restaurer sur le lecteur E par défaut, mais que l'ordinateur de destination n'ait pas de lecteur E.  
  
-   L'emplacement cible peut ne pas disposer d'un espace suffisant.  
  
-   Vous réutilisez un nom de base de données qui existe sur la destination de la restauration et un de ses fichiers porte le même nom qu'un fichier de base de données du jeu de sauvegarde, auquel cas, une des actions suivantes se produit :  
  
    -   Si le fichier de base de données existant peut être remplacé, il l'est (cela n'affecte pas un fichier appartenant à un nom de base de données différent).  
  
    -   Si le fichier existant ne peut pas être remplacé, une erreur de restauration se produit.  
  
 Pour éviter des erreurs et des conséquences inattendues, avant l’opération de restauration, vous pouvez utiliser la table d’historique [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md) pour identifier la base de données et les fichiers journaux de la sauvegarde que vous envisagez de restaurer.  
  
## <a name="moving-the-database-files"></a>Déplacement des fichiers de la base de données  
 Si les fichiers figurant dans la sauvegarde de la base de données ne peuvent pas être restaurés sur l’ordinateur de destination, il est nécessaire de transférer les fichiers vers un nouvel emplacement lors de leur restauration. Exemple :  
  
-   Vous voulez restaurer une base de données depuis des sauvegardes créées dans l'emplacement par défaut de la version antérieure.  
  
-   Il peut s'avérer nécessaire de restaurer certains fichiers de la base de données de la sauvegarde sur un lecteur différent à cause des capacités de stockage. Cela se produit fréquemment car, à l’intérieur d’une organisation, le nombre et la taille des unités de disques, ainsi que les configurations logicielles diffèrent souvent d’un ordinateur à l’autre.  
  
-   Il peut s'avérer nécessaire de créer une copie d'une base de données existante sur le même ordinateur afin de procéder à des tests. Dans ce cas, les fichiers de la base de données initiale existent déjà et il faut donc préciser des noms de fichiers différents lors de la création de la base de données pendant la restauration.  
  
 Pour plus d'informations, consultez la section « Pour restaurer des fichiers et des groupes de fichiers dans un nouvel emplacement » dans les pages suivantes de cette rubrique.  
  
## <a name="changing-the-database-name"></a>Modification du nom de la base de données  
 Le nom de la base de données peut être modifié lors de la restauration sur l'ordinateur de destination, sans avoir à restaurer d'abord la base de données puis modifier le nom manuellement. Il peut s'avérer nécessaire, par exemple, de remplacer le nom de la base de données **Sales** par **SalesCopy** pour indiquer qu'il s'agit d'une copie d'une base de données.  
  
 Le nom de la base de données fourni explicitement lors de la restauration d’une base de données est utilisé automatiquement comme nouveau nom. Le nom de la base de données n'existant pas encore, un nouveau nom est créé en utilisant les fichiers figurant dans la sauvegarde.  
  
## <a name="when-upgrading-a-database-by-using-restore"></a>Lors de la mise à niveau d’une base de données en utilisant la restauration  
 Lors de la restauration de sauvegardes à partir d'une version antérieure, il est utile de savoir à l'avance si le chemin d'accès (lecteur ou répertoire) de chaque catalogue de texte intégral figurant dans la sauvegarde existe sur l'ordinateur de destination. Pour établir la liste des noms logiques et des noms physiques, du chemin et du nom de chaque fichier dans une sauvegarde, y compris les fichiers de catalogues, utilisez l’instruction RESTORE FILELISTONLY FROM *<unité_sauvegarde>*. Pour plus d’informations, consultez [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
 Si le même chemin n'existe pas sur l'ordinateur de destination, vous pouvez procéder de l'une des deux manières suivantes :  
  
-   Créez le mappage lecteur/répertoire équivalent sur l'ordinateur de destination.  
  
-   Transférez les fichiers de catalogues vers un nouvel emplacement lors de la restauration en utilisant la clause WITH MOVE dans l'instruction RESTORE DATABASE. Pour plus d’informations, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Pour plus d’informations sur les autres options de mise à niveau des index de recherche en texte intégral, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="database-ownership"></a>Propriété de la base de données  
 Lorsqu'une base de données est restaurée sur un autre ordinateur, la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows à l'origine de l'opération de restauration devient automatiquement le propriétaire de la nouvelle base de données. Lorsque la base de données est restaurée, l'administrateur du système ou le nouveau propriétaire de la base de données peut modifier la propriété de la base de données. Pour éviter la restauration non autorisée d’une base de données, utilisez des mots de passe de supports de sauvegarde ou de jeux de sauvegardes.  
  
## <a name="managing-metadata-when-restoring-to-another-server-instance"></a>Gestion de métadonnées lors de la restauration vers une autre instance de serveur  
 Lorsque vous restaurez une base de données sur une autre instance de serveur, pour garantir une expérience cohérente aux utilisateurs et aux applications, vous devrez peut-être recréer tout ou partie des métadonnées de la base de données, telles que les connexions et les travaux, sur l'autre instance de serveur. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
 **Afficher les données et les fichiers journaux dans un jeu de sauvegarde**  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
 **Restaurer des fichiers et groupes de fichiers dans un nouvel emplacement**  
  
-   [Restaurer des fichiers à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
 **Restaurer des fichiers et groupes de fichiers en remplaçant des fichiers existants**  
  
-   [Restaurer des fichiers et groupes de fichiers en remplaçant des fichiers existants &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
 **Restaurer une base de données avec un nouveau nom**  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
 **Redémarrer une opération de restauration interrompue**  
  
-   [Redémarrer une opération de restauration interrompue &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
 **Modifier le propriétaire de la base de données**  
  
-   [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)  
  
 **Copier une base de données à l’aide de SMO (SQL Server Management Objects)**  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadFileList%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.RelocateFiles%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReplaceDatabase%2A>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore>  
  
## <a name="see-also"></a>Voir aussi  
 [Copier des bases de données sur d'autres serveurs](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)   
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
