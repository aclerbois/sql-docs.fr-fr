---
title: MSSQL_ENG014157 | Microsoft Docs
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
- MSSQL_ENG014157 error
ms.assetid: 1a0890cf-d977-43e0-a2ba-9c5ff1a8f856
caps.latest.revision: 17
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 531a4df4ea6212090129b4ed8d172c16f2cfc11e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssqleng014157"></a>MSSQL_ENG014157
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Détails du message  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|14157|  
|Source de l'événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|L'abonnement créé par l'Abonné '%1!' à la publication '%2!' a expiré et a été supprimé.|  
  
## <a name="explanation"></a>Explication  
 Un Abonné doit effectuer une synchronisation avec le serveur de publication dans les délais définis par la période de rétention de la publication. Si un Abonné n'effectue pas la synchronisation dans cette période, l'abonnement expire. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 L'abonnement doit être recréé et initialisé avant que l'Abonné ne recommence à recevoir des modifications de données :  
  
-   Pour plus d’informations sur la création d’abonnements, consultez [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md).  
  
-   Pour plus d’informations sur l’initialisation des abonnements, consultez [Initialiser un abonnement](../../relational-databases/replication/initialize-a-subscription.md).  
  
 Si la base de données d'abonnement contient des modifications qui n'ont pas été synchronisées avec le serveur de publication, vous pouvez utiliser l' [tablediff Utility](../../tools/tablediff-utility.md) pour identifier les lignes qui sont différentes dans les bases de publication et les bases d'abonnement.  
  
 Vous pouvez augmenter la période de rétention de la publication afin d'empêcher l'expiration des abonnements. Faites preuve de la plus grande vigilance lors de la définition d'une valeur élevée car plus le nombre de données et de métadonnées stockées est important, moins les performances sont bonnes. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
