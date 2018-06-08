---
title: ExecuteOptions, propriété (RDS) | Documents Microsoft
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
- ExecuteOptions property [ADO], VBScript example
ms.assetid: 62a4fd88-afc3-4f1f-b978-40710a30c4e9
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e44be9cb2b46b91d536d5a90cbb589365ad7d2c7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executeoptions-property-rds"></a>ExecuteOptions, propriété (RDS)
Indique si l’exécution asynchrone est activée.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne l’une des valeurs suivantes.  
  
|Constante| Description|  
|--------------|-----------------|  
|**adcExecSync**|Exécute l’actualisation suivante de la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) synchrone.|  
|**adcExecAsync**|Valeur par défaut. Exécute l’actualisation suivante de la **Recordset** en mode asynchrone.|  
  
> [!NOTE]
>  Chaque fichier exécutable qui utilise ces constantes doit fournir les déclarations. Vous pouvez couper et coller les déclarations de constante dans le fichier Adcvbs.inc, situé dans le dossier d’installation par défaut pour la bibliothèque de services Bureau à distance.  
  
## <a name="remarks"></a>Notes  
 Si **ExecuteOptions** a la valeur **adcExecAsync**, puis il exécute de manière asynchrone la prochaine **Actualiser** appeler sur le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) l’objet **Recordset**.  
  
 Si vous essayez d’appeler [réinitialiser](../../../ado/reference/rds-api/reset-method-rds.md), [Actualiser](../../../ado/reference/rds-api/refresh-method-rds.md), [SubmitChanges](../../../ado/reference/rds-api/submitchanges-method-rds.md), [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md), ou [Recordset](../../../ado/reference/rds-api/recordset-sourcerecordset-properties-rds.md) lors d’une autre opération asynchrone susceptible de modifier le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) l’objet **Recordset** s’exécute, une erreur se produit.  
  
 Si une erreur se produit pendant une opération asynchrone, le **RDS. DataControl** l’objet [ReadyState](../../../ado/reference/rds-api/readystate-property-rds.md) valeur change à partir de **adcReadyStateLoaded** à **adcReadyStateComplete**et le **Recordset** reste de la valeur de la propriété *rien*.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ExecuteOptions et FetchOptions, propriétés-exemple (VBScript)](../../../ado/reference/rds-api/executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Cancel, méthode (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)


