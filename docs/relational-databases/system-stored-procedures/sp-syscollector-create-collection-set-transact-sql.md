---
title: sp_syscollector_create_collection_set (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_syscollector_create_collection_set_TSQL
- sp_syscollector_create_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_create_collection_set
ms.assetid: 69e9ff0f-c409-43fc-89f6-40c3974e972c
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 6cefec5de4c7bba8d2b184a202f5a21c4a25901c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyscollectorcreatecollectionset-transact-sql"></a>sp_syscollector_create_collection_set (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un jeu d'éléments de collecte. Vous pouvez utiliser cette procédure stockée afin de créer un jeu d'éléments de collecte personnalisé pour la collecte de données.  
  
> [!WARNING]  
>  Dans les cas où le compte Windows configuré en tant que proxy est non interactif ou correspond à un utilisateur interactif qui ne s'est pas encore connecté, le répertoire de profils n'existera pas et la création du répertoire intermédiaire échouera. Par conséquent, si vous utilisez un compte proxy sur un contrôleur de domaine, vous devez spécifier un compte interactif qui a été utilisé au moins une fois afin de garantir que le répertoire de profils a été créé.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_create_collection_set   
      [ @name = ] 'name'  
    , [ [ @target = ] 'target' ]  
    , [ [ @collection_mode = ] collection_mode ]  
    , [ [ @days_until_expiration = ] days_until_expiration ]  
    , [ [ @proxy_id = ] proxy_id ]  
    , [ [ @proxy_name = ] 'proxy_name' ]  
    , [ [ @schedule_uid = ] 'schedule_uid' ]  
    , [ [ @schedule_name = ] 'schedule_name' ]  
    , [ [ @logging_level = ] logging_level ]  
    , [ [ @description = ] 'description' ]  
    , [ @collection_set_id = ] collection_set_id OUTPUT   
    , [ [ @collection_set_uid = ] 'collection_set_uid' OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@name =** ] '*nom*'  
 Est le nom de l’ensemble de la collection. *nom* est **sysname** et ne peut pas être une chaîne vide ou NULL.  
  
 *nom* doit être unique. Pour obtenir une liste de noms de jeux d'éléments de collecte actuels, interrogez la vue système syscollector_collection_sets.  
  
 [  **@target =** ] '*cible*'  
 Réservé pour un usage ultérieur. *nom* est **nvarchar (128)** avec une valeur par défaut NULL.  
  
 [  **@collection_mode =** ] *collection_mode*  
 Spécifie la manière dont les données sont recueillies et stockées. *collection_mode* est **smallint** et peut avoir l’une des valeurs suivantes :  
  
 0 - Mode mis en cache. La collecte et le téléchargement de données sont sur des planifications séparées. Spécifiez le mode mis en cache pour la collecte continue.  
  
 1 - Mode non mis en cache. Collecte de données et le téléchargement est sur la même planification. Spécifiez le mode non mis en cache pour une collecte ad hoc ou par instantané.  
  
 La valeur par défaut *collection_mode* est 0. Lorsque *collection_mode* est 0, *schedule_uid* ou *nom_de_la_planification* doit être spécifié.  
  
 [  **@days_until_expiration =** ] *days_until_expiration*  
 Est le nombre de jours pendant lesquels les données collectées sont enregistrées dans l’entrepôt de données de gestion. *days_until_expiration* est **smallint** avec la valeur par défaut 730 (deux ans). *days_until_expiration* doit être 0 ou un entier positif.  
  
 [  **@proxy_id =** ] *proxy_id*  
 Identificateur unique pour un compte d'Agent proxy [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *proxy_id* est **int** avec une valeur par défaut NULL. Si spécifié, *proxy_name* doit être NULL. Pour obtenir des *proxy_id*, interrogez la table système sysproxies. Le rôle de base de données fixe dc_admin doit avoir l'autorisation d'accéder au proxy. Pour plus d’informations, consultez [créer un Proxy de l’Agent SQL Server](http://msdn.microsoft.com/library/142e0c55-a8b9-4669-be49-b9dc602d5988).  
  
 [  **@proxy_name =** ] '*proxy_name*'  
 Nom du compte proxy. *proxy_name* est **sysname** avec une valeur par défaut NULL. Si spécifié, *proxy_id* doit être NULL. Pour obtenir des *proxy_name*, interrogez la table système sysproxies.  
  
 [  **@schedule_uid =** ] '*schedule_uid*'  
 GUID qui pointe vers une planification. *schedule_uid* est **uniqueidentifier** avec une valeur par défaut NULL. Si spécifié, *nom_de_la_planification* doit être NULL. Pour obtenir des *schedule_uid*, interrogez la table système sysschedules.  
  
 Lorsque *collection_mode* est définie sur 0, *schedule_uid* ou *nom_de_la_planification* doit être spécifié. Lorsque *collection_mode* est définie sur 1, *schedule_uid* ou *nom_de_la_planification* est ignoré s’il est spécifié.  
  
 [  **@schedule_name =** ] '*nom_de_la_planification*'  
 Est le nom de la planification. *nom_de_la_planification* est **sysname** avec une valeur par défaut NULL. Si spécifié, *schedule_uid* doit être NULL. Pour obtenir des *nom_de_la_planification*, interrogez la table système sysschedules.  
  
 [  **@logging_level =** ] *logging_level*  
 Niveau de journalisation. *logging_level* est **smallint** avec l’une des valeurs suivantes :  
  
 0 - informations de l'exécution du journal et événements [!INCLUDE[ssIS](../../includes/ssis-md.md)] qui effectuent le suivi des éléments suivants :  
  
-   Démarrage/arrêt de jeux d'éléments de collecte  
  
-   Démarrage/arrêt de packages  
  
-   Informations sur l'erreur  
  
 1 - Enregistrement de niveau 0 et :  
  
-   Statistiques d'exécution  
  
-   Progression de la collecte continuellement en cours d'exécution  
  
-   Événements d'avertissements de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 2 - Enregistrement de niveau 1 et informations d'événement détaillées de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 La valeur par défaut *logging_level* est 1.  
  
 [  **@description =** ] '*description*'  
 Description du jeu d'éléments de collecte. *Description* est **nvarchar (4000)** avec une valeur par défaut NULL.  
  
 [ **@collection_set_id =** ] *collection_set_id*  
 Identificateur local unique pour le jeu d'éléments de collecte. *collection_set_id* est **int** avec une sortie et est requis.  
  
 [  **@collection_set_uid =** ] '*collection_set_uid*'  
 GUID pour le jeu d'éléments de collecte. *collection_set_uid* est **uniqueidentifier** avec une sortie avec une valeur par défaut NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_create_collection_set doit être exécuté dans le contexte de la base de données système msdb.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-collection-set-by-using-default-values"></a>A. Création d'un jeu d'éléments de collecte à l'aide de valeurs par défaut  
 L'exemple suivant crée un jeu d'éléments de collecte en spécifiant uniquement les paramètres requis. `@collection_mode` n'est pas nécessaire, mais le mode de collecte par défaut (mis en cache) nécessite de spécifier un ID de planification ou un nom de planification.  
  
```  
USE msdb;  
GO  
DECLARE @collection_set_id int;  
EXECUTE dbo.sp_syscollector_create_collection_set  
    @name = N'Simple collection set test 1',  
    @description = N'This is a test collection set that runs in non-cached mode.',  
    @collection_mode = 1,  
    @collection_set_id = @collection_set_id OUTPUT;  
GO  
```  
  
### <a name="b-creating-a-collection-set-by-using-specified-values"></a>B. Création d'un jeu d'éléments de collecte à l'aide de valeurs spécifiées  
 L'exemple suivant crée un jeu d'éléments de collecte en spécifiant des valeurs pour un grand nombre des paramètres.  
  
```  
USE msdb;  
GO  
DECLARE @collection_set_id int;  
DECLARE @collection_set_uid uniqueidentifier;  
SET @collection_set_uid = NEWID();  
EXEC dbo.sp_syscollector_create_collection_set  
    @name = N'Simple collection set test 2',  
    @collection_mode = 0,  
    @days_until_expiration = 365,  
    @description = N'This is a test collection set that runs in cached mode.',  
    @logging_level = 2,  
    @schedule_name = N'CollectorSchedule_Every_30min',  
    @collection_set_id = @collection_set_id OUTPUT,  
    @collection_set_uid = @collection_set_uid OUTPUT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [Créer un jeu d’éléments de collecte personnalisé qui utilise le type de collecteur Requête T-SQL générique &#40;Transact-SQL&#41;](../../relational-databases/data-collection/create-custom-collection-set-generic-t-sql-query-collector-type.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_collection_sets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)  
  
  
