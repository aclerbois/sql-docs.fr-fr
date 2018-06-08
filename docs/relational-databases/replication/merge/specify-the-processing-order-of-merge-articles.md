---
title: Spécifier l’ordre de traitement d’articles de fusion | Microsoft Docs
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
helpviewer_keywords:
- articles [SQL Server replication], processing order
- merge replication [SQL Server replication], article processing order
ms.assetid: d151e2c5-cf50-4cb3-a829-8f32455dbd66
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b8c57a37c62881693eb96a3125adbb1999077ba3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-the-processing-order-of-merge-articles"></a>Spécifier l'ordre de traitement d'articles de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  À partir de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], il est possible de remplacer l'ordre de traitement par défaut des articles pour les publications de fusion. Ceci est utile par exemple si vous définissez une intégrité référentielle via des déclencheurs et que ces déclencheurs doivent s'exécuter dans un certain ordre.  
  
 **Pour spécifier l'ordre de traitement des articles**  
  
-   Programmation Transact-SQL de la réplication : [Spécifier l’ordre de traitement d’articles de table de fusion &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/publish/specify-the-processing-order-of-merge-table-articles.md)  
  
## <a name="how-processing-order-is-determined"></a>Détermination de l'ordre de traitement  
 Lors d'une synchronisation de fusion, les articles sont par défaut traités dans l'ordre requis par les dépendances entre les objets, et notamment les contraintes d'intégrité référentielle déclarative (DRI, Declarative Referential Integrity) définies sur les tables de base de données. Le traitement implique l'énumération des modifications apportées à une table, puis l'application de ces modifications. Si aucune intégrité référentielle déclarative n'est présente mais que des filtres de jointure ou des enregistrements logiques existent entre les articles des tables, les articles sont traités dans l'ordre requis par les filtres et les enregistrements logiques. Les articles non liés à d’autres articles via une intégrité référentielle déclarative, des filtres de jointure, des enregistrements logiques ou d’autres dépendances sont traités en fonction du surnom de l’article dans la table système [sysmergearticles &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/sysmergearticles-transact-sql.md).  
  
 Supposons une publication qui contient les tables **SalesOrderHeader** et **SalesOrderDetail** avec une colonne clé primaire **SalesOrderID** pour la table **SalesOrderHeader** et une colonne clé étrangère **SalesOrderID** pour la table **SalesOrderDetail** . Lors de la synchronisation, la réplication de fusion empêche les violations de clé étrangère en insérant toutes les nouvelles lignes dans **SalesOrderHeader** avant d'insérer les lignes associées dans **SalesOrderDetail**. De la même façon, les lignes sont supprimées dans **SalesOrderDetail** avant que la ligne associée soit supprimée dans **SalesOrderHeader**.  
  
 Cependant, dans certaines applications, l'intégrité référentielle est appliquée via des déclencheurs de base de données ou bien au niveau de l'application, et non pas via une intégrité référentielle déclarative. Étant donnée la publication décrite plus haut, à la place de l'intégrité référentielle déclarative, la table **SalesOrderDetail** pourrait avoir un déclencheur d'insertion qui vérifie que la ligne associée dans la table **SalesOrderHeader** existe avant d'autoriser une insertion. **SalesOrderHeader** pourrait avoir un déclencheur de suppression qui vérifie qu'il n' y a pas de ligne associée dans la table **SalesOrderDetail** avant d'autoriser une suppression. La réplication de fusion ne prend pas en compte les déclencheurs lors de la détermination de l'ordre de traitement des articles car elle ne peut pas déterminer ce que sera le résultat du déclencheur avant qu'il soit exécuté. De la même façon, la réplication ne peut pas prendre en compte les contraintes définies au niveau de l'application.  
  
 Quand l'intégrité référentielle est gérée via des déclencheurs ou au niveau de l'application, vous devez spécifier l'ordre dans lequel les articles doivent être traités. Dans l'exemple avec les déclencheurs, vous pouvez spécifier que la table **SalesOrderHeader** doit être traitée avant **SalesOrderDetail**, car l'ordre des articles est basé sur l'ordre d'insertion. La réplication de fusion inverse automatiquement l'ordre pour les suppressions. La réplication de fusion n'échoue pas s'il n'y a pas d'ordre pour les articles, car l'Agent de fusion continue à traiter les articles si une violation de contrainte se produit ; il essaye ensuite à nouveau toutes les opérations qui ont échoué, après le traitement des autres articles. La spécification de l'ordre des articles évite simplement les nouveaux essais et le temps de traitement supplémentaire qui y est associé. Si vous spécifiez un ordre incorrect (par exemple un ordre tel que les enregistrements de détail soient traités avant les enregistrements d'en-tête), la réplication de fusion essaye à nouveau le traitement jusqu'à ce qu'il réussisse.  
  
## <a name="see-also"></a> Voir aussi  
 [Options d’articles pour la réplication de fusion](../../../relational-databases/replication/merge/article-options-for-merge-replication.md)   
 [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)   
 [Filtres de jointure](../../../relational-databases/replication/merge/join-filters.md)  
  
  
