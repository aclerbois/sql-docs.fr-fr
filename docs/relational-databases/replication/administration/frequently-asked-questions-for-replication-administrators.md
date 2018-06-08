---
title: Questions fréquentes (FAQ) pour les administrateurs de la réplication | Microsoft Docs
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
- administering replication, frequently asked questions
- replication [SQL Server], administering
ms.assetid: 5a9e4ddf-3cb1-4baf-94d6-b80acca24f64
caps.latest.revision: 59
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: fd85694c8b2678d85b66db6c84b89a409fa0fc4a
ms.sourcegitcommit: df382099ef1562b5f2d1cd506c1170d1db64de41
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="frequently-asked-questions-for-replication-administrators"></a>Questions fréquentes (FAQ) pour les administrateurs de la réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les questions et réponses suivantes fournissent des indications sur une variété de tâches auxquelles les administrateurs de bases de données répliquées sont confrontés.  
  
## <a name="configuring-replication"></a>Configuration d'une réplication  
  
### <a name="does-activity-need-to-be-stopped-on-a-database-when-it-is-published"></a>L'activité doit-elle être arrêtée sur une base de données lorsqu'elle est publiée ?  
 Non. L'activité peut continuer sur une base de données pendant la création d'une publication. Sachez que la création d'un instantané utilise de nombreuses ressources, il est donc préférable de générer les instantanés lors de périodes de faible activité sur la base de données (un instantané est généré par défaut lorsque vous terminez l'Assistant Nouvelle publication).  
  
### <a name="are-tables-locked-during-snapshot-generation"></a>Les tables sont-elles verrouillées pendant la génération d'un instantané ?  
 La durée de verrouillage dépend du type de réplication utilisé :  
  
-   Pour les publications de fusion, l'Agent d'instantané n'utilise aucun verrou.  
  
-   Pour les publications transactionnelles, par défaut l'Agent d'instantané utilise des verrous uniquement lors de la phase initiale de génération d'instantané.  
  
-   Pour les publications d'instantané, l'Agent d'instantané utilise des verrous pendant toute la durée du processus de génération d'instantané.  
  
 Comme les verrous empêchent les autres utilisateurs de mettre à jour les tables, l'exécution de l'Agent d'instantané devrait être planifiée lors de périodes de faible activité sur la base de données, principalement pour les publications d'instantané.  
  
### <a name="when-is-a-subscription-available-when-can-the-subscription-database-be-used"></a>Quand un abonnement est-il disponible, quand la base de données d'abonnement peut-elle être utilisée ?  
 Un abonnement est disponible après que l'instantané ait été appliqué à la base de données d'abonnement. Bien que la base de données d'abonnement soit accessible avant, il est conseillé de ne pas l'utiliser tant que l'instantané n'a pas été appliqué. Utilisez le Moniteur de réplication pour vérifier l'état de génération et d'application d'instantané :  
  
-   L'instantané est généré par l'Agent d'instantané. Affichez l'état de génération de l'instantané sur l'onglet **Agents** pour une publication du moniteur de réplication. Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents associés à une publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md).  
  
-   L'instantané est appliqué par l'Agent de distribution ou par l'Agent de fusion. Affichez l'état d'application d'instantané sur la page **Agent de distribution** ou **Agent de fusion** du Moniteur de réplication. Pour plus d’informations, consultez [Afficher des informations et effectuer des tâches pour les agents d’abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
### <a name="what-happens-if-the-snapshot-agent-has-not-completed-when-the-distribution-or-merge-agent-starts"></a>Que se passe-t-il si l’Agent d'instantané n’a pas terminé lorsque l’Agent de distribution ou l'Agent de fusion démarre ?  
 Cela n'entraînera aucune erreur si l'Agent de distribution ou l'Agent de fusion s'exécute en même temps que l'Agent d'instantané. Vous devez cependant prendre en compte les éléments suivants :  
  
-   Si l'Agent de distribution ou l'Agent de fusion est configuré pour une exécution en continu, l'Agent applique automatiquement l'instantané une fois que l'Agent d'instantané est terminé.  
  
-   Si l'Agent de distribution ou l'Agent de fusion est configuré pour une exécution planifiée ou à la demande, et qu'aucun instantané n'est disponible pendant que l'Agent s'exécute, l'Agent s'arrêtera en affichant un message indiquant qu'un instantané n'est pas encore disponible. Vous devez relancer l'exécution de l'Agent pour appliquer l'instantané une fois que l'Agent d'instantané est terminé. Pour plus d’informations sur l’exécution des agents, consultez [Synchroniser un abonnement par émission de données](../../../relational-databases/replication/synchronize-a-push-subscription.md), [Synchroniser un abonnement par extraction](../../../relational-databases/replication/synchronize-a-pull-subscription.md) et [Concepts des exécutables de l’agent de réplication](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
### <a name="should-i-script-my-replication-configuration"></a>Dois-je créer un script de ma configuration de réplication ?  
 Oui. Le script de la configuration de réplication est un composant clé de tout plan de récupération d'urgence pour une topologie de réplication. Pour plus d'informations sur les scripts, consultez [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
### <a name="what-recovery-model-is-required-on-a-replicated-database"></a>Quel est le mode de récupération nécessaire sur une base de données répliquée ?  
 La réplication fonctionne correctement à l'aide de n'importe quel mode de récupération : simple, journalisé en bloc ou restauration complète. La réplication de fusion assure le suivi des modifications en stockant les informations dans des tables de métadonnées. La réplication transactionnelle assure le suivi des modifications par un marquage du journal des transactions, mais ce processus de marquage n'est pas concerné par le mode de récupération.  
  
### <a name="why-does-replication-add-a-column-to-replicated-tables-will-it-be-removed-if-the-table-isnt-published"></a>Pourquoi la réplication ajoute-t-elle une colonne aux tables répliquées, sera-t-elle supprimée si la table n'est pas publiée ?  
 Afin d'assurer le suivi des modifications, la réplication de fusion et la réplication transactionnelle avec mise à jour d'abonnements en attente doivent pouvoir identifier de manière unique chaque ligne dans chaque table publiée. À cet effet :  
  
-   La réplication de fusion ajoute la colonne **rowguid** à chaque table, à moins que la table ne dispose déjà d'une colonne de type de données **uniqueidentifier** avec la propriété **ROWGUIDCOL** définie (auquel cas cette colonne est utilisée). Si la table est supprimée de la publication, la colonne **rowguid** est supprimée, si une colonne existante était utilisée pour le suivi, la colonne n'est pas supprimée.  
  
-   Si une publication transactionnelle prend en charge la mise à jour d'abonnements en attente, la réplication ajoute la colonne **msrepl_tran_version** à chaque table. Si la table est supprimée de la publication, la colonne **msrepl_tran_version** n'est pas supprimée.  
  
-   Aucun filtre ne doit inclure le **rowguidcol** utilisé par la réplication pour identifier les lignes. Par défaut, il s'agit de la colonne ajoutée lorsque vous avez configuré la réplication de fusion et qui a pour nom **rowguid**.  
  
### <a name="how-do-i-manage-constraints-on-published-tables"></a>Comment puis-je gérer les contraintes sur les tables publiées ?  
 Il convient de tenir compte de plusieurs problèmes concernant les contraintes sur les tables publiées :  
  
-   La réplication transactionnelle nécessite une contrainte de clé primaire sur chaque table publiée. La réplication de fusion ne nécessite pas de clé primaire, mais s'il en existe une, elle doit être répliquée. La réplication d'instantané ne nécessite pas de clé primaire.  
  
-   Par défaut, les contraintes de clé primaire, les index et les contraintes de validation sont répliquées aux abonnés.  
  
-   L'option NOT FOR REPLICATION est spécifiée par défaut pour les contraintes de clé étrangère et les contraintes de validation, les contraintes étant renforcées pour les opérations d'utilisateurs mais pas pour les opérations d'agents.  
  
 Pour plus d'informations sur le paramétrage des options de schéma permettant de contrôler si les contraintes sont répliquées, consultez [Specify Schema Options](../../../relational-databases/replication/publish/specify-schema-options.md).  
  
### <a name="how-do-i-manage-identity-columns"></a>Comment puis-je gérer les colonnes d'identité ?  
 La réplication fournit une gestion automatique de plages d'identité pour les topologies de réplication comprenant des mises à jour sur l'Abonné. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
### <a name="can-the-same-objects-be-published-in-different-publications"></a>Les mêmes objets peuvent-ils être publiés dans différentes publications ?  
 Oui, mais avec quelques restrictions. Pour plus d’informations, consultez la section « Publication de tables dans plusieurs publications » de la rubrique [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
### <a name="can-multiple-publications-use-the-same-distribution-database"></a>Plusieurs publications peuvent-elles utiliser la même base de données de distribution ?  
 Oui. Il n'existe aucune restriction sur le nombre ou les types de publications pouvant utiliser la même base de données de distribution. Toutes les publications d'un serveur de publication donné doivent utiliser le même serveur de distribution et la même base de données de distribution.  
  
 Si vous avez plusieurs publications, vous pouvez configurer plusieurs bases de données de distribution sur l'Abonné pour garantir que les données passant dans chaque base de données de distribution proviennent d'une seule publication. Utilisez la boîte de dialogue **Propriétés du serveur de distribution** ou [sp_adddistributiondb &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) pour ajouter une base de données de distribution. Pour plus d’informations sur l’accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
### <a name="how-do-i-find-information-on-the-distributor-and-publisher-such-as-which-objects-in-a-database-are-published"></a>Comment puis-je trouver des informations sur le serveur de distribution et de publication, par exemple quels objets d'une base de données sont publiés ?  
 Ces informations sont disponibles via [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]et plusieurs procédures stockées de réplication. Pour plus d’informations, voir [Distributor and Publisher Information Script](../../../relational-databases/replication/administration/distributor-and-publisher-information-script.md).  
  
### <a name="does-replication-encrypt-data"></a>La réplication chiffre-t-elle les données ?  
 Non. La réplication ne chiffre pas les données stockées dans la base de données ou transférées sur le réseau. Pour plus d’informations, consultez la section « Chiffrement » de la rubrique [Vue d’ensemble de la sécurité &#40;réplication&#41;](../../../relational-databases/replication/security/security-overview-replication.md).  
  
### <a name="how-do-i-replicate-data-over-the-internet"></a>Comment puis-je répliquer des données via Internet ?  
 Vous pouvez répliquer des données via Internet en utilisant :  
  
-   Un réseau privé virtuel (VPN). Pour plus d’informations, consultez [Publier des données sur Internet à l’aide d’un réseau privé virtuel](../../../relational-databases/replication/publish-data-over-the-internet-using-vpn.md).  
  
-   L'option de synchronisation Web pour la réplication de fusion. Pour plus d’informations, voir [Web Synchronization for Merge Replication](../../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
 Tous les types de réplication [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peuvent répliquer des données via un réseau privé virtuel (VPN), mais vous devrez plutôt utiliser la synchronisation Web si vous utilisez une réplication de fusion.  
  
### <a name="does-replication-resume-if-a-connection-is-dropped"></a>La réplication reprend-elle si une connexion est supprimée ?  
 Oui. Le traitement de la réplication reprend là où il s'est interrompu si une connexion est supprimée. Si vous utilisez une réplication de fusion sur un réseau non fiable, envisagez d'utiliser des enregistrements logiques qui garantissent que les modifications liées sont traitées en tant qu'unité. Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
### <a name="does-replication-work-over-low-bandwidth-connections-does-it-use-compression"></a>La réplication fonctionne-t-elle sur les connexions à faible bande passante ? Utilise-t-elle la compression ?  
 Oui, la réplication fonctionne sur les connexions à faible bande passante. Pour les connexions via TCP/IP, elle utilise la compression fournie par le protocole mais ne fournit pas de compression supplémentaire. Pour les connexions à synchronisation Web via HTTPS, elle utilise la compression fournie par le protocole ainsi qu'une compression supplémentaire des fichiers XML utilisés pour répliquer les modifications.  
  
## <a name="logins-and-object-ownership"></a>Noms d'accès et propriété d'objet  
  
### <a name="are-logins-and-passwords-replicated"></a>Les noms d'accès et mots de passe sont-ils répliqués ?  
 Non. Vous pourriez créer un package DTS pour transférer les noms d'accès et mots de passe d'un serveur de publication sur un ou plusieurs abonnés.  
  
### <a name="what-are-schemas-and-how-are-they-replicated"></a>Que sont les schémas et comment sont-ils répliqués ?  
 À commencer par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], *schéma* a deux significations :  
  
-   La définition d'un objet, par exemple une instruction CREATE TABLE. Par défaut, la réplication copie les définitions de tous les objets répliqués sur l'Abonné.  
  
-   L’espace de noms dans lequel un objet est créé : \<Base de données>.\<Schéma>.\<Objet>. Les schémas sont définis à l'aide de l'instruction CREATE SCHEMA.  
  
-   La réplication fonctionne par défaut de la façon suivante dans l'Assistant Nouvelle publication quant aux schémas et à la propriété des objets :  
  
-   Pour les articles de publications de fusion d'un niveau de compatibilité de 90 ou supérieur, les publications d'instantané et les publications transactionnelles : par défaut, le propriétaire de l'objet sur l'Abonné est le même que le propriétaire de l'objet correspondant sur le serveur de publication. Si les schémas propriétaires des objets n'existent pas sur l'Abonné, ils sont créés automatiquement.  
  
-   Pour les articles de publications de fusion d'un niveau de compatibilité inférieur à 90 : par défaut, le propriétaire est laissé vide et spécifié comme **dbo** pendant la création de l'objet sur l'Abonné.  
  
-   Pour les articles des publications Oracle : par défaut, le propriétaire est spécifié comme **dbo**.  
  
-   Pour les articles de publications utilisant les instantanés en mode caractère (utilisées pour les abonnés non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les abonnés [!INCLUDE[ssEW](../../../includes/ssew-md.md)] ) : le propriétaire est laissé vide par défaut. Le propriétaire prend les valeurs par défaut du propriétaire associé au compte utilisé par l'Agent de distribution ou l'Agent de fusion pour se connecter à l'Abonné.  
  
 Le propriétaire de l’objet peut être changé par le biais de la boîte de dialogue **Propriétés de l’article - \<***Article***>** et les procédures stockées suivantes : **sp_addarticle**, **sp_addmergearticle**, **sp_changearticle** et **sp_changemergearticle**. Pour plus d’informations, consultez [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md), [Définir un article](../../../relational-databases/replication/publish/define-an-article.md) et [Afficher et modifier les propriétés d’un article](../../../relational-databases/replication/publish/view-and-modify-article-properties.md).  
  
### <a name="how-can-grants-on-the-subscription-database-be-configured-to-match-grants-on-the-publication-database"></a>Comment configurer les demandes sur la base de données d'abonnement afin qu'elles correspondent aux demandes sur la base de données de publication ?  
 Par défaut, la réplication n'exécute pas d'instructions GRANT sur la base de données d'abonnement. Si vous voulez que les autorisations sur la base de données d'abonnement correspondent à celles de la base de données de publication, utilisez une des méthodes suivantes :  
  
-   Exécutez les instructions GRANT directement sur la base de données d'abonnement.  
  
-   Utilisez un script après l'instantané pour exécuter les instructions. Pour plus d’informations, consultez [Exécuter des scripts avant et après l’application de l’instantané](../../../relational-databases/replication/execute-scripts-before-and-after-the-snapshot-is-applied.md).  
  
-   Utilisez la procédure stockée [sp_addscriptexec](../../../relational-databases/system-stored-procedures/sp-addscriptexec-transact-sql.md) pour exécuter les instructions.  
  
### <a name="what-happens-to-permissions-granted-in-a-subscription-database-if-a-subscription-is-reinitialized"></a>Qu'advient-il des autorisations accordées dans une base de données d'abonnement si un abonnement est réinitialisé ?  
 Par défaut, les objets sur l'Abonné sont supprimés et recréés lorsqu'un abonnement est réinitialisé, ce qui entraîne la suppression de toutes les autorisations accordées à ces objets. Il existe deux moyens de gérer cela :  
  
-   Appliquer à nouveau les demandes après la réinitialisation à l'aide des techniques décrites dans la section précédente.  
  
-   Spécifier de ne pas supprimer les objets lorsque l'abonnement est réinitialisé. Avant la réinitialisation, vous pouvez au choix :  
  
    -   Exécutez [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) ou [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Spécifiez une valeur « pre_creation_cmd » (**sp_changearticle**) ou « pre_creation_command » (**sp_changemergearticle**) pour le paramètre **@property** et une valeur « none », « delete » ou « truncate » pour le paramètre **@value**.  
  
    -   Dans la boîte de dialogue **Propriétés de l’article - \<Article>** de la section **Objet de destination**, sélectionnez une valeur de **Conserver l’objet existant inchangé**, **Supprimer les données. Si l’article possède un filtre de lignes, supprimez uniquement les données qui correspondent au filtre** ou **Tronquer toutes les données dans l’objet existant** pour l’option **Action si le nom est déjà utilisé**. Pour plus d’informations sur l’accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
## <a name="database-maintenance"></a>Maintenance de la base de données  
  
### <a name="why-cant-i-run-truncate-table-on-a-published-table"></a>Pourquoi ne puis-je pas exécuter TRUNCATE TABLE sur une table publiée ?  
 TRUNCATE TABLE est une instruction DDL qui ne consigne pas les suppressions de lignes individuelles et qui n’exécute pas de déclencheurs DML. Cette opération n’est pas autorisée, car la réplication ne peut pas assurer le suivi des modifications engendrées par cette opération : la réplication transactionnelle assure le suivi des modifications via le journal des transactions, tandis que la réplication de fusion s’en charge par le biais de déclencheurs DMS dans les tables publiées.  
  
### <a name="what-is-the-effect-of-running-a-bulk-insert-command-on-a-replicated-database"></a>Quel est l'effet de l'exécution d'une commande d'insertion en bloc sur une base de données répliquée ?  
 Pour la réplication transactionnelle, les insertions en bloc sont suivies et répliquées comme les autres insertions. Pour la réplication de fusion, vous devez vérifier que les métadonnées de suivi des modifications sont correctement mises à jour.  
  
### <a name="are-there-any-replication-considerations-for-backup-and-restore"></a>Existe-t-il des règles de réplication pour l'enregistrement et la restauration ?  
 Oui. Il existe de nombreuses règles spécifiques pour les bases de données impliquées dans une réplication. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données répliquées](../../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).  
  
### <a name="does-replication-affect-the-size-of-the-transaction-log"></a>La réplication affecte-t-elle la taille du journal des transactions ?  
 La réplication de fusion et la réplication d'instantané n'affectent pas la taille du journal des transactions, contrairement à la réplication transactionnelle. Si une base de données comprend une ou plusieurs publications transactionnelles, le journal n'est pas tronqué tant que toutes les transactions relatives aux publications n'ont pas été remises à la base de données de distribution. Si la taille du journal des transactions devient trop importante et que l'Agent de lecture du journal s'exécute de manière planifiée, envisagez de réduire l'intervalle entre les exécutions. Ou, définissez-le pour qu'il s'exécute en mode continu. S'il est paramétré pour s'exécuter en mode continu (par défaut), vérifiez qu'il fonctionne. Pour plus d’informations sur la vérification de l’état de l’Agent de lecture du journal, consultez [Afficher des informations et effectuer des tâches pour les agents associés à une publication &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md).  
  
 De plus, si vous avez défini l'option « sync with backup » sur la base de données de publication ou la base de données de distribution, le journal des transactions n'est pas tronqué tant que toutes les transactions ne sont pas sauvegardées. Si la taille du journal des transactions devient trop importante et que cette option est définie, envisagez de réduire l'intervalle entre les enregistrements du journal des transactions. Pour plus d’informations sur la sauvegarde et la restauration des bases de données impliquées dans la réplication transactionnelle, consultez [Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d’instantané](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
### <a name="how-do-i-rebuild-indexes-or-tables-in-replicated-databases"></a>Comment puis-je reconstruire les index ou les tables dans des bases de données répliquées ?  
 Il existe différents mécanismes pour la reconstruction d'index. Ils peuvent tous être utilisés sans règle spécifique à la réplication, à l'exception suivante près : des clés primaires sont nécessaires sur les tables dans les publications transactionnelles, vous ne pouvez donc pas supprimer et recréer de clés primaires sur ces tables.  
  
### <a name="how-do-i-add-or-change-indexes-on-publication-and-subscription-databases"></a>Comment puis-je ajouter ou modifier des index sur des bases de données de publication ou d'abonnement ?  
 Les index peuvent être ajoutés sur le serveur de publication ou sur les abonnés sans règle spécifique à la réplication (sachez que les index peuvent affecter les performances). CREATE INDEX et ALTER INDEX ne sont pas répliqués, donc si par exemple vous ajoutez ou modifiez un index sur le serveur de publication, vous devez effectuer le même ajout ou la même modification sur l'Abonné si vous souhaitez qu'il ou qu'elle soit prise en compte.  
  
### <a name="how-do-i-move-or-rename-files-for-databases-involved-in-replication"></a>Comment puis-je déplacer ou renommer des fichiers pour des bases de données impliquées dans une réplication ?  
 Dans les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], le déplacement ou le changement de noms de fichiers de base de données nécessitait le détachement et le rattachement de la base de données. Comme une base de données répliquée ne peut pas être détachée, la réplication a d'abord du être supprimée de ces bases de données. Avec [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], vous pouvez déplacer ou renommer les fichiers sans détachement ou rattachement de la base de données, sans aucune incidence sur la réplication. Pour plus d’informations sur le déplacement et le changement de noms de fichiers, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql.md).  
  
### <a name="how-do-i-drop-a-table-that-is-being-replicated"></a>Comment puis-je supprimer une table en cours de réplication ?  
 Commencez par supprimer l’article de la publication en utilisant [sp_droparticle](../../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md), [sp_dropmergearticle](../../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md) ou la boîte de dialogue **Propriétés de la publication - \<Publication>**, puis supprimez-le de la base de données à l’aide de `DROP <Object>`. Vous ne pouvez pas supprimer d'articles d'un instantané ou de publications transactionnelles après avoir ajouté les abonnements, vous devez d'abord supprimer les abonnements. Pour plus d’informations, consultez [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
### <a name="how-do-i-add-or-drop-columns-on-a-published-table"></a>Comment puis-je ajouter ou supprimer des colonnes sur une table publiée ?  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge une large variété de modifications de schéma sur les objets publiés, y compris l'ajout et la suppression de colonnes. Par exemple, si vous exécutez ALTER TABLE … DROP COLUMN sur le serveur de publication, l’instruction est répliquée vers les abonnés, puis exécutée pour supprimer la colonne. Les abonnés qui exécutent des versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] prennent en charge l'ajout et la suppression de colonnes à travers les procédures stockées [sp_repladdcolumn](../../../relational-databases/system-stored-procedures/sp-repladdcolumn-transact-sql.md) et [sp_repldropcolumn](../../../relational-databases/system-stored-procedures/sp-repldropcolumn-transact-sql.md). Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
## <a name="replication-maintenance"></a>Maintenance de réplication  
  
### <a name="how-do-i-determine-if-the-data-at-subscribers-is-synchronized-with-data-at-the-publisher"></a>Comment puis-je déterminer si les données sur les abonnés sont synchronisées avec celles du serveur de publication ?  
 Utilisez la validation. La validation fournit un rapport sur la synchronisation ou non de l'Abonné avec le serveur de publication. Pour plus d’informations, consultez [Valider des données répliquées](../../../relational-databases/replication/validate-replicated-data.md). La validation ne fournit pas d'informations sur les lignes non correctement synchronisées, à l'inverse de l'utilitaire [tablediff utility](../../../tools/tablediff-utility.md) .  
  
### <a name="how-do-i-add-a-table-to-an-existing-publication"></a>Comment puis-je ajouter une table à une publication existante ?  
 Il n'est pas nécessaire d'arrêter l'activité sur les bases de données de publication ou d'abonnement pour ajouter une table (ou un autre objet). Ajoutez une table à une publication par le biais de la boîte de dialogue **Propriétés de la publication - \<Publication>** ou des procédures stockées [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) et [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Pour plus d’informations, consultez [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
### <a name="how-do-i-remove-a-table-from-a-publication"></a>Comment puis-je supprimer une table d'une publication ?  
 Supprimez une table de la publication en utilisant [sp_droparticle](../../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md), [sp_dropmergearticle](../../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md) ou la boîte de dialogue **Propriétés de la publication - \<Publication>**. Vous ne pouvez pas supprimer d'articles d'un instantané ou de publications transactionnelles après avoir ajouté les abonnements, vous devez d'abord supprimer les abonnements. Pour plus d’informations, consultez [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
### <a name="what-actions-require-subscriptions-to-be-reinitialized"></a>Quelles actions nécessitent de réinitialiser les abonnements ?  
 Il existe de nombreuses modifications d'articles et de publications qui nécessitent de réinitialiser les abonnements. Pour plus d’informations, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
### <a name="what-actions-cause-snapshots-to-be-invalidated"></a>Quelles actions entraînent l'invalidation d'instantanés ?  
 Il existe de nombreuses modifications d'articles et de publications qui invalident les instantanés et nécessitent de générer un nouvel instantané. Pour plus d’informations, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
### <a name="how-do-i-remove-replication"></a>Comment puis-je supprimer une réplication ?  
 Les actions nécessaires à la suppression de la réplication d'une base de données varient suivant que la base de données ait servi de base de données de publication, de base de données d'abonnement, ou des deux à la fois.  
  
### <a name="how-do-i-determine-whether-there-are-transactions-or-rows-to-be-replicated"></a>Comment puis-je déterminer s'il existe des transactions ou des lignes à répliquer ?  
 Pour la réplication transactionnelle, utilisez les procédures stockées ou l'onglet **Commandes non distribuées** du Moniteur de réplication. Pour plus d’informations, consultez [Afficher les commandes répliquées et d’autres informations dans la base de données de distribution &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/monitor/view-replicated-commands-and-information-in-distribution-database.md) et [Afficher des informations et effectuer des tâches pour les agents associés à un abonnement &#40;moniteur de réplication&#41;](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
 Pour la réplication de fusion, utilisez la procédure stockée **sp_showpendingchanges**. Pour plus d’informations, consultez [sp_showpendingchanges &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-showpendingchanges-transact-sql.md).  
  
### <a name="how-far-behind-is-the-distribution-agent-should-i-reinitialize"></a>L'Agent de distribution est-il en retard ? Dois-je réinitialiser ?  
 Utilisez la procédure stockée **sp_replmonitorsubscriptionpendingcmds** ou l'onglet **Commandes non distribuées** du Moniteur de réplication. La procédure stockée et l'onglet affichent :  
  
-   Nombre de commandes dans la base de données de distribution qui n'ont pas été distribuées à l'abonné sélectionné. Une commande est constituée d'une instruction DML (Data Manipulation Language) Transact-SQL ou d'une instruction DDL (Data Definition Language).  
  
-   Délai estimé de distribution des commandes à l'Abonné. Si cette valeur est supérieure au délai nécessaire pour générer et appliquer un instantané à l'Abonné, réinitialisez l'Abonné. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
 Pour plus d’informations, consultez [sp_replmonitorsubscriptionpendingcmds (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-replmonitorsubscriptionpendingcmds-transact-sql.md) et [Afficher des informations et effectuer des tâches pour les agents associés à un abonnement (moniteur de réplication)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-subscription-agents.md).  
  
## <a name="replication-and-other-database-features"></a>Réplication et autres fonctionnalités de base de données  
  
### <a name="does-replication-work-in-conjunction-with-log-shipping-and-database-mirroring"></a>La réplication fonctionne-t-elle conjointement avec la copie des journaux et la mise en miroir de bases de données ?  
 Oui. Pour plus d’informations, consultez [Copie des journaux de transaction et réplication &#40;SQL Server&#41;](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md) et [Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
### <a name="does-replication-work-in-conjunction-with-clustering"></a>La réplication fonctionne-t-elle conjointement avec le clustering ?  
 Oui. Aucune règle spécifique n'est nécessaire car toutes les données sont stockées sur un ensemble de disques sur le cluster.  
  
## <a name="see-also"></a> Voir aussi  
 [Administration &#40;réplication&#41;](../../../relational-databases/replication/administration/administration-replication.md)   
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
