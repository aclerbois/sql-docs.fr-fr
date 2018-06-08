---
title: Accès aux données depuis les objets de base de données CLR | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], data access
- routines [CLR integration]
- data access [CLR integration]
- ADO.NET [CLR integration]
- internal data access [CLR integration]
- common language runtime [SQL Server], ADO.NET
- database objects [CLR integration], data access
- managed code [SQL Server], database objects
- .NET Data Access Provider for SQL Server [CLR integration]
- managed code [SQL Server], data access
- SqlClient provider
- in-process data access providers [CLR integration]
ms.assetid: 9a0f4dee-71c1-42e9-a85e-52382807010f
caps.latest.revision: 41
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 85229a6a5475e2b3e1b033cd070a13a008825951
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-access-from-clr-database-objects"></a>Accès aux données à partir d'objets de base de données CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une routine du common language runtime (CLR) peut accéder facilement aux données stockées dans l’instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans lequel elle s’exécute, ainsi que les données stockées dans les instances distantes. Les données particulières auxquelles la routine peut accéder sont déterminées par le contexte utilisateur dans lequel le code s'exécute. Accéder aux données à partir d’un objet de base de données CLR à l’aide du fournisseur de données .NET Framework pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], également connue sous le nom **SqlClient**. Il s'agit du même fournisseur que celui utilisé par les développeurs accédant aux données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d'applications clientes managées et de couche intermédiaire. Pour cette raison, vous pouvez approfondir vos connaissances d’ADO.NET et **SqlClient** dans les applications clientes et de couche intermédiaire.  
  
> [!NOTE]  
>  Les méthodes de type défini par l'utilisateur et les fonctions définies par l'utilisateur ne sont pas autorisées à effectuer un accès aux données par défaut. Vous devez définir le **DataAccess** propriété du **SqlMethodAttribute** ou **SqlFunctionAttribute** à **DataAccessKind.Read** pour activer l’accès en lecture seule des données à partir des méthodes de type défini par l’utilisateur (UDT) ou des fonctions définies par l’utilisateur. Les opérations de modification des données ne sont pas autorisées à partir des types définis par l'utilisateur ou des fonctions définies par l'utilisateur ; par ailleurs, elles lèvent des exceptions au moment de l'exécution, à la moindre tentative.  
  
 Cette section décrit uniquement les différences spécifiques en matière de fonctionnement et de comportement lors de l'accès aux données à partir d'un objet de base de données CLR. Pour plus d'informations sur les fonctionnalités d'ADO.NET, consultez la documentation ADO.NET incluse dans le Kit de développement logiciel .NET Framework (SDK).  
  
 Le tableau suivant décrit les rubriques de cette section.  
  
 [Connexion de contexte](../../../relational-databases/clr-integration/data-access/context-connection.md)  
 Décrit la connexion contextuelle à SQL Server.  
  
 [Emprunt d’identité et informations d’identification pour les connexions](../../../relational-databases/clr-integration/data-access/impersonation-and-credentials-for-connections.md)  
 Décrit l'emprunt d'identité et les informations d'identification des connexions.  
  
 [Extensions spécifiques in-process de SQL Server à ADO.NET](../../../relational-databases/clr-integration-data-access-in-process-ado-net/sql-server-in-process-specific-extensions-to-ado-net.md)  
 Décrit la spécifique dans le processus **SqlPipe**, **SqlContext**, **SqlTriggerContext**, et **SqlDataRecord** objets.  
  
 [Transactions et l’intégration du CLR](../../../relational-databases/clr-integration-data-access-transactions/clr-integration-and-transactions.md)  
 Explique comment la nouvelle infrastructure de transaction fournie dans l'espace de noms System.Transactions s'intègre à ADO.NET et décrit l'intégration du CLR de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Sérialisation XML à partir des objets de base de données CLR](http://msdn.microsoft.com/library/ac84339b-9384-4710-bebc-01607864a344)  
 Explique comment activer les scénarios de sérialisation XML des objets de base de données CLR dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
  
