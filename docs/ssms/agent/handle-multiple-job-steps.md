---
title: Gérer plusieurs étapes de travail | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- ordering job steps [SQL Server]
- multiple job steps
- SQL Server Agent jobs, job steps
- control of flow for jobs [SQL Server]
ms.assetid: 7aba19ff-72b3-45f6-8e54-23f4988d63a8
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3b88a2d50bd748da3e56d43367706715fa4739f4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handle-multiple-job-steps"></a>Gérer plusieurs étapes de travail
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Si votre travail est composé de plusieurs étapes, vous devez spécifier l'ordre dans lequel celles-ci sont exécutées. Ceci s’appelle *contrôle de flux**.* Vous pouvez ajouter de nouvelles étapes de travail et réorganiser le flux des étapes de travail à tout moment ; les modifications prennent effet lors de l'exécution suivante du travail. Cette illustration montre le contrôle de flux pour un travail de sauvegarde de base de données.  
  
![Contrôle du flux des étapes du travail de SQL Server Agent](../../ssms/agent/media/dbflow01.gif "Contrôle du flux des étapes du travail de SQL Server Agent")  
  
La première étape est la sauvegarde de la base de données. Si cette étape échoue, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] signale l'échec à l'opérateur destinataire des notifications. Si l'étape de la sauvegarde de la base de données réussit, le travail se poursuit avec l'étape suivante, en l'occurrence la purge des données client. Si cette étape échoue, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] passe à l'étape de restauration de la base de données. Si la purge des données client réussit, le travail se poursuit avec l'étape suivante, en l'occurrence la mise à jour des statistiques, et ainsi de suite jusqu'à la dernière étape, qui se solde par la réussite ou par l'échec du rapport.  
  
Vous définissez une action de contrôle de flux pour la réussite ou l'échec de chaque étape de travail. Vous devez préciser l'action à exécuter lors de la réussite ou de l'échec d'une étape de travail. Vous pouvez également définir le nombre de tentatives de reprises pour les étapes de travail ayant échoué ainsi que leurs intervalles.  
  
> [!NOTE]  
> Lorsque vous utilisez l'interface graphique utilisateur de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et que vous supprimez une ou plusieurs étapes à partir d'un travail multitâches, l'interface supprime toutes les étapes de travail puis rajoute les étapes restantes avec les références correctes en cas de réussite ou en cas d'échec. Par exemple, supposons que vous avez un travail comportant cinq étapes, et que la première étape est configurée pour passer à l'étape 4 si elle aboutit. Si vous supprimez l'étape 3, l'interface utilisateur graphique supprime toutes les étapes de ce travail et ajoute les quatre étapes restantes (1, 2, 4 et 5) avec les références corrigées. Dans ce cas, la référence de l'étape 1 est configurée pour passer à l'étape 3 si l'étape 1 aboutit.  
  
Les étapes de travail doivent être autonomes. En d'autres termes, un travail ne peut pas passer des valeurs booléennes, des données ou des valeurs numériques entre des étapes de travail. Toutefois, vous pouvez passer des valeurs d'une étape de travail [!INCLUDE[tsql](../../includes/tsql_md.md)] à une autre en utilisant des tables permanentes ou des tables temporaires globales. À partir des étapes de travail, vous pouvez transmettre des valeurs afin d'exécuter, d'une étape à l'autre, des programmes qui effectuent des opérations sur des fichiers. Par exemple, le programme exécuté par une étape de travail écrit un fichier, tandis que celui exécuté par une étape de travail ultérieure le lit.  
  
> [!NOTE]  
> Si vous créez des étapes de travail en boucle, (l'étape de travail 1 est suivie par l'étape de travail 2, puis l'étape de travail 2 revient à l'étape de travail 1), un message d'avertissement apparaît lors de la création du travail avec [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)].  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent enregistre dans l’historique des travaux les informations relatives aux travaux et aux étapes de travail.  
  
## <a name="see-also"></a> Voir aussi  
[sp_add_job](http://msdn.microsoft.com/en-us/6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274)  
[sysjobhistory](http://msdn.microsoft.com/en-us/1b1fcdbb-2af2-45e6-bf3f-e8279432ce13)  
[sysjobs (Transact-SQL)](http://msdn.microsoft.com/en-us/e244a6a5-54c2-47a6-8039-dd1852b0ae59)  
[sysjobsteps](http://msdn.microsoft.com/en-us/978b8205-535b-461c-91f3-af9b08eca467)  
[Implémenter des travaux](../../ssms/agent/implement-jobs.md)  
[Gérer les étapes de travail](../../ssms/agent/manage-job-steps.md)  
  
