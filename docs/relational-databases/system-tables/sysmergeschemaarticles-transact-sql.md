---
title: sysmergeschemaarticles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
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
- sysmergeschemaarticles_TSQL
- sysmergeschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemaarticles system table
ms.assetid: b5085979-2f76-48e1-bf3b-765a84003dd9
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1a84aa2ef9d3886b5e974bc38ccf3eae18b00c74
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergeschemaarticles-transact-sql"></a>sysmergeschemaarticles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet de suivre les articles de schéma exclusivement associés à la réplication de fusion. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom de l'article de schéma exclusivement associé à la publication de fusion.|  
|**type**|**tinyint**|Type de l'article de schéma exclusivement, pouvant être :<br /><br /> **0 x 20** = article de schéma uniquement de la procédure stockée.<br /><br /> **0 x 40** = article de schéma exclusivement de vue ou l’article de schéma uniquement de la vue indexée.|  
|**objid**|**int**|Identificateur de l'objet de base de l'article. L'identificateur d'objet peut être celui d'une procédure, vue, vue indexée ou fonction définie par l'utilisateur.|  
|**artid**|**uniqueidentifier**|Identificateur de l'article.|  
|**description**|**nvarchar(255)**|Description de l'article.|  
|**pre_creation_command**|**tinyint**|Action par défaut à effectuer lorsque l'article est créé dans la base de données d'abonnement :<br /><br /> **0 =** aucune - si la table existe déjà sur l’abonné, aucune action n’est effectuée.<br /><br /> **1** = Supprimer - supprime la table avant de créer à nouveau.<br /><br /> **2** = supprimer-entraîne une suppression basée sur la clause WHERE dans le filtre de sous-ensemble.<br /><br /> **3** = tronquer-identique **2**, mais supprime des pages au lieu de lignes. Toutefois, n'accepte pas la clause WHERE.|  
|**pubid**|**uniqueidentifier**|Identificateur unique de la publication.|  
|**status**|**tinyint**|État de l'article de schéma exclusivement, pouvant être :<br /><br /> **1** = Unsynced - le script de traitement initial pour publier la table est la prochaine exécution de l’Agent d’instantané.<br /><br /> **2** = active - le script de traitement initial pour publier la table a été exécuté.<br /><br /> **5** = New_inactive - à ajouter.<br /><br /> **6** = New_active - à ajouter.|  
|**creation_script**|**nvarchar(255)**|Chemin d'accès et nom d'un script de précréation de schéma d'article facultatif utilisé pour créer une table cible.|  
|**schema_option**|**binary(8)**|Représentation graphique de l'option de génération de schéma relative à l'article de schéma exclusivement, pouvant être le résultat logique au niveau du bit OU le résultat d'une ou plusieurs des valeurs suivantes :<br /><br /> **0 x 00** = désactiver scripts par l’Agent d’instantané et utilise le CreationScript fourni.<br /><br /> **0 x 01** = génère la création d’objets (CREATE TABLE, CREATE PROCEDURE, etc.).<br /><br /> **0 x 10** = génère un index cluster correspondant.<br /><br /> **0 x 20** = convertir des types de données définis par l’utilisateur en types de données de base.<br /><br /> **0 x 40** = index ou générer des index non cluster correspondant.<br /><br /> **0 x 80** = inclut l’intégrité référentielle déclarée dans les clés primaires.<br /><br /> **0 x 100** = les déclencheurs utilisateur répliqué sur un article de table, s’il est défini.<br /><br /> **0 x 200** = réplication contraintes de clé étrangère. Si la table référencée ne fait pas partie d'une publication, aucune contrainte de clé étrangère appliquée à une table publiée n'est répliquée.<br /><br /> **0 x 400** = des contraintes de validation répliqué.<br /><br /> **0 x 800** = réplication de valeurs par défaut.<br /><br /> **0 x 1000** = le classement au niveau des colonnes répliqué.<br /><br /> **0 x 2000** = réplique les propriétés étendues associées à l’objet de source de l’article publié.<br /><br /> **0 x 4000** = réplique les clés uniques, s’il est défini sur un article de table.<br /><br /> **0 x 8000** = réplique une clé primaire et les clés uniques sur une table de l’article en tant que contraintes à l’aide d’instructions ALTER TABLE.<br /><br /> Pour plus d’informations sur les valeurs possibles pour **schema_option**, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nom de l'objet de destination dans la base de données d'abonnement. Cette valeur s'applique uniquement aux articles de schéma exclusivement, tels que les procédures stockées, les vues et les fonctions définies par l'utilisateur.|  
|**destination_owner**|**sysname**|Le propriétaire de l’objet dans la base d’abonnement, si elle n’est pas **dbo**.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
