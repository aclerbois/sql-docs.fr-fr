---
title: Analyser R Services à l’aide de rapports personnalisés dans Management Studio | Microsoft Docs
ms.custom: ''
ms.date: 10/09/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: machine-learning-services
ms.prod_service: machine-learning-services
ms.component: r
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.author: heidist
author: HeidiSteen
manager: cgronlun
ms.workload: Inactive
ms.openlocfilehash: 3c8bbd59e8a5879320b75e28f1e0f7bdd5da62bc
ms.sourcegitcommit: 059fc64ba858ea2adaad2db39f306a8bff9649c2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="monitor-machine-learning-services-using-custom-reports-in-management-studio"></a>Surveiller les Services d’apprentissage Machine à l’aide des rapports personnalisés dans Management Studio
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Pour le rendre plus facile à gérer l’instance utilisée pour l’apprentissage, l’équipe de produit a fourni un nombre d’exemples de rapports personnalisés que vous pouvez ajouter à SQL Server Management Studio. Dans ces rapports, vous pouvez afficher des détails tels que :

- Sessions R Active ou Python
- Paramètres de configuration de l’instance
- Statistiques d’exécution des tâches d’apprentissage machine
- Événements étendus pour R Services
- Packages R ou Python installés sur l’instance actuelle

Cet article explique comment installer et utiliser les rapports personnalisés fournies spécifiquement pour leaerning de l’ordinateur. 

Pour obtenir une présentation générale des rapports dans Management Studio, consultez [rapports personnalisés dans Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>Comment installer les rapports

Les rapports sont conçus à l’aide de SQL Server Reporting Services, mais il peuvent être utilisés directement à partir de SQL Server Management Studio, même si Reporting Services n’est pas installé sur votre instance. 

Pour utiliser ces rapports :

* Téléchargez les fichiers RDL à partir du dépôt GitHub d’exemples des produits SQL Server.
* Ajoutez les fichiers au dossier des rapports personnalisés utilisé par SQL Server Management Studio.
* Ouvrez les rapports dans SQL Server Management Studio.


### <a name="step-1-download-the-reports"></a>Étape 1. Télécharger les rapports

1. Ouvrez le référentiel GitHub qui contient [exemples du produit SQL Server](https://github.com/Microsoft/sql-server-samples)et télécharger les exemples de rapports. 

    + [Rapports personnalisés de SSMS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports)

    > [!NOTE]
    > Les rapports peuvent être utilisés avec SQL Server 2017 MCM Learning Services ou SQL Server 2016 R Services.

2. Pour télécharger les exemples, vous pouvez également vous connecter à GitHub et effectuer un branchement local des exemples. 

### <a name="step-2-copy-the-reports-to-management-studio"></a>Étape 2. Copier les rapports dans Management Studio

3. Recherchez le dossier des rapports personnalisés utilisé par SQL Server Management Studio. Par défaut, les rapports personnalisés sont stockés dans ce dossier :
    
   `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

   Toutefois, vous pouvez spécifier un autre dossier, ou créer des sous-dossiers.

4. Copiez les fichiers *.RDL dans le dossier des rapports personnalisés.


### <a name="step-3-run-the-reports"></a>Étape 3. Exécuter les rapports

5. Dans Management Studio, cliquez avec le bouton droit sur le nœud **Bases de données** de l’instance où vous souhaitez exécuter les rapports.
6. Cliquez sur **Rapports**, puis sur **Rapports personnalisés**.
7. Dans la boîte de dialogue **Ouvrir un fichier** , recherchez le dossier des rapports personnalisés.
8. Sélectionnez l’un des fichiers RDL que vous avez téléchargés, puis cliquez sur **Ouvrir**.

> [!IMPORTANT]
> Sur certains ordinateurs, tels que ceux avec des périphériques d’affichage en haute résolution ou avec une résolution supérieure à 1080p, ou dans certaines sessions Bureau à distance, ces rapports ne sont pas utilisables. Il existe un bogue dans le contrôle Visionneuse de rapports dans SSMS qui bloque le rapport.

## <a name="report-list"></a>Liste des rapports

Actuellement, le référentiel d’exemples de produit dans GitHub inclut les rapports suivants :

+ **R Services - Sessions actives**

  Utilisez ce rapport pour afficher les utilisateurs actuellement connectés à l’instance de SQL Server et en cours d’exécution des tâches d’apprentissage. 
  
+ **R Services - Configuration**

  Utilisez ce rapport pour afficher la configuration de l’exécution du script externe et les services associés. Le rapport indique si un redémarrage est nécessaire et recherche les protocoles réseau requis. 
  
  L’authentification implicite est requise pour les tâches d’apprentissage automatique qui s’exécutent dans SQL Server en tant qu’un contexte de calcul. Pour vérifier que l’authentification implicite est configurée, l’état vérifie si une connexion de base de données existe pour le groupe SQLRUserGroup.

 + **R Services - Configurer l’instance** 

   Ce rapport est destiné à vous aider à configurer d’apprentissage. Vous pouvez également exécuter ce rapport pour corriger les erreurs de configuration trouvés dans l’état précédent.
 
+ **R Services - Statistiques d’exécution**

  Utilisez ce rapport pour afficher les statistiques d’exécution des tâches d’apprentissage machine. Par exemple, vous pouvez obtenir le nombre total de scripts R qui ont été exécutés, le nombre d’exécutions en parallèle et les fonctions RevoScaleR le plus fréquemment utilisées. Cliquez sur **afficher le Script SQL** pour obtenir le code T-SQL complet derrière ce rapport.

  Actuellement, le rapport analyse uniquement les statistiques pour les fonctions de package RevoScaleR.

+ **R Services - Événements étendus**

  Utilisez ce rapport pour afficher la liste des événements étendus qui sont disponibles pour l’analyse des tâches liées aux runtimes de script externe. Cliquez sur **afficher le Script SQL** pour obtenir le code T-SQL complet derrière ce rapport.

+ **R Services - Packages**

  Utilisez ce rapport pour afficher la liste des packages R ou Python installés sur l’instance de SQL Server.

+ **R Services - Utilisation des ressources**

  Ce rapport permet d’afficher la consommation des ressources du processeur, la mémoire et d’e/s par l’exécution du script externe. Vous pouvez également afficher le paramètre de mémoire de pools de ressources externes.

## <a name="see-also"></a>Voir aussi

[Analyse des services](managing-and-monitoring-r-solutions.md)

[Événements étendus pour R Services](extended-events-for-sql-server-r-services.md)
