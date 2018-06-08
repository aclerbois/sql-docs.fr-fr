---
title: 'Étape 4 : Déploiement du package de la Leçon 6 | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
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
ms.assetid: b613cef7-7993-4d89-a429-a8251d74d435
caps.latest.revision: 5
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 068b41cad8d65dd7fda2d1ad511113a9b1d1d868
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-6-4---deploying-the-lesson-6-package"></a>Leçon 6-4 : Déploiement du package de la Leçon 6
Pour déployer le package, vous devez l'ajouter au catalogue SSISDB dans Integration Services sur une instance de SQL Server. Dans cette leçon, vous allez ajouter le package de la leçon 6 au catalogue SSISDB, définir le paramètre, puis exécuter le package. Dans le cadre de cette leçon, vous utiliserez SQL Server Management Studio pour ajouter le package de la leçon 6 au catalogue SSISDB et déployer le package. Une fois le package déployé, vous modifierez le paramètre de façon à ce qu'il pointe vers un nouvel emplacement, puis vous exécuterez le package.  
  
Dans cette leçon, vous allez :  
  
-   ajouter le package au catalogue SSISDB dans le nœud SSIS dans SQL Server ;  
  
-   déployer le package ;  
  
-   définir la valeur de paramètre du package ;  
  
-   exécuter le package dans SSMS.  
  
### <a name="to-locate-or-add-the-ssisdb-catalog"></a>Pour rechercher ou ajouter le catalogue SSISDB  
  
1.  Cliquez sur Démarrer, pointez sur Tous les programmes, pointez sur Microsoft SQL Server 2012, puis cliquez sur SQL Management Studio.  
  
2.  Dans la boîte de dialogue Se connecter au serveur, vérifiez les paramètres par défaut, puis cliquez sur Se connecter. Pour vous connecter, la zone Nom du serveur doit contenir le nom de l'ordinateur sur lequel SQL Server est installé. Si le moteur de base de données est une instance nommée, la zone Nom du serveur doit également contenir le nom de l’instance au format <nom_ordinateur>\\<nom_instance>.  
  
3.  Dans l'Explorateur d'objets, développez Catalogues Integration Services.  
  
4.  Si aucun catalogue ne figure sous Catalogues Integration Services, ajoutez le catalogue SSISDB.  
  
5.  Pour ajouter le catalogue SSISDB, cliquez avec le bouton droit sur Catalogues Integration Services, puis cliquez sur Créer un catalogue.  
  
6.  Dans la boîte de dialogue Créer un catalogue, sélectionnez Activer l'intégration du CLR.  
  
7.  Dans la zone Mot de passe, tapez un nouveau mot de passe, puis entrez-le une nouvelle fois dans la zone Retapez le mot de passe. Veillez à mémoriser le mot de passe que vous tapez.  
  
8.  Cliquez sur OK pour ajouter le catalogue SSISDB.  
  
### <a name="to-add-the-package-to-the-ssisdb-catalog"></a>Pour ajouter le package au catalogue SSISDB  
  
1.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur SSISDB, puis cliquez sur Créer un dossier.  
  
2.  Dans la boîte de dialogue Créer un dossier, tapez SSIS Tutorial dans la zone Nom du dossier, puis cliquez sur OK.  
  
3.  Développez le dossier SSIS Tutorial, cliquez avec le bouton droit sur Projets, puis cliquez sur Importer un package.  
  
4.  Dans la page Introduction de l'Assistant Conversion de projet Integration Services, cliquez sur Suivant.  
  
5.  Dans la page Localiser les packages, vérifiez que l'option Système de fichiers est sélectionnée dans la liste Source, puis cliquez sur Parcourir.  
  
6.  Dans la boîte de dialogue Rechercher un dossier, naviguez jusqu'au dossier contenant le projet SSIS Tutorial, puis cliquez sur OK.  
  
7.  Cliquez sur Suivant.  
  
8.  Dans la page Sélectionner les packages, les six packages SSIS Tutorial doivent apparaître. Dans la liste Packages, sélectionnez Lesson 6.dtsx, puis cliquez sur Suivant.  
  
9. Dans la page Sélectionner la destination, tapez SSIS Tutorial Deployment dans la zone Nom du projet, puis cliquez sur Suivant.  
  
10. Cliquez sur Suivant dans chacune des pages restantes de l'Assistant jusqu'à ce que vous arriviez à la page Vérifier.  
  
11. Dans la page Vérifier, cliquez sur Convertir.  
  
12. Une fois la conversion terminée, cliquez sur Fermer.  
  
Quand vous fermez l'Assistant Conversion de projet Integration Services, SSIS affiche l'Assistant Déploiement d'Integration Services. Vous allez à présent utiliser cet Assistant pour déployer le package de la leçon 6.  
  
1.  Dans la page Introduction de l'Assistant Déploiement d'Integration Services, passez en revue les étapes à suivre pour déployer le projet, puis cliquez sur Suivant.  
  
2.  Dans la page Sélectionner la destination, vérifiez que le nom du serveur est l'instance de SQL Server contenant le catalogue SSISDB et que le chemin d'accès indique SSIS Tutorial Deployment, puis cliquez sur Suivant.  
  
3.  Dans la page Vérifier, passez en revue le résumé, puis cliquez sur Déployer.  
  
4.  Une fois le déploiement terminé, cliquez sur Fermer.  
  
5.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur Catalogues Integration Services, puis cliquez sur Actualiser.  
  
6.  Développez Catalogues Integration Services, puis SSISDB. Continuez à développer l'arborescence sous SSIS Tutorial jusqu'à ce que le projet soit entièrement développé. Le package Lesson 6.dtsx doit apparaître sous le nœud Packages du nœud SSIS Tutorial Deployment.  
  
Pour vérifier que le package est complet, cliquez avec le bouton droit sur Lesson 6.dtsx, puis cliquez sur Configurer. Dans la boîte de dialogue Configurer, sélectionnez Paramètres, puis vérifiez la présence des entrées suivantes : Lesson 6.dtsx en tant que conteneur, VarFolderName en tant que nom et le chemin d'accès à New Sample Data en tant que valeur. Ensuite, cliquez sur Fermer.  
  
Avant de continuer, créez un dossier d'exemple de données, nommez-le Sample Data Two, puis copiez dans celui-ci trois des exemples de fichiers d'origine.  
  
### <a name="to-create-and-populate-a-new-sample-data-folder"></a>Pour créer et remplir un nouveau dossier de données exemple  
  
1.  Dans l’Explorateur Windows, au niveau racine de votre lecteur (par exemple, C:\\), créez un dossier appelé Sample Data Two.  
  
2.  Ouvrez le dossier C:\Program Files\Microsoft SQL Server\110\Samples\Integration Services\Tutorial\Creating a Simple ETL Package\Sample Data, puis copiez trois des exemples de fichiers à partir du dossier.  
  
3.  Dans le dossier New Sample Data, collez les fichiers que vous venez de copier.  
  
### <a name="to-change-the-package-parameter-to-point-to-the-new-sample-data"></a>Pour faire pointer le paramètre du package vers le nouvel exemple de données  
  
1.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur Lesson 6.dtsx, puis cliquez sur Configurer.  
  
2.  Dans la boîte de dialogue Configurer, remplacez la valeur du paramètre par le chemin d'accès à Sample Data Two. Par exemple, C:\Sample Data Two, si vous avez placé le nouveau dossier dans le dossier racine du lecteur C.  
  
3.  Cliquez sur OK pour fermer la boîte de dialogue Configurer.  
  
### <a name="to-test-the-lesson-6-package-deployment"></a>Pour tester le déploiement du package de la leçon 6  
  
1.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur Lesson 6.dtsx, puis cliquez sur Exécuter.  
  
2.  Dans la boîte de dialogue Exécuter le package, cliquez sur OK.  
  
3.  Dans la zone de message, cliquez sur Oui pour ouvrir le rapport Vue d'ensemble.  
  
Le rapport Vue d'ensemble du package s'affiche avec le nom du package et une synthèse d'état. La section Vue d'ensemble de l'exécution indique le résultat de chaque tâche dans le package. Quant à la section Paramètres utilisés, elle indique les noms et les valeurs de tous les paramètres utilisés dans l'exécution du package, y compris VarFolderName.  
  
  
  
