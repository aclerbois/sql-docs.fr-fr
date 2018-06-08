---
title: Sys.dm_pdw_exec_requests (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/09/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 390225cc-23e8-4051-a5f6-221e33e4c0b4
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 0c4a06d95b1ce9549397ae27fb8e7e0763fe996c
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwexecrequests-transact-sql"></a>Sys.dm_pdw_exec_requests (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations sur toutes les demandes actives actuellement ou récemment dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Il répertorie une ligne par/requête de la demande.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Clé pour cette vue. Id numérique unique associé à la demande.|Le point d’entrée unique pour toutes les demandes dans le système.|  
|session_id|**nvarchar(32)**|Id numérique unique associé à la session dans laquelle cette requête a été exécutée. Consultez [sys.dm_pdw_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).||  
|status|**nvarchar(32)**|État actuel de la demande.|'En cours d’exécution », « Suspendu », 'Terminé', 'Annulation', 'Échec'.|  
|submit_time|**datetime**|Heure à laquelle la demande a été soumise pour exécution.|Valide **datetime** petit ou égal à l’heure actuelle et heure_début.|  
|start_time|**datetime**|Heure de début de l’exécution de la demande.|NULL pour les demandes en file d’attente ; Sinon, valide **datetime** inférieure ou égale à l’heure actuelle.|  
|end_compile_time|**datetime**|Heure à laquelle le moteur a terminé la compilation de la demande.|NULL pour les demandes qui n’ont pas été compilés dans le cas contraire valide **datetime** heure_début inférieure et inférieure ou égale à l’heure actuelle.|
|end_time|**datetime**|Heure à laquelle l’exécution de la demande s’est terminée, a échoué ou a été annulée.|NULL pour les demandes en file d’attente ou actives ; dans le cas contraire, valide **datetime** inférieure ou égale à l’heure actuelle.|  
|total_elapsed_time|**int**|Temps écoulé dans l’exécution, car la requête a été lancée, en millisecondes.|Comprise entre 0 et la différence entre start_time et heure_fin.<br /><br /> Si total_elapsed_time dépasse la valeur maximale pour un entier, total_elapsed_time continueront à être la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée. »<br /><br /> La valeur maximale, en millisecondes est équivalente à 24.8 jours.|  
|Étiquette|**nvarchar(255)**|Chaîne d’étiquette facultative associée à des instructions de la requête SELECT.|Toute chaîne contenant « a-z », « A-Z », « 0-9', '_'.|  
|error_id|**nvarchar(36)**|Id unique de l’erreur associée à la demande, le cas échéant.|Consultez [sys.dm_pdw_errors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md); la valeur NULL si aucune erreur ne s’est produite.|  
|database_id|**int**|Identificateur de la base de données utilisée par le contexte explicite (par exemple, utilisez DB_X).|Consultez le code dans [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|commande|**nvarchar(4000)**|Contient le texte intégral de la demande et soumis par l’utilisateur.|Tout texte de requête ou de demande valid. Requêtes de plus de 4 000 octets sont tronqués.|  
|resource_class|**nvarchar(20)**|La classe de ressource pour cette demande. Voir liée **concurrency_slots_used** dans [sys.dm_pdw_resource_waits &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-resource-waits-transact-sql.md).|SmallRC<br /><br /> MediumRC<br /><br /> LargeRC<br /><br /> XLargeRC|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez « Minimum et Maximum valeurs » dans le [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE.  
  
## <a name="security"></a>Sécurité  
 Sys.dm_pdw_exec_requests ne filtre pas les résultats de la requête en fonction des autorisations spécifiques à la base de données. Connexions avec l’autorisation VIEW SERVER STATE peuvent obtenir des résultats pour toutes les bases de données, les résultats de requête  
  
> [!WARNING]  
>  Une personne malveillante peut utiliser sys.dm_pdw_exec_requests pour récupérer des informations sur les objets de base de données spécifique par le simple fait d’avoir l’autorisation VIEW SERVER STATE et n’ayant ne pas une autorisation spécifique à la base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
