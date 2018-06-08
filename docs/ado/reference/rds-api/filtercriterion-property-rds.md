---
title: FilterCriterion, propriété (RDS) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- FilterCriterion property [RDS]
ms.assetid: 24eb03ba-ccfd-4353-b6af-03586b2da6fd
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: adc1d9d22b94ab3b6e03bddf37fa6058f4ccaca3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filtercriterion-property-rds"></a>FilterCriterion, propriété (RDS)
Indique l’opérateur d’évaluation à utiliser dans la valeur de filtre.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.FilterCriterion = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Une variable objet qui représente un [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet.  
  
 *Chaîne*  
 A **chaîne** valeur qui spécifie l’opérateur d’évaluation de la [FilterValue](../../../ado/reference/rds-api/filtervalue-property-rds.md) les enregistrements. Peut prendre l’une des opérations suivantes : <, \<=, >, > =, =, ou <>.  
  
## <a name="remarks"></a>Notes  
 Le [SortColumn](../../../ado/reference/rds-api/sortcolumn-property-rds.md), [SortDirection](../../../ado/reference/rds-api/sortdirection-property-rds.md), [FilterValue](../../../ado/reference/rds-api/filtervalue-property-rds.md), **FilterCriterion**, et [FilterColumn](../../../ado/reference/rds-api/filtercolumn-property-rds.md) fournissent des propriétés de tri et de filtrage des fonctionnalités sur le cache côté client. La fonctionnalité de tri organise les enregistrements par les valeurs d’une colonne. La fonctionnalité de filtrage affiche un sous-ensemble des enregistrements selon des critères de recherche, lors de la version complète [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) est conservé dans le cache. Le [réinitialiser](../../../ado/reference/rds-api/reset-method-rds.md) méthode exécute les critères et remplace l’actuel **Recordset** avec un texte modifiable **Recordset**.  
  
 Le « ! = » opérateur n’est pas valide pour **FilterCriterion**; au lieu de cela, utilisez « <> ».  
  
 Si les propriétés de filtre et de tri sont définies et que vous appelez le **réinitialiser** (méthode), l’ensemble de lignes est tout d’abord filtré, puis trié. Pour les tris croissants, les valeurs null sont en haut ; pour les tris décroissants, les valeurs null sont en bas (par ordre croissant est le comportement par défaut).  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [FilterColumn, FilterCriterion, FilterValue, SortColumn et SortDirection, propriétés et Reset, méthode-exemple (VBScript)](../../../ado/reference/rds-api/filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn, propriété (RDS)](../../../ado/reference/rds-api/filtercolumn-property-rds.md)   
 [FilterValue, propriété (RDS)](../../../ado/reference/rds-api/filtervalue-property-rds.md)   
 [SortColumn, propriété (RDS)](../../../ado/reference/rds-api/sortcolumn-property-rds.md)   
 [SortDirection, propriété (RDS)](../../../ado/reference/rds-api/sortdirection-property-rds.md)


