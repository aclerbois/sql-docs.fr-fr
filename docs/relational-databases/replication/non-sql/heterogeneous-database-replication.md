---
title: Réplication de bases de données hétérogènes | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- heterogeneous database replication, about heterogeneous database replication
- replication [SQL Server], heterogeneous database replication
- heterogeneous database replication
ms.assetid: 3fd983ad-e206-45db-9054-417c9b5bb815
caps.latest.revision: 41
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 77bf8eb00bdc54901e3b84250319212f1eac22a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="heterogeneous-database-replication"></a>réplication de bases de données hétérogènes  
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge les scénarios divers suivants pour la réplication transactionnelle et d'instantané :  
  
-   Publication de données de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers des Abonnés non[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  

-   La publication de données sur et depuis Oracle présente les restrictions suivantes :  
  | |Version 2016 ou antérieure |Version 2017 ou ultérieure |
  |-------|-------|--------|
  |Réplication depuis Oracle |Prise en charge d’Oracle 10g ou version antérieure uniquement |Prise en charge d’Oracle 10g ou version antérieure uniquement |
  |Réplication vers Oracle |Jusqu’à Oracle 12c |Non pris en charge |


 La réplication hétérogène sur les abonnés non SQL Server est déconseillée. La publication Oracle est déconseillée. Pour déplacer des données, créez des solutions à l'aide de la Capture de données modifiées et [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="publishing-data-from-oracle"></a>Publication de données à partir d'Oracle  
 Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour publier des données à partir d'Oracle avec la plupart des fonctions et la même facilité qu'offre la réplication transactionnelle et d'instantané de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Cette fonctionnalité requiert la version d’Oracle 10g ou une version antérieure. La publication de données à partir d'Oracle est idéale pour les scénarios suivants :  
  
|Scénario|Description|  
|--------------|-----------------|  
|Déploiements d'applications[!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework|Développez avec [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en exploitant des données répliquées à partir d’une base de données non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|Serveurs de secours pour l'entreposage des données|Gardez les bases de données intermédiaires [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] synchronisées avec une base de données non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|Migration vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Testez votre application en temps réel sur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tout en répliquant les changements du système source. Basculez vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lorsque vous êtes satisfait de la migration.|  
  
 Pour plus d’informations, consultez [Présentation de la publication Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md).  
  
## <a name="publishing-data-to-non-sql-server-subscribers"></a>Publication de données vers des Abonnés non-SQL Server  
 Les bases de données non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont prises en charge en tant qu'Abonnés aux publications transactionnelles et d'instantané :  
  
-   Oracle pour toutes les plateformes prises en charge par Oracle.  
  
-   IBM DB2 pour AS400, MVS, Unix, Linux et Windows.  
  
 Pour plus d’informations, consultez [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
  
