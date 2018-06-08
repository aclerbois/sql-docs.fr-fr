---
title: Définir des relations logiques dans une vue de Source de données (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d59812b7e1a427dc00f26c2d583d8cbca45506d5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="define-logical-relationships-in-a-data-source-view-analysis-services"></a>Définir des relations logiques dans une vue de source de données (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'Assistant Vue de source de données et le Concepteur de vue de source de données définissent automatiquement les relations entre les tables ajoutées à une vue de source de données (DSV), en fonction des relations de la base de données sous-jacente ou des critères de correspondance de noms que vous spécifiez.  
  
 Dans les cas où vous utilisez des données de plusieurs sources de données, vous devrez peut-être définir manuellement des relations logiques dans la vue DSV afin de compléter ces relations définies automatiquement. Des relations sont requises dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour identifier les tables de faits et de dimension, pour créer des requêtes pour la récupération de données et de métadonnées à partir de sources de données sous-jacentes, et pour tirer parti des fonctionnalités Business Intelligence avancées.  
  
 Vous pouvez définir les types de relations suivants dans le Concepteur de vue de source de données :  
  
-   Une relation d'une table à une autre table dans la même source de données.  
  
-   Une relation d'une table à elle-même, comme dans une relation parent-enfant.  
  
-   Une relation d'une table dans une source de données à une autre table dans une autre source de données.  
  
> [!NOTE]  
>  Les relations définies dans une vue DSV sont logiques et peuvent ne pas refléter les relations réelles définies dans la source de données sous-jacente. Vous pouvez créer des relations dans le Concepteur de vues de source de données, qui n'existent pas dans la source de données sous-jacente, et supprimer des relations créées par le Concepteur de vues de source de données à partir de relations de clé étrangère existantes dans la source de données sous-jacente.  
  
 Les relations sont dirigées. Pour chaque valeur dans la colonne source, il existe une valeur correspondante dans la colonne de destination. Dans un diagramme de vue de source de données, tel que les diagrammes affichés dans le volet **Diagramme** , une flèche placée sur la ligne entre deux tables indique le sens de la relation.  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Pour ajouter une relation entre des tables, des requêtes nommées ou des vues](#bkmk_addRel)  
  
 [Pour afficher ou modifier une relation dans le volet Diagramme](#bkmk_diagrampane)  
  
 [Pour afficher ou modifier une relation dans le volet Tables](#bkmk_tablespane)  
  
##  <a name="bkmk_addRel"></a> Pour ajouter une relation entre des tables, des requêtes nommées ou des vues  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet ou connectez-vous à la base de données qui contient la vue de source de données dans laquelle vous souhaitez ajouter une relation logique.  
  
2.  Dans l’Explorateur de solutions, développez le dossier **Vues des sources de données** , puis double-cliquez sur la vue de source de données afin de l’ouvrir dans le **Concepteur de vue de source de données**.  
  
3.  Cliquez avec le bouton droit sur la table, la requête nommée ou la vue à laquelle vous voulez ajouter une relation dans le volet **Tables** , puis sélectionnez **Nouvelle relation**.  
  
    > [!NOTE]  
    >  Pour rechercher une table, une vue ou une requête nommée, vous pouvez utiliser l’option **Rechercher une table** en cliquant sur le menu **Vue de source de données** ou en cliquant avec le bouton droit dans une zone ouverte du volet **Tables** ou **Diagramme** .  
  
4.  Dans la boîte de dialogue **Spécifier la relation** , procédez comme suit :  
  
    1.  Sélectionnez la table, la requête nommée ou la vue appropriée dans la liste **Table source (clé étrangère)** .  
  
    2.  Sélectionnez la table, la requête nommée ou la vue appropriée dans la liste **Table de destination (clé primaire)** .  
  
    3.  Sélectionnez des colonnes dans les listes **Colonnes sources** et **Colonnes de destination** pour créer une relation entre les deux tables.  
  
         Si [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] détecte, pendant l’échantillonnage des données dans la requête nommée, la vue ou la table sous-jacente, que vous avez défini la relation dans une direction incorrecte (de la clé primaire vers la clé étrangère plutôt que l’inverse), vous êtes invité à en inverser le sens. Pour inverser rapidement le sens, cliquez sur **Inverser**.  
  
         Si [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] détecte qu’une relation existe déjà pour les colonnes sélectionnées, vous recevez un message. Vous ne pouvez pas définir des relations en double.  
  
    4.  Si vous le souhaitez, dans la zone **Description** , tapez la description de la relation.  
  
##  <a name="bkmk_diagrampane"></a> Pour afficher ou modifier une relation dans le volet Diagramme  
  
-   Dans le volet **Diagramme** , dans **Concepteur de vue de source de données**, cliquez avec le bouton droit sur la relation que vous souhaitez afficher, puis sélectionnez **Modifier la relation** (ou double-cliquez simplement sur la flèche représentant la relation).  Utilisez la boîte de dialogue **Modifier la relation** pour modifier la relation.  
  
##  <a name="bkmk_tablespane"></a> Pour afficher ou modifier une relation dans le volet Tables  
  
1.  Dans le volet **Tables** dans **Concepteur de vue de source de données**, développez la table, la vue ou la requête nommée contenant la relation que vous souhaitez afficher ou modifier.  
  
2.  Développez le dossier **Relations** .  Les relations entre la table, la vue ou la requête nommée sélectionnée et les autres tables, vues ou requêtes nommées s'affichent dans la colonne des relations.  
  
3.  Cliquez avec le bouton droit sur la relation que vous souhaitez modifier, puis sélectionnez **Modifier la relation**.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)  
  
  
