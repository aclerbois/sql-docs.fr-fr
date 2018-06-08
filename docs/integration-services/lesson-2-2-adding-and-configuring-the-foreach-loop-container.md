---
title: 'Étape 2 : Ajout et configuration du conteneur de boucles Foreach | Microsoft Docs'
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
ms.assetid: 88a973cc-0f23-4ecf-adb6-5b06279c2df6
caps.latest.revision: 36
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7b1bff8ae63008a86637927f1547a9d9a8c1f42b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-2-2---adding-and-configuring-the-foreach-loop-container"></a>Leçon 2-2 : Ajout et configuration du conteneur de boucles Foreach
Dans cette tâche, vous allez activer la fonction qui permet d'effectuer des boucles dans un dossier de fichiers plats et d'appliquer la transformation de flux de données utilisée dans la leçon 1 à chacun de ces fichiers plats. Pour activer cette fonction, vous allez ajouter et configurer un conteneur de boucles Foreach dans le flux de contrôle.  
  
Le conteneur de boucles Foreach que vous allez ajouter doit pouvoir se connecter à chaque fichier plat dans le dossier. Étant donné que tous les fichiers du dossier ont le même format, le conteneur de boucles Foreach peut utiliser le même Gestionnaire de connexions de fichiers plats pour se connecter à chacun de ces fichiers. Le Gestionnaire de connexions de fichiers plats que le conteneur va utiliser est le même que celui créé au cours de la leçon 1.  
  
Pour l'instant, le Gestionnaire de connexions de fichiers plats créé au cours de la leçon 1 se connecte uniquement à un seul fichier plat spécifique. Pour que la connexion réitère et s'établisse à chaque fichier plat dans le dossier, vous devez configurer le conteneur de boucles Foreach et le Gestionnaire de connexions de fichiers plats comme suit :  
  
-   **Conteneur de boucles Foreach :** vous allez mapper la valeur énumérée du conteneur à une variable de package définie par l’utilisateur. Le conteneur va ensuite utiliser cette variable définie par l’utilisateur pour modifier dynamiquement la propriété **ConnectionString** du Gestionnaire de connexions de fichiers plats et se connecter de façon itérative à chaque fichier plat du dossier.  
  
-   **Gestionnaire de connexions de fichiers plats :** vous allez modifier le Gestionnaire de connexions qui a été créé dans la leçon 1 en utilisant une variable définie par l’utilisateur pour remplir la propriété **ConnectionString** du Gestionnaire de connexions.  
  
Les procédures de cette tâche montrent comment créer et modifier le conteneur de boucles Foreach pour utiliser une variable de package définie par l'utilisateur et comment ajouter la tâche de flux de données à la boucle. Au cours de la tâche suivante, vous allez apprendre à modifier le Gestionnaire de connexions de fichiers plats pour qu'il utilise une variable définie par l'utilisateur.  
  
Une fois ces modifications apportées au package et le package exécuté, le conteneur de boucles Foreach effectue une itération dans la collection de fichiers dans le dossier Sample Data. Chaque fois qu’un fichier répondant aux critères est trouvé, le conteneur de boucles Foreach remplit la variable définie par l’utilisateur avec le nom du fichier, mappe cette variable à la propriété **ConnectionString** du Gestionnaire de connexions de fichiers plats Sample Currency Data, puis exécute le flux de données par rapport à ce fichier. Par conséquent, à chaque itération de la boucle Foreach, la tâche de flux de données utilise un fichier plat différent.  
  
> [!NOTE]  
> Étant donné que [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sépare le flux de contrôle du flux de données, les boucles que vous ajoutez au flux de contrôle ne nécessitent pas la modification du flux de données. Par conséquent, il n'est pas nécessaire de modifier le flux de données que vous avez créé au cours de la leçon 1.  
  
### <a name="to-add-a-foreach-loop-container"></a>Pour ajouter un conteneur de boucles Foreach  
  
1.  Dans **SQL Server Data Tools**, cliquez sur l’onglet **Flux de contrôle** .  
  
2.  Dans la **Boîte à outils SSIS**, développez **Conteneurs**, puis faites glisser un **conteneur de boucles Foreach** dans l’aire de conception de l’onglet **Flux de contrôle** .  
  
3.  Cliquez avec le bouton droit sur le **conteneur de boucles Foreach** que vous venez d’ajouter et sélectionnez **Modifier**.  
  
4.  Dans la boîte de dialogue **Éditeur de boucle Foreach** , dans la page **Général** , pour **Nom**, entrez **Foreach File in Folder**. Cliquez sur **OK**.  
  
5.  Cliquez avec le bouton droit sur le conteneur de boucles Foreach, cliquez sur **Propriétés**puis, dans la fenêtre Propriétés, vérifiez que la propriété **LocaleID** est définie sur **Anglais (États-Unis)**.  
  
### <a name="to-configure-the-enumerator-for-the-foreach-loop-container"></a>Pour configurer l'énumérateur pour le conteneur de boucles Foreach  
  
1.  Double-cliquez sur Foreach File in Folder pour rouvrir **l’Éditeur de boucle Foreach**.  
  
2.  Cliquez sur **Collection**.  
  
3.  Dans la page **Collection** , cliquez sur **Énumérateur Foreach File**.  
  
4.  Dans le groupe **Configuration de l’énumérateur** , cliquez sur **Parcourir**.  
  
5.  Dans la boîte de dialogue **Rechercher un dossier** , recherchez dans votre ordinateur le dossier qui contient les fichiers Currency_*.txt.  
  
    Ces exemples de données sont inclus dans les packages de leçons [!INCLUDE[ssIS](../includes/ssis-md.md)] . Pour télécharger ces exemples de données et les packages de leçons, procédez comme suit.  
  
    1.  Accédez à [Exemples de produits Integration Services](http://go.microsoft.com/fwlink/?LinkId=275027). 
  
    2.  Cliquez sur l'onglet **DOWNLOADS** (Téléchargements).  
  
    3.  Cliquez sur le lien vers le fichier [SQL2012.Integration_Services.Create_Simple_ETL_Tutorial.Sample.zip](http://msftisprodsamples.codeplex.com/downloads/get/596031).  
  
6.  Dans la zone **Fichiers**, tapez **Currency_\*.txt**.  
  
### <a name="to-map-the-enumerator-to-a-user-defined-variable"></a>Pour mapper l'énumérateur à une variable définie par l'utilisateur  
  
1.  Cliquez sur **Mappages de variables**.  
  
2.  Dans la page **Mappages de variables**, dans la colonne **Variable**, cliquez sur la cellule vide et sélectionnez **\<Nouvelle variable>**.  
  
3.  Dans la boîte de dialogue **Ajouter une variable** , pour **Nom**tapez **varFileName**.  
  
    > [!IMPORTANT]  
    > Les noms des variables tiennent compte de la casse.  
  
4.  Cliquez sur **OK**.  
  
5.  Cliquez à nouveau sur **OK** pour quitter la boîte de dialogue **Éditeur de boucle Foreach** .  
  
### <a name="to-add-the-data-flow-task-to-the-loop"></a>Pour ajouter la tâche de flux de données à la boucle  
  
-   Faites glisser la tâche de flux de données **Extract Sample Currency Data** vers le conteneur de boucles Foreach maintenant renommé **Foreach File in Folder**.  
  
## <a name="next-lesson-task"></a>Tâche suivante de la leçon  
[Étape 3 : Modification du gestionnaire de connexions de fichiers plats](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
## <a name="see-also"></a> Voir aussi  
[Configurer un conteneur de boucles Foreach](http://msdn.microsoft.com/library/519c6f96-5e1f-47d2-b96a-d49946948c25)  
[Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)  
  
  
  
