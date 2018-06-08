---
title: optimize for ad hoc workloads (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- optimize for ad hoc workloads option
ms.assetid: 0972e028-3a8e-454b-a186-e814a1d431f2
caps.latest.revision: 14
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ff730d6eae4850887a2c3a58ab48395f3729e856
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="optimize-for-ad-hoc-workloads-server-configuration-option"></a>optimize for ad hoc workloads (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  L'option **optimize for ad hoc workloads** permet d'améliorer l'efficacité du cache du plan pour les charges de travail qui contiennent de nombreux lots ad hoc à usage unique. Lorsque cette option a la valeur 1, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke un petit stub du plan compilé dans le cache du plan lorsqu'un lot est compilé pour la première fois, au lieu du plan compilé complet. La mémoire est ainsi moins sollicitée car le cache du plan n'est pas saturé de plans compilés qui ne sont pas réutilisés. 
  
  Le stub du plan compilé permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de reconnaître que ce lot ad hoc a déjà été compilé mais qu'il n'a stocké qu'un stub du plan compilé. Par conséquent, lorsque le lot est à nouveau appelé (compilé ou exécuté), le [!INCLUDE[ssDE](../../includes/ssde-md.md)] compile le lot, supprime le stub du plan compilé du cache du plan et ajoute le plan compilé complet au cache du plan. 
  
 Le stub du plan compilé est l'un des types d'objets cache contenus dans l'affichage catalogue sys.dm_exec_cached_plans. Il possède un handle SQL et un handle de plan qui sont uniques. Aucun plan d'exécution n'est associé au stub du plan compilé et interroger le handle du plan ne retourne pas de plan d'exécution de requêtes XML.  
  
 L’[indicateur de trace 8032](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) rétablit les paramètres de limitation du cache au paramètre RTM [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] qui permet en général aux caches d’être plus volumineux. Utilisez ce paramètre quand les entrées du cache fréquemment utilisées ne tiennent pas dans le cache et que l’option de configuration de serveur Optimiser pour les charges de travail ad hoc ne permet pas de résoudre le problème avec le cache du plan.  
  
> [!WARNING]  
>  L'indicateur de trace 8 032 peut altérer les performances si des caches volumineux diminuent la mémoire disponible pour les autres consommateurs, tels que le pool de mémoires tampons.  

## <a name="recommendations"></a>Recommandations
Évitez d’avoir un grand nombre de plans à usage unique dans le cache du plan. Une cause courante de ce problème apparaît lorsque les types de données des paramètres de requête ne sont pas définis de manière cohérente. Cela s’applique en particulier à la longueur des chaînes mais peut s’appliquer aussi à n’importe quel type de données ayant un maxlength, une précision ou une échelle. Par exemple, si un paramètre nommé @Greeting est passé comme nvarchar (10) sur un appel, et comme nvarchar (20) lors du prochain appel, des plans distincts sont créés pour chaque taille de paramètre. Si une requête contient plusieurs paramètres et qu’ils ne sont pas définis de façon cohérente lorsqu’elle est appelée, un grand nombre de plans de requête peut exister pour chaque requête. Les plans peuvent exister pour chaque combinaison de longueurs et de types de données de paramètre de requête qui a été utilisée.

Si le nombre de plans d’usage unique prend une partie significative de la mémoire du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sur un serveur OLTP et que ces plans sont des plans Ad hoc, utilisez cette option de serveur pour réduire l’utilisation de la mémoire avec ces objets.
Pour trouver le nombre de plans d’usage unique mis en cache, exécutez la requête suivante :

```sql
SELECT objtype, cacheobjtype, 
  AVG(usecounts) AS Avg_UseCount, 
  SUM(refcounts) AS AllRefObjects, 
  SUM(CAST(size_in_bytes AS bigint))/1024/1024 AS Size_MB
FROM sys.dm_exec_cached_plans
WHERE objtype = 'Adhoc' AND usecounts = 1
GROUP BY objtype, cacheobjtype;
```

> [!IMPORTANT]
> Attribuer la valeur 1 à l'option **Optimiser pour les charges de travail ad hoc** affecte uniquement les nouveaux plans ; les plans qui se trouvent déjà dans le cache du plan ne sont pas concernés.
> Pour affecter immédiatement les plans de requête déjà mis en cache, le cache du plan doit être désactivé à l’aide de [ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md), ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit redémarrer.

## <a name="see-also"></a> Voir aussi  
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
  
  
