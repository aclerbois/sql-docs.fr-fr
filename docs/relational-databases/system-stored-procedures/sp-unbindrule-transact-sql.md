---
title: sp_unbindrule (Transact-SQL) | Documents Microsoft
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
- sp_unbindrule_TSQL
- sp_unbindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindrule
ms.assetid: f54ee155-c3c9-4f1a-952e-632a8339f0cc
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: be14a4885cea481edda6ba7465ac2c5aa969ec1b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spunbindrule-transact-sql"></a>sp_unbindrule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Dissocie une règle d'une colonne ou d'un type de données alias dans la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Nous vous recommandons de créer des définitions par défaut à l’aide du mot clé par défaut dans le [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) instructions à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_unbindrule [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@objname=** ] **'***nom_objet***'**  
 Nom de la table et de la colonne ou du type de données alias dont la règle est dissociée. *nom_objet* est **nvarchar(776)**, sans valeur par défaut. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] essaie d'abord de résoudre en noms de colonnes les identificateurs en deux parties, puis en types de données alias. Lorsque vous dissociez une règle d'un type de données alias, les colonnes de ce type de données ayant la même règle sont également dissociées. Les colonnes de ce type de données auxquelles des règles sont directement liées ne sont pas affectées.  
  
> [!NOTE]  
>  *nom_objet* peut contenir des crochets **[]** en tant que la délimitée par des caractères d’identificateur. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
 [  **@futureonly=** ] **'***futureonly_flag***'**  
 S'utilise seulement pour dissocier une règle d'un type de données alias. *l’argument futureonly_flag* est **varchar(15)**, avec NULL comme valeur par défaut. Lorsque *futureonly_flag* est **futureonly**, les colonnes existantes de ce type de données ne perdent pas la règle spécifiée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour afficher le texte d’une règle, exécutez **sp_helptext** en donnant le nom de la règle comme paramètre.  
  
 Lorsqu’une règle est détachée, les informations relatives à la liaison sont supprimées à partir de la **sys.columns** table si la règle a été liée à une colonne et à partir de la **sys.types** si la règle a été liée à un type de données d’alias de table.  
  
 Lorsqu'une règle est dissociée d'un type de données alias, elle l'est aussi des colonnes ayant ce type de données alias. La règle peut toujours être associée aux colonnes dont les types de données ont été modifiés ultérieurement par la clause ALTER COLUMN d’une instruction ALTER TABLE, vous devez dissocier spécifiquement la règle à partir de ces colonnes à l’aide de **sp_unbindrule** et en spécifiant le nom de colonne.  
  
## <a name="permissions"></a>Autorisations  
 La dissociation d'une règle d'une colonne de table nécessite l'autorisation ALTER sur la table. La dissociation d'une règle d'un type de données alias nécessite l'autorisation CONTROL sur le type ou l'autorisation ALTER sur le schéma auquel le type appartient.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-unbinding-a-rule-from-a-column"></a>A. Dissociation d'une règle d'une colonne  
 L'exemple suivant dissocie la règle de la colonne `startdate` de la table `employees`.  
  
```  
EXEC sp_unbindrule 'employees.startdate';  
```  
  
### <a name="b-unbinding-a-rule-from-an-alias-data-type"></a>B. Dissociation d'une règle d'un type de données alias  
 L'exemple suivant dissocie la règle du type de données alias `ssn`. Il dissocie la règle des colonnes existantes et à venir de ce type.  
  
```  
EXEC sp_unbindrule ssn;  
```  
  
### <a name="c-using-futureonlyflag"></a>C. Utilisation de l'argument futureonly_flag  
 L'exemple suivant dissocie la règle du type de données alias `ssn` sans affecter les colonnes `ssn` existantes.  
  
```  
EXEC sp_unbindrule 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. À l’aide d’identificateurs délimités  
 L’exemple suivant illustre l’utilisation des identificateurs délimités dans le *nom_objet* paramètre.  
  
```  
CREATE TABLE [t.4] (c1 int); -- Notice the period as part of the table   
-- name.  
GO  
CREATE RULE rule2 AS @value > 100;  
GO  
EXEC sp_bindrule rule2, '[t.4].c1' -- The object contains two   
-- periods; the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
GO  
EXEC sp_unbindrule '[t.4].c1';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
