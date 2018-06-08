---
title: Vérifier le mappage de type de données (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.reviewissues.f1
ms.assetid: 0625c4f9-b8ff-4593-b884-39398b9d43af
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6552bb3f8b4294fed3a14eaaf71dab1575380dad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="review-data-type-mapping-sql-server-import-and-export-wizard"></a>Vérifier le mappage de type de données (Assistant Importation et Exportation SQL Server)
Si vous avez spécifié un mappage de type de données qui risque d’échouer dans la liste **Mappages** de la boîte de dialogue **Mappages de colonnes** , l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche la page **Vérifier le mappage de type de données** . Dans cette page, vous passez en revue des informations détaillées sur les conversions de types de données que l’Assistant doit effectuer pour que les données sources soient compatibles avec la destination. Ces informations incluent des aides visuelles permettant de différencier les conversions de type de données qui sont censées aboutir de celles qui sont susceptibles de provoquer des erreurs ou des troncations. Pour chaque conversion, vous choisissez d’accepter, ou non, la conversion que l’Assistant suggère. De plus, vous spécifiez comment gérer les éventuelles erreurs.   
  
> [!TIP]
> Vous ne pouvez pas modifier les mappages de types de données dans la page **Vérifier le mappage de type de données** . Toutefois, vous pouvez cliquer sur **Précédent** pour revenir à la page **Sélectionner les tables et les vues sources** , puis cliquez sur **Modifier les mappages** pour ouvrir la boîte de dialogue **Mappage de colonnes** . Dans la boîte de dialogue **Mappages de colonnes** , vous pouvez spécifier les mappages de types de données qui sont les plus susceptibles d’aboutir. Pour examiner davantage la boîte de dialogue **Mappages de colonnes** , consultez [Mappages de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md).  
  
## <a name="screen-shot-of-the-review-data-type-mapping-page"></a>Capture d’écran de la page Vérifier le mappage de type de données
 La capture d’écran suivante montre un exemple de la page **Vérifier le mappage de type de données** de l’Assistant.
 
 Dans cet exemple :
 -   L’utilisateur a spécifié un mappage dans la boîte de dialogue **Mappage de colonnes** qui ne peut pas réussir.
 -   L’icône d’avertissement sur la ligne dans la liste **Table** indique que la conversion d’au moins une colonne de données à partir des résultats de la requête vers un type de données compatibles dans la table de destination pose problème.
 -   L’icône d’avertissement dans la première ligne de la liste **Mappage de type de données** indique que le mappage depuis le type de données **int** de la colonne source vers le type de données **smalldatetime** de la colonne de destination peut provoquer une perte de données.
 
 ![Page Vérifier le mappage de type de données de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/review-mapping.png "Page Vérifier le mappage de type de données de l’Assistant Importation et Exportation") 
 
## <a name="review-the-source-and-destination-tables"></a>Vérifier les tables source et de destination  
 La section supérieure de la page **Vérifier le mappage de type de données** est une liste **Table** qui répertorie les tables à copier de la source vers la destination. Pour consulter les informations de conversion d’une table spécifique, sélectionnez une table dans la liste **Table** . Les informations de conversion des colonnes individuelles de la table sélectionnée apparaissent dans la partie inférieure de la page, dans la grille **Mappage de type de données** .

Dans cet exemple, les résultats de la requête fournie par l’utilisateur vont être copiés dans la table Sales.CustomerNew2 de la destination. L’icône d’avertissement indique que la conversion d’au moins une colonne de données, à partir des résultats de la requête vers un type de données compatible dans la table de destination, pose problème.

![Vérifier le mappage - tables](../../integration-services/import-export-data/media/review-mapping-tables.png)
  
 Le tableau suivant décrit les colonnes de la liste **Table** .  
  
|colonne|Description|  
|------------|-----------------|  
|(Icône Source)|Indique la probabilité de réussite des conversions de types de données :<br /> -   Une icône représentant une coche **verte** indique que l’Assistant prévoit la réussite de toutes les conversions de types de données de cette table.<br />-   Une icône d’avertissement **jaune** indique que vous devez vérifier les différentes conversions qui seront effectuées par l’Assistant. Pour ce faire, sélectionnez la table, puis examinez les conversions correspondant aux différentes colonnes dans la liste **Mappage de type de données** .<br />-   Une icône d’erreur **rouge** indique que l’Assistant n’est pas en mesure d’effectuer de manière fiable certaines conversions pour cette table.|  
|**Source**|Nom de la table source.|  
|(Icône Destination)|Indique si la destination existe déjà ou si elle doit être créée par l'Assistant :<br /> -   Une icône de table indique que la destination est une table existante.<br />-   Une icône de table comportant des rayons de soleil indique que la destination est une nouvelle table qui sera créée par l’Assistant.|  
|**Destination**|Nom de la table de destination|  
  
## <a name="review-the-data-type-mappings"></a>Vérifier le mappage de type de données  
 La section centrale de la page **Vérifier le mappage de type de données** contient la liste **Mappage de type de données** . Cette grille donne des informations détaillées sur la conversion des colonnes de la table source sélectionnée à partir de la liste **Table** dans la partie supérieure de la page.

Dans cet exemple, chaque colonne de la source va être copiée dans une colonne de la destination ayant le même nom et le même type de données. L’icône d’avertissement dans la première ligne de la liste **Mappage de type de données** indique que le mappage depuis le type de données **int** de la colonne source vers le type de données **smalldatetime** de la colonne de destination peut provoquer une perte de données.
 
![Vérifier le mappage - mappages](../../integration-services/import-export-data/media/review-mapping-mappings.png)  

Le tableau suivant décrit les colonnes de la liste **Mappage de type de données** . 

|colonne|Description|  
|------------|-----------------|  
|(Icône Conversion)|Indique la probabilité de réussite des conversions de types de données :<br /> -   Une icône représentant une coche **verte** indique que l’Assistant prévoit la réussite de la conversion des types de données de cette colonne.<br />-   Une icône d’avertissement **jaune** indique que vous devez examiner la conversion qui sera effectuée par l’Assistant. Pour vérifier la conversion, double-cliquez sur la colonne afin d’afficher la boîte de dialogue **Détails de la conversion de colonne** . Pour plus d’informations, consultez [Boîte de dialogue Détails de la conversion de colonne](../../integration-services/import-export-data/column-conversion-details-dialog-box-sql-server-import-and-export-wizard.md).<br />-   Une icône d’erreur **rouge** indique que l’Assistant n’est pas en mesure d’effectuer la conversion de manière fiable.|  
|**Colonne source**|Nom de la colonne source.|  
|**Type de source**|Type de données de la colonne source.|  
|**Colonne de destination**|Nom de la colonne de destination.|  
|**Type de destination**|Type de données de la colonne de destination.|  
|**Convertir**|Spécifiez s’il faut poursuivre la conversion planifiée :<br /> - Cochez la case pour que l’Assistant poursuive la conversion planifiée.<br />- Décochez la case pour annuler la conversion des types de données.|  
|**En cas d’erreur**|Spécifiez comment l'Assistant gère les erreurs :<br /> -   Utiliser le paramètre **En cas d’erreur (global)** , que vous pouvez spécifier en bas de cette page.<br />-   Échouer avec une erreur et arrêter le processus d’importation ou d’exportation.<br />-   Ignorer l’erreur.|  
|**En cas de troncation**|Spécifiez comment l'Assistant gère les troncations :<br /> -   Utiliser le paramètre **En cas de troncation (global)** , que vous pouvez spécifier en bas de cette page.<br />-   Échouer avec une erreur et arrêter le processus d’importation ou d’exportation.<br />-   Ignorer la troncation.|  

> [!TIP]
> Pour obtenir des informations détaillées sur la conversion d’une colonne de données spécifique, double-cliquez sur une ligne dans la liste. La boîte de dialogue **Détails de la conversion de colonne** s'ouvre et affiche des informations de conversion plus détaillées pour la colonne. Pour plus d’informations, consultez [Boîte de dialogue Détails de la conversion de colonne](../../integration-services/import-export-data/column-conversion-details-dialog-box-sql-server-import-and-export-wizard.md).
 
## <a name="specify-global-error-handling-options"></a>Spécifier les options de gestion des erreurs globales  
 La section inférieure de la page **Vérifier le mappage de type de données** vous permet de spécifier des options de gestion des erreurs qui s’appliquent par défaut à toutes les colonnes. Ces paramètres s’appliquent à toutes les conversions pour lesquelles l’option **Utiliser Global** est sélectionnée dans la colonne **En cas d’erreur** ou **En cas de troncation** de la liste **Mappage de types de données** .   

Cet exemple montre les valeurs par défaut pour les deux options globales de gestion des erreurs.

![Vérifier le mappage - erreurs](../../integration-services/import-export-data/media/review-mapping-errors.png)

 **En cas d’erreur (global)**  
 Spécifiez comment l'Assistant gère les erreurs :  
 -   Échouez avec une erreur et arrêtez le processus d'importation ou d'exportation. Il s'agit de la valeur par défaut.
 -   Ignorez l'erreur et poursuivez le processus d'importation ou d'exportation.  
  
 **En cas de troncation (global)**  
 Spécifiez comment l’Assistant gère les troncations de données :  
 -   Échouez avec une erreur et arrêtez le processus d'importation ou d'exportation. Il s'agit de la valeur par défaut.
 -   Ignorez la troncation et poursuivez le processus d'importation ou d'exportation.  
   
## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Une fois que vous avez consulté les avertissements, spécifié les options de conversion et indiqué la façon de gérer les erreurs, la page **Vérifier le mappage de type de données** vous fait revenir à la boîte de dialogue **Mappage de colonnes** . Pour plus d’informations, consultez [Mappages de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md).  
 
 ## <a name="see-also"></a>Voir aussi
[Mappage de type de données dans l’Assistant Importation et Exportation SQL Server](../../integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard.md)

