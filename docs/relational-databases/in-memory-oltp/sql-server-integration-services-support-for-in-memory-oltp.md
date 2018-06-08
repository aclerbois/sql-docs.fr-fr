---
title: Prise en charge d’OLTP en mémoire par SQL Server Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ea82a9b9-e9ed-4d6f-b3fd-917f6c687ae3
caps.latest.revision: 12
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 55ae18c7a70f3e068d7e71efad101cd933305b43
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-integration-services-support-for-in-memory-oltp"></a>Prise en charge d'OLTP en mémoire par SQL Server Integration Services
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Utilisez une table mémoire optimisée, une vue référençant des tables mémoire optimisées ou une procédure stockée compilée en mode natif en tant que source ou destination pour votre package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (SSIS). Utilisez la [source ADO NET](../../integration-services/data-flow/ado-net-source.md), la [source OLE DB](../../integration-services/data-flow/ole-db-source.md)ou la [source ODBC](../../integration-services/data-flow/odbc-source.md) dans le flux de données d’un package SSIS et configurez le composant source pour récupérer des données d’une table mémoire optimisée ou d’une vue, ou spécifiez une instruction SQL pour exécuter une procédure stockée compilée en mode natif. De même, utilisez la [destination ADO NET](../../integration-services/data-flow/ado-net-destination.md), la [destination OLE DB](../../integration-services/data-flow/ole-db-destination.md)ou la [destination ODBC](../../integration-services/data-flow/odbc-destination.md) pour charger des données dans une table mémoire optimisée ou une vue, ou spécifiez une instruction SQL pour exécuter une procédure stockée compilée en mode natif.  
  
 Configurez les composants source et de destination mentionnés ci-dessus dans un package SSIS pour lire/écrire dans des tables mémoire optimisées et des vues de la même façon qu'avec d'autres tables et vues [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Toutefois, vous devez prendre en compte les points importants de la section suivante lorsque vous utilisez des procédures stockées compilées en mode natif.  
  
## <a name="invoking-a-natively-compiled-stored-procedure-from-an-ssis-package"></a>Appel d'une procédure stockée compilée en mode natif à partir d'un package SSIS  
 Pour appeler une procédure stockée compilée en mode natif à partir d’un package SSIS, nous vous recommandons d’utiliser une source ODBC ou une destination ODBC avec une instruction SQL au format suivant : **\<nom de procédure>** sans le mot clé **EXEC**. Si vous utilisez le mot clé EXEC dans l'instruction SQL, un message d'erreur s'affiche, car le gestionnaire de connexions ODBC interprète le texte de la commande SQL en tant qu'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] au lieu d'une procédure stockée et utilise des curseurs, qui ne sont pas pris en charge pour l'exécution des procédures stockées compilées en mode natif. Le gestionnaire de connexions traite l'instruction SQL sans le mot clé EXEC comme un appel de procédure stockée et n'utilise pas de curseur.  
  
 Utilisez également la source ADO.NET et la source OLE DB pour appeler une procédure stockée compilée en mode natif. Cependant, nous vous recommandons d'utiliser la source ODBC. Si vous configurez la source ADO.NET pour exécuter une procédure stockée compilée en mode natif, un message d'erreur s'affiche, car le fournisseur de données pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient), que la source ADO.NET utilise par défaut, ne prend pas en charge l'exécution des procédures stockées compilées en mode natif. Configurez la source ADO .NET de façon à ce qu'elle utilise le fournisseur de données ODBC, le fournisseur OLE DB pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Cependant, notez que la source ODBC est plus performante que la source ADO.NET avec le fournisseur de données ODBC.  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en charge d'OLTP en mémoire par SQL Server](../../relational-databases/in-memory-oltp/sql-server-support-for-in-memory-oltp.md)  
  
  
