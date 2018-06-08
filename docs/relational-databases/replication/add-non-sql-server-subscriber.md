---
title: Ajouter un Abonné non-SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.addnonsqlsubscriber.f1
ms.assetid: 21beeaa0-4b9e-48da-be63-1b9ff14e03d2
caps.latest.revision: 11
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 97688beba720f9bd99d5b621d0ae5abda70be6c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-non-sql-server-subscriber"></a>Ajouter un Abonné non-SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication prend en charge la création d'abonnement par envoi de données (push) à des publications d'instantané et transactionnelles pour les abonnés Oracle et IBM DB2.  
  
## <a name="options"></a>Options  
 **Type d'Abonné à ajouter**  
 Sélectionnez un abonné Oracle ou IBM DB2. Pour plus d’informations sur la prise en charge de ces Abonnés, consultez [Abonnés non-SQL Server](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
 **Nom de la source des données**  
 Nom utilisé pour localiser la base de données sur un réseau. La réplication produit une chaîne de connexion pour la base de données en utilisant le nom de la source de données, combiné à la connexion, au mot de passe et à toute autre option spécifiée dans la page **Sécurité de l'Agent de distribution** de cet Assistant.  
  
> [!NOTE]  
>  Le nom de la source de données et la chaîne de connexion ne sont pas validés par [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tant que l’Agent de distribution n’a pas tenté d’initialiser l’abonnement.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
