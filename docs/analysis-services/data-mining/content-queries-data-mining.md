---
title: Requêtes (exploration de données) de contenu | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4a308427ec839c316dbf0e3b215ea6d1506b1fa1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="content-queries-data-mining"></a>Requêtes de contenu (Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une requête de contenu est une façon d'extraire des informations sur les statistiques internes et la structure du modèle d'exploration de données. Parfois, une requête de contenu peut fournir des détails qui ne sont pas aisément disponibles dans la visionneuse. Vous pouvez également utiliser les résultats d'une requête de contenu pour extraire des informations par programme pour d'autres utilisations.  
  
 Cette section fournit des informations générales sur les types d'informations que vous pouvez récupérer à l'aide d'une requête de contenu, ainsi que la syntaxe DMX générale pour les requêtes de contenu.  
  
 [Requêtes de contenu de base](#bkmk_ContentQuery)  
  
-   [Requêtes sur les données de cas et de structure](#bkmk_Structure)  
  
-   [Requêtes sur les schémas de modèle](#bkmk_Patterns)  
  
 [Exemples](#bkmk_Examples)  
  
-   [Requête de contenu sur un modèle d'association](#bkmk_Assoc)  
  
-   [Requête de contenu sur un modèle d'arbre de décision](#bkmk_DecTree)  
  
 [Utilisation des résultats de requête](#bkmk_Results)  
  
##  <a name="bkmk_ContentQuery"></a> Requêtes de contenu de base  
 Vous pouvez créer des requêtes de contenu à l'aide du générateur de requêtes de prédiction, utiliser les modèles de requête de contenu DMX fournis dans SQL Server Management Studio ou écrire des requêtes directement dans DMX. Contrairement aux requêtes de prédiction, vous n'avez pas besoin de joindre des données externes ; il est donc facile d'écrire des requêtes de contenu.  
  
 Cette section fournit une vue d'ensemble des types de requêtes de contenu que vous pouvez créer.  
  
-   Les requêtes sur la structure d'exploration de données ou les données de cas vous permettent d'afficher les données détaillées utilisées pour l'apprentissage.  
  
-   Les requêtes sur le modèle peuvent retourner des schémas, des listes d'attributs, des formules, etc.  
  
###  <a name="bkmk_Structure"></a> Requêtes sur les données de cas et de structure  
 DMX prend en charge des requêtes sur les données mises en cache utilisées pour créer des structures et des modèles d'exploration de données. Par défaut, ce cache est créé lorsque vous définissez la structure d'exploration de données et est rempli lorsque vous traitez la structure ou le modèle.  
  
> [!WARNING]  
>  Ce cache ne peut pas être effacé ni supprimé si vous avez besoin de données distinctes dans les jeux d'apprentissage et de test. Si le cache est supprimé, vous ne pouvez pas interroger les données de cas.  
  
 Les exemples suivants illustrent les schémas courants pour créer des requêtes sur les données de cas ou des requêtes sur les données de la structure d'exploration de données :  
  
 **Obtenir toutes les cas d'un modèle**  
 `SELECT FROM <model>.CASES`  
  
 Utilisez cette instruction pour récupérer les colonnes spécifiées des données de cas utilisées pour générer un modèle. Vous devez disposer d'autorisations d'extraction sur le modèle pour exécuter cette requête.  
  
 **Afficher toutes les données incluses dans la structure**  
 `SELECT FROM <structure>.CASES`  
  
 Utiliser cette instruction pour afficher toutes les données incluses dans la structure, y compris les colonnes qui ne sont pas incluses dans un modèle d'exploration de données particulier. Vous devez disposer d'autorisations d'extraction sur le modèle, ainsi que sur la structure, afin de récupérer des données de la structure d'exploration de données.  
  
 **Obtenir une plage de valeurs**  
 `SELECT DISTINCT RangeMin(<column>), RangeMax(<column>) FROM <model>`  
  
 Utilisez cette instruction pour rechercher la valeur minimale, la valeur maximale et la moyenne d'une colonne continue, ou des compartiments d'une colonne discrétisée.  
  
 **Obtenir des valeurs distinctes**  
 `SELECT DISTINCT <column>FROM <model>`  
  
 Utilisez cette instruction pour récupérer toutes les valeurs d'une colonne DISCRÈTE.  N'utilisez pas cette instruction pour les colonnes discrétisées ; utilisez à la place les fonctions **RangeMin** et **RangeMax** .  
  
 **Rechercher les cas utilisés pour l'apprentissage d'un modèle ou d'une structure**  
 `SELECT  FROM <mining structure.CASES WHERE IsTrainingCase()`  
  
 Utilisez cette instruction pour obtenir le jeu complet de données qui ont été utilisées dans l'apprentissage d'un modèle.  
  
 **Rechercher les cas utilisés pour tester un modèle ou une structure**  
 `SELECT  FROM <mining structure.CASES WHERE IsTestingCase()`  
  
 Utilisez cette instruction pour obtenir les données qui ont été mises de côté pour tester des modèles d'exploration de données associés à une structure spécifique.  
  
 **Extraction d'un schéma de modèle spécifique vers des données de cas sous-jacentes**  
 `SELECT FROM <model>.CASESWHERE IsTrainingCase() AND IsInNode(<node>)`  
  
 Utilisez cette instruction pour récupérer des données de cas détaillées d'un modèle ayant fait l'objet d'un apprentissage. Vous devez spécifier un nœud spécifique : par exemple, vous devez connaître l'ID de nœud du cluster, la branche spécifique de l'arbre de décision, etc. De plus, vous devez disposer d'autorisations d'extraction sur le modèle pour exécuter cette requête.  
  
###  <a name="bkmk_Patterns"></a> Requêtes sur les schémas de modèle, les statistiques et les attributs  
 Le contenu d'un modèle d'exploration de données est utile à de nombreuses fins. Avec une requête de contenu de modèle, vous pouvez effectuer les opérations suivantes :  
  
-   Extraire les formules ou les probabilités permettant d'effectuer vos propres calculs.  
  
-   Pour un modèle d'association, récupérer les règles utilisées pour générer une prédiction.  
  
-   Récupérer les descriptions de règles spécifiques afin que vous puissiez utiliser les règles dans une application personnalisée.  
  
-   Afficher les moyennes mobiles détectées par un modèle de série chronologique.  
  
-   Obtenir la formule de régression pour plusieurs segments de la courbe de tendance.  
  
-   Récupérer des informations utilisables sur les clients identifiés comme faisant partie d'un cluster spécifique.  
  
 Les exemples suivants montrent certains schémas courants pour créer des requêtes sur le contenu du modèle :  
  
 **Obtenir des schémas du modèle**  
 `SELECT FROM <model>.CONTENT`  
  
 Utilisez cette instruction pour récupérer des informations détaillées sur les nœuds spécifiques dans le modèle. Selon le type d'algorithme, le nœud peut contenir des règles et des formules, des statistiques de variance et de prise en charge, etc.  
  
 **Récupérer des attributs utilisés dans un modèle ayant fait l'objet d'un apprentissage**  
 `CALL System.GetModelAttributes(<model>)`  
  
 Utilisez cette procédure stockée pour récupérer la liste des attributs utilisés par un modèle. Ces informations sont utiles pour déterminer les attributs qui ont été éliminés en raison de la sélection des fonctionnalités, par exemple.  
  
 **Récupérer le contenu stocké dans une dimension d'exploration de données**  
 `SELECT FROM <model>.DIMENSIONCONTENT`  
  
 Utilisez cette instruction pour récupérer les données d'une dimension d'exploration de données.  
  
 Ce type de requête est principalement destiné à une utilisation interne. Toutefois, tous les algorithmes ne prennent pas en charge cette fonctionnalité. La prise en charge est indiquée par un indicateur dans l'ensemble de lignes de schéma MINING_SERVICES.  
  
 Si vous développez votre propre algorithme de plug-in, vous pouvez utiliser cette instruction pour vérifier le contenu de votre modèle pour le test.  
  
 **Obtenir la représentation PMML d'un modèle**  
 `SELECT * FROM <model>.PMML`  
  
 Obtient un document XML qui représente le modèle au format PMML. Tous les types de modèle ne sont pas pris en charge.  
  
##  <a name="bkmk_Examples"></a> Exemples  
 Bien que certains contenus de modèle soient standard dans les algorithmes, certaines parties du contenu varient considérablement, selon l'algorithme que vous avez utilisé pour générer le modèle. Par conséquent, lorsque vous créez une requête de contenu, vous devez identifier les types d'informations qui sont les plus utiles dans votre modèle spécifique.  
  
 Certains exemples sont fournis dans cette section pour montrer comment le choix de l'algorithme affecte le type d'informations stockées dans le modèle. Pour plus d’informations sur le contenu du modèle d’exploration de données et sur le contenu spécifique à chaque type de modèle, consultez [Contenu du modèle d’exploration de données &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md).  
  
###  <a name="bkmk_Assoc"></a> Exemple 1 : requête de contenu sur un modèle d'association  
 L'instruction, `SELECT FROM <model>.CONTENT`, retourne différents types d'informations, selon le type de modèle que vous interrogez. Pour un modèle d'association, le *type de nœud*constitue une information clé. Les nœuds sont comme des conteneurs pour les informations dans le contenu du modèle. Dans un modèle d'association, les nœuds qui représentent des règles ont une valeur NODE_TYPE de 8, tandis que les nœuds qui représentent des jeux d'éléments ont une valeur NODE_TYPE de 7.  
  
 Ainsi, la requête suivante retourne les 10 premiers jeux d’éléments, classés par prise en charge (classement par défaut).  
  
```  
SELECT TOP 10 NODE_DESCRIPTION, NODE_PROBABILITY, SUPPORT  
FROM <model>.CONTENT WHERE NODE_TYPE = 7  
```  
  
 La requête suivante est générée sur la base de ces informations. La requête suivante retourne trois colonnes : l'ID du nœud, la règle complète et le produit dans la partie droite du jeu d'éléments, c'est-à-dire le produit prédit pour être associé à d'autres produits dans le cadre d'un jeu d'éléments.  
  
```  
SELECT FLATTENED NODE_UNIQUE_NAME, NODE_DESCRIPTION,  
     (SELECT RIGHT(ATTRIBUTE_NAME, (LEN(ATTRIBUTE_NAME)-LEN('Association model name')))   
FROM NODE_DISTRIBUTION  
WHERE LEN(ATTRIBUTE_NAME)>2  
)   
AS RightSideProduct  
FROM [<Association model name>].CONTENT  
WHERE NODE_TYPE = 8   
ORDER BY NODE_SUPPORT DESC  
```  
  
 Le mot clé FLATTENED indique que l'ensemble de lignes imbriqué doit être converti en table aplatie. L'attribut qui représente le produit dans la partie droite de la règle est contenu dans la table NODE_DISTRIBUTION ; par conséquent, nous récupérons uniquement la ligne qui contient un nom d'attribut en spécifiant que la longueur doit être supérieure à 2.  
  
 Une fonction de chaîne simple est utilisée pour supprimer le nom du modèle de la troisième colonne. (En général, le nom du modèle est préfixé avec les valeurs des colonnes imbriquées.)  
  
 La clause WHERE spécifie que la valeur de NODE_TYPE doit être 8 pour récupérer uniquement des règles.  
  
 Pour obtenir plus d’exemples, consultez [Exemples de requête de modèle d’association](../../analysis-services/data-mining/association-model-query-examples.md).  
  
###  <a name="bkmk_DecTree"></a> Exemple 2 : requête de contenu sur un modèle d'arbre de décision  
 Un modèle d'arbre de décision peut être utilisé pour la prédiction ainsi que pour la classification.  Dans cet exemple, on suppose que vous utilisez le modèle pour prédire un résultat, mais vous souhaitez également découvrir quels facteurs ou règles peuvent être utilisés pour classer les résultats.  
  
 Dans un modèle d'arbre de décision, les nœuds sont utilisés pour représenter des arbres et des nœuds terminaux. La légende de chaque nœud contient la description du chemin d'accès au résultat. Par conséquent, pour tracer le chemin d'accès d'un résultat particulier, vous devez identifier le nœud qui le contient, et obtenir des détails relatifs à ce nœud.  
  
 Dans votre requête de prédiction, vous ajoutez la fonction de prédiction [PredictNodeId &#40;DMX&#41;](../../dmx/predictnodeid-dmx.md), afin d’obtenir l’ID du nœud associé, comme le montre l’exemple suivant :  
  
```  
SELECT  Predict([Bike Buyer]), PredictNodeID([Bike Buyer])   
FROM [<decision tree model name>]  
PREDICTION JOIN   
<input rowset>   
```  
  
 Une fois que vous avez l'ID du nœud qui contient le résultat, vous pouvez récupérer la règle ou le chemin d'accès qui explique la prédiction en créant une requête de contenu incluant NODE_CAPTION, comme suit :  
  
```  
SELECT NODE_CAPTION  
FROM [<decision tree model name>]   
WHERE NODE_UNIQUE_NAME= '<node id>'  
```  
  
 Pour obtenir plus d’exemples, consultez [Exemples de requêtes de modèle d’arbre de décision](../../analysis-services/data-mining/decision-trees-model-query-examples.md).  
  
##  <a name="bkmk_Results"></a> Utilisation des résultats de requête  
 Comme le montrent les exemples, les requêtes de contenu retournent principalement des ensembles de lignes tabulaires, mais ils peuvent également inclure des informations des colonnes imbriquées. Vous pouvez aplatir l'ensemble de lignes retourné, mais cela peut rendre l'utilisation des résultats plus complexe. Le contenu du nœud NODE_DISTRIBUTION en particulier est imbriqué, mais il contient des informations intéressantes sur le modèle.  
  
 Pour plus d'informations sur l'utilisation des ensembles de lignes hiérarchiques, consultez la spécification OLEDB sur MSDN.  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation de l’instruction DMX Select](../../dmx/understanding-the-dmx-select-statement.md)   
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)  
  
  
