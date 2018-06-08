---
title: Ensembles de lignes | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-rowsets
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
ms.assetid: 5e7b3cbe-3670-4e18-8172-2226e0b6b142
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1d12a7c6dfc3a63d20a16a4a4361ffd46e4f8972
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708487"
---
# <a name="rowsets"></a>Ensembles de lignes
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Un ensemble de lignes est un jeu de lignes contenant des colonnes de données. Les ensembles de lignes sont des objets centraux qui permettent à tous les fournisseurs de données OLE DB d'exposer les données des jeux de résultats sous forme tabulaire.  
  
 Après un consommateur crée une session à l’aide de la **IDBCreateSession::CreateSession** (méthode), le consommateur peut utiliser soit le **IOpenRowset** ou **IDBCreateCommand** interface sur la session pour créer un ensemble de lignes. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge ces deux interfaces. Ces deux méthodes sont décrites ici.  
  
-   Créer un ensemble de lignes en appelant le **IOpenRowset::OpenRowset** (méthode).  
  
     Cette solution est équivalente à la création d'un ensemble de lignes sur une même table. Cette méthode ouvre et retourne un ensemble de lignes qui inclut toutes les lignes d'une même table de base. Un des arguments de **OpenRowset** est un ID qui identifie la table à partir duquel créer l’ensemble de lignes.  
  
-   Créer un objet de commande en appelant le **IDBCreateCommand::CreateCommand** (méthode).  
  
     L'objet de commande exécute les commandes que le fournisseur prend en charge. Avec le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, le consommateur peut spécifier une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)], telle qu'une instruction SELECT ou l'appel d'une procédure stockée. Les étapes de la création d'un ensemble de lignes en utilisant un objet de commande sont les suivantes :  
  
    1.  Le consommateur appelle la **IDBCreateCommand::CreateCommand** méthode sur la session pour obtenir un objet de commande demande la **ICommandText** interface sur l’objet de commande. Cela **ICommandText** interface définit et extrait le texte de commande. Le consommateur remplit la commande de texte en appelant le **ICommandText::SetCommandText** (méthode).  
  
    2.  L’utilisateur appelle le **ICommand::Execute** méthode sur la commande. L'objet de l'ensemble de lignes créé lors de l'exécution de la commande contient le jeu de résultats de la commande.  
  
 Le consommateur peut utiliser le **ICommandProperties** interface à obtenir ou définir les propriétés de l’ensemble de lignes retourné par la commande exécutée par le **ICommand::Execute** interfaces. Les propriétés les plus communément demandées sont les interfaces que l'ensemble de lignes doit prendre en charge. En plus des interfaces, le consommateur peut demander les propriétés qui modifient le comportement de l'ensemble de lignes ou de l'interface.  
  
 Les consommateurs libèrent les ensembles de lignes avec le **IRowset::Release** (méthode). La libération d'un ensemble de lignes entraîne celle des handles de ligne détenus par le consommateur sur cet ensemble de lignes. La libération d'un ensemble de lignes ne libère pas les accesseurs. Si vous avez un **IAccessor** interface, il toujours doit être libérée.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Création d’un ensemble de lignes avec IOpenRowset](../../relational-databases/native-client-ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [Création d’ensembles de lignes avec ICommand::Execute](../../relational-databases/native-client-ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [Propriétés et comportements des ensembles de lignes](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [Ensembles de lignes et curseurs SQL Server](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [Récupération de lignes](../../relational-databases/native-client-ole-db-rowsets/fetching-rows.md)  
  
-   [Récupération d’une ligne unique avec IRow](../../relational-databases/native-client-ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [Signets](../../relational-databases/native-client-ole-db-rowsets/bookmarks.md)  
  
-   [Mise à jour des données dans les ensembles de lignes](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
