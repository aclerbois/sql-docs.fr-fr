---
title: sp_stored_procedures (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_stored_procedures_TSQL
- sp_stored_procedures
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stored_procedures
ms.assetid: fe52dd83-000a-4665-83fb-7a0024193dec
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e4f11ee53e27ba983098d20e6b40ee0c0ef176d7
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spstoredprocedures-transact-sql"></a>sp_stored_procedures (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie la liste des procédures stockées dans l'environnement actuel.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_stored_procedures [ [ @sp_name = ] 'name' ]   
    [ , [ @sp_owner = ] 'schema']   
    [ , [ @sp_qualifier = ] 'qualifier' ]  
    [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@sp_name =** ] **'***nom***'**  
 Nom de la procédure utilisée pour renvoyer des informations de catalogue. *nom* est **390**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [  **@sp_owner =** ] **'***schéma***'**  
 Nom du schéma auquel appartient la procédure. *schéma* est **nvarchar (384)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si *propriétaire* n’est pas spécifié, les règles de visibilité de procédure par défaut du SGBD sous-jacent s’appliquent.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si le schéma actif contient une procédure avec le nom spécifié, celle-ci est renvoyée. Si une procédure stockée non qualifiée est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] la recherche dans l'ordre suivant :  
  
-   Schéma **sys** de la base de données active.  
  
-   Le schéma par défaut de l'appelant est exécuté dans un traitement ou dans des instructions SQL dynamiques ; ou bien, si le nom de la procédure non qualifiée figure dans le corps d'une autre définition de procédure, le schéma qui contient cette procédure est ensuite recherché.  
  
-   Schéma **dbo** dans la base de données active.  
  
 [  **@qualifier =** ] **'***qualificateur***'**  
 Nom du qualificateur de la procédure. *qualificateur* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables dans le formulaire (*qualificateur ***.*** schéma ***.*** nom*. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], *qualificateur* représente le nom de la base de données. Dans certains produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [  **@fUsePattern =** ] **'***fUsePattern***'**  
 Détermine si les caractères de trait de soulignement (_), de pourcentage (%) ou les crochets [ ]) sont interprétés comme des caractères génériques. *fUsePattern* est **bits**, avec 1 comme valeur par défaut.  
  
 **0** = la recherche de correspondance est désactivée.  
  
 **1** = la recherche de correspondance est sur.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nom du qualificateur de procédure. Cette colonne peut être NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nom du propriétaire de la procédure. Cette colonne renvoie toujours une valeur.|  
|**NOM_PROCÉDURE**|**nvarchar(134)**|Nom de la procédure. Cette colonne renvoie toujours une valeur.|  
|**NUM_INPUT_PARAMS**|**int**|Réservé pour un usage ultérieur.|  
|**NUM_OUTPUT_PARAMS**|**int**|Réservé pour un usage ultérieur.|  
|**NUM_RESULT_SETS**|**int**|Réservé pour un usage ultérieur.|  
|**SECTION NOTES**|**varchar(254)**|Description de la procédure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne retourne pas de valeur pour cette colonne.|  
|**PROCEDURE_TYPE**|**smallint**|Type de procédure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne toujours 2.0. Cette valeur peut être une des opérations suivantes :<br /><br /> 0 = SQL_PT_UNKNOWN<br /><br /> 1 = SQL_PT_PROCEDURE<br /><br /> 2 = SQL_PT_FUNCTION|  
  
## <a name="remarks"></a>Notes  
 Pour une interopérabilité maximale, le client de la passerelle doit utiliser seulement les recherches de correspondance avec des caractères génériques de SQL (caractères génériques % et _).  
  
 Les informations d'autorisations relatives à l'exécution par l'utilisateur actuel d'une procédure stockée particulière ne sont pas nécessairement vérifiées ; l'accès n'est donc pas garanti. Notez que la dénomination en trois parties est utilisée. Cela signifie que seules les procédures stockées locales, et non pas les procédures stockées distantes (qui utilisent la dénomination en quatre parties), sont renvoyées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’attribut de serveur ACCESSIBLE_SPROC est Y dans le jeu de résultats pour **sp_server_info**, seules les procédures stockées qui peuvent être exécutés par l’utilisateur actuel sont retournées.  
  
 **sp_stored_procedures** équivaut à **SQLProcedures** dans ODBC. Les résultats obtenus sont triés par **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER**, et **nom_procédure**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-all-stored-procedures-in-the-current-database"></a>A. Renvoi de toutes les procédures stockées présentes dans la base de données active  
 L'exemple suivant retourne toutes les procédures stockées contenues dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_stored_procedures;  
```  
  
### <a name="b-returning-a-single-stored-procedure"></a>B. Renvoi d'une procédure stockée unique  
 L'exemple suivant retourne un jeu de résultats pour la procédure stockée `uspLogError`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_stored_procedures N'uspLogError', N'dbo', N'AdventureWorks2012', 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
