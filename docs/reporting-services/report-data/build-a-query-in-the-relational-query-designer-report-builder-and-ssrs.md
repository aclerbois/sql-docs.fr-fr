---
title: Générer une requête dans le concepteur de requêtes relationnelles (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 28b25861-f3b4-4c3e-a9b0-03d6e4cfea26
caps.latest.revision: 7
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d0ca2e4b18c0bfbb267aa0bcf79da69459dcdc2f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="build-a-query-in-the-relational-query-designer-report-builder-and-ssrs"></a>Générer une requête dans le concepteur de requêtes relationnelles (Générateur de rapports et SSRS)
  Un concepteur de requêtes vous aide à spécifier les données à récupérer à partir d'une source de données externe pour un dataset de rapport. Vous utilisez un concepteur de requêtes lorsque vous générez une requête dans un Assistant ou créez une requête de dataset.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Un dataset repose sur une source de données. Le type de source de données et l'environnement de création détermine le concepteur de requêtes qui s'ouvre lorsque vous définissez la requête de dataset. Les fonctionnalités du concepteur de requêtes varient selon le type de source de données sous-jacent. Pour plus d’informations sur les couches de données, consultez [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34) ou [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
 Vous pouvez utiliser un concepteur de requêtes pour les tâches suivantes :  
  
-   explorer les métadonnées de plusieurs schémas sur la source de données externe ;  
  
-   spécifier des champs à récupérer pour le dataset ;  
  
-   spécifier des relations entre deux objets tels que des tables ;  
  
-   spécifier des filtres pour restreindre les données avant qu'elles ne soient récupérées en tant que données de rapport ;  
  
-   indiquer s'il faut créer des paramètres ;  
  
-   spécifier des agrégats pour effectuer des calculs sur la source de données externe.  
  
 Après avoir ouvert un concepteur de requêtes, vous générez une requête de la même façon pour un dataset incorporé ou un dataset partagé. Les procédures suivantes utilisent une requête de dataset incorporé.  
  
 Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes relationnelles &#40;Générateur de rapports&#41;](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md).  
  
### <a name="to-build-a-query-for-an-embedded-dataset-in-report-design-view"></a>Pour générer une requête pour un dataset incorporé en mode création de rapport  
  
1.  Ouvrir le concepteur de requêtes. Dans le volet des données de rapport, cliquez avec le bouton droit sur le dataset, puis cliquez sur **Requête**.  
  
     Le concepteur de requêtes associé à la source de données s'ouvre.  
  
2.  Dans le volet Vue de base de données, développez les dossiers qui affichent une vue hiérarchique des objets de schéma de base de données tels que les tables, les vues et les procédures stockées. Cliquez sur la zone de sélection pour sélectionner tous les champs d'un objet ou développez le nœud pour sélectionner des champs individuels.  
  
     Quand vous sélectionnez des champs du volet Vue de base de données, le volet **Champs sélectionnés** affiche vos sélections.  
  
     Si vous sélectionnez des champs à partir de plusieurs tables de base de données associées, utilisez le volet Relations pour voir les relations détectées entre les tables à partir du schéma de base de données.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     La liste des champs de dataset s'affiche dans le volet des données de rapport.  
  
### <a name="to-specify-limits-for-a-query"></a>Pour spécifier les limites d'une requête  
  
1.  Dans le concepteur de requêtes relationnelles, vérifiez que des champs sont sélectionnés et qu’ils sont affichés dans le volet **Champs sélectionnés** .  
  
2.  Dans la barre d’outils du volet Filtres appliqués, cliquez sur **Ajouter un filtre**. Une nouvelle ligne de filtres apparaît.  
  
3.  Dans **Nom du champ**, cliquez pour afficher la liste déroulante des champs, puis cliquez sur le nom du champ servant de filtre. Par exemple, pour filtrer par quantité, cliquez sur le champ qui contient le nombre d'éléments.  
  
4.  Dans **Opérateur**, cliquez pour afficher la liste déroulante des opérateurs, puis sélectionnez l’opérateur de comparaison à utiliser dans le filtre.  
  
5.  Dans **Valeur**, tapez la valeur qui doit servir de filtre. Par exemple, pour filtrer les quantités supérieures à 100, tapez 100.  
  
6.  Sélectionnez l'option de paramètre dans cette ligne pour créer un paramètre de dataset afin de permettre à l'utilisateur de spécifier une valeur de filtre. Un paramètre de rapport qui correspond au paramètre de dataset est créé automatiquement.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 La liste des champs de dataset s'affiche dans le volet des données de rapport.  
  
### <a name="to-view-a-query-result-set"></a>Pour afficher un jeu de résultats de requête  
  
1.  Dans la barre d’outils du Concepteur de requêtes, cliquez sur **Exécuter la requête (!)**.  
  
    > [!NOTE]  
    >  Le concepteur de requêtes utilise des informations d'identification au moment de la conception pour exécuter la requête et récupérer le jeu de résultats. Pour plus d’informations, consultez [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
 La requête s'exécute sur la source de données et retourne des exemples de données dans le volet Résultats de la requête.  
  
## <a name="see-also"></a> Voir aussi  
 [Jeux de données du rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)   
 [Ajouter des données à partir de sources de données externes &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md)   
 [Concepteurs de requêtes &#40;Générateur de rapports&#41;](http://msdn.microsoft.com/library/553f0d4e-8b1d-4148-9321-8b41a1e8e1b9)   
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)   
 [Vue Conception de rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/report-design-view-report-builder.md)   
 [Mode création de dataset partagé &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/shared-dataset-design-view-report-builder.md)   
 [Concepteurs de requêtes Reporting Services](http://msdn.microsoft.com/library/07efd3f1-804f-45f7-b62a-3e727a3d9835)  
  
  
