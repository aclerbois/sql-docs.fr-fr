---
title: 'Leçon du didacticiel Analysis Services 11 : créer des rôles | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfiles"
ms.openlocfilehash: 8e630d53aed8f722c2de4a21afea8391cefe8bd8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-roles"></a>Créer les rôles

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous créez des rôles. Les rôles fournissent la sécurité de données et l’objet de base de données de modèle en limitant l’accès aux seuls utilisateurs qui sont membres du rôle. Chaque rôle est défini avec une autorisation unique : aucune autorisation, autorisation de lecture, autorisation de lecture et traitement, autorisation de traitement ou autorisation d'administrateur. Les rôles peuvent être définis pendant la création du modèle à l’aide du Gestionnaire de rôles. Après avoir déployé un modèle, vous pouvez gérer les rôles à l’aide de SQL Server Management Studio (SSMS). Pour plus d’informations, consultez [rôles](../tabular-models/roles-ssas-tabular.md).
  
> [!NOTE]  
> La création de rôles n'est pas nécessaire pour effectuer ce didacticiel. Par défaut, le compte que vous êtes actuellement connecté dispose des privilèges d’administrateur sur le modèle. Toutefois, pour d’autres utilisateurs de votre organisation accéder à l’aide d’un client de création de rapports, vous devez créer au moins un rôle avec la lecture des autorisations et ajouter ces utilisateurs en tant que membres.  
  
Vous créez trois rôles :  
  
-   **Responsable des ventes** – ce rôle peut inclure des utilisateurs de votre organisation pour laquelle vous souhaitez avoir l’autorisation lecture sur tous les objets de modèle et les données.  
  
-   **Analyste des ventes aux États-Unis** – ce rôle peut inclure des utilisateurs de votre organisation pour laquelle vous souhaitez uniquement être en mesure de parcourir les données relatives aux ventes aux États-Unis. Pour ce rôle, vous utilisez une formule DAX pour définir un *filtre de lignes*, ce qui limite les membres pour parcourir les données uniquement pour les États-Unis.  
  
-   **Administrateur** – ce rôle peut inclure des utilisateurs pour lesquels vous souhaitez disposer de droits d’administrateur, ce qui permet un accès illimité et autorisations pour effectuer des tâches d’administration sur la base de données model.  
  
Étant donné que les comptes d'utilisateurs et de groupes Windows dans votre organisation sont uniques, vous pouvez ajouter des comptes de votre organisation aux membres. Toutefois, pour ce didacticiel, vous pouvez également laissez les membres vides. Tester l’effet de chaque rôle plus tard dans la leçon 12 : analyser dans Excel.  
  
Durée estimée pour effectuer cette leçon : **15 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 10 : créer des partitions](../tutorial-tabular-1400/as-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Créer les rôles  
  
#### <a name="to-create-a-sales-manager-user-role"></a>Pour créer un rôle d'utilisateur Sales Manager  
  
1.  Dans l’Explorateur de modèles tabulaires, cliquez sur **rôles** > **rôles**.  
  
2.  Dans le Gestionnaire de rôles, cliquez sur **nouveau**.  
  
3.  Cliquez sur le nouveau rôle, puis, dans le **nom** colonne, renommez le rôle en **responsable des ventes**.  
  
4.  Dans la colonne **Autorisations** , cliquez sur la liste déroulante, puis sélectionnez l’autorisation **Lecture** . 

    ![en tant que-lesson11-nouvelle-role](../tutorial-tabular-1400/media/as-lesson11-new-role.png) 
  
5.  Facultatif : Cliquez sur le **membres** onglet, puis cliquez sur **ajouter**. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , entrez les utilisateurs ou les groupes Windows de votre organisation à inclure dans le rôle.  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a>Pour créer un rôle d'utilisateur Sales Analyst US  
  
1.  Dans le Gestionnaire de rôles, cliquez sur **nouveau**.    
  
2.  Renommez le rôle en **Sales Analyst US**.  
  
3.  Attribuer ce rôle **en lecture** autorisation.  
  
4.  Cliquez sur l’onglet Filtres de lignes, puis, pour le **DimGeography** table uniquement, dans la colonne filtre DAX, tapez la formule suivante :  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Une formule de filtre de lignes doit être résolue en une valeur booléenne (TRUE/FALSE). Avec cette formule, vous spécifiez que seules les lignes avec la valeur de Code pays/région « US » sont visibles par l’utilisateur.  
    ![en tant que-lesson11-rôle-filtre](../tutorial-tabular-1400/media/as-lesson11-role-filter.png) 
  
6.  Facultatif : Cliquez sur le **membres** onglet, puis cliquez sur **ajouter**. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , entrez les utilisateurs ou les groupes Windows de votre organisation à inclure dans le rôle.  
  
#### <a name="to-create-an-administrator-user-role"></a>Pour créer un rôle d’utilisateur administrateur  
  
1.  Cliquez sur **Nouveau**.  
  
2.  Renommez le rôle en **administrateur**.  
  
3.  Attribuer ce rôle **administrateur** autorisation.  
  
4.  Facultatif : Cliquez sur le **membres** onglet, puis cliquez sur **ajouter**. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , entrez les utilisateurs ou les groupes Windows de votre organisation à inclure dans le rôle. 
  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?

[Leçon 12 : Analyser dans Excel](../tutorial-tabular-1400/as-lesson-12-analyze-in-excel.md).

  
  
