---
title: Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul | Microsoft Docs
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
- merge replication [SQL Server replication], download-only articles
- articles [SQL Server replication], download-only
- download-only articles
ms.assetid: 8851faa6-e6df-4ea5-a6ea-2a3471680fa3
caps.latest.revision: 37
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e3b8384afc96be2b71a1f2c51bbfbbd35920562e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="optimize-merge-replication-performance-with-download-only-articles"></a>Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication de fusion propose deux types d'articles différents permettant de répondre aux différents besoins des applications. Les publications peuvent contenir un ou plusieurs de chacun de ces types d'articles en fonction de l'application :  
  
-   Articles standards  
  
-   articles disponibles uniquement par téléchargement  
  
 Les articles en téléchargement seul offrent de meilleures performances par rapport aux articles standards et doivent être utilisés au moment opportun.  
  
> [!NOTE]  
>  Afin d'utiliser les articles en téléchargement seul, le niveau de compatibilité de la publication doit être au moins 90RTM.  
  
## <a name="standard-articles"></a>Articles standards  
 Les articles standards sont les articles par défaut, et offrent la gamme complète des fonctionnalités de la réplication de fusion, y compris la détection et la résolution des conflits. Les articles standard sont appropriés aux tables mises à jour par plusieurs abonnés ; les objets autres que les tables, comme les procédures stockées et les vues, sont toujours publiés en tant qu'articles standard.  
  
## <a name="download-only-articles"></a>articles disponibles uniquement par téléchargement  
 Les articles en téléchargement seul sont conçus pour les applications dont les données ne sont pas mises à jour sur les abonnés, comme un ensemble d'articles ne faisant pas partie d'un catalogue de produits. Un catalogue de produits est généralement mis à jour sur le serveur de publication, mais pas sur les abonnés. Étant donné que les articles disponibles uniquement par téléchargement ne peuvent pas être mis à jour sur l'abonné, le suivi des métadonnées n'est pas envoyé aux abonnés. Cela peut aboutir à une réduction du stockage sur les abonnés et à un gain de performances, notamment si la connexion réseau est lente.  
  
 Les articles en téléchargement uniquement fonctionnent conjointement avec les abonnements clients : si un article est considéré comme en téléchargement seul, les lignes de cet article ne peuvent pas être insérées, mises à jour ou supprimées pour les abonnés utilisant des abonnements clients. Les serveurs de publication et les abonnés qui utilisent le type d'abonnement serveur (généralement les abonnés qui publient à nouveau des données sur d'autres abonnés) peuvent insérer, mettre à jour et supprimer des données. Pour plus d’informations sur les abonnements clients, consultez [S’abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md).  
  
 Pour spécifier qu'un article est en téléchargement seul, consultez [Specify That a Merge Table Article is Download-Only](../../../relational-databases/replication/publish/specify-that-a-merge-table-article-is-download-only.md).  
  
## <a name="using-different-article-types-in-your-applications"></a>Utilisation de différents types d'articles dans vos applications  
 Grâce à la compréhension des besoins de votre application, vous pouvez faire des compromis entre flexibilité maximale et performance optimale. Par exemple, les applications comportant de nombreux conflits et modifications, à la fois sur le serveur de publication et sur les abonnés, utiliseront une publication composée d'articles standards. Certaines applications, comme une application d'automatisation des forces de vente, peuvent comporter des articles potentiellement conflictuels et d'autres articles fonctionnant comme des tables de correspondances, pouvant être spécifiés comme étant en téléchargement seul. Les applications d'entrée de données, comme les systèmes de points de vente et les applications d'automatisation des groupes opérationnels, partitionnent souvent les données de façon à éliminer les conflits, et les données d'un abonné ne vont jamais vers un autre. Dans ces situations, une combinaison de partitions ne se chevauchant pas, d'articles en téléchargement seul et de partitions précalculées offre des performances et une évolutivité maximales. Pour plus d'informations sur les partitions ne se chevauchant pas et les partitions précalculées, consultez [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Options d’articles pour la réplication de fusion](../../../relational-databases/replication/merge/article-options-for-merge-replication.md)   
 [Optimiser les performances de la réplication de fusion avec le suivi conditionnel des suppressions](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-conditional-delete-tracking.md)  
  
  
