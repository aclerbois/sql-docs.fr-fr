---
title: Créer un environnement multiserveur | Microsoft Docs
ms.custom: ''
ms.date: 01/30/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, multiserver environments
- master servers [SQL Server], about master servers
- target servers [SQL Server], about target servers
- multiserver environments [SQL Server]
ms.assetid: edc2b60d-15da-40a1-8ba3-f1d473366ee6
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 48b5e195f437b3bc173869ac7d8a7334c741e2b2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-multiserver-environment"></a>Créer un environnement multiserveur
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

L'administration multiserveur nécessite que vous configuriez un serveur maître (MSX) et au moins un serveur cible (TSX). Les travaux qui seront traités sur l'ensemble des serveurs cibles sont d'abord définis sur le serveur maître, puis téléchargés sur les serveurs cibles.  
  
Par défaut, le chiffrement SSL (Secure Sockets Layer) complet et la validation de certificats sont activés pour les connexions entre les serveurs maîtres et les serveurs cible par défaut. Pour plus d’informations, consultez [Définir des options de chiffrement sur des serveurs cibles](../../ssms/agent/set-encryption-options-on-target-servers.md).  
  
Si vous avez un grand nombre de serveurs cibles, évitez de définir votre serveur maître sur un serveur de production qui doit assurer des performances élevées pour d'autres fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , en effet, le trafic du serveur cible peut ralentir les performances du serveur de production. De plus, si vous transmettez des événements vers un serveur maître dédié, vous pouvez centraliser l'administration sur un seul serveur. Pour plus d’informations, consultez [Gérer les événements](../../ssms/agent/manage-events.md).  
  
> [!NOTE]  
> Pour utiliser le traitement de travaux multiserveurs, le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent doit être membre du rôle de base de données **TargetServersRole** de la base de données **msdb** sur le serveur maître. L'Assistant Serveur maître ajoute automatique le compte de service à ce rôle au cours du processus d'inscription.  
  
## <a name="considerations-for-multiserver-environments"></a>Considérations relatives aux environnements multiserveurs  
  
Tenez compte des éléments ci-dessous avant de créer un environnement multiserveur :  
  
-   Utilisez la version la plus récente comme serveur maître. La version actuelle et les deux précédentes sont prises en charge.

-   Chaque serveur cible dépend d’un seul serveur maître. Vous devez annuler l'inscription d'un serveur cible d'un serveur maître avant de pouvoir l'inscrire sur un autre.  
  
-   Lors de la modification du nom d'un serveur cible, vous devez annuler l'inscription de celui-ci avant de modifier son nom puis l'inscrire de nouveau.  
  
-   Si vous souhaitez démanteler une configuration multiserveur, vous devez annuler l'inscription de tous les serveurs cibles dans le serveur maître.  
  
-   SQL Server Integration Services ne prend en charge que les serveurs cibles qui présentent la même version ou une version ultérieure à la version du serveur maître.  
  
## <a name="related-tasks"></a>Related Tasks  
Les rubriques suivantes décrivent les tâches généralement impliquées dans la création d'un environnement multiserveur.  
  
|Description|Rubrique|  
|---------------|---------|  
|Décrit comment créer un serveur maître.|[Créer un serveur maître](../../ssms/agent/make-a-master-server.md)|  
|Décrit comment créer un serveur cible.|[Créer un serveur cible](../../ssms/agent/make-a-target-server.md)|  
|Décrit comment inscrire un serveur cible sur un serveur maître.|[Inscrire un serveur cible dans un serveur maître](../../ssms/agent/enlist-a-target-server-to-a-master-server.md)|  
|Décrit comment désinscrire un serveur cible d'un serveur maître.|[Annuler l’inscription d’un serveur cible dans un serveur maître](../../ssms/agent/defect-a-target-server-from-a-master-server.md)|  
|Décrit comment annuler l'inscription de plusieurs serveurs cibles d'un serveur maître.|[Annuler l’inscription de plusieurs serveurs cibles dans un serveur maître](../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)|  
|Décrit comment vérifier l'état d'un serveur cible.|[sp_help_targetserver (Transact-SQL)](http://msdn.microsoft.com/en-us/f841d3bd-901a-4980-ad0b-1c6eeba3f717)<br /><br />[sp_help_targetservergroup (Transact-SQL)](http://msdn.microsoft.com/en-us/ec3a4a68-b591-431c-9518-053ede522d0c)|  
  
## <a name="see-also"></a> Voir aussi  
[Résoudre les problèmes liés aux travaux multiserveurs qui utilisent des proxys](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md)  
  
