---
title: sysschemaarticles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
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
- sysschemaarticles_TSQL
- sysschemaarticles
dev_langs:
- TSQL
helpviewer_keywords:
- sysschemaarticles system table
ms.assetid: 67a1c039-c283-4a9c-bacc-b9b3973590c3
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d0a1236ed98de14228daa05e6aa0a036cee414ff
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysschemaarticles-transact-sql"></a>sysschemaarticles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet de suivre les articles de schéma uniquement pour des publications transactionnelles et d'instantané. Cette table est stockée dans la base de données de publication.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Identificateur de l'article.|  
|**creation_script**|**nvarchar(255)**|Chemin d'accès et nom d'un script de schéma d'article utilisé pour créer la table cible.|  
|**description**|**nvarchar(255)**|L’entrée descriptive de l’article.|  
|**dest_object**|**sysname**|Nom de l'objet dans la base de données d'abonnement si l'article est un article de schéma uniquement, tel qu'une procédure stockée, vue ou fonction définie par l'utilisateur.|  
|**nom**|**sysname**|Nom de l'article de schéma uniquement dans une publication.|  
|**objid**|**int**|Identificateur de l'objet de base de l'article. Il peut être l’identificateur d’objet d’une procédure, vue indexée, vue ou UDF.|  
|**pubid**|**int**|L’ID de la publication.|  
|**pre_creation_cmd**|**tinyint**|Indique l'action que doit entreprendre le système s'il détecte un objet existant de même nom sur l'Abonné lors de l'application de l'instantané pour cet article :<br /><br /> **0** = nothing.<br /><br /> **1** = supprimer la table de destination.<br /><br /> **2** = drop table de destination.<br /><br /> **3** = destination truncate table.|  
|**status**|**int**|Bitmap utilisé pour indiquer l'état de l'article.|  
|**type**|**tinyint**|Les valeurs indiquant le type d'article de schéma uniquement sont les suivantes :<br /><br /> **32** = procédure stockée.<br /><br /> **64** = vue ou la vue indexée. <br /><br /> **96** = la fonction d’agrégation.<br /><br /> **128** = fonction.|  
|**schema_option**|**binary(8)**|Le masque de bits de l’option de génération de schéma pour l’article donné. Il spécifie la création automatique de la procédure stockée dans la base de données de destination pour toute syntaxe CALL/MCALL/XCALL, et il peut correspondre au résultat OR logique au niveau du bit d'une ou plusieurs des valeurs suivantes :<br /><br /> **0 x 00** = désactive la génération de scripts par l’Agent d’instantané et utilise *creation_script*.<br /><br /> **0 x 01** = génère la création d’objets (CREATE TABLE, CREATE PROCEDURE, etc.). Cette valeur est la valeur par défaut pour les articles de procédure stockée.<br /><br /> **0 x 02** = génère les procédures stockées personnalisées pour l’article, s’il est défini.<br /><br /> **0 x 10** = génère un index cluster correspondant.<br /><br /> **0 x 20** = convertit les types de données définis par l’utilisateur en types de données de base.<br /><br /> **0 x 40**= génère correspondant ou les index non-cluster.<br /><br /> **0 x 80**= inclut l’intégrité référentielle déclarée dans les clés primaires.<br /><br /> **0 x 73** = génère l’instruction CREATE TABLE, crée les index cluster et non-cluster, convertit les types de données définis par l’utilisateur pour les types de données de base et génère des scripts de procédure stockée personnalisée à appliquer sur l’abonné. Cette valeur est la valeur par défaut pour tous les articles, à l'exception des articles de procédure stockée.<br /><br /> **0 x 100**= réplique les déclencheurs utilisateur sur un article de table, s’il est défini.<br /><br /> **0 x 200**= réplique les contraintes de clé étrangères. Si la table référencée ne fait pas partie d'une publication, aucune contrainte de clés étrangères appliquée à une table publiée n'est répliquée.<br /><br /> **0 x 400**= réplique les contraintes de vérification.<br /><br /> **0 x 800**= les valeurs par défaut sont répliquées.<br /><br /> **0 x 1000**= réplique le classement au niveau des colonnes.<br /><br /> **0 x 2000**= réplique les propriétés étendues associées à l’objet de source de l’article publié.<br /><br /> **0 x 4000**= réplique les clés uniques, s’il est défini sur un article de table.<br /><br /> **0 x 8000**= réplique la clé primaire et les clés uniques sur un article de table sous forme de contraintes à l’aide d’instructions ALTER TABLE.|  
|**dest_owner**|**sysname**|Propriétaire de la table dans la base de données de destination|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
