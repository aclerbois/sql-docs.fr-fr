---
title: Création d’un pilote de base de données OLE pour Application SQL Server | Documents Microsoft
description: Création d’un pilote OLE DB pour l’application de SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb-driver-for-sql-server
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, application creation
- applications [OLE DB Driver for SQL Server]
- OLE DB, creating applications
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d9990a169beca3f676a19d3f12aadd5d7cf6a671
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-an-ole-db-driver-for-sql-server-application"></a>Création d’un pilote de base de données OLE pour l’Application de SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Ces étapes implique la création d’un pilote OLE DB pour l’application de SQL Server :  
  
1.  Établissement d'une connexion à une source de données.  
  
2.  Exécution d'une commande.  
  
3.  Traitement des résultats.  
  
> [!NOTE]  
>  Lorsque c'est possible, utilisez l'authentification Windows. Si l'authentification Windows n'est pas disponible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Évitez de stocker ces informations dans un fichier. Si vous devez conserver les informations d’identification, chiffrez-les avec [cryptoAPI Win32](http://go.microsoft.com/fwlink/?LinkId=9504).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [L’établissement d’une connexion à une Source de données](../../oledb/ole-db-driver/establishing-a-connection-to-a-data-source.md)  
  
-   [Exécution d’une commande](../../oledb/ole-db-driver/executing-a-command.md)  
  
-   [Le traitement des résultats](../../oledb/ole-db-driver/processing-results.md)  
  
-   [À propos d’OLE DB, propriétés](../../oledb/ole-db-driver/about-ole-db-properties.md)  
  
-   [Utilisation de la clause OUTPUT avec OLE DB dans OLE DB Driver pour SQL Server](../../oledb/ole-db-driver/using-the-output-clause-with-ole-db-in-oledb-driver-for-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation OLE DB Driver pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
