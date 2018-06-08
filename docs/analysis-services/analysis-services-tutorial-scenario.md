---
title: Scénario du didacticiel de Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9a564052f268120fb02baefdf7991978bf24bfaa
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-tutorial-scenario"></a>Scénario du didacticiel Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]
Ce didacticiel est basé sur [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)], une société fictive. [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] est une grande entreprise multinationale spécialisée dans la fabrication et la distribution de métaux et de pièces détachées de vélos pour les marchés d'Amérique du Nord, d'Europe et d'Asie. Le siège de la société [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] est situé dans la ville de Bothell, à Washington, où 500 personnes sont employées. Par ailleurs, [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] emploie plusieurs équipes commerciales régionales pour couvrir son marché.  
  
Il y a quelques années, [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] a acheté une petite usine de fabrication, Importadores Neptuno, située au Mexique. Importadores Neptuno fabrique des sous-composants importants qui entrent dans la fabrication de la ligne de produits [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] . Ces sous-composants sont livrés à Bothell pour l'assemblage du produit final. En 2005, Importadores Neptuno est devenu le seul fabricant et distributeur de bicyclettes de tourisme du groupe de produits.  
  
La société [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] a enregistré un excellent exercice fiscal et souhaite maintenant augmenter sa part de marché. Pour cela, elle envisage de mener une campagne publicitaire adaptée à ses meilleurs clients, d'augmenter la disponibilité des produits en les proposant via un site Web externe et de réduire le coût des ventes en réduisant les coûts de production.  
  
## <a name="current-analysis-environment"></a>Environnement d'analyse actuel  
Pour répondre aux besoins en analyse des données des équipes commerciales et marketing, ainsi que de la direction, l’entreprise récupère actuellement les données transactionnelles dans la base de données [!INCLUDE[ssSampleDBnormal](../includes/sssampledbnormal-md.md)] et les données non transactionnelles telles que les quotas des ventes dans des feuilles de calcul, et consolide ces données dans l’entrepôt de données relationnelles **AdventureWorksDW2012** . Cependant, l'entrepôt de données relationnelles présente les problèmes suivants :  
  
-   Les rapports sont statiques. Les utilisateurs n'ont aucun moyen d'explorer les données des rapports de façon interactive pour obtenir davantage d'informations détaillées comme ils pourraient le faire s'ils utilisaient des tableaux croisés dynamiques [!INCLUDE[msCoName](../includes/msconame-md.md)] Office Excel. Bien que les jeux de rapports prédéfinis existants répondent aux besoins de la plupart des utilisateurs, les utilisateurs plus expérimentés ont besoin d'accéder directement aux requêtes de la base de données pour utiliser des requêtes interactives et des rapports spécialisés. Cependant, du fait de la complexité de la base de données **AdventureWorksDW2012** , beaucoup trop de temps serait nécessaire à ces utilisateurs pour apprendre à créer des requêtes efficaces.  
  
-   Les performances des requêtes sont aléatoires. Par exemple, certaines requêtes renvoient des résultats très rapidement, en quelques secondes seulement, tandis que d'autres renvoient les résultats au bout de plusieurs minutes.  
  
-   Les tables d'agrégation sont difficiles à gérer. En vue d'améliorer les temps de réponse des requêtes, l'équipe chargée de l'entrepôt de données chez [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] a créé plusieurs tables d'agrégation dans la base de données **AdventureWorksDW2012** . Par exemple, elle a créé une table qui résume les ventes par mois. Toutefois, si ces tables d'agrégation améliorent grandement les performances des requêtes, l'infrastructure créée pour maintenir à jour ces tables au fil du temps est fragile et source d'erreurs.  
  
-   La logique de calcul complexe enfouie dans les définitions des rapports est difficile à partager entre les rapports. Étant donné que cette logique d'entreprise est générée séparément pour chaque rapport, les informations de synthèse sont parfois différentes d'un rapport à l'autre. En conséquence, la direction accorde une confiance limitée aux rapports générés à partir de l'entrepôt de données.  
  
-   Les utilisateurs des différentes divisions ont des besoins différents en termes de vues de données. Ils sont gênés par les éléments de données qui ne les concernent pas.  
  
-   La logique de calcul est particulièrement importante pour les utilisateurs qui doivent utiliser des rapports spécialisés. Étant donné que ces utilisateurs doivent définir la logique de calcul séparément pour chaque rapport, il n'existe aucun contrôle centralisé sur la façon dont cette logique est définie. Par exemple, certains utilisateurs savent qu'ils devraient utiliser des techniques de statistiques de base comme les moyennes mobiles, mais ne sachant pas comment créer ces calculs, ils ne recourent pas à ces techniques.  
  
-   Il est difficile de combiner des datasets connexes. Les requêtes spécialisées qui combinent deux datasets connexes, telles que les ventes et les quotas des ventes, sont difficiles à créer pour les utilisateurs. Les requêtes de ce type surchargeant la base de données, l'entreprise a exigé des utilisateurs qu'ils obtiennent de l'équipe chargée de l'entrepôt de données des datasets de zones de sujets croisés. En conséquence, seuls quelques rapports prédéfinis combinant des données de plusieurs zones de sujets ont été définis. Par ailleurs, les utilisateurs hésitent à modifier ces rapports en raison de leur complexité.  
  
-   Les rapports portent essentiellement sur les données commerciales propres aux États-Unis. Cela satisfait peu les utilisateurs travaillant dans les filiales situées en dehors des États-Unis qui souhaitent pouvoir afficher les rapports dans des monnaies et des langues différentes.  
  
-   Les données sont difficiles à auditer. Le service financier utilise actuellement uniquement la base de données **AdventureWorksDW2012** comme source de données à partir de laquelle il effectue des requêtes en bloc. Il charge ensuite ces données dans des feuilles de calcul individuelles et passe beaucoup de temps à organiser les données et à manipuler les feuilles de calcul. Les rapports financiers de l'entreprise sont par conséquent difficiles à préparer, auditer et gérer au niveau de l'entreprise.  
  
## <a name="the-solution"></a>La solution  
L'équipe chargée de l'entrepôt de données a récemment examiné la conception du système d'analyse actuel. Cet examen a permis d'analyser les écarts entre les problèmes présents et les demandes futures. Cette équipe a pu déterminer que la base de données **AdventureWorksDW2012** était une base de données dimensionnelles bien conçue avec des dimensions conformes et des clés de substitution. Les dimensions conformes permettent à une dimension d'être utilisée dans plusieurs mini-Data Warehouses, telle qu'une dimension de temps ou une dimension de produits. Les clés de substitution sont des clés artificielles qui lient les dimensions et les tables de faits et qui sont utilisées pour garantir l'unicité et améliorer les performances. Par ailleurs, l'équipe chargée de l'entrepôt de données a déterminé qu'il n'existait actuellement aucun problème important concernant le chargement et la gestion des tables de base dans la base de données **AdventureWorksDW2012** . Cette équipe a par conséquent décidé d'utiliser [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour accomplir les opérations suivantes :  
  
-   fournir un accès unifié aux données via une couche de métadonnées communes pour le traitement analytique et les rapports ;  
  
-   simplifier les vues de données des utilisateurs, ce qui accélère le développement des requêtes interactives et prédéfinies et des rapports prédéfinis ;  
  
-   créer correctement des requêtes qui combinent des données issues de plusieurs zones de sujet ;  
  
-   gérer les agrégations ;  
  
-   stocker et réutiliser des calculs complexes ;  
  
-   permettre aux utilisateurs de l'entreprise en dehors des États-Unis d'utiliser des versions localisées.  
  
Les leçons du didacticiel Analysis Services fournissent de l'aide en générant une base de données de cube qui répond à tous ces critères. Pour commencer, passez à la première leçon : [Lesson 1: Create a New Tabular Model Project](../analysis-services/lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="see-also"></a>Voir aussi  
[Modélisation multidimensionnelle & #40 ; Didacticiel Adventure Works & #41 ;](../analysis-services/multidimensional-modeling-adventure-works-tutorial.md)  
  
  
  
