---
title: Didacticiels de SQL Server Python | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: c99e5605dad537fddef20fbd091a61cc4e711471
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="sql-server-python-tutorials"></a>Didacticiels de SQL Server Python
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit une liste des didacticiels et des exemples qui illustrent l’utilisation de Python avec SQL Server 2017. Grâce à ces exemples et les démonstrations, vous allez apprendre :

+ L’exécution de Python à partir de T-SQL
+ Quelles sont les contextes de calcul locaux et distants, et comment vous pouvez exécuter le code Python à l’aide de l’ordinateur SQL Server
+ Comment faire pour encapsuler le code Python dans une procédure stockée
+ Optimisation du code Python pour un environnement de production SQL
+ Des scénarios concrets pour l’incorporation d’apprentissage dans les applications

Pour plus d’informations sur le programme d’installation, consultez [conditions préalables](#bkmk_Prerequisites).

## <a name="bkmk_pythontutorials"></a>Didacticiels de Python

+ [En cours d’exécution de Python dans T-SQL](run-python-using-t-sql.md)

   Découvrez les principes fondamentaux de l’appel de Python dans T-SQL, en utilisant le mécanisme d’extensibilité fait son apparition dans SQL Server 2016.

+ [Créer un modèle dans Python à l’aide de revoscalepy d’apprentissage](use-python-revoscalepy-to-create-model.md)

   Cette leçon illustre l’exécution de code à partir d’un terminal de Python à distance, à l’aide du contexte de calcul de SQL Server. Vous devez être familiarisé avec les environnements et les outils Python. Exemple de code est fourni qui crée un modèle à l’aide de **rxLinMod**, à partir du nouveau **revoscalepy** bibliothèque. 

+ [Analytique Python de la base de données pour les développeurs SQL](sqldev-in-database-python-for-sql-developers.md)

    Cette procédure pas à pas de bout en bout illustre le processus de création d’une solution complète de Python à l’aide de procédures stockées T-SQL. Tout le code Python est inclus.


## <a name="python-samples"></a>Exemples de Python

Ces exemples et les démonstrations fournies par l’équipe de développement SQL Server mettez en surbrillance les façons dont vous pouvez utiliser analytique incorporées dans les applications réelles.

+ [Créer un modèle prédictif à l’aide de Python et SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)

  Découvrez comment une entreprise de location ski peut utiliser d’apprentissage prévoir les futures Locations, ce qui vous permet du plan d’entreprise et le personnel pour répondre à la demande.

  > [!TIP]
  > Inclut désormais un score natif à partir de modèles de Python !

+ [Exécuter le client clusters à l’aide de Python et SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/customerclustering/)

    Découvrez comment utiliser l’algorithme de Kmeans pour effectuer de clustering non supervisé de clients.

## <a name="bkmk_Prerequisites"></a>Conditions préalables

Pour utiliser ces didacticiels, vous devez disposer de SQL Server 2017, et vous devez installer explicitement et puis activez la fonctionnalité, Machine Learning Services (de-de base de données). 

SQL Server 2017 prend en charge les langues de R et de Python, mais aucune n’est installé ou activé par défaut. L’exécution de Python requiert que l’infrastructure d’extensibilité soit activé et que vous sélectionnez Python comme la langue à installer. 

### <a name="post-installation-configuration-tips"></a>Conseils de configuration après installation

Après avoir exécuté le programme d’installation de SQL Server, vous devrez peut-être effectuer quelques étapes supplémentaires pour vous assurer que SQL Server et les Python communiquent :

+ Activez la fonctionnalité d’exécution de script externe en exécutant `sp_configure 'external scripts enabled', 1`.
+ Redémarrez le serveur. 
+ Ouvrez le **Services** Panneau de configuration pour vérifier si le Launchpad a démarré. 
+ Assurez-vous que le service qui appelle le runtime externe dispose des autorisations nécessaires. Pour plus d’informations, consultez [activer l’authentification implicite](../r/add-sqlrusergroup-to-database.md).
+ Ouvrir un port sur le pare-feu pour SQL Server et activer les protocoles réseau requis.
+ Assurez-vous que votre compte de connexion SQL ou un compte d’utilisateur Windows dispose des autorisations nécessaires pour se connecter au serveur, pour lire des données et à créer des objets de base de données requis par l’exemple.

Consultez cet article pour certains problèmes courants : [dépannage Machine Learning Services](../machine-learning-troubleshooting-faq.md)

### <a name="resource-management"></a>Gestion des ressources

Vous pouvez installer R et Python sur le même ordinateur, mais les deux en cours d’exécution peut nécessiter des ressources importantes. Si vous obtenez des erreurs « mémoire insuffisante », ou si les tâches d’apprentissage machine en cours d’exécution est que le principal de destination du serveur, vous pouvez réduire la quantité de mémoire allouée au moteur de base de données. Pour plus d’informations, consultez [Managing and monitoring Python dans SQL Server](../python/managing-and-monitoring-python-solutions.md).

## <a name="see-also"></a>Voir aussi

[Didacticiels R pour SQL Server](sql-server-r-tutorials.md)
