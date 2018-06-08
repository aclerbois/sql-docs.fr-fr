---
title: À l’aide de la mise en miroir de base de données | Documents Microsoft
description: À l’aide de la mise en miroir de base de données avec le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- OLE DB Driver for SQL Server, database mirroring
- data access [OLE DB Driver for SQL Server], database mirroring
- database mirroring [SQL Server], connecting clients to
- MSOLEDBSQL, database mirroring
- OLE DB Driver for SQL Server, database mirroring
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 9ba469d6f17618f06b92c257982b430c3636b916
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-database-mirroring"></a>Utilisation de la mise en miroir de bases de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]Utilisez [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] à la place.  
  
 La mise en miroir de bases de données, introduite dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], est une solution permettant d'accroître la disponibilité de la base de données et la redondance des données. Pilote OLE DB pour SQL Server fournit la prise en charge implicite de mise en miroir de base de données, le développeur n’avez donc pas à écrire du code ou prendre aucune autre mesure une fois qu’il a été configuré pour la base de données.  
  
 Base de données mise en miroir, qui est implémentée sur une base par base de données, conserve une copie d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données de production sur un serveur de secours. Ce serveur est un serveur de secours automatique ou semi-automatique, selon la configuration et l'état de la session de mise en miroir de bases de données. Un serveur de secours automatique prend en charge le basculement rapide sans perte de transactions validées et un serveur de secours semi-automatique prend en charge le service forcé (avec perte de données possible).  
  
 La base de données de production est appelée la *base de données principale*, et la copie de secours est appelée le *base de données miroir*. La base de données principale et la base de données miroir doivent résider sur des instances distinctes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (instances de serveur), et ils doivent résider sur des ordinateurs distincts, si possible.  
  
 L’instance de serveur de production, appelée la *serveur principal*, communique avec l’instance de serveur de secours, appelée la *serveur miroir*. Les serveurs principal et miroir agissent en tant que partenaires dans une base de données mise en miroir *session*. Si le serveur principal échoue, le serveur miroir peut créer sa base de données dans la base de données principale via un processus appelé *basculement*. Par exemple, Partner_A et Partner_B sont deux serveurs partenaires, avec initialement la base de données principale sur Partner_A comme serveur principal et la base de données miroir sur Partner_B comme serveur miroir. Si Partner_A se retrouve hors connexion, la base de données sur Partner_B peut basculer pour devenir la base de données principale en cours. Lorsque Partner_A rejoint la session de mise en miroir, il devient le serveur miroir et sa base de données devient la base de données miroir.  
  
 D'autres configurations de mise en miroir de bases de données offrent des niveaux différents de performances et de sécurité des données, et prennent en charge diverses formes de basculement. Pour plus d’informations, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
 Il est possible d'utiliser un alias lors de la spécification du nom d'une base de données miroir.  
  
> [!NOTE]  
>  Pour plus d’informations sur les tentatives de connexion initiale et les tentatives de reconnexion à une base de données mise en miroir, consultez [connecter des Clients à une Session de mise en miroir de base de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md).  
  
## <a name="programming-considerations"></a>Éléments de programmation à prendre en considération  
 Lorsque le serveur de la base de données principale échoue, l'application cliente reçoit des erreurs en réponse aux appels d'API, erreurs qui signifient que la connexion à la base de données a été perdue. Lorsque cela se produit, toutes les modifications apportées à la base de données qui n'ont pas été validées sont perdues et la transaction en cours est annulée. Si cela se produit, l'application doit fermer la connexion (ou libérer l'objet source de données) et la rouvrir. La connexion est redirigée de façon transparente vers la base de données miroir, qui fait désormais office de serveur principal.  
  
 Lorsqu'une connexion est établie, le serveur principal envoie au client l'identité de son partenaire de basculement à utiliser en cas de basculement. Si une application a essayé d'établir une connexion après un échec du serveur principal, le client ne connaît pas l'identité du partenaire de basculement. Afin que les clients puissent faire face à ce scénario, une propriété d'initialisation et un mot clé de chaîne de connexion associé permettent au client de spécifier l'identité du partenaire de basculement. L'attribut client est utilisé uniquement dans ce scénario ; si le serveur principal est disponible, il n'est pas utilisé. Si le partenaire de basculement fourni par le client ne fait pas référence à un serveur agissant comme partenaire de basculement, la connexion est refusée par le serveur. Pour permettre aux applications de s'adapter aux modifications de configuration, l'identité du partenaire de basculement peut être déterminée en examinant l'attribut après que la connexion a été établie. Si possible, mettez en cache les informations sur le serveur partenaire pour mettre à jour la chaîne de connexion ou imaginer une stratégie de nouvelle tentative au cas où la première tentative d'établissement de la connexion échouerait.  
  
> [!NOTE]  
>  Vous devez spécifier explicitement la base de données utilisée par une connexion si vous souhaitez recourir à cette fonctionnalité dans un DSN, une chaîne de connexion ou une propriété (ou un attribut) de connexion. Pilote OLE DB pour SQL Server ne tente pas de basculer vers la base de données du partenaire, si cela n’est pas fait.  
>   
>  La mise en miroir est une fonctionnalité de la base de données. Il se peut que les applications qui utilisent plusieurs bases de données ne puissent pas exploiter cette fonctionnalité.  
>   
>  De plus, les noms de serveur ne respectent pas la casse, contrairement aux noms de base de données. Par conséquent, vous devez vous assurer que vous utilisez la même casse dans les DSN et les chaînes de connexion.  
  
## <a name="ole-db-driver-for-sql-server"></a>Pilote d’OLE DB pour SQL Server  
 Le pilote OLE DB pour SQL Server prend en charge la mise en miroir via les connexions et les attributs de chaîne. La propriété SSPROP_INIT_FAILOVERPARTNER a été ajoutée au jeu de propriétés DBPROPSET_SQLSERVERDBINIT et le **FailoverPartner** (mot clé) est un nouvel attribut de chaîne de connexion de DBPROP_INIT_PROVIDERSTRING. Pour plus d’informations, consultez [à l’aide de mots clés de chaîne de connexion avec le pilote OLE DB pour SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 Le cache de basculement est conservé tant que le fournisseur est chargé, autrement dit jusqu'à ce que **CoUninitialize** est appelée ou tant que l’application a une référence à un objet géré par le pilote OLE DB pour SQL Server comme un objet de source de données .  
  
 Pour plus d’informations sur le pilote OLE DB pour la prise en charge de SQL Server pour la mise en miroir de base de données, consultez [propriétés d’initialisation et d’autorisation](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md).  
 
  
## <a name="see-also"></a>Voir aussi  
 [Pilote de base de données OLE pour les fonctionnalités SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Connecter des clients à une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
