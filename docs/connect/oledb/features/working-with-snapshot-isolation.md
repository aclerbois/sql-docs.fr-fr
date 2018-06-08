---
title: Utilisation de l’isolement d’instantané | Documents Microsoft
description: Utilisation de l’isolation d’instantané dans le pilote OLE DB pour SQL Server
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
- data access [OLE DB Driver for SQL Server], snapshot isolation
- MSOLEDBSQL, snapshot isolation
- isolation levels [SQL Server], snapshot
- DBPROPSET_SESSION property set
- DBDROPSET_DATASOURCEINFO property set
- snapshot isolation [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, snapshot isolation
- SQLGetInfo function
- concurrency [OLE DB Driver for SQL Server]
- SQLSetConnectAttr function
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 7c70ad99b77f4dc70329d86adc936f18faced890
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-snapshot-isolation"></a>Utilisation de l’isolement d’instantané
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], un nouveau niveau d'isolement d'« instantané » a été introduit pour améliorer la concurrence pour les applications de traitement transactionnel en ligne (OLTP). Dans les versions antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la concurrence était uniquement basée sur le verrouillage, ce qui pouvait provoquer des problèmes de blocage pour certaines applications. Le niveau d'isolement d'instantané, qui dépend des améliorations apportées au contrôle de version de ligne, est conçu pour améliorer les performances en évitant les scénarios de blocage du lecteur/enregistreur.  
  
 Les transactions qui démarrent avec un niveau d'isolement d'instantané lisent un instantané de base de données au moment du démarrage de la transaction. Jeu de clés, les curseurs de serveur statiques et dynamiques, ouvert dans un contexte de transaction d’instantané se comportent comme très similaire à celles des curseurs statiques qui ont été ouverts dans des transactions sérialisables. Toutefois, lorsque les curseurs sont ouverts avec les verrous de niveau d’isolation snapshot ne sont pas prises. Cela peut réduire le blocage sur le serveur.  
  
## <a name="ole-db-driver-for-sql-server"></a>Pilote d’OLE DB pour SQL Server  
 Le pilote OLE DB pour SQL Server offre des améliorations qui tirent parti de l’isolation d’instantané introduite dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Ces améliorations incluent des modifications aux jeux de propriétés DBPROPSET_DATASOURCEINFO et DBPROPSET_SESSION.  
  
### <a name="dbpropsetdatasourceinfo"></a>DBPROPSET_DATASOURCEINFO  
 Le jeu de propriétés DBPROPSET_DATASOURCEINFO a été modifié pour indiquer que le niveau d'isolement d'instantané est pris en charge par l'ajout de la valeur DBPROPVAL_TI_SNAPSHOT utilisée dans la propriété DBPROP_SUPPORTEDTXNISOLEVELS. Cette nouvelle valeur indique que le niveau d'isolement d'instantané est pris en charge, que le contrôle de version ait été activé sur la base de données ou non. Le tableau suivant répertorie les valeurs DBPROP_SUPPORTEDTXNISOLEVELS :  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|DBPROP_SUPPORTEDTXNISOLEVELS|Type : VT_I4<br /><br /> Lecture/écriture : lecture seule<br /><br /> Description : masque de bits spécifiant les niveaux d'isolation de la transaction pris en charge. Combinaison de zéro ou plusieurs des éléments suivants :<br /><br /> DBPROPVAL_TI_CHAOS<br /><br /> DBPROPVAL_TI_READUNCOMMITTED<br /><br /> DBPROPVAL_TI_BROWSE<br /><br /> DBPROPVAL_TI_CURSORSTABILITY<br /><br /> DBPROPVAL_TI_READCOMMITTED<br /><br /> DBPROPVAL_TI_REPEATABLEREAD<br /><br /> DBPROPVAL_TI_SERIALIZABLE<br /><br /> DBPROPVAL_TI_ISOLATED<br /><br /> DBPROPVAL_TI_SNAPSHOT|  
  
### <a name="dbpropsetsession"></a>DBPROPSET_SESSION  
 Le jeu de propriétés DBPROPSET_SESSION a été modifié pour indiquer que le niveau d'isolement d'instantané est pris en charge par l'ajout de la valeur DBPROPVAL_TI_SNAPSHOT utilisée dans la propriété DBPROP_SESS_AUTOCOMMITISOLEVELS. Cette nouvelle valeur indique que le niveau d'isolement d'instantané est pris en charge, que le contrôle de version ait été activé sur la base de données ou non. Le tableau suivant répertorie les valeurs DBPROP_SESS_AUTOCOMMITISOLEVELS :
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|Type : VT_I4<br /><br /> Lecture/écriture : lecture seule<br /><br /> Description : spécifie un masque de bits qui indique le niveau d'isolation de la transaction lorsqu'elle se trouve en mode de validation automatique. Les valeurs qui peuvent être définies dans ce masque de bits sont identiques à celles qui peuvent être définies pour DBPROP_SUPPORTEDTXNISOLEVELS.|  
  
> [!NOTE]  
>  Les erreurs DB_S_ERRORSOCCURRED ou DB_E_ERRORSOCCURRED se produisent si DBPROPVAL_TI_SNAPSHOT est défini dans le cadre de l'utilisation de versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Pour plus d’informations sur la façon dont l’isolement d’instantané est pris en charge dans les transactions, consultez [prenant en charge les Transactions locales](../../oledb/ole-db-transactions/supporting-local-transactions.md).  

  
## <a name="see-also"></a>Voir aussi  
 [Pilote de base de données OLE pour les fonctionnalités SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Comportements et les propriétés de l’ensemble de lignes](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
  
