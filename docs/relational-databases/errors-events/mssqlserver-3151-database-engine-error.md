---
title: MSSQLSERVER_3151 | Microsoft Docs
ms.custom: 
ms.date: 04/04/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: errors-events
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
helpviewer_keywords:
- 3151 (Database Engine error)
ms.assetid: a8657a91-ec75-4649-a09a-21920e0030ff
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 6619b231e79efa08e43092de66671e27f78b1fd6
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="mssqlserver3151"></a>MSSQLSERVER_3151
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|3151|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LDDB_MASTER_LOAD_FAILED|  
|Texte du message|Échec de la restauration de la base de données master. Arrêt du serveur SQL Server. Check the error logs, and rebuild the master database. Pour plus d'informations sur la façon de reconstruire la base de données master, consultez la documentation en ligne de SQL Server.|  
  
## <a name="explanation"></a>Explication  
Ceci est un message d’erreur général qui indique des problèmes variés avec la base de données **MASTER**.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Consultez les journaux des erreurs pour plus d'informations. Pour créer une base de données **MASTER** utilisable, exécutez Setup.exe avec l’option REBUILDDATABASE. Pour plus d'informations, consultez « Procédure : installer SQL Server à partir de l'invite de commandes » dans la documentation en ligne de SQL Server.  
  
