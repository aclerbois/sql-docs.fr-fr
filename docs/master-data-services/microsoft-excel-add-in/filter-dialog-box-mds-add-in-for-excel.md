---
title: Boîte de dialogue Filtrer (Complément MDS pour Excel) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.component: microsoft-excel-add-in
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b987b141-5abf-4161-a073-4cfc3e7f5aae
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: e775f606b81fd849f7089f15116d4bd0f4c514d4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filter-dialog-box-mds-add-in-for-excel"></a>Boîte de dialogue Filtrer (Complément MDS pour Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], utilisez la boîte de dialogue **Filtre** pour limiter la liste des données managées MDS avant de les charger dans Excel.  
  
 Cette boîte de dialogue contient trois sections : **Colonnes**, **Lignes**et **Résumé**.  
  
## <a name="columns"></a>Colonnes  
 Utilisez la section **Colonnes** pour déterminer les attributs (colonnes) que vous souhaitez afficher dans Excel.  
  
|Nom du contrôle|Description|  
|------------------|-----------------|  
|Type d'attribut|Un type d'attribut décrit le type de membre que vous souhaitez utiliser. La plupart de temps, il s’agit d’un membre **feuille**. Pour plus d’informations sur les types de membres, consultez [Membres &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).|  
|Hiérarchie explicite|Si vous avez choisi le type d’attribut **Consolidé**, choisissez la hiérarchie à laquelle appartiennent les membres consolidés. Pour plus d’informations, consultez [Hiérarchies explicites &#40;Master Data Services&#41;](../../master-data-services/explicit-hierarchies-master-data-services.md).|  
|Groupes d'attributs|Les groupes d'attributs permettent de regrouper des sous-ensembles d'attributs. Choisissez un groupe d'attributs si vous souhaitez afficher un sous-ensemble d'attributs disponibles. Pour plus d’informations sur les groupes d’attributs, consultez [Groupes d’attributs &#40;Master Data Services&#41;](../../master-data-services/attribute-groups-master-data-services.md).|  
|Tout sélectionner|Cliquez pour sélectionner tous les attributs affichés dans la liste.|  
|Effacer tout|Cliquez pour effacer les attributs sélectionnés affichés dans la liste.<br /><br /> Vous ne pouvez pas effacer **Nom** et **Code**.|  
|Flèche haut/Flèche bas|Cliquez pour déplacer l’attribut sélectionné vers le haut et le bas dans la liste. L'ordre de haut en bas correspond à l'ordre de gauche à droite selon lequel les colonnes sont affichées dans la feuille de calcul.|  
  
## <a name="rows"></a>Lignes  
 Utilisez la section **Lignes** pour déterminer les membres (lignes) que vous souhaitez afficher dans Excel. Vous effectuez cette opération en définissant des critères pour filtrer les lignes qui seront affichées.  
  
|Nom du contrôle|Description|  
|------------------|-----------------|  
|Attribute|Affiche l'attribut utilisé pour filtrer. Si aucun attribut n'est répertorié, les attributs n'ont pas été ajoutés.<br /><br /> Remarque : vous pouvez filtrer à l’aide d’attributs que vous n’envisagez pas d’afficher dans la feuille de calcul.|  
|Opérateur|Affiche les opérateurs qui correspondent au type d'attribut sélectionné. Pour plus d’informations, consultez [Opérateurs de filtre &#40;Master Data Services&#41;](../../master-data-services/filter-operators-master-data-services.md).|  
|Critères|Critères qui doivent servir de filtre.|  
|Résumé de la mise à jour|Si vous utilisez de grands datasets, cliquez pour mettre à jour la section **Résumé** avec les informations relatives à la quantité de données qui seront chargées.|  
|Ajouter|Quand vous cliquez sur un attribut dans la section **Colonnes** , puis sur **Ajouter**, un attribut est ajouté à la liste de filtres.|  
|Supprimer tout|Supprime tous les filtres de la liste.|  
|Supprimer|Supprime de la liste le filtre sélectionné.|  
  
## <a name="summary"></a>Résumé  
 Utilisez la section **Résumé** pour afficher des informations sur la quantité de données qui seront chargées, avant de les charger.  
  
|Nom du contrôle|Description|  
|------------------|-----------------|  
|Modèle|Nom du modèle.|  
|Options de version|Nom de la version.|  
|Entité|Nom de l'entité.|  
|Lignes|Nombre de lignes qui seront chargées dans Excel, en fonction des filtres appliqués dans la section **Lignes** .|  
|Colonnes|Nombre de colonnes qui seront chargées dans Excel, en fonction des attributs sélectionnés dans la section **Colonnes** .|  
  
## <a name="see-also"></a> Voir aussi  
 [Filtrer les données avant l’exportation &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/filter-data-before-exporting-mds-add-in-for-excel.md)   
 [Vue d’ensemble : exportation de données vers Excel &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
  
