---
title: Modifier la durée de récupération cible d’une base de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e466419a-d8a4-48f7-8d97-13a903ad6b15
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7b61581cff893c7f338e638fd944beabe2842ab0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-the-target-recovery-time-of-a-database-sql-server"></a>Modifier la durée de récupération cible d'une base de données (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment modifier le temps de récupération cible d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par défaut, le temps de récupération cible est de 60 secondes, et la base de données utilise des *points de contrôle indirects*. Le temps de récupération cible établit une limite supérieure sur le temps de récupération pour cette base de données.  
  
> [!NOTE]  
>  La limite supérieure spécifiée pour une base de données spécifique par son paramètre de temps de récupération cible peut être dépassée si une transaction longue entraîne des durées UNDO excessives.  
  
-   **Avant de commencer :**  [Limitations et restrictions](#Restrictions), [Sécurité](#Security)  
  
-   **Pour modifier le temps de récupération cible, à l’aide de :**  [SQL Server Management Studio](#SSMSProcedure) ou de [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions 
  
> [!CAUTION]  
>  Une charge de travail transactionnelle en ligne sur une base de données configurée pour les points de contrôle indirects peut rencontrer une dégradation des performances. Les points de contrôle indirects permettent de s’assurer que le nombre de pages de modifications est inférieur à un certain seuil afin que la récupération de la base de données se termine dans le temps de récupération cible. L’option de configuration de l’intervalle de récupération utilise le nombre de transactions pour déterminer le temps de récupération, contrairement aux points de contrôle indirects qui utilisent le nombre de pages de modifications. Quand des points de contrôle indirects sont activés sur une base de données recevant un grand nombre d’opérations DML, l’enregistreur en arrière-plan peut commencer à vider de manière intense les mémoires tampons modifiées sur le disque afin de s’assurer que le délai nécessaire à la récupération se situe dans dans le temps de récupération cible défini de la base de données. Cela peut entraîner une activité supplémentaire en termes d’E/S sur certains systèmes, ce qui peut contribuer à un goulot d’étranglement des performances si le sous-système du disque fonctionne au-delà du seuil d’E/S ou s’en rapproche.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier le temps de récupération cible**  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]et développez-la.  
  
2.  Cliquez avec le bouton droit sur la base de données à modifier, puis sélectionnez la commande **Propriétés** .  
  
3.  Dans la boîte de dialogue **Propriétés de la base de données** , cliquez sur la page **Options** .  
  
4.  Dans le volet **Récupération** , dans le champ **Temps de récupération cible (secondes)** , spécifiez le nombre de secondes de votre choix pour définir la limite supérieure du temps de récupération de cette base de données.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier le temps de récupération cible**  
  
1.  Connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où réside la base de données.  
  
2.  Utilisez l'instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md)suivante, comme suit :  
  
     TARGET_RECOVERY_TIME **=***target_recovery_time* { SECONDS | MINUTES }  
  
     *target_recovery_time*  
     À partir de [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)], la valeur par défaut est de 1 minute. Lorsque la valeur est supérieure à 0 (valeur par défaut pour les versions antérieures), spécifie la limite supérieure du temps de récupération de la base de données spécifiée en cas de sinistre.  
  
     SECONDS  
     Indique que *target_recovery_time* correspond au nombre de secondes.  
  
     MINUTES  
     Indique que *target_recovery_time* correspond au nombre de minutes.  
  
     L'exemple suivant définit le temps de récupération cible de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur `60` secondes.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET TARGET_RECOVERY_TIME = 60 SECONDS;  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
