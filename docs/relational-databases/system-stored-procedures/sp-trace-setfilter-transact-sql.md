---
title: sp_trace_setfilter (Transact-SQL) | Documents Microsoft
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
- sp_trace_setfilter
- sp_trace_setfilter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setfilter
ms.assetid: 11e7c7ac-a581-4a64-bb15-9272d5c1f7ac
caps.latest.revision: 35
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: dc23abf114ce4458f40051db3d2bb86b667efca5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptracesetfilter-transact-sql"></a>sp_trace_setfilter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Applique un filtre à une trace. **sp_trace_setfilter** peut être exécutée uniquement sur les traces existantes qui sont arrêtées (*état* est **0**). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Retourne une erreur si cette procédure stockée est exécutée sur une trace qui n’existe pas ou dont le *état* n’est pas **0**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des événements étendus.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_trace_setfilter [ @traceid = ] trace_id   
          , [ @columnid = ] column_id  
          , [ @logical_operator = ] logical_operator  
          , [ @comparison_operator = ] comparison_operator  
          , [ @value = ] value  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@traceid=** ] *trace_id*  
 ID de la trace sur laquelle le filtre est défini. *l’argument trace_id* est **int**, sans valeur par défaut. L’utilisateur emploie cette *trace_id* valeur pour identifier, modifier et contrôler la trace.  
  
 [  **@columnid=** ] *column_id*  
 ID de la colonne sur laquelle le filtre est appliqué *column_id* est **int**, sans valeur par défaut. Si *column_id* est NULL, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] efface tous les filtres de la trace spécifiée.  
  
 [ **@logical_operator** =] *logical_operator*  
 Spécifie si l’AND (**0**) ou OR (**1**) opérateur est appliqué. *logical_operator* est **int**, sans valeur par défaut.  
  
 [  **@comparison_operator=** ] *opérateur_de_comparaison*  
 Spécifie le type de comparaison à effectuer. *opérateur_comparaison* est **int**, sans valeur par défaut. Le tableau contient les opérateurs de comparaison et leurs valeurs.  
  
|Valeur|Opérateur de comparaison|  
|-----------|-------------------------|  
|**0**|= (Égal à)|  
|**1**|<> (Différent de)|  
|**2**|> (Supérieur à)|  
|**3**|< (Inférieur à)|  
|**4**|>= (Supérieur ou égal à)|  
|**5**|< = (inférieur ou égal à)|  
|**6**|LIKE|  
|**7**|Ne correspond pas à|  
  
 [  **@value=** ] *valeur*  
 Indique la valeur sur laquelle appliquer le filtre. Type de données de *valeur* doit correspondre au type de données de la colonne à filtrer. Par exemple, si le filtre est défini sur une colonne d’ID d’objet qui est un **int** type de données, *valeur* doit être **int**. Si *valeur* est **nvarchar** ou **varbinary**, il peut avoir une longueur maximale de 8 000.  
  
 Lorsque l'opérateur de comparaison est LIKE ou NOT LIKE, l'opérateur logique peut inclure « % » ou tout autre filtre approprié pour l'opération LIKE.  
  
 Vous pouvez spécifier NULL pour *valeur* pour filtrer des événements avec les valeurs de colonne NULL. Uniquement **0** (= égal) et **1** (<> n’est pas égal) les opérateurs sont valides avec la valeur NULL. Dans ce cas, ils sont équivalents aux opérateurs IS NULL et IS NOT NULL de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Pour appliquer le filtre entre une plage de valeurs de colonne, **sp_trace_setfilter** doit être exécutée à deux reprises : une fois avec un supérieur à-ou-égal à ('> =') opérateur de comparaison et une autre fois avec un moins-à-ou-est égale à (« < = ») (opérateur).  
  
 Pour plus d’informations sur les types de données de colonne de données, consultez la [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Le tableau suivant décrit les valeurs de code que les utilisateurs peuvent recevoir à la fin de l'exécution de la procédure stockée.  
  
|Code de retour| Description|  
|-----------------|-----------------|  
|0|Aucune erreur.|  
|1|Erreur inconnue.|  
|2|La trace est en cours d'exécution. La modification de la Trace à cet instant précis entraîne une erreur.|  
|4|La colonne spécifiée n’est pas valide.|  
|5|La colonne spécifiée n'est pas autorisée pour le filtrage. Cette valeur est retournée uniquement à partir de **sp_trace_setfilter**.|  
|6|L'opérateur de comparaison spécifié n'est pas valide.|  
|7|L'opérateur logique spécifié n'est pas valide.|  
|9|Le descripteur de trace spécifié n'est pas valide.|  
|13|Mémoire insuffisante. Renvoyé lorsqu'il n'y a pas assez de mémoire pour exécuter l'action spécifiée.|  
|16|La fonction n'est pas valide pour cette trace.|  
  
## <a name="remarks"></a>Notes  
 **sp_trace_setfilter** est un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procédure stockée qui effectue la plupart des actions effectuées antérieurement par les procédures stockées étendues disponibles dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez **sp_trace_setfilter** au lieu du **xp_trace_set\*filtre** des procédures stockées étendues pour créer, appliquer, supprimer ou manipuler des filtres sur des traces. Pour plus d’informations, consultez [filtrer une Trace](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 Tous les filtres d’une colonne particulière doivent être activés ensemble dans une seule exécution de **sp_trace_setfilter**. Par exemple, si un utilisateur décide d'appliquer deux filtres sur la colonne Nom d'application et un filtre sur la colonne Nom d'utilisateur, il doit spécifier les filtres sur le nom d'application de manière séquentielle. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur si l'utilisateur tente de spécifier un filtre sur le nom d'application dans un appel de procédure stockée, suivi d'un filtre sur le nom d'utilisateur, puis d'un autre filtre sur le nom d'application.  
  
 Les paramètres de Trace de SQL toutes les procédures stockées (**sp_trace_xx**) sont de type strict. Si ces paramètres ne sont pas appelés à l'aide des types de données appropriés pour les paramètres d'entrée tels qu'ils sont spécifiés dans la description de l'argument, la procédure stockée retourne une erreur.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation ALTER TRACE.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit trois filtres sur Trace `1`. Les filtres `N'SQLT%'` et `N'MS%'` opèrent sur une colonne (`AppName`, valeur `10`) à l'aide de l'opérateur de comparaison « `LIKE` ». Le filtre `N'joe'` opère sur une autre colonne (`UserName`, valeur `11`) à l'aide de l'opérateur de comparaison « `EQUAL` ».  
  
```  
sp_trace_setfilter  1, 10, 0, 6, N'SQLT%';  
sp_trace_setfilter  1, 10, 0, 6, N'MS%';  
sp_trace_setfilter  1, 11, 0, 0, N'joe';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [Trace SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
