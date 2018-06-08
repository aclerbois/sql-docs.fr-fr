---
title: Propriétés (OLE DB) la Source de données | Documents Microsoft
description: Propriétés de Source de données (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-data-source-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, data source properties
- properties [OLE DB]
- data source properties [OLE DB]
- OLE DB data source properties [OLE DB Driver for SQL Server]
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: ef755447b9bb515fa6d05e1628ce7504e79f804a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-source-properties-ole-db"></a>Propriétés de la source de données (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le pilote OLE DB pour SQL Server implémente des propriétés de source de données comme suit.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|DBPROP_CURRENTCATALOG|R/W : lecture/écriture Par défaut : aucune<br /><br /> Description : La valeur de DBPROP_CURRENTCATALOG signale la base de données en cours pour un pilote OLE DB pour la session SQL Server. Définition de la valeur de propriété possède un effet identique que la définition de la base de données actuelle à l’aide de la [!INCLUDE[tsql](../../../includes/tsql-md.md)] utilisez *base de données* instruction.<br /><br /> À partir de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], si vous appelez [sp_defaultdb](../../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md) et spécifiez le nom de la base de données en minuscules, même si la base de données a été créé avec un nom de casse mixte, DBPROP_CURRENTCATALOG retourne le nom en minuscules. Avec les versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], DBPROP_CURRENTCATALOG retourne la casse mixte attendue.|  
|DBPROP_MULTIPLECONNECTIONS|R/W : lecture/écriture Par défaut : VARIANT_FALSE<br /><br /> Description : si la connexion exécute une commande qui ne produit pas un ensemble de lignes ou génère un ensemble de lignes qui n'est pas un curseur côté serveur et que vous exécutez une autre commande, une nouvelle connexion est créée pour exécuter la nouvelle commande si DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_TRUE.<br /><br /> Le pilote OLE DB pour SQL Server ne crée pas une autre connexion si DBPROP_MULTIPLECONNECTION a la valeur VARIANT_FALSE ou si une transaction est active sur la connexion. Le pilote OLE DB pour SQL Server retourne DB_E_OBJECTOPEN si DBPROP_MULTIPLECONNECTIONS a la valeur VARIANT_FALSE et retourne E_FAIL s’il existe une transaction active. Les transactions et le verrouillage sont gérés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] connexion par connexion. Si une deuxième connexion est générée, les commandes sur les connexions séparées ne partagent pas les verrous. Pour garantir qu'une commande n'en bloque pas une autre, maintenez les verrous sur les lignes demandées par l'autre commande. Ceci reste vrai en cas de création de plusieurs sessions.<br /><br /> Chaque session possède une connexion distincte.|  
  
 Dans l’ensemble de la propriété spécifique au fournisseur DBPROPSET_SQLSERVERDATASOURCE, le pilote OLE DB pour SQL Server définit les propriétés de la source de données supplémentaires.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|SSPROP_ENABLEFASTLOAD|R/W : lecture/écriture Par défaut : VARIANT_FALSE<br /><br /> Description : pour permettre la copie en bloc à partir de la mémoire, la propriété SSPROP_ENABLEFASTLOAD doit avoir la valeur VARIANT_TRUE. Avec cette propriété est définie sur la source de données, la session nouvellement créée autorise l’accès du consommateur à le [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) interface.<br /><br /> Si la propriété est définie avec la valeur VARIANT_TRUE, **IRowsetFastLoad** interface est disponible via **IOpenRowset::OpenRowset** en demandant le **IID_IRowsetFastLoad** interface ou en définissant **SSPROP_IRowsetFastLoad** avec la valeur VARIANT_TRUE.|  
|SSPROP_ENABLEBULKCOPY|R/W : lecture/écriture Par défaut : VARIANT_FALSE<br /><br /> Description : pour permettre la copie en bloc à partir de fichiers, la propriété SSPROP_ENABLEBULKCOPY doit avoir la valeur VARIANT_TRUE. Avec cette propriété définie sur la source de données, l'accès du consommateur à l'interface IBCPSession est disponible sous le même niveau que Sessions.<br /><br /> SSPROP_IRowsetFastLoad doit également être défini avec la valeur VARIANT_TRUE.|  
  
## <a name="see-also"></a>Voir aussi  
 [Objets Source de données & #40 ; OLE DB & #41 ;](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
