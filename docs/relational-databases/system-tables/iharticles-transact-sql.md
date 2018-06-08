---
title: IHarticles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- IHarticles
- IHarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHarticles system table
ms.assetid: 773ef9b7-c993-4629-9516-70c47b9dcf65
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 53542af40c2d5c477b9fbd2ff21467ffd6af69e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="iharticles-transact-sql"></a>IHarticles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **IHarticles** (table système) contient une ligne pour chaque article répliqué à partir un non - éditeur SQL Server à l’aide du serveur de distribution en cours. Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|Colonne d'identité fournissant un numéro d'identification unique pour l'article|  
|**nom**|**sysname**|Nom associé à l'article et unique dans la publication|  
|**publication_id**|**smallint**|Identificateur de la publication à laquelle appartient l'article|  
|**table_id**|**int**|L’ID de la table publiée à partir de [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md).|  
|**publisher_id**|**smallint**|L’ID du serveur de publication Non SQL Server.|  
|**creation_script**|**nvarchar(255)**|Script du schéma de l'article.|  
|**del_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des suppressions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**Filter**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**filter_clause**|**ntext**|Clause WHERE de l'article, utilisée pour le filtrage horizontal et écrite dans une instruction Transact-SQL standard interprétable par le serveur de publication non SQL.|  
|**ins_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des insertions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**pre_creation_cmd**|**tinyint**|Commande à exécuter avant d'appliquer l'instantané initial lorsqu'un objet de même nom existe déjà sur l'Abonné.<br /><br /> **0** = none - une commande n’est pas exécutée.<br /><br /> **1** = DROP : supprimer la table de destination.<br /><br /> **2** = DELETE : supprimer les données de la table de destination.<br /><br /> **3** = TRUNCATE : tronquer la table de destination.|  
|**status**|**tinyint**|Masque de bits de l'état et des options d'article, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **0** = aucune propriété supplémentaire.<br /><br /> **1** = actif.<br /><br /> **8** = inclut le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utilise des instructions paramétrée.<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur 17 dans cette colonne. La valeur 0 signifie que l'article est inactif et qu'aucune propriété supplémentaire n'est définie.|  
|**type**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.|  
|**upd_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des mises à jour avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**binary(8)**|Bitmap de l'option de génération de schéma d'un article donné, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **0 x 00** = désactiver scripts par l’Agent d’instantané et utilise le CreationScript fourni.<br /><br /> **0 x 01** = génère la création d’objets (CREATE TABLE, CREATE PROCEDURE, etc.).<br /><br /> **0 x 10** = génère un index cluster correspondant.<br /><br /> **0 x 40** = générer des index non-cluster correspondants.<br /><br /> **0 x 80** = inclut l’intégrité référentielle déclarée dans les clés primaires.<br /><br /> **0 x 1000** = réplique le classement au niveau des colonnes. Remarque : Cette option a la valeur par défaut pour les serveurs de publication Oracle permettre les comparaisons respectant la casse.<br /><br /> **0 x 4000** = réplique les clés uniques, s’il est défini sur un article de table.<br /><br /> **0 x 8000** = réplique une clé primaire et les clés uniques sur une table de l’article en tant que contraintes à l’aide d’instructions ALTER TABLE.|  
|**dest_owner**|**sysname**|Propriétaire de la table dans la base de données de destination|  
|**dest_table**|**sysname**|Nom de la table de destination|  
|**TABLESPACE_NAME**|**nvarchar(255)**|Identifie l'espace disque logique utilisé par la table d'enregistrement de l'article.|  
|**objid**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**sync_objid**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**description**|**nvarchar(255)**|L’entrée descriptive de l’article.|  
|**publisher_status**|**int**|Permet d’indiquer si la vue qui définit l’article publié a été définie en appelant [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).<br /><br /> **0** = [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) a été appelée.<br /><br /> **1** = [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) n’a pas été appelée.|  
|**article_view_owner**|**nvarchar(255)**|Propriétaire de l'objet de synchronisation sur le serveur de publication utilisé par l'Agent de lecture du journal.|  
|**article_view**|**nvarchar(255)**|Objet de synchronisation sur le serveur de publication utilisé par l'Agent de lecture du journal.|  
|**ins_scripting_proc**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**del_scripting_proc**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**upd_scripting_proc**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**custom_script**|**int**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**fire_triggers_on_snapshot**|**bit**|Cette colonne n’est pas utilisée et qu’il est incluse uniquement à rendre le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) afficher de la **IHarticles** table compatible avec le [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) vue utilisée pour les articles de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**instance_id**|**int**|Identifie l'instance active du journal d'article de la table publiée.|  
|**use_default_datatypes**|**bit**|Indique si l’article utilise les mappages de type de données par défaut ; la valeur **1** indique que les mappages de type de données par défaut sont utilisées.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)  
  
  
