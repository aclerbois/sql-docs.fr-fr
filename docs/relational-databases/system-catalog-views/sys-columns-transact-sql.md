---
title: Sys.Columns (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.columns_TSQL
- sys.columns
- columns_TSQL
- columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.columns catalog view
ms.assetid: 323ac9ea-fc52-4b8c-8a7e-e0e44f8ed86c
caps.latest.revision: 57
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 09ad6ea6485120184415ffad83e492372587c479
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syscolumns-transact-sql"></a>sys.columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une ligne pour chaque colonne d'un objet comportant des colonnes, telles que des vues ou des tables. Voici une liste de types d'objet comportant des colonnes :  
  
-   Fonctions table d'assembly (FT)  
  
-   Fonctions table inline SQL (IF)  
  
-   Tables internes (IT)  
  
-   Tables système (S)  
  
-   Fonctions table SQL (TF)  
  
-   Tables utilisateur  
  
-   Vues (V)  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificateur de l'objet auquel appartient cette colonne.|  
|name|**sysname**|Nom de la colonne. Unique dans l'objet.|  
|column_id|**int**|ID de la colonne. Unique dans l'objet.<br /><br /> Les ID de colonnes peuvent ne pas être séquentiels.|  
|system_type_id|**tinyint**|ID du type de système de la colonne.|  
|user_type_id|**int**|ID du type de colonne tel que défini par l'utilisateur.<br /><br /> Pour retourner le nom du type, joindre à la [sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) affichage sur cette colonne catalogue.|  
|max_length|**smallint**|Longueur maximale (en octets) de la colonne.<br /><br /> -1 = la colonne est de type de données **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, ou **xml**.<br /><br /> Pour **texte** colonnes, la valeur max_length sera 16 ou la valeur définie par l’option sp_tableoption 'text in row'.|  
|precision|**tinyint**|Précision de la colonne si elle est numérique ; Sinon, 0.|  
|scale|**tinyint**|Échelle de la colonne si elle est numérique ; sinon, la valeur est 0.|  
|collation_name|**sysname**|Nom du classement de la colonne si elle est basée sur les caractères ; Sinon, NULL.|  
|is_nullable|**bit**|1 = La colonne accepte les valeurs NULL.|  
|is_ansi_padded|**bit**|1 = La colonne utilise le comportement ANSI_PADDING ON si elle est de type caractère, binaire ou variant.<br /><br /> 0 = La colonne n'est pas de type caractère, binaire ou variant.|  
|is_rowguidcol|**bit**|1 = La colonne est un ROWGUIDCOL déclaré.|  
|is_identity|**bit**|1 = La colonne a des valeurs d'identité.|  
|is_computed|**bit**|1 = La colonne est calculée.|  
|is_filestream|**bit**|1 = La colonne est une colonne FILESTREAM.|  
|is_replicated|**bit**|1 = La colonne est répliquée.|  
|is_non_sql_subscribed|**bit**|1 = La colonne a un abonné autre que SQL Server.|  
|is_merge_published|**bit**|1 = La colonne est associée à une publication fusionnée.|  
|is_dts_replicated|**bit**|1 = La colonne est répliquée à l'aide de [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|is_xml_document|**bit**|1 = Le contenu est un document XML complet.<br /><br /> 0 = le contenu est un fragment de document ou le type de données de colonne n’est pas **xml**.|  
|xml_collection_id|**int**|Différent de zéro si le type de données de la colonne **xml** et le code XML est tapé. La valeur correspondra à l'ID de la collection qui contient l'espace de noms de schéma XML de validation de la colonne.<br /><br /> 0 = Aucune collection de schéma XML.|  
|default_object_id|**int**|ID de l’objet par défaut, qu’il s’agisse d’un objet autonome [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md), ou une ligne, la contrainte de valeur par défaut au niveau des colonnes. La colonne parent_object_id d'un objet inline par défaut de niveau colonne est une référence à la table elle-même.<br /><br /> 0 = Aucune valeur par défaut.|  
|rule_object_id|**int**|ID de la règle autonome liée à la colonne à l'aide de sys.sp_bindrule.<br /><br /> 0 = Aucune règle autonome. Pour les contraintes de validation au niveau des colonnes, consultez [sys.check_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = La colonne est éparse. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = La colonne est un jeu de colonnes. Pour plus d’informations, consultez [Utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
|generated_always_type|**tinyint**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Identifie la valeur de colonne est générée (sera toujours 0 pour les colonnes dans les tables système) :<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END<br /><br /> Pour plus d’informations, consultez [Tables temporelles &#40;bases de données relationnelles&#41;](../../relational-databases/tables/temporal-tables.md).|  
|generated_always_type_desc|**nvarchar(60)**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Description textuelle du `generated_always_type`de valeur (toujours NOT_APPLICABLE pour les colonnes dans les tables système) <br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
|encryption_type|**int**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Type de chiffrement :<br /><br /> 1 = le chiffrement déterministe<br /><br /> 2 = chiffrement aléatoire|  
|encryption_type_desc|**nvarchar(64)**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Description du type de chiffrement :<br /><br /> ALÉATOIRE<br /><br /> DETERMINISTIC|  
|encryption_algorithm_name|**sysname**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Nom de l’algorithme de chiffrement.<br /><br /> AEAD_AES_256_CBC_HMAC_SHA_512 uniquement est pris en charge.|  
|column_encryption_key_id|**int**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> ID de la clé CEK.|  
|column_encryption_key_database_name|**sysname**|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDW_md](../../includes/sssds-md.md)].<br /><br /> Le nom de la base de données où la clé de chiffrement de colonne existe si différente de la base de données de la colonne. NULL si la clé existe dans la même base de données que la colonne.|  
|is_hidden|**bit**|**S’applique à** : [!INCLUDE[ssCurrentLong](../../includes/sscurrentlong-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Indique si la colonne est masquée :<br /><br /> 0 = la colonne visible, masquée n’est pas régulière<br /><br /> 1 = la colonne masquée|  
|is_masked|**bit**|**S’applique à** : [!INCLUDE[ssCurrentLong](../../includes/sscurrentlong-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Indique si la colonne est masquée par un masquage dynamique des données :<br /><br /> 0 = colonne normal et non masqué<br /><br /> 1 = colonne est masquée.|  


 
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation des catalogues système SQL Server FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [Sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)  
  
  
