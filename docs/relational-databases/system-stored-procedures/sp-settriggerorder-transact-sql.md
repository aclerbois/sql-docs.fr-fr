---
title: sp_settriggerorder (Transact-SQL) | Documents Microsoft
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
- sp_settriggerorder
- sp_settriggerorder_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_settriggerorder
ms.assetid: 8b75c906-7315-486c-bc59-293ef12078e8
caps.latest.revision: 54
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: bef1fdf427c6bc510e77f8df55f3281d5b5db6cd
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "33263560"
---
# <a name="spsettriggerorder-transact-sql"></a>sp_settriggerorder (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Spécifie les déclencheurs AFTER activés en premier ou en dernier. L'ordre d'exécution des déclencheurs AFTER activés entre les premier et dernier déclencheurs est indéfini.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_settriggerorder [ @triggername = ] '[ triggerschema. ] triggername'   
    , [ @order = ] 'value'   
    , [ @stmttype = ] 'statement_type'   
    [ , [ @namespace = ] { 'DATABASE' | 'SERVER' | NULL } ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@triggername=** ] **'**[ *triggerschema ***.**] *nom_déclencheur ***'**  
 Nom du déclencheur et, éventuellement, du schéma auquel il appartient, dont l'ordre doit être défini ou modifié. [*triggerschema ***.**]* nom_déclencheur * est **sysname**. Si le nom ne correspond pas à un déclencheur ou si le nom correspond à un déclencheur INSTEAD OF, la procédure retourne une erreur. *triggerschema* ne peut pas être spécifié pour les déclencheurs DDL ou logon.  
  
 [ **@order=** ] **'***value***'**  
 Paramètre du nouvel ordre pour le déclencheur. *valeur* est **varchar (10)** et il peut prendre l’une des valeurs suivantes.  
  
> [!IMPORTANT]  
>  Le **première** et **dernière** déclencheurs doivent être deux déclencheurs différents.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Première**|Le déclencheur est activé en premier.|  
|**Dernière**|Le déclencheur est activé en dernier.|  
|**Aucun**|L'ordre d'activation du déclencheur n'est pas défini.|  
  
 [  **@stmttype=** ] **'***l’argument statement_type***'**  
 Spécifie l'instruction SQL qui active le déclencheur. *l’argument statement_type* est **varchar (50)** et peut être INSERT, UPDATE, DELETE, ouverture de session ou les [!INCLUDE[tsql](../../includes/tsql-md.md)] événement d’instruction répertorié dans [événements DDL](../../relational-databases/triggers/ddl-events.md). Les groupes d'événements ne peuvent pas être spécifiés.  
  
 Un déclencheur peut être désigné comme la **première** ou **dernière** déclencheur pour un type d’instruction uniquement après que le déclencheur a été défini comme un déclencheur pour ce type d’instruction. Par exemple, déclencher **TR1** peut être désignée **première** pour l’insertion d’une table **T1** si **TR1** est défini comme un déclencheur INSERT. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] renvoie une erreur si **TR1**, qui a été défini uniquement comme un déclencheur INSERT, est défini comme un **première**, ou **dernière**, déclencheur pour une instruction UPDATE. Pour plus d'informations, consultez la section Notes.  
  
 **@namespace=** { **'DATABASE'** | **'SERVER'** | NULL}  
 Lorsque *nom_déclencheur* est un déclencheur DDL, **@namespace** Spécifie si *nom_déclencheur* a été créé avec l’étendue de la base de données ou serveur. Si *nom_déclencheur* est un déclencheur d’ouverture de session, SERVER doit être spécifié. Pour plus d’informations sur l’étendue du déclencheur DDL, consultez [les déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md). Si non spécifié, ou si la valeur NULL est spécifiée, *nom_déclencheur* est un déclencheur DML.  
  
||  
|-|  
|SERVER s'applique à : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) et 1 (échec)  
  
## <a name="remarks"></a>Notes  
  
## <a name="dml-triggers"></a>Déclencheurs DML  
 Il peut y avoir qu’un seul **première** et **dernière** déclencheur pour chaque instruction sur une table unique.  
  
 Si un **première** déclencheur est déjà défini sur le serveur, la base de données ou la table, vous ne pouvez pas désigner un nouveau déclencheur en tant que **première** pour la même table, de la même base de données ou serveur pour le même *l’argument statement_type*. Cette restriction s’applique également **dernière** déclencheurs.  
  
 La réplication génère automatiquement un premier déclencheur pour toute table qui est incluse dans un abonnement avec mise à jour immédiate ou en attente. Elle nécessite un déclencheur qui soit le premier. Elle génère une erreur si vous essayez d'inclure une table détenant un premier déclencheur dans un abonnement mis à jour immédiatement ou en attente. Si vous tentez de définir un déclencheur comme premier déclencheur après qu’une table a été incluse dans un abonnement, **sp_settriggerorder** retourne une erreur. Si vous utilisez ALTER TRIGGER sur le déclencheur de réplication, ou **sp_settriggerorder** pour modifier le déclencheur de réplication à un **dernière** ou **aucun** déclencheur, l’abonnement ne fonctionne pas correctement.  
  
## <a name="ddl-triggers"></a>Déclencheurs DDL  
 Si un déclencheur DDL avec étendue de base de données et un déclencheur DDL avec étendue de serveur existent sur le même événement, vous pouvez spécifier que les deux déclencheurs sont un **première** déclencheur ou une **dernière** déclencheur. Toutefois, les déclencheurs avec étendue de serveur se déclenchent toujours en premier. En général, l'ordre d'exécution des déclencheurs DDL qui existent sur le même événement est le suivant :  
  
1.  Le déclencheur de niveau serveur marqué **premier**.  
  
2.  Autres déclencheurs de niveau serveur.  
  
3.  Le déclencheur de niveau serveur marqué **dernière**.  
  
4.  Le déclencheur de niveau base de données marquée **premier**.  
  
5.  Autres déclencheurs de niveau base de données.  
  
6.  Le déclencheur de niveau base de données marquée **dernière**.  
  
## <a name="general-trigger-considerations"></a>Considérations générales sur les déclencheurs  
 Si une instruction ALTER TRIGGER modifie un premier ou dernier déclencheur, le **première** ou **dernière** attribut défini à l’origine sur le déclencheur est supprimé, et la valeur est remplacée par **aucun**. La valeur d’ordre doit être réinitialisée à l’aide de **sp_settriggerorder**.  
  
 Si le même déclencheur doit être désigné comme le premier ou dernier ordre pour plus d’un type d’instruction, **sp_settriggerorder** doit être exécutée pour chaque type d’instruction. En outre, le déclencheur doit être d’abord défini pour un type d’instruction avant qu’il peut être désignée comme la **première** ou **dernière** déclencheur à activer pour ce type d’instruction.  
  
## <a name="permissions"></a>Autorisations  
 Pour définir l'ordre d'un déclencheur DDL avec étendue de serveur (à l'aide de la clause ON ALL SERVER) ou d'un déclencheur de connexion, requiert l'autorisation CONTROL SERVER sur le serveur.  
  
 Pour définir l'ordre d'un déclencheur DDL avec étendue de base de données (à l'aide de la clause ON DATABASE), requiert l'autorisation ALTER ANY DATABASE DDL TRIGGER.  
  
 Pour définir l'ordre d'un déclencheur DML, requiert l'autorisation ALTER sur la table ou vue sur laquelle le déclencheur est configuré.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-the-firing-order-for-a-dml-trigger"></a>A. Définition de l'ordre d'activation d'un déclencheur DML  
 L'exemple suivant spécifie que le déclencheur `uSalesOrderHeader` est le premier déclencheur à activer après une opération `UPDATE` sur la table `Sales.SalesOrderHeader`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'Sales.uSalesOrderHeader', @order='First', @stmttype = 'UPDATE';  
```  
  
### <a name="b-setting-the-firing-order-for-a-ddl-trigger"></a>B. Définition de l'ordre d'activation d'un déclencheur DDL  
 L'exemple suivant spécifie que le déclencheur `ddlDatabaseTriggerLog` est le premier déclencheur à activer après un événement `ALTER_TABLE` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'ddlDatabaseTriggerLog', @order='First', @stmttype = 'ALTER_TABLE', @namespace = 'DATABASE';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
  
