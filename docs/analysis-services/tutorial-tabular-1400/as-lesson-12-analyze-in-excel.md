---
title: 'Leçon du didacticiel Analysis Services 12 : analyser dans Excel | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d215045f87ed780a4adc97f9ae4fed9ac7e6991a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analyze-in-excel"></a>Analyser dans Excel

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous utilisez la fonctionnalité analyser dans Excel pour ouvrir Microsoft Excel, créer automatiquement une connexion à l’espace de travail du modèle et ajouter automatiquement un tableau croisé dynamique à la feuille de calcul. La fonction Analyser dans Excel offre un moyen simple et rapide de tester l'efficacité de votre conception de modèle avant de le déployer. Vous n’effectuez pas une analyse de données dans cette leçon. L'objectif de cette leçon est de vous familiariser, en tant qu'auteur de modèle, avec les outils que vous pouvez utiliser pour tester votre conception de modèle.   
  
Pour effectuer cette leçon, Excel doit être installé sur le même ordinateur que Visual Studio.
  
Durée estimée pour effectuer cette leçon : **5 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 11 : créer des rôles](../tutorial-tabular-1400/as-lesson-11-create-roles.md).  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a>Parcourir des données à l'aide des perspectives par défaut et Internet Sales  

Dans ces premières tâches, vous parcourez votre modèle à l’aide à la fois la perspective par défaut, qui inclut tous les objets de modèle, et également à l’aide de la perspective Internet Sales vous précédemment. La perspective Internet Sales exclut l'objet table Customer.  
  
#### <a name="to-browse-by-using-the-default-perspective"></a>Pour parcourir les données à l'aide de la perspective par défaut  
  
1.  Cliquez sur le **modèle** menu > **analyser dans Excel**.  
  
2.  Dans la boîte de dialogue **Analyser dans Excel** , cliquez sur **OK**.  
  
    Excel s’ouvre avec un nouveau classeur. Une connexion de source de données est créée à l'aide du compte d'utilisateur actuel et la perspective par défaut est utilisée pour définir les champs visibles. Un tableau croisé dynamique est automatiquement ajouté à la feuille de calcul.  
  
3.  Dans Excel, dans le **liste de champs de tableau croisé dynamique**, notez le **DimDate** et **FactInternetSales** groupes de mesures s’affichent. Le **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, et **FactInternetSales** tables avec leurs colonnes respectives apparaissent également.  
  
4.  Fermez Excel sans enregistrer le classeur.  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a>Pour parcourir les données à l'aide de perspective Internet Sales  
  
1.  Cliquez sur le **modèle** menu, puis sur **analyser dans Excel**.  
  
2.  Dans la boîte de dialogue **Analyser dans Excel** , laissez **Utilisateur Windows actuel** sélectionné, puis dans la zone de liste déroulante **Perspective** , sélectionnez **Internet Sales**, puis cliquez sur **OK**. 
    
    ![en tant que-leçon 12-perspective](../tutorial-tabular-1400/media/as-lesson12-perspective.png)
    
3.  Dans Excel, dans **PivotTable Fields**, notez la table DimCustomer est exclue de la liste de champs.  
    
    ![en tant que champs de leçon 12](../tutorial-tabular-1400/media/as-lesson12-fields.png)
    
4.  Fermez Excel sans enregistrer le classeur.  
  
## <a name="browse-by-using-roles"></a>Accédez à l’aide de rôles  

Les rôles sont une partie importante de n’importe quel modèle tabulaire. Au moins un rôle auquel les utilisateurs sont ajoutés en tant que membres, les utilisateurs ne peuvent pas accéder à et analyser les données à l’aide de votre modèle. La fonctionnalité Analyser dans Excel vous fournit un moyen de tester les rôles que vous avez définis.  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a>Pour accéder à l’aide du rôle d’utilisateur Sales Manager  
  
1.  Dans SSDT, cliquez sur le **modèle** menu, puis sur **analyser dans Excel**.  
  
2.  Dans **spécifier le nom d’utilisateur ou le rôle à utiliser pour se connecter au modèle**, sélectionnez **rôle**, puis, dans la zone de liste déroulante, sélectionnez **responsable des ventes**, puis cliquez sur **OK**.  
  
    Excel s’ouvre avec un nouveau classeur. Un tableau croisé dynamique est créé automatiquement. La liste de champs de tableau croisé dynamique inclut tous les champs de données disponibles dans votre nouveau modèle.  
      
3.  Fermez Excel sans enregistrer le classeur.  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?

Accédez à la leçon suivante : [leçon 13 : déployer](../tutorial-tabular-1400/as-lesson-13-deploy.md).

  
  
  
