---
title: Requêtes de prédiction (exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7a8bc3dac0b76adc326b5beab8444475fb76af8d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="prediction-queries-data-mining"></a>Prediction Queries (Data Mining)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un projet d'exploration de données type a pour objectif d'utiliser le modèle d'exploration de données pour faire des prédictions. Par exemple, vous pouvez prédire le temps d'inactivité prévu d'un certain cluster de serveurs, ou générer un score qui indique si des segments de clients sont susceptibles de répondre à une campagne de publicité. Pour effectuer toutes ces tâches, vous devez créer une requête de prédiction.  
  
 Fonctionnellement, il existe différents types de requêtes de prédiction prises en charge dans SQL Server, selon le type d'entrée de la requête :  
  
|Type de requête|Options de requête|  
|----------------|-------------------|  
|Requêtes de prédiction singleton|Utilisez une requête singleton lorsque vous souhaitez prédire des résultats pour un nouveau cas unique, ou plusieurs nouveaux cas. Vous fournissez les valeurs d'entrée directement dans la requête, et la requête est exécutée en tant que session unique.|  
|Prédictions par lot|Utilisez les prédictions par lot lorsque vous disposez de données externes que vous souhaitez alimenter dans le modèle, à utiliser comme base pour les prédictions. Pour effectuer des prédictions pour un jeu complet de données, mappez les données de la source externe aux colonnes du modèle, puis spécifiez le type de données prédictives que vous souhaitez générer.<br /><br /> La requête pour le dataset entier est exécutée au cours d'une même session, ce qui rend cette option beaucoup plus efficace que l'envoi de plusieurs requêtes répétées.|  
|Prédictions de série chronologique|Utilisez une requête de série chronologique lorsque vous souhaitez prédire une valeur sur un certain nombre d'étapes futures. L'exploration de données SQL Server fournit également les fonctionnalités suivantes dans les requêtes de série chronologique :<br /><br /> Vous pouvez étendre un modèle existant en ajoutant de nouvelles données dans le cadre de la requête, puis élaborer des prédictions basées sur la série composite.<br /><br /> Vous pouvez appliquer un modèle existant à une nouvelle série de données en utilisant l'option REPLACE_MODEL_CASES.<br /><br /> Vous pouvez effectuer une prédiction croisée.|  
  
 Les sections suivantes décrivent la syntaxe générale des requêtes de prédiction, les différents types de requêtes de prédiction et la manière d'utiliser les résultats des requêtes de prédiction.  
  
 [Conception d'une requête de prédiction de base](#bkmk_PredQuery)  
  
-   [Ajout de fonctions de prédiction](#bkmk_PredFunc)  
  
-   [Requêtes singleton](#bkmk_SingletonQuery)  
  
-   [Requêtes de prédiction par lot](#bkmk_BatchQuery)  
  
-   [Requêtes de série chronologique](#bkmk_TSQuery)  
  
 [Utilisation des résultats de requêtes](#bkmk_WorkResults)  
  
##  <a name="bkmk_PredQuery"></a> Conception d'une requête de prédiction de base  
 Lorsque vous créez une prédiction, vous fournissez en général des nouvelles données et vous demandez au modèle de générer une prédiction basée sur ces nouvelles données.  
  
-   Dans une requête de prédiction par lot, vous mappez le modèle à une source externe de données à l'aide d'une *jointure de prédiction*.  
  
-   Dans une requête de prédiction singleton, tapez une ou plusieurs valeurs à utiliser comme entrées. Vous pouvez créer plusieurs prédictions à l'aide d'une requête de prédiction singleton. Toutefois, si vous devez créer un grand nombre de prédictions, les performances sont meilleures si vous utilisez une requête par lot.  
  
 Les requêtes de prédiction singleton et par lot utilisent la syntaxe PREDICTION JOIN pour définir les nouvelles données. La différence réside dans la façon dont la partie entrée de la jointure de prédiction est spécifiée.  
  
-   Dans une requête de prédiction par lot, les données proviennent d'une source de données externe spécifiée selon la syntaxe OPENQUERY.  
  
-   Dans une requête de prédiction singleton, les données sont incluses dans le cadre de la requête.  
  
 Pour les modèles de série chronologique, les données d'entrée ne sont pas toujours nécessaires ; il est possible d'effectuer des prédictions en utilisant uniquement les données figurant déjà dans le modèle. Toutefois, si vous spécifiez de nouvelles données d'entrée, vous devez décider si vous allez utiliser les nouvelles données pour mettre à jour et étendre le modèle, ou remplacer la série d'origine de données utilisée dans le modèle.  Pour plus d'informations sur ces options, consultez [Time Series Model Query Examples](../../analysis-services/data-mining/time-series-model-query-examples.md).  
  
###  <a name="bkmk_PredFunc"></a> Ajout de fonctions de prédiction  
 Outre la prédiction d'une valeur, vous pouvez personnaliser une requête de prédiction en vue de retourner plusieurs types d'informations en rapport avec la prédiction. Par exemple, si la prédiction crée une liste de produits à recommander à un client, vous souhaiterez peut-être également retourner la probabilité de chaque prédiction, afin de pouvoir les classer et présenter uniquement les meilleures recommandations à l'utilisateur.  
  
 Pour cela, vous ajoutez des *fonctions de prédiction* à la requête. Chaque type de modèle ou de requête prend en charge des fonctions spécifiques. Par exemple, les modèles de clustering prennent en charge des fonctions de prédiction spéciales qui fournissent des détails supplémentaires sur les clusters créés par le modèle, tandis que les modèles de série chronologique comportent des fonctions qui calculent les différences au fil du temps. Il existe aussi des fonctions de prédiction générales qui fonctionnent avec la plupart des types de modèles. Pour obtenir la liste des fonctions de prédiction prises en charge dans les différents types de requête, consultez cette rubrique dans la référence DMX : [Fonctions de prédiction générales &#40;DMX&#41;](../../dmx/general-prediction-functions-dmx.md).  
  
###  <a name="bkmk_SingletonQuery"></a> Création de requêtes de prédiction singleton  
 Une requête de prédiction singleton est utile lorsque vous souhaitez créer des prédictions rapides en temps réel. Un scénario courant consiste à obtenir des informations à partir d'un client, probablement à l'aide d'un formulaire sur un site Web, puis à soumettre ces données comme entrée à une requête de prédiction singleton. Par exemple, lorsqu'un client choisit un produit dans une liste, vous pouvez utiliser cette sélection comme entrée à une requête qui prédit les meilleurs produits à recommander.  
  
 Les requêtes de prédiction singleton ne requièrent pas une table séparée qui contient l'entrée. À la place, vous fournissez une ou plusieurs lignes de valeurs comme entrée pour le modèle, et la ou les prédictions sont retournées en temps réel.  
  
> [!WARNING]  
>  Comme leur nom ne l'indique pas, les requêtes de prédiction singleton ne se contentent pas simplement de créer des prévisions uniques ; vous peuvent générer plusieurs prédictions pour chaque ensemble d'entrées. Pour fournir plusieurs cas d'entrée, créez une instruction SELECT pour chaque cas d'entrée et associez-la à l'opérateur UNION.  
  
 Lorsque vous créez une requête de prédiction singleton, vous devez fournir les nouvelles données au modèle sous la forme d'une jointure PREDICTION JOIN. Cela signifie que bien que vous n'effectuiez pas le mappage à une table réelle, vous devez vous assurer que les nouvelles données correspondent aux colonnes existantes dans le modèle d'exploration de données. Si les nouvelles colonnes de données et les nouvelles données correspondent exactement, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] mappera les colonnes pour vous. On parle alors d'une jointure *NATURAL PREDICTION JOIN*. Toutefois, si les colonnes ne correspondent pas, ou si les nouvelles données ne contiennent pas les mêmes types et quantités de données que le modèle, vous devez spécifier les colonnes du modèle qui seront mappées aux nouvelles données ou spécifier les valeurs manquantes.  
  
###  <a name="bkmk_BatchQuery"></a> Requêtes de prédiction par lot  
 Une requête de prédiction par lot est utile lorsque vous disposez de données externes à utiliser lors de l'élaboration de prédictions. Par exemple, vous pouvez avoir créé un modèle qui classe les clients par catégorie selon leur activité en ligne et leur historique d'achat. Vous pouvez appliquer ce modèle à une liste de prospects récemment acquis, pour créer des projections pour les ventes ou pour identifier les cibles des campagnes proposées.  
  
 Lorsque vous effectuez une jointure de prédiction, vous devez mapper les colonnes du modèle aux colonnes dans la nouvelle source de données. Par conséquent, la source de données que vous choisissez pour une entrée doit correspondre à des données qui sont quelque peu similaires aux données dans le modèle. Les nouvelles informations ne doivent pas nécessairement correspondre exactement, et elles peuvent être incomplètes. Par exemple, supposons que l'apprentissage du modèle a été effectué en utilisant des informations sur le revenu et l'âge, mais que la liste de clients que vous utilisez pour les prédictions comporte l'âge mais rien sur le revenu. Dans ce scénario, vous pouvez mapper les nouvelles données au modèle et créer une prédiction pour chaque client. Toutefois, si le revenu était un prédicteur important pour le modèle, l'absence d'informations complètes affecte la qualité des prédictions.  
  
 Pour obtenir de meilleurs résultats, vous devez joindre autant de colonnes correspondantes que possible entre les nouvelles données et le modèle. Toutefois, la requête réussira même s'il n'y a pas de correspondances. Si aucune colonne n'est jointe, la requête retournera la prédiction marginale, qui est équivalente à l'instruction `SELECT <predictable-column> FROM <model>` sans clause PREDICTION JOIN.  
  
 Une fois que vous avez correctement mappé toutes les colonnes appropriées, exécutez la requête ; [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] effectue des prédictions pour chaque ligne dans les nouvelles données en fonction des schémas dans le modèle. Vous pouvez enregistrer les résultats dans une nouvelle table dans la vue de source de données qui contient les données externes, ou vous pouvez copier et coller les données si vous utilisez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!WARNING]  
>  Si vous utilisez le concepteur de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la source de données externe doit d’abord être définie comme vue de source de données.  
  
 Si vous utilisez DMX pour créer une jointure de prédiction, vous pouvez spécifier la source de données externe en utilisant les commandes OPENQUERY, OPENROWSET ou SHAPE. La méthode d'accès aux données par défaut dans les modèles DMX est OPENQUERY. Pour plus d’informations sur ces méthodes, consultez [&#60;source data query&#62;](../../dmx/source-data-query.md).  
  
###  <a name="bkmk_TSQuery"></a> Prédictions dans les modèles d'exploration de données de série chronologique  
 Les modèles de série chronologique sont différents d'autres types de modèles ; vous pouvez soit utiliser le modèle tel quel afin de créer des prédictions, ou vous pouvez fournir de nouvelles données au modèle pour mettre à jour le modèle et créer des prédictions basées sur les tendances récentes. Si vous ajoutez de nouvelles données, vous pouvez spécifier la façon dont elles doivent être utilisées.  
  
-   L'*extension des cas de modèles* signifie que vous ajoutez les nouvelles données dans la série existante de données dans le modèle de série chronologique. Dorénavant, les prédictions sont basées sur les nouvelles séries combinées. Cette option est appropriée si vous souhaitez simplement ajouter quelques points de données à un modèle existant.  
  
     Par exemple, supposons que vous ayez un modèle de série chronologique existant dont l'apprentissage a été effectué sur les chiffres des ventes de l'année précédente. Après avoir collecté plusieurs mois de nouveaux chiffres de ventes, vous décidez de mettre à jour vos prévisions de ventes pour l'année actuelle. Vous pouvez créer une jointure de prédiction qui met à jour le modèle en ajoutant de nouvelles données et qui étend le modèle pour faire de nouvelles prédictions.  
  
-   *Remplacer les cas de modèle* signifie que vous conservez le modèle ayant fait l'objet d'un apprentissage, mais vous remplacez les cas sous-jacents par un nouveau jeu de données de cas. Cette option est utile lorsque vous souhaitez conserver la tendance dans le modèle, mais l'appliquer à un ensemble différent de données.  
  
     Par exemple, votre modèle d'origine a pu faire l'objet d'un apprentissage sur un ensemble de données avec des volumes de ventes très élevés ; lorsque vous remplacez les données sous-jacentes par une nouvelle série (peut-être d'un magasin avec un volume de ventes inférieur), vous conservez la tendance, mais les prédictions commencent à partir des valeurs de la série de remplacement.  
  
 Quelle que soit l'approche que vous utilisez, le point de départ pour les prédictions est toujours la fin de la série d'origine.  
  
 Pour plus d’informations sur la création de jointures de prédiction sur des modèles de série chronologique, consultez [Time Series Model Query Examples](../../analysis-services/data-mining/time-series-model-query-examples.md) ou [PredictTimeSeries &#40;DMX&#41;](../../dmx/predicttimeseries-dmx.md).  
  
##  <a name="bkmk_WorkResults"></a> Utilisation des résultats d'une requête de prédiction  
 Les options pour enregistrer les résultats d'une requête de prédiction d'exploration de données sont différentes selon la façon dont vous créez la requête.  
  
-   Lorsque vous générez une requête à l'aide du Générateur de requêtes de prédiction dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez enregistrer les résultats d'une requête de prédiction dans une source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante. Pour plus d’informations, consultez [Afficher et enregistrer les résultats d’une requête de prédiction](../../analysis-services/data-mining/view-and-save-the-results-of-a-prediction-query.md).  
  
-   Lorsque vous créez des requêtes de prédiction en utilisant DMX dans le volet de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez utiliser les options de résultat de la requête pour enregistrer les résultats dans un fichier, ou dans le volet de résultats de la requête au format texte ou dans une grille. Pour plus d’informations, consultez [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/query-and-text-editors-sql-server-management-studio.md).  
  
-   Lorsque vous exécutez une requête de prédiction à l'aide des composants Integration Services, les tâches permettent d'écrire les résultats dans une base de données à l'aide d'un gestionnaire de connexions ADO.NET disponible ou du gestionnaire de connexions OLEDB. Pour plus d’informations, consultez [Data Mining Query Task](../../integration-services/control-flow/data-mining-query-task.md).  
  
 Il est important de comprendre que les résultats d'une requête de prédiction sont différents des résultats d'une requête sur une base de données relationnelle, qui retourne toujours une seule ligne de valeurs associées. Chaque fonction de prédiction DMX que vous ajoutez à une requête retourne son propre ensemble de lignes. Par conséquent, lorsque vous faites une prédiction sur un cas unique, le résultat peut être une valeur prédite avec plusieurs colonnes de tables imbriquées contenant des détails supplémentaires.  
  
 Si vous combinez plusieurs fonctions dans une requête, les résultats sont regroupés dans un ensemble de lignes hiérarchique. Par exemple, supposons que vous utilisez un modèle de série chronologique pour prédire les valeurs futures du montant des ventes et la quantité de ventes, à l'aide d'une requête telle que cette instruction DMX :  
  
```  
SELECT  
  PredictTimeSeries([Forecasting].[Amount]) as [PredictedAmount]  
, PredictTimeSeries([Forecasting].[Quantity]) as [PredictedQty]  
FROM  
  [Forecasting]  
  
```  
  
 Les résultats de cette requête sont deux colonnes, une pour chaque série prédite, où chaque ligne contient une table imbriquée avec les valeurs prédites :  
  
 **PredictedAmount**  
  
|$TIME|Montant|  
|-----------|------------|  
|201101|172067.11|  
  
|$TIME|Montant|  
|-----------|------------|  
|201102|363390.68|  
  
 **PredictedQty**  
  
|$TIME|Quantité|  
|-----------|--------------|  
|201101|77|  
  
|$TIME|Quantité|  
|-----------|--------------|  
|201102|260|  
  
 Si votre fournisseur ne peut pas gérer d'ensembles de lignes hiérarchiques, vous pouvez aplatir les résultats en utilisant le mot clé FLATTEN dans la requête de prédiction. Pour plus d’informations et des exemples d’ensembles de lignes aplatis, consultez [SELECT &#40;DMX&#41;](../../dmx/select-dmx.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu des requêtes & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/content-queries-data-mining.md)   
 [Requêtes de définition de données & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/data-definition-queries-data-mining.md)  
  
  
