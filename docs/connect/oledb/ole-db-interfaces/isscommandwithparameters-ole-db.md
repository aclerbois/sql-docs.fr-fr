---
title: ISSCommandWithParameters (OLE DB) | Documents Microsoft
description: ISSCommandWithParameters (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-interfaces
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 3df5ab4476cdc41568c4bd2ee0fdac28c52b11cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="isscommandwithparameters-ole-db"></a>ISSCommandWithParameters (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  **ISSCommandWithParameters** interface expose la prise en charge de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] XML et les types définis par l’utilisateur (UDT). Il s’agit d’une interface facultative qui hérite de l’interface OLE DB **ICommandWithParameters**. Outre les trois méthodes héritées de **ICommandWithParameters**; **GetParameterInfo**, **MapParameterNames**, et **SetParameterInfo**; **ISSCommandWithParameters** fournit deux nouvelles méthodes qui permettent de gérer des types de données spécifiques au serveur.  
  
> [!NOTE]  
>  Le **ISSCommandWithParameters** interface peut être utilisée lorsque les composants de Service sont utilisés, mais les composants de Service ne sera pas utiliser cette interface.  
  
|Méthode| Description|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties & #40 ; OLE DB & #41 ;](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|Retourne une structure de jeu de propriétés **SSPARAMPROPS** dans le tableau pour chaque paramètre UDT ou XML passé à la commande, mais rien n'est retourné pour d'autres types de paramètres.|  
|[ISSCommandWithParameters::SetParameterProperties & #40 ; OLE DB & #41 ;](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|Définit les propriétés de paramètre pour chaque paramètre par ordinal ou définit des propriétés de paramètre en bloc en spécifiant un tableau de structures **SSPARAMPROPS** .|  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces & #40 ; OLE DB & #41 ;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [À l’aide des Types de données XML](../../oledb/features/using-xml-data-types.md)   
 [À l’aide des Types définis par l’utilisateur](../../oledb/features/using-user-defined-types.md)  
  
  
