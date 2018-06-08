---
title: Administrer et surveiller la capture de données modifiées (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: track-changes
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], monitoring
- change data capture [SQL Server], administering
- change data capture [SQL Server], jobs
ms.assetid: 23bda497-67b2-4e7b-8e4d-f1f9a2236685
caps.latest.revision: 15
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 17d9da96712828ca1a17830ff281a1e7a730f28f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="administer-and-monitor-change-data-capture-sql-server"></a>Administrer et surveiller la capture de données modifiées (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment administrer et surveiller la capture de données modifiées.  
  
##  <a name="Capture"></a> Travail de capture  
 Le travail de capture est démarré par l’exécution de la procédure stockée sans paramètre **sp_MScdc_capture_job**. Cette procédure stockée commence par extraire les valeurs configurées pour *maxtrans*, *maxscans*, *continuous*, et *pollinginterval* pour le travail de capture de msdb.dbo.cdc_jobs. Ces valeurs configurées sont ensuite transférées comme paramètres à la procédure stockée **sp_cdc_scan**. Cela permet d’appeler **sp_replcmds** pour effectuer l’analyse du journal.  
  
### <a name="capture-job-parameters"></a>Paramètres du travail de capture  
 Pour comprendre le comportement du travail de capture, vous devez comprendre comment les paramètres configurables sont utilisés par **sp_cdc_scan**.  
  
#### <a name="maxtrans-parameter"></a>Paramètre maxtrans  
 Le paramètre *maxtrans* spécifie le nombre maximal de transactions qui peuvent être traitées au cours d'un même cycle d'analyse du journal. Si, au cours de l'analyse, le nombre de transactions à traiter atteint cette limite, aucune transaction supplémentaire n'est incluse dans l'analyse en cours. Une fois le cycle d'analyse terminé, le nombre de transactions qui ont été traitées sera toujours inférieur ou égal à *maxtrans*.  
  
#### <a name="maxscans-parameter"></a>Paramètre maxscans  
 Le paramètre *maxscans* spécifie le nombre maximal de cycles d’analyse tentés pour vider le journal avant de retourner (continuous = 0) ou d’exécuter un waitfor (continuous = 1).  
  
#### <a name="continous-parameter"></a>Paramètre continuous  
 Le paramètre *continuous* détermine si **sp_cdc_scan** rend le contrôle après avoir vidé le journal ou après avoir exécuté le nombre maximal de cycles d’analyse (mode en une seule fois). Il détermine également si **sp_cdc_scan** poursuit son exécution tant qu’il n’est pas explicitement arrêté (mode continu).  
  
##### <a name="one-shot-mode"></a>Mode en une seule fois  
 En mode en une seule fois, le travail de capture demande à **sp_cdc_scan** d’effectuer jusqu’à *maxtrans* analyses pour essayer de vider le journal et retourner les données. Toute transaction au-delà de *maxtrans* qui est présente dans le journal sera traitée dans les analyses ultérieures.  
  
 Le mode en une seule fois est utilisé dans les tests contrôlés, où le volume des transactions à traiter est connu, et où il y a un avantage au fait que le travail se termine automatiquement une fois terminé. Le mode en une seule fois n'est pas recommandé dans un environnement de production. La raison en est que t s'appuie sur la planification du travail pour déterminer la fréquence d'exécution du cycle d'analyse.  
  
 En mode en une seule fois, vous pouvez calculer une limite supérieure pour le débit attendu du travail de capture, exprimé en transactions par seconde, à l'aide de la formule suivante :  
  
 `(maxtrans * maxscans) / number of seconds between scans`  
  
 Même si l'heure qui est requise pour analyser le journal et remplir les tables de modifications n'est pas significativement différente de 0, le débit moyen du travail ne peut pas dépasser la valeur obtenue en divisant le nombre maximum de transactions autorisé pour une seule analyse multiplié par le nombre maximum autorisé d'analyses par le nombre de secondes séparant deux traitements du journal.  
  
 Si le mode en une seule fois était utilisé pour régler l'analyse du journal, le nombre de secondes entre deux traitements du journal devrait être déterminé par la planification du travail. Lorsque ce type de comportement est souhaité, l'exécution du travail de capture en mode continu est préférable pour gérer la replanification de l'analyse du journal.  
  
##### <a name="continuous-mode-and-the-polling-interval"></a>Mode continu et fréquence d'interrogation  
 En mode continu, le travail de capture demande que **sp_cdc_scan** soit exécuté en continu. Cela permet à la procédure stockée de gérer sa propre boucle d'attente en fournissant non seulement une valeur à maxtrans et maxscans mais également au nombre de secondes entre deux traitements du journal (la fréquence d'interrogation). Dans ce mode, le travail de capture reste actif et exécute un **WAITFOR** entre chaque analyse du journal.  
  
> [!NOTE]  
>  Lorsque la valeur de la fréquence d'interrogation est supérieure à 0, la limite supérieure imposée au débit du travail en une seule fois périodique s'applique également au déroulement du travail en mode continu. Autrement dit, (*maxtrans* \* *maxscans*) divisé par une fréquence d’interrogation différente de zéro imposera une limite supérieure au nombre moyen des transactions pouvant être traitées par le travail de capture.  
  
### <a name="capture-job-customization"></a>Personnalisation du travail de capture  
 Pour le travail de capture, vous pouvez appliquer une logique supplémentaire afin de déterminer si une nouvelle analyse commence immédiatement ou à l'issue d'une période de veille, au lieu de s'en remettre à une fréquence d'interrogation fixe. Le choix pourrait reposer uniquement sur l'heure du jour, par exemple en mettant en place de très longues veilles pendant les périodes de pic d'activité, ou même passer à une fréquence d'interrogation de 0 à la fin de la journée, moment où il est important de mettre fin aux traitements de jour et de préparer les opérations de nuit. La progression du processus de capture peut également être surveillée afin de déterminer à quel moment toutes les transactions validées en milieu de la nuit ont été analysées et déposées dans les tables de modifications. Cela permet au travail de capture de s'achever, pour être redémarré par un redémarrage quotidien planifié. En remplaçant l’étape de remise de travail qui appelle **sp_cdc_scan** par un appel à un wrapper écrit par un utilisateur pour **sp_cdc_scan**, vous pouvez disposer d’un comportement hautement personnalisé, pour un minimum d’effort supplémentaire.  
  
##  <a name="Cleanup"></a> Travail de nettoyage  
 Cette section fournit des informations sur le fonctionnement du travail de nettoyage de la capture de données modifiées.  
  
### <a name="structure-of-the-cleanup-job"></a>Structure du travail de nettoyage  
 La capture de données modifiées utilise une stratégie de nettoyage reposant sur la rétention pour gérer la taille de la table des modifications. Le mécanisme de nettoyage consiste en un travail d' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent [!INCLUDE[tsql](../../includes/tsql-md.md)] qui est créé lors de l'activation de la première table de base de données. Un seul travail de nettoyage prend en charge le nettoyage de toutes les tables de modifications des bases de données et applique la même valeur de rétention à toutes les instances de capture définies.  
  
 Le travail de nettoyage est démarré par l’exécution de la procédure stockée sans paramètre **sp_MScdc_cleanup_job**. Cette procédure stockée commence par extraire de **msdb.dbo.cdc_jobs**les valeurs de rétention et de seuil configurées pour le travail de nettoyage. La valeur de rétention est utilisée pour calculer une nouvelle limite inférieure pour les tables de modifications. Le nombre spécifié de minutes est soustrait de la valeur *tran_end_time* maximale de la table **cdc.lsn_time_mapping** pour obtenir la nouvelle limite inférieure exprimée comme valeur datetime. La table CDC.lsn_time_mapping est ensuite utilisée pour convertir cette valeur datetime en valeur **lsn** correspondante. Si la même heure de validation est partagée par plusieurs entrées dans la table, le **lsn** qui correspond à l’entrée qui a le plus petit **lsn** est choisi comme nouvelle limite inférieure. Cette valeur **lsn** est transmise à **sp_cdc_cleanup_change_tables** pour supprimer les entrées de table de modifications dans les tables de modifications de base de données.  
  
> [!NOTE]  
>  L'avantage de l'utilisation de l'heure de validation de la dernière transaction comme base de calcul de la nouvelle limite inférieure est qu'elle permet aux modifications de rester dans les tables de modifications pour l'heure spécifiée. Cela arrive même lorsque le processus de capture prend du retard. Toutes les entrées qui ont la même heure de validation que la limite inférieure actuelle continuent d’être représentées dans les tables de modifications en choisissant le plus petit **lsn** présentant l’heure de validation partagée pour la limite inférieure effective.  
  
 Lorsqu'un nettoyage est effectué, la limite inférieure de toutes les instances de capture est initialement mise à jour au cours d'une même transaction. Le processus essaie ensuite de supprimer les entrées obsolètes des tables de modifications et de la table cdc.lsn_time_mapping. La valeur de seuil configurable limite le nombre d'entrées pouvant être supprimées au cours de chaque instruction. Tout échec de suppression sur une table individuelle n'empêchera pas l'opération d'être tentée sur les tables restantes.  
  
### <a name="cleanup-job-customization"></a>Personnalisation d'un travail de nettoyage  
 Pour le travail de nettoyage, la possibilité de personnalisation réside dans la stratégie utilisée pour déterminer quelles entrées de table de modifications doivent être ignorées. La seule stratégie prise en charge dans le travail de nettoyage réalisé est une stratégie basée sur le temps. Dans cette situation, la nouvelle limite inférieure est calculée en soustrayant la période de rétention autorisée de l'heure de validation de la dernière transaction traitée. Étant donné que les procédures de nettoyage sous-jacentes sont basées sur **lsn** au lieu de l’heure, vous pouvez utiliser autant de stratégies que vous le souhaitez pour déterminer le plus petit **lsn** à conserver dans les tables de modifications. Seules certaines sont strictement basées sur le temps. Par exemple, la connaissance des clients pourrait être utilisée comme mécanisme de prévention de défaillance si en aval, les processus qui requièrent l'accès aux tables de modifications ne peuvent pas s'exécuter. Par ailleurs, bien que la stratégie par défaut applique le même **lsn** pour nettoyer les tables de modifications de toutes les bases de données, la procédure de nettoyage sous-jacente peut également être appelée pour effectuer le nettoyage au niveau de l’instance de capture.  
  
##  <a name="Monitor"></a> Surveiller le processus de capture de données modifiées  
 La surveillance du processus de capture de données modifiées vous permet de déterminer si les modifications sont écrites correctement et avec une latence raisonnable aux tables de modifications. La surveillance peut également vous aider à identifier les erreurs qui peuvent se produire. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut deux vues de gestion dynamique pour vous aider à surveiller la capture de données modifiées : [sys.dm_cdc_log_scan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md) et [sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md).  
  
### <a name="identify-sessions-with-empty-result-sets"></a>Identifier les sessions avec des jeux de résultats vides  
 Chaque ligne dans sys.dm_cdc_log_scan_sessions représente une session d'analyse du journal (sauf la ligne avec un ID de 0). Une session d’analyse du journal est équivalente à une exécution de [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md). Pendant une session, l'analyse peut retourner des modifications ou un résultat vide. Si le jeu de résultats est vide, la colonne empty_scan_count dans sys.dm_cdc_log_scan_sessions est définie sur 1. S'il existe des jeux de résultats vides consécutifs, par exemple si le travail de capture s'exécute continuellement, empty_scan_count dans la dernière ligne existante est incrémenté. Ainsi, si sys.dm_cdc_log_scan_sessions contient déjà 10 lignes pour les analyses qui ont retourné des modifications et qu'il existe cinq résultats vides dans une ligne, la vue contient 11 lignes. La dernière ligne a une valeur de 5 dans la colonne empty_scan_count. Pour déterminer les sessions qui avaient une analyse vide, exécutez la requête suivante :  
  
 `SELECT * from sys.dm_cdc_log_scan_sessions where empty_scan_count <> 0`  
  
### <a name="determine-latency"></a>Déterminer la latence  
 La vue de gestion sys.dm_cdc_log_scan_sessions inclut une colonne qui enregistre la latence pour chaque session de capture. La latence correspond au temps écoulé entre la validation d'une transaction sur une table source et la dernière transaction capturée en cours de validation sur la table de modifications. La colonne de latence est remplie uniquement pour les sessions actives. Pour les sessions ayant une valeur supérieure à 0 dans la colonne empty_scan_count, la colonne de latence a la valeur 0. La requête suivante retourne la latence moyenne pour les sessions les plus récentes :  
  
 `SELECT latency FROM sys.dm_cdc_log_scan_sessions WHERE session_id = 0`  
  
 Vous pouvez utiliser des données de latence pour déterminer si le processus de capture traite les transactions rapidement ou lentement. Ces données sont très utiles lorsque le processus de capture s'exécute continuellement. Si le processus de capture s'exécute selon une planification, la latence peut être élevée à cause du décalage entre les transactions qui sont validées sur la table source et le processus de capture qui s'exécute à l'heure planifiée.  
  
 Une autre mesure importante du rendement du processus de la capture est le débit. Il s'agit du nombre moyen de commandes par seconde qui sont traitées pendant chaque session. Pour déterminer le débit d'une session, divisez la valeur dans la colonne command_count par la valeur dans la colonne de durée. La requête suivante retourne le débit moyen pour les sessions les plus récentes :  
  
 `SELECT command_count/duration AS [Throughput] FROM sys.dm_cdc_log_scan_sessions WHERE session_id = 0`  
  
### <a name="use-data-collector-to-collect-sampling-data"></a>Utiliser le collecteur de données pour recueillir des données d'échantillonnage  
 Le collecteur de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous permet de collecter des instantanés des données à partir de n'importe quelle table ou vue de gestion dynamique et de construire un entrepôt de données de performance. Lorsque la capture de données modifiées est activée sur une base de données, il est utile de prendre des instantanés de la vue sys.dm_cdc_log_scan_sessions et de la vue sys.dm_cdc_errors à intervalles réguliers à des fins d'analyse ultérieure. La procédure suivante installe un collecteur de données pour recueillir les exemples de données de la vue de gestion sys.dm_cdc_log_scan_sessions.  
  
 **Configuration de la collecte de données**  
  
1.  Activez un collecteur de données et configurez un entrepôt de données de gestion. Pour plus d’informations, consultez [Gérer la collecte de données](../../relational-databases/data-collection/manage-data-collection.md).  
  
2.  Exécutez le code suivant pour créer un collecteur personnalisé destiné à la capture de données modifiées.  
  
    ```sql  
    USE msdb;  
  
    DECLARE @schedule_uid uniqueidentifier;  
  
    -- Collect and upload data every 5 minutes  
    SELECT @schedule_uid = (  
    SELECT schedule_uid from sysschedules_localserver_view   
    WHERE name = N'CollectorSchedule_Every_5min')  
  
    DECLARE @collection_set_id int;  
  
    EXEC dbo.sp_syscollector_create_collection_set  
    @name = N' CDC Performance Data Collector',  
    @schedule_uid = @schedule_uid,          
    @collection_mode = 0,                   
    @days_until_expiration = 30,                
    @description = N'This collection set collects CDC metadata',  
    @collection_set_id = @collection_set_id output;  
  
    -- Create a collection item using statistics from   
    -- the change data capture dynamic management view.  
    DECLARE @paramters xml;  
    DECLARE @collection_item_id int;  
  
    SELECT @paramters = CONVERT(xml,   
        N'<TSQLQueryCollector>  
            <Query>  
              <Value>SELECT * FROM sys.dm_cdc_log_scan_sessions</Value>  
              <OutputTable>cdc_log_scan_data</OutputTable>  
            </Query>  
          </TSQLQueryCollector>');  
  
    EXEC dbo.sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = N'302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @name = ' CDC Performance Data Collector',  
    @frequency = 5,   
    @parameters = @paramters,  
    @collection_item_id = @collection_item_id output;  
  
    GO  
    ```  
  
3.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez **Gestion**, puis développez **Collecte de données**. Cliquez avec le bouton droit sur **Collecteur de données de performance de capture de données modifiées**, puis cliquez sur **Démarrer le jeu d'éléments de collecte de données**.  
  
4.  Dans l'entrepôt de données que vous avez configuré à l'étape 1, recherchez la table custom_snapshots.cdc_log_scan_data. Cette table fournit un instantané historique de données de sessions d'analyse du journal. Ces données peuvent être utilisées pour analyser la latence, le débit et d'autres mesures de la performance sur la durée.  
  
## <a name="see-also"></a> Voir aussi  
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Activer et désactiver la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)   
 [Utiliser les données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
  
