---
title: sp_check_for_sync_trigger (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- filter_TSQL
- sp_check_for_sync_trigger
helpviewer_keywords:
- sp_check_for_sync_trigger
ms.assetid: 54a1e2fd-c40a-43d4-ac64-baed28ae4637
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 5f389415adf06d6c7fce1862d42655bf4e96e2c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spcheckforsynctrigger-transact-sql"></a>sp_check_for_sync_trigger (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Détermine si un déclencheur ou une procédure stockée définis par l'utilisateur sont appelés dans le contexte d'un déclencheur de réplication utilisé pour les abonnements avec mise à jour immédiate. Cette procédure stockée est exécutée sur le serveur de publication sur la base de données de publication ou sur la base de données d’abonnement de l’abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_check_for_sync_trigger [ @tabid = ] 'tabid'   
    [ , [ @trigger_op = ] 'trigger_output_parameters' OUTPUT ]  
    [ , [ @fonpublisher = ] fonpublisher ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@tabid =** ] '*tabid*'  
 ID d'objet de la table dans laquelle les déclencheurs de mise à jour immédiate sont recherchés. *tabid* est **int** sans valeur par défaut.  
  
 [ **@trigger_op =** ] '*trigger_output_parameters*' sortie  
 Spécifie si le paramètre de sortie doit renvoyer le type du déclencheur à partir duquel il est appelé. *trigger_output_parameters* est **char (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Ins**|Déclencheur INSERT.|  
|**UPD**|Déclencheur UPDATE.|  
|**DEL**|Déclencheur DELETE.|  
|NULL (par défaut)||  
  
 [  **@fonpublisher =** ] *fonpublisher*  
 Spécifie l'emplacement où la procédure stockée est exécutée. *fonpublisher* est **bits**, avec 0 comme valeur par défaut. Si la valeur est 0, l'exécution s'effectue sur l'Abonné et, si la valeur est 1, l'exécution s'effectue sur le serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 indique que la procédure stockée n'est pas appelée dans le contexte d'un déclencheur de mise à jour immédiate. 1 indique qu’elle est appelée dans le contexte d’un déclencheur de mise à jour immédiate et le type de déclencheur renvoyé dans *@trigger_op*.  
  
## <a name="remarks"></a>Notes  
 **sp_check_for_sync_trigger** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
 **sp_check_for_sync_trigger** est utilisé pour la coordination entre la réplication et les déclencheurs définis par l’utilisateur. Cette procédure stockée détermine si elle est appelée dans le contexte d'un déclencheur de réplication. Par exemple, vous pouvez appeler la procédure **sp_check_for_sync_trigger** dans le corps d’un déclencheur défini par l’utilisateur. Si **sp_check_for_sync_trigger** retourne **0**, le déclencheur défini par l’utilisateur continue le traitement. Si **sp_check_for_sync_trigger** retourne **1**, le déclencheur défini par l’utilisateur se termine. Vous êtes ainsi assuré que le déclencheur défini par l'utilisateur ne s'active pas lorsque le déclencheur de réplication met à jour la table.  
  
## <a name="example"></a>Exemple  
 L'exemple suivant contient du code que vous pouvez utiliser dans un déclencheur appliqué à une table de l'Abonné.  
  
```  
DECLARE @retcode int, @trigger_op char(10), @table_id int  
SELECT @table_id = object_id('tablename')  
EXEC @retcode = sp_check_for_sync_trigger @table_id, @trigger_op OUTPUT  
IF @retcode = 1  
RETURN  
```  
  
## <a name="example"></a>Exemple  
 Le code peut également être ajouté à un déclencheur sur une table sur le serveur de publication ; le code est similaire, mais l’appel à **sp_check_for_sync_trigger** inclut un paramètre supplémentaire.  
  
```  
DECLARE @retcode int, @trigger_op char(10), @table_id int, @fonpublisher int  
SELECT @table_id = object_id('tablename')  
SELECT @fonpublisher = 1  
EXEC @retcode = sp_check_for_sync_trigger @table_id, @trigger_op OUTPUT, @fonpublisher  
IF @retcode = 1  
RETURN  
```  
  
## <a name="permissions"></a>Autorisations  
 **sp_check_for_sync_trigger** procédure stockée peut être exécutée par tout utilisateur disposant des autorisations SELECT dans le [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) vue système.  
  
## <a name="see-also"></a>Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
