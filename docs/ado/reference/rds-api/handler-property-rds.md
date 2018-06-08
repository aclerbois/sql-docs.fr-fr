---
title: Propriété de gestionnaire (RDS) | Documents Microsoft
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
- Handler property [ADO]
ms.assetid: fdc34362-6d47-4727-b171-8d033159408e
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b5cba5a709933f89cb7bb19666140053cd6cfc94
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handler-property-rds"></a>Propriété de gestionnaire (RDS)
Indique le nom d’un programme de personnalisation côté serveur (gestionnaire) qui étend les fonctionnalités de la [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)et tous les paramètres utilisés par le *gestionnaire*.  
  
 **S’applique à :** [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.Handler = String  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Une variable objet qui représente un [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet.  
  
 *Chaîne*  
 A **chaîne** valeur qui contient le nom du gestionnaire et tous les paramètres, toutes séparées par des virgules (par exemple, `"handlerName,parm1,parm2,...,parm` *N*`"`).  
  
## <a name="remarks"></a>Notes  
 Cette propriété prend en charge [personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md), une fonctionnalité qui nécessite le [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété **adUseClient**.  
  
 Le nom du gestionnaire et ses paramètres, le cas échéant, sont séparés par des virgules («, »). Génère un comportement imprévisible si un point-virgule (« ; ») apparaît n’importe où dans *chaîne*. Vous pouvez écrire votre propre gestionnaire, sous réserve qu’il prend en charge la **IDataFactoryHandler** interface.  
  
 Le nom du gestionnaire par défaut est **MSDFMAP. Gestionnaire**, et son paramètre par défaut est un fichier de personnalisation appelé **MSDFMAP. INI**. Cette propriété permet d’appeler d’autres fichiers de personnalisation créés par votre administrateur de serveur.  
  
 L’alternative à la définition la **gestionnaire** propriété consiste à spécifier un gestionnaire et des paramètres dans le [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) propriété ; autrement dit, « **Gestionnaire = *** handlerName, paramètre1, parameter2,... ;* ".  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété de gestionnaire (VB)](../../../ado/reference/rds-api/handler-property-example-vb.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)


