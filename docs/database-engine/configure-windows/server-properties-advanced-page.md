---
title: Propriétés du serveur (page Avancé) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.advanced.f1
ms.assetid: cc5e65c2-448e-4f37-9ad4-2dfb1cc84ebe
caps.latest.revision: 65
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d5776bd6e127c90555141826a0defa3a358f8934
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32869714"
---
# <a name="server-properties---advanced-page"></a>Propriétés du serveur - page Avancé
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez cette page pour afficher ou modifier vos paramètres de serveur avancés.  
  
 **Pour afficher les pages de propriétés de serveur**  
  
-   [Afficher ou modifier des propriétés de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md)  
  
## <a name="containment"></a>Containment  
 Activer les bases de données à relation contenant-contenu  
 Indique si cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise les bases de données à relation contenant-contenu. Si la valeur est **True**, une base de données à relation contenant-contenu peut être créée, restaurée ou attachée. Si la valeur est **False**, une base de données à relation contenant-contenu ne peut pas être créée, restaurée ou attachée à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le fait de modifier la propriété de relation contenant-contenu peut avoir un impact sur la sécurité de la base de données. Le fait d'activer les bases de données à relation contenant-contenu permet aux propriétaires de base de données d'octroyer l'accès à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L efait de désactiver les bases de données à relation contenant-contenu peut empêcher les utilisateurs de se connecter. Pour comprendre l’impact de la propriété de relation contenant-contenu, consultez [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md) et [Meilleures pratiques de sécurité recommandées avec les bases de données à relation contenant-contenu](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
## <a name="filestream"></a>FILESTREAM  
 **Niveau d'accès FILESTREAM**  
 Indique le niveau actuel de prise en charge de FILESTREAM sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour modifier le niveau d'accès, sélectionnez l'une des valeurs suivantes :  
  
 **Désactivé**  
 Les données BLOB ne peuvent pas être stockées dans le système de fichiers. Il s'agit de la valeur par défaut.  
  
 **Accès Transact-SQL activé**  
 Les données FILESTREAM sont accessibles à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)], mais pas par le biais du système de fichiers.  
  
 **Accès total activé**  
 Les données FILESTREAM sont accessibles à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)] et par le biais du système de fichiers.  
  
 Lorsque vous activez FILESTREAM pour la première fois, vous devrez peut-être redémarrer l'ordinateur afin de configurer les pilotes.  
  
 **Nom de partage FILESTREAM**  
 Affiche le nom en lecture seule du partage FILESTREAM qui a été sélectionné au cours de l'installation. Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
## <a name="miscellaneous"></a>Divers  
 **Autoriser les déclencheurs à activer d'autres déclencheurs**  
 Autorise des déclencheurs à en activer d'autres. Les déclencheurs peuvent compter jusqu'à 32 niveaux d'imbrication. Pour plus d’informations, consultez la section « Déclencheurs imbriqués » de [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Blocked Process Threshold**  
 Seuil, en secondes, auquel les rapports de processus bloqués sont générés. Le seuil peut être compris entre 0 et 86 400. Par défaut, aucun rapport de processus bloqué n'est généré. Pour plus d’informations, consultez [blocked process threshold (option de configuration de serveur)](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md).  
  
 **Cursor Threshold**  
 Spécifie le nombre de lignes dans le jeu de curseurs sur lesquelles sont générés les jeux de clés de curseurs en mode asynchrone. Lorsque le curseur génère un jeu de clés pour un jeu de résultats, l'optimiseur de requête évalue le nombre de lignes renvoyées pour ce jeu de résultats. Si l'optimiseur de requête estime que le nombre de lignes renvoyées est plus élevé que ce seuil, le curseur est généré de façon asynchrone, ce qui permet à l'utilisateur de rechercher des lignes à partir du curseur alors que ce dernier continue d'être rempli. Dans le cas contraire, le curseur est généré de façon synchrone et la requête attend que toutes les lignes soient renvoyées.  
  
 Si vous attribuez la valeur -1, tous les jeux de clés sont générés de façon synchrone (ce qui avantage les petits jeux de curseurs). Si vous attribuez la valeur 0, tous les jeux de clés de curseurs sont générés de façon asynchrone. Si d'autres valeurs sont définies, l'optimiseur de requête compare le nombre estimé de lignes de résultats dans le jeu de curseurs et crée le jeu de clés de façon asynchrone si ce nombre excède celui défini. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur cursor threshold](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md).  
  
 **Default Full Text Language**  
 Spécifie une langue par défaut pour les colonnes de texte intégral indexées. L'analyse linguistique des données de texte intégral indexées dépend de la langue des données. La valeur par défaut de cette option est la langue du serveur. Pour connaître le langue correspondant au paramètre affiché, consultez [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
 **Langue par défaut**  
 La langue par défaut pour toutes les nouvelles connexions, sauf indication contraire.  
  
 **Option de mise à niveau des index de recherche en texte intégral**  
 Contrôle la manière dont les index de recherche en texte intégral sont migrés lors d'une mise à niveau d'une base de données depuis [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Cette propriété s'applique à la mise à niveau par attachement d'une base de données, restauration d'une sauvegarde de la base de données, restauration d'une sauvegarde de fichiers ou copie de la base de données à l'aide de l'Assistant Copie de base de données.  
  
 Les alternatives sont les suivantes :  
  
 **Importer**  
 Les catalogues de texte intégral sont importés. Cette opération est considérablement plus rapide que **Reconstruire**. Toutefois, un catalogue de texte intégral importé n'utilise pas les analyseurs lexicaux nouveaux et améliorés introduits dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. Par conséquent, vous pouvez souhaiter reconstruire vos catalogues de texte intégral finalement.  
  
 Si aucun catalogue de texte intégral n'est disponible, les index de recherche en texte intégral associés sont reconstruits. Cette option est disponible uniquement pour les bases de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
 **Reconstruire**  
 Les catalogues de texte intégral sont reconstruits à l'aide des analyseurs lexicaux nouveaux et améliorés. La reconstruction des index peut prendre du temps, et une quantité importante de ressources en termes d'UC et de mémoire peut être requise après la mise à niveau.  
  
 **Réinitialiser**  
 Les catalogues de texte intégral sont réinitialisés. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Les fichiers de catalogue de texte intégral sont supprimés, mais les métadonnées pour les catalogues de texte intégral et les index de recherche en texte intégral sont conservés. Après leur mise à niveau, tous les index de recherche en texte intégral ont le suivi des modifications désactivé et aucune analyse n'est démarrée automatiquement. Le catalogue reste vide tant que vous n'avez pas procédé manuellement à une alimentation complète, au terme de la mise à niveau.  
  
 Pour plus d’informations sur le choix de l’option de mise à niveau de recherche en texte intégral, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  
> [!NOTE]  
>  Vous pouvez aussi définir l’option de mise à niveau du catalogue de texte intégral à l’aide de l’action [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)upgrade_option.  
  
 Après avoir attaché, restauré ou copié une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la base de données est immédiatement disponible et est ensuite automatiquement mise à niveau. Si la base de données comprend des index de recherche en texte intégral, la mise à niveau les importe, les réinitialise ou les reconstruit, selon le paramètre de la propriété de serveur **Option de mise à niveau des index de recherche en texte intégral** . Si l’option de mise à niveau a la valeur **Importer** ou **Reconstruire**, les index de recherche en texte intégral ne sont pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que quand l’option de mise à niveau est **Importer**, si un catalogue de texte intégral n’est pas disponible, les index de recherche en texte intégral associés sont reconstruits. Pour plus d’informations sur l’affichage ou la modification du paramètre de la propriété **Option de mise à niveau des index de recherche en texte intégral** , consultez [Gérer et surveiller la recherche en texte intégral pour une instance de serveur](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).  
  
 **Max Text Replication Size**  
 Spécifie la taille maximale (en octets) des données **text**, **ntext**, **varchar(max)**, **nvarchar(max)**, **xml**et **image** susceptibles d’être ajoutées à une colonne répliquée ou capturée dans une instruction INSERT, UPDATE, WRITETEXT ou UPDATETEXT. Toute modification apportée à ce paramètre prend effet immédiatement. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md).  
  
 **Scan For Startup Procs**  
 Spécifie que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherchera l'exécution automatique des procédures stockées au démarrage. Si vous attribuez la valeur **True**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche et exécute toutes les procédures stockées exécutées automatiquement définies sur le serveur. Si vous attribuez la valeur **False** (valeur par défaut), aucune recherche n’est exécutée. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md).  
  
 **Année de coupure à deux chiffres**  
 Indique le numéro d'année le plus élevé qui peut être entré en tant qu'année à deux chiffres. L'année répertoriée et les 99 années précédentes peuvent être entrées sous forme d'années à deux chiffres. Toutes les autres années doivent être entrées sous forme d'années à quatre chiffres.  
  
 Par exemple, le paramètre par défaut 2049 indique qu'une date entrée sous la forme "14/3/49" sera interprétée comme le 14 mars 2049, tandis qu'une date entrée sous la forme "14/3/50" sera interprétée comme le 14 mars 1950. Pour plus d’informations, voir [Configurer l'option de configuration de serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).  
  
## <a name="network"></a>Réseau  
 **Taille du paquet réseau**  
 Définit la taille des paquets (en octets) utilisée sur tout le réseau. La taille des paquets par défaut est 4 096 octets. Si une application effectue des opérations de copie en bloc, ou envoie ou reçoit de grandes quantités de données de type **text** ou **image** , l’utilisation d’une taille de paquet supérieure à la taille par défaut peut améliorer les performances car elle permet de réduire les lectures et écritures sur le réseau. Si une application envoie et reçoit de petites quantités d'informations, vous pouvez définir une taille de paquets de 512 octets, ce qui est suffisant pour la plupart des transferts de données. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur network packet size](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md).  
  
> [!NOTE]  
>  Ne modifiez pas la taille des paquets, sauf si vous êtes certain que cela permettra d'accroître les performances. Pour la plupart des applications, la taille de paquet par défaut représente la meilleure solution.  
  
 **Remote Login Timeout**  
 Spécifie le nombre de secondes d'attente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de l'échec d'une tentative de connexion d'accès distant. Ce paramètre affecte les connexions à des fournisseurs OLE DB établies pour des requêtes hétérogènes. La valeur par défaut est 20 secondes. Une valeur égale à 0 entraîne une attente infinie. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md).  
  
 Toute modification apportée à ce paramètre prend effet immédiatement.  
  
## <a name="parallelism"></a>Parallelism:  
 **Cost Threshold for Parallelism**  
 Spécifie le seuil de création et d'exécution des plans parallèles par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce coût fait référence à l'estimation du temps (exprimé en secondes) nécessaire à l'exécution du plan de série pour une configuration matérielle spécifique. Spécifiez cette option uniquement sur des multiprocesseurs symétriques. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur cost threshold for parallelism](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md).  
  
 **Verrous**  
 Définit le nombre maximal de verrous disponibles afin de limiter la quantité de mémoire que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise pour eux. La valeur par défaut est 0, ce qui permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'allouer et de libérer dynamiquement des verrous en modifiant la configuration système requise.  
  
 La configuration recommandée est l'autorisation de l'utilisation dynamique des verrous par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Configurer l’option de configuration du serveur locks](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md).  
  
 **Max Degree of Parallelism**  
 Limite le nombre de processeurs (64 au maximum) à utiliser dans l'exécution des plans parallèles. La valeur par défaut 0 utilise tous les processeurs disponibles. La valeur 1 supprime la génération de plans parallèles. Un nombre supérieur à 1 limite le nombre maximal de processeurs utilisés au cours de l'exécution d'une requête simple. Si une valeur supérieure au nombre de processeurs disponibles est spécifiée, le nombre réel de processeurs disponibles est utilisé. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
 **Query Wait**  
 Spécifie le nombre de secondes (de 0 à 2 147 483 647) pendant lequel une requête peut attendre des ressources avant d'expirer. Si la valeur par défaut -1 est utilisée, le délai d'attente est calculé comme étant 25 fois le coût estimé de la requête. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
  
  
