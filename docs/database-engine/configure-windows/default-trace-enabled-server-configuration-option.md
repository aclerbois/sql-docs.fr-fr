---
title: default trace enabled (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], traces
- traces [SQL Server], logs
- default trace enabled option
ms.assetid: 1322d668-44f4-469e-8fd6-e0d02a81c8f2
caps.latest.revision: 36
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d412f30057436bf10903b616039808b1a546c211
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="default-trace-enabled-server-configuration-option"></a>default trace enabled (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Utilisez l’option **default trace enabled** pour activer ou désactiver les fichiers journaux de trace par défaut. La fonctionnalité de trace par défaut comprend un journal enrichi et cohérent sur l'activité et les modifications principalement liées aux options de configuration.  
  
> [!WARNING]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des événements étendus.  
  
## <a name="purpose"></a>Fonction  
 La trace par défaut aide les administrateurs de bases de données à résoudre les problèmes en leur fournissant les données de journal qui leur permettent de les diagnostiquer dès leur première occurrence.  
  
## <a name="viewing"></a>Affichage  
 Les journaux de trace par défaut peuvent être ouverts et examinés par le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ou interrogés avec [!INCLUDE[tsql](../../includes/tsql-md.md)] par le biais de la fonction système `fn_trace_gettable` . [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] peut ouvrir les fichiers journaux de trace par défaut comme n’importe quel fichier de résultat de trace normal. Par défaut, le journal de trace par défaut est stocké dans le répertoire `\MSSQL\LOG` au moyen d'un fichier de substitution. Le nom de fichier de base du journal de trace par défaut est `log.trc`. Dans une installation classique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la trace par défaut est activée et devient donc TraceID 1. Si elle est activée après l'installation et après la création d'autres traces, la trace TraceID peut s’incrémenter.  
  
 Pour plus d’informations sur l’utilisation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler pour afficher ce fichier de trace, consultez [Ouvrir un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md).  
  
### <a name="example"></a>Exemple :  
 L'instruction suivante ouvre le journal de trace par défaut dans l'emplacement par défaut :  
  
```  
SELECT *   
FROM fn_trace_gettable  
('C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\LOG\log.trc', default);  
GO  
  
```  
  
## <a name="configuring"></a>Configuration  
 Quand elle a la valeur 1, l’option **default trace enabled** active la **trace par défaut**. Le paramètre par défaut pour cette option est 1 (ON). La valeur 0 désactive la trace.  
  
 L’option **default trace enabled** est une option avancée. Si vous utilisez la procédure stockée système **sp_configure** pour changer sa valeur, vous ne pouvez modifier l’option **default trace enabled** que si l’option **show advanced options** a la valeur 1. Le paramètre prend effet immédiatement (sans redémarrage du serveur).  
  
## <a name="see-also"></a> Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
