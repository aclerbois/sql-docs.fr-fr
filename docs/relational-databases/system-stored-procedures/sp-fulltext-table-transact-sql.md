---
title: sp_fulltext_table (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b6c759bd422ae815a284ae2cd14bc8b1861281ad
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfulltexttable-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Active ou désactive l'indexation de texte intégral pour une table.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md), et [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@tabname=**] **'***nom_table_qualifée***'**  
 Nom de table en une ou deux parties. La table doit déjà exister dans la base de données actuelle. *nom_table_qualifée* est **nvarchar (517)**, sans valeur par défaut.  
  
 [  **@action=**] **'***action***'**  
 Action à exécuter. *action* est **nvarchar (50)**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Créer**|Crée les métadonnées pour un index de recherche en texte intégral pour la table référencée par *nom_table_qualifée* et spécifie que les données d’index de recherche en texte intégral pour cette table doivent résider dans *fulltext_catalog_name*. Cette opération permet également l’utilisation de *nom_index_unique* en tant que la colonne de clé de recherche en texte intégral. Cet index unique doit déjà exister et être défini sur une colonne de la table.<br /><br /> Une recherche en texte intégral ne peut pas être réalisée vis à vis de cette table tant que le catalogue de texte intégral n'est pas rempli.|  
|**Supprimer**|Supprime les métadonnées de l’index de recherche en texte intégral pour *nom_table_qualifée*. Si cet index est actif, il est automatiquement désactivé avant d'être supprimé. Il n'est pas nécessaire de supprimer les colonnes avant de supprimer l'index de texte intégral.|  
|**Activate**|Active la possibilité pour les données d’index de texte intégral doivent être collectées pour *nom_table_qualifée*, une fois qu’il a été désactivé. Une colonne au moins doit faire partie de l'index de texte intégral pour pouvoir activer cette option.<br /><br /> Un index de texte intégral est automatiquement activé (au niveau du remplissage) dès l'ajout de la première colonne à indexer. Si la dernière colonne est supprimée de l'index, celui-ci devient inactif. Si le suivi des modifications est activé, l'activation d'un index inactif démarre un nouveau remplissage.<br /><br /> Notez que cela n’est pas réellement effectué de l’index de recherche en texte intégral, mais simplement la table est inscrite dans le catalogue de texte intégral dans le système de fichiers afin que les lignes à partir de *nom_table_qualifée* peuvent être récupérées au cours du remplissage d’index de recherche en texte intégral suivant.|  
|**Désactiver**|Désactive l’index de recherche en texte intégral pour *nom_table_qualifée* afin que les données de l’index de recherche en texte intégral n’est plus peuvent être regroupées pour le *nom_table_qualifée*. Les métadonnées de l'index de texte intégral sont conservées et la table peut être réactivée.<br /><br /> Si le suivi des modifications est activé, la désactivation d'un index actif gèle l'état de l'index : tout remplissage en cours est arrêté et aucune modification supplémentaire n'est diffusée à l'index.|  
|**start_change_tracking**|Démarre un remplissage incrémentiel de l'index de texte intégral. Si la table ne dispose pas de données d'une colonne de type timestamp, démarre un remplissage complet de l'index de texte intégral. Démarre le suivi des modifications apportées à la table.<br /><br /> Suivi des modifications de texte intégral ne suit pas toutes les opérations WRITETEXT ou UPDATETEXT effectuées sur les colonnes indexées en texte intégral qui sont de type **image**, **texte**, ou **ntext**.|  
|**stop_change_tracking**|Arrête le suivi des modifications apportées à la table.|  
|**update_index**|Diffuse le jeu courant des modifications suivies à l'index de texte intégral.|  
|**start_background_updateindex**|Démarre la diffusion instantanée des modifications suivies à l'index de texte intégral.|  
|**stop_background_updateindex**|Arrête la diffusion instantanée des modifications suivies à l'index de texte intégral.|  
|**start_full**|Démarre un remplissage complet de l'index de texte intégral de la table.|  
|**start_incremental**|Démarre un remplissage incrémentiel de l'index de texte intégral de la table.|  
|**Arrêter**|Arrête un remplissage complet ou incrémentiel.|  
  
 [  **@ftcat=**] **'***fulltext_catalog_name***'**  
 Est un nom de catalogue de texte intégral existant valide pour un **créer** action. Pour toutes les autres actions, ce paramètre doit avoir la valeur NULL. *fulltext_catalog_name* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@keyname=**] **'***nom_index_unique***'**  
 Est un index valide non null de colonne de clé unique, unique sur *nom_table_qualifée* pour un **créer** action. Pour toutes les autres actions, ce paramètre doit avoir la valeur NULL. *nom_index_unique* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Une fois un index de recherche en texte intégral est désactivé pour une table particulière, l’index de recherche en texte intégral existant reste en place jusqu'à ce que le remplissage complet suivant ; Toutefois, cet index n’est pas utilisé, car [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bloque les requêtes sur les tables désactivées.  
  
 Si la table est réactivée et que l'index n'a pas été rempli à nouveau, l'ancien index est toujours disponible pour les requêtes sur les colonnes qui permettent la recherche en texte intégral et qui existent déjà, à l'exception des nouvelles colonnes. Les données des colonnes supprimées sont récupérables par les requêtes dans lesquelles une recherche en texte intégral sur toute une colonne est spécifiée.  
  
 Après avoir défini une table pour l’indexation de texte intégral, basculement de la colonne clé unique de texte intégral à partir d’un type de données vers un autre, soit en modifiant le type de données de cette colonne ou en modifiant la clé unique de texte intégral d’une colonne à un autre, sans un nouveau remplissage complet peut provoquer un échec se produit pendant une requête et en retournant le message d’erreur : « la Conversion en type *data_type* a échoué pour la valeur de clé de recherche en texte intégral *é chec*. » Pour éviter ce problème, supprimez la définition de la recherche en texte intégral pour cette table à l’aide de la **supprimer** action de **sp_fulltext_table** et redéfinir à l’aide de **sp_fulltext_table** et **sp_fulltext_column**.  
  
 La taille de la colonne clé de texte intégral doit être de 900 octets ou moins. Pour des raisons de performances, il est recommandé de limiter au minimum la taille de la colonne clé.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle de serveur fixe **db_owner** et **db_ddladmin** fixe des rôles de base de données ou d’un utilisateur avec des autorisations sur le catalogue de texte intégral peuvent exécuter la référence **sp_fulltext_table**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>A. Activation d'une table pour l'indexation de texte intégral  
 L'exemple ci-dessous crée des métadonnées d'index de texte intégral pour la table `Document` de la base de données `AdventureWorks`. `Cat_Desc` est un catalogue de texte intégral. `PK_Document_DocumentID` est un index à colonne unique sur `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>B. Activation et propagation du suivi des modifications  
 L'exemple ci-dessous active et commence la propagation des modifications suivies à l'index de texte intégral.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>C. Suppression d'un index de texte intégral  
 L'exemple ci-dessous supprime les métadonnées d'index de texte intégral pour la table `Document` de la base de données `AdventureWorks`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de recherche en texte intégral et la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
