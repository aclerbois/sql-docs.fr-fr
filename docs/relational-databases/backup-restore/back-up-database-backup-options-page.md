---
title: Sauvegarder la base de données (page Options de sauvegarde) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.backupdatabase.options.f1
- swb.backupdatabase.options.f1
ms.assetid: df0ddcdb-c94e-472b-b786-469ae8117b93
caps.latest.revision: 62
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 148589c86bfd1b0762e3d87c67e0d2c624d268f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-database-backup-options-page"></a>Sauvegarder la base de données (page Options de sauvegarde)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la page  **Options de sauvegarde** de la boîte de dialogue **Sauvegarder la base de données** pour afficher ou modifier les options de sauvegarde de la base de données.  
  
 **Pour créer une sauvegarde à l'aide de SQL Server Management Studio**  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
> [!IMPORTANT]  
>  Vous pouvez définir un plan de maintenance de base de données pour créer des sauvegardes de base de données. Pour plus d’informations, consultez [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md) et [Utiliser l’Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).  
  
> [!NOTE]  
>  Quand vous spécifiez une tâche de sauvegarde à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez générer le script [!INCLUDE[tsql](../../includes/tsql-md.md)][BACKUP](../../t-sql/statements/backup-transact-sql.md) correspondant en cliquant sur le bouton **Script** et en sélectionnant une destination pour le script.  
  
## <a name="options"></a>Options  
  
### <a name="backup-set"></a>Jeu de sauvegarde  
 Les options du volet **Jeu de sauvegarde** vous permettent de spécifier des informations facultatives concernant le jeu de sauvegarde créé par l'opération de sauvegarde.  
  
 **Nom**  
 Spécifiez le nom du jeu de sauvegarde. Le système suggère automatiquement un nom par défaut en fonction du nom de la base de données et du type de sauvegarde.  
  
 Pour plus d’informations sur les jeux de sauvegarde, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
 **Description**  
 Entrez une description du jeu de sauvegarde.  
  
 **Expiration du jeu de sauvegarde**  
 Choisissez une des options d'expiration ci-dessous. Cette option est désactivée si **URL** est choisie en tant que destination de sauvegarde.  
  
|||  
|-|-|  
|**After**|Spécifiez le nombre de jours qui doivent s'écouler avant que le jeu de sauvegarde expire et puisse être écrasé. Cette valeur doit être comprise entre 0 et 99999 jours ; une valeur de 0 jour signifie que le jeu de sauvegarde n'expirera jamais.<br /><br /> La valeur par défaut d’expiration de sauvegarde correspond à la valeur définie dans l’option **Délai de rétention par défaut du support de sauvegarde (jours)** . Pour accéder à cette option, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets et cliquez sur **Propriétés**; ensuite, cliquez sur la page **Paramètres de base de données** de la boîte de dialogue **Propriétés du serveur** .|  
|**Actif**|Spécifiez une date spécifique à laquelle le jeu de sauvegarde expire et peut être écrasé.|  
  
### <a name="compression"></a>Compression  
 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou une version ultérieure) prend en charge la [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **Définir la compression de la sauvegarde**  
 Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (ou version ultérieure), sélectionnez l'une des valeurs de compression de la sauvegarde suivantes :  
  
|||  
|-|-|  
|**Utiliser le paramètre du serveur par défaut**|Cliquez sur cette option pour utiliser la valeur par défaut au niveau du serveur.<br /><br /> Cette valeur par défaut est définie par l’option de configuration de serveur **Compression par défaut des sauvegardes** . Pour plus d’informations sur l’affichage du paramétrage actuel de cette option, consultez [Afficher ou configurer l’option de configuration du serveur valeur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
|**Compresser la sauvegarde**|Cliquez sur cette option pour compresser la sauvegarde, indépendamment de la valeur par défaut au niveau du serveur.<br /><br /> **\*\* Important \*\*** Par défaut, la compression augmente considérablement l’utilisation de l’UC et l’UC supplémentaire consommée par le processus de compression peut avoir un impact néfaste sur les opérations simultanées. Par conséquent, il peut être préférable de créer une sauvegarde compressée de priorité basse dans une session où l’utilisation de l’UC est limitée par [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Pour plus d'informations, consultez [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
|**Ne pas compresser la sauvegarde**|Cliquez sur cette option pour créer une sauvegarde non compressée, indépendamment de la valeur par défaut au niveau du serveur.|  
  
### <a name="encryption"></a>Chiffrement  
 Pour créer une sauvegarde chiffrée, activez la case à cocher **Chiffrer le fichier de sauvegarde** . Sélectionnez l'algorithme de chiffrement à utiliser pour l'étape de chiffrement et fournissez un certificat ou une clé asymétrique dans la liste des certificats ou clés numériques existants. Les algorithmes disponibles pour le chiffrement sont :  
  
-   AES 128  
  
-   AES 192  
  
-   AES 256  
  
-   Triple DES  
  
> [!TIP]  
>  L'option de chiffrement est désactivée si vous avez choisi d'ajouter la sauvegarde à un jeu de sauvegarde existant.  
>   
>  Il s'agit de la méthode conseillée pour sauvegarder votre certificat ou vos clés et les stocker dans un emplacement différent de celui de la sauvegarde chiffrée.  
>   
>  Seules les clés résidant dans la gestion de clés extensible (EKM) sont prises en charge.  
  
## <a name="see-also"></a> Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
  
