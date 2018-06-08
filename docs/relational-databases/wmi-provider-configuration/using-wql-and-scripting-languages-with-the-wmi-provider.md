---
title: À l’aide de WQL et les langages avec le fournisseur WMI de script | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- queries [WMI]
- scripts [WMI]
- query language [WMI]
- WMI Query Language [WMI]
- WQL [WMI]
- WMI Provider for Configuration Management, WQL
- WMI Provider for Configuration Management, scripts
ms.assetid: c1e64905-3c2b-4974-88f4-abf17cf7e289
caps.latest.revision: 18
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: f399dd4045c1eea0465c6fe6f61a9a57ce7c8412
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-wql-and-scripting-languages-with-the-wmi-provider"></a>À l’aide de WQL ou langage de script avec le fournisseur WMI
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les applications de gestion accèdent aux services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux paramètres de réseau à l'aide des objets Fournisseur WMI (Windows Management Instrumentation) pour la gestion de configuration de deux manières :  
  
-   à l'aide d'un outil d'édition ou de requête WQL, tel que WBEMTest.exe, pour interroger l'objet défini avec le langage WQL ;  
  
-   à l'aide d'un langage de script, tel que VBScript.  
  
 Les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les paramètres de réseau peuvent être gérés par programme également à l'aide des objets managés WMI dans SMO. Pour plus d’informations sur la programmation WMI des objets managés, consultez [gestion des Services et les paramètres de réseau en utilisant le fournisseur WMI](../../relational-databases/server-management-objects-smo/tasks/managing-services-and-network-settings-by-using-wmi-provider.md).  
  
 Le fournisseur WMI pour la gestion de configuration peut être accédé en utilisant le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console. Pour plus d’informations sur l’accès au fournisseur WMI à partir d’une interface utilisateur, consultez [la gestion des Services de rubriques &#40;Gestionnaire de Configuration SQL Server&#41;](http://msdn.microsoft.com/library/78dee169-df0c-4c95-9af7-bf033bc9fdc6).  
  
## <a name="see-also"></a>Voir aussi  
 [Accéder au fournisseur de WMI pour la gestion de la Configuration à l’aide de WQL](../../relational-databases/wmi-provider-configuration/access-wmi-provider-for-configuration-management-using-wql.md)   
 [Modifier les propriétés SQL Server Service avancées à l’aide de VBScript](../../relational-databases/wmi-provider-configuration/access-wmi-provider-for-configuration-management-using-vbscript.md)  
  
  
