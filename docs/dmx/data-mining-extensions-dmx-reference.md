---
title: Data Mining Extensions (DMX) référence | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- DMX
helpviewer_keywords:
- DMX [Analysis Services]
- statements [DMX]
- Data Mining Extensions [Analysis Services], statements
- DMX [Analysis Services], about Data Mining Extensions
- DMX [Analysis Services], statements
- data definition statements [DMX]
- predictions [DMX]
- Data Mining Extensions [Analysis Services]
- SSAS, DMX
- queries [DMX], extensions reference
- SQL Server Analysis Services, DMX
- OLE DB for Data Mining
- data manipulation statements [DMX]
- Data Mining Extensions [Analysis Services], about Data Mining Extensions
- mining models [Analysis Services], DMX
ms.assetid: 6d85ca20-de67-4e20-b3b5-b734c6cfcece
caps.latest.revision: 40
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 2a9338c6db570ae14e78b0aa7b8c6e891b648385
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-mining-extensions-dmx-reference"></a>Guide de référence du langage DMX (Data Mining Extensions)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Extensions DMX (Data Mining) est un langage que vous pouvez utiliser pour créer et utiliser des modèles d’exploration de données dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Vous pouvez utiliser DMX pour créer la structure de nouveaux modèles d'exploration de données, pour l'apprentissage de ces modèles et pour les explorer, les gérer et y effectuer des prévisions. Le langage DMX se compose d'instructions DDL (langage de définition de données), d'instructions DML (langage de manipulation de données), de fonctions et d'opérateurs.  
  
## <a name="microsoft-ole-db-for-data-mining-specification"></a>Spécification Microsoft OLE DB pour l'exploration de données  
 Les fonctionnalités d’exploration de données dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] sont conçues pour répondre à la [!INCLUDE[msCoName](../includes/msconame-md.md)] OLE DB pour la spécification de l’exploration de données.  
  
 Le [!INCLUDE[msCoName](../includes/msconame-md.md)] OLE DB pour l’exploration de données définit les éléments suivants :  
  
-   Une structure pour conserver les informations qui définissent un modèle d'exploration de données.  
  
-   Un langage pour créer et utiliser des modèles d'exploration de données.  
  
 La spécification définit la base de l'exploration de données comme étant l'objet virtuel de modèle d'exploration de données. L'objet de modèle d'exploration de données encapsule tout ce qui est connu concernant un modèle d'exploration de données particulier. L'objet de modèle d'exploration de données est structuré comme une table SQL, avec des colonnes, des types de données et des méta-informations qui décrivent le modèle. Cette structure vous permet d'employer le langage DMX, qui est une extension du langage SQL, pour créer des modèles et les utiliser.  
  
 **Pour plus d’informations :** [des Structures d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)  
  
##  <a name="BKMK_DMXStatements"></a> Instructions DMX  
 Vous pouvez utiliser des instructions DMX pour créer, traiter, supprimer, copier, explorer et effectuer des prévisions dans des modèles d'exploration de données. Il existe deux types d'instructions DMX : les instructions de définition de données et les instructions de manipulation de données. Chaque type d'instruction permet d'effectuer différentes sortes de tâches.  
  
 Les sections suivantes fournissent des informations supplémentaires sur l'utilisation des instructions DMX :  
  
-   [Instructions de définition de données](#BKMK_DDL)  
  
-   [Instructions de Manipulation de données](#BKMK_DML)  
  
-   [Requêtes de base](#BKMK_Queries)  
  
###  <a name="BKMK_DDL"></a> Instructions de définition de données  
 Les instructions de définition de données dans DMX permettent de créer et de définir de nouvelles structures et modèles d'exploration de données, d'importer et d'exporter des modèles et des structures d'exploration de données et de supprimer des modèles existants d'une base de données. Les instructions de définition de données dans DMX font partie du langage de définition de données (DDL).  
  
 En utilisant les instructions de définition de données dans DMX, vous pouvez effectuer les tâches suivantes :  
  
-   Créer une structure d’exploration de données à l’aide de la [CREATE MINING STRUCTURE](../dmx/create-mining-structure-dmx.md) instruction et ajouter un modèle d’exploration de données à la structure d’exploration de données à l’aide de la [ALTER MINING STRUCTURE](../dmx/alter-mining-structure-dmx.md) instruction.  
  
-   Créer un modèle d’exploration et la structure d’exploration de données associée simultanément à l’aide de la [CREATE MINING MODEL](../dmx/create-mining-model-dmx.md) instruction pour créer un objet de modèle d’exploration de données les données vides.  
  
-   Exporter un modèle d’exploration et la structure d’exploration de données associée à un fichier à l’aide de la [exporter](../dmx/export-dmx.md) instruction. Importer un modèle d’exploration et la structure d’exploration de données associée à partir d’un fichier qui est créé par l’instruction EXPORT à l’aide de la [importation](../dmx/import-dmx.md) instruction.  
  
-   Copier la structure d’un modèle d’exploration de données existant dans un nouveau modèle et l’apprentissage avec les mêmes données, à l’aide de la [SELECT INTO](../dmx/select-into-dmx.md) instruction.  
  
-   Supprimer complètement un modèle d’exploration de données à partir d’une base de données à l’aide de la [DROP MINING MODEL](../dmx/drop-mining-model-dmx.md) instruction. Supprimer complètement une structure d’exploration de données et tous ses modèles d’exploration de données associée à partir de la base de données à l’aide de la [DROP MINING STRUCTURE](../dmx/drop-mining-structure-dmx.md) instruction.  
  
 Pour en savoir plus sur les tâches d’exploration de données que vous pouvez effectuer à l’aide d’instructions DMX, consultez [Data Mining Extensions &#40;DMX&#41; référence des instructions](../dmx/data-mining-extensions-dmx-statements.md).  
  
 [Retour à instructions DMX](#BKMK_DMXStatements)  
  
###  <a name="BKMK_DML"></a> Instructions de Manipulation de données  
 Les instructions de manipulation de données dans DMX permettent d'utiliser des modèles d'exploration de données existants, d'explorer ces modèles et de créer des prévisions dans ces derniers. Les instructions de manipulation de données dans DMX font partie du langage de manipulation de données (DML).  
  
 En utilisant les instructions de manipulation de données dans DMX, vous pouvez effectuer les tâches suivantes :  
  
-   L’apprentissage d’un modèle d’exploration de données à l’aide de la [INSERT INTO](../dmx/insert-into-dmx.md) instruction. Cette opération n'insert pas les données source réelles dans un modèle d'exploration de données, mais crée plutôt une abstraction qui décrit le modèle d'exploration de données que l'algorithme crée. La requête source pour une instruction INSERT INTO est décrit dans [ \<requête de source de données >](../dmx/source-data-query.md).  
  
-   Étendre l’instruction SELECT pour parcourir les informations qui sont calculées pendant l’apprentissage du modèle et stockées dans le modèle d’exploration de données, telles que les statistiques de la source de données. Voici les clauses que vous pouvez inclure pour étendre la puissance de l’instruction SELECT :  
  
    -   [SELECT DISTINCT FROM &#60;modèle &#62; &#40;DMX&#41;](../dmx/select-distinct-from-model-dmx.md)  
  
    -   [SELECT FROM &#60;modèle&#62;. CONTENU &#40;DMX&#41;](../dmx/select-from-model-content-dmx.md)  
  
    -   [SELECT FROM &#60;modèle&#62;. CAS &#40;DMX&#41;](../dmx/select-from-model-cases-dmx.md)  
  
    -   [SELECT FROM &#60;modèle&#62;. SAMPLE_CASES &#40;DMX&#41;](../dmx/select-from-model-sample-cases-dmx.md)  
  
    -   [SELECT FROM &#60;modèle&#62;. DIMENSION_CONTENT &#40;DMX&#41;](../dmx/select-from-model-dimension-content-dmx.md)  
  
-   Créer des prédictions basées sur un modèle d’exploration de données existant à l’aide de la [PREDICTION JOIN](../dmx/select-from-model-prediction-join-dmx.md) clause de l’instruction SELECT. La requête source pour une instruction PREDICTION JOIN est décrit dans [ \<requête de source de données >](../dmx/source-data-query.md).  
  
-   Supprimer toutes les données d’apprentissage à partir d’un modèle ou une structure à l’aide de la [supprimer &#40;DMX&#41; ](../dmx/delete-dmx.md) instruction.  
  
 Pour en savoir plus sur les tâches d’exploration de données que vous pouvez effectuer à l’aide d’instructions DMX, consultez [Data Mining Extensions &#40;DMX&#41; référence des instructions](../dmx/data-mining-extensions-dmx-statements.md).  
  
 [Retour à instructions DMX](#BKMK_DMXStatements)  
  
###  <a name="BKMK_Queries"></a> Notions de base de requête DMX  
 L’instruction SELECT est la base pour la plupart des requêtes DMX. En fonction des clauses que vous utilisez avec ce type d'instruction, vous pouvez explorer ou copier des modèles d'exploration de données, ou effectuer des prévisions dans ces derniers. La requête de prédiction utilise une forme de sélection pour créer des prédictions basées sur des modèles d’exploration de données existants. Des fonctions augmentent vos capacités à explorer et interroger les modèles d'exploration de données au-delà des possibilités intrinsèques du modèle d'exploration de données.   
  
 Les fonctions DMX permettent d'obtenir des informations qui sont découvertes au cours de l'apprentissage de vos modèles, et de calculer de nouvelles informations. Ces fonctions peuvent être utilisées pour différents objectifs, notamment pour retourner des statistiques décrivant les données sous-jacentes ou l'exactitude d'une prévision ou pour retourner l'explication développée d'une prévision.  
  
 **Pour plus d’informations****informations :** [présentation de l’instruction DMX Select (instruction)](../dmx/understanding-the-dmx-select-statement.md), [fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md), [Structure et l’utilisation de requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md), [Data Mining Extensions &#40;DMX&#41; référence de fonction  ](../dmx/data-mining-extensions-dmx-function-reference.md)  
  
 [Retour à instructions DMX](#BKMK_DMXStatements)  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
