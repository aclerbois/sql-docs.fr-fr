---
title: 'Leçon 12 : Créer des rôles | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7ec7afeddeeb4aae53f1368f55e2af5ec3ffcd0e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-11-create-roles"></a>Leçon 11 : Créer des rôles
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans cette leçon, vous allez créer des rôles. Les rôles fournissent la sécurité des objets et des données d'une base de données de modèles, en limitant l'accès aux utilisateurs Windows qui y sont membres. Chaque rôle est défini avec une autorisation unique : aucune autorisation, autorisation de lecture, autorisation de lecture et traitement, autorisation de traitement ou autorisation d'administrateur. Les rôles peuvent être définis pendant la création du modèle à l’aide du Gestionnaire de rôles. Une fois le modèle déployé, vous pouvez gérer les rôles à l'aide de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Rôles](../analysis-services/tabular-models/roles-ssas-tabular.md).  
  
> [!NOTE]  
> La création de rôles n'est pas nécessaire pour effectuer ce didacticiel. Par défaut, le compte auquel vous êtes connecté aura des privilèges d'administrateur sur le modèle. Toutefois, pour autoriser d’autres utilisateurs dans votre organisation pour parcourir le modèle à l’aide d’un client de création de rapports, vous devez créer au moins un rôle avec la lecture des autorisations et ajouter ces utilisateurs en tant que membres.  
  
Vous allez créer trois rôles :  
  
-   **Responsable des ventes** – ce rôle peut inclure des utilisateurs de votre organisation pour laquelle vous souhaitez avoir l’autorisation lecture sur tous les objets de modèle et les données.  
  
-   **Analyste des ventes aux États-Unis** – ce rôle peut inclure des utilisateurs de votre organisation pour laquelle vous souhaitez uniquement être en mesure de parcourir les données relatives aux ventes aux États-Unis. Pour ce rôle, vous utilisez une formule DAX pour définir un *filtre de lignes*, qui limite l’accès des membres aux seules données concernant les États-Unis.  
  
-   **Administrateur** – ce rôle peut inclure des utilisateurs pour lesquels vous souhaitez disposer de droits d’administrateur, ce qui permet un accès illimité et autorisations pour effectuer des tâches d’administration sur la base de données model.  
  
Étant donné que les comptes d'utilisateurs et de groupes Windows dans votre organisation sont uniques, vous pouvez ajouter des comptes de votre organisation aux membres. Toutefois, pour ce didacticiel, vous pouvez également laissez les membres vides. Vous pourrez toujours tester l'effet de chaque rôle plus loin dans la leçon 12 : analyser dans Excel.  
  
Durée estimée pour effectuer cette leçon : **15 minutes**  
  
## <a name="prerequisites"></a>Conditions préalables  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 10 : créer des Partitions](../analysis-services/lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Créer les rôles  
  
#### <a name="to-create-a-sales-manager-user-role"></a>Pour créer un rôle d'utilisateur Sales Manager  
  
1.  Dans l’Explorateur de modèles tabulaires, cliquez sur **rôles** > **rôles**.  
  
2.  Dans le Gestionnaire de rôles, cliquez sur **nouveau**.  
  
3.  Cliquez sur le nouveau rôle, puis, dans le **nom** colonne, renommez le rôle en **responsable des ventes**.  
  
4.  Dans la colonne **Autorisations** , cliquez sur la liste déroulante, puis sélectionnez l’autorisation **Lecture** . 

    ![en tant que-tabulaire-lesson11-nouvelle-role](../analysis-services/media/as-tabular-lesson11-new-role.png) 
  
5.  Facultatif : Cliquez sur le **membres** onglet, puis cliquez sur **ajouter**. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , entrez les utilisateurs ou les groupes Windows de votre organisation à inclure dans le rôle.  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a>Pour créer un rôle d'utilisateur Sales Analyst US  
  
1.  Dans le Gestionnaire de rôles, cliquez sur **nouveau**.    
  
2.  Renommez le rôle en **Sales Analyst US**.  
  
3.  Attribuer ce rôle **en lecture** autorisation.  
  
4.  Cliquez sur l’onglet Filtres de lignes, puis pour le **DimGeography** table uniquement, dans la colonne filtre DAX, tapez la formule suivante :  
  
    ```
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Une formule de filtre de lignes doit être résolue en une valeur booléenne (TRUE/FALSE). Avec cette formule, vous spécifiez que seules les lignes avec la valeur Country Region Code « US » sont visibles pour l'utilisateur.  
    ![en tant que-tabulaire-lesson11-rôle-filtre](../analysis-services/media/as-tabular-lesson11-role-filter.png) 
  
6.  Facultatif : cliquez sur l’onglet **Membres** , puis cliquez sur **Ajouter**. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , entrez les utilisateurs ou les groupes Windows de votre organisation à inclure dans le rôle.  
  
#### <a name="to-create-an-administrator-user-role"></a>Pour créer un rôle d’utilisateur administrateur  
  
1.  Cliquez sur **Nouveau**.  
  
2.  Renommez le rôle en **administrateur**.  
  
3.  Attribuer ce rôle **administrateur** autorisation.  
  
4.  Facultatif : cliquez sur l’onglet **Membres** , puis cliquez sur **Ajouter**. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , entrez les utilisateurs ou les groupes Windows de votre organisation à inclure dans le rôle. 
  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?
Accédez à la leçon suivante : [leçon 12 : analyser dans Excel](../analysis-services/lesson-12-analyze-in-excel.md).

  
  
