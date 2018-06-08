---
title: 'Étape 3 : Ajout de packages et autres fichiers | Microsoft Docs'
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
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: a7e6ec9c-d31d-4613-9525-8947a7b358f7
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 910d7759a7298e7285eb90c6d9e0d165245ec799
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-3---adding-packages-and-other-files"></a>Leçon 1-3 : Ajout de packages et autres fichiers
Au cours de cette tâche, vous allez ajouter des packages existants, des fichiers annexes qui prennent en charge des packages individuels et un fichier Lisezmoi pour le projet Didacticiel de déploiement que vous avez créé dans la tâche précédente. Par exemple, vous allez ajouter un fichier de données XML qui contient les données destinées à un package et un fichier texte qui fournit des informations de fichier Lisez-moi relatives à tous les packages du projet.  
  
Lorsque vous déployez des packages dans un environnement de test ou de production, vous n'incluez généralement pas les fichiers de données dans le déploiement, mais vous utilisez à la place des configurations pour mettre à jour les chemins d'accès des sources de données afin d'accéder aux versions de test ou de production des fichiers de données ou de base de données. À des fins pédagogiques, ce didacticiel inclut des fichiers de données dans le déploiement de package.  
  
Les packages et les exemples de données utilisés par les packages sont installés lors de l'installation des exemples [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Vous allez ajouter les packages suivants au projet Didacticiel de déploiement :  
  
-   **DataTransfer** Package de base qui extrait des données d'un fichier plat, évalue les valeurs des colonnes pour conserver de manière conditionnelle les lignes dans le dataset et charge les données dans une table de la base de données AdventureWorks.  
  
-   **LoadXMLData.** Package de transfert de données qui extrait les données d'un fichier de données XML, évalue et agrège les valeurs des colonnes, puis charge des données dans une table de la base de données AdventureWorks.  
  
Pour prendre en charge le déploiement de ces packages, vous allez ajouter les fichiers annexes suivants au projet Didacticiel de déploiement.  
  
|Package|Fichier|  
|-----------|--------|  
|DataTransfer|NewCustomers.txt|  
|LoadXMLData|orders.xml et orders.xsd|  
  
Vous allez ajouter aussi un fichier Lisez-moi qui est un fichier texte contenant des informations relatives au projet Didacticiel de déploiement.  
  
Les chemins d'accès utilisés dans les procédures suivantes supposent que les exemples [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sont installés dans l'emplacement par défaut, [!INCLUDE[ssSampPathIS](../includes/sssamppathis-md.md)]. Si vous avez installé les exemples dans un autre emplacement, utilisez plutôt cet emplacement dans les procédures.  
  
Au cours de la tâche suivante, vous allez ajouter des configurations aux packages DataTransfer et LoadXMLData. Toutes les configurations sont stockées dans des fichiers XML et vous allez utiliser une variable d'environnement système pour spécifier l'emplacement des fichiers. Après avoir créé les fichiers de configuration, vous allez les ajouter au projet.  
  
### <a name="to-add-packages-to-the-deployment-tutorial-project"></a>Pour ajouter des packages au projet Didacticiel de déploiement  
  
1.  Si [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] n’est pas déjà ouvert, dans le menu **Démarrer**, pointez sur **Tous les programmes**, puis sur **Microsoft SQL Server**et cliquez sur **SQL Server Data Tools**.  
  
2.  Dans le menu **Fichier** , cliquez sur **Ouvrir**, sur **Projet/Solution**, sur le dossier **Didacticiel de déploiement** et sur **Ouvrir**, puis double-cliquez sur **Deployment Tutorial.sln**.  
  
3.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur Didacticiel de déploiement, cliquez sur **Ajouter**, puis sur **Package existant**.  
  
4.  Dans la boîte de dialogue **Ajouter une copie des packages existants** , sous **Emplacement du package**, sélectionnez **Système de fichiers**.  
  
5.  Cliquez sur le bouton Parcourir **(…)** , accédez à C:\Program Files\Microsoft SQL Server\100\Samples\Integration ServicesTutorial\Deploying Packages\Completed Packages, sélectionnez **DataTransfer.dtsx**, puis cliquez sur **Ouvrir**.  
  
6.  Cliquez sur **OK**.  
  
7.  Répétez les étapes 3 à 6 et ajoutez cette fois LoadXMLData.dtsx qui se trouve dans C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages.  
  
### <a name="to-add-ancillary-files-to-the-deployment-tutorial-project"></a>Pour ajouter des fichiers annexes au projet Didacticiel de déploiement  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur Didacticiel de déploiement, cliquez sur **Ajouter**, puis sur **Élément existant**.  
  
2.  Dans la boîte de dialogue **Ajouter un élément existant - Didacticiel de déploiement** , accédez à C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deployment Packages\Sample Data, sélectionnez orders.xml, orders.xsd et NewCustomers.txt, puis cliquez sur **Ajouter**.  
  
3.  Dans la boîte de dialogue **Ajouter un élément existant - Didacticiel de déploiement** , accédez à C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deployment Packages\\, sélectionnez Readme.txt et cliquez sur **Ajouter**.  
  
4.  Dans le menu Fichier, cliquez sur **Enregistrer tout**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 4 : Ajout de configurations au package](../integration-services/lesson-1-4-adding-package-configurations.md)  
  
  
  
