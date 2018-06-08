---
title: Objets BLOB et OLE | Documents Microsoft
description: Objets BLOB et OLE
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-blobs
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- BLOBs
- storage object [OLE DB]
- OLE DB Driver for SQL Server, BLOBs
- large data, OLE objects
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: e78fe8db35684bb35e4111a38d3d0ba938891785
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="blobs-and-ole-objects"></a>Objets BLOB et OLE
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le pilote OLE DB pour SQL Server expose la **ISequentialStream** interface pour prendre en charge d’accès client aux [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **ntext**, **texte**, **image** , **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, et les types de données xml sous forme binaire des objets volumineux (BLOB). Le **en lecture** méthode sur **ISequentialStream** permet au consommateur de récupérer le volume de données en segments maniables.  
  
 Pour obtenir un exemple illustrant cette fonctionnalité, consultez [de définition de données volumineux & #40 ; OLE DB & #41 ;](../../oledb/ole-db-how-to/set-large-data-ole-db.md).  
  
 Le pilote OLE DB pour SQL Server peut utiliser implémentée par un consommateur **IStorage** interface lorsque le consommateur fournit le pointeur d’interface dans un accesseur lié pour la modification des données.  
  
 Pour les types de données de valeur élevée, le pilote OLE DB pour SQL Server vérifie les hypothèses de taille de type dans **IRowset** et les interfaces DDL. Les colonnes qui ont **varchar**, **nvarchar**, et **varbinary** types de données et la taille maximale illimitée sont représentées comme ISLONG via les ensembles de lignes de schéma et via interfaces qui retournent des types de données de colonne.  
  
 Le pilote OLE DB pour SQL Server expose la **varchar (max)**, **varbinary (max)** et **nvarchar (max)** respectivement les types en tant que DBTYPE_STR, DBTYPE_BYTES et DBTYPE_WSTR.  
  
 Pour utiliser ces types, une application dispose des options suivantes :  
  
-   Lier comme type (DBTYPE_STR, DBTYPE_BYTES, DBTYPE_WSTR). Si le tampon n’est pas insuffisante, une troncation se produit, exactement comme pour ces types dans les versions précédentes (bien que les valeurs plus élevées soient maintenant disponibles).  
  
-   Lier comme type et spécifier également DBTYPE_BYREF.  
  
-   Lier comme DBTYPE_IUNKNOWN et utiliser la diffusion en continu.  
  
 En cas de liaison à DBTYPE_IUNKNOWN, la fonctionnalité de flux ISequentialStream est utilisée. Le pilote OLE DB pour SQL Server prend en charge la liaison des paramètres de sortie en tant que DBTYPE_IUNKNOWN pour les types de données de valeur élevée. Il s’agit pour prendre en charge les scénarios où une procédure stockée retourne ces types de données en tant que valeurs de retour, qui seront restitués au client en tant que DBTYPE_IUNKNOWN.  
  
## <a name="storage-object-limitations"></a>Limitations des objets de stockage  
  
-   Le pilote OLE DB pour SQL Server peut prendre en charge qu’un objet de stockage ouvert unique. Toute tentative d’ouverture de plusieurs objets de stockage (pour obtenir une référence sur plusieurs **ISequentialStream** pointeur d’interface) retournent DBSTATUS_E_CANTCREATE.  
  
-   Dans le pilote OLE DB pour SQL Server, la valeur par défaut de la propriété en lecture seule de DBPROP_BLOCKINGSTORAGEOBJECTS est VARIANT_TRUE. Par conséquent, si un objet de stockage est actif, certaines méthodes (autres que les méthodes sur les objets de stockage) échouent avec E_UNEXPECTED.  
  
-   La longueur des données fournies par un objet de stockage implémenté consommateur doit avoir connaissance pour le pilote OLE DB pour SQL Server lors de la création de l’accesseur de ligne qui fait référence à l’objet de stockage. Le consommateur doit lier un indicateur de longueur dans la structure DBBINDING utilisée pour la création de l'accesseur.  
  
-   Si une ligne contient plus une valeur de données élevées et le DBPROP_ACCESSORDER n’est pas DBPROPVAL_AO_RANDOM, le consommateur doit utiliser un pilote OLE DB pour l’ensemble de lignes de curseur pris en charge de SQL Server pour récupérer des données de ligne ou traiter toutes les valeurs de données de grande taille avant les autres valeurs de ligne. Si DBPROP_ACCESSORDER a pour valeur DBPROPVAL_AO_RANDOM, le pilote OLE DB pour SQL Server met en cache tous les types de données xml en tant qu’objets binaires volumineux (BLOB) pour qu’il peut être accessible dans n’importe quel ordre.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Obtention de données volumineuses](../../oledb/ole-db-blobs/getting-large-data.md)  
  
-   [Définition de données volumineuses](../../oledb/ole-db-blobs/setting-large-data.md)  
  
-   [Prise en charge de diffusion en continu pour les paramètres de sortie BLOB](../../oledb/ole-db-blobs/streaming-support-for-blob-output-parameters.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation de SQL Server OLE DB pilote](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)        
 [À l’aide des Types de valeur élevée](../../oledb/features/using-large-value-types.md)  
  
  
