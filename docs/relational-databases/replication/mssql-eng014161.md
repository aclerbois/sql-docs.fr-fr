---
title: MSSQL_ENG014161 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
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
- MSSQL_ENG014161 error
ms.assetid: 4b983e76-bb77-43c5-b44b-19919d3da619
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e06b9d0eb7f6249d6f386fb256845fe44fcef4ef
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng014161"></a>MSSQL_ENG014161
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|14161|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le seuil [%s:%s] de la publication [%s] a été défini. Assurez-vous que le lecteur du journal et les agents de distribution s'exécutent et peuvent respecter les conditions de latence.|  
  
## <a name="explanation"></a>Explication  
 La réplication vous permet d'activer des avertissements pour plusieurs situations. Vous pouvez, entre autres, signaler le dépassement d'une latence déterminée pour les abonnements transactionnels. La latence correspond à la durée qui s'écoule entre la validation d'une modification de données sur le serveur de publication et la validation de la modification correspondante sur l'Abonné.  
  
 Lorsque vous activez un avertissement à l'aide du moniteur de réplication ou de la procédure stockée [sp_replmonitorchangepublicationthreshold](../../relational-databases/system-stored-procedures/sp-replmonitorchangepublicationthreshold-transact-sql.md), vous spécifiez un seuil qui détermine à quel moment l'avertissement sera déclenché. Quand ce seuil est atteint ou dépassé, un avertissement s'affiche dans le moniteur de réplication et un événement est enregistré dans le journal des événements Windows. Le franchissement d'un seuil peut également déclencher une alerte de l'Agent SQL Server. Pour plus d’informations, consultez [Définir des seuils et des avertissements dans le moniteur de réplication](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md) et [Surveiller la réplication par programme](../../relational-databases/replication/monitor/programmatically-monitor-replication.md).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Si un abonnement dépasse un seuil de latence, vous devez déterminer si le système a un problème de performance ou si le seuil doit être modifié. Une fois la réplication configurée, élaborez un référentiel de performances qui vous permettra d'évaluer les performances de la réplication avec une charge de travail standard pour vos applications et votre topologie. Intégrez la latence à ce référentiel afin de pouvoir définir une valeur appropriée pour le seuil.  
  
 Si la valeur du seuil est appropriée, mais qu'elle continue à être franchie, vous devez déterminer où se trouve le goulet d'étranglement dans le système. Pour plus d'informations sur le contrôle des performances de réplication et la résolution des éventuels problèmes, consultez les rubriques suivantes :  
  
-   [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
-   [Analyser les performances avec le moniteur de réplication](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
