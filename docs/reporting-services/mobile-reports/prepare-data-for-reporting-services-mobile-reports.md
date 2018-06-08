---
title: Préparer des données pour les rapports mobiles Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 02/08/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: mobile-reports
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 8adce9ad-6a08-4d20-b1cf-d3c45544d8de
caps.latest.revision: 15
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 731b28c5f2f269d9527a7ba846beec747680ffe5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepare-data-for-reporting-services-mobile-reports"></a>Prepare data for Reporting Services mobile reports
  
[!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-long.md)] prend en charge un nombre d’opérations de données complexes, dont le filtrage, l’agrégation et le découpage temporel. Cet article présente certains points à prendre en compte lors de la préparation des données. La pré-agrégation des données peut optimiser la création et l’utilisation de rapports mobiles. Certains modèles de rapport mobile l’exigent même.   
  
## <a name="date-and-time-formats"></a>Formats de date et d’heure 
Lorsque vous traitez des intervalles de date et d’heure en vue d’une utilisation dans un rapport mobile, en particulier avec TimeNavigator, il est important de formater correctement la colonne date/heure afin que l’ [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] puisse l’identifier en tant que tel. Voici quelques exemples de formats d’horodatage valides :  
  
    05/01/2009    
    2009-05-01    
    05/01/2009 14:57:32.8    
    2009-05-01 14:57:32.8    
    2009-05-01T14:57:32.8375298-04:00    
    5/01/2008 14:57:32.80 -07:00    
    1 May 2008 2:57:32.8 PM    
    Fri, 15 May 2009 20:10:57 GMT    
  
La plupart du temps, les jeux de données horodatées peuvent être décrits par un ou plusieurs intervalles de temps (horaire, quotidien, mensuel, trimestriel et annuel). [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] peut combiner plusieurs tables de granularité différente et les afficher dans le même rapport mobile. Toutefois, gardez à l’esprit les intervalles appropriés au(x) jeu(x) de données d’origine, car ils permettent de décider quelles options de filtre d’horodatage afficher à l’utilisateur dans le rapport mobile final.  

Les champs de date [!INCLUDE[ssASnoversion_md](../../includes/ssasnoversion-md.md)] dans les modèles multidimensionnels et tabulaires peuvent perdre leur mise en forme de date dans les datasets partagés. Pour une solution qui préserve leur mise en forme, consultez [Conserver la mise en forme des dates pour Analysis Services dans les rapports mobiles](../../reporting-services/mobile-reports/retain-date-formatting-for-analysis-services-in-mobile-reports.md) .
  
## <a name="preparing-filter-data"></a>Préparer les données du filtre ##  
[!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] peut filtrer les données en fonction des champs d’horodatage et des champs clés. Les champs clés peuvent être numériques, mais dans la plupart des cas, ils contiennent un ID ou une valeur de type chaîne. Pour préparer un champ de filtre à utiliser avec un élément de navigateur tel qu’une liste de sélection, la clé du filtre doit être une colonne unique de la table de données. Ainsi, vous pouvez regrouper les lignes de la table en fonction de la valeur dans la colonne du filtre. Le fait que plusieurs colonnes contiennent différentes clés de filtre (ou critères de filtrage) permet d’utiliser des rapports mobiles avec plusieurs navigateurs de filtre ensemble hiérarchique ou individuellement.  
  
| Secteur  | Pays   | Région    |  
| ------------- | ------------- | ------------- |  
| Banques     | AFGHANISTAN   | ASIE      |  
| Services commerciaux et professionnels | AFGHANISTAN | ASIE |  
| Alimentation, boissons et tabac | AFGHANISTAN | ASIE |  
| Support | AFGHANISTAN | ASIE |  
| Produits pharmaceutiques | AFGHANISTAN | ASIE |  
| Vente au détail de produits alimentaires et de produits de première nécessité | ALBANIE | EUROPE |  
  
  
Les filtres basés sur des clés peuvent également être structurés hiérarchiquement en vue d’une utilisation avec une liste de sélection dans une structure arborescente. Pour préparer les données à une utilisation dans ce type de scénario, créez une table de recherche décrivant la structure hiérarchique. Utilisez une structure de table qui comprend une colonne Key et une colonne ParentKey pour indiquer où un nœud se trouve dans la hiérarchie globale.  
  
Dans cette table, les éléments ParentKey sont d’abord répertoriés dans la colonne ItemKey, puis dans la colonne ParentItemKey en regard de leurs éléments enfants.   
  
|ItemKey    | ParentItemKey |  
| ------------- | ------------- |  
| Finance    |   |  
| Industrie   |   |  
| Biens de consommation de première nécessité |    |  
| Biens de consommation non essentiels |  |     
| Soins de santé   |   |  
| Technologies de l’information |  |  
| Banques | Finance |  
| Immobilier | Finance |  
| Finance - Divers |  Finance |   
| Assurance |   Finance |  
| Services commerciaux et professionnels |  Industrie |  
| Biens d’équipement |   Industrie |  
| Transport |  Industrie |  
| Alimentation, boissons et tabac |    Biens de consommation de première nécessité |  
| Vente au détail de produits alimentaires et de produits de première nécessité |    Biens de consommation de première nécessité |  
| Produits ménagers et personnels | Biens de consommation de première nécessité |  
| Support | Biens de consommation non essentiels |  
| Automobiles et pièces |  Biens de consommation non essentiels |  
| Biens de consommation durables et habillement |Biens de consommation non essentiels |  
| Services aux consommateurs |   Biens de consommation non essentiels |  
| Vente au détail | Biens de consommation non essentiels |  
| Produits pharmaceutiques   | Soins de santé |  
| Équipements et services de santé |    Soins de santé |  
| Logiciels et services | Technologies de l’information |  
| Équipements et matériels technologiques   | Technologies de l’information |  
| Services de télécommunication |Technologies de l’information |  
  
### <a name="see-also"></a>Voir aussi  
- [Préparer les données Excel pour les rapports mobiles Reporting Services](../../reporting-services/mobile-reports/prepare-excel-data-for-reporting-services-mobile-reports.md)  
- [Conserver la mise en forme des dates pour Analysis Services dans les rapports mobiles](../../reporting-services/mobile-reports/retain-date-formatting-for-analysis-services-in-mobile-reports.md)
- [Créer des rapports mobiles avec l’Éditeur de rapports mobiles SQL Server](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)
  
  
  

