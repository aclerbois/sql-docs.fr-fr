---
title: Combiner des données (Complément MDS pour Excel) | Microsoft Docs
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
ms.assetid: a867dc15-5a0d-457c-8304-ac323bcf9377
caps.latest.revision: 6
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 0accd3ed6823e0a3614f419c79148b84f86f92b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="combine-data-mds-add-in-for-excel"></a>Combiner des données (Complément MDS pour Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], combinez les données de deux feuilles de calcul lorsque vous souhaitez comparer les données avant la publication. Dans cette procédure, vous combinez les données de deux feuilles de calcul en une seule. Vous pouvez ensuite réaliser d'autres comparaisons et déterminer les données, le cas échéant, à publier dans le référentiel MDS.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
  
-   Vous devez disposer d'une feuille de calcul qui contient des données gérées par MDS. Pour plus d’informations, consultez [Exporter des données dans Excel depuis Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md).  
  
-   Vous devez disposer d'une feuille de calcul qui contient les données que vous souhaitez combiner avec les données managées MDS. Cette feuille doit avoir une ligne d'en-tête.  
  
### <a name="to-combine-non-managed-data-into-an-mds-managed-sheet"></a>Pour combiner des données non managées dans une feuille managée MDS  
  
1.  Sur la feuille qui contient les données managées MDS, dans le groupe **Publier et valider** , cliquez sur **Combiner des données**.  
  
2.  Dans la boîte de dialogue **Combiner des données** , en regard de la zone de texte **Plage à combiner avec les données MDS** , cliquez sur l'icône. La boîte de dialogue se réduit.  
  
3.  Cliquez sur la feuille contenant les données que vous souhaitez combiner.  
  
4.  Mettez en surbrillance toutes les cellules de la feuille que vous souhaitez combiner, notamment la ligne d'en-tête.  
  
5.  Dans la boîte de dialogue **Combiner des données** , cliquez sur l'icône. La boîte de dialogue se développe.  
  
6.  Pour une colonne répertoriée pour l'entité MDS, sélectionnez une colonne sous **Colonne correspondante**. Toutes les colonnes MDS n'ont pas besoin de colonnes correspondantes.  
  
7.  Cliquez sur **Combiner**. Une colonne **SOURCE** s'affiche, laquelle indique si les données proviennent de MDS ou d'une source externe.  
  
## <a name="next-steps"></a>Next Steps  
  
-   Pour rechercher des similitudes entre les données managées MDS et les données externes, consultez [Mettre en correspondance les données similaires &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/match-similar-data-mds-add-in-for-excel.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble : exportation de données vers Excel &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)   
 [Mise en correspondance de la qualité des données dans le complément MDS pour Excel](../../master-data-services/microsoft-excel-add-in/data-quality-matching-in-the-mds-add-in-for-excel.md)  
  
  
