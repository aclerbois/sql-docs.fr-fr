---
title: Dépannage des serveurs de publication Oracle | Microsoft Docs
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
- Oracle publishing [SQL Server replication], troubleshooting
- troubleshooting [SQL Server replication], Oracle publishing
ms.assetid: be94f1c1-816b-4b1d-83f6-2fd6f5807ab7
caps.latest.revision: 62
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: d849d3fdf5c0242c8d3b5f09af78d3649cb1d1f6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshooting-oracle-publishers"></a>Dépannage des serveurs de publication Oracle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique présente une liste de plusieurs problèmes qui peuvent se produire lors de la configuration et de l'utilisation d'un serveur de publication Oracle.  
  
## <a name="an-error-is-raised-regarding-oracle-client-and-networking-software"></a>Une erreur s'est produite avec le logiciel client et réseau d'Oracle  
 Le compte sous lequel [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécute sur le serveur de distribution doit avoir des autorisations en lecture et en exécution sur le répertoire (et tous les sous-répertoires) où le logiciel réseau client Oracle est installé. Si les autorisations ne sont pas accordées ou si les composants du client Oracle ne sont pas installés correctement, vous recevez le message d'erreur suivant :  
  
 « Échec de la connexion au serveur avec [Fournisseur Microsoft OLE DB pour Oracle]. Les composants client et réseau Oracle sont introuvables. Ces composants sont fournis par Oracle Corporation dans l'installation client d'Oracle Version 7.3.3 (ou ultérieure). Vous ne pourrez pas utiliser ce fournisseur avant d'avoir installé ces composants. »  
  
 Si un client Oracle approprié a été installé sur le serveur de distribution, vérifiez que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a été arrêté puis redémarré une fois le client installé. Cette opération permet à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de reconnaître les composants du client.  
  
 Si vous avez vérifié que ces autorisations sont accordées et que ces composants sont installés correctement, mais que cette erreur persiste, vérifiez que les paramètres du Registre pour HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSDTC\MTxOCI sont corrects :  
  
-   Pour Oracle 10g, les paramètres corrects sont  
  
    -   OracleOciLib = oci.dll  
  
    -   OracleSqlLib = orasql10.dll  
  
    -   OracleXaLib = oraclient10.dll  
  
-   Pour Oracle 9i, les paramètres corrects sont  
  
    -   OracleOciLib = oci.dll  
  
    -   OracleSqlLib = orasql9.dll  
  
    -   OracleXaLib = oraclient9.dll  
  
## <a name="the-sql-server-distributor-cannot-connect-to-the-oracle-database-instance"></a>Le serveur de distribution SQL Server ne peut pas se connecter à l'instance de base de données Oracle  
 Si le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas se connecter au serveur de publication Oracle, vérifiez que :  
  
-   Le logiciel Oracle requis est installé sur le serveur de distribution.  
  
-   La base de données Oracle est en ligne et que vous pouvez vous y connecter avec un outil comme SQL*Plus.  
  
-   La réplication de connexion utilisée pour se connecter au serveur de publication Oracle a des autorisations suffisantes. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
-   Les noms TNS définis lors de la configuration du serveur de publication Oracle figurent dans le fichier tnsnames.ora.  
  
-   Le dossier de base et le chemin d'accès d'Oracle corrects sont utilisés. Même si vous n'avez qu'un ensemble de fichiers binaires Oracle installés sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vérifiez que les variables d'environnement relatives au dossier de base d'Oracle sont définies correctement. Si vous modifiez des valeurs pour les variables d'environnement, vous devez arrêter et redémarrer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour que les modifications soient prises en compte.  
  
 Pour plus d’informations sur la configuration et le test de la connectivité, consultez « Installation et configuration de logiciels réseau clients Oracle sur le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] » dans [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="the-oracle-publisher-is-associated-with-another-distributor"></a>Le serveur de publication Oracle est associé à un autre serveur de distribution  
 Un serveur de publication Oracle peut être associé avec un seul serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Si un autre serveur de distribution est associé au serveur de publication Oracle, il doit être supprimé avant de pouvoir utiliser un autre distributeur. Si le serveur de distribution en cours n'est pas d'abord supprimé, vous recevez un des messages d'erreur suivants :  
  
-   « L’instance de serveur Oracle '\<*NomServeurPublicationOracle*>' a été précédemment configurée pour utiliser '\<*NomServeurDistributionSQLServer*>' en tant que serveur de distribution. Pour démarrer l’utilisation de '\<*NouveauNomServeurDistributionSQLServer*>' en tant que serveur de distribution, vous devez supprimer la configuration de réplication actuelle sur l’instance de serveur Oracle, ce qui entraînera également la suppression de toutes les publications sur cette instance de serveur. »  
  
-   « Le serveur Oracle '\<*NomServeurOracle*>' est déjà défini comme serveur de publication '\<*NomServeurPublicationOracle*>' sur le serveur de distribution '\<*NomServeurDistributionSQLServer*>.*\<NomBaseDeDonnéesDistribution>*. Supprimez le serveur de publication ou le synonyme public '*\<NomSynonyme>*' à recréer. »  
  
 Quand un serveur de publication Oracle est supprimé, les objets de réplication de la base de données Oracle sont automatiquement nettoyés. Cependant, un nettoyage manuel des objets de réplication Oracle est nécessaire dans certains cas. Pour nettoyer manuellement des objets de réplication Oracle créés par réplication :  
  
1.  Connectez-vous au serveur de publication Oracle avec des autorisations d'administrateur de base de données.  
  
2.  Lancez la commande SQL `DROP PUBLIC SYNONYM MSSQLSERVERDISTRIBUTOR;`.  
  
3.  Lancez la commande SQL `DROP USER <replication_administrative_user_schema>``CASCADE;`.  
  
## <a name="sql-server-error-21663-is-raised-regarding-the-lack-of-a-primary-key"></a>Une erreur SQL Server 21663 est provoquée par l'absence d'une clé primaire  
 Les articles des publications transactionnelles doivent avoir une clé primaire valide. Si ce n'est pas le cas, vous recevez le message d'erreur suivant si vous essayez d'ajouter un article :  
  
 « Aucune clé primaire valide n’a été trouvée pour la table source [\<*PropriétaireTable*>].[\<*NomTable*>] »  
  
 Pour des informations sur les conditions requises des clés primaires, consultez la section « Contraintes et index uniques » dans la rubrique [Design Considerations and Limitations for Oracle Publishers](../../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md).  
  
## <a name="sql-server-error-21642-is-raised-regarding-a-duplicate-linked-server-login"></a>Une erreur SQL Server 21642 est provoquée par une connexion à un serveur lié en double  
 Quand un serveur de publication Oracle est initialement configuré, une entrée de serveur lié est créée pour la connexion entre le serveur de publication et le serveur de distribution. Le serveur lié a le même nom que le service TNS d'Oracle. Si vous essayez de créer un serveur lié avec le même nom, le message d'erreur suivant apparaît :  
  
 « Les serveurs de publication hétérogènes requièrent un serveur lié. Un serveur lié appelé '*\<NomServeurLié>* existe déjà. Supprimez ce serveur ou choisissez un autre nom de serveur de publication. »  
  
 Cette erreur peut se produire si vous tentez de créer directement un serveur lié ou si vous avez précédemment supprimé la relation entre le serveur de publication Oracle et le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , et que vous tentez de le reconfigurer. Si vous recevez cette erreur en tentant de reconfigurer le serveur de publication, supprimez le serveur lié avec [sp_dropserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md).  
  
 Si vous devez vous connecter au serveur de publication Oracle par l’intermédiaire d’une connexion de serveur lié, créez un autre nom de service TNS puis utilisez ce nom lors de l’appel de [sp_addlinkedserver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Pour des informations sur la création de noms de service TNS, consultez la documentation d'Oracle.  
  
## <a name="sql-server-error-21617-is-raised"></a>Une erreur SQL Server 21617 est signalée  
 La publication Oracle utilise l'application Oracle SQL*PLUS pour télécharger le package de code de prise en charge du serveur de publication dans la base de données Oracle. Avant de tenter de configurer le serveur de publication Oracle, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vérifie que SQL\*PLUS est accessible via le chemin d’accès système sur le serveur de distribution. S’il est impossible de charger SQL\*PLUS, le message d’erreur suivant s’affiche :  
  
 « Impossible d'exécuter SQL*PLUS. Assurez-vous qu'une version actuelle du code client Oracle est installée sur le serveur de distribution. »  
  
 Essayez de trouver SQL\*PLUS sur le serveur de distribution. Pour une installation client Oracle 10g, le nom de cet exécutable est sqlplus.exe. Il doit être installé dans %ORACLE_HOME%/bin. Pour vérifier que le chemin d’accès de SQL\*PLUS apparaît dans le chemin d’accès système, examinez la valeur de la variable système **Path** :  
  
1.  Cliquez avec le bouton droit sur **Poste de travail**, puis cliquez sur **Propriétés**.  
  
2.  Cliquez sur l'onglet **Avancé** , puis sur **Variables d'environnement**.  
  
3.  Dans la boîte de dialogue **Variables d'environnement** , dans la liste **Variables système** , sélectionnez la variable **Path** , puis cliquez sur **Modifier**.  
  
4.  Dans la boîte de dialogue **Modifier la variable système** : si le chemin d'accès au dossier contenant sqlplus.exe ne figure pas dans la zone de texte **Valeur de la variable** , modifiez la chaîne afin de l'inclure.  
  
5.  Cliquez sur **OK** sur chaque boîte de dialogue ouverte pour quitter et enregistrer les modifications.  
  
 Si vous ne trouvez pas sqlplus.exe sur le serveur de distribution, installez-y une version actuelle du logiciel client Oracle. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="sql-server-error-21620-is-raised"></a>Une erreur SQL Server 21620 est signalée  
 Si vous vous connectez à une base de données Oracle antérieure à la version 8.1, la publication Oracle nécessite que le logiciel client Oracle installé sur le serveur de distribution soit de version 9. Si vous vous connectez à une base de données Oracle de version 8.1 ou ultérieure, il est recommandé que le logiciel client Oracle soit de version 10 ou ultérieure.  
  
 Avant de tenter de configurer le serveur de publication Oracle, la publication Oracle vérifie que la version de SQL*PLUS accessible via le chemin d'accès système sur le serveur de distribution est de version 9 ou ultérieure. Si ce n'est pas le cas, l'erreur suivante est signalée :  
  
 « La version de SQL*PLUS qui est accessible via la variable système Path n'est pas assez récente pour prendre en charge la publication dans Oracle. Assurez-vous qu'une version actuelle du code client Oracle est installée sur le serveur de distribution. »  
  
 Si plusieurs versions du logiciel client Oracle sont installées sur le serveur de distribution, assurez-vous que la plus récente soit au moins de version 9 et que la variable système path se réfère d'abord à cette version (des références à d'autres versions sont possibles à condition que la plus récente apparaisse en premier). Pour plus d'informations sur la modification de la variable système path, consultez la section « Une erreur SQL Server 21617 est signalée » plus haut dans cette rubrique.  
  
## <a name="sql-server-error-21624-or-error-21629-is-raised"></a>Une erreur SQL Server 21624 ou 21629 est signalée  
 Pour les serveurs de distribution en 64 bits, la publication Oracle utilise le fournisseur Oracle OLEDB pour Oracle (OraOLEDB.Oracle). Assurez-vous que le fournisseur Oracle OLEDB est installé et enregistré sur le serveur de distribution. Si ce n'est pas le cas, un (ou deux) des messages suivants s'affiche :  
  
-   « Impossible de trouver le fournisseur Oracle OLEDB enregistré, OraOLEDB.Oracle, sur le serveur de distribution '%s'. Assurez-vous qu'une version actuelle du fournisseur Oracle OLEDB est installée et enregistrée sur le serveur de distribution.»  
  
-   « La clé de Registre CLSID indiquant que le fournisseur Oracle OLEDB pour Oracle, OraOLEDB.Oracle, a été enregistré ne se trouve pas sur le serveur de distribution. Assurez-vous que le fournisseur Oracle OLEDB est installé et enregistré sur le serveur de distribution. »  
  
 Si vous utilisez la version 10g du logiciel client Oracle, le fournisseur est OraOLEDB10.dll (OraOLEDB.dll pour la version 9i). Le fournisseur est installé dans %ORACLE_HOME%\BIN (par exemple, C:\oracle\product\10.1.0\Client_1\bin). Si vous constatez que le fournisseur Oracle OLEDB n'est pas installé sur le serveur de distribution, installez-le depuis le CD d'installation du logiciel client Oracle fourni par Oracle. Pour plus d’informations, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
 Si le fournisseur Oracle OLEDB est installé, assurez-vous qu'il est enregistré. Pour enregistrer le fournisseur DLL, exécutez la commande suivante à partir du répertoire dans lequel la DLL est installée, puis arrêtez et redémarrez l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
1.  `regsvr32 OraOLEDB10.dll` ou `regsvr32 OraOLEDB.dll`.  
  
## <a name="sql-server-error-21626-or-error-21627-is-raised"></a>Une erreur SQL Server 21626 ou 21627 est signalée  
 Pour vérifier que l'environnement de la publication Oracle est configuré correctement, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tente de se connecter au serveur de publication Oracle avec les informations d'identification de connexion que vous avez spécifiées au cours de la configuration. Si le serveur de distribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas se connecter au serveur de publication Oracle, le message d'erreur suivant s'affiche :  
  
-   « Impossible de se connecter au serveur de base de données Oracle '%s' au moyen du fournisseur Oracle OLEDB OraOLEDB.Oracle. »  
  
 Si ce message d'erreur s'affiche, vérifiez la connectivité avec la base de données Oracle en exécutant SQL*PLUS directement en employant le nom de connexion et le mot de passe spécifiés lors de la configuration du serveur de publication Oracle. Pour plus d'informations, consultez la section « Le serveur de distribution SQL Server ne peut pas se connecter à l'instance de base de données Oracle », plus haut dans cette rubrique.  
  
## <a name="sql-server-error-21628-is-raised"></a>Une erreur SQL Server 21628 est signalée  
 Pour les serveurs de distribution en 64 bits, la publication Oracle utilise le fournisseur Oracle OLEDB pour Oracle (OraOLEDB.Oracle). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crée une entrée de Registre pour permettre au fournisseur Oracle de s'exécuter en processus avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. En cas de problème de lecture ou d'écriture de cette entrée de Registre, l'erreur suivante est signalée :  
  
 « Impossible de mettre à jour le Registre du serveur de distribution '%s' pour permettre au fournisseur Oracle OLEDB OraOLEDB.Oracle de s'exécuter en processus avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Assurez-vous que la connexion actuelle est autorisée pour modifier les clés de Registre appartenant à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . »  
  
 La publication Oracle exige une entrée de Registre définie à **1** pour les serveurs de distribution 64 bits. Si cette entrée n'existe pas, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tente de la créer. Si elle existe, mais qu'elle est définie à **0**, le paramètre ne sera pas modifié ; la configuration du serveur de publication Oracle échouera.  
  
 Pour afficher et modifier les paramètres du Registre :  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Dans la boîte de dialogue **Exécuter** , tapez **regedit**et cliquez sur **OK**.  
  
3.  Accédez à HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\*\<Nom_instance>* \Providers.  
  
     Sous Providers doit se trouver un répertoire nommé OraOLEDB.Oracle contenant le nom de valeur DWORD **AllowInProcess**, définie à **1**.  
  
4.  Si vous constatez que **AllowInProcess** est définie à **0**, mettez à jour l'entrée du Registre en spécifiant **1**:  
  
    1.  Cliquez avec le bouton droit sur l'entrée, puis cliquez sur **Modifier**.  
  
    2.  Dans la boîte de dialogue **Modification de la chaîne** , tapez **1** dans la zone **Données de la valeur** .  
  
## <a name="sql-server-error-21684-is-raised"></a>Une erreur SQL Server 21684 est signalée  
 Si le compte d'utilisateur administratif ne dispose pas des privilèges suffisants, le message d'erreur suivant s'affiche :  
  
 « Les autorisations associées à la connexion administrateur du serveur de publication Oracle '% s' ne suffisent pas. »  
  
 Pour vérifier les autorisations accordées à l'utilisateur, exécutez la requête suivante : `SELECT * from session_privs`. La sortie doit ressembler à ce qui suit :  
  
 `PRIVILEGE`  
  
 `------------------`  
  
 `CREATE SESSION`  
  
 `CREATE TABLE`  
  
 `CREATE PUBLIC SYNONYM`  
  
 `DROP PUBLIC SYNONYM`  
  
 `CREATE VIEW`  
  
 `CREATE SEQUENCE`  
  
 `CREATE PROCEDURE`  
  
 `CREATE TRIGGER`  
  
## <a name="you-encounter-permissions-issues-for-the-replication-user-schema"></a>Vous rencontrez des problèmes d'autorisations pour le schéma utilisateur de réplication  
 Le schéma utilisateur de réplication doit avoir les autorisations décrites dans la section « Création manuelle du schéma utilisateur » de la rubrique [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="oracle-error-ora-01000"></a>Erreur Oracle ORA-01000  
 La réplication utilise des curseurs sur le serveur de publication Oracle au cours du processus d'ajout d'articles à une publication. Il est possible que le nombre maximal de curseurs disponibles sur le serveur de publication soit dépassé au cours de ce processus. Si cela se produit, l'erreur suivante est signalée :  
  
 « ORA-01000 : Nombre maximum de curseurs ouverts atteint »  
  
 Pour éviter ce problème, vérifiez que le paramètre **max_open_cursors** des bases de données Oracle est défini à un nombre suffisamment élevé (au moins 1 000). Pour plus d'informations sur ce paramètre, consultez la documentation Oracle.  
  
## <a name="oracle-error-ora-01555"></a>Erreur Oracle ORA-01555  
 L'erreur de base de données Oracle suivante n'est pas liée à la réplication d'instantané ; elle est liée à la façon dont Oracle construit des vues de données cohérentes en lecture :  
  
 « ORA-01555 : Instantané trop ancien »  
  
 À l'aide d'objets appelés segments d'annulation, Oracle construit des vues de données cohérentes en lecture à partir du moment où une instruction SQL est émise. Une erreur « instantané trop ancien » peut se produire quand des informations de restauration sont écrasées par d'autres sessions concurrentes. Avant Oracle 9i, la méthode recommandée pour réduire la fréquence de cette erreur était d'augmenter la taille et/ou le nombre de segments d'annulation, et d'attribuer les grosses transactions à un segment d'annulation spécifique.  
  
 Dans Oracle 9i, Oracle a introduit le concept de tablespace UNDO, qui remplace le segment d'annulation. Pour éviter l'erreur « instantané trop ancien » dans Oracle 9i, les actions suivantes sont recommandées :  
  
-   Créer un tablespace UNDO avec une quantité appropriée d'espace libre.  
  
-   Définir la garantie de rétention sur l'espace de table (Oracle 10G et ultérieur).  
  
-   Configurer les paramètres d'initialisation d'Oracle UNDO_MANAGEMENT et UNDO_RETENTION.  
  
 Pour plus d'informations sur la façon d'éviter l'erreur « instantané trop ancien », consultez la documentation d'Oracle.  
  
## <a name="oracle-error-ora-22285"></a>Erreur Oracle ORA-22285  
 Si une table comprend une colonne BFILE, les données pour cette colonne sont stockées dans le système de fichiers. Le compte d'utilisateur d'administration de réplication doit recevoir le droit d'accès au répertoire où les données sont stockées, via la syntaxe suivante :  
  
 `GRANT READ ON DIRECTORY <directory_name> TO <replication_administrative_user_schema>`  
  
 Si l'accès n'est pas accordé, l'erreur suivante est générée par l'Agent de lecture du journal :  
  
 « ORA-22285 : répertoire ou fichier inexistant pour l'opération fileopen »  
  
## <a name="changes-are-made-that-require-reconfiguration-of-the-publisher"></a>Des modifications sont effectuées et nécessitent la reconfiguration du serveur de publication  
 Des modifications apportées aux tables de métadonnées ou aux procédures de réplication nécessitent la suppression et la reconfiguration du serveur de publication. Pour reconfigurer le serveur de publication, vous devez supprimer ce serveur de publication et le configurer à nouveau à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de Transact-SQL ou de RMO. Pour obtenir des informations sur la configuration du serveur de publication, consultez [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
 **Pour supprimer un serveur de publication Oracle (** SQL Server Management Studio **)**  
  
1.  Connectez-vous au serveur de distribution correspondant au serveur de publication Oracle dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur **Réplication**, puis cliquez sur **Propriétés du serveur de distribution**.  
  
3.  Sur la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution** , désactivez la case à cocher pour le serveur de publication Oracle.  
  
4.  Cliquez sur **OK**.  
  
 **Pour supprimer un serveur de publication Oracle (Transact-SQL)**  
  
-   Exécutez **sp_dropdistpublisher**. Pour plus d’informations, consultez [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer un serveur de publication Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Vue d’ensemble de la publication Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)  
  
  
