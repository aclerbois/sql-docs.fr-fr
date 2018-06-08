---
title: Sys.fn_get_sql (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- fn_get_sql
- sys.fn_get_sql_TSQL
- fn_get_sql_TSQL
- sys.fn_get_sql
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_sql function
- text [SQL Server], SQL handles
- sys.fn_get_sql function
- valid SQL handles [SQL Server]
- SQL handles
ms.assetid: d5fe49b5-0813-48f2-9efb-9187716b2fd4
caps.latest.revision: 39
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 5051b76490bc27a5e16aedf16be2bdff2dab8c95
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfngetsql-transact-sql"></a>sys.fn_get_sql (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie le texte de l'instruction SQL pour le handle SQL spécifié.  
  
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une prochaine version de Microsoft SQL Server. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez sys.dm_exec_sql_text à la place. Pour plus d’informations, consultez [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_get_sql ( SqlHandle )  
```  
  
## <a name="arguments"></a>Arguments  
 *SqlHandle*  
 Valeur du handle. *SqlHandle* est **varbinary(64)** sans valeur par défaut.  
  
## <a name="tables-returned"></a>Tables retournées  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|dbid|**smallint**|ID de la base de données. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.|  
|objectid|**int**|Identificateur de l'objet de base de données. NULL pour les instructions SQL ad hoc.|  
|nombre|**smallint**|Indique le numéro du groupe, si les procédures sont groupées.<br /><br /> 0 = les entrées ne sont pas des procédures.<br /><br /> NULL = instructions SQL ad hoc.|  
|encrypted|**bit**|Indique si l'objet est chiffré.<br /><br /> 0 = Non chiffrée<br /><br /> 1 = Chiffrée|  
|texte|**texte**|Texte de l'instruction SQL. NULL pour les objets chiffrés.|  
  
## <a name="remarks"></a>Notes  
 Vous pouvez obtenir un handle SQL valide à partir de la colonne sql_handle de la [sys.dm_exec_requests &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) vue de gestion dynamique.  
  
 Si vous passez un handle qui n’est plus existe dans le cache, fn_get_sq**l** renvoie un jeu de résultats vide. Si vous passez un handle qui n'est pas valide, le lot s'arrête et un message d'erreur s'affiche.  
  
 Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ne peut pas mettre en cache certaines [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions, telles que les instructions de copie en bloc et les instructions avec des littéraux de chaîne sont supérieures à 8 Ko. Les handles de ces instructions ne peuvent pas être récupérés avec fn_get_sql.  
  
 Le **texte** colonne du jeu de résultats est filtrée pour tout texte qui peut contenir des mots de passe. Pour plus d’informations liées à la sécurité sur les procédures stockées qui ne sont pas contrôlées, consultez [filtrer une Trace](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 La fonction fn_get_sql renvoie des informations qui sont similaires à la [DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md) commande. Les exemples suivants montrent quand vous pouvez utiliser la fonction fn_get_sql parce que DBCC INPUTBUFFER ne peut pas l'être :  
  
-   lorsque des événements contiennent plus de 255 caractères ;  
  
-   lorsque vous devez renvoyer le niveau d'imbrication actuel le plus élevé d'une procédure stockée. Imaginons par exemple deux procédures stockées appelées sp_1 et sp_2. Si sp_1 appelle sp_2 et que vous obtenez le handle de la vue de gestion dynamique sys.dm_exec_requests alors que sp_2 est en cours d'exécution, la fonction fn_get_sql renvoie des informations sur sp_2. En outre, la fonction fn_get_sql renvoie le texte complet de la procédure stockée au niveau d'imbrication actuel le plus élevé.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur a besoin de l’autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Les administrateurs de base de données peuvent utiliser la fonction fn_get_sql, comme indiqué dans l'exemple suivant pour diagnostiquer les problèmes de traitement. Lorsque l'administrateur identifie un ID de session problématique, il peut récupérer le handle SQL de cette session, appeler fn_get_sql avec le handle, puis utiliser les décalages de début et de fin pour déterminer le texte SQL de l'ID de session problématique.  
  
```  
DECLARE @Handle varbinary(64);  
SELECT @Handle = sql_handle   
FROM sys.dm_exec_requests   
WHERE session_id = 52 and request_id = 0;  
SELECT * FROM sys.fn_get_sql(@Handle);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)   
 [sys.sysprocesses &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
