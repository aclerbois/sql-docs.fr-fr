---
title: sp_syscollector_update_collection_set (Transact-SQL) | Documents Microsoft
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
- sp_syscollector_update_collection_set_TSQL
- sp_syscollector_update_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_update_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 2dccc3cd-0e93-4e3e-a4e5-8fe89b31bd63
caps.latest.revision: 28
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 62867f22c044a42c40499e0a1143557621931db8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyscollectorupdatecollectionset-transact-sql"></a>sp_syscollector_update_collection_set (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet de modifier les propriétés d'un jeu d'éléments de collecte définis par l'utilisateur ou de le renommer.  
  
> [!WARNING]  
>  Dans les cas où le compte Windows configuré en tant que proxy est non interactif ou correspond à un utilisateur interactif qui ne s'est pas encore connecté, le répertoire de profils n'existera pas et la création du répertoire intermédiaire échouera. Par conséquent, si vous utilisez un compte proxy sur un contrôleur de domaine, vous devez spécifier un compte interactif qui a été utilisé au moins une fois afin de garantir que le répertoire de profils a été créé.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_update_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @new_name = ] 'new_name' ]  
    , [ [ @target = ] 'target' ]  
    , [ [ @collection_mode = ] collection_mode ]  
    , [ [ @days_until_expiration = ] days_until_expiration ]  
    , [ [ @proxy_id = ] proxy_id ]  
    , [ [ @proxy_name = ] 'proxy_name' ]  
    ,[ [ @schedule_uid = ] 'schedule_uid' ]  
    ,[ [ @schedule_name = ] 'schedule_uid' ]  
    , [ [ @logging_level = ] logging_level ]  
    , [ [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@collection_set_id =** ] *collection_set_id*  
 Identificateur local unique pour le jeu d'éléments de collecte. *collection_set_id* est **int** et doit avoir une valeur si *nom* est NULL.  
  
 [  **@name =** ] '*nom*'  
 Est le nom de l’ensemble de la collection. *nom* est **sysname** et doit avoir une valeur si *collection_set_id* a la valeur NULL.  
  
 [  **@new_name =** ] '*nouveau_nom*'  
 Nouveau nom pour le jeu d'éléments de collecte. *nouveau_nom* est **sysname**, et si utilisé, ne peut pas être une chaîne vide. *nouveau_nom* doit être unique. Pour obtenir une liste de noms de jeux d'éléments de collecte actuels, interrogez la vue système syscollector_collection_sets.  
  
 [  **@target =** ] '*cible*'  
 Réservé pour un usage ultérieur.  
  
 [  **@collection_mode =** ] *collection_mode*  
 Collection de type de données à utiliser. *collection_mode* est **smallint** et peut avoir l’une des valeurs suivantes :  
  
 0 - Mode mis en cache. La collecte et le téléchargement de données sont sur des planifications séparées. Spécifiez le mode mis en cache pour la collecte continue.  
  
 1 - Mode non mis en cache. Collecte de données et le téléchargement est sur la même planification. Spécifiez le mode non mis en cache pour une collecte ad hoc ou par instantané.  
  
 Si le changement de mode non mis en cache en mode mis en cache (0), vous devez également spécifier *schedule_uid* ou *nom_de_la_planification*.  
  
 [  **@days_until_expiration=** ] *days_until_expiration*  
 Est le nombre de jours pendant lesquels les données collectées sont enregistrées dans l’entrepôt de données de gestion. *days_until_expiration* est **smallint**. *days_until_expiration* doit être 0 ou un entier positif.  
  
 [  **@proxy_id =** ] *proxy_id*  
 Identificateur unique pour un compte d'Agent proxy [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *proxy_id* est **int**.  
  
 [  **@proxy_name =** ] '*proxy_name*'  
 Nouveau nom du proxy. *proxy_name* est **sysname** et accepte les valeurs NULL.  
  
 [ **@schedule_uid** =] '*schedule_uid*'  
 GUID qui pointe vers une planification. *schedule_uid* est **uniqueidentifier**.  
  
 Pour obtenir des *schedule_uid*, interrogez la table système sysschedules.  
  
 Lorsque *collection_mode* est définie sur 0, *schedule_uid* ou *nom_de_la_planification* doit être spécifié. Lorsque *collection_mode* est définie sur 1, *schedule_uid* ou *nom_de_la_planification* est ignoré s’il est spécifié.  
  
 [  **@schedule_name =** ] '*nom_de_la_planification*'  
 Est le nom de la planification. *nom_de_la_planification* est **sysname** et accepte les valeurs NULL. Si spécifié, *schedule_uid* doit être NULL. Pour obtenir des *nom_de_la_planification*, interrogez la table système sysschedules.  
  
 [  **@logging_level =** ] *logging_level*  
 Niveau de journalisation. *logging_level* est **smallint** avec l’une des valeurs suivantes :  
  
 0 - Informations de l'exécution du journal et événements [!INCLUDE[ssIS](../../includes/ssis-md.md)] qui effectuent le suivi des éléments suivants :  
  
-   Démarrage/arrêt de jeux d'éléments de collecte  
  
-   Démarrage/arrêt de packages  
  
-   Informations sur l'erreur  
  
 1 - enregistrement de niveau 0 et :  
  
-   Statistiques d'exécution  
  
-   Progression de la collecte continuellement en cours d'exécution  
  
-   Événements d'avertissements de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 2 - Enregistrement de niveau 1 et informations sur l'événement détaillées de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 La valeur par défaut *logging_level* est 1.  
  
 [  **@description =** ] '*description*'  
 Description du jeu d'éléments de collecte. *Description* est **nvarchar (4000)**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_update_collection_set doit être exécuté dans le contexte de la base de données système msdb.  
  
 Soit *collection_set_id* ou *nom* doit avoir une valeur, les deux ne peut pas être NULL. Pour obtenir ces valeurs, interrogez la vue système syscollector_collection_sets.  
  
 Si l’ensemble de la collection est en cours d’exécution, vous pouvez uniquement mettre à jour *schedule_uid* et *description*. Pour arrêter l’ensemble de la collection, utilisez [sp_syscollector_stop_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-stop-collection-set-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin ou dc_operator (avec autorisation EXECUTE) pour exécuter cette procédure. Même si dc_operator peut exécuter cette procédure stockée, les membres de ce rôle sont limités en ce qui concerne les propriétés qu'ils peuvent modifier. Les propriétés suivantes peuvent être modifiées uniquement par dc_admin :  
  
-   @new_name  
  
-   @target  
  
-   @proxy_id  
  
-   @description  
  
-   @collection_mode  
  
-   @days_until_expiration  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-renaming-a-collection-set"></a>A. Modification du nom d'un jeu d'éléments de collecte  
 L'exemple suivant renomme un jeu d'éléments de collecte définis par l'utilisateur :  
  
```  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_update_collection_set  
@name = N'Simple collection set test 1',  
@new_name = N'Collection set test 1 in cached mode';  
GO  
```  
  
### <a name="b-changing-the-collection-mode-from-non-cached-to-cached"></a>B. Passage du mode de collecte non mis en cache au mode de collecte mis en cache  
 L'exemple suivant passe du mode de collecte non mis en cache au mode de collecte mis en cache. Cette modification nécessite de spécifier un ID ou un nom de planification.  
  
```  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_update_collection_set  
@name = N'Collection set test 1 in cached mode',  
@collection_mode = 0,  
@schedule_uid = 'C7022AF3-51B8-4011-B159-64C47C88FF70';  
-- alternatively, use @schedule_name.  
-- @schedule_name = N'CollectorSchedule_Every_15min;  
GO  
```  
  
### <a name="c-changing-other-collection-set-parameters"></a>C. Modification d'autres paramètres de jeu d'éléments de collecte  
 L'exemple suivant met à jour différentes propriétés du jeu d'éléments de collecte appelé « Simple collection set test 2 » :  
  
```  
USE msdb;  
GO  
EXEC dbo.sp_syscollector_update_collection_set  
@name = N'Simple collection set test 2',  
@collection_mode = 1,  
@days_until_expiration = 5,  
@description = N'This is a test collection set that runs in noncached mode.',  
@logging_level = 0;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [syscollector_collection_sets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)   
 [dbo.sysschedules &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
  
  
