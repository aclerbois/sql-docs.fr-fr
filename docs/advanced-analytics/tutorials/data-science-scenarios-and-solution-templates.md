---
title: Scénarios de science des données et des modèles de solution (SQL Server Machine Learning) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: d67fd15c44d188870989f2ad6498733c5901fb9d
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="data-science-scenarios-and-solution-templates"></a>Scénarios de science des données et des modèles de solution
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Les modèles sont des exemples de solutions qui illustrent les bonnes pratiques et fournissent les bases qui vous aideront à implémenter une solution rapidement. Chaque modèle est conçu pour résoudre un problème spécifique, pour un vertical spécifique ou d’un secteur d’activité. Les tâches dans chaque modèle vont de la préparation des données à la formation du modèle et au calcul des scores, en passant par l’ingénierie des caractéristiques. Utilisez ces modèles pour savoir comment [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] fonctionne. Ensuite, n’hésitez pas à personnaliser le modèle pour l’adapter à votre propre scénario et créer une solution personnalisée. 

Chaque solution inclut des exemples de données, du code de R ou code Python et les procédures stockées SQL le cas échéant. Le code peut être exécuté dans votre environnement de développement R ou Python préféré, avec des calculs demandés SQL Server. Dans certains cas, vous pouvez exécuter le code directement à l’aide de T-SQL et n’importe quel outil client SQL, tels que SQL Server Management Studio.

> [!TIP]
> 
> La plupart des modèles sont disponibles dans plusieurs versions prenant en charge à la fois localement et plateformes pour l’apprentissage de cloud computing. Par exemple, vous pouvez générer la solution en utilisant uniquement à SQL Server, ou vous pouvez générer la solution dans Microsoft R Server, ou dans Azure Machine Learning.

+ Pour plus d’informations et les mises à jour, consultez cette annonce : [passionnantes de nouveaux modèles dans Azure ML](https://blogs.technet.microsoft.com/machinelearning/2015/04/09/exciting-new-templates-in-azure-ml/)

+ Pour le téléchargement et les instructions d’installation, consultez [comment utiliser les modèles](#bkmk_HowTo).

## <a name="fraud-detection"></a>Détection des fraudes

[Modèle de détection de fraude (SQL Server R Services)](https://github.com/Microsoft/r-server-fraud-detection)

**Ce que :** la possibilité de détecter les transactions frauduleuses est importante pour les entreprises en ligne. Pour réduire les pertes de refacturation, les entreprises doivent identifier rapidement les transactions qui ont été apportées à l’aide des informations d’identification ou de paiement du vol. Quand des transactions frauduleuses sont découvertes, les entreprises prennent généralement des mesures pour bloquer certains comptes dès que possible, afin d’empêcher toute perte supplémentaire. Dans ce scénario, vous allez apprendre à utiliser les données des transactions d’achat en ligne pour identifier les fraudes probablement.

**Comment :** la détection des fraudes est résolue comme un problème de classification binaire. Vous pouvez facilement appliquer la méthodologie utilisée dans ce modèle à la détection des fraudes dans d’autres domaines.


## <a name="campaign-optimization"></a>Optimisation de la campagne

[Prévoir comment et quand contacter les prospects](https://microsoft.github.io/r-server-campaign-optimization/)

**Ce que :** cette solution utilise des données du secteur des assurances pour prédire les prospects selon les données démographiques, les données de réponse d’historique et les détails spécifiques au produit.  Recommandations sont également générées pour indiquer le canal meilleures et le temps aux utilisateurs d’approche pour influencer le comportement d’achat.

**Comment :** la solution crée et compare plusieurs modèles. La solution montre également l’intégration de données automatisés et la préparation des données à l’aide de procédures stockées. Exemples parallèles sont fournis pour SQL Server dans-base de données dans Azure et HDInsight Spark. 

## <a name="health-care-predict-length-of-stay-in-hospital"></a>Soins de santé : prévoir la durée de hôpital 

[Prédiction de durée de nombre](https://gallery.cortanaintelligence.com/Solution/Predicting-Length-of-Stay-in-Hospitals-1)

**Ce que :** prévision précise les patients peuvent nécessiter hospitalisation à long terme est une partie importante de soins et de planification. Les administrateurs doivent être en mesure de déterminer les installations nécessitent plus de ressources et personnel de santé souhaitez garantir qu’ils peuvent répondre aux besoins de patients.

**Procédure :** cette solution utilise l’ordinateur virtuel de science des données et inclut une instance de SQL Server avec machine learning est activé. Il inclut également un ensemble de rapports Power BI que vous pouvez utiliser pour interagir avec un modèle déployé.

## <a name="customer-churn"></a>Évolution du code client

[Modèle de prévision de l’évolution du client (SQL Server R Services)](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/Churn/Introduction.md)

**Ce que :** analyse et prédiction d’évolution du code client est important dans n’importe quel secteur d’activité où la perte de clients à des concurrents doit être gérée et a empêché : bancaire, télécommunications et vente au détail, à titre d’exemple. L’objectif de l’analyse de l’attrition consiste à identifier les clients susceptibles de passer à la concurrence, puis d’effectuer les actions appropriées pour conserver ces clients.

**Procédure :** ce modèle formule le problème d’évolution du code comme un **classification binaire** problème. Il utilise des exemples de données de deux sources, données démographiques et transactions de clients, pour classer les clients comme susceptibles ou non de passer à la concurrence.
  
## <a name="predictive-maintenance"></a>Maintenance prédictive

[Modèle de maintenance prédictive (SQL Server 2016)](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/PredictiveMaintenance/Introduction.md)

**Ce que :** maintenance prédictive vise à augmenter l’efficacité des tâches de maintenance en capturant au-delà de pannes et l’utilisation de ces informations pour prédire quand et où un périphérique peut échouer. La possibilité de prévoir l’obsolescence de périphérique est particulièrement utile pour les applications qui reposent sur les données distribuées ou capteurs. Cette méthode peut également être appliquée pour surveiller et de prévoir l’erreur dans les appareils IoT (Internet of Things).

Consultez cette annonce pour plus d’informations : [nouveau modèle de maintenance prédictive](https://blogs.technet.microsoft.com/machinelearning/2015/04/09/exciting-new-templates-in-azure-ml/)

**Procédure :** cette solution se concentre sur la réponse à la question « Quand échoue un ordinateur en service ? » Les données d’entrée représentent des mesures de capteurs simulées pour des moteurs d’avion. Données obtenues à partir de l’analyse actuel opération conditions du moteur, telles que le cycle de travail en cours, les paramètres et les mesures de capteur, sont utilisées pour créer trois types de modèles prédictifs :

-   **Modèles de régression**, pour prédire dans combien de temps un moteur tombera en panne. L’exemple de modèle prédit la mesure « Restantes cycle de vie » (RUL), également appelée heure à « Échec » (TTF).
  
-   **Modèles de classification**, pour prédire si un moteur est susceptible de tomber en panne.
  
    Le **modèle de classification binaire** prédit si un moteur échoue dans un intervalle de temps donné.

    Le **modèle de classification de multi-class** prédit si un moteur particulier peut échouer et fournit une fenêtre de temps probable de l’échec. Par exemple, pour un jour donné, vous pouvez prédire si un appareil est susceptible de tomber en panne ce jour-là ou dans un laps de temps suivant ce jour.

## <a name="energy-demand-forecasting"></a>Prévision de la demande d’énergie

[Demande d’énergie en prévision de modèle avec SQL Server R Services](https://gallery.cortanaintelligence.com/Tutorial/Energy-Demand-Forecast-Template-with-SQL-Server-R-Services-1)

**Ce que :**: prévision de la demande est un problème important dans différents domaines, y compris l’énergie, de vente au détail et de services. Prévision de la demande exacte aide les entreprises à effectuer la planification, l’allocation de ressources, de la production mieux et autres importantes décisions commerciales. Dans le secteur de l’énergie, la prévision de la demande est critique pour réduire le coût de stockage de l’énergie et de l’équilibrage offre et la demande.

**Procédure :** ce modèle utilise SQL Server R Services pour prédire la demande de l’électricité. Le modèle utilisé pour la prédiction est un modèle de régression de forêt aléatoire selon **rxDForest**, un algorithme inclus dans Microsoft R Server hautes performances d’apprentissage. La solution comprend un simulateur de demande, tout le code R et T-SQL nécessaire pour former un modèle, ainsi que les procédures stockées que vous pouvez utiliser pour générer des prédictions et créer des rapports. 


## <a name="bkmk_HowTo"></a>Comment utiliser les modèles

Pour télécharger les fichiers fournis avec chaque modèle, vous pouvez utiliser des commandes GitHub ou ouvrir le lien et cliquer sur **Download Zip** pour enregistrer tous les fichiers sur votre ordinateur.  Une fois téléchargée, la solution contient généralement ces dossiers :
  
-   **Data**: contient les exemples de données pour chaque application.
  
-   **R**: contient tout le code de développement R dont vous avez besoin pour la solution. La solution nécessite les bibliothèques fournies par Microsoft R Server, mais peut être ouverte et modifiée dans n’importe quel IDE R. Le code R a été optimisé pour que les calculs soient effectués « dans la base de données », en affectant une instance de SQL Server comme contexte de calcul.
  
-   **SQLR**: contient plusieurs fichiers .sql que vous pouvez exécuter dans un environnement SQL comme [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour créer les procédures stockées qui effectuent des tâches associées, telles que le traitement des données, l’ingénierie des caractéristiques et le déploiement de modèle.
  
    Ce dossier contient également un script PowerShell que vous pouvez exécuter pour appeler tous les scripts et créer l’environnement de bout en bout. N’oubliez pas de modifier le script pour l’adapter à votre environnement.

## <a name="next-steps"></a>Étapes suivantes

+ [Didacticiels de SQL Server machine learning](machine-learning-services-tutorials.md)




