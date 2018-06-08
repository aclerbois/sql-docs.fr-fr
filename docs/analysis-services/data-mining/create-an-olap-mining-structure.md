---
title: Créer une Structure d’exploration de données OLAP | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2a4721bf42a3c223905d982e8be6a470983beb06
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-an-olap-mining-structure"></a>Créer une structure d'exploration de données OLAP
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Il existe de nombreux avantages à la création d'un modèle d'exploration de données basé sur un cube OLAP ou une autre banque de données multidimensionnelles. Une solution OLAP contient déjà de grandes quantités de données qui sont correctement organisées, nettoyées et mises en forme ; toutefois, la complexité des données est telle que les utilisateurs ont peu de chances de trouver des modèles explicites par une exploration ad hoc. L'exploration de données offre la possibilité de découvrir de nouvelles corrélations et de fournir un éclairage utilisable.  
  
 Cette rubrique décrit comment créer une structure d'exploration de données OLAP, en fonction d'une dimension et de mesures associées dans une solution multidimensionnelle existante.  
  
 [Spécifications](#bkmk_Reqs)  
  
 [Vue d'ensemble du processus d'exploration de données OLAP](#bkmk_Overview)  
  
 [Scénarios d'utilisation de l'exploration de données dans des solutions OLAP](#bkmk_OLAP_Scenarios)  
  
 [Filtres](#bkmk_Filters)  
  
 [Utilisation de tables imbriquées](#bkmk_Nested)  
  
 [Dimensions d'exploration de données](#bkmk_DMDimension)  
  
##  <a name="bkmk_Reqs"></a> Conditions requises pour la structure et les modèles d'exploration de données OLAP  
 Si vous concevez un modèle d'exploration de données OLAP, votre source de données existe déjà, dans la base de données utilisée pour générer le cube. Vous ne pouvez pas vous connecter à un cube distant et générer des objets d'exploration de données ; les objets de cube doivent être disponibles dans la même solution de la base de données que la structure d'exploration de données que vous allez générer.  
  
 Si vous n’avez pas les fichiers de projet d’origine, ou ne souhaitez pas les modifier, vous pouvez utiliser l’option dans Visual Studio, **Importer à partir du serveur (multidimensionnel ou exploration de données)**, pour obtenir une copie des métadonnées et objets de la solution. Vous pouvez ensuite modifier la cible de déploiement, modifier les sources de données et utiliser des objets de cube sans affecter les objets existants.  
  
 Pour plus d’informations, consultez [Importer un projet d’exploration de données à l’aide de l’Assistant Importation d’Analysis Services](../../analysis-services/data-mining/import-a-data-mining-project-using-the-analysis-services-import-wizard.md).  
  
##  <a name="bkmk_Overview"></a> Vue d'ensemble du processus d'exploration de données OLAP  
 Démarrez l’Assistant Exploration de données en cliquant avec le bouton droit sur le nœud **Structures d’exploration de données** dans l’Explorateur de solutions, puis en sélectionnant  **Nouvelle structure d’exploration de données**. L'Assistant vous guide à travers les étapes suivantes pour créer la structure d'un nouveau modèle :  
  
1.  **Sélectionner la méthode de définition**: vous sélectionnez ici un type de source de données, puis choisissez **À partir d'un cube existant**.  
  
    > [!NOTE]  
    >  Le cube OLAP que vous utilisez comme source doit exister dans la même base de données que la structure d'exploration de données, comme décrit ci-dessus. Par ailleurs, vous ne pouvez pas utiliser un cube créé par le [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel comme source pour l’exploration de données.  
  
2.  **Créer la structure d'exploration de données**: déterminez si vous allez générer simplement une structure ou une structure avec un modèle d'exploration de données.  
  
     Vous devez également choisir un algorithme approprié pour analyser vos données. Pour obtenir de l’aide sur l’algorithme le mieux approprié à certaines tâches, consultez HYPERLINK "ms-help://SQL111033/as_1devconc/html/ed1fc83b-b98c-437e-bf53-4ff001b92d64.htm" Algorithmes d’exploration de données (Analysis Services – exploration de données).  
  
3.  **Sélectionner la dimension de cube source**: cette étape est le même que la sélection d'une source de données. Vous devez choisir l'unique dimension qui contient les données les plus importantes utilisées pour l'apprentissage de votre modèle. Vous pouvez ajouter des données d'autres dimensions ultérieurement, ou filtrer la dimension.  
  
4.  **Sélectionner la clé de cas**: dans la dimension que vous venez de sélectionner, choisissez un attribut (colonne) pour servir d’identificateur unique de vos données de cas.  
  
     En général, une colonne sera présélectionnée pour vous, mais vous pouvez en changer si en fait il existe plusieurs clés.  
  
5.  **Sélectionner les colonnes de niveau de cas**: vous choisissez ici les attributs de la dimension sélectionnée, et les mesures associées, qui sont appropriés à votre analyse. Cette étape est équivalente à la sélection de colonnes d'une table.  
  
     L'Assistant inclut automatiquement toutes les mesures créées à l'aide des attributs de la dimension sélectionnée pour que vous puissiez les consulter et les sélectionner.  
  
     Par exemple, si votre cube contient une mesure qui calcule le coût de transport selon la situation géographique du client et que vous avez choisi la dimension Customer comme source de données principale pour la modélisation, la mesure sera proposée en tant que candidat à ajouter au modèle. Prenez garde d'ajouter trop de mesures qui sont déjà directement basées sur des attributs, car il existe déjà une relation implicite entre les colonnes, comme défini dans la formule de mesure, et la force de cette corrélation (attendue) peut masquer d'autres relations que vous pouvez sinon découvrir.  
  
6.  **Utilisation des colonnes du modèle d'exploration**: pour chaque attribut ou mesure que vous avez ajoutés à la structure, vous devez spécifier si l'attribut doit être utilisé pour la prédiction ou comme entrée. Si vous ne sélectionnez aucune de ces options, les données sont traitées mais ne sont pas utilisées pour l'analyse ; toutefois, elles sont disponibles comme données d'arrière-plan au cas où vous activeriez ultérieurement l'extraction.  
  
7.  **Ajouter des tables imbriquées**: cliquez pour ajouter des tables associées. Dans la boîte de dialogue **Sélectionner une dimension de groupe de mesures** , vous pouvez choisir une dimension unique parmi les dimensions liées à la dimension actuelle.  
  
     Ensuite, vous utilisez la boîte de dialogue **Sélectionner une colonne clé de table imbriquée** pour définir la façon dont la nouvelle dimension est associée à la dimension qui contient les données de cas.  
  
     Utilisez la boîte de dialogue **Sélectionner les colonnes de la table imbriquée** pour choisir les attributs et mesures de la nouvelle dimension que vous voulez utiliser dans l'analyse. Vous devez également spécifier si l'attribut imbriqué sera utilisé pour la prédiction.  
  
     Après avoir ajouté tous les attributs imbriqués dont vous pouvez avoir besoin, revenez à la page **Utilisation des colonnes du modèle d'exploration**et cliquez sur **Suivant**.  
  
8.  **Spécifier le type de contenu et de données des colonnes**: à ce stade, vous avez ajouté toutes les données à utiliser pour l'analyse et devez spécifier le *type de données* et le *type de contenu* pour chaque attribut.  
  
     Dans un modèle OLAP, vous n'avez pas l'option de détecter automatiquement les types de données, car le type de données est déjà défini par la solution multidimensionnelle et ne peut pas être modifié. Les clés sont également automatiquement identifiées. Pour plus d’informations, consultez [Types de données &#40;exploration de données&#41;](../../analysis-services/data-mining/data-types-data-mining.md).  
  
     Le *type de contenu* que vous choisissez pour chaque colonne que vous utilisez dans le modèle indique à l'algorithme le mode de traitement des données. Pour plus d’informations, consultez [Types de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
9. **Découper le cube source**: vous pouvez ici définir des filtres dans un cube pour sélectionner uniquement un sous-ensemble de données et effectuer l'apprentissage des modèles qui sont plus ciblés.  
  
     Vous filtrez un cube en choisissant la dimension sur laquelle filtrer, en sélectionnant le niveau de la hiérarchie qui contient les critères que vous souhaitez utiliser, puis en tapant une condition à utiliser comme filtre.  
  
10. **Créer un jeu de test**: dans cette page, vous pouvez indiquer à l'Assistant combien de données doivent être mises de côté pour être utilisées lors du test du modèle. Si vos données prennent en charge plusieurs modèles, il est judicieux de créer un jeu de données d'exclusion, afin que tous les modèles puissent être testés sur les mêmes données.  
  
     Pour plus d’informations, consultez [Test et validation &#40;exploration de données&#41;](../../analysis-services/data-mining/testing-and-validation-data-mining.md).  
  
11. **Fin de l'Assistant**: dans cette page, vous donnez un nom à la nouvelle structure d'exploration de données et au modèle d'exploration de données associé, puis vous enregistrez la structure et le modèle.  
  
     Dans cette page, vous pouvez également définir les options suivantes :  
  
    -   **Accepter l'extraction**  
  
    -   **Créer une dimension du modèle d'exploration de données**  
  
    -   **Créer le cube en utilisant une dimension du modèle d'exploration de données**  
  
     Pour en savoir plus sur ces options, consultez la section plus loin dans cette rubrique, [Fonctionnement de l'extraction et des dimensions d'exploration de données](#bkmk_DMDimension).  
  
 À ce stade, la structure d'exploration de données et son modèle sont simplement des métadonnées ; vous devez les traiter tous deux pour obtenir des résultats.  
  
##  <a name="bkmk_OLAP_Scenarios"></a> Scénarios d'utilisation de l'exploration de données avec des données OLAP  
 Les cubes OLAP contiennent souvent tant de membres et de dimensions qu'il peut être difficile de déterminer l'emplacement où doit commencer l'exploration de données. Pour aider à identifier les modèles que les cubes contiennent, vous identifiez généralement une seule dimension d'intérêt, puis commencez à explorer les modèles en rapport avec cette dimension. Le tableau suivant répertorie plusieurs tâches d'exploration de données OLAP courantes, décrit des exemples de scénarios d'application de chaque tâche et identifie l'algorithme d'exploration de données à utiliser pour chaque tâche.  
  
|Tâche|Exemple de scénario|Algorithm|  
|----------|---------------------|---------------|  
|Regrouper des membres en clusters|Segmentez une dimension de clients en fonction des propriétés de membre des clients, des produits achetés par les clients et du montant dépensé par les clients.|Algorithme MC ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Clustering)|  
|Rechercher des membres intéressants ou anormaux|Identifiez des magasins intéressants ou anormaux dans une dimension de magasins en fonction des ventes, des bénéfices, de la situation géographique et de la taille des magasins.|Algorithme MDT ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees)|  
|Rechercher des cellules intéressantes ou anormales|Identifiez les ventes des magasins qui ne suivent pas les tendances générales dans le temps.|[!INCLUDE[msCoName](../../includes/msconame-md.md)]Algorithme de série chronologique|  
|Rechercher des corrélations|Identifiez les facteurs qui sont liés au temps mort de serveur, notamment la zone, le type de l'ordinateur, le système d'exploitation ou la date d'achat.|Algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naïve Bayes|  
  
##  <a name="bkmk_Filters"></a>Découpage d’un Cube et. filtrage de modèles  
 Le découpage du cube lorsque vous générez un modèle revient à créer un filtre sur un modèle d'exploration de données relationnel. Dans un modèle relationnel, le filtre sur la source de données est défini comme une clause WHERE sur une instruction SQL ; dans un cube, vous utilisez l’éditeur pour créer des instructions de filtre à l’aide de MDX.  
  
 Par exemple, un cube peut contenir des informations sur des achats de produits dans le monde entier, mais pour votre campagne de marketing, vous souhaitez créer un modèle basé sur l'analyse des clients femmes de plus de 30 ans résidant au Royaume-Uni.  
  
 Dans ce scénario, vous devez créer deux filtres :  
  
-   Pour le premier filtre, vous choisissez la dimension Geography, vous choisissez la hiérarchie pour Region, puis vous utilisez la liste **Expression de filtre** pour choisir « Royaume-Uni » parmi les valeurs possibles.  
  
-   Pour le deuxième filtre, vous choisissez la dimension Customer, vous sélectionnez l’attribut Gender et vous sélectionnez « Femme » dans la liste des valeurs d’attribut.  
  
 Une fois la structure d'exploration de données créée, vous pouvez modifier la définition des données du cube et les critères de filtre. Pour plus d’informations, consultez [filtres pour les modèles d’exploration de données](~/analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md).  
  
 Les onglets **Structure d'exploration de données** et **Modèle d'exploration de données** fournissent tous deux une option pour ajouter un filtre à une structure d'exploration de données existante, en cliquant sur **Définir une coupe de cube**. La boîte de dialogue **Découper un cube** vous aide à générer une expression de filtre MDX valide en choisissant une valeur dans les listes déroulantes.  
  
> [!WARNING]  
>  Notez que l'interface pour concevoir et parcourir des cubes a été modifiée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Parcourir les données et métadonnées de cube](../../analysis-services/multidimensional-models/browse-data-and-metadata-in-cube.md).  
  
 Vous pouvez ajouter autant de filtres que nécessaire sur le cube pour renvoyer les données dont vous avez besoin pour le modèle d'exploration de données. Vous pouvez aussi définir des coupes sur des coupes de cube individuelles. Par exemple, si votre structure contient deux tables imbriquées qui sont basées sur des produits, vous pouvez découper une table sur mars 2004 et l'autre table sur avril 2004 Le modèle résultant peut alors servir à prédire les achats effectués en avril en fonction des achats qui ont été effectués en mars.  
  
##  <a name="bkmk_Nested"></a> Utilisation de tables imbriquées dans un modèle d'exploration de données OLAP  
 Lorsque vous utilisez l'Assistant Exploration de données pour créer un modèle basé sur des données de cube, vous pouvez ajouter des tables imbriquées en spécifiant les noms des dimensions associées, puis en choisissant les attributs ou les mesures à ajouter au modèle  
  
 Par exemple, si la dimension principale utilisée pour les données de cas est Customer, vous pouvez ajouter la dimension Products comme dimension associée, car vous pensez qu’un client peut avoir commandé plusieurs produits au fil du temps, et le cube lie déjà chaque client aux nombreux produits par le biais des tables de faits de commandes.  
  
 Vous ajoutez des tables imbriquées dans la page **Utilisation des colonnes du modèle d'exploration** de l'Assistant, en cliquant sur **Ajouter des tables imbriquées**. Une boîte de dialogue s'ouvre et vous guide tout au long du processus du choix d'une dimension associée, ainsi que des mesures. Les dimensions imbriquées et de cas doivent être liées par une clé étrangère, et les mesures doivent utiliser l'un des attributs qui sont déjà inclus dans les tables imbriquées ou de cas. Malheureusement, ces restrictions n'étant pas réellement efficaces pour limiter la portée, vous devez veiller à sélectionner uniquement les attributs qui sont utiles pour la modélisation.  
  
 Pour chaque attribut ou mesure que vous ajoutez à la table imbriquée, vous devez spécifier si l'attribut imbriqué est utilisé ou non pour la prédiction, en sélectionnant **Prédictible** ou **Entrée** dans la boîte de dialogue **Sélectionner les colonnes de la table imbriquée** . Si vous ne sélectionnez aucune de ces options, les données sont ajoutées à la structure d'exploration de données mais ne sont pas utilisées pour l'analyse.  
  
 Pour chaque attribut et chaque mesure, vous devez également spécifier si l'attribut est discret, discrétisé ou continu. L'Assistant présélectionne une valeur par défaut selon le type de données de l'attribut, mais vous devrez peut-être la modifier, selon les spécifications d'algorithme. Si vous choisissez un type de contenu qui n'est pas compatible avec l'algorithme choisi (par exemple, vous utilisez un type numérique continu avec un modèle Naïve Bayes), vous ne recevez pas un message d'erreur tant que vous n'essayez pas de traiter le modèle.  
  
 Lorsque vous avez fini de définir ces options, l'Assistant ajoute la table imbriquée à la table de cas. Le nom par défaut de la table imbriquée correspond au nom de la dimension imbriquée, mais vous pouvez renommer la table imbriquée et ses colonnes. Vous pouvez répéter ce processus pour ajouter plusieurs tables imbriquées à la structure d'exploration de données.  
  
 La possibilité d'utiliser des données de table imbriquée de cette façon est une fonctionnalité de l'exploration de données SQL Server qui est particulièrement puissante et, dans un cube, il existe des possibilités presque sans limites d'utilisation des sous-ensembles connexes de données.  
  
##  <a name="bkmk_DMDimension"></a> Fonctionnement de l'extraction et des dimensions d'exploration de données  
 L'option **Accepter l'extraction**vous permet d'exécuter des requêtes sur les données du cube sous-jacentes pendant que vous parcourez le modèle. Les données ne sont pas contenues dans la nouvelle dimension d'exploration de données, mais la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut utiliser les liaisons de données pour récupérer les informations du cube source.  
  
 L'option **Créer une dimension du modèle d'exploration de données**vous permet de générer une nouvelle dimension dans le cube existant qui contient les modèles découverts par l'algorithme. La hiérarchie de la nouvelle dimension est déterminée principalement par le type de modèle. Par exemple, la représentation d'un modèle de clustering est assez plate, avec le nœud (Tout) en haut de la hiérarchie et chaque cluster au niveau suivant. En revanche, la dimension créée pour un modèle d'arbre de décision peut avoir une hiérarchie très profonde, représentant la création des branches de l'arbre.  
  
 L'option **Créer le cube au moyen d'une dim. du mod. d'explor. de données**vous permet d'exporter la nouvelle dimension d'exploration de données dans un nouveau cube. Tous les objets requis pour l'extraction sur la dimension d'exploration de données seront inclus automatiquement.  
  
> [!WARNING]  
>  Seuls les types de modèles suivants prennent en charge la création de dimensions d'exploration de données : modèles basés sur l'algorithme de gestion de clusters Microsoft, l'algorithme MDT (Microsoft Decision Trees) ou l'algorithme Microsoft Association.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Colonnes de Structure d’exploration de données](../../analysis-services/data-mining/mining-structure-columns.md)   
 [Colonnes du modèle d’exploration de données](../../analysis-services/data-mining/mining-model-columns.md)   
 [Propriétés du modèle d’exploration de données](../../analysis-services/data-mining/mining-model-properties.md)   
 [Propriétés de Structure d’exploration de données et les colonnes de Structure](../../analysis-services/data-mining/properties-for-mining-structure-and-structure-columns.md)  
  
  
