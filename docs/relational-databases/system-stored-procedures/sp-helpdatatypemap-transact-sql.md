---
title: sp_helpdatatypemap (Transact-SQL) | Documents Microsoft
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
- sp_helpdatatypemap
- sp_helpdatatypemap_TSQL
helpviewer_keywords:
- sp_helpdatatypemap
ms.assetid: 800c9c65-723e-4961-a63d-327987f129f0
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f19860d48b00b5eb9276c62ec46f18e2f5842c81
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpdatatypemap-transact-sql"></a>sp_helpdatatypemap (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les mappages de types de données définis entre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de données des systèmes de gestion (SGBD). Cette procédure stockée est exécutée sur une base de données du serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdatatypemap [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @defaults_only = ] defaults_only ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@source_dbms**=] **'***source_dbms***'**  
 Nom du SGBD à partir duquel les types de données sont mappés. *source_dbms* est **sysname**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données source au format [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|Base de données Oracle source.|  
  
 [ **@source_version**=] **'***source_version***'**  
 Version de produit du SGBD source. *source_version*est **varchar (10)**, et si non spécifié, les données de mappages de type de toutes les versions du SGBD source est retournés. Permet de filtrer le jeu de résultats en fonction de la version source du SGBD.  
  
 [ **@source_type**=] **'***source_type***'**  
 Le type de données est répertorié dans le SGBD source. *source_type* est **sysname**, et si non spécifié, les mappages de tous les types de données dans le SGBD source sont retournés. Permet de filtrer le jeu de résultats en fonction du type de données indiqué dans le SGBD source.  
  
 [ **@destination_dbms** =] **'***destination_dbms***'**  
 Nom du SGBD de destination. *destination_dbms* est **sysname**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**MSSQLSERVER**|Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destination.|  
|**ORACLE**|Base de données Oracle de destination.|  
|**DB2**|Base de données IBM DB2 de destination.|  
|**SYBASE**|Base de données Sybase de destination.|  
  
 [ **@destination_version**=] **'***destination_version***'**  
 Est la version de produit du SGBD de destination. *destination_version*est **varchar (10)**, et si non spécifié, les mappages de toutes les versions du SGBD de destination sont retournés. Permet de filtrer le jeu de résultats en fonction de la version de destination du SGBD.  
  
 [ **@destination_type**=] **'***destination_type***'**  
 Le type de données est répertorié dans le SGBD de destination. *destination_type*est **sysname**, et si non spécifié, les mappages de tous les types de données du SGBD de destination sont retournés. Permet de filtrer le jeu de résultats en fonction du type de données indiqué dans le SGBD de destination.  
  
 [ **@defaults_only**=] *defaults_only*  
 Indique si seuls les mappages de type de données par défaut sont retournés. *defaults_only* est **bits**, avec une valeur par défaut **0**. **1** signifie que seules les données de la valeur par défaut des mappages de type est retournées. **0** signifie que la valeur par défaut et toutes les données définies par l’utilisateur mappages de type est retournées.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**mapping_id**|Identifie un mappage de type de données.|  
|**source_dbms**|Nom et numéro de version du SGBD source.|  
|**source_type**|Type de données répertorié dans le SGBD source.|  
|**destination_dbms**|Nom du SGBD de destination.|  
|**destination_type**|Type de données répertorié dans le SGBD de destination.|  
|**is_default**|Indique si le mappage est un mappage par défaut ou un autre mappage. La valeur **0** indique que ce mappage est défini par l’utilisateur.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpdatatypemap** définit les mappages de type de données de publication non SQL Server et à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éditeurs non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
 Lors de la combinaison spécifiée de la source et le SGBD de destination n’est pas pris en charge, **sp_helpdatatypemap** renvoie un jeu de résultats vide.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe sur le serveur de distribution ou les membres de la **db_owner** du rôle de base de données fixe sur la base de données de distribution permettre exécuter **sp_helpdatatypemap**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_getdefaultdatatypemapping &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)  
  
  
