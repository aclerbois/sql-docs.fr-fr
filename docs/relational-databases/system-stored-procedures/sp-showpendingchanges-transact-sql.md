---
title: sp_showpendingchanges (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_showpendingchanges
- sp_showpendingchanges_TSQL
helpviewer_keywords:
- sp_showpendingchanges
ms.assetid: 8013a792-639d-4550-b262-e65d30f9d291
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2b59856ba83d3118a9246bb5cd93a8d63e7745f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spshowpendingchanges-transact-sql"></a>sp_showpendingchanges (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne un jeu de résultats affichant les modifications en attente de réplication. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication et sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  Cette procédure fournit un nombre approximatif des modifications apportées ainsi que les lignes concernées par ces modifications. Par exemple, la procédure récupère les informations du serveur de publication ou de l'abonné, mais pas à la fois en même temps. Les informations stockées à l'autre nœud peuvent engendrer un plus petit jeu de modifications à synchroniser que les estimations de procédure.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_showpendingchanges [ [ @destination_server = ] 'destination_server' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article']  
    [ , [ @show_rows = ] show_rows]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @destination_server **=** ] **'***destination_server***'**  
 Nom du serveur sur lequel les modifications répliquées sont appliquées. *destination_server* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ @publication **=** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec NULL comme valeur par défaut. Lorsque *publication* est spécifié, les résultats sont uniquement limités à la publication spécifiée.  
  
 [ @article **=** ] **'***article***'**  
 Nom de l'article. *article* est **sysname**, avec NULL comme valeur par défaut. Lorsque *article* est spécifié, les résultats sont uniquement limités à l’article spécifié.  
  
 [ @show_rows **=** ] *show_rows*  
 Spécifie si le jeu de résultats contient des informations plus spécifiques sur les modifications en attente, avec la valeur par défaut **0**. Si la valeur **1** est spécifiée, le jeu de résultats contient les colonnes is_delete et rowguid.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|destination_server|**sysname**|Nom du serveur vers lequel les modifications sont en cours de réplication.|  
|pub_name|**sysname**|Nom de la publication.|  
|destination_db_name|**sysname**|Nom de la base de données vers laquelle les modifications sont en cours de réplication.|  
|is_dest_subscriber|**bit**|Indique si les modifications sont en cours de réplication vers un Abonné. La valeur **1** indique que les modifications sont répliquées sur un abonné. **0** signifie que les modifications sont répliquées vers un serveur de publication.|  
|article_name|**sysname**|Nom de l'article de la table d'origine des modifications.|  
|pending_deletes|**int**|Nombre de suppressions en attente de réplication.|  
|pending_ins_and_upd|**int**|Nombre d'insertions et de mises à jour en attente de réplication.|  
|is_delete|**bit**|Indique si la modification en attente est une suppression. La valeur **1** indique que la modification est une suppression. Requiert une valeur de **1** pour @show_rows.|  
|rowguid|**uniqueidentifier**|GUID qui identifie la ligne modifiée. Requiert une valeur de **1** pour @show_rows.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_showpendingchanges est utilisée pour la réplication de fusion.  
  
 sp_showpendingchanges est utilisée pour le dépannage de la réplication de fusion.  
  
 Le résultat de sp_showpendingchanges ne comprend pas de lignes dans la génération 0.  
  
 Lorsqu’un article spécifié pour *article* n’appartient pas à la publication indiquée pour *publication,* un nombre de 0 est retourné pour pending_deletes et pending_ins_and_upd.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle de serveur fixe sysadmin ou du rôle de base de données fixe db_owner peuvent exécuter la procédure sp_showpendingchanges.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
