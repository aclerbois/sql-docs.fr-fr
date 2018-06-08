---
title: FetchOptions, propriété (RDS) | Documents Microsoft
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
- FetchOptions property [ADO]
ms.assetid: 7b2e254a-9354-4541-bc98-bb185276388f
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dcbb951a5d17c7b7f0d8be540bef5c74e96b101d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fetchoptions-property-rds"></a>FetchOptions, propriété (RDS)
Indique le type de l’extraction asynchrone.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="setting-and-return-values"></a>Définition et valeurs de retour  
 Définit ou retourne l’une des valeurs suivantes.  
  
|Constante| Description|  
|--------------|-----------------|  
|**adcFetchUpFront**|Tous les enregistrements de la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) sont extraites avant que le contrôle est retourné à l’application. Le texte complet **Recordset** est extrait avant que l’application est autorisée à faire quoi que ce soit avec lui.|  
|**adcFetchBackground**|Contrôle peut retourner à l’application dès que le premier lot d’enregistrements a été extrait. Suivante de la lecture de la **Recordset** que non extrait dans le premier lot est retardée jusqu'à ce que l’enregistrement recherché est extrait, moment auquel le contrôle retourne à l’application.|  
|**adcFetchAsync**|Valeur par défaut. Contrôle retourne immédiatement à l’application, tandis que les enregistrements sont récupérés en arrière-plan. Si l’application tente de lire un enregistrement qui n’a pas encore été extrait, l’enregistrement le plus proche de l’enregistrement recherché est lues et le contrôle retourne immédiatement, ce qui indique que la fin actuelle de la **Recordset** a été atteinte. Par exemple, un appel à [MoveLast](../../../ado/reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) déplacera la position actuelle vers le dernier enregistrement extrait, même si plusieurs enregistrements continuera à remplir la **Recordset**.|  
  
> [!NOTE]
>  Chaque fichier exécutable côté client qui utilise ces constantes doit fournir les déclarations. Vous pouvez couper et coller les déclarations de constante souhaitées dans le fichier Adcvbs.inc, situé dans le dossier d’installation par défaut pour la bibliothèque de services Bureau à distance.  
  
## <a name="remarks"></a>Notes  
 Dans une application Web, vous devez généralement utiliser **adcFetchAsync** (la valeur par défaut), car elle offre de meilleures performances. Dans une application cliente compilée, vous devez généralement utiliser **adcFetchBackground**.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ExecuteOptions et FetchOptions, propriétés-exemple (VBScript)](../../../ado/reference/rds-api/executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Cancel, méthode (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)


