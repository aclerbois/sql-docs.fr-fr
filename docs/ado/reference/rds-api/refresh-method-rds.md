---
title: Refresh, méthode (RDS) | Documents Microsoft
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: sql
ms.prod_service: drivers
ms.component: reference
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Refresh
- RDS.DataControl::Refresh
- DataControl::Refresh
helpviewer_keywords:
- Refresh method [RDS]
ms.assetid: c90a8050-0ff4-4c83-9925-261f2f2ccfe9
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e86c8965180eb6cec2fda86a2c5141d0c3e6fa75
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="refresh-method-rds"></a>Refresh, méthode (RDS)
Actualise la source de données spécifiée dans le [Connect](../../../ado/reference/rds-api/connect-property-rds.md) mises à jour les résultats de requête et la propriété.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.Refresh  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Une variable objet qui représente un [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet.  
  
## <a name="remarks"></a>Notes  
 Vous devez définir le [Connect](../../../ado/reference/rds-api/connect-property-rds.md), [Server](../../../ado/reference/rds-api/server-property-rds.md), et [SQL](../../../ado/reference/rds-api/sql-property.md) propriétés avant d’utiliser le **Actualiser** (méthode). Tous les contrôles liés aux données sur le formulaire associé à un **RDS. DataControl** objet reflète le nouveau jeu d’enregistrements. Toutes les existantes [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet est libéré et les modifications non enregistrées sont ignorées. Le **Actualiser** méthode fait automatiquement du premier enregistrement l’enregistrement actif.  
  
 Il est judicieux d’appeler le **Actualiser** méthode régulièrement lorsque vous travaillez avec des données. Si vous récupérez des données et laissez sur un ordinateur client pendant un certain temps, il est susceptible de devenir obsolètes. Il est possible que les modifications que vous apportez échouera, car une autre personne peut avoir modifié l’enregistrement valide les modifications avant vous.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Actualiser l’exemple de méthode (VB)](../../../ado/reference/ado-api/refresh-method-example-vb.md)   
 [Refresh, méthode-exemple (VBScript)](../../../ado/reference/rds-api/refresh-method-example-vbscript.md)   
 [Boutons de commande de carnet d’adresses](../../../ado/guide/remote-data-service/address-book-command-buttons.md)   
 [CancelUpdate, méthode (RDS)](../../../ado/reference/rds-api/cancelupdate-method-rds.md)   
 [Refresh, méthode (ADO)](../../../ado/reference/ado-api/refresh-method-ado.md)   
 [SubmitChanges, méthode (RDS)](../../../ado/reference/rds-api/submitchanges-method-rds.md)


