---
title: MSSQL_ENG020598 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- MSSQL_ENG020598 error
ms.assetid: 1c3032f2-23d1-4ead-94ee-16ac4d75cd0c
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ce68b9d2c976ffba48b7b983e8ae224afbcc57dd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng020598"></a>MSSQL_ENG020598
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|20598|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|La ligne n'a pas été trouvée chez l'Abonné lorsque la commande répliquée est appliquée.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur est générée dans la réplication transactionnelle lorsque l'Agent de distribution tente de mettre à jour sur l'Abonné, mais que la ligne a été supprimée ou que la clé primaire de la ligne a été modifiée. Par défaut, les Abonnés à des publications transactionnelles doivent être traités en lecture seule, parce que les changements ne sont pas propagés vers le serveur de publication. Dans le cas de la réplication transactionnelle, les modifications des utilisateurs doivent être effectuées sur l'Abonné, uniquement si les abonnements devant être mis à jour ou la réplication d'égal à égal sont utilisés. Pour plus d'informations sur ces options, consultez [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md) et [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 **Pour rectifier ce problème :**  
  
1.  Si la réplication doit continuer pendant que vous identifiez la source de l’erreur, spécifiez le paramètre **-SkipErrors 20598** pour l’Agent de distribution. L'agent peut alors ignorer les modifications qui provoquent l'erreur 20598, tout en permettant la réplication des autres modifications.  
  
2.  Identifiez sur l'Abonné les modifications qui ont été supprimées ou qui comportent une clé primaire différente de celle des lignes correspondantes sur le serveur de publication. Vous pouvez utiliser l' [tablediff Utility](../../tools/tablediff-utility.md) pour déterminer les lignes qui sont différentes dans les bases de données de publication et d'abonnement. Pour obtenir des informations sur l’utilisation de cet utilitaire avec des bases de données répliquées, consultez [Comparer des tables répliquées pour identifier les différences &#40;programmation de la réplication&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
3.  Corrigez les lignes sur l'Abonné à l'aide de l'utilitaire **tablediff** ou d'une autre méthode.  
  
4.  (Facultatif) Supprimez le paramètre **-SkipErrors** .  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
