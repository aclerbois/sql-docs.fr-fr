---
title: Sys.dm_os_windows_info (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_os_windows_info
- dm_os_windows_info_TSQL
- sys.dm_os_windows_info
- sys.dm_os_windows_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_windows_info dynamic management view
ms.assetid: adc81283-fdc2-46c0-bb48-abe82bbf2459
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f6f6669704242a780d947dc271cb81724429992a
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmoswindowsinfo-transact-sql"></a>sys.dm_os_windows_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne qui affiche les informations relatives à la version du système d'exploitation Windows.  
  
  S’applique uniquement à SQL Server s’exécutant sous Windows. Pour afficher des informations similaires pour SQL Server s’exécutant sur un ordinateur hôte non Windows, tels que Linux, utilisez [sys.dm_os_host_info &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md). 
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**windows_release**|**nvarchar (256)**|Pour Windows, retourne le numéro de version. Pour obtenir la liste de valeurs et les descriptions, consultez [Version du système d’exploitation (Windows)](http://msdn.microsoft.com/library/ms724832\(VS.85\).aspx). Ne peut pas avoir la valeur NULL.|  
|**windows_service_pack_level**|**nvarchar (256)**| Pour Windows, retourne le numéro de service pack. Ne peut pas avoir la valeur NULL. |  
|**windows_sku**|**int**|Pour Windows, retourne l’ID de Windows en conservant unité référence SKU (Stock). Pour obtenir la liste des ID de référence (SKU) et des descriptions, consultez [GetProductInfo, fonction](http://msdn.microsoft.com/library/ms724358.aspx). Est NULLable. |  
|**os_language_version**|**int**| Pour Windows, retourne l’identificateur de paramètres régionaux (LCID) Windows du système d’exploitation. Pour obtenir la liste des valeurs LCID et des descriptions, consultez [ID de paramètres régionaux assignés par Microsoft](http://go.microsoft.com/fwlink/?LinkId=208080). Ne peut pas avoir la valeur NULL.|  
  
  
## <a name="permissions"></a>Autorisations  
L’autorisation SELECT sur sys.dm_os_windows_info est accordée au rôle public par défaut. Si révoqués, nécessite l’autorisation VIEW SERVER STATE sur le serveur.  

## <a name="limitations-and-restrictions"></a>Limitations et restrictions
Pour afficher des informations pour SQL en cours d’exécution sur un ordinateur hôte non Windows, tels que Linux, utilisez [sys.dm_os_host_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md). 
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne toutes les colonnes de la **sys.dm_os_windows_info** vue.  
  
```  
SELECT windows_release, windows_service_pack_level, windows_sku, os_language_version  
FROM sys.dm_os_windows_info;  
```  
  
 Un exemple d'ensemble de résultats est présenté ci-dessous.  
  
 `windows_release  windows_service_pack_level   windows_sku   os_language_version`  
  
 `---------------  ---------------------------  ------------  -------------------`  
  
 `6.0              Service Pack 2                4            1033`  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_os_host_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-host-info-transact-sql.md)  
  
  

