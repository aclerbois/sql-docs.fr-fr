---
Title: 'Tutorial: SQL Server Management Studio components and configuration'
description: Ce tutoriel décrit les composants et les options de configuration de base pour votre environnement SQL Server Management Studio.
keywords: SQL Server, SSMS, SQL Server Management Studio
author: MashaMSFT
ms.author: mathoma
ms.date: 03/16/2018
ms.topic: Tutorial
ms.suite: sql
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
manager: craigg
ms.openlocfilehash: 645f52265cbb8e80c7265bcae111300f03e0bc7a
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="tutorial-sql-server-management-studio-components-and-configuration"></a>Tutoriel : Composants et configuration de SQL Server Management Studio
Ce tutoriel décrit les différents composants de fenêtre dans SQL Server Management Studio (SSMS) et certaines options de configuration de base pour votre espace de travail. Dans cet article, vous apprenez à : 

> [!div class="checklist"]
> * Identifier les composants qui constituent l’environnement SSMS
> * Changer la disposition de l’environnement et rétablir la valeur par défaut
> * Optimiser l’éditeur de requête
> * Changer la police 
> * Configurer les options de démarrage 
> * Rétablir la configuration par défaut 

## <a name="prerequisites"></a>Conditions préalables requises
Pour effectuer ce tutoriel, vous avez besoin de SQL Server Management Studio.  

- Installez [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

## <a name="sql-server-management-studio-components"></a>Composants de SQL Server Management Studio
Cette section décrit les différents composants de fenêtre disponibles dans l’espace de travail et comment les utiliser. 

- Pour fermer une fenêtre, sélectionnez le **X** en haut à droite de la barre de titre. 
- Pour rouvrir une fenêtre, sélectionnez la fenêtre dans le menu **Affichage**. 

    ![Menu Affichage](media/ssms-configuration/viewmenu.png)

- **Explorateur d’objets** (F8) : l’Explorateur d’objets est une arborescence qui présente tous les objets de base de données sur un serveur. Cette vue inclut les bases de données du moteur de base de données SQL Server, SQL Server Analysis Services, SQL Server Reporting Services et SQL Server Integration Services. L’Explorateur d’objets contient des informations pour tous les serveurs qui y sont connectés. 
    
    ![Explorateur d’objets](media/ssms-configuration/objectexplorer.png)
- **Fenêtre de requête** (Ctrl + N) : Après avoir sélectionné **Nouvelle requête**, entrez vos requêtes Transact-SQL (T-SQL) dans cette fenêtre. Les résultats de vos requêtes s’affichent également ici.
    
    ![Fenêtre Nouvelle requête](media/ssms-configuration/newquery.png)

- **Propriétés** (F4) : Vous pouvez voir la vue Propriétés quand la fenêtre de requête est ouverte. La vue montre les propriétés de base de la requête. Par exemple, elle montre l’heure de début d’une requête, le nombre de lignes retournées et les détails de la connexion.  

    ![Propriétés](media/ssms-configuration/properties.png)

- **Explorateur de modèles** (Ctrl + Alt + T) : L’Explorateur de modèles a divers modèles T-SQL prédéfinis. Vous pouvez utiliser ces modèles pour effectuer diverses fonctions, comme créer ou sauvegarder une base de données. 

    ![Explorateur de modèles](media/ssms-configuration/templates.png)

- **Détails de l’Explorateur d’objets** (F7) : Cette vue est plus précise que la vue de l’Explorateur d’objets. Vous pouvez utiliser Détails de l’Explorateur d’objets pour manipuler plusieurs objets en même temps. Par exemple, dans cette fenêtre, vous pouvez sélectionner plusieurs bases de données, puis les supprimer ou les scripter simultanément. 

    ![Détails de l’Explorateur d’objets](media/ssms-configuration/objectexplorerdetails.PNG) 
 
    

## <a name="change-the-environment-layout"></a>Changer la disposition de l’environnement 
Cette section décrit comment changer la disposition de l’environnement, notamment comment déplacer plusieurs fenêtres. 

- Pour déplacer une fenêtre, appuyez de façon prolongée sur le titre, puis faites glisser la fenêtre. 
- Pour épingler ou détacher une fenêtre, sélectionnez l’icône de punaise dans la barre de titre :
    
    ![Épingler un objet](media/ssms-configuration/pushpin.png)

- Chaque composant de la fenêtre a un menu déroulant que vous pouvez utiliser pour manipuler la fenêtre de différentes manières : 

    ![Options de la fenêtre](media/ssms-configuration/windowoptions.png)

- Quand deux fenêtres de requête ou plus sont ouvertes, vous pouvez les organiser en onglets verticalement ou horizontalement pour les voir toutes. Pour voir les fenêtres sous forme d’onglets, cliquez avec le bouton droit sur le titre de la requête, puis sélectionnez l’option d’onglets souhaitée : 
 
    ![Options d’onglets de requête](media/ssms-configuration/querytabbedoptions.png)

    - Voici un groupe d’onglets horizontal :

      ![Exemple de groupe d’onglets horizontal](media/ssms-configuration/horizontaltab.png)     
    
    - Voici un groupe d’onglets vertical :

      ![Exemple de groupe d’onglets vertical](media/ssms-configuration/verticaltabgroup.png)
        
    - Pour fusionner les onglets, cliquez avec le bouton droit sur le titre de la requête, puis sélectionnez **Déplacer vers le groupe d’onglets précédent** ou **Déplacer vers le groupe d’onglets suivant** :
    
      ![Fusionner des onglets de requête](media/ssms-configuration/mergetabgroups.png)

- Pour restaurer la disposition par défaut de l’environnement, dans le menu **Fenêtre**, sélectionnez **Rétablir la disposition de fenêtre** :
 
    ![Restaurer la disposition de fenêtre](media/ssms-configuration/resetwindowlayout.png)
    
## <a name="maximize-query-editor"></a>Agrandir l'Éditeur de requête
Vous pouvez agrandir l’éditeur de requête en mode plein écran :

1. Cliquez n'importe où dans la fenêtre de l'Éditeur de requête.
2. Appuyez sur Maj + Alt + Entrée pour basculer entre le mode plein écran et le mode normal. 

Ce raccourci clavier fonctionne dans toute fenêtre de document. 



## <a name="change-basic-settings"></a>Changer les paramètres de base
Cette section décrit comment changer certains paramètres de base dans SSMS à partir du menu **Outils**.

  ![Outils (menu)](media/ssms-configuration/tools.png)


- Pour modifier la barre d’outils en surbrillance, sélectionnez **Outils** > **Personnaliser** :

    ![Personnaliser une barre d’outils](media/ssms-configuration/toolbar.png)

### <a name="change-the-font"></a>Changer la police
- Pour changer la police, sélectionnez **Outils** > **Options** > **Polices et couleurs** :

     ![Changer les polices et couleurs](media/ssms-configuration/fontsandcolors.png)

### <a name="change-startup-options"></a>Changer les options de démarrage
- Les options de démarrage déterminent l’apparence de votre espace de travail quand vous ouvrez SSMS. Pour changer les options de démarrage, sélectionnez **Outils** > **Options** > **Démarrage** :
 
    ![Changer les options de démarrage](media/ssms-configuration/startup.png)

### <a name="reset-settings-to-the-default"></a>Rétablir les paramètres par défaut
- Vous pouvez exporter et importer tous ces paramètres à partir du menu. Pour importer ou exporter des paramètres, ou pour restaurer les paramètres par défaut, sélectionnez **Outils** > **Importer et exporter des paramètres** 

    ![Importer et exporter des paramètres](media/ssms-configuration/settings.png)



## <a name="next-steps"></a>Étapes suivantes
L’article suivant vous donne quelques conseils et astuces supplémentaires pour utiliser SSMS, notamment comment rechercher votre journal des erreurs SQL Server et le nom de votre instance SQL. 

> [!div class="nextstepaction"]
> [Conseils et astuces supplémentaires pour utiliser SSMS](ssms-tricks.md)
 
 




