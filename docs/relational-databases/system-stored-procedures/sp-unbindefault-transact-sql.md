---
title: sp_unbindefault (Transact-SQL) | Documents Microsoft
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
- sp_unbindefault_TSQL
- sp_unbindefault
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindefault
ms.assetid: c96a6c5e-f3ca-4c1e-b64b-0d8ef6986af8
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 80453657de70133f269b35387813389ecb7cff0a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spunbindefault-transact-sql"></a>sp_unbindefault (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime la liaison d'une valeur par défaut (ou supprime cette valeur) avec une colonne ou un type de données alias dans la base de données actuelle.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Nous vous recommandons de créer des définitions par défaut à l’aide du mot clé par défaut dans le [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) instructions à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_unbindefault [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@objname=** ] **'***nom_objet***'**  
 Nom de la table et de la colonne ou type de données alias dont la valeur par défaut doit être dissociée. *nom_objet* est **nvarchar(776)**, sans valeur par défaut. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] essaie d'abord de résoudre en noms de colonnes les identificateurs en deux parties, puis en types de données alias.  
  
 Lorsque vous supprimez la liaison d'une valeur par défaut avec un type de données alias, toutes les colonnes de ce type de données ayant la même valeur par défaut sont également dissociées. Les colonnes de ce type de données auxquelles des valeurs par défaut sont directement liées ne sont pas affectées.  
  
> [!NOTE]  
>  *nom_objet* peut contenir des crochets **[]** en tant que la délimitée par des caractères d’identificateur. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
 [  **@futureonly=** ] **'***futureonly_flag***'**  
 S'utilise seulement pour dissocier une valeur par défaut d'un type de données alias. *l’argument futureonly_flag* est **varchar(15)**, avec NULL comme valeur par défaut. Lorsque *futureonly_flag* est **futureonly**, les colonnes existantes du type de données ne perdent pas la valeur par défaut spécifiée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour afficher le texte de la valeur par défaut, exécutez **sp_helptext** par le nom de la valeur par défaut comme paramètre.  
  
## <a name="permissions"></a>Autorisations  
 Pour dissocier une valeur par défaut d'une colonne de table, une autorisation ALTER sur cette table est requise. Pour supprimer la liaison d'une valeur par défaut avec un type de données, il faut disposer d'une autorisation CONTROL sur ce type ou d'une autorisation ALTER sur le schéma auquel appartient ce type.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-unbinding-a-default-from-a-column"></a>A. Suppression de la liaison d'une valeur par défaut avec une colonne  
 Cet exemple supprime la liaison de la valeur par défaut avec la colonne `hiredate` de la table `employees`.  
  
```  
EXEC sp_unbindefault 'employees.hiredate';  
```  
  
### <a name="b-unbinding-a-default-from-an-alias-data-type"></a>B. Suppression de la liaison d'une valeur par défaut avec un type de données alias  
 Cet exemple supprime la liaison de la valeur par défaut avec le type de données alias `ssn`. Il supprime la liaison des colonnes existantes et futures de ce type.  
  
```  
EXEC sp_unbindefault 'ssn';  
```  
  
### <a name="c-using-the-futureonlyflag"></a>C. À l’aide de l’argument futureonly_flag  
 Cet exemple supprime la liaison des utilisations futures du type de données alias `ssn` sans affecter les colonnes existantes de type `ssn`.  
  
```  
EXEC sp_unbindefault 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. À l’aide d’identificateurs délimités  
 L’exemple suivant illustre l’utilisation des identificateurs délimités dans *nom_objet* paramètre.  
  
```  
CREATE TABLE [t.3] (c1 int); -- Notice the period as part of the table   
-- name.  
CREATE DEFAULT default2 AS 0;  
GO  
EXEC sp_bindefault 'default2', '[t.3].c1' ;  
-- The object contains two periods;  
-- the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
EXEC sp_unbindefault '[t.3].c1';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
