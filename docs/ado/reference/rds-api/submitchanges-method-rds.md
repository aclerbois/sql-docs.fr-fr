---
title: SubmitChanges, méthode (RDS) | Documents Microsoft
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
helpviewer_keywords:
- SubmitChanges method [ADO]
ms.assetid: 250062a4-13c4-4bed-807d-8b9ad81536d4
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6c4f2b3a0cf70d068e1f6a7124e7152c8329ebd1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="submitchanges-method-rds"></a>SubmitChanges, méthode (RDS)
Soumet les modifications en attente de localement mis en cache et être mise à jour [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) à la source de données spécifiée dans le [Connect](../../../ado/reference/rds-api/connect-property-rds.md) propriété ou le [URL](../../../ado/reference/rds-api/url-property-rds.md) propriété.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.SubmitChanges DataFactory.SubmitChanges Connection, Recordset  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Une variable objet qui représente un [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet.  
  
 *DataFactory*  
 Une variable objet qui représente un [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet.  
  
 *Connexion*  
 A **chaîne** valeur qui représente la connexion créée avec la **RDS. DataControl** l’objet [Connect](../../../ado/reference/rds-api/connect-property-rds.md) propriété.  
  
 *Recordset*  
 Une variable objet qui représente un **Recordset** objet.  
  
## <a name="remarks"></a>Notes  
 Le [Connect](../../../ado/reference/rds-api/connect-property-rds.md), [Server](../../../ado/reference/rds-api/server-property-rds.md), et [SQL](../../../ado/reference/rds-api/sql-property.md) propriétés doivent être définies avant de pouvoir utiliser le **SubmitChanges** méthode avec le **RDS. DataControl** objet.  
  
 Si vous appelez le [CancelUpdate](../../../ado/reference/rds-api/cancelupdate-method-rds.md) méthode après avoir appelé **SubmitChanges** pour le même **Recordset** objet, le **CancelUpdate** appel échoue, car les modifications ont déjà été validées.  
  
 Uniquement les enregistrements modifiés sont envoyés pour modification et toutes les modifications soit réussissent ou échouent de toutes les modifications de l’ensemble.  
  
 Vous pouvez utiliser **SubmitChanges** uniquement avec la valeur par défaut **RDSServer.DataFactory** objet. Objets métier personnalisés ne peuvent pas utiliser cette méthode.  
  
 Si le **URL** propriété a été définie, **SubmitChanges** soumettra des modifications à l’emplacement spécifié par l’URL.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)|[DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode SubmitChanges (VBScript)](../../../ado/reference/rds-api/submitchanges-method-example-vbscript.md)   
 [Boutons de commande de carnet d’adresses](../../../ado/guide/remote-data-service/address-book-command-buttons.md)   
 [CancelUpdate, méthode (RDS)](../../../ado/reference/rds-api/cancelupdate-method-rds.md)   
 [Refresh, méthode (RDS)](../../../ado/reference/rds-api/refresh-method-rds.md)



