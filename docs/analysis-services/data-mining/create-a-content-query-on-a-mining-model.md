---
title: Créer une requête de contenu sur un modèle d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7a0bc8d9a216f55f04cab4a4012945d2b11cf429
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-content-query-on-a-mining-model"></a>Créer une requête de contenu sur un modèle d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez interroger par programme le contenu du modèle d'exploration de données en utilisant AMO ou XML/A, mais il est plus facile de créer des requêtes à l'aide de DMX. Vous pouvez créer des requêtes sur les ensembles de lignes de schéma d'exploration de données en établissant une connexion à l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et en créant une requête utilisant les vues DMV fournies par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Les procédures suivantes montrent comment créer des requêtes sur un modèle d'exploration de données en utilisant DMX, et comment interroger les ensembles de lignes de schéma d'exploration de données.  
  
 Pour obtenir un exemple illustrant la façon de créer une requête similaire en utilisant XML/A, consultez [Créer une requête d’exploration de données en utilisant XMLA](../../analysis-services/data-mining/create-a-data-mining-query-by-using-xmla.md).  
  
## <a name="querying-data-mining-model-content-by-using-dmx"></a>Interrogation du contenu de modèle d'exploration de données en utilisant DMX  
  
#### <a name="to-create-a-dmx-model-content-query"></a>Pour créer une requête de contenu de modèle DMX  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le menu **Affichage** , cliquez sur **Explorateur de modèles**.  
  
2.  Dans le volet **Explorateur de modèles** , cliquez sur l'icône de cube pour modifier la liste et afficher les modèles Analysis Services.  
  
3.  Dans la liste des catégories de modèles, développez **DMX**, développez **Contenu des modèles**et double-cliquez sur **Requête de contenu**.  
  
4.  Dans la boîte de dialogue **Se connecter à Analysis Services** , sélectionnez l'instance qui contient le modèle d'exploration de données que vous voulez interroger et cliquez sur **Connexion**.  
  
     Le modèle **Requête de contenu** s'ouvre dans l'éditeur de code approprié. Le volet de métadonnées répertorie les modèles qui sont disponibles dans la base de données active. Pour changer la base de données, sélectionnez une autre base de données dans la liste **Bases de données disponibles** .  
  
5.  Entrez le nom d’un modèle d’exploration de données dans la ligne, `FROM` [*\<le modèle d’exploration de données, name, MyModel >*]`.CONTENT`. Si le nom du modèle d'exploration de données contient des espaces, vous devez le mettre entre crochets.  
  
     Si vous ne voulez pas taper le nom, vous pouvez sélectionner un modèle d'exploration de données dans l' **Explorateur d'objets** et le faire glisser dans le modèle.  
  
6.  Dans la ligne, `SELECT` *\<liste de sélection, la liste expr, \* >*, tapez les noms de colonnes dans l’ensemble de lignes du schéma de contenu du modèle d’exploration de données.  
  
     Pour afficher une liste des colonnes que vous pouvez retourner dans les requêtes de contenu de modèle d’exploration de données, consultez [Contenu du modèle d’exploration &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md).  
  
7.  En option, tapez une condition dans la clause WHERE du modèle pour restreindre les lignes retournées à des nœuds ou des valeurs spécifiques.  
  
8.  Cliquez sur **Exécuter**.  
  
## <a name="querying-the-data-mining-schema-rowsets"></a>Interrogation des ensembles de lignes de schéma d'exploration de données  
  
#### <a name="to-create-a-query-against-the-data-mining-schema-rowset"></a>Pour créer une requête sur l'ensemble de lignes de schéma d'exploration de données  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans la barre d'outils **Nouvelle requête** , cliquez sur **Requête DMX Analysis Services**ou sur **Requête MDX Analysis Services**.  
  
2.  Dans la boîte de dialogue **Se connecter à Analysis Services** , sélectionnez l'instance qui contient les objets que vous voulez interroger et cliquez sur **Connexion**.  
  
     Le modèle **Requête de contenu** s'ouvre dans l'éditeur de code approprié. Le volet de métadonnées répertorie les objets qui sont disponibles dans la base de données active. Pour changer la base de données, sélectionnez une autre base de données dans la liste **Bases de données disponibles** .  
  
3.  Dans l'éditeur de requête, tapez :  
  
     `SELECT *`  
  
     `FROM $system.DMSCHEMA_MINING_MODEL_CONTENT`  
  
     `WHERE MODEL_NAME = '<model name>'`  
  
4.  Cliquez sur **Exécuter**.  
  
     Le volet Résultats affiche le contenu du modèle.  
  
    > [!NOTE]  
    >  Pour afficher la liste de tous les ensembles de lignes de schéma que vous pouvez interroger sur l'instance active, utilisez cette requête : `SELECT * FROM $system.`DISCOVER_SCHEMA_ROWSETS. Ou, pour obtenir la liste des ensembles de lignes de schéma spécifiques à l'exploration de données, consultez [Data Mining Schema Rowsets](../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu du modèle d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md)   
 [Ensembles de lignes de schéma de données d’exploration de données](../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
  
