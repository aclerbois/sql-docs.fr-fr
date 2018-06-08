---
title: sp_setdefaultdatatypemapping (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- sp_setdefaultdatatypemapping
- sp_setdefaultdatatypemapping_TSQL
helpviewer_keywords:
- sp_setdefaultdatatypemapping
ms.assetid: 7394e8ca-4ce1-4e99-a784-205007c2c248
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6b8d3751448d553666f7b7301c42329f32a7713a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spsetdefaultdatatypemapping-transact-sql"></a>sp_setdefaultdatatypemapping (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Marque un mappage de type de données existant entre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de données de gestion (SGBD) en tant que la valeur par défaut. Cette procédure stockée est exécutée sur une base de données du serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_setdefaultdatatypemapping [ [ @mapping_id = ] mapping_id ]  
    [ , [ @source_dbms = ] 'source_dbms' ]  
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @source_length_min = ] source_length_min ]  
    [ , [ @source_length_max = ] source_length_max ]  
    [ , [ @source_precision_min = ] source_precision_min ]  
    [ , [ @source_precision_max = ] source_precision_max ]  
    [ , [ @source_scale_min = ] source_scale_min ]  
    [ , [ @source_scale_max = ] source_scale_max ]  
    [ , [ @source_nullable = ] source_nullable ]  
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @destination_length = ] destination_length ]  
    [ , [ @destination_precision = ] destination_precision ]  
    [ , [ @destination_scale = ] destination_scale ]  
    [ , [ @destination_nullable = ] source_nullable ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@mapping_id=** ] *mapping_id*  
 Identifie un mappage de types de données existant.  *mapping_id* est **int**, avec NULL comme valeur par défaut. Si vous spécifiez *mapping_id*, puis les paramètres restants ne sont pas obligatoires.  
  
 [ **@source_dbms**=] **'***source_dbms***'**  
 Nom du SGBD à partir duquel les types de données sont mappés. *source_dbms* est **sysname**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données source au format [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Base de données Oracle source.|  
|NULL (par défaut)||  
  
 Vous devez spécifier ce paramètre si *mapping_id* a la valeur NULL.  
  
 [  **@source_version=** ] **'***source_version***'**  
 Numéro de version du SGBD source. *source_version* est **varchar (10)**, avec NULL comme valeur par défaut.  
  
 [ **@source_type**=] **'***source_type***'**  
 Type de données répertorié dans le SGBD source. *source_type* est **sysname**. Vous devez spécifier ce paramètre si *mapping_id* a la valeur NULL.  
  
 [  **@source_length_min=** ] *source_length_min*  
 Longueur minimale du type de données dans le SGBD source. *source_length_min* est **bigint**, avec NULL comme valeur par défaut.  
  
 [  **@source_length_max=** ] *source_length_max*  
 Longueur maximale du type de données dans le SGBD source. *source_length_max* est **bigint**, avec NULL comme valeur par défaut.  
  
 [  **@source_precision_min=** ] *source_precision_min*  
 Précision minimale du type de données dans le SGBD source. *source_precision_min* est **bigint**, avec NULL comme valeur par défaut.  
  
 [  **@source_precision_max=** ] *source_precision_max*  
 Précision maximale du type de données dans le SGBD source. *source_precision_max* est **bigint**, avec NULL comme valeur par défaut.  
  
 [  **@source_scale_min=** ] *source_scale_min*  
 Échelle minimale du type de données dans le SGBD source. *source_scale_min* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@source_scale_max=** ] *source_scale_max*  
 Échelle maximale du type de données dans le SGBD source. *source_scale_max* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@source_nullable=** ] *source_nullable*  
 Indique si le type de données du SGBD source prend en charge la valeur NULL. *source_nullable* est **bits**, avec NULL comme valeur par défaut. **1** signifie que les valeurs NULL sont prises en charge.  
  
 [ **@destination_dbms** =] **'***destination_dbms***'**  
 Nom du SGBD de destination. *destination_dbms* est **sysname**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination.|  
|**ORACLE**|Base de données Oracle de destination.|  
|**DB2**|Base de données IBM DB2 de destination.|  
|**SYBASE**|Base de données Sybase de destination.|  
|NULL (par défaut)||  
  
 [ **@destination_version**=] **'***destination_version***'**  
 Est la version de produit du SGBD de destination. *destination_version* est **varchar (10)**, avec NULL comme valeur par défaut.  
  
 [ **@destination_type**=] **'***destination_type***'**  
 Le type de données est répertorié dans le SGBD de destination. *destination_type* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@destination_length=** ] *destination_length*  
 Longueur du type de données du SGBD de destination. *destination_length* est **bigint**, avec NULL comme valeur par défaut.  
  
 [  **@destination_precision=** ] *destination_precision*  
 Est la précision du type de données du SGBD de destination. *destination_precision* est **bigint**, avec NULL comme valeur par défaut.  
  
 [  **@destination_scale=** ] *destination_scale*  
 Correspond à l’échelle du type de données du SGBD de destination. *destination_scale* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@destination_nullable=** ] *destination_nullable*  
 Indique si le type de données du SGBD de destination prend en charge la valeur NULL. *destination_nullable* est **bits**, avec NULL comme valeur par défaut. **1** signifie que les valeurs NULL sont prises en charge.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_setdefaultdatatypemapping** est utilisée dans tous les types de réplication entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SGBD.  
  
 Les mappages de types de données par défaut s'appliquent à toutes les topologies de réplication qui comprennent le SGBD spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_setdefaultdatatypemapping**.  
  
## <a name="see-also"></a>Voir aussi  
 [Spécifier des mappages de Type de données pour un serveur de publication Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [sp_getdefaultdatatypemapping &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_helpdatatypemap &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)  
  
  
