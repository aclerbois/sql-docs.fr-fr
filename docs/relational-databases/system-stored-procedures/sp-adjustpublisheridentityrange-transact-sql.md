---
title: sp_adjustpublisheridentityrange (Transact-SQL) | Documents Microsoft
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
- sp_adjustpublisheridentityrange_TSQL
- sp_adjustpublisheridentityrange
helpviewer_keywords:
- sp_adjustpublisheridentityrange
ms.assetid: 64f111fd-fb7d-4459-93f7-65f0f8dd7efe
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e8d39485c2ed9519353a2175fa4e127a5900b0e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spadjustpublisheridentityrange-transact-sql"></a>sp_adjustpublisheridentityrange (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajuste la plage d'identités sur une publication et réaffecte de nouvelles plages en fonction de la valeur de seuil définie pour la publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adjustpublisheridentityrange [ [ @publication = ] 'publication' ]  
    [ , [ @table_name = ] 'table_name' ]  
    [ , [ @table_owner= ] 'table_owner' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication dans laquelle de nouvelles plages d'identité sont réaffectées. *publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@table_name=**] **'***table_name***'**  
 Nom de la table dans laquelle de nouvelles plages d'identité sont réaffectées. *nom_table* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@table_owner=**] **'***table_owner***'**  
 Nom du propriétaire de la table au niveau du serveur de publication. *TABLE_OWNER* est **sysname**, avec NULL comme valeur par défaut. Si *table_owner* n’est pas spécifié, le nom de l’utilisateur actuel est utilisé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adjustpublisheridentityrange** est utilisée dans tous les types de réplication.  
  
 Lorsque le paramètre d'affectation automatique de plage d'identités est activé pour une publication, l'Agent de distribution ou de fusion est responsable de l'ajustement automatique de la plage d'identités en fonction de la valeur de seuil de la publication. Toutefois, si pour une raison quelconque l’Agent de Distribution ou l’Agent de fusion n'a pas été exécuté pendant une période de temps, et la ressource de plage d’identité a été sensiblement sollicitée jusqu’au point de seuil, les utilisateurs peuvent appeler **sp_adjustpublisheridentityrange** pour allouer une nouvelle plage de valeurs pour un serveur de publication.  
  
 Lors de l’exécution **sp_adjustpublisheridentityrange**, *publication* ou *table_name* doit être spécifié. Si les deux sont spécifiés ou aucun des deux, un message d'erreur est renvoyé.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_adjustpublisheridentityrange**.  
  
## <a name="see-also"></a>Voir aussi  
 [Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
