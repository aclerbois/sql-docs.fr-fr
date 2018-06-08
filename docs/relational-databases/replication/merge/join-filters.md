---
title: Filtres de jointure | Microsoft Docs
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
helpviewer_keywords:
- filters [SQL Server replication], join
- publications [SQL Server replication], join filters
- merge replication join filters [SQL Server replication]
- join filters
ms.assetid: dd78fd8f-56e3-4582-9abd-6bc25c91e075
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 5776139f4f461070400ab05c09c5c484b1b077b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="join-filters"></a>Filtres de jointure
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Un filtre de jointure permet de filtrer une table en fonction du filtrage d'une table associée dans la publication. En général, une table parente est filtrée à l'aide d'un filtre paramétré ; un ou plusieurs filtres de jointure sont ensuite définis à peu près de la même façon que vous définissez une jointure entre des tables. Les filtres de jointure étendent le filtre paramétré de telle façon que les données des tables liées soient répliquées seulement si elles correspondent à la clause du filtre de jointure.  
  
 En règle générale, les filtres de jointure suivent les relations clé primaire/clé étrangère définies pour les tables auxquelles ils sont appliqués, mais ils ne sont pas strictement limités à ces relations. Le filtre de jointure peut être basé sur toute logique comparant des données associées dans deux tables.  
  
 Supposons les tables suivantes dans la base de données exemple [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] , qui sont associées via des relations de clé primaire à clé étrangère :  
  
-   **HumanResources.Employee**  
  
-   **Sales.SalesOrderHeader**  
  
-   **Sales.SalesOrderDetail**  
  
 Ces tables peuvent être utilisées dans une application d'aide aux forces de ventes mobiles, mais elles doivent être filtrées de façon à ce que chaque commercial de la table **HumanResources.Employee** reçoive seulement les données se rapportant aux commandes de ses clients.  
  
 La première étape consiste à définir un filtre paramétré sur la table parente, qui est dans cet exemple la table **HumanResources.Employee** . Cette table comprend la colonne **LoginID**, qui contient le nom de connexion de chaque employé sous la forme *domain\login*. Pour filtrer cette table de façon à ce que chaque employé reçoive seulement les données qui le concernent, spécifiez la clause de filtre paramétré suivante :  
  
```  
LoginID = SUSER_SNAME()  
```  
  
 Ce filtre garantit que l'abonnement de chaque employé contient seulement les données de la table **HumanResources.Employee** qui se rapportent à cet employé (une seule ligne dans le cas présent). Pour plus d’informations, voir [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 L'étape suivante consiste à étendre ce filtre à chacune des tables associées, à l'aide d'une syntaxe similaire à celle qui est utilisée pour spécifier une jointure entre deux tables. La première clause du filtre de jointure est :  
  
```  
Employee.EmployeeID = SalesOrderHeader.SalesPersonID  
```  
  
 Ceci fait que l'abonnement contient seulement les données des commandes concernant chacun des commerciaux. La seconde clause du filtre de jointure est :  
  
```  
SalesOrderHeader.SalesOrderID = SalesOrderDetail.SalesOrderID  
```  
  
 Ceci fait que l'abonnement contient seulement les données détaillées associées aux données des commandes pour chacun des commerciaux. Cet exemple montre une table jointe à chaque point ; il est également possible de joindre plusieurs tables à chaque point.  
  
 Les filtres de jointure peuvent être ajoutés un par un via l'Assistant Nouvelle publication et la boîte de dialogue **Propriétés de la publication** , ou bien ils peuvent être ajoutés par programmation. Ils peuvent aussi être générés automatiquement via l'Assistant Nouvelle publication : vous spécifiez un filtre de lignes pour une table et des filtres de jointure sont appliqués à toutes les tables associées. Pour plus d’informations, consultez [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md), [Générer automatiquement un ensemble de filtres de jointure entre des articles de fusion &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md), et [Définir un article](../../../relational-databases/replication/publish/define-an-article.md).  
  
## <a name="optimizing-join-filter-performance"></a>Optimisation des performances des filtres de jointure  
 Les performances des filtres de jointure peuvent être optimisées en suivant ces conseils :  
  
-   Limitez le nombre de tables dans la hiérarchie du filtre de jointure.  
  
     Les filtres de jointure peuvent impliquer un nombre illimité de tables, mais des filtres avec un grand nombre de tables peuvent avoir un impact significatif sur les performances lors du traitement de la fusion. Si vous générez des filtres de jointure pour au moins cinq tables, envisagez d'autres solutions : ne filtrez pas les petites tables, les tables qui ne changent pas ou les tables de recherche principales. Utilisez des filtres de jointure seulement entre des tables qui doivent être partitionnées entre des abonnements.  
  
-   Définissez l'option **join unique key** à **True** là où c'est approprié.  
  
     Le traitement de fusion a des possibilités d'optimisation des performances spéciales si la colonne jointe dans le parent est unique. Si la condition de jointure est basée sur une colonne unique, définissez l'option **join unique key** pour le filtre de jointure. Pour des informations sur la définition de cette option, consultez les rubriques Procédure dont la liste figure dans la section précédente.  
  
-   Vérifiez que les colonnes référencées dans les filtres de jointure sont indexées.  
  
     Si les colonnes référencées dans le filtre sont indexées, la réplication peut traiter les filtres plus efficacement.  
  
-   Ne créez pas de filtres de lignes équivalents aux filtres de jointure.  
  
     Il est possible de créer des filtres de lignes qui sont équivalents à des filtres de jointure en utilisant une sous-requête dans une clause WHERE, comme ceci :  
  
    ```  
    WHERE Customer.SalesPersonID IN (SELECT EmployeeID FROM Employee WHERE LoginID = SUSER_SNAME())   
    ```  
  
     Il est fortement recommandé que toutes les logiques de ce type soient exprimées dans un filtre de jointure plutôt que dans une sous-requête. Si votre application requiert qu'un filtre de lignes utilise une sous-requête, vérifiez que la sous-requête référence seulement des données de consultation qui ne font pas l'objet de modifications.  
  
## <a name="see-also"></a> Voir aussi  
 [Filtrer des données publiées en vue de la réplication de fusion](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)   
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
