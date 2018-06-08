---
title: Améliorer les performances générales de la réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- publications [SQL Server replication], design and performance
- designing databases [SQL Server], replication performance
- Snapshot Agent, performance
- snapshots [SQL Server replication], performance considerations
- merge replication performance [SQL Server replication]
- snapshot replication [SQL Server], performance
- subscriptions [SQL Server replication], performance considerations
- agents [SQL Server replication], performance
- performance [SQL Server replication], general considerations
- transactional replication, performance
ms.assetid: 895b1ad7-ffb9-4a5c-bda6-e1dfbd56d9bf
caps.latest.revision: 45
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6fa663e269559fb2eb87d599723734639a717d2e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enhance-general-replication-performance"></a>Améliorer les performances générales de la réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez améliorer les performances globales de tous les types de réplication de votre application et de votre réseau à l'aide des indications décrites dans cette rubrique :  
  
## <a name="server-and-network"></a>Serveur et réseau  
  
-   Définissez la quantité minimale et maximale de mémoire allouée à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)].  
  
     Par défaut, le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] modifie ses besoins de mémoire de façon dynamique en fonction des ressources système disponibles. Pour éviter un problème de saturation de la mémoire lors des opérations de réplication, indiquez une taille de mémoire minimale disponible en vous servant de l'option **mémoire minimale du serveur** . Pour éviter que le système d'exploitation n'utilise la mémoire paginable sur le disque, vous pouvez également indiquer une taille de mémoire maximale à l'aide de l'option **mémoire maximale du serveur** . Pour en savoir plus, consultez [Mémoire du serveur (option de configuration de serveur)](../../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
-   Assurez-vous que l'allocation des fichiers de données des bases de données et des fichiers journaux est correcte. Utilisez une unité de disque séparée pour le journal des transactions de toutes les bases de données impliquées dans la réplication.  
  
     Vous pouvez réduire le temps nécessaire à l'écriture des transactions en stockant les fichiers-journaux sur une unité de disque différente de celle qui est utilisée pour le stockage de la base de données. Vous pouvez mettre ce lecteur en miroir, par le biais d'un dispositif RAID-1 (Redundant Array of Inexpensive Disks), si la tolérance de panne est requise. Utilisez RAID 0 ou 0+1 (selon vos besoins en tolérance de panne) pour les autres fichiers de base de données. Cette mesure est pertinente, qu'une réplication soit ou non appliquée.  
  
-   Envisagez d'ajouter de la mémoire aux serveurs utilisés dans la réplication, surtout au serveur de distribution.  
  
-   Utilisez des ordinateurs multiprocesseurs.  
  
     Les agents de réplication peuvent tirer profit de processeurs supplémentaires sur le serveur. Si vous faites un usage intensif de l'UC, envisagez l'installation d'une UC plus rapide ou de plusieurs UC.  
  
-   Utilisez un réseau rapide.  
  
     Le réseau peut s'avérer être un goulet d'étranglement important en terme de performance, principalement pour la réplication transactionnelle. La propagation des modifications sur les abonnés peut être améliorée de manière significative en utilisant un réseau rapide de 100 mégaoctets par seconde (Mbits/s) ou plus rapide. Si le réseau est lent, spécifiez les paramètres appropriés pour le réseau et les agents.  
  
## <a name="database-design"></a>Création de bases de données  
  
-   Respectez les méthodes conseillées en matière de conception de bases de données.  
  
     Une base de données répliquée bénéficie généralement des mêmes optimisations de performances qu'une base de données non répliquée. Les index doivent cependant être utilisés prudemment sur l'Abonné : la colonne de clé primaire de l'Abonné doit être indexée, mais l'ajout d'index supplémentaires peut affecter les performances d'insertion, de mise à jour et de suppression.  
  
-   Envisagez de configurer l'option de base de données READ_COMMITTED_SNAPSHOT.  
  
     Pour aider à réduire les contentions entre l'activité de l'utilisateur et l'activité de l'Agent de réplication, définissez cette option pour les bases de données de publication et d'abonnement :  
  
    ```  
    ALTER DATABASE AdventureWorks  
    SET READ_COMMITTED_SNAPSHOT ON  
    ```  
  
     Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql.md).  
  
-   Méfiez-vous de la logique des vos applications dans les déclencheurs.  
  
     La logique métier dans les déclencheurs définis par l'utilisateur sur l'Abonné peut ralentir la réplication des modifications sur l'Abonné :  
  
    -   Dans le cas d'une réplication transactionnelle, il peut être plus efficace d'inclure cette logique dans des procédures stockées personnalisées utilisées pour appliquer les commandes répliquées. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
    -   Dans le cas d'une réplication de fusion, il peut être plus efficace d'utiliser des gestionnaires de logique métier. Pour plus d’informations, consultez [Exécuter la logique métier pendant la synchronisation de fusion](../../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
     Si vous utilisez des déclencheurs pour conserver l'intégrité référentielle dans les tables publiées pour la réplication de fusion, indiquez l'ordre de traitement des tables afin de réduire le nombre de tentatives nécessaires à l'Agent de fusion. Pour plus d’informations, consultez [Spécifier l’ordre de traitement d’articles de fusion](../../../relational-databases/replication/merge/specify-the-processing-order-of-merge-articles.md).  
  
-   Limitez l'utilisation des types de données d'objets volumineux (LOB).  
  
     Les objets volumineux nécessitent plus d'espace de stockage et de traitement que les autres types de données de colonnes. N'incluez pas ces colonnes dans les articles à moins que votre application en ait besoin. Les types de données **text**, **ntext**et **image** sont déconseillés. Si vous incluez des données de type LOB, il est recommandé d'utiliser respectivement les types de données **varchar(max)**, **nvarchar(max)**, **varbinary(max)**.  
  
     Dans le cas d'une réplication transactionnelle, envisagez d'utiliser le profil de l'Agent de distribution appelé **Profil de distribution pour le flux OLEDB**. Pour plus d’informations, voir [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
## <a name="publication-design"></a>Conception de la publication  
  
-   Ne publiez que les données nécessaires.  
  
     Parce que la réplication est facile à initialiser, vous aurez tendance à publier plus de données que nécessaire. Cela peut entraîner une surconsommation des ressources dans la base de données de distribution, ainsi que dans les fichiers d'instantané et diminuer la capacité de traitement des données requises. Par conséquent, évitez d'éditer des tables superflues et envisagez de réduire la fréquence de mise à jour des publications.  
  
-   Réduisez les conflits grâce à la conception de la publication et au comportement de l'application.  
  
     Les types de réplications suivants permettent de modifier les données sur les abonnés : réplication de fusion, réplication transactionnelle avec mise à jour des abonnements et réplication transactionnelle d'égal à égal. La réplication de fusion et la réplication transactionnelle avec abonnements pouvant être mis à jour prennent en charge les conflits de données si une ligne donnée est mise à jour sur plus d'un nœud entre les synchronisations. La réplication transactionnelle d'égal à égal ne prend pas en charge les conflits de données, les modifications de données doivent être partitionnées. Quel que soit le type de réplication utilisé, il est recommandé de partitionner les modifications chaque fois que cela est possible, car cela réduit le traitement nécessaire pour la détection et la résolution des conflits.  
  
     Les modifications peuvent être partitionnées en publiant des sous-ensembles de données sur chaque abonné, ou en utilisant une application qui dirige les modifications d'une ligne donnée sur un nœud donné :  
  
    -   La réplication de fusion prend en charge la publication de sous-ensembles de données à l'aide de paramètres filtrés avec une seule publication. Pour plus d'informations, voir [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
    -   La réplication transactionnelle prend en charge la publication de sous-ensembles de données à l'aide de filtres statiques avec plusieurs publications. Pour plus d’informations, consultez [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md).  
  
-   Servez-vous des filtres de lignes judicieusement.  
  
     Lorsqu'une publication transactionnelle comporte un ou plusieurs articles utilisant des filtres de lignes, l'Agent de lecture du journal doit appliquer le filtre sur chaque ligne concernée par une mise à jour de la table, car il analyse le journal des transactions. La productivité de l'Agent de lecture du journal est donc affectée.  
  
     De même, la réplication de fusion doit évaluer les lignes modifiées ou supprimées pour déterminer quels abonnés doivent recevoir ces lignes. Lorsque les filtres de lignes sont utilisés pour réduire les données requises sur un abonné, ce traitement est plus complexe et peut s'avérer plus lent que lors de la publication de toutes les lignes d'une table. Déterminez soigneusement le meilleur compromis entre la réduction des contraintes de stockage sur chaque abonné et la nécessité d'une productivité maximale. Pour plus d’informations sur le filtrage, consultez [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md).  
  
## <a name="subscription-considerations"></a>Considérations sur les abonnements  
  
-   Utilisez les abonnements par extraction de données (pull) lorsqu'il y a un grand nombre d'abonnés.  
  
     L'Agent de distribution et l'Agent de fusion s'exécutent sur le serveur de distribution pour les abonnements par envoi de données (push), et sur les abonnés pour les abonnements par extraction de données (pull). L'utilisation des abonnements par extraction de données (pull) permet d'améliorer les performances en déplaçant le traitement de l'Agent du serveur de distribution aux abonnés. Pour plus d’informations, consultez [S’abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md).  
  
-   Envisagez de réinitialiser l'abonnement si les abonnés sont trop en retard.  
  
     Lorsque des quantités importantes de modifications doivent être envoyées aux abonnés, la réinitialisation de ces derniers à partir d'un nouvel instantané peut s'avérer plus rapide que l'utilisation de la réplication pour déplacer chaque modification. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
     Pour la réplication transactionnelle, le moniteur de réplication affiche des informations sur l'onglet **Commandes non distribuées** sur : le nombre de transactions dans la base de données de distribution qui n'ont pas encore été distribuées à un Abonné ; et sur le temps estimé pour distribuer ces transactions. Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
## <a name="snapshot-considerations"></a>Considérations sur les instantanés  
  
-   N'exécutez l'Agent d'instantané que lorsque cela est nécessaire et aux heures creuses.  
  
     L'Agent d'instantané copie en bloc les données de la table publiée sur le serveur de publication vers un fichier du dossier d'instantanés sur le serveur de distribution. La génération d'un instantané demeure un processus gourmand en ressources et donne ses meilleurs résultats pendant les heures creuses.  
  
-   Utilisez un instantané en mode natif à moins qu'un instantané en mode caractère ne soit nécessaire.  
  
     Utilisez l'instantané en mode natif par défaut pour tous les Abonnés, sauf pour les abonnés non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les abonnés exécutant [!INCLUDE[ssEW](../../../includes/ssew-md.md)], qui nécessitent un instantané en mode caractère.  
  
-   Utilisez un dossier d'instantanés unique pour une publication.  
  
     Lorsque vous définissez les propriétés de publication relatives à l'emplacement de l'instantané, vous pouvez choisir de générer les fichiers d'instantané dans le dossier d'instantanés par défaut, dans un autre dossier d'instantanés ou dans les deux dossiers à la fois. La génération des fichiers d'instantané dans les deux emplacements nécessite un espace disque et un traitement supplémentaires lors de l'exécution de l'Agent d'instantané.  
  
-   Placez le dossier d'instantanés sur un disque local du serveur de distribution ne servant pas à stocker les fichiers de bases de données ou de journaux.  
  
     L'Agent d'instantané effectue un enregistrement séquentiel des données sur le dossier d'instantanés. Le fait de placer le dossier d'instantanés sur un disque séparé des fichiers de bases de données ou de journaux réduit les contentions entre les disques et permet au processus d'instantané de se terminer plus rapidement.  
  
-   Lorsque vous créez la base de données d'abonnement sur l'Abonné, envisagez de spécifier un mode de récupération simple ou utilisant les journaux des transactions. Cela permet de réduire au minimum la journalisation des insertions en bloc effectuées pendant l'application de l'instantané sur l'Abonné. Une fois que l'instantané a été appliqué sur la base de données d'abonnement, vous pouvez passer à un autre mode de récupération si nécessaire (les bases de données répliquées peuvent utiliser n'importe quel mode de récupération). Pour plus d’informations sur la sélection d’un modèle de récupération, consultez [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
-   Envisagez d'utiliser le dossier d'instantanés de remplacement et les instantanés compressés sur support amovible pour les réseaux à faible bande passante.  
  
     La compression des fichiers d'instantanés dans le dossier d'instantanés de remplacement permet de réduire les besoins de stockage sur disque, et de simplifier le transfert des fichiers d'instantanés sur support amovible.  
  
     Compresser les instantanés permet dans certains cas d'améliorer les performances du transfert des fichiers d'instantanés sur le réseau. Toutefois, la compression de l'instantané nécessite un traitement supplémentaire de la part de l'Agent d'instantané lors de la génération des fichiers d'instantanés, et de la part de l'Agent de distribution lors de l'application de ces fichiers. Dans certains cas, cela peut ralentir la génération de l'instantané et rallonger le délai d'application d'un instantané. De plus, les instantanés compressés ne peuvent pas reprendre si une défaillance du réseau se produit ; ils ne conviennent donc pas aux réseaux non fiables. Évaluez ces différents facteurs avec précaution lors de l'utilisation d'instantanés compressés sur un réseau. Pour plus d'informations, consultez [Alternate Snapshot Folder Locations](../../../relational-databases/replication/alternate-snapshot-folder-locations.md) et [Compressed Snapshots](../../../relational-databases/replication/compressed-snapshots.md).  
  
-   Envisagez d'initialiser manuellement un abonnement.  
  
     Dans certains scénarios, comme ceux impliquant de volumineux datasets initiaux, il est préférable d'initialiser un abonnement à l'aide d'une autre méthode que l'instantané. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
## <a name="agent-parameters"></a>Paramètres de l'Agent  
  
-   Réduisez les niveaux de détail des Agents de réplication, sauf au cours des test initiaux, de l'analyse ou du débogage.  
  
     Réduisez le paramètre **–HistoryVerboseLevel** et le paramètre **–OutputVerboseLevel** des Agents de distribution ou de fusion. Cela diminue la quantité de nouvelles lignes insérées pour le suivi de l'historique et des valeurs de sortie de l'Agent. En contrepartie, les messages d'historique antérieurs présentant le même état sont mis à jour à partir des nouvelles informations d'historique. Augmentez les niveaux de commentaire pour les tests, l'analyse et le débogage afin d'obtenir le plus d'informations possibles sur l'activité de l'Agent.  
  
-   Utilisez le paramètre **–MaxBCPThreads** de l’Agent d’instantané, de l’Agent de fusion et de l’Agent de distribution (le nombre de threads spécifié ne doit pas dépasser le nombre de processeurs sur l’ordinateur). Ce paramètre indique le nombre d'opérations de copie en bloc pouvant être effectuées en parallèle lorsque l'instantané est créé et appliqué.  
  
-   Utilisez le paramètre **–UseInprocLoader** de l’Agent de distribution et de l’Agent de fusion (il est impossible d’utiliser ce paramètre si des tables publiées comportent des colonnes XML). Ce paramètre entraîne l'utilisation de la commande BULK INSERT par l'Agent lorsque l'instantané est appliqué.  
  
 Les paramètres des agents peuvent être spécifiés dans des profils d'agent et sur la ligne de commande. Pour plus d'informations, consultez :  
  
-   [Utiliser des profils d’agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
-   [Afficher et modifier des paramètres d’invite de commandes d’un Agent de réplication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
-   [Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
  
