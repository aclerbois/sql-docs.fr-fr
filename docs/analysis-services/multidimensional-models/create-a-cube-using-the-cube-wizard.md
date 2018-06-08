---
title: Créer un Cube à l’aide de l’Assistant Cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d682ada2175d94f7b077bfe06626885862d45e21
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-cube-using-the-cube-wizard"></a>Créer un cube à l'aide de l'Assistant Cube
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez créer un cube en utilisant l’Assistant Cube dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
### <a name="to-create-a-new-cube"></a>Pour créer un cube  
  
1.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Cubes**, puis cliquez sur **Nouveau cube**.  
  
2.  Dans la page **Sélectionner la méthode de création** de l’Assistant Cube, sélectionnez **Utiliser des tables existantes**, puis cliquez sur **Suivant**.  
  
    > [!NOTE]  
    >  Parfois, vous devez éventuellement créer un cube sans utiliser de tables existantes. Pour créer un cube vide, sélectionnez **Créer un cube vide**. Pour générer des tables, sélectionnez **Générer des tables dans la source de données**.  
  
3.  Dans la page **Sélectionner les tables de groupes de mesures** , suivez les procédures ci-dessous :  
  
    1.  Dans la liste **Vue de source de données** , sélectionnez une vue de source de données.  
  
    2.  Dans la liste **Tables de groupes de mesures** , sélectionnez les tables qui à utiliser pour créer des groupes de mesures.  
  
    3.  Cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner les mesures** , sélectionnez les mesures à inclure dans le cube, puis cliquez sur **Suivant**.  
  
     Vous pouvez éventuellement modifier les noms des mesures et groupes de mesures.  
  
5.  Dans la page **Sélectionner des dimensions existantes** , sélectionnez les dimensions existantes à inclure dans le cube, puis cliquez sur **Suivant**.  
  
    > [!NOTE]  
    >  La page **Sélectionner des dimensions existantes** apparaît si des dimensions existent déjà dans la base de données pour les groupes de mesures sélectionnés.  
  
6.  Dans la page **Sélectionner de nouvelles dimensions** , sélectionnez les nouvelles dimensions à créer, puis cliquez sur **Suivant**.  
  
    > [!NOTE]  
    >  La page **Sélectionner de nouvelles dimensions** apparaît si des tables conviennent aux tables de dimension et si elles n’ont pas déjà été utilisées par des dimensions existantes.  
  
7.  Dans la page **Sélectionner les clés de dimension manquantes** , sélectionnez une clé pour la dimension, puis cliquez sur **Suivant**.  
  
    > [!NOTE]  
    >  La page **Sélectionner les clés de dimension manquantes** apparaît si une clé n’est pas définie pour une table de dimension que vous avez spécifiée.  
  
8.  Dans la page **Fin de l’Assistant** , entrez le nom du nouveau cube et examinez sa structure. Si vous souhaitez apporter des modifications, cliquez sur **Précédent**; sinon, cliquez sur **Terminer**.  
  
    > [!NOTE]  
    >  Vous pouvez utiliser le Concepteur de cube une fois l'exécution de l'Assistant Cube terminée pour définir le cube. Vous pouvez également utiliser le Concepteur de dimensions pour ajouter, supprimer et configurer des attributs et des hiérarchies dans les dimensions que vous avez créées.  
  
  
