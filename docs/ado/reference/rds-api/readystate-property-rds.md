---
title: ReadyState, propriété (RDS) | Documents Microsoft
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
- ReadyState property [ADO]
ms.assetid: 5be75bc7-1171-4440-a37e-c8cc6b5cd865
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1afc0e84acaf044a7271f0d46d599f4422c650b1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="readystate-property-rds"></a>ReadyState, propriété (RDS)
Indique la progression d’un [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) de l’objet lorsqu’il récupère des données dans son [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**adcReadyStateLoaded**|La requête actuelle est en cours d’exécution et aucune ligne n’a été extraite. Le **DataControl** l’objet **Recordset** n’est pas disponible pour utilisation.|  
|**adcReadyStateInteractive**|Un ensemble initial de lignes récupérées par la requête actuelle a été stocké dans le **DataControl** l’objet **Recordset** et peuvent être utilisés. Les lignes restantes sont encore récupérées.|  
|**adcReadyStateComplete**|Toutes les lignes récupérées par la requête en cours ont été stockés dans le **DataControl** l’objet **Recordset** et peuvent être utilisés.<br /><br /> Cet état sera également exister si une opération abandonnée en raison d’une erreur, ou si le **Recordset** objet n’est pas initialisé.|  
  
> [!NOTE]
>  Chaque fichier exécutable côté client qui utilise ces constantes doit fournir les déclarations. Vous pouvez couper et coller les déclarations de constante souhaitées dans le fichier Adcvbs.inc, situé dans le dossier d’installation par défaut pour la bibliothèque de services Bureau à distance.  
  
## <a name="remarks"></a>Notes  
 Utilisez le [onReadyStateChange](../../../ado/reference/rds-api/onreadystatechange-event-rds.md) événement pour surveiller les modifications dans le **ReadyState** propriété pendant une opération de requête asynchrone. Cela est plus efficace que de vérifier régulièrement la valeur de la propriété.  
  
 Si une erreur se produit pendant une opération asynchrone, le **ReadyState** modifications apportées aux propriétés **adcReadyStateComplete**, le [état](../../../ado/reference/ado-api/state-property-ado.md) propriété passe de **adStateExecuting** à **adStateClosed**et le **Recordset** objet [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété reste *rien*.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété ReadyState (VBScript)](../../../ado/reference/rds-api/readystate-property-example-vbscript.md)   
 [Cancel (méthode) (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)   
 [ExecuteOptions, propriété (RDS)](../../../ado/reference/rds-api/executeoptions-property-rds.md)


