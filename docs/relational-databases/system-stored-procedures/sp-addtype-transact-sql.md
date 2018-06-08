---
title: sp_addtype (Transact-SQL) | Documents Microsoft
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
- sp_addtype
- sp_addtype_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addtype
ms.assetid: ed72cd8e-5ff7-4084-8458-2d8ed279d817
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 96a2d930ae6c85e4da6d516d6c30d6c54a7fd3ac
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddtype-transact-sql"></a>sp_addtype (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un type de données alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addtype [ @typename = ] type,   
    [ @phystype = ] system_data_type   
    [ , [ @nulltype = ] 'null_type' ] ;  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@typename=** ] *type*  
 Nom du type de données alias. Alias des noms de type de données doivent respecter les règles de [identificateurs](../../relational-databases/databases/database-identifiers.md) et doivent être uniques dans chaque base de données. *type* est **sysname**, sans valeur par défaut.  
  
 [  **@phystype=**] *type_données_système*  
 Physique ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fourni, le type de données sur laquelle repose le type de données alias. *type_données_système* est **sysname**, sans valeur par défaut et peut prendre l’une des valeurs suivantes :  
  
||||  
|-|-|-|  
|**bigint**|**binary(n)**|**bit**|  
|**char(n)**|**datetime**|**decimal**|  
|**float**|**image**|**int**|  
|**money**|**nchar(n)**|**ntext**|  
|**numeric**|**nvarchar(n)**|**real**|  
|**smalldatetime**|**smallint**|**smallmoney**|  
|**sql_variant**|**texte**|**tinyint**|  
|**uniqueidentifier**|**varbinary(n)**|**varchar(n)**|  
  
 Les guillemets sont obligatoires pour tous les paramètres comportant des espaces ou des signes de ponctuation. Pour plus d’informations sur les types de données disponibles, consultez [des Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
 *n*  
 Entier non négatif qui indique la longueur du type de données choisi.  
  
 *P*  
 Entier non négatif qui indique le nombre total maximal de chiffres d'un nombre décimal pouvant figurer à gauche et à droite de la virgule décimale. Pour plus d’informations, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 *S*  
 Entier non négatif qui indique le nombre maximal de décimales qui peuvent figurer à droite d'une virgule décimale ; il doit être inférieur ou égal à la précision. Pour plus d’informations, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 [  **@nulltype =** ] **'***type_null***'**  
 Indique la manière dont le type de données alias gère les valeurs NULL. *type_null* est **varchar (** 8 **)**, avec NULL comme valeur par défaut et doit être entouré de guillemets simples ('NULL', 'NOT NULL' ou 'NONULL'). Si *type_null* n’est pas défini explicitement par **sp_addtype**, il a la valeur actuelle. Utilisez la fonction système GETANSINULL pour déterminer la possibilité de valeurs NULL actuellement définie. Il est possible d'ajuster cette valeur au moyen de l'instruction SET ou ALTER DATABASE. La possibilité de valeurs NULL doit être définie explicitement. Si **@phystype** est **bits**, et **@nulltype** n’est pas spécifié, la valeur par défaut n’est pas NULL.  
  
> [!NOTE]  
>  Le *type_null* paramètre définit uniquement la possibilité de valeur NULL par défaut pour ce type de données. Si la possibilité d'utiliser des valeurs NULL est explicitement définie quand le type de données alias est utilisé lors de la création de la table, elle est prioritaire sur la possibilité de valeurs NULL actuellement définie. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41; ](../../t-sql/statements/alter-table-transact-sql.md) et [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Un nom de type de données alias doit être unique dans la base de données. Cependant, plusieurs types de données alias portant des noms différents peuvent avoir la même définition.  
  
 L’exécution de **sp_addtype** crée un type de données alias qui s’affiche dans le **sys.types** affichage pour une base de données spécifique du catalogue. Si le type de données alias doit être disponible dans toutes les nouvelles bases de données défini par l’utilisateur, ajoutez-le à **modèle**. Lorsque qu'un type de données alias est créé, vous pouvez l'utiliser dans CREATE TABLE ou ALTER TABLE, et y lier des valeurs par défaut et des règles. Tous les types de données alias scalaires créés à l’aide de **sp_addtype** sont contenus dans le **dbo** schéma.  
  
 Tous les types de données alias héritent du classement par défaut de la base de données. Les classements des colonnes et les variables des types d’alias sont définis dans le [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TABLE, ALTER TABLE et DECLARE @*local_variable* instructions. La modification du classement par défaut de la base de données s'applique seulement aux nouvelles colonnes et variables de ce type ; elle ne modifie pas le classement des colonnes et des variables existantes.  
  
> [!IMPORTANT]  
>  Pour des raisons de compatibilité descendante, le **public** rôle de base de données est accordé automatiquement l’autorisation REFERENCES sur les types de données alias qui sont créés à l’aide de **sp_addtype**. Remarque Lorsque les types de données alias sont créés à l’aide de l’instruction CREATE TYPE à la place de **sp_addtype**, aucun octroi automatique se produit.  
  
 Types de données alias ne peut pas être définies à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **timestamp**, **table**, **xml**, **varchar (max)**, **nvarchar (max)** ou **varbinary (max)** des types de données.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **db_owner** ou **db_ddladmin** rôle de base de données fixe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-an-alias-data-type-that-does-not-allow-for-null-values"></a>A. Création d'un type de données alias qui n'accepte pas les valeurs NULL  
 L’exemple suivant crée un type de données d’alias nommé `ssn` (numéro de sécurité sociale) basé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-fourni **varchar** type de données. Le type de données `ssn` est utilisé pour les colonnes comportant des numéros de sécurité sociale à 11 chiffres (999-99-9999). Cette colonne ne peut pas avoir la valeur NULL.  
  
 Notez que `varchar(11)` est spécifié entre guillemets car il contient des signes de ponctuation (parenthèses).  
  
```  
USE master;  
GO  
EXEC sp_addtype ssn, 'varchar(11)', 'NOT NULL';  
GO  
```  
  
### <a name="b-creating-an-alias-data-type-that-allows-for-null-values"></a>B. Création d'un type de données alias qui autorise les valeurs NULL  
 L'exemple suivant crée un type de données alias (basé sur `datetime`) appelé `birthday`, qui autorise les valeurs NULL.  
  
```  
USE master;  
GO  
EXEC sp_addtype birthday, datetime, 'NULL';  
```  
  
### <a name="c-creating-additional-alias-data-types"></a>C. Création d'autres types de données alias  
 L'exemple suivant crée deux autres types de données alias, `telephone` et `fax`, pour des numéros de téléphone et de télécopie nationaux et internationaux.  
  
```  
USE master;  
GO  
EXEC sp_addtype telephone, 'varchar(24)', 'NOT NULL';  
GO  
EXEC sp_addtype fax, 'varchar(24)', 'NULL';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_droptype &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droptype-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
