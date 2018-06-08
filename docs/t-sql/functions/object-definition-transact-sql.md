---
title: OBJECT_DEFINITION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- OBJECT_DEFINITION_TSQL
- OBJECT_DEFINITION
dev_langs:
- TSQL
helpviewer_keywords:
- viewing source text
- source text [SQL Server]
- displaying source text
- OBJECT_DEFINITION function
ms.assetid: 2ac837c7-eca9-4d29-b06e-72e30450c68d
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4da307c294b754ec58fe4b68fd1bdbae8c35af07
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="objectdefinition-transact-sql"></a>OBJECT_DEFINITION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie le texte source [!INCLUDE[tsql](../../includes/tsql-md.md)] de la définition de l'objet spécifié.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
OBJECT_DEFINITION ( object_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *object_id*  
 ID de l'objet à utiliser. *object_id* est de type **int** et est censé représenter un objet dans le contexte de la base de données active.  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar(max)**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
 Un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'une autorisation. Cela signifie que les fonctions intégrées générant des métadonnées, telles que OBJECT_DEFINITION, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Notes   
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] considère que *object_id* se situe dans le contexte de la base de données active. Le classement de la définition de l'objet correspond toujours au contexte de la base de données d'appel.  
  
 OBJECT_DEFINITION s'applique aux types d'objets suivants :  
  
-   C = Contrainte CHECK  
  
-   D = Valeur par défaut (contrainte ou autonome)  
  
-   P = Procédure stockée SQL  
  
-   FN = Fonction scalaire SQL  
  
-   R = Règle  
  
-   RF = Procédure de filtre de réplication  
  
-   TR = Déclencheur SQL (déclencheur DML avec une étendue de schéma ou déclencheur DDL avec une étendue de base de données ou de serveur)  
  
-   IF = Fonction table en ligne SQL  
  
-   TF = Fonction table SQL  
  
-   V = Vue  
  
## <a name="permissions"></a>Autorisations  
 Les définitions de l'objet système sont visibles publiquement. La définition des objets utilisateur est visible par le propriétaire de l'objet ou les bénéficiaires de l'une des autorisations suivantes : ALTER, CONTROL, TAKE OWNERSHIP ou VIEW DEFINITION. Ces autorisations sont implicitement possédées par des membres des rôles de base de données fixes **db_owner**, **db_ddladmin**et **db_securityadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-source-text-of-a-user-defined-object"></a>A. Renvoi du texte source d'un objet défini par l'utilisateur  
 L'exemple suivant renvoie la définition d'un déclencheur défini par l'utilisateur, `uAddress`, dans le schéma `Person`. La fonction intégrée `OBJECT_ID` est utilisée pour renvoyer l'identificateur d'objet du déclencheur de l'instruction `OBJECT_DEFINITION`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID(N'Person.uAddress')) AS [Trigger Definition];   
GO  
```  
  
### <a name="b-returning-the-source-text-of-a-system-object"></a>B. Renvoi du texte source d'un objet système  
 L'exemple suivant renvoie la définition de la procédure stockée système `sys.sp_columns`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID(N'sys.sp_columns')) AS [Object Definition];  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)  
  
  
