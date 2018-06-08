---
title: Déployer et de consommer analytique à l’aide de mrsdeploy | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 0f785b4aece13dd2df27988ef6a2e5c6934045fd
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="deploy-and-consume-analytics-using-mrsdeploy"></a>Déployer et de consommer analytique à l’aide de mrsdeploy
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Microsoft R Server inclut une fonctionnalité à l’Opérationnalisation, **mrsdeploy**, qui prend en charge ces tâches :

+ Publication et la gestion des modèles R et Python et le code sous la forme de services web
+ Utilisation de ces services dans les applications clientes

Cette rubrique fournit des informations sur la façon d’activer et configurer la fonctionnalité.

Pour plus d’informations sur les scénarios pris en charge par **mrsdeploy**, consultez [à l’Opérationnalisation avec R Server](https://docs.microsoft.com/r-server/what-is-operationalization).

## <a name="using-mrsdeploy-for-operationalization"></a>À l’aide de mrsdeploy pour une Opérationnalisation rapides

Le mot *à l’Opérationnalisation* peut signifier beaucoup de choses :

+ La possibilité de publier des modèles à un service web pour une utilisation par les applications
+ Prise en charge pour le calcul d’évolutif ou distribuée
+ Développer une seule fois, de déployer autant de fois
+ Score rapide, pour la ligne à la fois unique et le score de lot

Si vous avez installé la Machine Learning Services avec SQL Server, *à l’Opérationnalisation* est une question d’encapsuler votre code R ou Python dans une procédure stockée. Toute application peut alors appeler la procédure stockée pour former un modèle, de générer des scores ou de créer des rapports. Vous pouvez également automatiser des tâches à l’aide de mécanismes de planification existantes dans SQL Server.

Toutefois, Microsoft R Server fournit un mécanisme différent pour la prise en charge du déploiement, à l’aide des services web qui prennent en charge la publication de travaux R et d’un utilitaire d’administration pour l’exécution de travaux R distribués. Microsoft R Server utilise les fonctions dans le **mrsdeploy** package pour établir une session avec les nœuds de calcul à distance et d’exécuter du code R dans une application console.

Cette fonctionnalité de déploiement de R Server offre les avantages :

+ Contrôle d’accès basé sur le rôle aux services web analytique

    Déterminer qui peut publier, mettre à jour et supprimer leurs propres services web, les personnes qui peuvent également mettre à jour et supprimer les services web publiés par d’autres utilisateurs et qui peut seule liste et consomment des services web.

+ Calcul de score plus rapide
  
  Vous pouvez utiliser en temps réel avec un objet de modèle R pris en charge de calcul de score pour améliorer la vitesse des opérations de calcul de score.

+ Consommation de lot asynchrone

  Les services Web qui appellent pour les données d’entrée volumineuses maintenant peuvent être utilisées en mode asynchrone via l’exécution par lots.

+ Échelle d’une grille de nœuds de calcul et de web

  Un modèle de script est fourni pour vous permettre de lancer facilement un ensemble de machines virtuelles de serveur R dans Azure et les configurer comme une grille pour Opérationnalisation analytique et l’exécution à distance. Cette grille peut être mis à l’échelle ou vers le bas en fonction de l’utilisation du processeur.

+ Exécution à distance asynchrone

    Maintenant pris en charge à l’aide de la **mrsdeploy** package R. Pour continuer à travailler dans votre environnement de développement pendant l’exécution du script à distance, exécutez votre script R à l’aide de façon asynchrone le `async` paramètre. Cela est particulièrement utile lorsque vous exécutez des scripts qui ont des durées d’exécution longues.

## <a name="requirements-and-configuration"></a>Configuration requise et la configuration

SQL Server 2017 CTP 2.0 et versions ultérieur inclut cette fonctionnalité, qui était auparavant disponible uniquement avec les serveurs de R et pas installé avec SQL Server R Services. Le **mrsdeploy** package est installé sur l’ordinateur SQL Server, si vous sélectionnez l’option d’installation **Microsoft Machine Learning Server**, à partir de la **fonctionnalités partagées** section du programme d’installation de SQL Server.

En règle générale, nous déconseillons installer Machine Learning Server sur le même ordinateur qui exécute SQL Server Machine Learning Services. Nous vous recommandons d’installer **Microsoft Machine Learning Server** sur un ordinateur distinct à partir de SQL Server, puis configurez les fonctionnalités à l’Opérationnalisation sur cet ordinateur.

Toutefois, si vous avez besoin de les installer ensemble, suivez ces étapes supplémentaires pour configurer correctement le service.

1. Installer DotNetCore 1.1

    Si .NET Core n’est pas installé dans le cadre de SQL Server, vous devez l’installer avant de lancer le programme d’installation de R Server.

2. Installez le serveur d’apprentissage Microsoft.

3. Après avoir terminé l’installation de **Microsoft Machine Learning Server**manuellement ajouter la clé de Registre suivante pour **mrsdeploy**, qui spécifie le dossier de base pour les fichiers R_SERVER. 

    + Créer une nouvelle clé de Registre `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path`
    + Définir la valeur de la clé à `"C:\Program Files\Microsoft SQL Server\140\R_SERVER"`.

4. Lorsque vous avez terminé, ouvrez le [utilitaire Administrateur](https://docs.microsoft.com/r-server/operationalize/configure-use-admin-utility).

5. Continuer à configurer le **mrsdeploy** de service comme indiqué ici : [Configuration pour les administrateurs](https://docs.microsoft.com/r-server/operationalize/configure-start-for-administrators)

6. Pour plus d’informations, consultez [mrsdeploy fonctions](https://docs.microsoft.com/r-server/r-reference/mrsdeploy/mrsdeploy-package).
