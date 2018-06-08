---
title: Fournisseur WMI pour les événements serveur Classes et propriétés | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- event classes [WMI]
- WMI Provider for Server Events, events listed
- classes [WMI]
ms.assetid: e2916cd7-a3ed-41e6-97b4-2ee060754cbe
caps.latest.revision: 33
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 3514bd676b6b84436141cdcf669cc6c8f33598e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wmi-provider-for-server-events-classes-and-properties"></a>Fournisseur WMI pour les classes et propriétés d'événements serveur
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les événements serveur suivants composent le modèle de programmation du fournisseur WMI pour les événements serveur. Deux catégories principales d'événements peuvent être interrogées en émettant des requêtes WQL sur le fournisseur. Il s'agit des événements DDL (Data Definition Language) et des événements de trace. Les événements de Service Broker QUEUE_ACTIVATION et BROKER_QUEUE_DISABLED peuvent également être interrogés. Notez la nature inclusive des arborescences suivantes. L'événement DDL_ASSEMBLY_EVENTS, par exemple, inclut tout événement ALTER_ASSEMBLY, CREATE_ASSEMBLY et DROP_ASSEMBLY. De la même façon, l'événement TRC_FULL_TEXT inclut tout événement FT_CRAWL_ABORTED, FT_CRAWL_STARTED et FT_CRAWL_STOPPED. ALL_EVENTS couvre tous les événements DDL, événements de trace, QUEUE_ACTIVATION et BROKER_QUEUE_DISABLED.  
  
 Pour savoir quelles propriétés d'un événement ou d'un groupe d'événements peuvent être interrogées, reportez-vous au schéma d'événement. Par défaut, le schéma d'événement est installé dans le répertoire suivant : [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Tools\Binn\schemas\sqlserver\2006\11\events\events .xsd.  
  
 Ou bien, vous pouvez faire référence au schéma d’événement publié à l’adresse [ http://schemas.microsoft.com/sqlserver ](http://go.microsoft.com/fwlink/?linkid=43100).  
  
 Par exemple, en faisant référence à l’événement ALTER_DATABASE, vous apprendrez que son événement parent est DDL_SERVER_LEVEL_EVENTS et ses propriétés sont **TSQLCommand** et **DatabaseName**. L’événement hérite également des propriétés **SQLInstance**, **PostTime**, **Nom_Ordinateur**, **SPID**, et **LoginName**. Il ne possède pas d'événements enfants.  
  
> [!NOTE]  
>  Les procédures stockées système qui exécutent des opérations de type DDL peuvent également déclencher des notifications d'événements. Testez vos notifications d'événements pour déterminer leur réponse aux procédures stockées du système qui sont exécutées. Par exemple, l’instruction CREATE TYPE et **sp_addtype** procédure stockée activeront toutes deux une notification d’événement qui est créée sur un événement CREATE_TYPE. Pour plus d’informations, consultez[événements DDL](../../relational-databases/triggers/ddl-events.md).  
  
 **Événements de langage de définition de données et des groupes d’événements**  
  
 ![Fournisseur WMI pour les événements événements arborescence](../../relational-databases/wmi-provider-server-events/media/sql-wmi-ddl-events-ktm.gif "fournisseur WMI pour l’arborescence de d’événements de serveur")  
  
 **Les groupes d’événements et les événements de trace**  
  
 ![Et groupes d’événements de trace](../../relational-databases/wmi-provider-server-events/media/sql-wmi-trc-all-events.gif "et groupes d’événements de Trace")  
  
## <a name="see-also"></a>Voir aussi  
 [Fournisseur WMI pour les Concepts des événements de serveur](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)   
 [Utilisation de WQL avec le fournisseur WMI pour les événements de serveur](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md)  
  
  
