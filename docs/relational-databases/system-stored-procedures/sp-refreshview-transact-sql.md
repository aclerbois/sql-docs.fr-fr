---
title: sp_refreshview (Transact-SQL) | Documents Microsoft
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
- sp_refreshview
- sp_refreshview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refreshview
ms.assetid: 9ce1d07c-ee66-4a83-8c73-cd2cc104dd08
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f9871a4ce62ce2916a1cd27c3ca8735de28cfa1b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sprefreshview-transact-sql"></a>sp_refreshview (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Met à jour les métadonnées de la vue non liée au schéma spécifiée. Les métadonnées persistantes d'une vue peuvent devenir obsolètes en raison des modifications des objets sous-jacents dont dépend l'affichage.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_refreshview [ @viewname = ] 'viewname'   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@viewname=** ] **'***viewname***'**  
 Nom de la vue. *ViewName* est **nvarchar**, sans valeur par défaut. *ViewName* peut être un identificateur multipartie, mais ne peut faire référence à des vues de la base de données actuelle.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou un nombre différent de zéro (échec)  
  
## <a name="remarks"></a>Notes  
 Si aucune vue n’est pas créée avec schemabinding, **sp_refreshview** doit être exécuté lorsque des modifications sont apportées aux objets sous-jacents de la vue qui affectent la définition de la vue. Autrement, la vue risque de produire des résultats imprévisibles en cas d'interrogation.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER sur la vue ainsi que l'autorisation REFERENCES sur les types CLR (Common Language Runtime) définis par l'utilisateur et sur les collections de schémas XML référencés par les colonnes de la vue.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-updating-the-metadata-of-a-view"></a>A. Mise à jour des métadonnées d'une vue  
 L'exemple suivant actualise les métadonnées de la vue `Sales.vIndividualCustomer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sp_refreshview N'Sales.vIndividualCustomer';  
```  
  
### <a name="b-creating-a-script-that-updates-all-views-that-have-dependencies-on-a-changed-object"></a>B. Création d'un script qui met à jour toutes les vues ayant des dépendances envers un objet modifié  
 Supposez que la table `Person.Person` a été modifiée d'une manière qui affecte la définition de toute vue créée dessus. L'exemple suivant crée un script qui actualise les métadonnées de toutes les vues qui ont une dépendance envers une table `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT 'EXEC sp_refreshview ''' + name + ''''   
FROM sys.objects AS so   
INNER JOIN sys.sql_expression_dependencies AS sed   
    ON so.object_id = sed.referencing_id   
WHERE so.type = 'V' AND sed.referenced_id = OBJECT_ID('Person.Person');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sp_refreshsqlmodule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md)  
  
  
