---
title: "Extensions spécifiques de SQL Server In-Process à ADO.NET | Documents Microsoft"
ms.custom: 
ms.date: 03/06/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: clr
ms.reviewer: 
ms.suite: sql
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: reference
helpviewer_keywords:
- data access [CLR integration]
- ADO.NET [CLR integration]
- common language runtime [SQL Server], ADO.NET
- database objects [CLR integration], in-process extensions
- in-process data access providers [CLR integration]
- extensions [CLR integration]
ms.assetid: 781b812e-eb14-472a-85fa-aa4cdb929bee
caps.latest.revision: 
author: rothja
ms.author: jroth
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 778957280803b758bbf31775d71a7aca141d8494
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="sql-server-in-process-specific-extensions-to-adonet"></a>Extensions spécifiques in-process de SQL Server à ADO.NET
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Quatre extensions fonctionnelles principales à ADO.NET sont spécifiquement destinées à une utilisation in-process. Les rubriques suivantes traitent ces extensions en détail.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Objet SqlContext](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqlcontext-object.md)  
 Cette classe fournit l'accès aux autres extensions en faisant abstraction du contexte d'un appelant d'une routine SQL Server qui exécute le code managé in-process.  
  
 [Objet SqlPipe](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqlpipe-object.md)  
 Cette classe contient les routines pour envoyer les messages et les résultats tabulaires au client.  
  
 [Objet SqlTriggerContext](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqltriggercontext-object.md)  
 Cette classe fournit des informations sur le contexte dans lequel un déclencheur est exécuté.  
  
 [Objet SqlDataRecord](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqldatarecord-object.md)  
 La classe SqlDataRecord représente une ligne unique de données, avec ses métadonnées connexes, et permet que les procédures stockées retournent des jeux de résultats personnalisés au client.  
  
  
