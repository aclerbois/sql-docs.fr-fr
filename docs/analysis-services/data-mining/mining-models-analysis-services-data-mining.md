---
title: Les modèles d’exploration de données (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8febdd6f68cae6f28182a5734e46717a3b597624
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mining-models-analysis-services---data-mining"></a>Modèles d'exploration de données (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un *modèle d’exploration de données* est créé en appliquant un algorithme aux données, mais c’est plus qu’un algorithme ou qu’un conteneur de métadonnées : il s’agit d’un jeu de données, de statistiques et de modèles qui peuvent être appliqués à de nouvelles données pour générer des prédictions et pour effectuer des inférences sur les relations.  
  
 Cette section explique ce qu'est un modèle d'exploration de données et son utilisation : l'architecture de base des modèles et des structures, les propriétés des modèles d'exploration de données, ainsi que les méthodes pour créer et utiliser des modèles d'exploration de données.  
  
 [Architecture du modèle d'exploration de données](#bkmk_mdlArch)  
  
 [Définition des modèles d'exploration de données](#bkmk_mdlDefine)  
  
 [Propriétés du modèle d'exploration de données](#bkmk_mdlProps)  
  
 [Colonnes d'un modèle d'exploration de données](#bkmk_mdlCols)  
  
 [Traitement des modèles d'exploration de données](#bkmk_mdlProcess)  
  
 [Affichage et interrogation de modèles d'exploration de données](#bkmk_mdlView)  
  
##  <a name="bkmk_mdlArch"></a> Architecture du modèle d'exploration de données  
 Un modèle d'exploration de données récupère les données d'une structure d'exploration de données, puis les analyse à l'aide d'un algorithme d'exploration de données. Le modèle et la structure d'exploration de données sont des objets distincts. La structure d'exploration de données stocke les informations qui définissent la source de données. Un modèle d'exploration de données stocke les informations découlant du traitement statistique des données, telles que les motifs trouvés suite à l'analyse.  
  
 Un modèle d'exploration de données est vide jusqu'à ce que les données fournies par la structure d'exploration de données aient été traitées et analysées. Après traitement, un modèle d'exploration de données contiendra des métadonnées, des résultats et des liaisons se rapportant à la structure d'exploration de données.  
  
 ![modèle contient des métadonnées, les modèles et les liaisons](../../analysis-services/data-mining/media/dmcon-modelarch2.gif "modèle contient des métadonnées, les modèles et les liaisons")  
  
 Les métadonnées indiquent le nom du modèle et le serveur où il est stocké, ainsi qu'une définition du modèle, y compris les colonnes de la structure d'exploration de données utilisées pour créer le modèle, les définitions des filtres éventuels appliqués lors du traitement du modèle et l'algorithme utilisé pour analyser les données. Tous ces choix, les colonnes de données et leurs types de données, les filtres et l'algorithme, ont une influence importante sur les résultats de l'analyse.  
  
 Par exemple, vous pouvez utiliser les mêmes données pour créer plusieurs modèles, en utilisant éventuellement un algorithme de clustering, un algorithme d'arbre de décision et un algorithme Naïve Bayes. Chaque type de modèle crée un ensemble différent de modèles, de jeux d'éléments, de règles ou de formules, que vous pouvez utiliser pour faire des prédictions. Généralement, chaque algorithme analyse les données d’une manière différente afin que le *contenu* du modèle résultant soit également organisé en différentes structures. Dans un type de modèle, les données et les modèles peuvent être regroupés en *clusters*; dans un autre type de modèle, les données peuvent être organisées en arborescences et en branches, ainsi que selon les règles qui les divisent et les définissent.  
  
 Le modèle est également affecté par les données sur lesquelles vous effectuez l'apprentissage : même les modèles qualifiés sur la même structure d'exploration de données peuvent donner des résultats différents si vous filtrez les données différemment ou utilisez différentes valeurs de départ durant l'analyse. Toutefois, les données réelles ne sont pas stockées dans le modèle ; seules les statistiques sommaires sont stockées, les données réelles résidant dans la structure d'exploration de données. Si vous avez créé des filtres sur les données lors de l'apprentissage du modèle, les définitions de filtre sont également enregistrées avec l'objet de modèle.  
  
 Le modèle contient un jeu de liaisons, qui renvoient aux données mises en cache dans la structure d'exploration de données. Si les données ont été mises en cache dans la structure et n'ont pas été effacées après leur traitement, ces liaisons permettent d'extraire des résultats les cas les prenant en charge. Toutefois, les données réelles sont stockées dans le cache de la structure, et non dans le modèle.  
  
 [Architecture du modèle d'exploration de données](#bkmk_mdlArch)  
  
##  <a name="bkmk_mdlDefine"></a> Définition des modèles d'exploration de données  
 Pour créer un modèle d'exploration de données, suivez ces étapes générales :  
  
-   Créez la structure d'exploration de données sous-jacente et incluez des colonnes de données qui peuvent être nécessaires.  
  
-   Sélectionnez l'algorithme le mieux adapté à la tâche analytique.  
  
-   Choisissez les colonnes de la structure à utiliser dans le modèle, puis spécifiez comment elles doivent être utiliser : quelle colonne contient les résultats que vous souhaitez prédire, quelles colonnes servent d'entrée uniquement, etc.  
  
-   Facultativement, définition des paramètres pour régler avec précision le traitement par l'algorithme.  
  
-   Remplissez le modèle avec des données en *traitant* la structure et le modèle.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit les outils suivants pour gérer les modèles d’exploration de données :  
  
-   L'Assistant Exploration de données permet de créer une structure et un modèle d'exploration de données connexe. Il s'agit de la méthode la plus simple à utiliser. L'Assistant crée automatiquement la structure d'exploration de données requise et permet de configurer les paramètres importants.  
  
-   Une instruction DMX CREATE MODEL peut être utilisée pour définir un modèle. La structure requise est créée automatiquement dans le cadre du processus ; par conséquent, vous ne pouvez pas réutiliser une structure existante avec cette méthode. Utilisez cette méthode si vous savez déjà exactement quel modèle vous souhaitez créer ou si vous souhaitez écrire des scripts pour des modèles.  
  
-   Une instruction A DMX ALTER STRUCTURE ADD MODEL peut être utilisée pour ajouter un nouveau modèle d'exploration de données à une structure existante. Utilisez cette méthode si vous souhaitez essayer différents modèles basés sur le même jeu de données.  
  
 Vous pouvez également créer par programmation des modèles d'exploration de données, avec AMO ou XML/A, ou en utilisant d'autres clients, tels que le Client d'exploration de données pour Excel. Pour plus d'informations, consultez les rubriques suivantes :  
  
 [Architecture du modèle d'exploration de données](#bkmk_mdlArch)  
  
##  <a name="bkmk_mdlProps"></a> Propriétés du modèle d'exploration de données  
 Chaque modèle d'exploration de données a des propriétés qui définissent le modèle et ses métadonnées. Celles-ci incluent le nom, la description, la date du dernier traitement du modèle, les autorisations liées au modèle, ainsi que tous les filtres appliqués aux données utilisées pour l'apprentissage.  
  
 Chaque modèle d'exploration de données possède également des propriétés découlant de la structure d'exploration de données qui décrivent les colonnes de données utilisées par le modèle. Si une colonne utilisée par le modèle est une table imbriquée, il est également possible qu'un filtre séparé lui soit appliqué.  
  
 De plus, chaque modèle d’exploration de données contient deux propriétés spéciales : <xref:Microsoft.AnalysisServices.MiningModel.Algorithm%2A> et <xref:Microsoft.AnalysisServices.MiningModelColumn.Usage%2A>.  
  
-   **Propriété Algorithme** : spécifie l’algorithme utilisé pour créer le modèle. Les algorithmes disponibles dépendent du fournisseur utilisé. Pour une liste des algorithmes inclus avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Algorithmes d’exploration de données &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md). La propriété **Algorithme** s’applique au modèle d’exploration de données et ne peut être définie qu’une seule fois pour chaque modèle. Vous pouvez modifier l'algorithme ultérieurement mais certaines colonnes du modèle d'exploration de données peuvent devenir non valides si elles ne sont pas prises en charge par l'algorithme choisi. Vous devez toujours retraiter le modèle suivant la modification de cette propriété.  
  
-   **Propriété Utilisation** : définit la façon dont chaque colonne est utilisée par le modèle. Vous pouvez définir l’utilisation de la colonne comme **Entrée**, **Prédire**, **Prédire uniquement**ou **Clé**. La propriété **Utilisation** s’applique à des colonnes individuelles du modèle d’exploration de données et doit être définie individuellement pour chaque colonne incluse dans un modèle. Si la structure contient une colonne que vous n’utilisez pas dans le modèle, l’utilisation est définie sur **Ignorer**. Les exemples de données que vous pouvez inclure dans la structure d'exploration de données mais pas utiliser dans l'analyse, peuvent être des noms de client ou des adresses de messagerie. De cette façon, vous pouvez les interroger ultérieurement sans avoir à les inclure dans la phase d'analyse.  
  
 Après avoir créé un modèle d'exploration de données, vous pouvez modifier la valeur de ses propriétés. Toutefois, toute modification, même sur le nom du modèle d'exploration de données, requiert un traitement supplémentaire du modèle. Lorsque le modèle a été traité de nouveau, vous pouvez voir des résultats différents.  
  
 [Architecture du modèle d'exploration de données](#bkmk_mdlArch)  
  
##  <a name="bkmk_mdlCols"></a> Colonnes d'un modèle d'exploration de données  
 Le modèle d'exploration de données contient des colonnes de données qui sont obtenues à partir des colonnes définies dans la structure d'exploration de données. Vous pouvez choisir les colonnes provenant de la structure d'exploration de données à utiliser dans le modèle et créer des copies des colonnes de la structure d'exploration de données, puis les renommer ou modifier leur utilisation. Dans le cadre du processus de création de modèles, vous devez également définir l'utilisation de la colonne par le modèle. Cela inclut des informations indiquant notamment si la colonne est une clé, si elle est utilisée pour la prédiction ou si elle peut être ignorée par l'algorithme.  
  
 Lorsque vous générez un modèle, plutôt que d'ajouter automatiquement chaque colonne de données disponibles, il est recommandé de vérifier les données de la structure avec soin et de n'inclure dans le modèle que les colonnes qui se justifient pour l'analyse. Par exemple, vous devez éviter d'inclure plusieurs colonnes qui répètent les mêmes données et éviter d'utiliser des colonnes contenant principalement des valeurs uniques. Si vous pensez qu'une colonne ne doit pas être utilisée, il n'est pas nécessaire de la supprimer de la structure ou du modèle d'exploration de données ; à la place, vous pouvez placer sur cette colonne un indicateur qui spécifie qu'elle doit être ignorée lors de la génération du modèle. Cela signifie que la colonne restera dans la structure d'exploration de données, mais ne sera pas utilisée dans le modèle d'exploration de données. Si vous avez activé l'extraction du modèle vers la structure d'exploration de données, vous pouvez récupérer les informations de la colonne ultérieurement.  
  
 Selon l'algorithme choisi, il est possible que certaines colonnes de la structure d'exploration de données soient incompatibles avec certains types de modèle ou qu'elles aboutissent à des résultats incorrects. Par exemple, si vos données contiennent des données numériques continues, telles qu'une colonne de revenus, et que votre modèle requiert des valeurs discrètes, vous pouvez convertir les données en plages discrètes ou les supprimer du modèle. Dans certains cas, l'algorithme convertit automatiquement ou lie les données à votre place, mais les résultats peuvent ne pas toujours être conformes à vos souhaits ou attentes. Pensez à effectuer des copies supplémentaires de la colonne et essayez différents modèles. Vous pouvez également définir des indicateurs sur les différentes colonnes pour indiquer où est requis un traitement spécial. Par exemple, si vos données contiennent des valeurs Null, vous pouvez utiliser un indicateur de modélisation pour contrôler la gestion. Si vous souhaitez qu'une colonne particulière soit considérée comme régresseur dans un modèle, vous pouvez le faire à l'aide d'un indicateur de modélisation.  
  
 Après avoir créé le modèle, vous pouvez apporter des modifications, telles que l'ajout ou la suppression de colonnes, et la modification du nom du modèle. Toutefois, toute modification, même appliquée uniquement aux métadonnées du modèle, requiert un retraitement du modèle.  
  
 [Architecture du modèle d'exploration de données](#bkmk_mdlArch)  
  
##  <a name="bkmk_mdlProcess"></a> Traitement des modèles d'exploration de données  
 Un modèle d'exploration de données est un objet vide tant qu'il n'est pas traité. Lorsque vous traitez un modèle, les données mises en cache par la structure passent dans un filtre (s'il a été défini dans le modèle), puis elles sont analysées par l'algorithme. L'algorithme calcule un ensemble de statistiques de résumé qui décrivent les données, identifie les règles et les schémas figurant dans les données, puis les utilise pour remplir le modèle.  
  
 Après avoir été traité, le modèle d'exploration de données contient quantité d'informations sur les données et les schémas détectés par l'analyse, y compris des statistiques, des règles et des formules de régression. Vous pouvez utiliser les visionneuses personnalisées pour parcourir ces informations, ou vous pouvez créer des requêtes d'exploration de données pour récupérer ces informations et les utiliser pour l'analyse et la présentation.  
  
 [Architecture du modèle d'exploration de données](#bkmk_mdlArch)  
  
##  <a name="bkmk_mdlView"></a> Affichage et interrogation de modèles d'exploration de données  
 Une fois vous avez traité un modèle, vous pouvez l’explorer en utilisant les visionneuses personnalisées fournies dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. For  
  
 Vous pouvez également créer des requêtes sur le modèle d'exploration de données pour élaborer des prédictions, ou récupérer les métadonnées du modèle ou les motifs créés par celui-ci. Vous pouvez créer des requêtes avec les extensions DMX (Data Mining Extensions).  
  
## <a name="related-content"></a>Contenu connexe  
  
|Rubriques|Liens|  
|------------|-----------|  
|Apprendre à créer des structures d'exploration de données qui peuvent prendre en charge plusieurs modèles d'exploration de données. En savoir plus sur l'utilisation de colonnes dans les modèles.|[Colonnes de structure d'exploration de données](../../analysis-services/data-mining/mining-structure-columns.md)<br /><br /> [Colonnes d'un modèle d'exploration de données](../../analysis-services/data-mining/mining-model-columns.md)<br /><br /> [Contenu des Types de & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/content-types-data-mining.md)|  
|Découvrir les différents algorithmes et la manière dont le choix de l'algorithme affecte le contenu du modèle.|[Contenu du modèle d’exploration &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md)<br /><br /> [Algorithmes d’exploration de données &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)|  
|Découvrir comment vous pouvez définir des propriétés sur le modèle qui affectent sa composition et son comportement.|[Propriétés du modèle d'exploration de données](../../analysis-services/data-mining/mining-model-properties.md)<br /><br /> [Modélisation des indicateurs & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/modeling-flags-data-mining.md)|  
|En savoir plus sur les interfaces programmables pour l'exploration de données.|[Développement avec Analysis Management Objects & #40 ; AMO & #41 ;](../../analysis-services/multidimensional-models/analysis-management-objects/developing-with-analysis-management-objects-amo.md)<br /><br /> [Informations de référence sur le langage DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-reference.md)|  
|Apprendre à utiliser les visionneuses personnalisées d'exploration de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|[Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)|  
|Afficher des exemples de différents types de requête que vous pouvez utiliser sur des modèles d'exploration de données.|[Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)|  
  
## <a name="related-tasks"></a>Tâches associées  
 Utilisez les liens suivants pour obtenir des informations spécifiques sur l'utilisation des modèles d'exploration de données  
  
|Tâche|Lien|  
|----------|----------|  
|Ajouter et supprimer des modèles d'exploration de données|[Ajouter un modèle d’exploration de données à une Structure d’exploration de données existante](../../analysis-services/data-mining/add-a-mining-model-to-an-existing-mining-structure.md)<br /><br /> [supprimer un modèle d'exploration de données d'une structure d'exploration de données](../../analysis-services/data-mining/delete-a-mining-model-from-a-mining-structure.md)|  
|Utiliser des colonnes de modèle d'exploration de données|[Exclure une colonne d'un modèle d'exploration de données](../../analysis-services/data-mining/exclude-a-column-from-a-mining-model.md)<br /><br /> [Créer un Alias pour une colonne de modèle](../../analysis-services/data-mining/create-an-alias-for-a-model-column.md)<br /><br /> [Modifier la discrétisation d’une colonne dans un modèle d’exploration de données](../../analysis-services/data-mining/change-the-discretization-of-a-column-in-a-mining-model.md)<br /><br /> [Spécifier une colonne à utiliser comme régresseur dans un modèle](../../analysis-services/data-mining/specify-a-column-to-use-as-regressor-in-a-model.md)|  
|Modifier les propriétés du modèle|[Modifier les propriétés d’un modèle d’exploration de données](../../analysis-services/data-mining/change-the-properties-of-a-mining-model.md)<br /><br /> [Appliquer un filtre à un modèle d’exploration de données](../../analysis-services/data-mining/apply-a-filter-to-a-mining-model.md)<br /><br /> [Supprimer un filtre d’un modèle d’exploration de données](../../analysis-services/data-mining/delete-a-filter-from-a-mining-model.md)<br /><br /> [Activer l’extraction pour un modèle d’exploration de données](../../analysis-services/data-mining/enable-drillthrough-for-a-mining-model.md)<br /><br /> [Afficher ou modifier les paramètres d’algorithme](../../analysis-services/data-mining/view-or-change-algorithm-parameters.md)|  
|Copier, déplacer ou gérer les modèles|[Effectuer une copie d’un modèle d’exploration de données](../../analysis-services/data-mining/make-a-copy-of-a-mining-model.md)<br /><br /> [Copie d’une vue d’un modèle d’exploration de données](../../analysis-services/data-mining/copy-a-view-of-a-mining-model.md)<br /><br /> [EXPORTATION & #40 ; DMX & #41 ;](../../dmx/export-dmx.md)<br /><br /> [IMPORT &#40;DMX&#41;](../../dmx/import-dmx.md)|  
|Remplir les modèles avec des données ou mettre à jour des données dans un modèle|[Traiter un modèle d’exploration de données](../../analysis-services/data-mining/process-a-mining-model.md)|  
|Utiliser des modèles OLAP|[Créer une Dimension d’exploration de données](../../analysis-services/data-mining/create-a-data-mining-dimension.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Objets de bases de données &#40;Analysis Services – Données multidimensionnelles&#41;](../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
