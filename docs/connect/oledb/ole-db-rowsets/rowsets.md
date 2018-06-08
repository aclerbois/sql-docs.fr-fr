---
title: Ensembles de lignes | Documents Microsoft
description: Ensembles de lignes dans le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-rowsets
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- OLE DB Driver for SQL Server, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d3f187118cd273712ed8145bbfef3af712091028
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rowsets"></a>Ensembles de lignes
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Un ensemble de lignes est un jeu de lignes contenant des colonnes de données. Les ensembles de lignes sont des objets centraux qui permettent à tous les fournisseurs de données OLE DB d'exposer les données des jeux de résultats sous forme tabulaire.  
  
 Après un consommateur crée une session à l’aide de la **IDBCreateSession::CreateSession** (méthode), le consommateur peut utiliser soit le **IOpenRowset** ou **IDBCreateCommand** interface sur la session pour créer un ensemble de lignes. Le pilote OLE DB pour SQL Server prend en charge ces deux interfaces. Ces deux méthodes sont décrites ici.  
  
-   Créer un ensemble de lignes en appelant le **IOpenRowset::OpenRowset** (méthode).  
  
     Cette solution est équivalente à la création d'un ensemble de lignes sur une même table. Cette méthode ouvre et retourne un ensemble de lignes qui inclut toutes les lignes d'une même table de base. Un des arguments de **OpenRowset** est un ID qui identifie la table à partir duquel créer l’ensemble de lignes.  
  
-   Créer un objet de commande en appelant le **IDBCreateCommand::CreateCommand** (méthode).  
  
     L'objet de commande exécute les commandes que le fournisseur prend en charge. Avec le pilote OLE DB pour SQL Server, le consommateur peut spécifier toute [!INCLUDE[tsql](../../../includes/tsql-md.md)] instruction, comme une instruction SELECT ou un appel à une procédure stockée. Les étapes de la création d'un ensemble de lignes en utilisant un objet de commande sont les suivantes :  
  
    1.  Le consommateur appelle la **IDBCreateCommand::CreateCommand** méthode sur la session pour obtenir un objet de commande demande la **ICommandText** interface sur l’objet de commande. Cela **ICommandText** interface définit et extrait le texte de commande. Le consommateur remplit la commande de texte en appelant le **ICommandText::SetCommandText** (méthode).  
  
    2.  L’utilisateur appelle le **ICommand::Execute** méthode sur la commande. L'objet de l'ensemble de lignes créé lors de l'exécution de la commande contient le jeu de résultats de la commande.  
  
 Le consommateur peut utiliser le **ICommandProperties** interface à obtenir ou définir les propriétés de l’ensemble de lignes retourné par la commande exécutée par le **ICommand::Execute** interfaces. Les propriétés les plus communément demandées sont les interfaces que l'ensemble de lignes doit prendre en charge. En plus des interfaces, le consommateur peut demander les propriétés qui modifient le comportement de l'ensemble de lignes ou de l'interface.  
  
 Les consommateurs libèrent les ensembles de lignes avec le **IRowset::Release** (méthode). La libération d'un ensemble de lignes entraîne celle des handles de ligne détenus par le consommateur sur cet ensemble de lignes. La libération d'un ensemble de lignes ne libère pas les accesseurs. Si vous avez un **IAccessor** interface, il toujours doit être libérée.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Création d’un ensemble de lignes avec IOpenRowset](../../oledb/ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [Création d’ensembles de lignes avec ICommand::Execute](../../oledb/ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [Comportements et les propriétés de l’ensemble de lignes](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [Ensembles de lignes et curseurs SQL Server](../../oledb/ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [Extraction de lignes](../../oledb/ole-db-rowsets/fetching-rows.md)  
  
-   [Extraction d’une ligne unique avec IRow](../../oledb/ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [Signets](../../oledb/ole-db-rowsets/bookmarks.md)  
  
-   [Mise à jour des données dans les ensembles de lignes](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation OLE DB Driver pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
