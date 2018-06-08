---
title: 'Étape 6 : Ajout et configuration des transformations de recherche | Microsoft Docs'
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
ms.assetid: 5c59f723-9707-4407-80ae-f05f483cf65f
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d2702b9e69660b4ff0b4fcd2055d522f47d094c6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-6---adding-and-configuring-the-lookup-transformations"></a>Leçon 1-6 : Ajout et configuration des transformations de recherche
Une fois que vous avez configuré la source de fichier plat pour extraire les données du fichier source, la tâche suivante consiste à définir les transformations de recherche nécessaires pour obtenir les valeurs de **CurrencyKey** et **DateKey**. Une transformation de recherche effectue une recherche en joignant les données dans la colonne d'entrée spécifiée à une colonne dans un dataset de référence. Le dataset de référence peut être une table ou une vue existante, une nouvelle table ou le résultat d'une instruction SQL. Dans ce didacticiel, la transformation de recherche utilise un gestionnaire de connexions OLE DB pour se connecter à la base de données qui contient les données servant de source au jeu de données de référence.  
  
> [!NOTE]  
> Vous pouvez également configurer la transformation de recherche afin qu'elle se connecte à un cache qui contient le jeu de données de référence. Pour plus d’informations, voir [Lookup Transformation](../integration-services/data-flow/transformations/lookup-transformation.md).  
  
Dans le cadre de ce didacticiel, vous allez ajouter et configurer les deux composants de transformation de recherche suivants dans le package :  
  
-   Une transformation pour effectuer une recherche des valeurs à partir de la colonne **CurrencyKey** de la table de dimensions **DimCurrency** basée sur la correspondance avec les valeurs de la colonne **CurrencyID** à partir du fichier plat.  
  
-   Une transformation pour effectuer une recherche des valeurs à partir de la colonne **DateKey** de la table de dimensions **DimDate** basée sur la correspondance avec les valeurs de la colonne **CurrencyDate** à partir du fichier plat.  
  
Dans les deux cas, la transformation de recherche utilise le Gestionnaire de connexions OLE DB que vous avez créé précédemment.  
  
### <a name="to-add-and-configure-the-lookup-currency-key-transformation"></a>Pour ajouter et configurer la transformation Lookup Currency Key  
  
1.  Dans la **boîte à outils SSIS**, développez **Commun**, puis faites glisser **Recherche** sur l'aire de conception de l'onglet **Flux de données** . Placez la recherche directement sous la source **Extract Sample Currency Data** .  
  
2.  Sélectionnez la source de fichier plat **Extract Sample Currency Data** et faites glisser la flèche verte vers la transformation de **recherche** que vous venez d'ajouter pour connecter les deux composants.  
  
3.  Dans l'aire de conception **Flux de données** , cliquez sur **Recherche** dans la transformation de **Recherche** , puis remplacez le nom par **Lookup Currency Key**.  
  
4.  Double-cliquez sur la transformation **Lookup CurrencyKey** pour afficher l’éditeur de transformation de recherche.  
  
5.  Dans la page **Général** , effectuez les sélections suivantes :  
  
    1.  Sélectionnez **Cache complet**.  
  
    2.  Dans la zone **Type de connexion** , sélectionnez **Gestionnaire de connexions OLE DB**.  
  
6.  Dans la page **Connexion** , effectuez les sélections suivantes :  
  
    1.  Dans la boîte de dialogue **Gestionnaire de connexions OLE DB** , assurez-vous que **localhost.AdventureWorksDW2012** est affiché.  
  
    2.  Sélectionnez **Utiliser les résultats d'une requête SQL**, puis tapez ou copiez l'instruction SQL suivante :  
  
        ```sql
        SELECT * FROM [dbo].[DimCurrency]
        WHERE [CurrencyAlternateKey]
        IN ('ARS', 'AUD', 'BRL', 'CAD', 'CNY',
            'DEM', 'EUR', 'FRF', 'GBP', 'JPY',
            'MXN', 'SAR', 'USD', 'VEB')
        ```  
  
7.  Dans la page **Colonnes** , effectuez les sélections suivantes :  
  
    1.  Dans le volet **Colonnes d'entrée disponibles** , faites glisser **CurrencyID** vers le volet **Colonnes de recherche disponibles** et déposez cet élément sur **CurrencyAlternateKey**.  
  
    2.  Dans la liste **Colonnes de recherche disponibles** , activez la case à cocher située à gauche de **CurrencyKey**.  
  
8.  Cliquez sur **OK** pour revenir à l'aire de conception **Flux de données** .  
  
9. Double-cliquez sur la transformation Lookup Currency Key, puis cliquez sur **Propriétés**.  
  
10. Dans la fenêtre Propriétés, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** et la propriété **DefaultCodePage** la valeur **1252**.  
  
### <a name="to-add-and-configure-the--lookup-datekey-transformation"></a>Pour ajouter et configurer la transformation Lookup Date Key  
  
1.  Dans la **Boîte à outils SSIS**, faites glisser **Recherche** vers l'aire de conception **Flux de données** . Placez la recherche directement sous la transformation **Lookup Currency Key** .  
  
2.  Sélectionnez la transformation **Lookup Currency Key** et faites glisser la flèche verte vers la transformation de **recherche** que vous venez d'ajouter pour connecter les deux composants.  
  
3.  Dans la boîte de dialogue **Sélection entrée et sortie** , cliquez sur **Sortie de recherche avec correspondance** dans la zone de liste de **Sortie** , puis cliquez sur **OK**.  
  
4.  Dans l'aire de conception **Flux de données** , cliquez sur **Recherche** dans la transformation **Recherche** que vous venez d'ajouter, puis remplacez le nom par **Lookup Date Key**.  
  
5.  Double-cliquez sur la transformation **Lookup Date Key** .  
  
6.  Dans la page **Général** , sélectionnez **Cache partiel**.  
  
7.  Dans la page **Connexion** , effectuez les sélections suivantes :  
  
    1.  Dans la boîte de dialogue **Gestionnaire de connexions OLE DB** , vérifiez que **localhost.AdventureWorksDW2012** est affiché.  
  
    2.  Dans la zone **Utiliser une table ou une vue** , tapez ou sélectionnez **[dbo].[DimDate]**.  
  
8.  Dans la page **Colonnes** , effectuez les sélections suivantes :  
  
    1.  Dans le volet **Colonnes d'entrée disponibles** , faites glisser **CurrencyDate** vers le volet **Colonnes de recherche disponibles** et déposez cet élément sur **FullDateAlternateKey**.  
  
    2.  Dans la liste **Colonnes de recherche disponibles** , activez la case à cocher située à gauche de **DateKey**.  
  
9. Dans la page **Avancé** , examinez les options de mise en cache.  
  
10. Cliquez sur **OK** pour revenir à l'aire de conception **Flux de données** .  
  
11. Double-cliquez sur la transformation Lookup Date Key, puis cliquez sur **Propriétés**.  
  
12. Dans la fenêtre Propriétés, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** et la propriété **DefaultCodePage** la valeur **1252**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 7 : ajout et configuration de la destination OLE DB](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
## <a name="see-also"></a> Voir aussi  
[Lookup Transformation](../integration-services/data-flow/transformations/lookup-transformation.md)  
  
  
  
