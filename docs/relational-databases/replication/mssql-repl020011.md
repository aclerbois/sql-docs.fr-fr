---
title: MSSQL_REPL020011 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_REPL020011 error
ms.assetid: f72072d7-bbb6-48ad-ac88-afa74aeb4d58
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 32c913300be3f8d3c2a8e9b5c5a7f06e017e4368
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqlrepl020011"></a>MSSQL_REPL020011
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|20011|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le processus n'a pas pu exécuter '%1' sur '%2'.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur peut être signalée dans plusieurs circonstances lors du traitement de la réplication transactionnelle, par exemple quand l’Agent de lecture du journal exécute **sp_replcmds** (Le processus n’a pas pu exécuter « sp_replcmds » sur \<Nom_Serveur>) ou **sp_repldone** (Le processus n’a pas pu exécuter « sp_repldone » sur \<Nom_Serveur>).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Si cette erreur est signalée dans une base de données que vous venez tout juste de restaurer à partir d'une sauvegarde, vérifiez que vous avez suivi les étapes indiquées dans la documentation sur la sauvegarde et la restauration, notamment l'exécution de **sp_replrestart** si elle est appropriée. Pour plus d’informations, voir [Strategies for Backing Up and Restoring Snapshot and Transactional Replication](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 Cette erreur est une erreur de traitement interne et, si elle est signalée dans d'autres circonstances que celle d'une restauration, elle indique généralement que la réplication doit être supprimée et reconfigurée. Si vous ne parvenez pas à supprimer la réplication, contactez le support technique pour obtenir de l'aide.  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)  
  
  
