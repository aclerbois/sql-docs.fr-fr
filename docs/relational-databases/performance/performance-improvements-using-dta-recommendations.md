---
title: Optimisation des performances à l’aide des recommandations DTA | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, performance improvements
ms.assetid: 2e51ea06-81cb-4454-b111-da02808468e6
caps.latest.revision: 3
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 748a0ca7d58da911b8144ae56f48d1bdccc8d484
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="performance-improvements-using-dta-recommendations"></a>Optimisation des performances à l’aide des recommandations DTA
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]


---
Les performances des charges de travail de stockage et d’analyse peuvent grandement profiter des index **columnstore**, en particulier pour des requêtes qui doivent analyser de grandes tables. Les index **rowstore** (arborescence B+) sont particulièrement efficaces pour des requêtes qui accèdent à des quantités de données relativement petites, à la recherche d’une valeur particulière ou d’une plage de valeurs particulière. Puisque les index rowstore peuvent fournir des lignes dans un ordre de tri, ils peuvent également réduire le coût du tri dans des plans d’exécution de requête. Par conséquent, le choix de la combinaison des index rowstore et columnstore à générer dépend de la charge de travail de votre application.

L’Assistant Paramétrage du moteur de base de données (DTA), à compter de SQL Server 2016, peut recommander une **combinaison d’index rowstore et columnstore** adéquate en analysant une charge de travail de base de données donnée. 

Pour démontrer les avantages des recommandations de l’Assistant DTA sur les performances de charge de travail, nous avons expérimenté plusieurs charges de travail client réelles. Pour chaque charge de travail client, nous avons laissé l’Assistant DTA analyser les requêtes individuelles, ainsi que la charge de travail complète des requêtes. Nous considérons trois possibilités :
  
  1. **ColumnStore uniquement** : générer uniquement des index columnstore pour toutes les tables sans utiliser l’Assistant DTA. 
  2. **DTA (rowstore uniquement)**  : exécuter l’Assistant DTA avec l’option permettant de recommander les index rowstore uniquement.
  3. **DTA (rowstore + columnstore)**  : exécuter l’Assistant DTA avec l’option permettant de recommander à la fois les index rowstore et columnstore.  
   
Dans chaque cas, nous avons ensuite implémenté les index recommandés. Nous signalons le temps processeur (en millisecondes) moyen sur plusieurs exécutions de la requête ou de la charge de travail. La figure ci-dessous indique le temps processeur en millisecondes pour des charges de travail sur deux bases de données client différentes. Notez que l’axe des y (temps processeur) utilise une échelle logarithmique.   


![DTA-columnstore-rowstore-performance](../../relational-databases/performance/media/dta-columnstore-rowstore-performance.gif)



**Besoin en conceptions physiques mixtes** : premier ensemble de barres correspondant à la requête 1 du client 1. L’Assistant DTA (rowstore + columnstore) recommande un ensemble de quatre index columnstore et de six index rowstore, ce qui entraîne un temps processeur 2,5 – 4 fois plus faible que celui pour l’index columnstore uniquement et DTA (rowstore uniquement). Cela montre les avantages des conceptions physiques mixtes comprenant des index rowstore et columnstore *même pour une seule requête*. 

**Efficacité des recommandations d’index rowstore** : les deuxième et troisième ensembles de barres (correspondants à la requête 2 du client 1 et à la requête 1 du client 2) sont des cas où les requêtes ont des prédicats de filtre sélectifs qui tirent parti des index rowstore appropriés. Pour ces deux requêtes, l’Assistant DTA (rowstore uniquement) et l’Assistant DTA (rowstore + columnstore) recommandent des index rowstore uniquement. Ces exemples illustrent également que même si l’Assistant DTA est appelé avec l’option permettant de recommander des index columnstore, son approche basée sur les coûts garantit qu’il recommande un index columnstore uniquement si la charge de travail peut en fait en bénéficier.

**Efficacité des recommandations d’index columnstore** : le quatrième ensemble de barres correspondant à la requête 2 du client 2 représente un cas où la requête analyse de grandes tables qui tireraient parti des index columnstore. L’Assistant DTA (rowstore uniquement) génère une recommandation dont le temps processeur est supérieur à celui correspondant à des index columnstore présents. L’Assistant DTA (rowstore + columnstore) recommande des index columnstore appropriés, correspondant à la performance d’exécution de requête de l’option columnstore uniquement.

**Efficacité des recommandations pour la charge de travail avec plusieurs requêtes** : le dernier ensemble de barres correspondant à la charge de travail complète pour le client 2 illustre la capacité de l’Assistant DTA à analyser plusieurs requêtes dans la charge de travail pour recommander un ensemble approprié d’index rowstore et columnstore pouvant améliorer le coût d’exécution de la charge de travail globale. L’Assistant DTA (rowstore + columnstore) recommande quatre index columnstore et des dizaines d’index rowstore qui entraînent une amélioration au-delà d’un ordre de grandeur pour la charge de travail par rapport à l’option qui génère uniquement des index columnstore ; et une amélioration environ 4-5 fois supérieure à celle correspondant à l’Assistant DTA (rowstore uniquement).

En résumé, les exemples ci-dessus illustrent la capacité de l’Assistant DTA à exploiter correctement les index rowstore et columnstore pris en charge dans le moteur de base de données SQL Server. Ils recommandent aussi une combinaison appropriée des index, susceptible de réduire considérablement le temps processeur pour la charge de travail. 

<a name="see-also"></a> Voir aussi
---
[Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/database-engine-tuning-advisor.md)

[Recommandations relatives aux index columnstore dans l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md)

[Guide des index columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)

[Index columnstore pour l’entreposage des données](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)

[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)

[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)



