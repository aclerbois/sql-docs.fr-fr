---
title: affinity Input-Output mask (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- affinity I/O mask option
- processor affinity [SQL Server]
- binding processors [SQL Server]
- CPU affinity mask option
ms.assetid: 9950a8c9-9fe0-4003-95df-6f0d1becb0e7
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: c449fccc6aa19b85329d75272119d16e6fb5802c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="affinity-input-output-mask-server-configuration-option"></a>affinity Input-Output mask (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Pour réaliser les travaux multitâches, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows déplace parfois les threads de processus entre les différents processeurs. Bien qu’efficace du point de vue du système d’exploitation, cette activité peut réduire les performances de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous des charges de système intenses, car des données sont rechargées de façon répétée dans chaque cache de processeur. L'affectation de processeurs à des threads spécifiques peut améliorer les performances sous ces conditions en éliminant les rechargements de processeur ; une telle association entre un thread et un processeur est qualifiée d'affinité de processeur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge l’affinité du processeur au moyen de deux options de masque d’affinité : **affinity mask** (également appelé **CPU affinity mask**) et **affinity I/O mask**. Pour plus d’informations sur l’option **affinity mask** , consultez [affinity mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md). La prise en charge de l’affinité du processeur et d’E/S pour les serveurs dotés de 33 à 64 processeurs nécessite l’utilisation supplémentaire de [l’option de configuration de serveur affinity64 mask](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) et de [l’option de configuration du serveur affinity64 Input-Output mask](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md) , respectivement.  
  
> [!NOTE]  
>  La prise en charge de l'affinité pour les serveurs dotés de 33 à 64 processeurs est uniquement disponible sur les systèmes d'exploitation 64 bits.  
  
 L’option **affinity I/O mask** lie les E/S disque de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à un sous-ensemble de processeurs spécifié. Dans les environnements de traitement transactionnel en ligne (OLTP) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] haut de gamme, cette extension permet d'améliorer les performances des threads [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] émettant des E/S. Cette amélioration ne prend pas en charge l'affinité matérielle pour les disques ou les contrôleurs de disques individuels.  
  
 La valeur de **affinity I/O mask** spécifie quels processeurs dans un ordinateur multiprocesseur sont éligibles pour le traitement d’opérations d’E/S disque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le masque est un bitmap dans lequel le bit le plus à droite spécifie le processeur d'ordre le plus bas CPU(0), le bit à sa gauche immédiate spécifie le processeur d'ordre le plus bas suivant CPU(1), etc. Pour configurer plus de 32 processeurs, définissez les options **affinity I/O mask** et **affinity64 I/O mask**.  
  
 Les valeurs de l’option **affinity I/O mask** sont les suivantes :  
  
-   Un masque **affinity I/O mask** à 1 octet prend en charge jusqu’à 8 processeurs sur un ordinateur multiprocesseur.  
  
-   Un masque **affinity I/O mask** à 2 octets prend en charge jusqu’à 16 processeurs sur un ordinateur multiprocesseur.  
  
-   Un masque **affinity I/O mask** à 3 octets prend en charge jusqu’à 24 processeurs sur un ordinateur multiprocesseur.  
  
-   Un masque **affinity I/O mask** à 4 octets prend en charge jusqu’à 32 processeurs sur un ordinateur multiprocesseur.  
  
-   Pour prendre en charge plus de 32 processeurs, configurez un masque **affinity I/O mask** à 4 octets pour les 32 premiers processeurs et un masque **affinity64 I/O mask** jusqu’à 4 octets pour les autres processeurs.  
  
 Un bit à 1 dans le modèle d'affinité d'E/S spécifie que le processeur correspondant peut effectuer des opérations d'E/S de disque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; un bit à 0 spécifie qu'aucune opération d'E/S de disque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne doit être planifiée pour le processeur correspondant. Lorsque tous les bits sont à zéro ou lorsque l’option **affinity I/O mask** n’est pas spécifiée, l’E/S disque de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est confiée à n’importe lequel des processeurs éligibles au traitement de threads [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le paramétrage de l’option [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **affinity I/O mask** étant une opération spécialisée, elle ne doit être utilisée qu’exceptionnellement. Dans la plupart des cas, l’option d’affinité par défaut de Windows 2000 ou de Windows Server 2003 offre des performances optimales.  
  
 Lorsque vous spécifiez l’option **affinity I/O mask** , vous devez l’utiliser avec l’option de configuration **affinity mask** . Évitez d’activer sur le même processeur le commutateur **affinity I/O mask** et l’option **affinity mask** . Les bits correspondant à chaque processeur doivent être dans l'un des trois états suivants :  
  
-   0 pour l’option **affinity I/O mask** et pour l’option **affinity mask** .  
  
-   1 pour l’option **affinity I/O mask** et 0 pour l’option **affinity mask** .  
  
-   0 pour l’option **affinity I/O mask** et 1 pour l’option **affinity mask** .  
  
 L’option **affinity I/O mask** est une option avancée. Si vous utilisez la procédure stockée système **sp_configure** pour changer sa valeur, vous ne pouvez modifier l’option **affinity I/O mask** que si la valeur 1 a été attribuée à l’option **show advanced options** . Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], reconfigurer l’option **affinity I/O mask** requiert un redémarrage de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!CAUTION]  
>  Évitez de configurer l'affinité de processeur dans le système d'exploitation Windows et le masque d'affinité dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces paramètres tentent d'obtenir le même résultat et, si les configurations sont incohérentes, les résultats risquent d'être imprévisibles. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L’affinité de processeur de **est configurée de façon optimale à l’aide de l’option** sp_configure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
