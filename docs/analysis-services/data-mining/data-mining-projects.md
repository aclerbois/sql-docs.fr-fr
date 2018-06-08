---
title: Projets d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1e7a4ea87642ba31693eeea6ea17bedb14c20a24
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-mining-projects"></a>Projets d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un projet d'exploration de données fait partie d'une solution [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Pendant le processus de conception, les objets que vous créez dans ce projet sont disponibles à des fins de test et d'interrogation dans le cadre d'une base de données d'espace de travail. Lorsque vous souhaitez que les utilisateurs puissent interroger ou parcourir les objets dans le projet, vous devez déployer le projet sur une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s'exécutant en mode multidimensionnel.  
  
 Cette rubrique fournit les informations de base nécessaires à la compréhension et à la création des projets d'exploration de données.  
  
 [Création de projets d'exploration de données](#bkmk_Overview)  
  
 [Objets dans les projets d'exploration de données](#bkmk_Objects)  
  
-   [Sources de données](#bkmk_DataSources)  
  
-   [Vues de source de données](#bkmk_DSV)  
  
-   [Structures d'exploration de données](#bkmk_Structures)  
  
-   [Modèles d'exploration de données](#bkmk_Models)  
  
 [Utilisation du projet d'exploration de données terminé](#bkmk_Complete)  
  
-   [Afficher et explorer les modèles](#bkmk_ViewExplore)  
  
-   [Tester et valider les modèles](#bkmk_Validate)  
  
-   [Créer des prédictions](#bkmk_Predict)  
  
 [Accès par programmation aux projets d'exploration de données](#bkmk_API)  
  
##  <a name="bkmk_Overview"></a> Création de projets d'exploration de données  
 Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous créez des projets d'exploration de données à l'aide du modèle, **Projet OLAP et expl. de données**. Vous pouvez également créer des projets d'exploration de données par programmation en utilisant AMO. Les différents objets d'exploration de données peuvent faire l'objet d'un script à l'aide du langage ASSL (Analysis Services Scripting Language). Pour plus d’informations, consultez [Accès aux données de modèles multidimensionnels &#40;Analysis Services - Données multidimensionnelles &#41;](../../analysis-services/multidimensional-models/mdx/multidimensional-model-data-access-analysis-services-multidimensional-data.md).  
  
 Si vous créez un projet d'exploration de données dans une solution existante, par défaut les objets d'exploration de données seront déployés vers une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] avec le même nom que le fichier solution. Vous pouvez modifier ce nom et le serveur cible à l'aide de la boîte de dialogue **Propriétés du projet** . Pour plus d’informations, consultez [Configurer les propriétés d’un projet Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/configure-analysis-services-project-properties-ssdt.md).  
  
> [!WARNING]  
>  Pour correctement générer et déployer votre projet, vous devez avoir accès à une instance d'[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s'exécutant en mode d'exploration de données OLAP/Data. Vous ne pouvez pas développer ou déployer des solutions d’exploration de données sur une instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui prend en charge les modèles tabulaires. Vous ne pouvez pas non plus utiliser directement les données d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou d’un modèle tabulaire qui utilise la banque de données en mémoire. Pour déterminer si l’instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que vous avez peut prendre en charge l’exploration de données, consultez [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md).  
  
 Dans chaque projet d'exploration de données que vous créez, suivez ces étapes :  
  
1.  Choisissez une *source de données*, telle qu'un cube, une base de données, ou des fichiers Excel ou texte, qui contient les données brutes que vous utiliserez pour générer des modèles.  
  
2.  Définissez un sous-ensemble des données dans la source de données à utiliser pour l'analyse, puis enregistrez-le comme *vue de source de données*.  
  
3.  Définissez une *structure d'exploration de données* pour prendre en charge la modélisation.  
  
4.  Ajoutez des *modèles d'exploration de données* à la structure d'exploration de données, en choisissant un *algorithme* et en spécifiant la façon dont l'algorithme traite les données.  
  
5.  Effectuez l'apprentissage des modèles en les remplissant avec les données sélectionnées ou un sous-ensemble filtré des données.  
  
6.  Explorez, testez et régénérez les modèles.  
  
 Lorsque le projet terminé, vous pouvez le déployer pour que les utilisateurs puissent le parcourir ou l'interroger, ou autoriser l'accès par programmation aux modèles d'exploration de données dans une application, afin de prendre en charge des prédictions et l'analyse.  
  
  
##  <a name="bkmk_Objects"></a> Objets dans les projets d'exploration de données  
 Tous les projets d'exploration de données contiennent les quatre types d'objets suivants. Vous pouvez avoir plusieurs objets de tous les types.  
  
-   Sources de données  
  
-   Vues de source de données  
  
-   Structures d'exploration de données  
  
-   Modèles d'exploration de données  
  
 Par exemple, un projet d'exploration de données unique peut contenir une référence à plusieurs sources de données et chacune de ces sources de données prend en charge plusieurs vues de source de données. Ensuite, chaque vue de source de données peut prendre en charge plusieurs structures d'exploration de données, chacune avec de nombreux modèles d'exploration de données associés.  
  
 En outre, votre projet peut inclure des algorithmes de plug-in, des assemblys personnalisés ou des procédures stockées personnalisées ; toutefois, ces objets ne sont pas décrits ici. Pour plus d’informations, consultez [Guide du développeur (Analysis Services)](../../analysis-services/analysis-services-developer-documentation.md).  
  
  
###  <a name="bkmk_DataSources"></a> Data Sources  
 La source de données définit la chaîne de connexion et les informations d'authentification que le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilisera pour se connecter à la source de données. La source de données peut contenir plusieurs tables ou vues ; elle peut être aussi simple qu'un classeur Excel ou fichier texte unique, ou aussi complexe qu'une base de données de traitement analytique en ligne (OLAP) ou qu'une vaste base de données relationnelle.  
  
 Un projet d'exploration de données unique peut référencer plusieurs sources de données. Bien qu'un modèle d'exploration de données puisse utiliser une seule source de données à la fois, le projet peut avoir plusieurs dessin de modèles sur différentes sources de données.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge des données de nombreux fournisseurs externes et l'exploration de données SQL Server peut utiliser des données relationnelles et des données du cube comme source de données. Toutefois, si vous développez les deux types de projet (modèles basés sur des sources relationnelles et modèles basés sur des cubes OLAP) vous pouvez développer et gérer ces derniers dans des projets distincts.  
  
-   En général, les modèles basés sur un cube OLAP doivent être développés dans la solution de conception OLAP. En effet, les modèles basés sur un cube doivent traiter le cube pour mettre à jour les données. En règle générale, vous devez utiliser des données de cube uniquement lorsqu'il s'agit du moyen principal de stockage et d'accès aux données, ou lorsque vous avez besoin des agrégations, des dimensions et des attributs créés par le projet multidimensionnel.  
  
-   Si votre projet utilise des données relationnelles uniquement, vous devez créer des modèles relationnels dans un projet distinct, afin de ne pas retraiter d'autres objets inutilement. Dans de nombreux cas, la base de données de la zone de transit ou l'entrepôt de données utilisé pour prendre en charge la création de cube contient déjà les vues nécessaires pour exécuter l'exploration de données, et vous pouvez utiliser ces vues pour l'exploration de données à la place des agrégations et des dimensions du cube.  
  
-   Vous ne pouvez pas utiliser directement les données en mémoire ou [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour générer des modèles d’exploration de données.  
  
 La source de données identifie uniquement le serveur ou le fournisseur et le type de données général. Si vous devez modifier la mise en forme des données et les agrégations, utilisez l'objet de vue de source de données.  
  
 Pour contrôler la façon dont les données de la source de données sont traitées, vous pouvez ajouter des colonnes dérivées ou un calcul, modifier des agrégats ou renommer des colonnes de données dans la vue de source de données. (Vous pouvez également utiliser des données en aval, en modifiant les colonnes de la structure d'exploration de données, ou en utilisant des indicateurs et des filtres de modélisation au niveau de la colonne du modèle d'exploration de données.)  
  
 Si le nettoyage de données est requis ou que les données de l'entrepôt de données doivent être modifiées pour créer des variables supplémentaires, modifier des types de données ou créer une autre agrégation, vous devrez peut-être créer des types de projet supplémentaires pour l'exploration de données. Pour plus d’informations sur ces projets connexes, consultez [Projets connexes pour des solutions d’exploration de données](../../analysis-services/data-mining/related-projects-for-data-mining-solutions.md).  
  
  
###  <a name="bkmk_DSV"></a> Data Source Views  
 Après avoir défini cette connexion vers une source de données, vous pouvez créer une vue qui identifie les données spécifiques se rapportant à votre modèle.  
  
 La vue de source de données permet également de personnaliser la manière dont les données de la source sont fournies au modèle d'exploration de données. Vous pouvez modifier la structure des données pour la rendre plus pertinente par rapport à votre projet ou ne choisir que certains types de données.  
  
 Par exemple, à l'aide de l'éditeur de vue de source de données, vous pouvez :  
  
-   Créez des colonnes dérivées, telles que des parties de date, des sous-chaînes, etc.  
  
-   Valeurs d'agrégation utilisant des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] telles que GROUP BY  
  
-   Limitez les données temporairement ou échantillonnez les données  
  
 Pour plus d’informations sur la façon dont vous pouvez modifier des données dans une vue de source de données, consultez [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md).  
  
> [!WARNING]  
>  Si vous souhaitez filtrer les données, vous pouvez effectuer cette opération dans la vue de source de données, mais vous pouvez également créer des filtres sur les données au niveau du modèle d'exploration de données. La définition de filtre étant stockée avec le modèle d'exploration de données, l'utilisation de filtres de modèle simplifie la détermination des données utilisées pour l'apprentissage du modèle. De plus, vous pouvez créer plusieurs modèles associés, avec différents critères de filtre. Pour plus d’informations, consultez [Filtres pour les modèles d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md).  
  
 Notez que la vue de source de données créée peut contenir des informations supplémentaires qui ne sont pas directement utilisées pour l'analyse. Par exemple, vous pouvez ajouter à votre vue de source de données des données utilisées pour le test, les prédictions ou l'extraction. Pour plus d’informations sur ces utilisations, consultez [Test et validation &#40;exploration de données&#41;](../../analysis-services/data-mining/testing-and-validation-data-mining.md) et [Extraction](../../analysis-services/data-mining/drillthrough-queries-data-mining.md).  
  
  
###  <a name="bkmk_Structures"></a> Mining Structures  
 Une fois que vous avez créé votre source de données et votre vue de source de données, vous devez sélectionner les colonnes de données les plus pertinentes pour votre problème professionnel, en définissant des *structures d'exploration de données* dans le projet. Une structure d'exploration de données indique au projet les colonnes de données de la vue de source de données qui doivent être utilisées réellement dans la modélisation, l'apprentissage, et le test.  
  
 Pour ajouter une nouvelle structure d'exploration de données, démarrez l'Assistant Exploration de données. L'Assistant définit automatiquement une structure d’exploration de données, vous guide tout au long de la procédure de sélection des données et, de manière facultative, vous permet d'ajouter un modèle d’exploration de données initial à la structure. Dans la structure d'exploration de données, choisissez les tables et les colonnes de la vue de source de données ou d'un cube OLAP et définissez des relations entre les tables, si vos données contiennent des tables imbriquées.  
  
 Votre choix de données aura un aspect très différent dans l'Assistant Exploration de données, selon que vous utilisez des sources de données relationnelles ou de traitement analytique en ligne (OLAP).  
  
-   Lorsque vous choisissez les données d'une source de données relationnelle, la configuration d'une structure d'exploration de données est simple : choisissez des colonnes de données dans la vue de source de données et définissez des personnalisations supplémentaires telles que des alias, ou définissez la façon dont les valeurs de la colonne doivent être groupées ou placées dans un conteneur. Pour plus d’informations, consultez [Créer une structure d’exploration de données relationnelle](../../analysis-services/data-mining/create-a-relational-mining-structure.md).  
  
-   Lorsque vous utilisez des données d'un cube OLAP, la structure d'exploration de données doit être dans la même base de données que la solution OLAP.  Pour créer une structure d'exploration de données, sélectionnez les attributs des dimensions et les mesures associées dans votre solution OLAP. Les valeurs numériques sont généralement détectées dans les mesures et les variables catégorielles dans les dimensions. Pour plus d’informations, consultez [Créer une structure d’exploration de données OLAP](../../analysis-services/data-mining/create-an-olap-mining-structure.md).  
  
-   Vous pouvez également définir des structures d'exploration de données en utilisant DMX. Pour plus d’informations, consultez [Instructions de définition de données DMX &#40;Data Mining Extensions&#41;](../../dmx/dmx-statements-data-definition.md).  
  
 Après avoir créé la structure d'exploration de données initiale, vous pouvez copier, modifier et affecter un alias aux colonnes de la structure.  
  
 Chaque structure d'exploration de données peut contenir plusieurs modèles d'exploration de données. Par conséquent, une fois que vous avez terminé, vous pouvez rouvrir la structure d'exploration de données, puis utilisez [Data Mining Designer](../../analysis-services/data-mining/data-mining-designer.md) pour ajouter des modèles d'exploration de données à la structure.  
  
 Vous avez également la possibilité de séparer vos données en jeu de données d'apprentissage, utilisé pour générer des modèles, et en jeu de données d'exclusion à des fins de test ou de validation de vos modèles d'exploration de données.  
  
> [!WARNING]  
>  Certains types de modèle, tels que les modèles de série chronologique, ne prennent pas en charge la création de jeux de données d'exclusion car ils nécessitent une série continue de données pour l'apprentissage. Pour plus d'informations, voir [Training and Testing Data Sets](../../analysis-services/data-mining/training-and-testing-data-sets.md).  
  
  
###  <a name="bkmk_Models"></a> Mining Models  
 Le modèle d’exploration de données définit l’algorithme ou la méthode d’analyse que vous utiliserez sur les données. Vous pouvez ajouter un ou plusieurs modèles d'exploration de données dans chaque structure.  
  
 Selon vos besoins, vous pouvez combiner plusieurs modèles dans un projet unique ou créer des projets distincts pour chaque type de modèle ou de tâche analytique.  
  
 Après avoir créé une structure et un modèle, vous *traitez* chaque modèle en exécutant les données de la vue de source de données au moyen de l'algorithme, qui génère un modèle mathématique des données. Ce processus porte le nom d’ *apprentissage du modèle*. Pour plus d’informations, consultez [Exigences et considérations concernant le traitement &#40;exploration de données&#41;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md).  
  
 Une fois le modèle traité, vous pouvez explorer visuellement le modèle d’exploration de données et créer des requêtes de prédiction s'y rapportant. Si les données du processus d'apprentissage ont été mises en cache, vous pouvez utiliser des requêtes d’ *extraction* qui renvoient des informations détaillées sur les cas utilisés dans le modèle.  
  
 Lorsque vous souhaitez utiliser un modèle pour la production (par exemple, pour effectuer des prédictions ou pour l'exploration par des utilisateurs généraux) vous pouvez déployer le modèle sur un autre serveur. Si vous devez retraiter le modèle à l'avenir, vous devez également exporter la définition de la structure d'exploration de données sous-jacente (et, nécessairement, la définition de la source de données et de la vue de source de données) en même temps.  
  
 Lorsque vous déployez un modèle, vous devez également garantir que les options de traitement appropriées sont définies sur la structure et le modèle, et que les utilisateurs potentiels disposent des autorisations nécessaires pour exécuter des requêtes, afficher des modèles, ou extraire dans les données de la structure ou du modèle. Pour plus d’informations, consultez [Vue d’ensemble de la sécurité &#40;exploration de données&#41;](../../analysis-services/data-mining/security-overview-data-mining.md).  
  
  
##  <a name="bkmk_Complete"></a> Utilisation du projet d'exploration de données terminé  
 Cette section résume les façons dont vous pouvez utiliser le projet terminé d'exploration de données. Vous pouvez créer des graphiques d'analyse de précision, explorer et valider les données et rendre les modèles d'exploration de données disponibles aux utilisateurs.  
  
> [!WARNING]  
>  Les graphiques, les requêtes et les visualisations que vous utilisez avec des modèles d'exploration de données ne sont pas enregistrés dans le cadre du projet d'exploration de données et ne peuvent pas être déployés. Si vous devez rendre ces objets persistants, vous devez enregistrer le contenu qui est présenté ou créer un script à partir de ce contenu comme indiqué pour chaque objet.  
  
  
###  <a name="bkmk_ViewExplore"></a> View and Explore Models  
 Après avoir créé un modèle, vous pouvez utiliser des outils visuels et des requêtes pour explorer les schémas du modèle et pour en savoir plus sur les schémas et les statistiques sous-jacents. Dans l’onglet **Visionneuse de modèle d'exploration de données** du Concepteur d'exploration de données, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit des visionneuses pour chaque type de modèle d’exploration de données que vous pouvez utiliser pour explorer les modèles.  
  
 Ces visualisations sont temporaires et sont fermées sans enregistrement lorsque vous fermez la session avec [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Par conséquent, si vous devez exporter les visualisations vers une autre application pour les présenter ou les analyser plus en détails, utilisez la commande **Copier** à disposition dans chaque onglet ou volet de l'interface de la visionneuse.  
  
 Les Compléments d'exploration de données pour Excel fournissent également un modèle Visio que vous pouvez utiliser pour représenter vos modèles dans un diagramme Visio et pour annoter et modifier le diagramme à l'aide des outils Visio. Pour plus d’informations, consultez [Compléments d’exploration de données Microsoft SQL Server 2008 SP2 pour Microsoft Office 2007](http://go.microsoft.com/fwlink/?LinkID=123146).  
  
  
###  <a name="bkmk_Validate"></a> Test and Validate Models  
 Une fois un modèle créé, vous pouvez étudier les résultats et déterminer quels sont les modèles les plus efficaces.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit plusieurs graphiques que vous pouvez utiliser pour fournir des outils que vous pouvez utiliser pour comparer directement les modèles d’exploration de données et choisir le modèle le plus précis ou le plus utile. Ces outils incluent un graphique de courbes d'élévation, un graphique des bénéfices et une matrice de classification. Vous pouvez générer ces graphiques en utilisant l’onglet **Graphique d'analyse de précision de l'exploration de données** du Concepteur d'exploration de données.  
  
 Vous pouvez également utiliser le rapport de validation croisée pour effectuer un sous-échantillonnage itératif de vos données afin de déterminer si le modèle est influencé par un jeu de données particulier. Les statistiques de ce rapport peuvent être utilisées pour comparer des modèles objectivement et évaluer la qualité de vos données d'apprentissage.  
  
 Notez que ces rapports et graphiques ne sont pas stockés avec le projet ou dans la base de données ssASnoversion, par conséquent si vous devez conserver ou dupliquer les résultats, vous devez les enregistrer ou créer un script à partir des objets en utilisant DMX ou AMO. Vous pouvez également utiliser des procédures stockées pour la validation croisée.  
  
 Pour plus d’informations, consultez [Test et validation &#40;exploration de données&#41;](../../analysis-services/data-mining/testing-and-validation-data-mining.md).  
  
  
###  <a name="bkmk_Predict"></a> Create Predictions  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit un langage de requête appelé DMX (data mining extensions) qui est à la base de la création des prédictions et qui est facilement scriptable. Pour vous permettre de générer des requêtes de prédiction DMX, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit un générateur de requêtes, disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Il existe également de nombreux modèles DMX pour l'éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si vous n'êtes pas familiarisé avec les requêtes de prédiction, nous vous recommandons d'utiliser le générateur de requêtes fourni dans le Concepteur d'exploration de données et dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations, voir [Data Mining Tools](../../analysis-services/data-mining/data-mining-tools.md).  
  
 Les prédictions que vous créez dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ne sont pas conservées, par conséquent si vos requêtes sont complexes ou si vous devez reproduire les résultats, nous vous recommandons d'enregistrer vos requêtes de prédiction dans des fichiers de requête DMX, les inclure dans des scripts ou les incorporer dans le cadre d'un package Integration Services.  
  
  
##  <a name="bkmk_API"></a> Accès par programmation aux objets d'exploration de données  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]fournit plusieurs outils qui vous pouvez d’effectuer par programme des projets d’exploration de données et les objets qu’elles contiennent. Le langage DMX fournit des déclarations que vous pouvez utiliser pour créer des sources de données et des vues de source de données, et pour créer, effectuer l'apprentissage et utiliser des structures et des modèles d'exploration de données. Pour plus d’informations, consultez [Guide de référence du langage DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-reference.md).  
  
 Vous pouvez également effectuer ces tâches en utilisant ASSL (Analysis Services Scripting Language), ou en utilisant des objets AMO (Analysis Management Objects). Pour plus d’informations, consultez [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md).  
  
  
## <a name="related-tasks"></a>Tâches associées  
 Les rubriques suivantes décrivent l'utilisation de l'Assistant Exploration de données pour créer un projet d'exploration de données et ses objets associés.  
  
|Tâches|Rubriques|  
|-----------|------------|  
|Décrit la façon d'utiliser des colonnes de structure d'exploration de données|[Créer une Structure d’exploration de données relationnelles](../../analysis-services/data-mining/create-a-relational-mining-structure.md)|  
|Fournit des informations sur l'ajout de nouveaux modèles d'exploration de données et le traitement d'une structure et de modèles|[Ajouter des modèles d’exploration de données à une Structure & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/add-mining-models-to-a-structure-analysis-services-data-mining.md)|  
|Fournit des liens vers des ressources qui vous permettent de personnaliser les algorithmes qui génèrent des modèles d'exploration de données|[Personnaliser la Structure et les modèles d’exploration de données](../../analysis-services/data-mining/customize-mining-models-and-structure.md)|  
|Fournit des liens vers des informations sur chacune des visionneuses de modèles d'exploration de données|[Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)|  
|En savoir plus sur la création d'un graphique de courbes d'élévation, d'un graphique des bénéfices ou d'une matrice de classification, ou sur le test d'une structure d'exploration de données|[Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)|  
|En savoir plus sur les options de traitement et les autorisations|[Traitement des objets d’exploration de données](../../analysis-services/data-mining/processing-data-mining-objects.md)|  
|Fournit des informations supplémentaires sur Analysis Services|[Bases de données de modèle multidimensionnel ](../../analysis-services/multidimensional-models/multidimensional-model-databases-ssas.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Concepteur d’exploration de données](../../analysis-services/data-mining/data-mining-designer.md)   
 [Création de modèles multidimensionnels à l’aide des outils de données SQL Server & #40 ; SSDT & #41 ;](../../analysis-services/multidimensional-models/creating-multidimensional-models-using-sql-server-data-tools-ssdt.md)   
 [Base de données d’espace de travail](../../analysis-services/tabular-models/workspace-database-ssas-tabular.md)  
  
  
