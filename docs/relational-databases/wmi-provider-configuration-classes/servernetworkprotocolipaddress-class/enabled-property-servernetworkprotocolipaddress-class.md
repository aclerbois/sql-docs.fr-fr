---
title: "Enabled, propriété (classe ServerNetworkProtocolIpAddress) | Documents Microsoft"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: wmi
ms.reviewer: 
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: reference
apiname: Enabled Property (ServerNetworkProtocolIpAddress Class)
apilocation: sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords: Enabled property
ms.assetid: 870fd4d0-6c77-462a-b480-d42eb044b2e7
caps.latest.revision: "14"
author: JennieHubbard
ms.author: jhubbard
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 755178e6e89d380bc1ad6d38b755a9d6194cbfe5
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2017
---
# <a name="enabled-property-servernetworkprotocolipaddress-class"></a>Propriété Enabled (classe ServerNetworkProtocolIpAddress)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]Obtient la propriété booléenne qui spécifie si une adresse IP est activée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Enabled [= value]  
```  
  
## <a name="parts"></a>Éléments  
 *objet*  
 A [classe ServerNetworkProtocolIPAdress](../../../relational-databases/wmi-provider-configuration-classes/servernetworkprotocolipaddress-class/servernetworkprotocolipaddress-class.md) qui représente une adresse IP pour le protocole réseau sur l'instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Valeur booléenne qui spécifie si l'adresse IP est activée : **true** si l'adresse IP est activée ou **false** si l'adresse IP est désactivée.  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration des bibliothèques réseau et des protocoles réseau du serveur](http://msdn.microsoft.com/library/ms177485\(v=sql.100\).aspx)  
  
  