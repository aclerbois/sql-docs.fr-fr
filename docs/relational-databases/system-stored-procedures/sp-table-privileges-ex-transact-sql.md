---
title: sp_table_privileges_ex (Transact-SQL) | Documents Microsoft
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
- sp_table_privileges_ex
- sp_table_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges_ex
ms.assetid: b58d4a07-5c40-4f17-b66e-6d6b17188dda
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: fc97335ccb0e014e130a2f47c70f480dd7c23554
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptableprivilegesex-transact-sql"></a>sp_table_privileges_ex (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les privilèges relatifs à la table spécifiée provenant du serveur lié indiqué.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_table_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
     [ , [@fUsePattern =] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table_server =** ] **'***serveur_de_la_table***'**  
 Nom du serveur lié pour lequel renvoyer les informations. *serveur_de_la_table* est **sysname**, sans valeur par défaut.  
  
 [  **@table_name =** ] **'***table_name***'**]  
 Nom de la table pour laquelle les informations de privilège de table sont demandées. *nom_table* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@table_schema =** ] **'***table_schema***'**  
 Schéma de la table. Propriétaire de la table dans certains environnements SGBD. *table_schema* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@table_catalog =** ] **'***table_catalog***'**  
 Nom de la base de données dans laquelle le texte spécifié *table_name* réside. *table_catalog* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@fUsePattern =**] **'***fUsePattern***'**  
 Détermine si les caractères « _ », « % », « [ » et « ] » doivent être interprétés en tant que caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nom du qualificateur de table. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualificateur ***.*** propriétaire ***.*** nom de*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table. Ce champ peut contenir la valeur NULL.|  
|**TABLE_SCHEM**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**NOM_TABLE**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**FOURNISSEUR D’AUTORISATIONS**|**sysname**|Nom d’utilisateur de base de données qui a accordé les autorisations sur ce **TABLE_NAME** à la liste **bénéficiaire**. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne est toujours le même que le **TABLE_OWNER**. Ce champ retourne toujours une valeur. En outre, la colonne GRANTOR peut être soit le propriétaire de la base de données (**TABLE_OWNER**) ou un utilisateur auquel le propriétaire de la base de données a accordé des autorisations à l’aide de la clause WITH GRANT OPTION de l’instruction GRANT.|  
|**BÉNÉFICIAIRE**|**sysname**|Nom d’utilisateur de base de données qui a été accordé des autorisations sur **TABLE_NAME** par le **GRANTOR**. Ce champ retourne toujours une valeur.|  
|**PRIVILÈGE**|**varchar(** 32 **)**|L'une des autorisations disponibles sur la table. Les autorisations sur les tables peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source des données si leur implémentation est définie) :<br /><br /> Sélectionnez = **bénéficiaire** peut récupérer des données pour un ou plusieurs des colonnes.<br /><br /> INSERT = **bénéficiaire** peut fournir des données pour les nouvelles lignes pour un ou plusieurs des colonnes.<br /><br /> Mise à jour = **bénéficiaire** peut modifier des données existantes pour un ou plusieurs des colonnes.<br /><br /> DELETE = **bénéficiaire** peut supprimer des lignes de la table.<br /><br /> RÉFÉRENCES = **bénéficiaire** peut faire référence à une colonne dans une table étrangère dans une relation de clé étrangère/clé primaire. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les relations clé primaire/clé étrangère sont définies grâce à des contraintes de table.<br /><br /> Le rayon d’action pour le **bénéficiaire** par une table spécifique privilège est dépendante de la source de données. Par exemple, l’autorisation de mise à jour pourrait permettre la **bénéficiaire** pour mettre à jour toutes les colonnes d’une table pour une source de données et uniquement les colonnes pour lesquelles le **GRANTOR** a l’autorisation de mise à jour sur une autre source de données.|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|Indique si le **bénéficiaire** est autorisé à accorder des autorisations à d’autres utilisateurs. On appelle habituellement ce mécanisme « transmission des droits ». Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue (ou NULL) fait référence à une source de données où la « transmission des droits » ne s'applique pas.|  
  
## <a name="remarks"></a>Notes  
 Les résultats obtenus sont triés par **TABLE_QUALIFIER**, **TABLE_OWNER**, **TABLE_NAME**, et **privilège**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations relatives aux privilèges des tables dont le nom commence par `Product` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] résidant sur le serveur lié `Seattle1` ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est considéré comme étant le serveur lié).  
  
```  
EXEC sp_table_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Product%',   
   @table_schema = 'Production',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_column_privileges_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-ex-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Distributed des procédures stockées de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  
