---
title: Sys.fn_cdc_get_min_lsn (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server (starting with 2008)
f1_keywords:
- sys.fn_cdc_get_min_lsn
- fn_cdc_get_min_lsn
- fn_cdc_get_min_lsn_TSQL
- sys.fn_cdc_get_min_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_min_lsn
- sys.fn_cdc_get_min_lsn
ms.assetid: bd49e28a-128b-4f6b-8545-6a2ec3f4afb3
caps.latest.revision: 17
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 4c4c6a9bf83e83628891104f0c95a6baefa08234
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfncdcgetminlsn-transact-sql"></a>sys.fn_cdc_get_min_lsn (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la valeur de la colonne start_lsn pour l’instance de capture spécifiée à partir de la [cdc.change_tables](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md) (table système). Cette valeur représente le point de terminaison inférieur de l'intervalle de validité pour l'instance de capture.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_cdc_get_min_lsn ( 'capture_instance_name' )  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *capture_instance_name* **'**  
 Est le nom de l’instance de capture. *capture_instance_name* est **sysname**.  
  
## <a name="return-types"></a>Types de retour  
 **binary(10)**  
  
## <a name="remarks"></a>Notes  
 Retourne 0x00000000000000000000 lorsque l'instance de capture n'existe pas ou que l'appelant n'est pas autorisé à accéder aux données de modification associées à l'instance de capture.  
  
 Cette fonction est utilisée en général pour identifier le point de terminaison inférieur de la chronologie de capture des données modifiées associé à une instance de capture. Vous pouvez également utiliser cette fonction pour valider que les points de terminaison d'une plage de requêtes se situent dans la chronologie de l'instance de capture avant de demander les données de modification. Il est important d'effectuer de tels contrôles, car le point de terminaison inférieur d'une instance de capture change lorsque le nettoyage est effectué sur les tables de modifications. Si l'intervalle entre les demandes de données de modification est significatif, même un point de terminaison inférieur qui a pour valeur le point de terminaison supérieur de la demande de données de modification précédente peut se trouver à l'extérieur de la chronologie actuelle.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-minimum-lsn-value-for-a-specified-capture-instance"></a>A. Retour de la valeur LSN minimale pour une instance de capture spécifiée  
 L'exemple suivant retourne la valeur LSN minimale pour l'instance de capture `HumanResources_Employee` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2-12;  
GO  
SELECT sys.fn_cdc_get_min_lsn ('HumanResources_Employee')AS min_lsn;  
  
```  
  
### <a name="b-verifying-the-low-endpoint-of-a-query-range"></a>B. Vérification du point de terminaison inférieur d'une plage de requêtes  
 L'exemple suivant utilise la valeur LSN minimale retournée par `sys.fn_cdc_get_min_lsn` pour vérifier que le point de terminaison inférieur proposé pour une requête de données de modification est valide pour la chronologie actuelle pour l'instance de capture `HumanResources_Employee`. Cet exemple suppose que la valeur LSN du point de terminaison supérieur précédent pour l'instance de capture a été enregistrée et est disponible pour définir la variable `@save_to_lsn`. Pour les besoins de cet exemple, `@save_to_lsn` a pour valeur 0x000000000000000000 pour forcer l'exécution de la section de gestion des erreurs.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @min_lsn binary(10), @from_lsn binary(10),@save_to_lsn binary(10), @to_lsn binary(10);  
-- Sets @save_to_lsn to the previous high endpoint saved from the last change data request.  
SET @save_to_lsn = 0x000000000000000000;  
-- Sets the upper endpoint for the query range to the current maximum LSN.  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
-- Sets the @min_lsn parameter to the current minimum LSN for the capture instance.  
SET @min_lsn = sys.fn_cdc_get_min_lsn ('HumanResources_Employee');  
-- Sets the low endpoint for the query range to the LSN that follows the previous high endpoint.  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
-- Tests to verify the low endpoint is valid for the current capture instance.  
IF (@from_lsn < @min_lsn)  
    BEGIN  
        RAISERROR('Low endpoint of the request interval is invalid.', 16, -1);  
    END  
ELSE  
-- Return the changes occurring within the query range.  
    SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee(@from_lsn, @to_lsn, 'all');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_cdc_get_max_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)  
  
  
