---
title: Profilage des données et visionneuse, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling task [Integration Services], about data profiling
- data profiling
- profiling data
ms.assetid: 756840e3-aa09-45cd-9951-1a17af4b5925
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 64a073fde50047ebb20b13b56818a9c910d1642e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-profiling-task-and-viewer"></a>Tâche de profilage des données et visionneuse
  La tâche de profilage des données fournit des fonctionnalités de profilage de données à l'intérieur du processus d'extraction, de transformation et de chargement de données. Grâce à la tâche de profilage des données, vous pouvez bénéficier des avantages suivants :  
  
-   Analyser les données sources plus efficacement  
  
-   Mieux comprendre les données sources  
  
-   Empêcher les problèmes de qualité des données avant qu'ils ne soient introduits dans l'entrepôt de données  
  
> [!IMPORTANT]  
>  La tâche de profilage des données fonctionne uniquement avec les données stockées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle ne fonctionne pas avec les sources de données tierces ou basées sur des fichiers.  
  
## <a name="data-profiling-overview"></a>Présentation du profilage des données  
 La qualité des données est cruciale dans toute entreprise. Compte tenu du fait que les systèmes analytiques et décisionnels des entreprises sont fondés sur leurs systèmes transactionnels, la fiabilité des indicateurs de performance clés et des prédictions d'exploration de données dépend entièrement de la validité des données sur lesquelles ils sont basés. Parallèlement à l'importance croissante des données valides dans la prise de décision en entreprise, le processus de validation de ces données est de plus en plus complexe. Les données affluent constamment dans l'entreprise, en provenance de systèmes et de sources variés et d'un grand nombre d'utilisateurs.  
  
 Les mesures de la qualité des données peuvent être difficiles à mettre en place car elles sont spécifiques au domaine ou à l'application. Une approche commune à la définition de la qualité des données est le profilage des données.  
  
 Un profil de données est une collection de statistiques agrégées sur les données qui peut regrouper, par exemple :  
  
-   le nombre de lignes dans la table Customer ;  
  
-   le nombre de valeurs distinctes dans la colonne State ;  
  
-   le nombre de valeurs Null ou manquantes dans la colonne Zip ;  
  
-   la distribution des valeurs dans la colonne City ;  
  
-   la puissance de la dépendance fonctionnelle de la colonne State sur la colonne Zip (en d'autres termes, un État américain doit toujours être le même pour une valeur de code postal donnée).  
  
 Les statistiques fournies par un profil de données vous donnent les informations nécessaires pour minimiser de manière efficace les problèmes de qualité qui peuvent résulter de l'utilisation des données sources.  
  
## <a name="integration-services-and-data-profiling"></a>Integration Services et profilage des données  
 Dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], le processus de profilage des données comprend les étapes suivantes :  
  
 **Étape 1 : Configuration de la tâche de profilage des données**  
 La tâche de profilage des données vous permet de configurer les profils à calculer. Vous exécutez ensuite le package qui contient la tâche de profilage des données pour calculer les profils. La tâche enregistre la sortie du profil au format XML dans un fichier ou une variable de package.  
  
 **Pour plus d’informations :** [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)  
  
 **Étape 2 : Vérification des profils calculés par la tâche de profilage des données**  
 Pour examiner les profils de données calculés par la tâche de profilage des données, vous envoyez la sortie à un fichier, puis vous utilisez la visionneuse du profil des données. Cette visionneuse est un utilitaire autonome qui affiche la sortie du profil, sous forme d'informations résumées et détaillées, avec en option une fonction d'exploration vers le bas.  
  
 **Pour plus d’informations :** [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md)  
  
### <a name="addition-of-conditional-logic-to-the-data-profiling-workflow"></a>Ajout de la logique conditionnelle au flux de travail de profilage des données  
 La tâche de profilage des données n'inclut pas de fonctionnalités intégrées vous permettant d'utiliser la logique conditionnelle pour connecter cette tâche aux tâches en aval basées sur la sortie du profil. Toutefois, vous pouvez ajouter facilement cette logique, avec un minimum de programmation, dans une tâche de script. Par exemple, vous pouvez définir une tâche de script qui effectue une requête XPath sur le fichier de sortie de la tâche de profilage des données. La requête peut déterminer si le pourcentage de valeurs NULL dans une colonne particulière dépasse un certain seuil. Si tel est le cas, vous pouvez interrompre le package et résoudre le problème dans les données sources avant de continuer. Pour plus d’informations, consultez [Incorporer une tâche de profilage des données dans le flux de travail du package](../../integration-services/control-flow/incorporate-a-data-profiling-task-in-package-workflow.md).  
  
## <a name="related-content"></a>Contenu associé  
 [Schéma du profileur de données](http://go.microsoft.com/fwlink/?LinkId=251524)  
  
  
