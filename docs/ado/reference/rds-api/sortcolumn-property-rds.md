---
title: SortColumn, propriété (RDS) | Documents Microsoft
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: sql
ms.prod_service: drivers
ms.component: reference
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- SortColumn property [RDS]
ms.assetid: f6f80f67-f0fb-4e63-a5f5-8fdf312aac63
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fae3b712f32ad2b3b5c9f478a600af611379e798
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sortcolumn-property-rds"></a>SortColumn, propriété (RDS)
Indique la colonne selon laquelle trier les enregistrements.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.SortColumn = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Une variable objet qui représente un [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet.  
  
 *Chaîne*  
 A **chaîne** valeur qui représente le nom ou alias de la colonne selon laquelle trier les enregistrements.  
  
## <a name="remarks"></a>Notes  
 Le **SortColumn**, [SortDirection](../../../ado/reference/rds-api/sortdirection-property-rds.md), [FilterValue](../../../ado/reference/rds-api/filtervalue-property-rds.md), [FilterCriterion](../../../ado/reference/rds-api/filtercriterion-property-rds.md), et [FilterColumn](../../../ado/reference/rds-api/filtercolumn-property-rds.md) fournissent des propriétés de tri et de filtrage des fonctionnalités sur le cache côté client. La fonctionnalité de tri organise les enregistrements par les valeurs d’une colonne. La fonctionnalité de filtrage affiche un sous-ensemble des enregistrements selon des critères de recherche, lors de la version complète [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) est conservé dans le cache. Le [réinitialiser](../../../ado/reference/rds-api/reset-method-rds.md) méthode exécute les critères et remplace l’actuel **Recordset** avec un texte modifiable **Recordset**.  
  
 Pour effectuer un tri sur un **Recordset**, vous devez tout d’abord enregistrer les modifications en attente. Si vous utilisez la **RDS. DataControl**, vous pouvez utiliser la [SubmitChanges](../../../ado/reference/rds-api/submitchanges-method-rds.md) (méthode). Par exemple, si votre **RDS. DataControl** est nommé ADC1, votre code sera `ADC1.SubmitChanges`. Si vous utilisez ADO **Recordset**, vous pouvez utiliser ses [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) (méthode). À l’aide de **UpdateBatch** est la méthode recommandée pour **Recordset** les objets créés avec le [CreateRecordset](../../../ado/reference/rds-api/createrecordset-method-rds.md) (méthode). Par exemple, votre code peut être `myRS.UpdateBatch` ou `ADC1.Recordset.UpdateBatch`.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [FilterColumn, FilterCriterion, FilterValue, SortColumn et SortDirection, propriétés et Reset, méthode-exemple (VBScript)](../../../ado/reference/rds-api/filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Propriété de tri](../../../ado/reference/ado-api/sort-property.md)   
 [SortDirection, propriété (RDS)](../../../ado/reference/rds-api/sortdirection-property-rds.md)





