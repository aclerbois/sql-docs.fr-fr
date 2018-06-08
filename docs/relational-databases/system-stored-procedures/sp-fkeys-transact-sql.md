---
title: sp_fkeys (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fkeys
- sp_fkeys_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fkeys
ms.assetid: 18110444-d38d-4cff-90d2-d1fc6236668b
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: fdb582cf8e77e61d7723ea1c6ed2e854ef8f6940
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfkeys-transact-sql"></a>sp_fkeys (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des informations sur les clés étrangères logiques pour l'environnement actuel. Cette procédure montre les relations de clés étrangères comprenant des clés étrangères désactivées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_fkeys [ @pktable_name = ] 'pktable_name'   
     [ , [ @pktable_owner = ] 'pktable_owner' ]   
     [ , [ @pktable_qualifier = ] 'pktable_qualifier' ]   
     { , [ @fktable_name = ] 'fktable_name' }   
     [ , [ @fktable_owner = ] 'fktable_owner' ]   
     [ , [ @fktable_qualifier = ] 'fktable_qualifier' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @pktable_name=] '*nom_de_la_pktable*'  
 Nom de la table, avec la clé primaire, utilisée pour retourner les informations de catalogue. *nom_de_la_pktable* est **sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Ce paramètre ou le *fktable_name* paramètre, ou les deux, doivent être fournis.  
  
 [ @pktable_owner=] '*nom_propriétaire_pk*'  
 Est le nom du propriétaire de la table (contenant la clé primaire) utilisée pour retourner des informations de catalogue. *nom_propriétaire_pk* est **sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *nom_propriétaire_pk* n’est pas spécifié, les règles de visibilité de table par défaut du SGBD sous-jacent s’appliquent.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l'utilisateur actuel possède une table ayant le nom spécifié, ce sont les colonnes de cette table qui sont retournées. Si *nom_propriétaire_pk* n’est pas spécifié et l’utilisateur actuel ne possède pas d’une table avec l’objet *nom_de_la_pktable*, la procédure recherche une table avec l’objet *nom_de_la_pktable* appartenant au propriétaire de la base de données. S'il en existe une, les colonnes de cette table sont retournées.  
  
 [ @pktable_qualifier =] '*l’argument identificateur_table_fk*'  
 Est le nom du qualificateur de table (contenant la clé primaire). *l’argument identificateur_table_fk* est de type sysname, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualifier.owner.name*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le qualificateur représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [ @fktable_name=] '*fktable_name*'  
 Nom de la table (contenant une clé étrangère) utilisée pour retourner les informations de catalogue. *FKTABLE_NAME* est de type sysname, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Ce paramètre ou le *nom_de_la_pktable* paramètre, ou les deux, doivent être fournis.  
  
 [ @fktable_owner =] '*fktable_owner*'  
 Nom du propriétaire de la table (contenant une clé étrangère) utilisée pour retourner les informations de catalogue. *fktable_owner* est **sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *fktable_owner* n’est pas spécifié, les règles de visibilité de table par défaut du SGBD sous-jacent s’appliquent.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l'utilisateur actuel possède une table ayant le nom spécifié, ce sont les colonnes de cette table qui sont retournées. Si *fktable_owner* n’est pas spécifié et l’utilisateur actuel ne possède pas d’une table avec l’objet *fktable_name*, la procédure recherche une table avec l’objet *fktable_name* appartenant au propriétaire de la base de données. S'il en existe une, les colonnes de cette table sont retournées.  
  
 [ @fktable_qualifier=] '*fktable_qualifier*'  
 Est le nom du qualificateur de table (contenant une clé étrangère). *FKTABLE_QUALIFIER* est **sysname**, avec NULL comme valeur par défaut. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le qualificateur représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|PKTABLE_QUALIFIER|**sysname**|Nom du qualificateur de la table (où figure la clé primaire). Ce champ peut contenir la valeur NULL.|  
|PKTABLE_OWNER|**sysname**|Nom du propriétaire de la table (où figure la clé primaire). Ce champ retourne toujours une valeur.|  
|PKTABLE_NAME|**sysname**|Nom de la table (contenant la clé primaire). Ce champ retourne toujours une valeur.|  
|PKCOLUMN_NAME|**sysname**|Nom des colonnes clés primaires, pour chaque colonne de la table TABLE_NAME retournée. Ce champ retourne toujours une valeur.|  
|FKTABLE_QUALIFIER|**sysname**|Nom du qualificateur de la table (contenant une clé étrangère). Ce champ peut contenir la valeur NULL.|  
|FKTABLE_OWNER|**sysname**|Nom du propriétaire de la table (contenant une clé étrangère). Ce champ retourne toujours une valeur.|  
|FKTABLE_NAME|**sysname**|Nom de la table (contenant une clé étrangère). Ce champ retourne toujours une valeur.|  
|FKCOLUMN_NAME|**sysname**|Nom de la colonne clé étrangère, pour chaque colonne de la table TABLE_NAME retournée. Ce champ retourne toujours une valeur.|  
|KEY_SEQ|**smallint**|Numéro de séquence de la colonne dans une clé primaire multicolonne. Ce champ retourne toujours une valeur.|  
|UPDATE_RULE|**smallint**|Action appliquée à la clé étrangère lorsque l'opération SQL est une mise à jour.  Valeurs possibles :<br /> 0=Modifications de type CASCADE apportées à la clé étrangère.<br /> 1=Modifications de type NO ACTION en présence de clé étrangère.<br />   2 = null du jeu <br /> 3 = définir par défaut |  
|DELETE_RULE|**smallint**|Action appliquée à la clé étrangère lorsque l'opération SQL est une suppression. Valeurs possibles :<br /> 0=Modifications de type CASCADE apportées à la clé étrangère.<br /> 1=Modifications de type NO ACTION en présence de clé étrangère.<br />   2 = null du jeu <br /> 3 = définir par défaut |  
|FK_NAME|**sysname**|Identificateur de clé étrangère. NULL si non applicable à la source de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne le nom de la contrainte FOREIGN KEY.|  
|PK_NAME|**sysname**|Identificateur de clé primaire. NULL si non applicable à la source de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne le nom de la contrainte PRIMARY KEY.|  
  
 Les résultats retournés sont classés par FKTABLE_QUALIFIER, FKTABLE_OWNER, FKTABLE_NAME et KEY_SEQ.  
  
## <a name="remarks"></a>Notes  
 Si le code de votre application fait référence à des tables avec des clés étrangères désactivées, vous pouvez l'implémenter en utilisant l'une des méthodes suivantes :  
  
-   Désactiver temporairement la vérification des contraintes (ALTER TABLE NOCHECK ou CREATE TABLE NOT FOR REPLICATION) pendant que vous travaillez sur les tables, et la réactiver par la suite.  
  
-   Utiliser les déclencheurs ou le code de l'application pour assurer l'intégrité des relations.  
  
Si le nom de la table de clé primaire est spécifié et que le nom de la table de clé étrangère est NULL, la procédure sp_fkeys retourne toutes les tables qui contiennent une clé étrangère faisant référence à la table donnée. Si le nom de la table de clé étrangère est spécifié et que le nom de la table de clé primaire est NULL, la procédure sp_fkeys retourne toutes les tables liées par une relation clé primaire/clé étrangère aux clés étrangères qui se trouvent dans la table de clé étrangère.  
  
La procédure stockée sp_fkeys est équivalente à SQLForeignKeys dans ODBC.  
  
## <a name="permissions"></a>Autorisations  
 Requiert `SELECT` autorisation sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant extrait une liste de clés étrangères pour la table `HumanResources.Department` dans la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_fkeys @pktable_name = N'Department'  
    ,@pktable_owner = N'HumanResources';  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'exemple suivant extrait une liste de clés étrangères pour la table `DimDate` dans la base de données `AdventureWorksPDW2012`. Ne retourne aucune ligne, car [!INCLUDE[ssDW](../../includes/ssdw-md.md)] ne prend pas en charge les clés étrangères.  
  
```sql  
EXEC sp_fkeys @pktable_name = N'DimDate;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_pkeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-pkeys-transact-sql.md)  
  
  

