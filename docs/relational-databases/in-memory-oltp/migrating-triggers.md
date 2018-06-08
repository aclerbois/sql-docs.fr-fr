---
title: Migration de déclencheurs | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ad5385c5-5a50-40ca-a319-97d5606b8511
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 81f631999a3cf9867c4668f108f4cebdc7a03fe1
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="migrating-triggers"></a>Migration de déclencheurs
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Cette rubrique présente les déclencheurs DDL et les tables optimisées en mémoire.  
  
 Les déclencheurs DML sont pris en charge sur les tables optimisées en mémoire, mais uniquement avec l’événement déclencheur FOR | AFTER. Pour obtenir un exemple, consultez [Implémentation d’UPDATE avec FROM ou des sous-requêtes](../../relational-databases/in-memory-oltp/implementing-update-with-from-or-subqueries.md). 
  
 Les déclencheurs LOGON sont des déclencheurs définis pour se déclencher sur les événements LOGON. Ces déclencheurs n'affectent pas les tables mémoire optimisées.  
  
## <a name="ddl-triggers"></a>Déclencheurs DDL  
 Les déclencheurs DDL sont des déclencheurs définis pour se déclencher lorsqu'une instruction CREATE, ALTER, DROP, GRANT, DENY, REVOKE ou UPDATE STATISTICS est exécutée sur la base de données ou sur le serveur sur lequel elle est définie.  
  
 Il est impossible de créer des tables mémoire optimisées si la base de données ou le serveur contient un ou plusieurs déclencheurs DDL définis sur l'événement CREATE_TABLE ou un groupe d'événements qui inclut cet événement. Il est impossible de supprimer une table mémoire optimisée si la base de données ou le serveur contient un ou plusieurs déclencheurs DDL définis sur l'événement DROP_TABLE ou un groupe d'événements qui inclut cet événement.  
  
 Il est impossible de créer des procédures stockées compilées en mode natif s'il existe un ou plusieurs déclencheurs DDL définis sur les événements CREATE_PROCEDURE, DROP_PROCEDURE ou un groupe d'événements qui inclut ces événements.  
  
## <a name="see-also"></a> Voir aussi  
 [Migration vers OLTP en mémoire](../../relational-databases/in-memory-oltp/migrating-to-in-memory-oltp.md)  
  
  
