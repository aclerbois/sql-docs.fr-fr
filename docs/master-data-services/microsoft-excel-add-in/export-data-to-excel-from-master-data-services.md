---
title: Exporter des données vers Excel à partir de Master Data Services | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: microsoft-excel-add-in
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: dd29389b-928c-4e50-995c-c6af27f97805
caps.latest.revision: 16
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 96451b8d2e1de6c6ba56c1102a748fd920e934ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="export-data-to-excel-from-master-data-services"></a>Export Data to Excel from Master Data Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], vous devez exporter les données à partir du référentiel MDS afin de les manipuler.  
  
 Si vous souhaitez filtrer le dataset avant le chargement, consultez plutôt [Filtrer les données avant l’exportation &#40;complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md).  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** .  
  
### <a name="to-export-data-from-mds-into-excel"></a>Pour exporter des données MDS vers Excel  
  
1.  Ouvrez Excel et accédez à l'onglet **Données de référence** , puis connectez-vous à un référentiel MDS. Pour plus d’informations, consultez [Se connecter à un référentiel MDS &#40;complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md).  
  
2.  Dans le volet **Explorateur de données de référence** , sélectionnez un modèle et une version. La liste des entités est remplie.  
  
    -   Si le volet **Explorateur de données de référence** n'est pas visible, dans le groupe **Se connecter et charger** , cliquez sur **Afficher l'Explorateur**.  
  
    -   Si le volet **Explorateur de données de référence** est désactivé, cela signifie que la feuille de calcul existante contient déjà des données managées MDS. Pour activer le volet, ouvrez une nouvelle feuille de calcul.  
  
3.  Dans le volet **Explorateur de données de référence** , dans la liste des entités, double-cliquez sur l’entité à charger.  
  
    > [!NOTE]  
    >  -   Seul le premier million de membres est chargé dans Excel. Pour filtrer la liste avant le chargement, dans le ruban, dans le groupe **Se connecter et charger** , cliquez sur **Filtre**.  
    > -   Dans les colonnes qui représentent des listes contraintes (attributs basés sur un domaine), seules les 25 000 premières valeurs sont chargées par défaut. Vous pouvez modifier ce nombre dans la propriété MaximumDbaEntitySize du fichier excelusersettings.config situé sur l'ordinateur sur lequel Excel est installé. Ce fichier se trouve sous C:\Users\\<utilisateur\>\AppData\Local\Microsoft\Microsoft SQL Server\130\MasterDataServices\\.  
    >   
    >      Si un attribut basé sur un domaine possède un nombre de valeurs qui dépasse le paramètre de propriété MaximumDbEntitySize, la liste de valeurs ne sera pas chargée.  
  
    > [!NOTE]  
    >  Quand vous chargez des données de texte délimitées à l’aide du complément pour Microsoft Excel avec Excel 32 bits et que les paramètres des propriétés **Nombre de cellules à charger** et **Nombre de cellules à publier** ont une valeur maximale de 1000, une erreur de mémoire insuffisante se produit. Vous devez utiliser Excel 64 bits pour utiliser les paramètres maximaux de **Nombre de cellules à charger** et **Nombre de cellules à publier**.  
  
## <a name="next-steps"></a>Next Steps  
 [Publier des données d’Excel dans Master Data Services &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/import-data-from-excel-to-master-data-services-mds-add-in-for-excel.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble : exportation de données vers Excel &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)   
 [Boîte de dialogue Filtrer &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-dialog-box-mds-add-in-for-excel.md)   
 [Vue d’ensemble : importation de données à partir d’Excel &#40;complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
