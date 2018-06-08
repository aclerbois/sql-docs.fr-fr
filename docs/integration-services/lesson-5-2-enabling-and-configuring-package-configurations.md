---
title: 'Étape 2 : Activation et configuration des configurations de package | Microsoft Docs'
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
ms.assetid: 005218ab-8dd5-48e9-a185-6bc60cd43a7a
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1e70fa8824db8bebf314d3b8d0280ec3d321df6a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-5-2---enabling-and-configuring-package-configurations"></a>Leçon 5-2 : Activation et configuration des configurations de package
Au cours de cette tâche, vous allez convertir le projet en modèle de déploiement de package et activer les configurations du package à l'aide de l'Assistant Configuration de package. Vous allez utiliser cet Assistant pour générer un fichier de configuration XML qui contient les paramètres de configuration de la propriété **Directory** du conteneur de boucles Foreach. La valeur de la propriété Directory est fournie par la nouvelle variable de niveau package que vous pouvez mettre à jour au moment de l'exécution. De plus, vous allez remplir un nouveau dossier de données exemple à utiliser au cours du test.  
  
### <a name="to-create-a-new-package-level-variable-mapped-to-the-directory-property"></a>Pour créer une nouvelle variable de niveau package mappée à la propriété Directory  
  
1.  Cliquez sur l’arrière-plan de l’onglet **Flux de contrôle** dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] . Cette opération permet de définir l'étendue de la variable que vous allez créer pour le package.  
  
2.  Dans le menu [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez sur **Variables**.  
  
3.  Dans la fenêtre **Variables** , cliquez sur l’icône Ajouter une variable.  
  
4.  Dans la zone **Nom** , tapez **varFolderName**.  
  
    > [!IMPORTANT]  
    > Les noms des variables tiennent compte de la casse.  
  
5.  Vérifiez que la zone **Étendue** affiche le nom du package de la leçon 5 (Lesson 5).  
  
6.  Définissez la valeur de la zone **Type de données** de la variable `varFolderName` à **String**.  
  
7.  Réaffichez l’onglet **Flux de contrôle** et double-cliquez sur le conteneur **Foreach File in Folder** .  
  
8.  Dans la page **Collection** de **l’Éditeur de boucle Foreach**, cliquez sur **Expressions**, puis sur le bouton **(…)**.  
  
9. Dans **l’Éditeur d’expressions de la propriété**, cliquez dans la liste **Propriété** , puis sélectionnez **Directory**.  
  
10. Dans la zone **Expression** , cliquez sur le bouton **(…)**.  
  
11. Dans le **Générateur d’expressions**, développez le dossier Variables et faites glisser la variable **User::varFolderName** vers la zone **Expression** .  
  
12. Cliquez sur **OK** pour quitter le **Générateur d’expressions**.  
  
13. Cliquez sur **OK** pour quitter **l’Éditeur d’expressions de la propriété**.  
  
14. Cliquez sur **OK** pour fermer la boîte de dialogue **Éditeur de boucle Foreach**.  
  
### <a name="to-enable-package-configurations"></a>Pour activer les configurations de package  
  
1.  Dans le menu **Projet**, cliquez sur **Convertir en modèle de déploiement de package**.  
  
2.  Cliquez sur **OK** dans la boîte de dialogue d’avertissement et, une fois la conversion terminée, cliquez sur **OK** dans la boîte de dialogue **Convertir en modèle de déploiement de package** .  
  
3.  Cliquez sur l’arrière-plan de l’onglet **Flux de contrôle** dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)].  
  
4.  Dans le menu **SSIS** , cliquez sur **Configurations du package**.  
  
5.  Dans la boîte de dialogue **Bibliothèque des configurations du package** , sélectionnez **Activer les configurations du package**et cliquez sur **Ajouter**.  
  
6.  Dans la page Assistant Configuration de package, cliquez sur **Suivant**.  
  
7.  Dans la page **Sélectionner le type de configuration** , vérifiez que **Type de configuration** a la valeur **Fichier de configuration XML**.  
  
8.  Dans la page **Sélectionner le type de configuration** , cliquez sur **Parcourir**.  
  
9. Par défaut, quand la boîte de dialogue **Sélectionner l’emplacement du fichier de configuration** s’ouvre, elle contient le dossier du projet.  
  
10. Dans la boîte de dialogue **Sélectionner l’emplacement du fichier de configuration** , tapez **SSISTutorial** dans la zone **Nom de fichier**, puis cliquez sur **Enregistrer**.  
  
11. Dans la page **Sélectionner le type de configuration** , cliquez sur **Suivant**.  
  
12. Dans la page **Sélectionner les propriétés à exporter** , dans le volet **Objets** , développez **Variables**, **varFolderName**, **Propriétés**, puis sélectionnez **Valeur**.  
  
13. Dans la page **Sélectionner les propriétés à exporter** , cliquez sur **Suivant**.  
  
14. Dans la page **Fin de l’Assistant** , tapez un nom de configuration par exemple : **Configuration du répertoire du didacticiel SSIS**. Il s’agit du nom de configuration qui s’affiche dans la boîte de dialogue **Bibliothèque des configurations du package** .  
  
15. Cliquez sur **Terminer**.  
  
16. Cliquez sur **Fermer**.  
  
17. L’Assistant crée un fichier de configuration appelé SSISTutorial.dtsConfig, qui contient les paramètres de configuration pour la**valeur** de la variable, qui définit à son tour la propriété **Directory** de l’énumérateur.  
  
    > [!NOTE]  
    > Un fichier de configuration contient généralement des informations complexes sur les propriétés de package, mais dans le cadre de ce didacticiel, la seule information qui nous concerne est :  
    > <Configuration ConfiguredType="Property"  
    > Path="\Package.Variables[User::varFolderName].Properties[Value]" ValueType\="String">  
    >  <ConfiguredValue>\<\/ConfiguredValue>  
    > \<\/Configuration>.  
  
### <a name="to-create-and-populate-a-new-sample-data-folder"></a>Pour créer et remplir un nouveau dossier de données exemple  
  
1.  Dans l’Explorateur Windows, à la racine de votre lecteur (par exemple, C:\\), créez un dossier appelé **New Sample Data**.  
  
2.  Localisez les fichiers d'exemple sur votre ordinateur et copiez trois des fichiers du dossier.  
  
3.  Dans le dossier **New Sample Data** , collez les fichiers que vous venez de copier.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 3 : modification de la valeur de configuration de la propriété Directory](../integration-services/lesson-5-3-modifying-the-directory-property-configuration-value.md)  
  
