---
title: SQL Server, objet Databases | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Databases object
- SQLServer:Databases
- Availability Groups [SQL Server], performance counters
ms.assetid: a7f9e7d4-fff4-4c72-8b3e-3f18dffc8919
caps.latest.revision: 40
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2349e70ca346a28743b5161ff1d2d554b4914c44
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-databases-object"></a>SQL Server, objet Databases
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L’objet **SQLServer:Databases** dans SQL Server fournit des compteurs pour analyser les opérations de copie en bloc, le débit des sauvegardes et des restaurations, ainsi que l’activité des journaux des transactions. Surveillez les transactions et le journal des transactions pour déterminer l'intensité de l'activité de l'utilisateur dans la base de données et le taux de remplissage du journal des transactions. Le volume d'activité de l'utilisateur peut déterminer les performances de la base de données et affecter la taille du journal, le verrouillage et la réplication. La surveillance de l'activité du journal de bas niveau afin de mesurer l'activité de l'utilisateur et l'exploitation des ressources peut permettre d'identifier les goulots d'étranglement des performances.  
  
 Plusieurs instances de l’objet **Databases** , chacune représentant une seule base de données, peuvent être analysées simultanément.  
  
 Le tableau suivant décrit les compteurs **Databases** SQL Server.  
  
|Compteurs Bases de données SQL Server|Description|  
|-----------------------------------|-----------------|  
|**Transactions actives**|Nombre de transactions actives pour la base de données.|  
|**Avg Dist From EOL/LP Request**|Distance moyenne en octets de la fin du journal par requête de pool du journal, pour les requêtes figurant dans le dernier fichier journal virtuel.| 
|**Débit de sauvegarde/restauration/seconde**|Débit de lecture/écriture par seconde pour les opérations de sauvegarde et de restauration d'une base de données. Par exemple, il est possible de mesurer l'évolution des performances de l'opération de sauvegarde d'une base de données si l'on utilise davantage d'unités de sauvegarde en parallèle ou si ces dernières sont plus rapides. Le débit d'une opération de sauvegarde ou de restauration d'une base de données permet de déterminer la progression et les performances de ces opérations.|  
|**Lignes de la copie en bloc/s**|Nombre de lignes copiées en bloc par seconde.|  
|**Débit de la copie en bloc/s**|Quantité de données copiées en bloc (en kilo-octets) par seconde.|  
|**Entrées de la table de validation**|Taille de la partie en mémoire de la table de validation pour la base de données. Pour plus d’informations, consultez [sys.dm_tran_commit_table &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/change-tracking-sys-dm-tran-commit-table.md).|  
|**Taille des fichiers de données (Ko)**|Taille cumulée (en kilo-octets) de tous les fichiers de données de la base de données prenant en compte la croissance automatique. Il est utile d’analyser ce compteur, par exemple pour déterminer la taille appropriée de **tempdb**.|  
|**Octets d'analyse logique DBCC/s**|Nombre d'octets d'analyse de lecture logique par seconde pour les commandes DBCC (Database Console Commands).|  
|**Durée de validation du groupe/sec**|Temps d’attente du groupe (microsecondes) par seconde|
|**Octets de journal vidés/s**|Nombre total d'octets de journal vidés.|  
|**Taux d'accès au cache du journal**|Pourcentage de lectures du cache du journal satisfaites à partir du cache du journal.|  
|**Base du taux d’accès au cache de journal**|À usage interne uniquement| 
|**Lectures du cache du journal/s**|Lectures réalisées par seconde à partir du cache du gestionnaire du journal.|  
|**Taille des fichiers journaux (Ko)**|Taille cumulée (en kilo-octets) de tous les fichiers du journal des transactions dans la base de données.|  
|**Taille de fichier(s) journal(aux) utilisée (Ko)**|La taille utilisée cumulée de tous les fichiers journaux de la base de données.|  
|**Temps d'attente de vidage du journal**|Temps d'attente total (en millisecondes) pour vider le journal. Sur une base de données secondaire Always On, cette valeur indique le temps d’attente pour les enregistrements de journal à sécuriser de manière renforcée sur le disque.|  
|**Attentes de vidage du journal/s**|Nombre de validations par seconde en attente du vidage du journal.|  
|**Temps d'attente de vidage du journal (ms)**|Temps en millisecondes des vidages du journal au cours de la dernière seconde.|  
|**Vidages du journal/s**|Nombre de vidages du journal par seconde.|  
|**Croissances de journal**|Nombre total d'extensions du journal des transactions pour la base de données.|  
|**Journaliser les absences dans le cache/s du pool**|Nombre de requêtes pour lesquelles le bloc de journal n'est pas disponible dans le pool de journaux. Le *pool de journaux* est un cache en mémoire du journal des transactions. Ce cache est utilisé pour optimiser la lecture du journal à des fins de récupération, réplication des transactions, annulation et [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].|  
|**Journaliser les lectures du disque/s du pool**|Nombre de lectures du disque émise par le pool du jourrnal pour extraire des blocs du journal.|  
|**Journaliser les suppressions/s de hachage du pool**|Taux de suppressions d’entrée de hachage brut du pool du journal.|
|**Insertions de hachage dans le pool du journal/s**|Taux d’insertions d’entrées de hachage brut dans le pool du journal.|
|**Journaliser les entrées/s de hachage non valide du pool**|Taux de recherches de hachage qui échouent parce qu’elles sont non valides.|
|**Journaliser les push/s de l’analyse de journal du pool**|Taux de push de bloc de journal par analyse de journal, qui peuvent venir du disque ou de la mémoire.|
|**Journaliser les push/s du LogWriter du pool**|Taux de push de bloc de journal par thread de l’enregistreur de journal.|
|**Envois au pool du journal – Pool libre vide/seconde**|Taux d’échecs d’envoi de blocs du journal dus à un pool libre vide.|
|**Envois au pool du journal – Mémoire insuffisante/seconde**|Taux d’échecs d’envoi de blocs du journal dus à une mémoire insuffisante.|
|**Envois au pool du journal – Pas de mémoire tampon libre/seconde**|Taux d’échecs d’envoi de blocs du journal dus à l’indisponibilité de mémoires tampon libres.|
|**Demande de pool du journal derrière troncation/s**|Absences dans le cache du pool du journal liées à une demande de bloc derrière un LSN de troncation.|
|**Journaliser la base des requêtes du pool**|À usage interne uniquement| 
|**Journaliser le fichier journal virtuel des anciennes requêtes/s du pool**|Journaliser les requêtes du pool qui n’étaient pas dans le dernier fichier journal virtuel du journal.|  
|**Journaliser les requêtes/s du pool**|Nombre de requêtes de bloc du journal traitées par le pool du journal.|  
|**Taille du journal actif total du pool du journal**|Journal actif total actuel stocké dans le gestionnaire de mémoire tampon du cache partagé, en octets.|
|**Taille totale du pool partagé du pool du journal**|Utilisation actuelle totale de la mémoire du gestionnaire de mémoire tampon du cache partagé, en octets.|
|**Compactages de journal**|Nombre total de compactages de journal pour cette base de données.|  
|**Troncatures de journal**|Nombre de fois où le journal des transactions a été réduit.|  
|**Pourcentage utilisé du journal**|Pourcentage de l'espace en cours d'utilisation dans le journal.|  
|**Transactions de réplication en attente**|Nombre de transactions dans le journal des transactions de la base de données de publication, marquées pour la réplication, mais non encore remises à la base de données de distribution.|  
|**Répl. Trans. Fréquence**|Nombre de transactions extraites par seconde du journal des transactions de la base de données de publication et remises à la base de données de distribution.|  
|**Mouvement de réduction de données en octets/s**|Volume de données déplacées par seconde par les opérations Autoshrink, ou par les instructions DBCC SHRINKDATABASE ou DBCC SHRINKFILE.|  
|**Transactions suivies/s**|Nombre de transactions validées enregistrées dans la table de validation pour la base de données.|  
|**Transactions/s**|Nombre de transactions démarrées pour la base de données par seconde.<br /><br /> **Transactions/s** ne tient pas compte des transactions XTP uniquement (transactions commencées par une procédure stockée compilée en mode natif).|  
|**Transactions d'écriture/s**|Nombre des transactions qui ont écrit dans la base de données et qui ont été validées au cours de la dernière seconde.|  
|**Base de latence DLC du contrôleur XTP**|À usage interne uniquement| 
|**Latence DLC du contrôleur XTP/extraction**|Latence moyenne en microsecondes par seconde entre l’entrée des blocs de journal dans le consommateur de journal direct (DLC) et leur récupération par le contrôleur XTP.|
|**Latence maximale DLC du contrôleur XTP**|Latence en microsecondes la plus élevée enregistrée pour une extraction du consommateur de journal direct par le contrôleur XTP.|
|**Traitement du journal par le contrôleur XTP/s**|Quantité d’octets de journal traités par le thread du contrôleur XTP par seconde.|
|**Mémoire XTP utilisée (Ko)**|La quantité de mémoire utilisée par XTP dans la base de données.| 
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, réplica de base de données](../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
  
