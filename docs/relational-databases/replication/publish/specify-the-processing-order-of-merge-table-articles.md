---
title: Spécifier l’ordre de traitement d’articles de table de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- articles [SQL Server replication], processing order
- merge replication [SQL Server replication], article processing order
ms.assetid: 9fe576a2-f5fb-4fdf-bd7d-cb322021b669
caps.latest.revision: 33
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6d0a1519c85e1b398fc4be3491b680997cac01b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-the-processing-order-of-merge-table-articles"></a>Spécifier l’ordre de traitement d’articles de table de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication de fusion vous permet de spécifier l'ordre dans lequel les articles sont traités par l'Agent de fusion pendant le processus de synchronisation. Vous pouvez attribuer par programme un ordre à chaque article lors de la création de l'article à l'aide des procédures stockées de réplication. Les articles sont traités à partir de la valeur la plus faible vers la valeur la plus élevée. Si deux articles ont la même valeur, ils sont traités simultanément. Pour plus d’informations, consultez [Spécifier l’ordre de traitement d’articles de fusion](../../../relational-databases/replication/merge/specify-the-processing-order-of-merge-articles.md).  
  
### <a name="to-specify-the-processing-order-for-a-new-merge-article"></a>Pour spécifier l'ordre de traitement d'un nouvel article de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez une valeur entière qui représente l'ordre de traitement de l'article pour **@processing_order**. Pour plus d'informations, voir [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Lorsque vous créez des articles ordonnés, vous devez laisser des intervalles entre les valeurs d'ordre des articles. Cela permet de définir facilement de nouvelles valeurs dans le futur. Par exemple, si vous avez trois articles pour lesquels vous devez spécifier un ordre de traitement fixe, affectez à **@processing_order** les valeurs 10, 20 et 30 plutôt que 1, 2 et 3, respectivement.  
  
### <a name="to-change-the-processing-order-of-a-merge-article"></a>Pour modifier l'ordre de traitement d'un article de fusion  
  
1.  Pour déterminer l’ordre de traitement d’un article, exécutez [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) et notez la valeur de **processing_order** dans le jeu de résultats.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Spécifiez une valeur de **processing_order** pour **@property** et une valeur entière qui représente l'ordre de traitement pour **@value**.  
  
## <a name="see-also"></a> Voir aussi  
 [Spécifier l’ordre de traitement d’articles de fusion](../../../relational-databases/replication/merge/specify-the-processing-order-of-merge-articles.md)  
  
  
