---
title: Validation des données (Complément MDS pour Excel) | Microsoft Docs
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
ms.assetid: 71eda98f-01a4-4fff-8246-be3133782523
caps.latest.revision: 9
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: ab8cf0017260edd45a7538191689cd47198f8a05
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="validating-data-mds-add-in-for-excel"></a>Validation des données (Complément MDS pour Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans le [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], lorsque vous publiez des données, deux types de validation ont lieu :  
  
-   Toutes les règles d'entreprise définies sont appliquées aux données.  
  
-   Les données sont comparées aux valeurs d'attribut autorisées (par exemple, le nombre de caractères ou le type de données).  
  
 Dans chaque cas, les données valides sont publiées dans le référentiel MDS. Les données non valides sont mises en surbrillance et les détails de l'erreur peuvent être affichés dans les colonnes d'état.  
  
## <a name="when-validation-occurs"></a>Lorsque la validation a lieu  
 Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], la validation a lieu lorsque vous publiez des données nouvelles ou modifiées, ou lorsque vous appliquez manuellement des règles d'entreprise.  
  
 Lorsque les règles d'entreprise échouent, les données sont tout de même publiées dans le référentiel MDS. Lorsque la validation d'entrée échoue, les données ne sont pas publiées dans le référentiel.  
  
## <a name="validation-statuses"></a>États de validation  
 Dans le [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], les états de validation suivants sont possibles.  
  
 Pour plus d’informations sur les autres états, consultez [Statuts de validation &#40;Master Data Services&#41;](../../master-data-services/validation-statuses-master-data-services.md)  
  
|État|Description|  
|------------|-----------------|  
|Échec de la validation|Une ou plusieurs valeurs dans la ligne n'ont pas pu être validées par rapport aux règles d'entreprise définies par un administrateur MDS.|  
|Validation réussie|Toutes les valeurs dans la ligne ont réussi la validation par rapport aux règles d'entreprise.|  
  
## <a name="input-statuses"></a>États d'entrée  
 Dans le [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], les états d’entrée suivants sont possibles :  
  
|État|Description|  
|------------|-----------------|  
|Error|Une ou plusieurs valeurs dans la ligne ne répondent pas à la configuration requise, notamment en termes de longueur ou de type de données. La valeur n'est pas mise à jour dans le référentiel MDS.|  
|Nouvelle ligne|Les valeurs dans la ligne n'ont pas encore été publiées dans le référentiel MDS.|  
|Lecture seule|L'utilisateur connecté dispose d'autorisations en lecture seule sur une ou plusieurs valeurs dans la ligne et les valeurs ne peuvent pas être mises à jour.|  
|Inchangé|Aucune valeur dans la ligne n'a été modifiée dans la feuille de calcul. Cela ne signifie pas que les valeurs contenues dans le référentiel n’ont pas changé ; pour obtenir les données les plus récentes dans la feuille, dans le groupe **Se connecter et charger** , cliquez **Charger ou actualiser**.<br /><br /> Il s'agit du paramètre par défaut pour chaque ligne.|  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Déterminez les valeurs qui ne respectent pas les règles d'entreprise définies.|[Appliquer des règles d’entreprise &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
|Pour aider à corriger les erreurs de validation, affichez toutes les transactions qui ont eu lieu pour un membre.|[Afficher toutes les annotations ou transactions pour un membre &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Vue d’ensemble : importation de données à partir d’Excel &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
