---
title: Vue d’ensemble de la restauration et de la récupération (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- restoring tables [SQL Server]
- backups [SQL Server], restore scenarios
- database backups [SQL Server], restore scenarios
- database restores [SQL Server]
- restoring [SQL Server]
- restores [SQL Server]
- table restores [SQL Server]
- restoring databases [SQL Server], about restoring databases
- database restores [SQL Server], scenarios
ms.assetid: e985c9a6-4230-4087-9fdb-de8571ba5a5f
caps.latest.revision: 46
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 7defe7f931b5fde516381264cf4af3589ebce49c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="restore-and-recovery-overview-sql-server"></a>Vue d'ensemble de la restauration et de la récupération (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Pour récupérer une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suite à une erreur, un administrateur de base de données doit restaurer un jeu de sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une séquence de restauration correcte du point de vue logique et explicite. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la restauration de données à partir de sauvegardes d'une base de données entière, d'un fichier de données ou d'une page de données, comme suit :  
  
-   La base de données ( *restauration de base de données complète*)  
  
     La base de données complète est restaurée et récupérée, et la base de données est hors connexion pendant la durée des opérations de restauration et de récupération.  
  
-   Le fichier de données ( *restauration de fichiers*)  
  
     Un fichier de données ou un ensemble de fichiers est restauré et récupéré. Au cours d'une restauration de fichiers, les groupes de fichiers contenant les fichiers sont mis automatiquement hors connexion pendant la durée de la restauration. Toute tentative d'accès à un groupe de fichiers hors connexion produit une erreur.  
  
-   La page de données ( *restauration de pages*)  
  
     En mode de restauration complète ou de récupération utilisant les journaux de transactions, vous pouvez restaurer des bases de données individuelles. La restauration des pages peut être effectuée pour n'importe quelle base de données, quel que soit le nombre de groupes de fichiers.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La sauvegarde et la restauration fonctionnent sur tous les systèmes d’exploitation pris en charge. Pour plus d’informations sur les systèmes d’exploitation pris en charge, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Pour plus d’informations sur la prise en charge de sauvegardes provenant de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez la section « Prise en charge de la compatibilité » de [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 **Dans cette rubrique :**  
  
-   [Présentation des scénarios de restauration](#RestoreScenariosOv)  
  
-   [Modes de récupération et opérations de restauration prises en charge](#RMsAndSupportedRestoreOps)  
  
-   [Restrictions de restauration en mode de récupération simple](#RMsimpleScenarios)  
  
-   [Restauration en mode de récupération utilisant les journaux de transactions](#RMblogRestore)  
  
-   [Assistant de récupération de base de données (SQL Server Management Studio)](#DRA)  
  
-   [Contenu associé](#RelatedContent)  
  
##  <a name="RestoreScenariosOv"></a> Présentation des scénarios de restauration  
 Un *scénario de restauration* dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est le processus de restauration des données à partir d'une ou de plusieurs sauvegardes, puis de récupération de la base de données. Les scénarios de restauration pris en charge dépendent du mode de récupération de la base de données et de l'édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le tableau suivant présente les scénarios de restauration possibles pris en charge pour différents modes de récupération.  
  
|scénario de restauration|En mode de récupération simple|En modes de restauration complète et de récupération utilisant les journaux de transactions|  
|----------------------|---------------------------------|----------------------------------------------|  
|restauration de base de données complète|Il s'agit de la stratégie de restauration de base. Une restauration complète de base de données peut impliquer simplement la restauration et la récupération d'une sauvegarde complète de base de données. Une restauration complète de base de données peut également impliquer la restauration d'une sauvegarde complète de base de données suivie de la restauration et de la récupération d'une sauvegarde différentielle.<br /><br /> Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md).|Il s'agit de la stratégie de restauration de base. Une restauration complète de base de données inclut la restauration d'une sauvegarde complète, et éventuellement d'une sauvegarde différentielle (le cas échéant), suivie par la restauration de toutes les sauvegardes de journal consécutives (en séquence). La restauration de base de données complète s'achève par la récupération de la dernière sauvegarde de journal ainsi que sa restauration (RESTORE WITH RECOVERY).<br /><br /> Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|  
|File restore **\***|Restaure un ou plusieurs fichiers endommagés en lecture seule, sans restaurer toute la base de données. La restauration de fichiers est uniquement disponible si la base de données comporte au moins un groupe de fichiers en lecture seule.|Restaure un ou plusieurs fichiers, sans restaurer la totalité de la base de données. La restauration de fichiers peut être effectuée lorsque la base de données est hors connexion ou, pour certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], alors que la base de données reste en ligne. Pendant une restauration de fichiers, les groupes de fichiers contenant les fichiers à restaurer restent toujours hors connexion.|  
|restauration de pages|Non applicable|Restaure une ou plusieurs pages endommagées. La restauration de pages peut être effectuée lorsque la base de données est hors connexion ou, pour certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], alors que la base de données reste en ligne. Pendant une restauration de pages, les pages en cours de restauration restent toujours hors connexion.<br /><br /> Une chaîne ininterrompue de sauvegardes de journaux doit être disponible, jusqu'au fichier journal actuel, et elles doivent toutes être appliquées pour mettre la page à jour par rapport au fichier journal actuel.<br /><br /> Pour plus d’informations, consultez [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).|  
|Restauration fragmentaire **\***|Restaure et récupère la base de données par phases au niveau du groupe de fichiers, en commençant par les groupes de fichiers primaires et tous les groupes de fichiers secondaires en lecture-écriture.|Restaure et récupère la base de données par étapes au niveau du groupe de fichiers, en commençant par le groupe de fichiers primaire.|  
  
 **\*** La restauration en ligne est prise en charge uniquement dans l'édition Enterprise.  
  
 Indépendamment du mode de restauration des données, avant de pouvoir récupérer une base de données, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] garantit la cohérence logique de l'intégralité de la base de données. Par exemple, si vous restaurez un fichier, vous ne pouvez pas le récupérer et le mettre en ligne tant qu'il n'a pas été restauré par progression suffisamment pour être cohérent avec la base de données.  
  
### <a name="advantages-of-a-file-or-page-restore"></a>Avantages de la restauration d'un fichier ou d'une page  
 Restaurer et récupérer des fichiers ou des pages plutôt que la base de données entière offre plusieurs avantages :  
  
-   Le fait de restaurer moins de données réduit le temps nécessaire pour les copier et les récupérer.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la restauration des pages ou des fichiers peut permettre à d'autres données de la base de données de rester en ligne pendant l'opération de restauration.  
  
##  <a name="RMsAndSupportedRestoreOps"></a> Modes de récupération et opérations de restauration prises en charge  
 Les opérations de restauration disponibles pour une base de données dépendent de son mode de récupération. Le tableau suivant présente le niveau de prise en charge des modes de récupération dans un scénario de restauration donné.  
  
|Opération de restauration|Mode de restauration complète|Mode de récupération utilisant les journaux de transactions|Mode de récupération simple|  
|-----------------------|-------------------------|---------------------------------|---------------------------|  
|Récupération de données|Récupération complète (si le journal est disponible).|Risque de perte de données.|Les données postérieures à la dernière sauvegarde différentielle ou complète sont perdues.|  
|Restauration dans le temps|Toute heure couverte par les sauvegardes de fichiers journaux.|Non autorisé si la sauvegarde de fichier journal contient des modifications journalisées en bloc.|Non pris en charge.|  
|File restore **\***|Prise en charge complète.|Parfois.**\*\***|Disponible uniquement pour les fichiers secondaires en lecture seule.|  
|Page restore **\***|Prise en charge complète.|Parfois.**\*\***|Aucun.|  
|Restauration fragmentaire (niveau groupe de fichiers) **\***|Prise en charge complète.|Parfois.**\*\***|Disponible uniquement pour les fichiers secondaires en lecture seule.|  
  
 **\*** Disponible uniquement dans l'édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 **\*\*** Pour les conditions requises, consultez [Restrictions de restauration en mode de récupération simple](#RMsimpleScenarios), plus loin dans cette rubrique.  
  
> [!IMPORTANT]  
>  Quel que soit le mode de récupération d'une base de données, une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être restaurée par une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à la version qui a créé la sauvegarde.  
  
##  <a name="RMsimpleScenarios"></a> Restrictions de restauration en mode de récupération simple  
 Le mode de récupération simple impose les restrictions suivantes aux opérations de restauration :  
  
-   La restauration de fichiers et la restauration fragmentaire ne s'adressent qu'aux groupes de fichiers secondaires en lecture seule. Pour plus d’informations sur ces scénarios de restauration, consultez [Restaurations de fichiers &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md) et [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
-   La restauration de pages n'est pas autorisée.  
  
-   La restauration dans le temps n'est pas autorisée.  
  
 Si ces restrictions ne correspondent pas à vos besoins de récupération, nous vous recommandons d'envisager le mode de restauration complète. Pour plus d’informations, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
> [!IMPORTANT]  
>  Quel que soit le mode de récupération d'une base de données, une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être restaurée par une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à la version qui a créé la sauvegarde.  
  
##  <a name="RMblogRestore"></a> Restauration en mode de récupération utilisant les journaux de transactions  
 Cette section traite de considérations relatives à la restauration qui sont propres au mode de récupération utilisant les journaux de transactions et qui vient en complément d'une utilisation exclusive du mode de restauration complète.  
  
> [!NOTE]  
>  Pour obtenir une présentation du mode de récupération utilisant les journaux de transactions, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Généralement, le mode de récupération utilisant les journaux de transactions est comparable au mode de restauration complète. En outre, les informations fournies pour ce dernier valent aussi pour le premier. Cependant, la récupération jusqu'à une date et heure et la restauration en ligne sont concernées par le mode de récupération utilisant les journaux de transactions.  
  
### <a name="restrictions-for-point-in-time-recovery"></a>Restrictions relatives à la récupération jusqu'à une date et heure  
 Si une sauvegarde du journal effectuée dans le mode de récupération utilisant les journaux de transactions contient des modifications journalisées en bloc, la récupération jusqu'à une date et heure n'est pas autorisée. Si vous tentez d'effectuer une récupération jusqu'à une date et heure sur une sauvegarde du journal contenant des modifications en bloc, l'opération de restauration peut échouer.  
  
### <a name="restrictions-for-online-restore"></a>Restrictions relatives à la restauration en ligne  
 Une séquence de restauration en ligne fonctionne uniquement si les conditions suivantes sont satisfaites :  
  
-   Toutes les sauvegardes de journaux nécessaires doivent avoir été effectuées avant le démarrage de la séquence de restauration.  
  
-   Les modifications en bloc doivent avoir été sauvegardées avant de démarrer la séquence de restauration en ligne.  
  
-   Si des modifications en bloc existent dans la base de données, tous les fichiers doivent être en ligne ou[obsolètes](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md). (Cela signifie qu'ils n'appartiennent plus à la base de données.)  
  
 Si ces conditions ne sont pas satisfaites, la séquence de restauration en ligne échoue.  
  
> [!NOTE]  
>  Il est recommandé de basculer en mode de restauration complète avant de démarrer la restauration en ligne. Pour plus d’informations, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 Pour plus d’informations sur l’exécution d’une restauration en ligne, consultez [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
##  <a name="DRA"></a> Assistant de récupération de base de données (SQL Server Management Studio)  
 L'Assistant Récupération de base de données permet de créer des plans de restauration qui implémentent des séquences de restauration correctes et optimales. De nombreux problèmes connus, liés à la restauration de la base de données, et améliorations demandées par les clients ont été pris en considération. Les améliorations importantes introduites par l'Assistant Récupération de base de données sont les suivantes :  
  
-   **Algorithme de plan de restauration :**  l’algorithme utilisé pour créer des plans de restauration a été amélioré considérablement, en particulier pour les scénarios de restauration complexes. Nombre de cas limites, notamment la réplication de scénarios dans les restaurations ponctuelles, sont gérés plus efficacement que dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **Restaurations dans le temps :**  l’Assistant Récupération de base de données simplifie considérablement la restauration d’une base de données à un moment donné. Une chronologie visuelle de sauvegarde améliore considérablement la prise en charge des restaurations dans le temps. La chronologie visuelle vous permet d'identifier un point possible comme point de récupération cible pour restaurer une base de données. La chronologie permet de parcourir un chemin de récupération ramifié (un chemin d'accès qui couvre les branchements de récupération). Un plan spécifique de restauration dans le temps inclut automatiquement les sauvegardes pertinentes pour la restauration à un point cible (date et heure). Pour plus d’informations, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
 Pour plus d'informations sur l'Assistant Récupération de base de données, consultez les blogs de gestion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivants :  
  
-   [Assistant Récupération : introduction](http://blogs.msdn.com/b/managingsql/archive/2011/07/13/recovery-advisor-an-introduction.aspx)  
  
-   [Assistant Récupération : utilisation de SSMS pour créer/restaurer des sauvegardes fractionnées](http://blogs.msdn.com/b/managingsql/archive/2011/07/13/recovery-advisor-using-ssms-to-create-restore-split-backups.aspx)  
  
##  <a name="RelatedContent"></a> Contenu associé  
 Aucun.  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)  
  
  
