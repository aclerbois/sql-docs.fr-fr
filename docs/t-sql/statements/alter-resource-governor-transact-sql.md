---
title: ALTER RESOURCE GOVERNOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER RESOURCE GOVERNOR
- ALTER_RESOURCE_GOVERNOR_TSQL
- ALTER RESOURCE GOVERNOR RECONFIGURE
- ALTER_RESOURCE_GOVERNOR_RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE GOVERNOR
- RECONFIGURE, ALTER RESOURCE GOVERNOR
ms.assetid: 442c54bf-a0a6-4108-ad20-db910ffa6e3c
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 312448ec80ce4f63a62cba9bee1db251655c7835
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="alter-resource-governor-transact-sql"></a>ALTER RESOURCE GOVERNOR (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Cette instruction permet d’exécuter les opérations Resource Governor suivantes dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Appliquer les modifications de configuration spécifiées quand les instructions CREATE|ALTER|DROP WORKLOAD GROUP or CREATE|ALTER|DROP RESOURCE POOL ou CREATE|ALTER|DROP EXTERNAL RESOURCE POOL sont exécutées.  
  
-   Activer ou désactiver le gouverneur de ressources.  
  
-   Configurer la classification des demandes entrantes.  
  
-   Réinitialiser le groupe de charges de travail et les statistiques de pool de ressources.  
  
-   Définir les opérations d'E/S maximales par volume disque.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ALTER RESOURCE GOVERNOR   
      { DISABLE | RECONFIGURE }  
    | WITH ( CLASSIFIER_FUNCTION = { schema_name.function_name | NULL } )  
    | RESET STATISTICS  
    | WITH ( MAX_OUTSTANDING_IO_PER_VOLUME = value )   
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 DISABLE  
 Désactive le gouverneur de ressources. La désactivation de Resource Governor a les conséquences suivantes :  
  
-   La fonction classifieur n’est pas exécutée.  
  
-   Toutes les nouvelles connexions sont classifiées automatiquement dans le groupe par défaut.  
  
-   Les demandes initiées par le système sont classées dans le groupe de charge de travail interne.  
  
-   Tous les paramètres de groupe de charges de travail et de pool de ressources existants sont réinitialisés à leurs valeurs par défaut. Dans ce cas, aucun événement n'est déclenché lorsque les limites sont atteintes.  
  
-   La surveillance système normale n'est pas affectée.  
  
-   La configuration peut être modifiée, mais les modifications n’entrent en vigueur qu’une fois que Resource Governor est activé.  
  
-   Lors du redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le gouverneur de ressources ne chargera pas sa configuration, mais à la place aura uniquement les groupes et pools par défaut et internes.  
  
 RECONFIGURE  
 Lorsque le gouverneur de ressources n'est pas activé, RECONFIGURE l'active. L’activation de Resource Governor a les conséquences suivantes :  
  
-   La fonction classifieur est exécutée pour les nouvelles connexions afin que leur charge de travail puisse être assignée aux groupes de charges de travail.  
  
-   Les limites de ressource qui sont spécifiées dans la configuration de Resource Governor sont honorées et appliquées.  
  
-   Les demandes qui ont existé avant d'activer le gouverneur de ressources sont affectées par les modifications de configuration qui ont été effectuées lorsque le gouverneur de ressources était désactivé.  
  
 Quand Resource Governor est en cours d’exécution, l’option RECONFIGURE s’applique à toutes les modifications de configuration demandées quand les instructions CREATE|ALTER|DROP WORKLOAD GROUP or CREATE|ALTER|DROP RESOURCE POOL ou CREATE|ALTER|DROP EXTERNAL RESOURCE POOL sont exécutées.  
  
> [!IMPORTANT]  
>  L'instruction ALTER RESOURCE GOVERNOR RECONFIGURE doit être exécutée pour que les modifications de configuration soient appliquées.  
  
 CLASSIFIER_FUNCTION = { *schema_name ***.*** function_name* | NULL }  
 Enregistre la fonction de classification spécifiée par *schema_name.function_name*. Cette fonction classifie chaque nouvelle session et assigne les demandes et requêtes de session à un groupe de charges de travail. Lorsque la valeur NULL est utilisée, les nouvelles sessions sont assignées automatiquement au groupe de charges de travail par défaut.  
  
 RESET STATISTICS  
 Réinitialise les statistiques sur tous les groupes de charges de travail et pools de ressources. Pour plus d’informations, consultez [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md) et [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
 MAX_OUTSTANDING_IO_PER_VOLUME = *value*  
 **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Définit les opérations d'E/S maximales en file d'attente par volume disque. Ces opérations d'E/S en peuvent être des lectures ou des écritures de toute taille.  La valeur maximale pour MAX_OUTSTANDING_IO_PER_VOLUME est 100. Ce n'est pas un pourcentage. Ce paramètre est conçu pour adapter la gouvernance des ressources d'E/S aux spécifications d'E/S d'un volume disque. Nous vous recommandons de faire des essais avec différentes valeurs et d’utiliser un outil d’étalonnage comme IOMeter, [DiskSpd](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) ou SQLIO (déprécié) afin d’identifier la valeur maximale pour votre sous-système de stockage. Ce paramètre offre une vérification de la sécurité au niveau du système qui permet à SQL Server d'obtenir le minimum d'E/S par seconde pour les pools de ressources même si d'autres pools sont définis avec le paramètre MAX_IOPS_PER_VOLUME illimité. Pour plus d’informations sur MAX_IOPS_PER_VOLUME, consultez [CREATE RESOURCE POOL](../../t-sql/statements/create-resource-pool-transact-sql.md).  
  
## <a name="remarks"></a>Notes   
 ALTER RESOURCE GOVERNOR DISABLE, ALTER RESOURCE GOVERNOR RECONFIGURE et ALTER RESOURCE GOVERNOR RESET STATISTICS ne peuvent pas être utilisés dans une transaction utilisateur.  
  
 Le paramètre RECONFIGURE fait partie de la syntaxe de Resource Governor et ne doit pas être confondu avec [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), qui est une instruction DDL distincte.  
  
 Nous vous recommandons de connaître les états du gouverneur de ressources avant d'exécuter des instructions DDL. Pour plus d’informations, consultez [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-starting-the-resource-governor"></a>A. Démarrage du gouverneur de ressources  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé pour la première fois, le gouverneur de ressources est désactivé. L'exemple suivant permet de démarrer le gouverneur de ressources. Après l'exécution de l'instruction, le gouverneur de ressources s'exécute et peut utiliser les groupes de charges de travail et pools de ressources prédéfinis.  
  
```  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="b-assigning-new-sessions-to-the-default-group"></a>B. Attribution de nouvelles sessions au groupe par défaut  
 L'exemple suivant assigne toutes les nouvelles sessions au groupe de charges de travail par défaut en supprimant toute fonction classifieur existante de la configuration du gouverneur de ressources. Lorsqu'aucune fonction n'est désignée comme fonction classifieur, toutes les nouvelles sessions sont assignées au groupe de charges de travail par défaut. Cette modification s'applique uniquement aux nouvelles sessions. Les sessions existantes ne sont pas affectées.  
  
```  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION = NULL);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
```  
  
### <a name="c-creating-and-registering-a-classifier-function"></a>C. Création et enregistrement d'une fonction classifieur  
 L'exemple suivant crée une nouvelle fonction classifieur nommée `dbo.rgclassifier_v1`. La fonction classifie chaque nouvelle session sur la base du nom d'utilisateur ou du nom d'application et assigne les demandes et requêtes de session à un groupe de charges de travail spécifique. Les sessions qui ne sont pas mappées aux noms d'utilisateur ou d'application spécifiés sont assignés au groupe de charges de travail par défaut. La fonction classifieur est ensuite enregistrée et la modification de configuration est appliquée.  
  
```  
-- Store the classifier function in the master database.  
USE master;  
GO  
SET ANSI_NULLS ON;  
GO  
SET QUOTED_IDENTIFIER ON;  
GO  
CREATE FUNCTION dbo.rgclassifier_v1() RETURNS sysname   
WITH SCHEMABINDING  
AS  
BEGIN  
-- Declare the variable to hold the value returned in sysname.  
    DECLARE @grp_name AS sysname  
-- If the user login is 'sa', map the connection to the groupAdmin  
-- workload group.   
    IF (SUSER_NAME() = 'sa')  
        SET @grp_name = 'groupAdmin'  
-- Use application information to map the connection to the groupAdhoc  
-- workload group.  
    ELSE IF (APP_NAME() LIKE '%MANAGEMENT STUDIO%')  
        OR (APP_NAME() LIKE '%QUERY ANALYZER%')  
            SET @grp_name = 'groupAdhoc'  
-- If the application is for reporting, map the connection to  
-- the groupReports workload group.  
    ELSE IF (APP_NAME() LIKE '%REPORT SERVER%')  
        SET @grp_name = 'groupReports'  
-- If the connection does not map to any of the previous groups,  
-- put the connection into the default workload group.  
    ELSE  
        SET @grp_name = 'default'  
    RETURN @grp_name  
END;  
GO  
-- Register the classifier user-defined function and update the   
-- the in-memory configuration.  
ALTER RESOURCE GOVERNOR WITH (CLASSIFIER_FUNCTION=dbo.rgclassifier_v1);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="d-resetting-statistics"></a>D. Réinitialisation des statistiques  
 L'exemple suivant réinitialise tout le groupe de charges de travail et les statistiques de pool de ressources.  
  
```  
ALTER RESOURCE GOVERNOR RESET STATISTICS;  
```  
  
### <a name="e-setting-the-maxoutstandingiopervolume-option"></a>E. Définition de l'option MAX_OUTSTANDING_IO_PER_VOLUME  
 L'exemple suivant affecte la valeur 20 à l'option MAX_OUTSTANDING_IO_PER_VOLUME.  
  
```  
ALTER RESOURCE GOVERNOR  
WITH (MAX_OUTSTANDING_IO_PER_VOLUME = 20);   
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.dm_resource_governor_workload_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)  
  
  
