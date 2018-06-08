---
title: CommandTypeEnum | Documents Microsoft
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
- CommandTypeEnum
helpviewer_keywords:
- CommandTypeEnum enumeration [ADO]
ms.assetid: 4b1feb9c-a855-40fe-a906-efe688687e9f
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5d15cf7ce3c4af4d6bb4072dd3070298a846e825
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="commandtypeenum"></a>CommandTypeEnum
Spécifie comment un argument de commande doit être interprété.  
  
 Il est important de valider fourni par l’utilisateur *CommandString* valeurs pour éviter de révéler les utilisateurs de l’application la possibilité d’injecter des commandes potentiellement dangereuses pour ADO exécuter.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adCmdUnspecified**|-1|Ne spécifiez pas l’argument de type de commande.|  
|**adCmdText**|1|Prend la valeur [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) une définition textuelle d’une commande ou une procédure stockée à appeler.|  
|**adCmdTable**|2|Prend la valeur **CommandText** comme nom de table dont les colonnes sont renvoyées par une requête SQL générée en interne.|  
|**adCmdStoredProc**|4|Prend la valeur **CommandText** comme nom de procédure stockée.|  
|**adCmdUnknown**|8|Valeur par défaut. Indique que le type de commande dans le **CommandText** propriété n’est pas connue.<br /><br /> Lorsque le type de commande n’est pas connu, ADO effectuera plusieurs tentatives d’interpréter le **CommandText**.<br /><br /> -   **CommandText** est interprété comme une définition textuelle d’un appel de procédure stockée ou de commande. Il s’agit du même comportement que **adCmdText**.<br />-   **CommandText** est le nom d’une procédure stockée. Il s’agit du même comportement que **valeur adCmdStoredProc**.<br />-   **CommandText** est interprété comme le nom d’une table. Toutes les colonnes sont retournées par une requête SQL générée en interne. Il s’agit du même comportement que **adCmdTable**.|  
|**adCmdFile**|256|Prend la valeur **CommandText** comme nom de fichier de façon persistante stockée [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md). Utilisé avec **Recordset.** [Ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) ou [Requery](../../../ado/reference/ado-api/requery-method.md) uniquement.|  
|**adCmdTableDirect**|512|Prend la valeur **CommandText** comme nom de table dont les colonnes sont renvoyées. Utilisé avec **Recordset.Open** ou **Requery** uniquement. Pour utiliser le [recherche](../../../ado/reference/ado-api/seek-method.md) (méthode), la **Recordset** doit être ouvert avec **adCmdTableDirect**.<br /><br /> Cette valeur ne peut pas être combinée avec le [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) valeur **adAsyncExecute**.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.CommandType.UNSPECIFIED|  
|AdoEnums.CommandType.TEXT|  
|AdoEnums.CommandType.TABLE|  
|AdoEnums.CommandType.STOREDPROC|  
|AdoEnums.CommandType.UNKNOWN|  
|AdoEnums.CommandType.FILE|  
|AdoEnums.CommandType.TABLEDIRECT|  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[CommandType, propriété (ADO)](../../../ado/reference/ado-api/commandtype-property-ado.md)|[Execute, méthode (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)|  
|[Execute, méthode (objet Connection ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)|[Open, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)|  
|[Requery, méthode](../../../ado/reference/ado-api/requery-method.md)||
