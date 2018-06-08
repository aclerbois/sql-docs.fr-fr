---
title: 'Leçon 2 : Ajout d’un bouclage avec SSIS | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 01f2ed61-1e5a-4ec6-b6a6-2bd070c64077
caps.latest.revision: 32
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a17dbabd2998f4578da9faca1d217fdee00b559d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-adding-looping-with-ssis"></a>Leçon 2 : Ajout d’un bouclage avec SSIS
Au cours de la [Leçon 1 : Créer un projet et un package de base avec SSIS](../integration-services/lesson-1-create-a-project-and-basic-package-with-ssis.md), vous avez créé un package qui a extrait des données d’une source de fichier plat unique, transformé les données au moyen de la fonction de transformation des recherches, puis chargé les données dans la table de faits **FactCurrency** de l’exemple de base de données **AdventureWorksDW2012** .  
  
Toutefois, il est rare qu'un processus d'extraction, de transformation et de chargement (ETL, extract, transform, and load) utilise un seul fichier plat. Un processus ETL classique extrait généralement les données de plusieurs sources de fichiers plats. L'extraction des données à partir de plusieurs sources nécessite un flux de contrôle répétitif. L'une des fonctions les plus appréciées de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] est la facilité avec laquelle vous pouvez ajouter une itération ou un bouclage aux packages.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] fournit deux types de conteneurs pour effectuer des boucles dans des packages : le conteneur de boucles Foreach et le conteneur de boucles For. Le conteneur de boucles Foreach utilise un énumérateur pour effectuer le bouclage, tandis que le conteneur de boucles For utilise généralement une expression variable. Cette leçon est basée sur le conteneur de boucles Foreach.  
  
Le conteneur de boucles Foreach permet à un package de répéter le flux de contrôle pour chaque membre d'un énumérateur spécifié. Avec le conteneur de boucles Foreach, vous pouvez énumérer :  
  
-   Lignes du recordset ADO  
  
-   Informations du schéma ADO.NET  
  
-   des structures de fichiers et de répertoires ;  
  
-   des variables système, package et utilisateur ;  
  
-   Objets énumérables contenus dans une variable  
  
-   Éléments d'une collection  
  
-   Nœuds dans une expression de langage XML Path (XPath)  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Objects (SMO)  
  
Au cours de cette leçon, vous allez modifier le package ETL simple que vous avez créé au cours de la leçon 1 pour tirer parti du conteneur de boucles Foreach. Vous allez également définir des variables de package définies par l'utilisateur pour faire en sorte que le package du didacticiel effectue une itération sur tous les fichiers plats du dossier. Si vous n'avez pas effectué la leçon précédente, vous pouvez également copier le package final de la leçon 1 inclus dans le didacticiel.  
  
Au cours de cette leçon, vous n'allez pas modifier le flux de données mais uniquement le flux de contrôle.  
  
> [!IMPORTANT]  
> Pour suivre ce didacticiel, vous devez disposer de l'exemple de base de données **AdventureWorksDW2012** . Pour plus d'informations sur l'installation et le déploiement d' **AdventureWorksDW2012**, consultez [Reporting Services Product Samples sur CodePlex](http://go.microsoft.com/fwlink/p/?LinkID=526910).  
  
## <a name="lesson-tasks"></a>Tâches de la leçon  
Cette leçon contient les tâches suivantes :  
  
-   [Étape 1 : Copie du package de la leçon 1](../integration-services/lesson-2-1-copying-the-lesson-1-package.md)  
  
-   [Étape 2 : ajout et configuration du conteneur de boucles Foreach](../integration-services/lesson-2-2-adding-and-configuring-the-foreach-loop-container.md)  
  
-   [Étape 3 : modification du gestionnaire de connexions de fichiers plats](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
-   [Étape 4 : test de la leçon 2 du Package du didacticiel](../integration-services/lesson-2-4-testing-the-lesson-2-tutorial-package.md)  
  
## <a name="start-the-lesson"></a>Démarrer la leçon  
[Étape 1 : Copie du package de la leçon 1](../integration-services/lesson-2-1-copying-the-lesson-1-package.md)  
  
## <a name="see-also"></a> Voir aussi  
[Conteneur de boucles For](../integration-services/control-flow/for-loop-container.md)  
  
  
  
