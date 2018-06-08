---
Title: 'Tutorial: Using templates in SQL Server Management Studio'
description: Tutoriel pour utiliser des modèles dans SSMS.
keywords: SQL Server, SSMS, SQL Server Management Studio, modèles
author: MashaMSFT
ms.author: mathoma
ms.date: 03/13/2018
ms.topic: Tutorial
ms.suite: sql
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
manager: craigg
helpviewer_keywords:
- templates [SQL Server], SQL Server Management Studio
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
ms.openlocfilehash: 539906b1a09838e43e34be96e4ee32daec19fab7
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="tutorial-using-templates-in-sql-server-management-studio"></a>Tutoriel : Utilisation de modèles dans SQL Server Management Studio
Ce tutoriel vous présente les modèles Transact-SQL (T-SQL) prédéfinis qui sont disponibles dans SQL Server Management Studio (SSMS). Dans cet article, vous apprenez à :

> [!div class="checklist"]
> * Utiliser l’Explorateur de modèles pour générer des scripts T-SQL
> * Modifier un modèle existant 
> * Rechercher des modèles sur le disque
> * Créer un modèle
   

## <a name="prerequisites"></a>Conditions préalables requises
Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio et de l’accès à un serveur SQL. 

- Installez [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).

 

## <a name="use-template-browser"></a>Utiliser l’Explorateur de modèles
Dans cette section, vous apprenez à localiser et utiliser l’Explorateur de modèles. 

1. Ouvrez SQL Server Management Studio.
2. Dans le menu **Affichage**, sélectionnez **Explorateur de modèles** (Ctrl + Alt + T) : 

    ![Ouvrir l’Explorateur de modèles](media/templates-ssms/templatebrowser.png)
    
    Vous pouvez voir les modèles récemment utilisés en bas de l’Explorateur de modèles.

3. Développez le nœud qui vous intéresse. Cliquez avec le bouton droit sur le modèle, puis sélectionnez **Ouvrir** :

    ![Ouvrir un modèle](media/templates-ssms/opentemplate.png)
    
    Vous pouvez également double-cliquer sur le nom du modèle pour l’ouvrir.

4. Une nouvelle fenêtre de requête s’ouvre. Le script T-SQL est déjà rempli. 
5. Modifiez le modèle pour l’adapter à vos besoins, puis sélectionnez **Exécuter** pour exécuter la requête :
    
    ![Créer un modèle de base de données](media/templates-ssms/createdbtemplate.png)


## <a name="edit-an-existing-template"></a>Modifier un modèle existant
Vous pouvez aussi modifier les modèles existants dans l’Explorateur de modèles.  

1. Dans l’Explorateur de modèles, accédez au modèle que vous voulez utiliser.
2. Cliquez avec le bouton droit sur le modèle, puis sélectionnez **Modifier** :

    ![Modifier un modèle](media/templates-ssms/edittemplate.png)

3. Dans la fenêtre de requête qui s’ouvre, apportez les modifications souhaitées.
4. Pour enregistrer le modèle, sélectionnez **Fichier** > **Enregistrer** (Ctrl + S).
5. Fermez la fenêtre de requête.
6. Rouvrez le modèle. Vos modifications doivent apparaître.
 

## <a name="locate-templates-on-disk"></a>Rechercher des modèles sur le disque
Quand un modèle est ouvert, vous pouvez localiser les modèles qui se trouvent sur le disque.

1. Dans l’Explorateur de modèles, sélectionnez un modèle, puis **Modifier**.
2. Cliquez avec le bouton droit sur **Titre de la requête**, puis sélectionnez **Ouvrir le dossier contenant**. L’explorateur doit s’ouvrir à l’endroit où les modèles sont stockés sur le disque : 

   ![Modèles sur le disque](media/templates-ssms/templatesondisk.png)
  

## <a name="create-a-new-template"></a>Créer un modèle
Vous pouvez aussi créer un modèle dans l’Explorateur de modèles. Les étapes suivantes vous montrent comment créer un dossier, puis créer un modèle dans ce dossier. Vous pouvez aussi utiliser ces étapes pour créer un modèle personnalisé dans un dossier existant. 

1. Ouvrez l’Explorateur de modèles.
2. Cliquez avec le bouton droit sur **Modèles SQL Server**, puis sélectionnez **Nouveau** > **Dossier**.
3. Nommez ce dossier **Modèles personnalisés** :

    ![Créer un dossier Modèles personnalisés](media/templates-ssms/creatingcustomtemplate.png)

4. Cliquez avec le bouton droit sur le dossier Modèles personnalisés que vous venez de créer, puis sélectionnez **Nouveau** > **Modèle**. Entrez un nom pour votre modèle :
 
    ![Créer un modèle personnalisé](media/templates-ssms/createnewtemplate.png)
   
5. Cliquez avec le bouton droit sur le modèle que vous avez créé, puis sélectionnez **Modifier**. Une nouvelle fenêtre de requête s’ouvre.
6. Entrez le texte T-SQL à enregistrer. 
7. Dans le menu **Fichier**, sélectionnez **Enregistrer**.
8. Fermez la fenêtre de requête existante et ouvrez votre nouveau modèle personnalisé. 

    

## <a name="next-steps"></a>Étapes suivantes
L’article suivant propose des conseils et astuces supplémentaires pour utiliser SQL Server Management Studio. 

> [!div class="nextstepaction"]
> [Conseils et astuces supplémentaires pour utiliser SSMS](ssms-tricks.md)
