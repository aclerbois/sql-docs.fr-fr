---
title: Graphique des bénéfices (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7cb1a6e0212251eea45e4cc0b8add1cdd99b342c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="profit-chart-analysis-services---data-mining"></a>Graphique des bénéfices (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un graphique des bénéfices affiche la rentabilité estimée associée à l'utilisation d'un modèle d'exploration de données. Par exemple, supposons que votre modèle prédit les clients qu'une société doit contacter dans un scénario d'application professionnelle. Dans ce cas, vous devez ajouter au graphique des bénéfices des informations relatives au coût de la campagne de publipostage ciblée. Ensuite, dans le graphique terminé, consultez les bénéfices estimés si les clients sont correctement ciblés, par à rapport à la prise de contact aléatoire avec des clients.  
  
## <a name="build-a-profit-chart"></a>Créer un graphique des bénéfices  
 Un graphique des bénéfices est similaire à un graphique de courbes d'élévation. Commencez par créer un graphique de courbes d'élévation, puis ajoutez les informations relatives au coût et aux bénéfices.  
  
 Pour créer un graphique des bénéfices, vous devez disposer d'un modèle existant.  
  
 Pour cet exemple, nous avons utilisé le modèle d'arbre de décision de publipostage ciblé. Le modèle identifie les clients qui sont susceptibles d'acheter un vélo. Vous pouvez appliquer le **Graphique des bénéfices** pour déterminer le nombre de clients à cibler pour optimiser vos bénéfices.  
  
 Si vous n’avez pas d’exemple de modèle, vous pouvez en créer un à l’aide du [Didacticiel d’exploration de données de base](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c).  
  
1.  Ouvrez le concepteur Graphique d'analyse de précision de l'exploration de données.  
  
    -   Dans SQL Server Management Studio, cliquez avec le bouton droit sur le modèle, puis sélectionnez **Afficher le graphique de courbe d’élévation**.  
  
    -   Dans SQL Server Data Tools, ouvrez le projet dans lequel vous avez créé le modèle, puis cliquez sur l’onglet **Graphique d’analyse de précision de l’exploration de données** .  
  
2.  Sous l’onglet **Sélection d’entrée** , sélectionnez le modèle et choisissez la valeur d’attribut prédictible.  
  
     Pour ce scénario particulier, vous êtes uniquement intéressé par la rentabilité de prédiction avec précision d'une seule valeur : [Bike Buyer] =1.  
  
     Cependant, il existe d'autres scénarios, qui vous intéressent tout autant, de prédiction des valeurs False. Par exemple, le coût d'un faux positif sur un test de diagnostic médical peut être significatif et doit être factorisé dans la rentabilité de la prédiction, comme le coût des faux négatifs. Dans ces scénarios, vous devez mesurer tous les résultats.  
  
3.  Sélectionnez un jeu de données de test. Pour cet exemple, sélectionnez le jeu de données de test.  
  
4.  Cliquez maintenant sur l’onglet **Graphique de courbes d’élévation** .  
  
     Un graphique de courbes d'élévation est automatiquement créé.  
  
5.  Pour le transformer en graphique des bénéfices, sélectionnez **Graphique des bénéfices** dans la liste **Type de graphique** .  
  
6.  La boîte de dialogue **Paramètres du graphique des bénéfices** s’ouvre automatiquement dès lors que vous choisissez le graphique des bénéfices comme type de graphique.  
  
     Cette boîte de dialogue vous permet de spécifier les coûts et les bénéfices associés à une campagne de publipostage ciblée. Pour le graphique affiché dans ces exemples, nous avons utilisé les valeurs suivantes :  
  
    |Paramètre|Value|Commentaires|  
    |-------------|-----------|--------------|  
    |**Remplissage**|20 000.|Définir la valeur de la population cible totale<br /><br /> Votre base de données peut contenir de nombreux clients, mais pour économiser sur les dépenses de publipostage, vous ne ciblez que les 20 000 clients les plus susceptibles de répondre. Vous pouvez obtenir cette liste lors de l'exécution d'une requête de prédiction et du tri sur la probabilité générée par le modèle prédictif.|  
    |**Coût fixe**|500|Entrez le coût fixe de la configuration d'une campagne de publipostage ciblée pour 20 000 personnes. Cela peut inclure l'impression, ou le coût de configuration d'une campagne de publipostage.|  
    |**Coût individuel**|3|Entrez le coût unitaire de la campagne de publipostage ciblée.<br /><br /> Ce montant sera multiplié par un nombre inférieur ou égal à 20 000, en fonction du nombre de clients que le modèle prédira comme étant des prospects valables.|  
    |**Revenu par personne**|400|Entrez une valeur qui représente le montant des bénéfices ou des recettes qui peut être attendu d'un résultat réussi. Dans ce cas, nous supposerons que le publipostage d'un catalogue entraîne l'achat d'accessoires ou de vélos d'une valeur moyenne de $400.<br /><br /> Ce montant sera utilisé pour projeter le total des bénéfices associés aux cas à probabilité élevée.|  
  
7.  Après avoir défini les paramètres obligatoires, cliquez sur **OK**.  
  
8.  Le graphique est mis à jour pour afficher la courbe des bénéfices.  
  
## <a name="understanding-the-profit-chart"></a>Fonctionnement du graphique des bénéfices  
 Le diagramme qui suit montre le graphique qui était basé sur ces paramètres. L'axe Y du graphique représente les bénéfices, tandis que l'axe X représente le pourcentage des clients qui ont été contactés par la campagne de publipostage ciblée.  
  
 Tel que cela est indiqué ici, vous pouvez utiliser un graphique des bénéfices pour comparer plusieurs modèles, à condition que tous prédisent le même attribut discret.  
  
 ![une marge de graphique comparant trois modèles](../../analysis-services/data-mining/media/dm14-profitchartupdated.gif "profit du graphique comparant trois modèles")  
  
 Notez la ligne verticale grise dans le graphique. Lorsque vous cliquez et faites glisser la ligne, l'info-bulle affiche le pourcentage de la population cible incluse dans la courbe à ce stade.  
  
 Chaque fois que vous faites glisser la ligne, la **Légende d’exploration de données** est mise à jour pour afficher la valeur de pourcentage, un score de bénéfices et la probabilité de prédiction qui est associée au pourcentage de population indiqué au niveau de la ligne grise verticale.  
  
 Par exemple, si vous utilisez ce modèle pour décider à qui envoyer vos documents promotionnels, vous pouvez décider de cibler 25 % de la population, en fonction des probabilités de prédiction. Cependant, la zone sous la courbe des bénéfices du graphique est optimale entre 40 et 70 pour cent, ce qui signifie que le publipostage à un plus grand nombre de personnes permet d'augmenter votre retour sur investissement, même si un pourcentage de population global plus faible répond.  
  
## <a name="saving-charts"></a>Enregistrement des graphiques  
 Lorsque vous créez un graphique d'analyse de précision ou un graphique des bénéfices, aucun objet n'est créé sur le serveur. À la place, les requêtes sont exécutées dans un modèle existant et les résultats sont générés dans la visionneuse. Si vous devez enregistrer les résultats, vous devez copier le graphique ou les résultats dans Excel ou dans un autre fichier.  
  
## <a name="related-content"></a>Contenu connexe  
 Les rubriques suivantes contiennent davantage d'informations sur la façon dont vous pouvez créer et utiliser des graphiques d'analyse de précision.  
  
|Rubriques|Liens|  
|------------|-----------|  
|Propose une procédure pas à pas permettant de créer un graphique de courbes d'élévation pour le modèle de publipostage ciblé.|[Didacticiel d’exploration de données de base de données](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c)<br /><br /> [Test de la précision avec des graphiques de courbes d’élévation & #40 ; Didacticiel d’exploration de données de base de données & #41 ;](http://msdn.microsoft.com/library/822d414b-4a39-473f-80c3-53476e30655a)|  
|Explique les types de graphique associés.|[Graphique de courbes d’élévation & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/lift-chart-analysis-services-data-mining.md)<br /><br /> [Matrice de classification & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/classification-matrix-analysis-services-data-mining.md)<br /><br /> [Nuage de points & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/scatter-plot-analysis-services-data-mining.md)|  
|Décrit la validation croisée des modèles d'exploration de données et des structures d'exploration de données.|[La Validation croisée & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md)|  
|Décrit les étapes permettant de créer des graphiques de courbes d'élévation et d'autres graphiques d'analyse de précision.|[Test et de tâches de Validation et de procédures & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-tasks-and-how-tos-data-mining.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)   
 [Test de la précision avec des graphiques de courbes d’élévation & #40 ; Didacticiel d’exploration de données de base de données & #41 ;](http://msdn.microsoft.com/library/822d414b-4a39-473f-80c3-53476e30655a)  
  
  
