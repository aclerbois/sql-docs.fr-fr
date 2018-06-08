---
title: Définir le seuil et la durée d’inactivité de l’UC (SQL Server Management Studio) | Microsoft Docs
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
- CPU [SQL Server], idle conditions
- time [SQL Server], CPU idle and duration
- duration of CPU idle [SQL Server]
- SQL Server Agent, CPU idle conditions
- idle time [SQL Server]
ms.assetid: 8647b465-d899-4cc7-9640-134a506d0a2e
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 43ae32554b49df98f6e6ed4118b897e65d4f2246
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-cpu-idle-time-and-duration-sql-server-management-studio"></a>Définir le seuil et la durée d'inactivité de l'UC (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique indique comment définir la condition d'inactivité de l'UC pour votre serveur dans [!INCLUDE[ssCurrent](../../includes/sscurrent_md.md)] au moyen de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)]. La définition d'inactivité d'UC a une incidence sur la réponse aux événements de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Par exemple, supposons que vous définissez la condition d'inactivité de l'UC comme une utilisation UC moyenne tombant sous 10 % et restant à ce niveau pendant 10 minutes. Ensuite, si vous avez défini des travaux à exécuter lorsque l'UC du serveur atteint une condition d'inactivité, le travail démarre lorsque l'utilisation de l'UC tombe sous 10 % et reste à ce niveau pendant 10 minutes. S'il s'agit d'un travail qui a un impact significatif sur les performances de votre serveur, la définition de la condition d'inactivité de l'UC est importante.  
  
## <a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-cpu-idle-time-and-duration"></a>Pour définir le seuil et la durée d'inactivité de l'UC  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Cliquez avec le bouton droit sur **Agent SQL Server**, cliquez sur **Propriétés**, puis sélectionnez la page **Avancé** .  
  
3.  Sous **Condition d'inactivité de l'UC**, procédez comme suit :  
  
    -   Activez la case à cocher **Définir la condition d'inactivité de l'UC**.  
  
    -   Spécifiez un pourcentage pour la zone **La moyenne d’utilisation de l’UC tombe en dessous de** (tous les UC). Cette intervention définit le niveau d'utilisation sous lequel l'UC doit tomber avant d'être considérée inactive.  
  
    -   Spécifiez un nombre de secondes pour la zone **Et reste sous ce niveau pendant** . Cette intervention définit la durée d'utilisation d'UC minimale au terme de laquelle l'UC est considérée inactive.  
  
