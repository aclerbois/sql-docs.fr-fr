---
title: sp_trace_setevent (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_trace_setevent_TSQL
- sp_trace_setevent
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setevent
ms.assetid: 7662d1d9-6d0f-443a-b011-c901a8b77a44
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 352d406b2c8735cb09787e9cd1554a4df7dd1bc0
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptracesetevent-transact-sql"></a>sp_trace_setevent (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute ou supprime un événement ou une colonne d'événement dans une trace. **sp_trace_setevent** peut être exécutée uniquement sur les traces existantes qui sont arrêtées (*état* est **0**). Une erreur est retournée si cette stockée procédure est exécutée sur une trace qui n’existe pas ou dont le *état* n’est pas **0**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des événements étendus.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_trace_setevent [ @traceid = ] trace_id   
          , [ @eventid = ] event_id  
          , [ @columnid = ] column_id  
          , [ @on = ] on  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@traceid=** ] *trace_id*  
 ID de la trace à modifier. *l’argument trace_id* est **int**, sans valeur par défaut. L’utilisateur emploie cette *trace_id* valeur pour identifier, modifier et contrôler la trace.  
  
 [  **@eventid=** ] *event_id*  
 ID de l'événement à activer. *event_id* est **int**, sans valeur par défaut.  
  
 Ce tableau répertorie les événements qui peuvent être ajoutés ou supprimés d'une trace.  
  
|Numéro d'événement|Nom d'événement| Description|  
|------------------|----------------|-----------------|  
|0-9|Réservé|Réservé|  
|10|RPC:Completed|Se produit lorsqu'un appel de procédure distante (RPC) s'est terminé.|  
|11|RPC:Starting|Se produit lorsqu'un appel de procédure distante a commencé.|  
|12|SQL:BatchCompleted|Se produit lorsqu'un traitement d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] est terminé.|  
|13|SQL:BatchStarting|Se produit lorsqu'un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] a démarré.|  
|14|Audit Login|Se produit lorsqu'un utilisateur réussit à se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|15|Audit Logout|Se produit lorsqu'un utilisateur se déconnecte de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|16|Attention|Se produit en même temps que les événements d'avertissement, tels que les requêtes d'interruption du client ou les ruptures de connexion client.|  
|17|ExistingConnection|Détecte toutes les activités des utilisateurs connectés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant le démarrage de la trace.|  
|18|Audit Server Starts And Stops|Se produit lorsque l'état des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est modifié.|  
|19|DTCTransaction|Assure le suivi des transactions coordonnées MS DTC ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator) entre plusieurs bases de données.|  
|20|Audit Login Failed|Indique qu'une tentative de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] depuis un client a échoué.|  
|21|EventLog|Indique que les événements ont été enregistrés dans le journal des applications Windows.|  
|22|ErrorLog|Indique que les événements d'erreurs ont été enregistrés dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|23|Lock:Release|Indique qu'un verrou sur une ressource, une page par exemple, a été débloqué.|  
|24|Lock:Acquire|Indique qu'un verrou a été acquis sur une ressource, une page de données par exemple.|  
|25|Lock:Deadlock|Indique que deux transactions concurrentes ont généré un interblocage, l'une essayant d'obtenir des verrous incompatibles sur des ressources appartenant à l'autre.|  
|26|Lock:Canceled|Indique que l'acquisition d'un verrou sur une ressource a été annulée (par exemple à cause d'un interblocage).|  
|27|Lock:Timeout|Indique qu'une demande de verrou sur une ressource (par exemple une page) a dépassé le délai d'attente parce qu'une autre transaction retient un verrou bloquant sur la ressource requise. Délai d’expiration est déterminée par le @@LOCK_TIMEOUT la fonction et peut être définie avec l’instruction SET LOCK_TIMEOUT.|  
|28|Degré d'événement de parallélisme (Insertion 7.0)|Se produit avant l'exécution d'une instruction SELECT, INSERT ou UPDATE.|  
|29-31|Réservé|Utilisez plutôt l'événement 28.|  
|32|Réservé|Réservé|  
|33|Exception|Indique qu'une exception s'est produite dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|34|SP:CacheMiss|Indique qu'une procédure stockée est introuvable dans le cache de procédure.|  
|35|SP:CacheInsert|Indique qu'un élément est inséré dans le cache de procédure.|  
|36|SP:CacheRemove|Indique qu'un élément est supprimé du cache de procédure.|  
|37|SP:Recompile|Indique qu'une procédure stockée a été recompilée.|  
|38|SP:CacheHit|Indique qu'une procédure stockée est trouvée dans le cache de procédure.|  
|39|Déprécié|Déprécié|  
|40|SQL:StmtStarting|Se produit lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] a démarré.|  
|41|SQL:StmtCompleted|Se produit lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est terminée.|  
|42|SP:Starting|Indique que la procédure stockée a démarré.|  
|43|SP:Completed|Indique que la procédure stockée s'est terminée.|  
|44|SP:StmtStarting|Indique qu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] d'une procédure stockée a commencé à s'exécuter.|  
|45|SP:StmtCompleted|Indique qu'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] d'une procédure stockée a fini de s'exécuter.|  
|46|Object:Created|Indique qu'un objet a été créé, par exemple avec les instructions CREATE INDEX, CREATE TABLE et CREATE DATABASE.|  
|47|Object:Deleted|Indique qu'un objet a été supprimé, par exemple avec les instructions DROP INDEX et DROP TABLE.|  
|48|Réservé||  
|49|Réservé||  
|50|SQL Transaction|Assure le suivi des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] BEGIN, COMMIT, SAVE et ROLLBACK TRANSACTION.|  
|51|Scan:Started|Indique qu'une analyse de table ou d'index a démarré.|  
|52|Scan:Stopped|Indique qu'une analyse de table ou d'index s'est terminée.|  
|53|CursorOpen|Indique qu'un curseur a été ouvert dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] par ODBC, OLE DB ou la bibliothèque de bases de données.|  
|54|TransactionLog|Trace à quel moment les transactions sont écrites dans le journal des transactions.|  
|55|Avertissement de hachage|Indique qu'une opération de hachage (jointure hachée, agrégation hachée, union hachée, hachage distinct) qui n'est pas en cours sur un tampon de partition bascule vers un plan de rechange. Ceci se produit à cause de la profondeur de récurrence, du décalage des données, des indicateurs de trace ou du comptage des bits.|  
|56-57|Réservé||  
|58|Auto Stats|Indique qu'une mise à jour automatique des statistiques d'index a eu lieu.|  
|59|Lock:Deadlock Chain|Produit pour chacun des événements entraînant le blocage.|  
|60|Lock:Escalation|Se produit lorsqu'un verrouillage spécifique s'est transformé en verrouillage de grande ampleur (par exemple, un verrou de page augmenté ou converti en verrou TABLE ou HoBT).|  
|61|OLE DB Errors|Indique qu'une erreur OLE DB s'est produite.|  
|62-66|Réservé||  
|67|ExecutionWarnings|Indique les messages d'avertissement survenus lors de l'exécution d'une instruction ou d'une procédure stockée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|68|Showplan Text (non encodée)|Affiche l'arborescence du plan de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutée.|  
|69|Sort Warnings|Indique les opérations de tri ne pouvant pas être effectuées en mémoire. N'inclut pas les opérations de tri comprenant la création d'index, seulement les opérations de tri au sein d'une requête (une clause ORDER BY dans une commande SELECT, par exemple).|  
|70|CursorPrepare|Indique qu'un curseur dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est préparé pour être utilisé par ODBC, OLE DB ou la bibliothèque de bases de données.|  
|71|Prepare SQL|ODBC, OLE DB ou la bibliothèque de bases de données a préparé une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à utiliser.|  
|72|Exec Prepared SQL|ODBC, OLE DB ou la bibliothèque de bases de données a exécuté une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] préparées.|  
|73|Unprepare SQL|ODBC, OLE DB ou la bibliothèque de bases de données a annulé la préparation (supprimé) d'une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] préparées.|  
|74|CursorExecute|Un curseur qui a été précédemment préparé dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] par ODBC, OLE DB ou la bibliothèque de bases de données est exécuté.|  
|75|CursorRecompile|Un curseur ouvert dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] par ODBC ou la bibliothèque de bases de données a été recompilé soit directement, soit du fait d'un changement de schéma.<br /><br /> Déclenché pour les curseurs ANSI et non-ANSI.|  
|76|CursorImplicitConversion|Un curseur dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est converti par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'un type à un autre.<br /><br /> Déclenché pour les curseurs ANSI et non-ANSI.|  
|77|CursorUnprepare|Un curseur préparé dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] voit sa préparation annulée (= est supprimé) par ODBC, OLE DB ou la bibliothèque de bases de données.|  
|78|CursorClose|Un curseur qui a été précédemment ouvert dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] par ODBC, OLE DB ou la bibliothèque de bases de données est fermé.|  
|79|Missing Column Statistics|Des statistiques de colonne qui auraient pu être utiles pour l'optimiseur ne sont pas disponibles.|  
|80|Missing Join Predicate|Une requête sans prédicat de jointure est en cours d'exécution. Ceci peut rendre la requête longue à exécuter.|  
|81|Server Memory Change|L'utilisation de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a augmenté ou diminué, soit de 1 mégaoctet (Mo), soit de 5 % de la taille maximale de la mémoire du serveur, selon celle qui est la plus importante.|  
|82-91|User Configurable (0 -9)|Données d’événement définies par l'utilisateur.|  
|92|Data File Auto Grow|Indique qu'un fichier de données a été augmenté automatiquement par le serveur.|  
|93|Log File Auto Grow|Indique qu'un fichier journal a été augmenté automatiquement par le serveur.|  
|94|Data File Auto Shrink|Indique qu'un fichier de données a été réduit automatiquement par le serveur.|  
|95|Log File Auto Shrink|Indique qu'un fichier journal a été réduit automatiquement par le serveur.|  
|96|Showplan Text|Affiche l'arborescence du plan de requête de l'instruction SQL à partir de l'optimiseur de requête. Notez que la **TextData** colonne ne contient pas le plan d’exécution pour cet événement.|  
|97|Showplan All|Affiche le plan de requête avec les détails complets de la compilation de l'instruction SQL en cours d'exécution. Notez que la **TextData** colonne ne contient pas le plan d’exécution pour cet événement.|  
|98|Showplan Statistics Profile|Affiche le plan de requête avec les détails complets de l'exécution de l'instruction SQL en cours. Notez que la **TextData** colonne ne contient pas le plan d’exécution pour cet événement.|  
|99|Réservé||  
|100|RPC Output Parameter|Produit les valeurs de sortie des paramètres pour tous les RPC.|  
|101|Réservé||  
|102|Audit Database Scope GDR|Se produit chaque fois qu'une instruction GRANT DENY ou REVOKE est émise pour une autorisation d'instruction par tout utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les actions de base de données uniquement, telles que l'accord d'autorisations sur une base de données.|  
|103|Audit Object GDR Event|Se produit chaque fois qu'un utilisateur émet une instruction GRANT, DENY ou REVOKE relative à une autorisation d'objet dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|104|Audit AddLogin Event|Se produit lorsqu’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion est ajoutée ou supprimée ; pour **sp_addlogin** et **sp_droplogin**.|  
|105|Audit Login GDR Event|Se produit lorsqu’un droit de connexion Windows est ajouté ou supprimé ; pour **sp_grantlogin**, **sp_revokelogin**, et **sp_denylogin**.|  
|106|Audit Login Change Property Event|Se produit lorsqu’une propriété d’une connexion, à l’exception des mots de passe est modifiée ; pour **sp_defaultdb** et **sp_defaultlanguage**.|  
|107|Audit Login Change Password Event|Se produit lorsqu'un mot de passe de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est modifié.<br /><br /> Les mots de passe ne sont pas enregistrés.|  
|108|Audit Add Login to Server Role Event|Se produit lorsqu’une connexion est ajoutée ou supprimée d’un rôle de serveur fixe ; pour **sp_addsrvrolemember**, et **sp_dropsrvrolemember**.|  
|109|Audit Add DB User Event|Se produit lorsqu’une connexion est ajoutée ou supprimée comme utilisateur de base de données (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) dans une base de données ; pour **sp_grantdbaccess**, **sp_revokedbaccess**, **sp_adduser**, et **sp_dropuser**.|  
|110|Audit Add Member to DB Role Event|Se produit lorsqu’une connexion est ajoutée ou supprimée en tant qu’un utilisateur de base de données (fixe ou défini par l’utilisateur) à une base de données ; pour **sp_addrolemember**, **sp_droprolemember**, et **sp_changegroup**.|  
|111|Audit Add Role Event|Se produit lorsqu’une connexion est ajoutée ou supprimée comme utilisateur de base de données à une base de données ; pour **sp_addrole** et **sp_droprole**.|  
|112|Audit App Role Change Password Event|Se produit lorsque le mot de passe d'un rôle d'application est modifié.|  
|113|Audit Statement Permission Event|Se produit lorsqu'une autorisation d'instruction (par exemple, CREATE TABLE) est utilisée.|  
|114|Audit Schema Object Access Event|Se produit lorsqu'une autorisation d'objet (par exemple, SELECT) est utilisée, de manière réussie ou non.|  
|115|Événement de sauvegarde/restauration d’audit|Se produit lorsqu'une commande BACKUP ou RESTORE est émise.|  
|116|Audit DBCC Event|Se produit lorsque des commandes DBCC sont émises.|  
|117|Audit Change Audit Event|Se produit lorsque des modifications de trace d'audit sont effectuées.|  
|118|Audit Object Derived Permission Event|Se produit à l'émission de commandes des objets CREATE, ALTER et DROP.|  
|119|OLEDB Call Event|Se produit lorsque des appels de fournisseur OLE DB sont effectués pour des requêtes distribuées et des procédures stockées distantes.|  
|120|OLEDB QueryInterface Event|Se produit lorsque OLE DB **QueryInterface** appels sont effectués pour les requêtes distribuées et les procédures stockées distantes.|  
|121|OLEDB DataRead Event|Se produit lorsqu'un appel de requête de données est effectué vers le fournisseur OLE DB.|  
|122|Showplan XML|Se produit lors de l'exécution d'une instruction SQL. Incluez cet événement pour identifier des opérateurs Showplan. Chaque événement est stocké dans un document XML correct. Notez que la **binaire** colonne pour cet événement contient le plan d’exécution encodé. Utilisez SQL Server Profiler pour ouvrir la trace et afficher le plan d'exécution de requêtes.|  
|123|SQL:FullTextQuery|Se produit lors de l'exécution d'une requête de texte intégral.|  
|124|Broker:Conversation|Indique la progression d'une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|125|Deprecation Announcement|Se produit lorsque vous utilisez une fonction qui sera supprimée d'une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|126|Deprecation Final Support|Se produit lorsque vous utilisez une fonction qui sera supprimée de la prochaine version importante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|127|Exchange Spill Event|Se produit lorsque les tampons de communication dans un plan de requête parallèle ont été temporairement écrits dans le **tempdb** base de données.|  
|128|Audit Database Management Event|Intervient lors de la création, de la modification ou de la suppression d'une base de données.|  
|129|Audit Database Object Management Event|Se produit lorsqu'une instruction CREATE, ALTER ou DROP s'exécute sur des objets de base de données, tels que des schémas.|  
|130|Audit Database Principal Management Event|Intervient lors de la création, de la modification ou de la suppression d'une base de données de principaux, tels que des utilisateurs.|  
|131|Audit Schema Object Management Event|Intervient lors de la création, de la modification ou de la suppression d'objets serveur.|  
|132|Audit Server Principal Impersonation Event|Se produit en cas d'emprunt d'identité dans une étendue du serveur, comme EXECUTE AS LOGIN.|  
|133|Audit Database Principal Impersonation Event|Intervient si un emprunt d'identité se produit dans une étendue de base de données, comme EXECUTE AS USER ou SETUSER.|  
|134|Audit Server Object Take Ownership Event|Intervient lors de la modification du propriétaire par des objets dans l'étendue du serveur.|  
|135|Audit Database Object Take Ownership Event|Intervient lors de la modification d'un propriétaire par des objets au sein de l'étendue de base de données.|  
|136|Broker:Conversation Group|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée un nouveau groupe de conversations ou supprime un groupe de conversations existant.|  
|137|Blocked Process Report|Se produit lorsqu’un processus a été bloqué plus longtemps qu’un laps de temps. Cette classe d'événements n'inclut pas de tâches système ou de tâches qui attendent des ressources à blocage non détectable. Utilisez **sp_configure** pour configurer le seuil et la fréquence à laquelle les rapports sont générés.|  
|138|Broker:Connection|Indique l'état d'une connexion de transport gérée par [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|139|Broker:Forwarded Message Sent|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] transfère un message.|  
|140|Broker:Forwarded Message Dropped|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] supprime un message qui devait être transféré.|  
|141|Broker:Message Classify|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] détermine le routage d'un message.|  
|142|Broker:Transmission|Indique que des erreurs se sont produites dans la couche de transport de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Le numéro de l'erreur et les valeurs d'état indiquent la source de l'erreur.|  
|143|Broker:Queue Disabled|Indique qu'un message incohérent a été détecté parce qu'une file d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)] comportait cinq restaurations de transactions successives. L'événement contient l'ID de base de données et l'ID de file d'attente de la file d'attente contenant le message concerné.|  
|144-145|Réservé||  
|146|Showplan XML Statistics Profile|Se produit lors de l'exécution d'une instruction SQL. Identifie les opérateurs Showplan et affiche des données complètes de compilation. Notez que la **binaire** colonne pour cet événement contient le plan d’exécution encodé. Utilisez SQL Server Profiler pour ouvrir la trace et afficher le plan d'exécution de requêtes.|  
|148|Deadlock Graph|Se produit lors de l'annulation d'une tentative faisant partie d'un blocage et qui était choisie en tant que victime de blocage. Fournit une description XML d'un blocage.|  
|149|Broker:Remote Message Acknowledgement|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie ou reçoit un accusé de réception de message.|  
|150|Trace File Close|Intervient lors de la fermeture d'un fichier de trace au cours de la substitution de ce dernier.|  
|151|Réservé||  
|152|Audit Change Database Owner|Se produit lorsque ALTER AUTHORIZATION sert à modifier le propriétaire d'une base de données et que des autorisations sont vérifiées à cet effet.|  
|153|Audit Schema Object Take Ownership Even|Se produit lorsque ALTER AUTHORIZATION sert à affecter un propriétaire à un objet et que des autorisations sont vérifiées à cet effet.|  
|154|Réservé||  
|155|FT:Crawl Started|Se produit lorsqu'une analyse de texte intégral (remplissage) a démarré. Utilisez-la pour vérifier si une demande d'analyse est actuellement sélectionnée par des tâches de travail.|  
|156|FT:Crawl Stopped|Se produit lorsqu'une analyse de texte intégral (remplissage) s'arrête. Les arrêts se produisent lors de l'achèvement réussi d'une analyse ou si une erreur irrécupérable survient.|  
|157|FT:Crawl Aborted|Intervient lorsqu'une exception est détectée au cours d'une analyse de texte intégral. Elle engendre généralement l'arrêt de l'analyse de texte intégral.|  
|158|Audit Broker Conversation|Crée un rapport de messages d'audit associés à la sécurité du dialogue de [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|159|Audit Broker Login|Crée un rapport de messages d'audit associés à la sécurité de transport de [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|160|Broker:Message Undeliverable|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] ne parvient pas à conserver un message reçu qui aurait dû être remis à un service.|  
|161|Broker:Corrupted Message|Se produit lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] reçoit un message corrompu.|  
|162|User Error Message|Affiche des messages d'erreur que les utilisateurs aperçoivent en cas d'erreur ou d'exception.|  
|163|Broker:Activation|Se produit lorsqu'un moniteur de file d'attente démarre une procédure stockée d'activation, envoie une notification QUEUE_ACTIVATION, ou lorsqu'il existe une procédure stockée d'activation démarrée par un moniteur de file d'attente.|  
|164|Object:Altered|Se produit lors de la modification d'un objet de base de données.|  
|165|Performance statistics|Indique qu'un plan de requête compilé a été mis en mémoire cache pour la première fois, recompilé ou supprimé de la mémoire cache du plan.|  
|166|SQL:StmtRecompile|Se produit lors d'une recompilation au niveau de l'instruction.|  
|167|Database Mirroring State Change|Se produit lorsque l'état d'une base de données mise en miroir change.|  
|168|Showplan XML For Query Compile|Se produit lors de la compilation d'une instruction SQL. Affiche les données complètes de compilation. Notez que la **binaire** colonne pour cet événement contient le plan d’exécution encodé. Utilisez SQL Server Profiler pour ouvrir la trace et afficher le plan d'exécution de requêtes.|  
|169|Showplan All For Query Compile|Se produit lors de la compilation d'une instruction SQL. Affiche les données complètes, lors de la compilation. Utilisez cet événement pour identifier des opérateurs Showplan.|  
|170|Audit Server Scope GDR Event|Indique qu'un événement d'attribution, de refus ou de révocation d'autorisations s'est produit dans l'étendue du serveur, tel que la création d'une connexion.|  
|171|Audit Server Object GDR Event|Indique qu'un événement d'attribution, de refus ou de révocation d'un objet de schéma, tel qu'une table ou une fonction, s'est produit.|  
|172|Audit Database Object GDR Event|Indique qu'un événement d'attribution, de refus ou de révocation d'objets de base de données, tel que des assemblys et des schémas, s'est produit.|  
|173|Audit Server Operation Event|Se produit lorsque des opérations d'audit de sécurité, telles que la modification de paramètres, de ressources, d'accès externe ou d'autorisation sont utilisées.|  
|175|Audit Server Alter Trace Event|Se produit lorsqu'une instruction vérifie l'autorisation ALTER TRACE.|  
|176|Audit Server Object Management Event|Intervient lors de la création, de la modification ou de la suppression d'objets serveur.|  
|177|Audit Server Principal Management Event|Intervient lors de la création, de la modification ou de la suppression de principaux du serveur.|  
|178|Audit Database Operation Event|Se produit lorsque surviennent diverses opérations dans une base de données, tel qu'un point de contrôle ou une notification de requête d'abonnement.|  
|180|Audit Database Object Access Event|Se produit lors de l'accès à des objets de base de données, tels que des schémas.|  
|181|TM: Begin Tran starting|Intervient lors du démarrage d'une requête BEGIN TRANSACTION.|  
|182|TM: Begin Tran completed|Intervient lors de l'achèvement d'une requête BEGIN TRANSACTION.|  
|183|TM: Promote Tran starting|Intervient lors du démarrage d'une requête PROMOTE TRANSACTION.|  
|184|TM: Promote Tran completed|Intervient lors de l'achèvement d'une requête PROMOTE TRANSACTION.|  
|185|TM: Commit Tran starting|Intervient lors du démarrage d'une requête COMMIT TRANSACTION.|  
|186.|TM: Commit Tran completed|Intervient lors de l'achèvement d'une requête COMMIT TRANSACTION.|  
|187|TM: Rollback Tran starting|Intervient lors du démarrage d'une requête ROLLBACK TRANSACTION.|  
|188|TM: Rollback Tran completed|Intervient lors de l'achèvement d'une requête ROLLBACK TRANSACTION.|  
|189|Lock : Timeout (timeout > 0)|Se produit lors de l'expiration d'une demande de verrou sur une ressource, telle qu'une page.|  
|190|Progress Report: Online Index Operation|Indique la progression d'une opération de génération d'index en ligne pendant l'exécution du processus de création.|  
|191|TM: Save Tran starting|Intervient lors du démarrage d'une requête SAVE TRANSACTION.|  
|192|TM: Save Tran completed|Intervient lors de l'achèvement d'une requête SAVE TRANSACTION.|  
|193|Background Job Error|Se produit lorsque le travail en arrière-plan s'est terminé anormalement.|  
|194|OLEDB Provider Information|Intervient lorsqu'une requête distribuée s'exécute et collecte des informations correspondant à la connexion du fournisseur.|  
|195|Mount Tape|Se produit lorsqu'une demande de montage de bande est reçue.|  
|196|Assembly Load|Se produit lorsqu'une demande de chargement d'un assembly CLR est exécutée.|  
|197|Réservé||  
|198|XQuery Static Type|Se produit avant l'exécution d'une expression XQuery. Cette classe d'événements fournit le type statique de l'expression XQuery.|  
|199|QN: subscription|Se produit lorsqu'il est impossible de s'abonner à une inscription de requête. Le **TextData** colonne contienne des informations sur l’événement.|  
|200|QN: parameter table|Des informations sur les abonnements actifs sont stockées dans des tables de paramètres internes. Cette classe d'événements se produit lorsqu'une table de paramètres est créée ou supprimée. En général, ces tables sont créées ou supprimées lors du redémarrage de la base de données. Le **TextData** colonne contienne des informations sur l’événement.|  
|201|QN: template|Un modèle de requête représente une classe de requêtes d'abonnement. En règle générale, les requêtes de même classe sont identiques, à l'exception de leurs valeurs de paramètre. Cette classe d'événements se produit lorsqu'une nouvelle demande d'abonnement correspond à une classe déjà existante de (Correspondance), une nouvelle classe (Créer), ou une classe Supprimer, qui indique un nettoyage des modèles de classes de requêtes sans abonnement actif. Le **TextData** colonne contienne des informations sur l’événement.|  
|202|QN: dynamics|Effectue un suivi des activités internes de notifications de requête. Le **TextData** colonne contienne des informations sur l’événement.|  
|212|Bitmap Warning|Indique quand les filtres bitmap ont été désactivés dans une requête.|  
|213|Database Suspect Data Page|Indique qu’une page est ajoutée à la **suspect_pages** table **msdb**.|  
|214|CPU Threshold Exceeded|Indique quand le gouverneur de ressources détecte qu'une requête a dépassé la valeur de seuil de l'UC (REQUEST_MAX_CPU_TIME_SEC).|  
|215|Indique quand un déclencheur LOGON ou une fonction classifieur du gouverneur de ressources commence à s'exécuter.|Indique quand un déclencheur LOGON ou une fonction classifieur du gouverneur de ressources commence à s'exécuter.|  
|216|PreConnect:Completed|Indique quand un déclencheur LOGON ou la fonction classifieur du gouverneur de ressources se termine l’exécution.|  
|217|Plan Guide Successful|Indique que SQL Server a pu produire un plan d'exécution pour une requête ou un lot qui contenait un repère de plan.|  
|218|Plan Guide Unsuccessful|Indique que SQL Server n'a pas pu produire un plan d'exécution pour une requête ou un lot qui contenait un repère de plan. SQL Server a tenté de générer un plan d'exécution pour cette requête ou ce lot sans appliquer le repère de plan. Un repère de plan non valide peut être à l'origine de ce problème. Vous pouvez valider le repère de plan à l'aide de la fonction système sys.fn_validate_plan_guide.|  
|235|Audit Fulltext||  
  
 [  **@columnid=** ] *column_id*  
 ID de la colonne à ajouter pour l'événement. *column_id* est **int**, sans valeur par défaut.  
  
 Le tableau suivant répertorie les colonnes qui peuvent être ajoutées pour un événement.  
  
|Numéro de colonne|Nom de colonne| Description|  
|-------------------|-----------------|-----------------|  
|1|**TextData**|Valeur de texte dépendant de la classe d'événements capturée dans la trace.|  
|2|**BinaryData**|Valeur binaire dépendante de la classe d'événements capturés dans la trace.|  
|3|**DatabaseID**|ID de la base de données spécifiée par l’utilisation *base de données* instruction ou la base de données par défaut si aucune utilisation *base de données* instruction est émise pour une connexion donnée.<br /><br /> La valeur pour une base de données peut être déterminée à l'aide de la fonction DB_ID.|  
|4|**TransactionID**|ID affecté par le système à la transaction.|  
|5|**LineNumber**|Indique le numéro de la ligne qui contient l'erreur. Pour les événements qui impliquent des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] , comme **SP:StmtStarting**, la colonne **LineNumber** contient le numéro de ligne de l’instruction dans la procédure stockée ou le lot.|  
|6|**NTUserName**|Nom d'utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.|  
|7|**NTDomainName**|Domaine Windows auquel appartient l'utilisateur.|  
|8|**HostName**|Nom de l'ordinateur client à l'origine de la requête.|  
|9|**ClientProcessID**|ID affecté par l'ordinateur client au processus dans lequel s'exécute l'application cliente.|  
|10|**ApplicationName**|Nom de l'application cliente qui a créé la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|11|**LoginName**|Nom de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du client.|  
|12|**SPID**|ID de processus serveur affecté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus associé au client.|  
|13|**Duration**|Quantité de temps (en microsecondes) prise par l’événement. Cette colonne de données n'est pas remplie par l'événement Hash Warning.|  
|14|**StartTime**|Heure à laquelle a débuté l'événement, si disponible.|  
|15|**EndTime**|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme **SQL:BatchStarting** ou **SP:Starting**. Il n’est pas également remplie par le **Hash Warning** événement.|  
|16|**Reads**|Nombre de lectures logiques sur disque effectuées par le serveur pour l'événement. Cette colonne n’est pas remplie par le **verrou : publié** événement.|  
|17|**Writes**|Nombre d'écritures physiques effectuées par le serveur pour l'événement.|  
|18|**Unité centrale**|Temps processeur (en millisecondes) utilisé par l'événement.|  
|19|**Autorisations**|Représente l'image bitmap des autorisations ; utilisé par l'audit de sécurité.|  
|20|**Severity**|Niveau de gravité d'une exception.|  
|21|**EventSubClass**|Type de sous-classe d'événements. Cette colonne de données n'est pas remplie pour toutes les classes d'événements.|  
|22|**ObjectID**|ID affecté à l'objet par le système.|  
|23|**Réussi**|Succès de la tentative d'utilisation des autorisations ; utilisé pour l'audit.<br /><br /> **1** = réussite**0** = Échec|  
|24|**IndexID**|ID de l'index de l'objet affecté par l'événement. Pour déterminer l’ID d’index d’un objet, utilisez la colonne **indid** de la table système **sysindexes** .|  
|25|**IntegerData**|Valeur entière qui dépend de la classe d'événements capturée dans la trace.|  
|26|**ServerName**|Nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], *nom_serveur* ou *nomserveur\nominstance*, tracée.|  
|27|**EventClass**|Type de classe d'événement actuellement enregistrée.|  
|28|**ObjectType**|Type d'objet : table, fonction ou procédure stockée, par exemple.|  
|29|**NestLevel**|Niveau d'imbrication où s'exécute la procédure stockée. Consultez [@@NESTLEVEL &#40;Transact-SQL&#41;](../../t-sql/functions/nestlevel-transact-sql.md).|  
|30|**État**|État du serveur, en cas d'erreur.|  
|31|**Erreur**|Numéro d’erreur.|  
|32|**Mode**|Mode de verrouillage du verrou acquis. Cette colonne n’est pas remplie par le **verrou : publié** événement.|  
|33|**Handle**|Handle de l'objet référencé dans l'événement.|  
|34|**ObjectName**|Nom de l'objet en cours d'accès.|  
|35|**DatabaseName**|Nom de la base de données spécifié dans l’utilisation *base de données* instruction.|  
|36|**FileName**|Nom logique du nom de fichier modifié.|  
|37|**OwnerName**|Nom de propriétaire de l'objet référencé.|  
|38|**RoleName**|Nom du rôle de base de données ou de serveur ciblé par une instruction.|  
|39|**TargetUserName**|Nom d'utilisateur de la cible d'une action.|  
|40|**DBUserName**|Nom d'utilisateur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du client.|  
|41|**LoginSid**|Identificateur de sécurité (SID) de l'utilisateur connecté.|  
|42|**TargetLoginName**|Nom de connexion d'accès de la cible d'une action.|  
|43|**TargetLoginSid**|SID de la connexion d'accès qui représente la cible d'une action.|  
|44|**ColumnPermissions**|État des autorisations au niveau des colonnes ; utilisé par l'audit de sécurité.|  
|45|**LinkedServerName**|Nom du serveur lié.|  
|46|**ProviderName**|Nom du fournisseur OLE DB.|  
|47|**MethodName**|Nom de la méthode OLE DB.|  
|48|**RowCounts**|Nombre de lignes dans le traitement.|  
|49|**RequestID**|ID de la demande contenant l'instruction.|  
|50|**XactSequence**|Jeton servant à décrire la transaction en cours.|  
|51|**EventSequence**|Numéro de séquence de cet événement.|  
|52|**BigintData1**|**bigint** valeur, qui est dépendante de la classe d’événements capturée dans la trace.|  
|53|**BigintData2**|**bigint** valeur, qui est dépendante de la classe d’événements capturée dans la trace.|  
|54|**GUID**|Valeur GUID dépendante de la classe d'événements capturés dans la trace.|  
|55|**IntegerData2**|Valeur d'entier dépendante de la classe d'événements capturés dans la trace.|  
|56|**ObjectID2**|ID de l'objet ou de l'entité connexe, si disponible.|  
|57|**Type**|Valeur d'entier dépendante de la classe d'événements capturés dans la trace.|  
|58|**OwnerID**|Type de l'objet qui possède le verrou. Pour les événements de verrou uniquement.|  
|59|**ParentName**|Nom du schéma dans lequel se trouve l'objet.|  
|60|**IsSystem**|Indique si l'événement s'est produit sur un processus système ou sur un processus utilisateur.<br /><br /> **1** = système<br /><br /> **0** = utilisateur.|  
|61|**Offset**|Décalage de départ de l'instruction dans la procédure stockée ou le lot.|  
|62|**SourceDatabaseID**|ID de la base de données dans laquelle existe la source de l'objet.|  
|63|**SqlHandle**|Hachage 64 bits basé sur le texte d'une requête ad hoc ou sur l'ID de base de données et d'objet d'un objet SQL. Cette valeur peut être transmise à **sys.dm_exec_sql_text()** pour récupérer le texte SQL associé.|  
|64|**SessionLoginName**|Nom de connexion de l'utilisateur à l'origine de la session. Par exemple, si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de **Login1** et que vous exécutez une instruction en tant que **Login2**, **SessionLoginName** affiche **Login1**, tandis que **LoginName** affiche **Login2**. Cette colonne de données affiche les noms de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows.|  
  
 **[ @on=]** *sur*  
 Indique si l'événement doit être activé, ON (1) ou désactivé, OFF (0). *sur* est **bits**, sans valeur par défaut.  
  
 Si *sur* a la valeur **1**, et *column_id* est NULL, alors l’événement est défini sur ON et toutes les colonnes sont effacées. Si *column_id* n’est pas null, alors la colonne est définie sur ON pour cet événement.  
  
 Si *sur* a la valeur **0**, et *column_id* est NULL, l’événement est désactivé (OFF) et toutes les colonnes sont effacées. Si *column_id* n’est pas null, la colonne est désactivée (OFF).  
  
 Le tableau suivant illustre l’interaction entre **@on** et **@columnid**.  
  
|@on|@columnid|Résultat|  
|---------|---------------|------------|  
|ON (**1**)|NULL|Événement activé (ON).<br /><br /> Toutes les colonnes sont effacées.|  
||NOT NULL|La colonne est activée (ON) pour l'événement spécifié.|  
|OFF (**0**)|NULL|Événement désactivé (OFF).<br /><br /> Toutes les colonnes sont effacées.|  
||NOT NULL|La colonne est désactivée (OFF) pour l'événement spécifié.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Le tableau suivant décrit les valeurs de code que les utilisateurs peuvent recevoir à la fin de l'exécution de la procédure stockée.  
  
|Code de retour| Description|  
|-----------------|-----------------|  
|0|Aucune erreur.|  
|1|Erreur inconnue.|  
|2|La trace est en cours d'exécution. La modification de la trace à cet instant précis entraîne une erreur.|  
|3|L'événement spécifié n'est pas valide. L'événement peut ne pas exister ou être inapproprié pour la procédure stockée.|  
|4|La colonne spécifiée n'est pas valide.|  
|9|Le handle de trace spécifié n'est pas valide.|  
|11|La colonne spécifiée est utilisée de manière interne et ne peut pas être supprimée.|  
|13|Mémoire insuffisante. Renvoyé lorsqu'il n'y a pas assez de mémoire pour exécuter l'action spécifiée.|  
|16|La fonction n'est pas valide pour cette trace.|  
  
## <a name="remarks"></a>Notes  
 **sp_trace_setevent** effectue la plupart des actions effectuées antérieurement par les procédures stockées étendues disponibles dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez **sp_trace_setevent** au lieu de ce qui suit :  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_eventclassrequired**  
  
-   **xp_trace_seteventclassrequired**  
  
 Les utilisateurs doivent exécuter **sp_trace_setevent** pour chaque colonne ajoutée pour chaque événement. Durant chaque exécution, si **@on** a la valeur **1**, **sp_trace_setevent** ajoute l’événement spécifié à la liste des événements de la trace. Si **@on** a la valeur **0**, **sp_trace_setevent** supprime l’événement spécifié de la liste.  
  
 Les paramètres de Trace de SQL toutes les procédures stockées (**sp_trace_xx**) sont de type strict. Si ces paramètres ne sont pas appelés avec des types de données appropriés pour les paramètres d'entrée tels qu'ils sont spécifiés dans la description de l'argument, la procédure stockée renvoie une erreur.  
  
 Pour obtenir un exemple d’utilisation de procédures stockées de trace, consultez [Créer une trace &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation ALTER TRACE.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_trace_geteventinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [Trace SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
