---
title: affinity mask (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- default affinity mask option
- reloading processor cache
- processor cache [SQL Server]
- CPU [SQL Server], licensing
- deferred process call
- affinity mask option
- processor affinity [SQL Server]
- SMP
- DPC
ms.assetid: 5823ba29-a75d-4b3e-ba7b-421c07ab3ac1
caps.latest.revision: 52
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 746783ff9985f901a806735e2db100bddffed86e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="affinity-mask-server-configuration-option"></a>affinity mask (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez plutôt [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).  
  
 Pour réaliser les travaux multitâches, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows déplace parfois les threads de processus entre les différents processeurs. Même si cela est efficace du point de vue du système d'exploitation, cette activité peut réduire les performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du fait des charges système élevées, puisque chaque cache de processeur est rechargé par des données de façon répétée. L'affectation de processeurs à des threads spécifiques permet d'améliorer les performances dans ces conditions en éliminant les rechargements de processeurs et en réduisant la migration des threads entre les processeurs (réduisant ainsi les changements de contexte) ; une telle association entre un thread et un processeur est appelée affinité du processeur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge l’affinité du processeur au moyen de deux options de masque d’affinité : affinity mask (également appelé **CPU affinity mask**) et affinity I/O mask. Pour plus d’informations sur l’option affinity I/O mask, consultez [affinity Input-Output mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md). La prise en charge de l’affinité du processeur et d’E/S pour les serveurs dotés de 33 à 64 processeurs nécessite l’utilisation supplémentaire de l’[option de configuration de serveur affinity64 mask](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) et de l’[option de configuration du serveur affinity64 Input-Output mask](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md), respectivement.  
  
> [!NOTE]  
>  La prise en charge de l'affinité pour les serveurs dotés de 33 à 64 processeurs est uniquement disponible sur les systèmes d'exploitation 64 bits.  
  
 L'option affinity mask, qui existait dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contrôle dynamiquement l'affinité du processeur.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'option affinity mask peut être configurée sans exiger le redémarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque vous utilisez sp_configure, vous devez exécuter RECONFIGURE ou RECONFIGURE WITH OVERRIDE après la définition d'une option de configuration. Si vous utilisez [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], la modification de l'option de masque d'affinité ne nécessite aucun redémarrage.  
  
 Les modifications apportées aux masques d'affinité s'exécutent dynamiquement, autorisant ainsi le démarrage et l'arrêt à la demande des planificateurs d'UC qui lient les threads de processus au sein de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela peut se produire en cas de changement des conditions sur le serveur. Par exemple, si une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est ajoutée au serveur, il peut être nécessaire d'ajuster l'option affinity mask afin de redistribuer la charge des processeurs.  
  
 Les modifications apportées aux masques de bits d'affinité nécessitent que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] active un nouveau planificateur d'UC et désactive le planificateur d'UC existant. Les nouveaux traitements peuvent alors être traités sur le nouveau planificateur ou sur les planificateurs restants.  
  
 Pour lancer un nouveau planificateur d'UC, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un nouveau planificateur et l'ajoute à la liste de ses planificateurs standard. Le nouveau planificateur n'est pris en compte que pour les nouveaux traitements entrants. Les traitements en cours continuent de s'exécuter sur le même planificateur. Les threads de travail migrent vers le nouveau planificateur lorsqu'ils sont libérés, ou lorsque de nouveaux threads de travail sont créés.  
  
 L'arrêt d'un planificateur exige l'achèvement et la clôture des activités de tous les traitements sur le planificateur. Un planificateur arrêté est marqué hors ligne afin qu'aucun nouveau traitement ne soit planifié sur celui-ci.  
  
 Qu'un nouveau planificateur soit ajouté ou supprimé, les tâches système permanentes, telles que lockmonitor, checkpoint, system task yhread (processing DTC) et signal process, continuent de s'exécuter sur le planificateur alors que le serveur est opérationnel. Ces tâches système permanentes ne migrent pas dynamiquement. Si vous souhaitez redistribuer la charge des processeurs pour ces tâches systèmes entre les planificateurs, il est nécessaire de redémarrer l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d'arrêter un planificateur associé à une tâche système permanente, la tâche continue de s'exécuter sur le planificateur hors ligne (aucune migration). Ce planificateur est lié aux processeurs dans le masque d'affinité modifié et ne doit imposer aucune charge au processeur avec lequel il avait une affinité avant la modification. Le fait de disposer de planificateurs hors ligne supplémentaires n'affecte pas vraiment la charge du système. Si ce n'est pas le cas, il est nécessaire de redémarrer le serveur de bases de données pour reconfigurer ces tâches.  
  
 Les tâches d'affinité d'E/S (telles que lazywriter et logwriter) sont directement affectées par le masque d'affinité d'E/S. Si les tâches lazywriter et logwriter ne possèdent pas d'affinité, elles respectent les mêmes règles que celles définies pour les tâches permanentes telles que lockmonitor ou checkpoint.  
  
 Pour vous assurer que le nouveau masque d'affinité est valide, la commande RECONFIGURE vérifie que les affinités du processeur et d'E/S normales s'excluent mutuellement. Si ce n'est pas le cas, un message d'erreur est envoyé à la session cliente et consigné dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour indiquer qu'un tel paramètre n'est pas recommandé. L'exécution des options RECONFIGURE WITH OVERRIDE autorise les affinités du processeur et celles d'E/S qui ne s'excluent pas mutuellement.  
  
 Si vous spécifiez un masque d'affinité qui tente d'effectuer un mappage à une UC inexistante, la commande RECONFIGURE envoie un message d'erreur à la session cliente et le consigne dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'utilisation de l'option RECONFIGURE WITH OVERRIDE n'a aucun effet dans ce cas, et la même erreur de configuration est à nouveau signalée.  
  
 Vous pouvez également exclure l'activité propre à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des processeurs auxquels le système d'exploitation Windows 2000 ou Windows Server 2003 attribue des charges de travail données. En attribuant la valeur 1 à un bit qui représente un processeur, ce processeur est sélectionné par le moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'affectation des threads. Si vous attribuez la valeur 0 (valeur par défaut) à l’option **affinity mask** , les algorithmes de planification de Microsoft Windows 2000 ou Windows Server 2003 définissent l’affinité du thread. Si vous attribuez à **affinity mask** une valeur différente de zéro, l’affinité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprète cette valeur comme étant un masque de bits indiquant que ces processeurs sont ceux sur lesquels doit porter la sélection.  
  
 La non-exécution des threads [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur des processeurs particuliers permet à Microsoft Windows 2000 ou Windows Server 2003 de mieux évaluer la gestion par le système de traitement des processus propres à Windows. Par exemple, sur un serveur à 8 processeurs exécutant deux instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (instances A et B), l'administrateur système peut utiliser l'option affinity mask pour affecter le premier jeu de 4 processeurs à l'instance A et le deuxième jeu de 4 processeurs à l'instance B. Pour configurer plus de 32 processeurs, définissez à la fois affinity mask et affinity64 mask. Les valeurs de l’option **affinity mask** sont les suivantes :  
  
-   Un masque **affinity mask** à un octet prend en charge jusqu’à huit processeurs sur un ordinateur multiprocesseur.  
  
-   Un masque **affinity mask** à deux octets prend en charge jusqu’à 16 processeurs sur un ordinateur multiprocesseur.  
  
-   Un masque **affinity mask** à trois octets prend en charge jusqu’à 24 processeurs sur un ordinateur multiprocesseur.  
  
-   Un masque **affinity mask** à quatre octets prend en charge jusqu’à 32 processeurs sur un ordinateur multiprocesseur.  
  
-   Si vous disposez de plus de 32 processeurs, configurez un masque d'affinité à quatre octets pour les 32 premiers processeurs et un masque d'affinity64 à quatre octets pour les processeurs restants.  
  
 Étant donné que la définition de l'affinité du processeur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une opération spécialisée, il est recommandé de n'utiliser cette option que lorsque cela est véritablement nécessaire. Dans la plupart des cas, l'option d'affinité par défaut de Microsoft Windows 2000 ou Windows Server 2003 offre des performances optimales. Lors de la définition des masques d'affinité, tenez également compte des exigences des autres applications en matière de processeurs. Pour plus d'informations, consultez la documentation du système d'exploitation Windows.  
  
> [!NOTE]  
>  Vous pouvez utiliser le Moniteur système Windows pour afficher et analyser l'utilisation des processeurs individuels.  
  
 Si vous spécifiez l'option affinityI/O mask, vous devez l'utiliser conjointement avec l'option de configuration affinity mask. Évitez d’activer sur le même processeur le commutateur affinity I/O mask et l’option **affinity mask** . Les bits correspondant à chaque processeur doivent être dans l'un des trois états suivants :  
  
-   0 dans l'option affinity mask et dans l'option affinity I/O mask.  
  
-   1 dans l'option affinity mask et 0 dans l'option affinity I/O mask.  
  
-   0 dans l'option affinity mask et 1 dans l'option affinity I/O mask.  
  
> [!CAUTION]  
>  Évitez de configurer l'affinité de processeur dans le système d'exploitation Windows et le masque d'affinité dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces paramètres tentent d'obtenir le même résultat et, si les configurations sont incohérentes, les résultats risquent d'être imprévisibles. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il est préférable de configurer l’affinité de processeur à l’aide de l’option sp_configure dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a> Exemple  
 Par exemple, pour définir l'option affinity mask, si les processeurs 1, 2 et 5 sont sélectionnés comme étant disponibles et si les bits 1, 2 et 5 ont la valeur 1 alors que les bits 0, 3, 4, 6 et 7 ont la valeur 0, une valeur hexadécimale égale à 0x26 ou l'équivalent décimal `38` est spécifiée. Numérotez les bits de droite à gauche. L'option affinity mask commence à compter les processeurs de 0 à 31, de sorte que dans l'exemple suivant, le compteur `1` représente le deuxième processeur sur le serveur.  
  
```  
sp_configure 'show advanced options', 1;  
RECONFIGURE;  
GO  
sp_configure 'affinity mask', 38;  
RECONFIGURE;  
GO  
```  
  
 Vous trouverez ci-dessous les valeurs de l’option **affinity mask** pour un système à huit processeurs.  
  
|Valeur décimale|Masque binaire|Autorise les threads SQL Server sur les processeurs|  
|-------------------|---------------------|--------------------------------------------|  
| 1|00000001|0|  
|3|00000011|0 et 1|  
|7|00000111|0, 1 et 2|  
|15|00001111|0, 1, 2 et 3|  
|31|00011111|0, 1, 2, 3 et 4|  
|63|00111111|0, 1, 2, 3, 4 et 5|  
|127|01111111|0, 1, 2, 3, 4, 5 et 6|  
|255|11111111|0, 1, 2, 3, 4, 5, 6 et 7|  
  
 L'option affinity mask est une option avancée. Si vous utilisez la procédure stockée système sp_configure pour changer sa valeur, vous ne pouvez modifier l’option **affinity mask** que si la valeur 1 a été attribuée à l’option **show advanced options** . Après l'exécution de la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] RECONFIGURE, le nouveau paramètre prend immédiatement effet sans redémarrage de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="non-uniform-memory-access-numa"></a>Non-Uniform Memory Access (NUMA)  
 Lorsque vous utilisez l'accès NUMA (Non-Uniform Memory Access) matériel et que le masque d'affinité est défini, chaque planificateur dans un nœud possède une affinité avec sa propre unité centrale. Lorsque le masque d'affinité n'est pas défini, chaque planificateur possède une affinité avec le groupe d'unités centrales au sein du nœud NUMA et un planificateur mappé sur le nœud NUMA N1 peut planifier du travail sur toute unité centrale dans le nœud, mais pas sur des unités centrales associées à un autre nœud.  
  
 Toute opération en cours sur un nœud NUMA unique peut utiliser uniquement des pages tampon à partir de ce nœud. Lorsqu'une opération est exécutée en parallèle sur des unités centrales issues de plusieurs nœuds, de la mémoire peut être utilisée à partir de tout nœud impliqué.  
  
## <a name="licensing-issues"></a>À propos des licences  
 L'affinité dynamique dépend étroitement des licences des processeurs. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas la configuration d'options de masque d'affinité qui enfreignent la stratégie de licences.  
  
### <a name="startup"></a>Démarrage  
 Si un masque d'affinité spécifié ne respecte pas la stratégie de licences au moment du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de l'attachement de la base de données, la couche du moteur achève le processus de démarrage ou l'opération de restauration/attachement de base de données, puis remet à zéro la valeur d'exécution sp_configure du masque d'affinité, en consignant un message d'erreur dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="reconfigure"></a>Reconfiguration  
 Si un masque d'affinité spécifié ne respecte pas la stratégie de licences lors de l'exécution de la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] RECONFIGURE, un message d'erreur est envoyé à la session cliente et est consigné dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , exigeant ainsi que l'administrateur de la base de données reconfigure le masque d'affinité. Aucune commande RECONFIGURE WITH OVERRIDE n'est acceptée dans ce cas.  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md)  
  
  
