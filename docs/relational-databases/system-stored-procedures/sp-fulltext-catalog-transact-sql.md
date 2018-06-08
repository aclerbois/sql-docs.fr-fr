---
title: sp_fulltext_catalog (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fulltext_catalog_TSQL
- sp_fulltext_catalog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_catalog
ms.assetid: e49b98e4-d1f1-42b2-b16f-eb2fc7aa1cf5
caps.latest.revision: 37
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: fccb384317102ef2818a49ef09c5faeb0fc42eb1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfulltextcatalog-transact-sql"></a>sp_fulltext_catalog (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Permet de créer ou de supprimer un catalogue de texte intégral, et de démarrer ou d'arrêter l'indexation d'un catalogue. Plusieurs catalogues de texte intégral peuvent être créés pour chaque base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md), [ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md), et [DROP FULLTEXT CATALOG](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_fulltext_catalog [ @ftcat= ] 'fulltext_catalog_name' ,   
     [ @action= ] 'action'   
     [ , [ @path= ] 'root_directory' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@ftcat=**] **'***fulltext_catalog_name***'**  
 Nom du catalogue de texte intégral. Les noms de catalogues doivent être uniques dans chaque base de données. *fulltext_catalog_name* est **sysname**.  
  
 [  **@action=**] **'***action***'**  
 Action à exécuter. *action* est **varchar (20)**, et peut prendre l’une des valeurs suivantes.  
  
> [!NOTE]  
>  En fonction de vos besoins, vous pouvez créer, supprimer et modifier des catalogues de texte intégral. Toutefois, il vaut mieux éviter de procéder à des modifications de schéma sur plusieurs catalogues à la fois. Ces actions peuvent être effectuées à l’aide de la **sp_fulltext_table** procédure stockée, qui est la méthode recommandée.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Créer**|Crée un catalogue de texte intégral vide dans le système de fichiers et ajoute une ligne correspondante dans **sysfulltextcatalogs** avec la *fulltext_catalog_name* et *root_directory*, le cas échéant, les valeurs. *fulltext_catalog_name* doit être unique dans la base de données.|  
|**Supprimer**|Supprime *fulltext_catalog_name* en le supprimant du système de fichiers, en supprimant la ligne correspondante dans **sysfulltextcatalogs**. Cette action échoue si le catalogue contient des index pour une ou plusieurs tables. **sp_fulltext_table** '*table_name*', 'drop' doit être exécutée pour supprimer les tables du catalogue.<br /><br /> Une erreur est affichée si le catalogue n'existe pas.|  
|**start_incremental**|Démarre un remplissage incrémentiel *fulltext_catalog_name*. Une erreur est affichée si le catalogue n'existe pas. Si une alimentation d'index de recherche en texte intégral est déjà active, un avertissement est affiché, mais aucune opération d'alimentation ne se produit. Avec un remplissage incrémentiel uniquement les lignes modifiées sont récupérées pour l’indexation de texte intégral, si tant est un **timestamp** les colonnes présentes dans la table en cours de recherche en texte intégral indexées.|  
|**start_full**|Démarre un remplissage complet pour *fulltext_catalog_name*. Même si elle a déjà été indexée, chaque ligne de chaque table associée à ce catalogue de texte intégral est récupérée lors de l'indexation en texte intégral.|  
|**Arrêter**|Arrête un remplissage d’index pour *fulltext_catalog_name*. Une erreur est affichée si le catalogue n'existe pas. Aucun avertissement n'est affiché si l'alimentation était déjà arrêtée.|  
|**Reconstruire**|Reconstruit *fulltext_catalog_name*. Dans ce cas, le catalogue existant est supprimé et un autre catalogue est créé à sa place. Toutes les tables qui comportent des références d'indexation de texte intégral sont associées au nouveau catalogue. La reconstruction redéfinit les métadonnées de texte intégral des tables système de la base de données.<br /><br /> Si le suivi des modifications est désactivé (OFF), la reconstruction ne déclenche pas de réalimentation du catalogue de texte intégral récemment créé. Dans ce cas, pour réalimenter, exécutez **sp_fulltext_catalog** avec la **start_full** ou **start_incremental** action.|  
  
 [  **@path=**] **'***root_directory***'**  
 Est le répertoire racine (pas le chemin physique complet) pour un **créer** action. *root_directory* est **nvarchar (100)** et a la valeur par défaut NULL, ce qui indique l’utilisation de l’emplacement par défaut spécifié lors de l’installation. Il s’agit du sous-répertoire Ftdata situé dans le répertoire Mssql ; par exemple, C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\MSSQL\FTData. Le répertoire racine spécifié doit se trouver sur un lecteur du même ordinateur, ne pas être désigné seulement par une lettre de lecteur et ne pas être un chemin d'accès relatif. Les disques réseau et amovibles, les disquettes et chemins UNC ne sont pas pris en charge. Les catalogues de texte intégral doivent être créés sur un lecteur de disque local associé à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **@path** est valide uniquement lorsque *action* est **créer**. Pour les actions autres que **créer** (**arrêter**, **reconstruire**, et ainsi de suite), **@path** doit être NULL ou omis.  
  
 Si l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un serveur virtuel de cluster, le répertoire de catalogue spécifié doit être installé sur un lecteur de disque partagé dont la ressource [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dépend. Si @path n’est pas spécifié, l’emplacement du répertoire de catalogue par défaut est sur le lecteur de disque partagé dans le répertoire qui a été spécifié lors de l’installation du serveur virtuel.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Le **start_full** action est utilisée pour créer un instantané complet des données de recherche en texte intégral dans *fulltext_catalog_name*. Le **start_incremental** action permet de réindexer uniquement les lignes modifiées dans la base de données. Alimentation incrémentielle peut être appliquée uniquement si la table possède une colonne de type **timestamp**. Si une table dans le catalogue de texte intégral ne contient pas une colonne de type **timestamp**, la table subit un remplissage complet.  
  
 Les données d'index et de catalogue de texte intégral sont enregistrées dans des fichiers créés dans un répertoire du catalogue de texte intégral. Le répertoire de catalogue de texte intégral est créé en tant que sous-répertoire du répertoire spécifié dans **@path** ou dans le répertoire du catalogue de texte intégral par défaut serveur si **@path** n’est pas spécifié. Le nom du répertoire du catalogue de texte intégral est créé de telle façon qu'il est unique sur le serveur. Par conséquent, tous les répertoires de catalogue de texte intégral peuvent partager le même chemin.  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être membre de la **db_owner** rôle. Selon l’action demandée, l’appelant doit disposer des autorisations ALTER ou CONTROL (qui **db_owner** a) sur le catalogue de texte intégral cible.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-create-a-full-text-catalog"></a>A. Création d'un catalogue de texte intégral  
 Cet exemple crée un catalogue de texte intégral vide, **Cat_Desc**, dans le **AdventureWorks2012** base de données.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'create';  
GO  
```  
  
### <a name="b-to-rebuild-a-full-text-catalog"></a>B. Pour reconstruire un catalogue de texte intégral  
 Cet exemple reconstruit un catalogue de texte intégral existant, **Cat_Desc**, dans le **AdventureWorks2012** base de données.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'rebuild';  
GO  
```  
  
### <a name="c-start-the-population-of-a-full-text-catalog"></a>C. Démarrage de l'alimentation d'un catalogue de texte intégral  
 Cet exemple démarre un remplissage complet de le **Cat_Desc** catalogue.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'start_full';  
GO  
```  
  
### <a name="d-stop-the-population-of-a-full-text-catalog"></a>D. Arrêt de l'alimentation d'un catalogue de texte intégral  
 Cet exemple arrête le remplissage de la **Cat_Desc** catalogue.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'stop';  
GO  
```  
  
### <a name="e-to-remove-a-full-text-catalog"></a>E. Suppression d'un catalogue de texte intégral  
 L’exemple suivant supprime le **Cat_Desc** catalogue.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_catalog 'Cat_Desc', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [FULLTEXTCATALOGPROPERTY & #40 ; Transact-SQL & #41 ;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)   
 [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)   
 [sp_help_fulltext_catalogs_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
