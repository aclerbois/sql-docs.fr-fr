---
title: MSdistpublishers (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-tables
ms.reviewer: 
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: 
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSdistpublishers
- MSdistpublishers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistpublishers system table
ms.assetid: 31844099-4b33-4dc9-84b4-bac70aa82598
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 39435281a7b2eb1ab26648c8b768a3abb8d4777f
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="msdistpublishers-transact-sql"></a>MSdistpublishers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]Le **MSdistpublishers** table contient une ligne pour chaque serveur de publication distant pris en charge par le serveur de distribution local. Cette table est stockée dans le **msdb** base de données.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom du serveur de distribution du serveur de publication.|  
|**distribution_db**|**sysname**|Le nom de la base de données de distribution.|  
|**working_directory**|**nvarchar(255)**|Le nom du répertoire de travail utilisé pour stocker les fichiers de données et le schéma de la publication.|  
|**security_mode**|**int**|Mode de sécurité implémenté sur le serveur de distribution :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.<br /><br /> **1** = authentification Windows.|  
|**connexion**|**sysname**|L’ID de connexion pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**mot de passe**|**nvarchar (524)**|Mot de passe (chiffré) pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Active**|**bit**|Indique si le serveur de distribution local est utilisé par le serveur de publication distant.|  
|**approuvé**|**bit**|Indique si le serveur de publication distant utilise le même mot de passe que le serveur de distribution local :<br /><br /> **0** = un mot de passe est nécessaire sur le serveur de publication distant pour se connecter au serveur de distribution.<br /><br /> **1** = aucun mot de passe est nécessaire.|  
|**third_party**|**bit**|Spécifie si le serveur de publication est une installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installation. **1** = source de données hétérogènes.|  
|**publisher_type**|**sysname**|Type du serveur de publication :<br /><br /> **MSSQLSERVER**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.<br /><br /> **ORACLE** = serveur de publication Oracle standard.<br /><br /> **ORACLE GATEWAY** = serveur de publication Oracle Gateway.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40; Transact-SQL &#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
