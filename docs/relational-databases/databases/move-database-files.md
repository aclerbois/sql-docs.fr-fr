---
title: Déplacer des fichiers de bases de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], moving database files
- files [SQL Server], moving
- data files [SQL Server], moving
- file moving [SQL Server]
- moving full-text catalogs
- moving databases
- full-text catalogs [SQL Server], moving
- moving database files
- scheduled disk maintenance [SQL Server]
- moving files
- relocating database files
- planned database relocations [SQL Server]
- databases [SQL Server], moving
ms.assetid: 89f01b10-5fae-4ed8-b0fb-a4b9f540fd28
caps.latest.revision: 34
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a92b002e6590903ae90f584e7b87d565df7272fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="move-database-files"></a>Déplacer des fichiers de bases de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez déplacer les bases de données système et utilisateur en spécifiant le nouvel emplacement des fichiers dans la clause FILENAME de l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) . Cette méthode peut être appliquée aux fichiers de données, aux fichiers journaux et aux fichiers de catalogues de texte intégral. Elle peut être utile dans les cas suivants :  
  
-   Récupération après défaillance. Par exemple, la base de données est en mode suspect ou a été fermée en raison d'une défaillance matérielle.  
  
-   Déplacement prévu.  
  
-   Déplacement en vue d'une maintenance de disque planifiée.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Déplacer des bases de données utilisateur](../../relational-databases/databases/move-user-databases.md)|Décrit les procédures permettant de déplacer les fichiers de base de données utilisateur et les fichiers de catalogue de texte intégral vers un nouvel emplacement.|  
|[Déplacer des bases de données système](../../relational-databases/databases/move-system-databases.md)|Décrit les procédures permettant de déplacer les fichiers de base de données système vers un nouvel emplacement.|  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
  
  
