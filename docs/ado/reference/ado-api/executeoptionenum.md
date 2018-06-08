---
title: ExecuteOptionEnum | Documents Microsoft
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
f1_keywords:
- ExecuteOptionEnum
helpviewer_keywords:
- ExecuteOptionEnum enumeration [ADO]
ms.assetid: 68bfa83a-5df4-4bef-8736-0f88ae8c29ea
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f139ca30fc6fa8a23d93934e6b7cf5bd665ead69
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executeoptionenum"></a>ExecuteOptionEnum
Spécifie comment un fournisseur doit exécuter une commande.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adAsyncExecute**|0x10|Indique que la commande doit s’exécuter de façon asynchrone.<br /><br /> Cette valeur ne peut pas être combinée avec le [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) valeur **adCmdTableDirect**.|  
|**adAsyncFetch**|0x20|Indique que les lignes restantes après la quantité spécifiée dans le [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) propriété doit être récupérée de façon asynchrone.|  
|**adAsyncFetchNonBlocking**|0x40|Indique que le thread principal n’est jamais bloqué lors de la récupération. Si la ligne demandée n’a pas été récupérée, la ligne actuelle déplace automatiquement à la fin du fichier.<br /><br /> Si vous ouvrez un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) d’un [flux](../../../ado/reference/ado-api/stream-object-ado.md) contenant un stockés de façon permanente **Recordset**, **adAsyncFetchNonBlocking** n’aura pas un effet ; l’opération sera synchrone et bloquante.<br /><br /> **adAsynchFetchNonBlocking** n’a aucun effet lorsque les [adCmdTableDirect](../../../ado/reference/ado-api/commandtypeenum.md) option permet d’ouvrir le **Recordset**.|  
|**adExecuteNoRecords**|0x80|Indique que le texte de commande est une commande ou une procédure stockée qui ne retourne pas de lignes (par exemple, une commande qui insère des données uniquement). Si toutes les lignes sont récupérées, elles sont ignorées et pas retournées.<br /><br /> **adExecuteStream** peut être passé comme paramètre facultatif pour la **commande** ou **connexion exécuter** (méthode).|  
|**adExecuteStream**|0x400|Indique que les résultats d’une exécution de commande doivent être renvoyés en tant que flux.<br /><br /> **adExecuteStream** peut être passé comme paramètre facultatif pour la **commande exécuter** (méthode).|  
|**adExecuteRecord**||Indique que le **CommandText** est une commande ou une procédure stockée qui retourne une seule ligne qui doit être retournée comme un **enregistrement** objet.|  
|**adOptionUnspecified**|-1|Indique que la commande n’est pas spécifiée.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.ExecuteOption.ASYNCEXECUTE|  
|AdoEnums.ExecuteOption.ASYNCFETCH|  
|AdoEnums.ExecuteOption.ASYNCFETCHNONBLOCKING|  
|AdoEnums.ExecuteOption.NORECORDS|  
|AdoEnums.ExecuteOption.UNSPECIFIED|  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Execute, méthode (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)|[Execute, méthode (objet Connection ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)|  
|[Open, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)|[Requery, méthode](../../../ado/reference/ado-api/requery-method.md)|
