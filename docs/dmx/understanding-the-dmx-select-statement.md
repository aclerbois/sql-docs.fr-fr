---
title: Présentation de l’instruction DMX Select (instruction) | Documents Microsoft
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
- browsing mining model [Analysis Services]
- Data Mining Extensions [Analysis Services], statements
- DMX [Analysis Services], statements
- predictions [DMX]
- queries [DMX], Select statement
- SELECT statement [DMX]
- statements [DMX], SELECT statement
- copying mining models
ms.assetid: 61e97285-4a06-4434-9a40-38cde5af7c3f
caps.latest.revision: 40
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 84f8a735323fe347f97585f453527e9b55afe374
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-the-dmx-select-statement"></a>Présentation de l'instruction DMX Select
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Le [sélectionnez](../dmx/select-dmx.md) instruction constitue la base de la plupart des requêtes que vous créez avec les Extensions DMX (Data Mining) dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Elle peut effectuer une grande diversité de tâches, par exemple la navigation et la prévision dans les modèles d'exploration de données.  
  
 Voici les tâches que vous pouvez accomplir à l’aide de la **sélectionnez** instruction :  
  
-   Explorer un modèle d'exploration de données. L'ensemble de lignes du schéma définit la structure d'un modèle.  
  
-   Découvrir les valeurs possibles de la colonne d'un modèle d'exploration de données.  
  
-   Explorer les cas qui sont affectés aux nœuds d'un modèle d'exploration de données, ou obtenir un cas représentatif.  
  
-   Créer des prédictions à l'aide de diverses entrées.  
  
-   Copier des modèles d'exploration de données.  
  
 Chacune de ces tâches utilise un ensemble différent de données, que nous appellerons un *domaine de données*. Vous définissez le domaine de données dans le **FROM** clause de l’instruction.  
  
-   Vous recherchez les objets dans le modèle d'exploration de données lui-même, par exemple la règle définissant un jeu de données, ou une formule utilisée pour faire des prédictions.  
  
     Dans ce cas, vous devez examiner les métadonnées stockées dans le modèle lui-même. Par conséquent, votre domaine de données correspond aux colonnes de l'ensemble de lignes de schéma d'exploration de données.  
  
-   Vous obtenez des informations détaillées à partir des cas utilisés pour générer le modèle.  
  
     Dans ce cas, vous devez extraire la structure d'exploration de données, qui est votre domaine de données, puis rechercher les lignes individuelles dans les colonnes telles que Gender, Bike Buyer, et ainsi de suite.  
  
 **IMPORTANT :** tout ce qui est inclus dans la liste d’expressions ou dans le **où** clause doit provenir du domaine de données défini par le **FROM** clause. Vous ne pouvez pas mélanger des domaines de données.  
  
##  <a name="Select_Types"></a> Sélectionner les Types  
 La syntaxe de **sélectionnez** instruction prend en charge de nombreuses tâches différentes. Utilisez les modèles suivants pour effectuer ces tâches :  
  
-   [PRÉDICTION](#Predicting)  
  
-   [Navigation](#Browsing)  
  
-   [Copie](#Copying)  
  
-   [extraction](#Drillthrough)  
  
###  <a name="Predicting"></a> PRÉDICTION  
 Vous effectuez des prévisions sur la base d'un modèle d'exploration de données en utilisant les types de requêtes suivants.  
  
 Vous pouvez inclure une navigation ou de prévision **sélectionnez** instructions dans le **FROM** et **où** clauses d’une jointure de prédiction **sélectionnez** instruction.  
  
|Type de requête| Description|  
|----------------|-----------------|  
|SELECT FROM [NATURAL] PREDICTION JOIN|Retourne une prévision qui est créée en joignant les colonnes du modèle d'exploration de données aux colonnes d'une source de données interne.<br /><br /> Le domaine de ce type de requête sont les colonnes prédictibles du modèle et les colonnes de la source de données d'entrée.<br /><br /> [SELECT FROM &#60;modèle&#62; PREDICTION JOIN &#40;DMX&#41;](../dmx/select-from-model-prediction-join-dmx.md)<br /><br /> [Requêtes de prédiction & #40 ; exploration de données & #41 ;](../analysis-services/data-mining/prediction-queries-data-mining.md)|  
|SELECT FROM  *\<modèle >*|Retourne l'état le plus probable de la colonne prédictible, uniquement sur la base du modèle d'exploration de données. Ce type de requête est un raccourci pour créer une prévision avec une jointure de prévision vide.<br /><br /> Le domaine de ce type de requête sont les colonnes prédictibles du modèle.<br /><br /> [SELECT FROM &#60;modèle&#62; &#40;DMX&#41;](../dmx/select-from-model-dmx.md)<br /><br /> [Requêtes de prédiction & #40 ; exploration de données & #41 ;](../analysis-services/data-mining/prediction-queries-data-mining.md)|  
  
 [Retour à Types Select](#Select_Types)  
  
###  <a name="Browsing"></a> Navigation  
 Vous pouvez naviguer dans le contenu d'un modèle d'exploration de données en utilisant les types de requêtes suivants.  
  
|Type de requête| Description|  
|----------------|-----------------|  
|SELECT DISTINCT FROM  *\<modèle >*|Retourne toutes les valeurs d'état provenant du modèle d'exploration de données pour la colonne spécifiée.<br /><br /> Le domaine de données pour ce type de requête est le modèle d'exploration de données.<br /><br /> [SELECT DISTINCT FROM &#60;modèle &#62; &#40;DMX&#41;](../dmx/select-distinct-from-model-dmx.md)<br /><br /> [Requêtes de contenu &#40;d’exploration de données&#41;](../analysis-services/data-mining/content-queries-data-mining.md)|  
|SELECT FROM  *\<modèle >*. CONTENU|Retourne le contenu décrivant le modèle d'exploration de données.<br /><br /> Le domaine de données pour ce type de requête est l'ensemble de lignes du contenu.<br /><br /> [SELECT FROM &#60;modèle&#62;. CONTENU &#40;DMX&#41;](../dmx/select-from-model-content-dmx.md)<br /><br /> [Requêtes de contenu &#40;d’exploration de données&#41;](../analysis-services/data-mining/content-queries-data-mining.md)|  
|SELECT FROM  *\<modèle >*. DIMENSION_CONTENT|Retourne le contenu décrivant le modèle d'exploration de données.<br /><br /> Le domaine de données pour ce type de requête est l'ensemble de lignes du contenu.<br /><br /> [SELECT FROM &#60;modèle&#62;. DIMENSION_CONTENT &#40;DMX&#41;](../dmx/select-from-model-dimension-content-dmx.md)|  
|SELECT FROM  *\<modèle >*. PMML|Retourne la représentation PMML (Predictive Model Markup Language) du modèle d'exploration de données, pour les algorithmes qui prennent en charge cette fonctionnalité.<br /><br /> Le domaine pour ce type de requête est l'ensemble de lignes du schéma PMML.<br /><br /> [Ensemble de lignes DMSCHEMA_MINING_MODEL_CONTENT_PMML](../analysis-services/schema-rowsets/data-mining/dmschema-mining-model-content-pmml-rowset.md)|  
  
 [Retour à Types Select](#Select_Types)  
  
###  <a name="Copying"></a> Copie  
 Vous pouvez copier un modèle d'exploration de données et sa structure d'exploration de données associée dans un nouveau modèle, puis renommer le modèle dans l'instruction.  
  
|Type de requête| Description|  
|----------------|-----------------|  
|SELECT INTO  *\<nouveau modèle >*|Crée une copie du modèle d'exploration de données.<br /><br /> Le domaine pour ce type de requête est le modèle d'exploration de données.<br /><br /> [SELECT INTO &AMP;#40;DMX&AMP;#41;](../dmx/select-into-dmx.md)|  
  
 [Retour à Types Select](#Select_Types)  
  
###  <a name="Drillthrough"></a> extraction  
 Vous pouvez naviguer dans les cas, ou dans une représentation des cas, qui ont servi à l'apprentissage du modèle, en utilisant les types de requêtes suivants.  
  
|Type de requête| Description|  
|----------------|-----------------|  
|SELECT FROM  *\<modèle >*. CAS|Retourne les cas utilisés pour l'apprentissage du modèle d'exploration de données.<br /><br /> Le domaine pour ce type de requête est le modèle d'exploration de données.<br /><br /> [SELECT FROM &#60;modèle&#62;. CAS &#40;DMX&#41;](../dmx/select-from-model-cases-dmx.md)<br /><br /> [Créer des requêtes d’extraction à l’aide de DMX](../analysis-services/data-mining/create-drillthrough-queries-using-dmx.md)|  
|SELECT FROM  *\<modèle >*. SAMPLE_CASES|Retourne un exemple de cas, qui représente les cas utilisés pour l'apprentissage du modèle d'exploration de données.<br /><br /> Le domaine pour ce type de requête est le modèle d'exploration de données.<br /><br /> [SELECT FROM &#60;modèle&#62;. SAMPLE_CASES &#40;DMX&#41;](../dmx/select-from-model-sample-cases-dmx.md)|  
|SELECT FROM  *\<structure >*. CAS|Retourne des lignes de données détaillées de la structure d'exploration de données sous-jacente, même si certains détails n'ont pas été utilisés dans l'apprentissage du modèle d'exploration de données.<br /><br /> [SELECT FROM &#60;structure&#62;. CAS](../dmx/select-from-structure-cases.md)<br /><br /> [Requêtes d’extraction & #40 ; exploration de données & #41 ;](../analysis-services/data-mining/drillthrough-queries-data-mining.md)|  
  
 [Retour à Types Select](#Select_Types)  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)  
  
  
