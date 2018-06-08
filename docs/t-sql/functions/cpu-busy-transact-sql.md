---
title: '@@CPU_BUSY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '@@CPU_BUSY_TSQL'
- '@@CPU_BUSY'
dev_langs:
- TSQL
helpviewer_keywords:
- CPU [SQL Server]
- status information [SQL Server], CPU
- ticks [SQL Server]
- time [SQL Server], CPU activity
- '@@CPU_BUSY function'
- statistical information [SQL Server], CPU
- CPU [SQL Server], activity
ms.assetid: 81ae0e64-79fa-4a74-9aa5-37045c4cd211
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 27eb30bf1a8b176024610affa368a8db1c6607e1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="x40x40cpubusy-transact-sql"></a>&#x40;&#x40;CPU_BUSY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

Cette fonction retourne le temps qu’a passé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une opération active depuis son dernier démarrage. `@@CPU_BUSY` retourne un résultat exprimé en incréments de temps processeur ou « cycles ». Cette valeur étant cumulée pour tous les processeurs, elle peut être supérieure au temps écoulé réel. Pour la convertir en microsecondes, multipliez-la par [@@TIMETICKS](./timeticks-transact-sql.md).
  
> [!NOTE]  
>  Si le temps renvoyé dans @@CPU_BUSY ou @@IO_BUSY dépasse approximativement 49 jours de temps processeur cumulé, vous risquez de recevoir un avertissement de dépassement arithmétique. Dans ce cas, la valeur des variables `@@CPU_BUSY`, `@@IO_BUSY` et `@@IDLE` n’est pas précise.  
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```
@@CPU_BUSY  
```  
  
## <a name="return-types"></a>Types de retour
**entier**
  
## <a name="remarks"></a>Notes   
Pour afficher un rapport contenant plusieurs statistiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y compris l'activité du processeur, exécutez [sp_monitor](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md).
  
## <a name="examples"></a>Exemples  
Cet exemple montre l'activité de l'UC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la date et à l'heure actuelles. L’exemple convertit une des valeurs au type de données `float`. Cela évite les problèmes de dépassement arithmétique lors du calcul d’une valeur en microsecondes.
  
```sql
SELECT @@CPU_BUSY * CAST(@@TIMETICKS AS float) AS 'CPU microseconds',   
   GETDATE() AS 'As of' ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
CPU microseconds As of
---------------- -----------------------
18406250         2006-12-05 17:00:50.600
```
  
## <a name="see-also"></a>Voir aussi
[sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)  
[@@IDLE &#40;Transact-SQL&#41;](../../t-sql/functions/idle-transact-sql.md)  
[@@IO_BUSY &#40;Transact-SQL&#41;](../../t-sql/functions/io-busy-transact-sql.md)  
[sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)  
[Fonctions statistiques système &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)
  
  
