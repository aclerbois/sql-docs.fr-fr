---
title: 'Leçon 6 : Utilisation des paramètres avec le modèle de déploiement de projet dans SSIS | Microsoft Docs'
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
ms.assetid: 9216f18c-1762-4f2d-8c22-bd0ab7107555
caps.latest.revision: 5
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 84c96d55cc2aead955b637f5246ead7bff610c97
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-6-using-parameters-with-the-project-deployment-model-in-ssis"></a>Leçon 6 : Utilisation des paramètres avec le modèle de déploiement de projet dans SSIS
SQL Server 2012 introduit un nouveau modèle de déploiement qui permet de déployer vos projets sur le serveur Integration Services. Le serveur Integration Services vous permet de gérer et d'exécuter les packages, et de configurer leurs valeurs d'exécution.  
  
Dans cette leçon, vous allez modifier le package que vous avez créé dans [Leçon 5 : Ajouter des configurations de package SSIS pour le modèle de déploiement de package](../integration-services/lesson-5-add-ssis-package-configurations-for-the-package-deployment-model.md) pour utiliser le modèle de déploiement de package. Vous remplacerez la valeur de configuration par un paramètre pour spécifier l'emplacement des exemples de données. Vous pouvez également copier le package final de la leçon 5 inclus dans le didacticiel.  
  
À l'aide de l'Assistant Configuration de projet Integration Services, vous convertirez le projet en modèle de déploiement de projet, et vous utiliserez un paramètre au lieu d'une valeur de configuration pour définir la propriété Directory. Cette leçon couvre partiellement les étapes que vous suivriez pour convertir les packages SSIS existants en nouveau modèle de déploiement de projet.  
  
Quand vous réexécutez le package, le service Integration Services utilise le paramètre pour donner sa valeur à la variable, qui à son tour met à jour la propriété Répertoire. Ainsi, le package itère dans les fichiers du nouveau dossier de données spécifié par la valeur du paramètre, et non du dossier défini dans le fichier de configuration du package.  
  
> [!IMPORTANT]  
> Pour suivre ce didacticiel, vous devez disposer de l'exemple de base de données **AdventureWorksDW2012** . Pour plus d’informations sur l’installation et le déploiement de **AdventureWorksDW2012**, consultez [Considérations relatives à l’installation d’exemples de bases de données et d’exemples de code SQL Server](http://technet.microsoft.com/library/ms161556%28v=sql.105%29).  
  
## <a name="lesson-tasks"></a>Tâches de la leçon  
Cette leçon contient les tâches suivantes :  
  
1.  [Étape 1 : copie du package de la leçon 5](../integration-services/lesson-6-1-copying-the-lesson-5-package.md)  
  
2.  [Étape 2 : Conversion du projet en modèle de déploiement de projet](../integration-services/lesson-6-2-converting-the-project-to-the-project-deployment-model.md)  
  
3.  [Étape 3 : Test du package de la leçon 6](../integration-services/lesson-6-3-testing-the-lesson-6-package.md)  
  
4.  [Étape 4 : Déploiement du package de la leçon 6](../integration-services/lesson-6-4-deploying-the-lesson-6-package.md)  
  
## <a name="start-the-lesson"></a>Démarrer la leçon  
[Étape 1 : copie du package de la leçon 5](../integration-services/lesson-6-1-copying-the-lesson-5-package.md)  
  
