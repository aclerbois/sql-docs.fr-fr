---
title: Sauvegarder la base de données (page Général) | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.backupdatabase.general.f1
ms.assetid: 5c344dfd-1ad3-41cc-98cd-732973b4a162
caps.latest.revision: 64
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 1715f116d875a0037a926ab36900e52701b7bf98
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-database-general-page"></a>Sauvegarder la base de données (page Général)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Général** de la boîte de dialogue **Sauvegarder la base de données** vous permet d'afficher ou de modifier les paramètres d'une opération de sauvegarde de base de données.  
  
 Pour plus d’informations sur les concepts de base de la sauvegarde, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
> [!NOTE]  
>  Quand vous spécifiez une tâche de sauvegarde à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez générer le script [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) correspondant en cliquant sur le bouton **Script** et en sélectionnant une destination pour le script.  
  
 **Pour créer une sauvegarde à l'aide de SQL Server Management Studio**  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
    > [!IMPORTANT]  
    >  Vous pouvez définir un plan de maintenance de base de données pour créer des sauvegardes de base de données. Pour plus d'informations, consultez [Plans de maintenance de base de données](http://msdn.microsoft.com/library/ms187658.aspx) dans la documentation en ligne de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
 **Pour créer une sauvegarde partielle**  
  
-   Pour créer une sauvegarde partielle, vous devez utiliser l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) avec l'option PARTIAL.  
  
## <a name="options"></a>Options  
  
### <a name="source"></a>Source  
 Les options du volet **Source** identifient la base de données et spécifient le type de sauvegarde et le composant pour l'opération de sauvegarde.  
  
 **Sauvegarde de la base de données**  
 Sélectionnez la base de données à sauvegarder.  
  
 **Mode de récupération**  
 Spécifie le mode de récupération (simple, complète ou avec journal des transactions) pour la base de données sélectionnée.  
  
 **Type de sauvegarde**  
 Sélectionnez le type de sauvegarde que vous voulez réaliser sur la base de données spécifiée.  
  
|Type de sauvegarde|Disponible pour|Restrictions|  
|-----------------|-------------------|------------------|  
|Complète|Bases de données, fichiers et groupes de fichiers|Sur la base de données **master** , seules des sauvegardes complètes sont possibles.<br /><br /> En mode de récupération simple, les sauvegardes des fichiers et des groupes de fichiers sont disponibles uniquement pour les groupes de fichiers en lecture seule.|  
|Différentielle|Bases de données, fichiers et groupes de fichiers|En mode de récupération simple, les sauvegardes des fichiers et des groupes de fichiers sont disponibles uniquement pour les groupes de fichiers en lecture seule.|  
|Journal des transactions|Journaux des transactions|Les sauvegardes des journaux des transactions ne sont pas disponibles en mode de récupération simple.|  
  
 **Sauvegarde de copie uniquement**  
 Permet de créer une sauvegarde de type copie uniquement. Une *sauvegarde de copie uniquement* est une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indépendante du mécanisme des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conventionnelles. Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).  
  
> [!NOTE]  
>  Lorsque l'option **Différentielle** est sélectionnée, vous ne pouvez pas créer de sauvegarde de copie uniquement.  
  
 **Composant de sauvegarde**  
 Sélectionnez le composant de base de données à sauvegarder. Si **Journal des transactions** est sélectionné dans la liste **Type de sauvegarde** , cette option n'est pas activée.  
  
 Sélectionnez l'une des cases d'option suivantes :  
  
|||  
|-|-|  
|**Sauvegarde de la base de données**|Indique que l'intégralité de la base de données doit être sauvegardée.|  
|**Fichiers et groupes de fichiers**|Indique que les fichiers et/ou groupes de fichiers spécifiés doivent être sauvegardés.<br /><br /> La sélection de cette option provoque l'affichage de la boîte de dialogue **Sélection de fichiers et de groupes de fichiers** . Une fois que vous avez sélectionné les fichiers ou groupes de fichiers à sauvegarder et cliqué sur **OK**, vos sélections apparaissent dans la zone **Fichiers et groupes de fichiers** .|  
  
### <a name="destination"></a>Destination  
 Les options du volet **Destination** vous permettent de spécifier le type d'unité de sauvegarde pour l'opération de sauvegarde et de rechercher une unité de sauvegarde logique ou physique.  
  
> [!NOTE]  
>  Pour plus d’informations sur les unités de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 **Sauvegarde sur**  
 Sélectionnez un des types de support ci-dessous pour la sauvegarde. Les destinations que vous sélectionnez apparaissent dans la liste **Sauvegarde sur** .  
  
|||  
|-|-|  
|**Disque**|Permet de sauvegarder sur un disque. Il peut s'agir d'un fichier système ou d'une unité logique de sauvegarde sur disque créée pour la base de données. Les disques sélectionnés apparaissent dans la liste **Sauvegarde sur** . Vous pouvez sélectionner jusqu'à 64 disques pour l'opération de sauvegarde.|  
|**Bande**|Permet de sauvegarder sur bande. Il peut s'agir d'un lecteur de bandes local ou d'une unité logique de sauvegarde sur bande créée pour la base de données. Les bandes sélectionnées apparaissent dans la liste **Sauvegarde sur** . Le nombre maximal est de 64. Si aucun lecteur de bandes n'est connecté au serveur, cette option est désactivée. Les bandes que vous sélectionnez sont répertoriées dans la liste **Sauvegarde sur** .<br /><br /> Remarque : la prise en charge des unités de sauvegarde sur bande sera supprimée dans une prochaine version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.|  
|**URL**|Permet de sauvegarder dans le stockage d’objets blob Microsoft Azure.|  
  
 L'affichage des options suivantes dépend du type de destination sélectionné. Si vous sélectionnez Disque ou Bande, les options suivantes s'affichent.  
  
 **Ajouter**  
 Permet d’ajouter un fichier ou une unité à la liste **Sauvegarde sur** . Vous pouvez sauvegarder jusqu'à 64 unités simultanément sur un disque local ou distant. Pour spécifier un fichier sur un disque distant, utilisez le nom complet UNC (Universal Naming Convention). Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
 
 
  
 **Supprimer**  
 Permet de supprimer une ou plusieurs unités sélectionnées de la liste **Sauvegarde sur** .  
  
 **Sommaire**  
Permet d’afficher le contenu du support pour l’unité sélectionnée, s’il existe.  Le bouton n’exécute pas de fonction quand une **URL** est spécifiée. 
   
Boîte de dialogue**Sélectionner la destination de la sauvegarde** La boîte de dialogue **Sélectionner la destination de la sauvegarde** s’affiche quand vous sélectionnez **Ajouter**.   L’ensemble d’options qui s’affiche dépend du type de destination sélectionné. 

Si vous avez sélectionné **Disque** ou **Bande** comme destination de la sauvegarde, l’option suivante s’affiche.  

*
  **Nom de fichier**  
    Spécifiez le nom du fichier de sauvegarde.

Si vous avez sélectionné **URL** comme destination de la sauvegarde, les options suivantes s’affichent :
*
  **Conteneur de stockage Windows Azure**  
  Nom du conteneur de stockage Microsoft Azure pour stocker les fichiers de sauvegarde. 
   
*
  **Stratégie d’accès partagé :**  
  Entrez la signature d’accès partagé pour un conteneur entré manuellement.  Ce champ n’est pas disponible si un conteneur existant a été choisi.
  
*
  **Fichier de sauvegarde :**  
  Nom du fichier de sauvegarde.

*
  **Nouveau conteneur :**  
Permet d’enregistrer un conteneur existant pour lequel vous n’avez pas de signature d’accès partagé.  Consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un fichier de disque &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
