---
title: Sauvegarder la base de données, tâche (Plan de maintenance) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.maintplanproperties.logbackup.f1
- sql13.swb.maint.backup.f1
helpviewer_keywords:
- Back Up Database Task dialog box
ms.assetid: ed1ef012-fa14-4ba5-bafe-d1527ba065b3
caps.latest.revision: 52
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b7023f73087a0cb1b09f8c94ac7742252bca4cb7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="options-in-the-back-up-database-task-for-maintenance-plan"></a>Options de la boîte de dialogue Sauvegarder la base de données pour le plan de maintenance
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la boîte de dialogue **Tâche Sauvegarder la base de données** pour ajouter une tâche de sauvegarde au plan de maintenance. La sauvegarde de la base de données est importante pour pallier son endommagement possible à la suite d'une défaillance matérielle ou logicielle (ou d'erreurs des utilisateurs), en permettant sa restauration à partir d'une copie de sauvegarde. Cette tâche vous permet d'effectuer des sauvegardes des journaux des transactions, des sauvegardes de groupe de fichiers et de fichiers, des sauvegardes différentielles et complètes.  
  
 **Pour créer une tâche de sauvegarde de base de données**  
  
-   [Créer un plan de maintenance](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
## <a name="options"></a>Options  
 **Connexion**  
 Sélectionnez la connexion serveur à utiliser pour exécuter la tâche.  
  
 **Nouveau**  
 Crée une nouvelle connexion serveur à utiliser pour exécuter la tâche. La boîte de dialogue **Nouvelle connexion** est décrite ci-dessous.  
  
 **Bases de données**  
 Spécifie les bases de données faisant l'objet de cette tâche. Lorsque vous sélectionnez cette option, la liste déroulante comprend les options suivantes : **Toutes les bases de données**, **Toutes les bases de données système**, **Toutes les bases de données utilisateur**, **Ces bases de données**.  
  
 **Toutes les bases de données**  
 Génère un plan de maintenance qui exécute les tâches de maintenance sur toutes les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Toutes les bases de données système (master, model et msdb)**  
 Génère un plan de maintenance qui exécute les tâches de maintenance sur chacune des bases de données système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Aucune tâche de maintenance n'est exécutée sur les bases de données créées par l'utilisateur.  
  
 **Toutes les bases de données utilisateur (autre que master, model et msdb)**  
 Génère un plan de maintenance qui exécute des tâches de maintenance sur toutes les bases de données créées par l'utilisateur. Aucune tâche de maintenance n'est exécutée sur les bases de données système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Ces bases de données**  
 Génère un plan de maintenance qui n'exécute les tâches de maintenance que sur les bases de données sélectionnées. Si vous choisissez cette option, sélectionnez au moins une base de données.  
  
 **Type de sauvegarde**  
 Affiche le type de sauvegarde à effectuer.  
  
 **Composant de sauvegarde**  
 Sélectionnez **Base de données** pour sauvegarder la totalité de la base de données. Sélectionnez **Fichier et groupes de fichiers** pour sauvegarder seulement une partie de la base de données. Spécifiez ensuite le nom du fichier ou du groupe de fichiers. Si vous avez sélectionné plusieurs bases de données dans la zone **Base de données** , ne spécifiez que **Bases de données** pour **Composant de sauvegarde**. Pour exécuter des sauvegardes de fichiers ou de groupes de fichiers, créez une tâche pour chaque base de données.  
  
 **Expiration du jeu de sauvegarde**  
 Indique la date à laquelle le jeu de sauvegarde peut être écrasé par un autre jeu de sauvegarde.  
  
 **Sauvegarde sur**  
 Sauvegarde la base de données dans un fichier ou sur une bande. Seuls les périphériques à bande connectés à l'ordinateur sur lequel figure la base de données sont disponibles.  
  
 **Sauvegarder les bases de données sur un ou plusieurs fichiers**  
 Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** , puis indiquez un ou plusieurs emplacements sur le disque ou sur un périphérique à bandes.  
  
 **Si des fichiers de sauvegarde existent**  
 Sélectionnez **Ajouter** pour ajouter cette sauvegarde à la fin du fichier. Sélectionnez **Remplacer**pour supprimer toutes les anciennes sauvegardes du fichier et les remplacer par la nouvelle.  
  
 **Créer un fichier de sauvegarde pour chaque base de données**  
 Crée un fichier de sauvegarde à l'emplacement spécifié dans la zone Dossier. Un fichier unique est créé pour chaque base de données sélectionnée.  
  
 **Créer un sous-répertoire pour chaque base de données**  
 Sélectionnez cette option pour placer chaque base de données dans un sous-dossier.  
  
> [!IMPORTANT]  
>  Bien que les plans de maintenance puissent créer des sous-répertoires, les tâches de maintenance ne peuvent pas en supprimer. Cette fonctionnalité réduit la possibilité d'une attaque malveillante qui utilise la tâche de nettoyage de maintenance pour supprimer des fichiers.  
  
> [!IMPORTANT]  
>  Le sous-répertoire hérite les autorisations du répertoire parent. Limitez les autorisations pour éviter les accès non autorisés.  
  
 **Dossier**  
 Spécifiez le dossier dans lequel seront placés les fichiers de base de données créés automatiquement.  
  
 **Extension du fichier de sauvegarde**  
 Spécifiez l'extension à utiliser pour les fichiers de sauvegarde. La valeur par défaut est **.bak**.  
  
 **Vérifier l'intégrité de la sauvegarde**  
 Vérifie si le jeu de sauvegarde est complet et que tous les volumes sont lisibles.  
  
 **Sauvegarder la fin du journal et laisser la base de données dans l'état de restauration**  
 Effectue une sauvegarde de journal comme dernière étape avant la restauration d'une base de données. Pour plus d’informations, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 **Définir la compression de la sauvegarde**  
 Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure), sélectionnez l’une des valeurs de [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md) suivantes :  
  
|||  
|-|-|  
|**Utiliser le paramètre du serveur par défaut**|Cliquez sur cette option pour utiliser la valeur par défaut au niveau du serveur.<br /><br /> Cette valeur par défaut est définie par l’option de configuration de serveur **Compression par défaut des sauvegardes** . Pour plus d’informations sur l’affichage du paramétrage actuel de cette option, consultez [Afficher ou configurer l’option de configuration du serveur valeur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
|**Compresser la sauvegarde**|Cliquez sur cette option pour compresser la sauvegarde, indépendamment de la valeur par défaut au niveau du serveur.<br /><br /> **\*\* Important \*\*** Par défaut, la compression augmente considérablement l’utilisation de l’UC, et l’UC supplémentaire consommée par le processus de compression peut nuire aux opérations simultanées. Par conséquent, il peut être préférable de créer une sauvegarde compressée de priorité basse dans une session où l’utilisation de l’UC est limitée par [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Pour plus d'informations, consultez [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
|**Ne pas compresser la sauvegarde**|Cliquez sur cette option pour créer une sauvegarde non compressée, indépendamment de la valeur par défaut au niveau du serveur.|  
  
 **Vue T-SQL**  
 Affiche les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées sur le serveur pour cette tâche, selon les options sélectionnées.  
  
> [!NOTE]  
>  Si le nombre d'objets impliqués est élevé, l'affichage des instructions peut prendre un temps considérable.  
  
## <a name="new-connection-dialog-box"></a>Boîte de dialogue Nouvelle connexion  
 **Nom de la connexion**  
 Entrez un nom pour la nouvelle connexion.  
  
 **Sélectionnez ou entrez un nom de serveur.**  
 Sélectionnez un serveur auquel établir la connexion pour exécuter la tâche.  
  
 **Actualiser**  
 Actualise la liste des serveurs disponibles.  
  
 **Entrez des informations pour vous connecter au serveur**  
 Spécifiez le mode d'authentification sur le serveur.  
  
 **Utiliser la sécurité intégrée à Windows NT**  
 Permet de se connecter à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l’aide de l’authentification Windows.  
  
 **Utiliser un nom d'utilisateur et un mot de passe spécifiques**  
 Permet de se connecter à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] en utilisant l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option n'est pas disponible.  
  
 **User name**  
 Fournit le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
 **Mot de passe**  
 Fournit un mot de passe à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  
  
  
