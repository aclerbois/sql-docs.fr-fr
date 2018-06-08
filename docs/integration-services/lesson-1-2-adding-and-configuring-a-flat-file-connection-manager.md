---
title: 'Étape 2 : Ajout et configuration d’un gestionnaire de connexions de fichiers plats | Microsoft Docs'
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
ms.assetid: 9a77dd32-d8c2-4961-ad37-2a971f9d6043
caps.latest.revision: 42
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0a114b291767449f4e187c0d3747dac7a77435b8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-2---adding-and-configuring-a-flat-file-connection-manager"></a>Leçon 1-2 : Ajout et configuration d’un gestionnaire de connexions de fichiers plats
Dans cette tâche, vous ajoutez un gestionnaire de connexions de fichiers plats au package que vous venez de créer. Un gestionnaire de connexions de fichiers plats permet à un package d'extraire des données d'un fichier plat. Grâce à ce Gestionnaire, vous pouvez spécifier le nom et l'emplacement du fichier, les paramètres régionaux et la page de codes et enfin, le format du fichier, y compris les séparateurs de colonnes, à appliquer lorsque le package extrait les données du fichier plat. Par ailleurs, vous pouvez spécifier manuellement le type de données pour les colonnes individuelles ou utiliser la boîte de dialogue **Suggérer les types de colonnes** pour mapper automatiquement les colonnes de données extraites aux types de données [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
Vous devez créer un nouveau gestionnaire de connexions de fichiers plats pour chaque format de fichier utilisé. Étant donné que ce didacticiel extrait des données à partir de plusieurs fichiers plats qui ont exactement le même format de données, il vous faudra ajouter et configurer un seul gestionnaire de connexions de fichiers plats uniquement pour votre package.  
  
Pour les besoins de ce didacticiel, vous allez configurer les propriétés suivantes dans votre gestionnaire de connexions de fichiers plats :  
  
-   **Noms des colonnes :** étant donné que le fichier plat ne contient pas de noms de colonnes, le gestionnaire de connexions de fichiers plats crée des noms de colonnes par défaut. Ces noms par défaut ne servent pas à identifier ce que représente chaque colonne. Pour que ces noms par défaut soient plus significatifs, vous devez les remplacer par ceux qui correspondent à la table de faits dans laquelle les données du fichier plat doivent être chargées.  
  
-   **Mappages des données :** les mappages des types de données que vous spécifiez pour le gestionnaire de connexions de fichiers plats sont utilisés par tous les composants des sources de données de fichiers plats qui font référence au Gestionnaire de connexions. Vous pouvez choisir de mapper les types de données manuellement en utilisant le gestionnaire de connexions de fichiers plats ou bien d’utiliser la boîte de dialogue **Suggérer les types de colonnes** . Dans ce didacticiel, vous visualisez les mappages suggérés dans la boîte de dialogue **Suggérer les types de colonnes** , puis vous créez manuellement les mappages nécessaires dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** .  
  
Le gestionnaire de connexions de fichiers plats fournit des informations de paramètres régionaux sur le fichier de données. Si votre ordinateur n’est pas configuré pour l’utilisation des paramètres régionaux Anglais (États-Unis), vous devez définir des propriétés supplémentaires dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** .  
  
### <a name="to-add-a-flat-file-connection-manager-to-the-ssis-package"></a>Pour ajouter un gestionnaire de connexions de fichiers plats au package SSIS  
  
1.  Cliquez avec le bouton droit dans la zone **Gestionnaires de connexions** , puis cliquez sur **Nouvelle connexion de fichier plat**.  
  
2.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , pour **Nom du gestionnaire de connexions**, tapez **Sample Flat File Source Data**.  
  
3.  Cliquez sur **Parcourir**.  
  
4.  Dans la boîte de dialogue **Ouvrir** , accédez au fichier SampleCurrencyData.txt sur votre ordinateur.  
  
    Ces données exemple sont incluses dans les packages de leçons [!INCLUDE[ssIS](../includes/ssis-md.md)] . Pour télécharger ces exemples de données et les packages de leçons, procédez comme suit.  
  
    1.  Accédez à [Exemples de produits Integration Services](http://go.microsoft.com/fwlink/?LinkId=275027)  
  
    2.  Cliquez sur l'onglet **DOWNLOADS** (Téléchargements).  
  
    3.  Cliquez sur le fichier SQL2012.Integration_Services.Create_Simple_ETL_Tutorial.Sample.zip.  
  
5.  Désélectionnez la case à cocher Noms des colonnes dans la première ligne de données.  
  
### <a name="to-set-locale-sensitive-properties"></a>Pour définir des propriétés de paramètres régionaux  
  
1.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , cliquez sur **Général**.  
  
2.  Définissez **Paramètres régionaux** sur Anglais (États-Unis) et **Page de codes** sur 1252.  
  
### <a name="to-rename-columns-in-the-flat-file-connection-manager"></a>Pour renommer les colonnes dans le gestionnaire de connexions de fichiers plats  
  
1.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , cliquez sur **Avancé**.  
  
2.  Dans le volet des propriétés, apportez les modifications suivantes :  
  
    -   Changez la propriété de nom de **Colonne 0** en **AverageRate**.  
  
    -   Changez la propriété de nom de **Colonne 1** en **CurrencyID**.  
  
    -   Changez la propriété de nom de **Colonne 2** en **CurrencyDate**.  
  
    -   Changez la propriété de nom de **Colonne 3** en **EndOfDayRate**.  
  
    > [!NOTE]  
    > Par défaut, ces quatre colonnes ont initialement le type de données chaîne [DT_STR] avec **OutputColumnWidth** égal à 50.  
  
### <a name="to-remap-column-data-types"></a>Pour remapper les types de données des colonnes  
  
1.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , cliquez sur **Suggérer les types**.  
  
    [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggère automatiquement les types de données les plus appropriés en se basant sur les 200 premières lignes de données. Vous pouvez aussi modifier les options de suggestion et augmenter ou réduire l'échantillon de données, spécifier le type de données par défaut pour les entiers ou les données booléennes ou bien ajouter des espaces pour séparer les colonnes de type chaîne.  
  
    Pour l’instant, n’apportez aucune modification aux options de la boîte de dialogue **Suggérer les types de colonnes** et cliquez sur **OK** pour que [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggère des types de données pour les colonnes. Ceci vous ramène au volet **Avancé** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** où vous pouvez afficher les types de données pour les colonnes suggérés par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. (Si vous cliquez sur **Annuler**, aucune suggestion n’est faite pour les métadonnées des colonnes, et le type de données chaîne par défaut (DT_STR) est utilisé.)  
  
    Dans ce didacticiel, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggère les types de données montrées dans la deuxième colonne de la table ci-dessous pour les données issues du fichier SampleCurrencyData.txt. Toutefois, les types de données requis pour les colonnes dans la destination, qui seront définis ultérieurement, sont montrés dans la dernière colonne de la table suivante.  
  
    |Colonne de fichier plat|Type suggéré|Colonne de destination|Type de destination|  
    |--------------------|------------------|----------------------|--------------------|  
    |AverageRate|float [DT_R4]|FactCurrency.AverageRate|FLOAT|  
    |CurrencyID|string [DT_STR]|DimCurrency.CurrencyAlternateKey|nchar(3)|  
    |CurrencyDate|date [DT_DATE]|DimDate.FullDateAlternateKey|Date|  
    |EndOfDayRate|float [DT_R4]|FactCurrency.EndOfDayRate|FLOAT|  
  
    Le type de données suggéré pour la colonne **CurrencyID** n’est pas compatible avec le type de données du champ de la table de destination. Étant donné que le type de données de `DimCurrency.CurrencyAlternateKey` est nchar (3), le type de la colonne **CurrencyID** , chaîne [DT_STR], doit être changé en chaîne [DT_WSTR]. De plus, le champ `DimDate.FullDateAlternateKey` est défini avec le type de données date ; par conséquent, **CurrencyDate** doit être changé de [DT_Date] en date de base de données [DT_DBDATE].  
  
2.  Dans la liste, sélectionnez la colonne CurrencyID et, dans le volet des propriétés, changez le type de données de la colonne **CurrencyID** de chaîne [DT_STR] en chaîne Unicode [DT_WSTR].  
  
3.  Dans le volet des propriétés, changez le type de données de la colonne **CurrencyDate** de date [DT_DATE] en date de base de données [DT_DBDATE].  
  
4.  Cliquez sur **OK**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 3 : ajout et configuration d'un gestionnaire de connexions OLE DB](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
## <a name="see-also"></a> Voir aussi  
[Gestionnaire de connexions de fichiers plats](../integration-services/connection-manager/flat-file-connection-manager.md)  
[Types de données d'Integration Services](../integration-services/data-flow/integration-services-data-types.md)  
  
  
  
