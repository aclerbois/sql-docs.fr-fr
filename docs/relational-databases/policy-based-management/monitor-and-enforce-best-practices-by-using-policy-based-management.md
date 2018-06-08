---
title: Surveiller et appliquer les bonnes pratiques à l’aide de la gestion basée sur des stratégies | Microsoft Docs
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
- Best Practices [Database Engine]
ms.assetid: 46788407-187e-4b0b-bfe4-529af8d77c60
caps.latest.revision: 27
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d29ff5465669ed8c37448f0461b349b6d37906c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitor-and-enforce-best-practices-by-using-policy-based-management"></a>Surveiller et appliquer les bonnes pratiques à l'aide de la Gestion basée sur des stratégies
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La gestion basée sur des stratégies vous permet de surveiller les bonnes pratiques relatives au [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] propose un ensemble de fichiers de stratégie que vous pouvez importer en tant que stratégies de meilleures pratiques, pour ensuite évaluer ces stratégies par rapport à un jeu de cibles qui inclut des instances, des objets d’instance, des bases de données ou des objets de base de données. Vous pouvez évaluer des stratégies manuellement, définir des stratégies pour évaluer un jeu de cibles selon une planification ou définir des stratégies pour évaluer un jeu de cibles en fonction d’un événement. Pour plus d’informations sur la gestion basée sur des stratégies, consultez [Administrer des serveurs à l’aide de la gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).  
  
## <a name="policy-and-rules-for-database-engine"></a>Stratégies et règles du moteur de base de données  
 Le tableau qui suit répertorie les stratégies qui sont incluses avec l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournit des informations sur les règles de meilleures pratiques qui sont évaluées par chaque stratégie. Les stratégies sont stockées sous la forme de fichiers XML et doivent être importées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur l’importation de stratégies, consultez [Importer une stratégie de gestion basée sur des stratégies](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md).  
  
|Nom de stratégie|Règle de meilleure pratique|  
|-----------------|------------------------|  
|Algorithme de chiffrement à clé asymétrique|[Force de chiffrement des clés asymétriques](../../relational-databases/policy-based-management/asymmetric-keys-encryption-strength.md)|  
|Emplacement des fichiers de données et de sauvegarde|[Les fichiers de sauvegarde doivent être placés sur des périphériques distincts des fichiers de base de données](http://msdn.microsoft.com/library/7039bebb-1f25-4cf3-81f1-393dfb78da12)|  
|Emplacement des fichiers de données et des fichiers journaux|[Placer les fichiers de données et les fichiers journaux sur des lecteurs distincts](../../relational-databases/policy-based-management/place-data-and-log-files-on-separate-drives.md)|  
|Fermeture automatique de la base de données|[Définir l'option de base de données AUTO_CLOSE sur OFF](../../relational-databases/policy-based-management/set-the-auto-close-database-option-to-off.md)|  
|Réduction automatique de la base de données|[Définir l'option de base de données AUTO_SHRINK sur OFF](../../relational-databases/policy-based-management/set-the-auto-shrink-database-option-to-off.md)|  
|Classement de base de données|[Définir le même classement pour les bases de données définies par l'utilisateur que pour les bases de données MASTER ou model](http://msdn.microsoft.com/library/c686446f-dae1-4b05-a3df-837b3422988d)|  
|Vérification de la page de base de données|[Définir l'option de base de données PAGE_VERIFY sur CHECKSUM](../../relational-databases/policy-based-management/set-the-page-verify-database-option-to-checksum.md)|  
|État de la page de base de données|[Vérifier l'intégrité d'une base de données contenant des pages suspectes](../../relational-databases/policy-based-management/check-integrity-of-database-with-suspect-pages.md)|  
|Autorisations Invité|[Autorisations Invité sur les bases de données utilisateur](../../relational-databases/policy-based-management/guest-permissions-on-user-databases.md)|  
|Date de la dernière sauvegarde réussie|[Sauvegarde obsolète](../../relational-databases/policy-based-management/outdated-backup.md)|  
|Autorisations du serveur public non accordées|[Autorisations du serveur public](../../relational-databases/policy-based-management/server-public-permissions.md)|  
|Chevauchement de masque d'affinité SQL Server 64 bits|[Correct Affinity Mask and Affinity Input and Output Mask Overlap](../../relational-databases/policy-based-management/correct-affinity-mask-and-affinity-input-and-output-mask-overlap.md)|  
|Masque d'affinité SQL Server|[Conserver la valeur par défaut du masque d'affinité](../../relational-databases/policy-based-management/keep-the-affinity-mask-default-value.md)|  
|Seuil de processus bloqué SQL Server|[Augmenter la valeur de l'option blocked process threshold ou la désactiver](../../relational-databases/policy-based-management/increase-or-disable-blocked-process-threshold.md)|  
|Trace par défaut SQL Server|[Fichiers journaux de trace par défaut désactivés](../../relational-databases/policy-based-management/default-trace-log-files-disabled.md)|  
|Verrous dynamiques SQL Server|[Conserver la valeur par défaut de l'option de configuration locks](../../relational-databases/policy-based-management/keep-the-locks-configuration-option-default-value.md)|  
|Regroupement léger SQL Server|[Désactiver le regroupement léger](../../relational-databases/policy-based-management/disable-lightweight-pooling.md)|  
|Mode de connexion SQL Server|[Choisir un mode d'authentification](../../relational-databases/security/choose-an-authentication-mode.md)|  
|Degré maximal de parallélisme SQL Server|[Définir l'option max degree of parallelism pour des performances optimales](../../relational-databases/policy-based-management/set-the-max-degree-of-parallelism-option-for-optimal-performance.md)|  
|Nombre maximal de threads de travail SQL Server pour SQL Server 2000 32 bits|[Vérifier le paramètre de l'option Max Worker Threads](../../relational-databases/policy-based-management/verify-max-worker-threads-setting.md)|  
|Nombre maximal de threads de travail SQL Server pour SQL Server 2000 64 bits|[Vérifier le paramètre Nombre maximum de threads de travail](../../relational-databases/policy-based-management/verify-max-worker-threads-setting.md)|  
|Nombre maximal de threads de travail SQL Server pour SQL Server 2005 et versions ultérieures|[Vérifier le paramètre de l'option Max Worker Threads](../../relational-databases/policy-based-management/verify-max-worker-threads-setting.md)|  
|Taille du paquet réseau SQL Server|[La taille du paquet réseau ne doit pas dépasser 8060 octets](../../relational-databases/policy-based-management/network-packet-size-should-not-exceed-8060-bytes.md)|  
|Expiration du mot de passe SQL Server|[Expiration du mot de passe de connexion SQL Server](../../relational-databases/policy-based-management/sql-server-login-password-expiration.md)|  
|Stratégie de mot de passe SQL Server|[Force du mot de passe de connexion SQL Server](../../relational-databases/policy-based-management/sql-server-login-password-strength.md)|  
|Chiffrement à clé symétrique pour les bases de données utilisateur|[Clés symétriques sur les bases de données utilisateur](../../relational-databases/policy-based-management/symmetric-keys-on-user-databases.md)|  
|Clé symétrique pour base de données MASTER|[Clés symétriques sur les bases de données système](../../relational-databases/policy-based-management/symmetric-keys-on-system-databases.md)|  
|Clé symétrique pour bases de données système|[Clés symétriques sur les bases de données système](../../relational-databases/policy-based-management/symmetric-keys-on-system-databases.md)|  
|Base de données de confiance|[Bit de confiance](../../relational-databases/policy-based-management/trustworthy-bit.md)|  
|Journal des événements Windows : erreur liée à des ressources disque de cluster endommagées|[Détecter des problèmes de carte hôte SCSI](../../relational-databases/policy-based-management/detect-scsi-host-adapter-issues.md)|  
|Journal des événements Windows : erreur liée au contrôle de pilote de périphérique|[Erreur de contrôle du pilote de périphérique](../../relational-databases/policy-based-management/device-driver-control-error.md)|  
|Journal des événements Windows : erreur liée à un périphérique non prêt|[Erreur de périphérique non prêt](../../relational-databases/policy-based-management/device-not-ready-error.md)|  
|Journal des événements Windows : erreur liée à un échec de requête d'E/S|[Détecter les échecs de requêtes d’entrée et de sortie](../../relational-databases/policy-based-management/detect-failed-input-and-output-requests.md)|  
|Journal des événements Windows : avertissement lié à un retard d'E/S|[Rechercher des problèmes de délai d’E/S dans le sous-système d’E/S disque](../../relational-databases/policy-based-management/check-disk-input-and-output-subsystem-for-io-delay-problems.md)|  
|Journal des événements Windows : erreur liée à une erreur d'E/S durant une défaillance de page matérielle|[Erreur d’E/S lors du contrôle des erreurs de défaut de page matérielle](../../relational-databases/policy-based-management/input-and-output-error-during-hard-page-fault.md)|  
|Journal des événements Windows : erreur liée à une nouvelle tentative de lecture|[Rechercher des problèmes de nouvelle tentative de lecture dans le sous-système d’entrées/sorties](../../relational-databases/policy-based-management/check-disk-input-output-subsystem-for-read-retry-problems.md)|  
|Journal des événements Windows : erreur liée à un délai d'attente d'E/S au niveau du système de stockage|[Délai d’attente d’entrée-sortie du système de stockage](../../relational-databases/policy-based-management/storage-system-input-output-time-out.md)|  
|Journal des événements Windows : erreur liée à une défaillance du système|[Défaillances inattendues du système](../../relational-databases/policy-based-management/unexpected-system-failures.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser les facettes de la gestion basée sur des stratégies](../../relational-databases/policy-based-management/working-with-policy-based-management-facets.md)  
  
  
