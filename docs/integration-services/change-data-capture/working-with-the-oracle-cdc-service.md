---
title: Utilisation d’Oracle CDC Service| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 04be5896-2301-45f5-a8ce-5f4ef2b69aa5
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c62c28da819aa4293258784648b1af88a333e156
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-the-oracle-cdc-service"></a>Utilisation du service de capture de données modifiées Oracle
  Cette section décrit des concepts importants du service de capture de données modifiées Oracle. Les concepts inclus dans cette section sont les suivants :  
  
-   [Base de données MSXDBCDC](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_MSXDBCDC)  
  
     Cette section décrit les tables incluses dans cette base de données et leur importance pour la capture de données modifiées.  
  
-   [Bases de données CDC](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_CDCdatabase)  
  
     Cette section fournit une brève description des bases de données CDC. Ces bases de données sont créées à l'aide de la console du concepteur de capture de données modifiées Oracle. Pour plus d'informations sur les bases de données CDC, consultez la documentation fournie avec votre installation de la console du concepteur CDC.  
  
-   [Utilisation de la ligne de commande pour configurer le service de capture de données modifiées](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_CommandConfigCDC)  
  
     Cette section décrit les commandes de ligne de commande qui peuvent être utilisées pour configurer le service de capture de données modifiées Oracle.  
  
##  <a name="BKMK_MSXDBCDC"></a> Base de données MSXDBCDC  
 La base de données MSXDBCDC (Microsoft External Database CDC) est une base de données spéciale qui est requise lors de l'utilisation du service de capture de données modifiées pour Oracle avec une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le nom de cette base de données ne peut pas être modifié. Si une base de données appelée MSXDBCDC existe sur l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hôte et contient des tables autres que celles définies par le service de capture de données modifiées pour Oracle, l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hôte ne peut pas être utilisée.  
  
 Les utilisations principales de cette base de données sont les suivantes :  
  
-   Sert de Registre des services de capture de données modifiées Oracle associés à une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces informations sont utilisées pour les composants de configuration du service et de création et pour prendre en charge la coordination de plusieurs services de capture de données modifiées par le même nom sur des nœuds différents sur lesquels l'un d'entre eux est actif.  
  
-   Sert de Registre des instances Oracle CDC contenues dans une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le service de capture de données modifiées qui gère chaque instance, et la version de configuration utilisée par chacune d'elles. Ces informations sont équivalentes à la colonne **is_cdc_enabled** dans la table **sys.databases** de la base de données MASTER. Le service de capture de données modifiées analyse régulièrement la table **dbo.xdbcdc_databases** pour identifier les modifications apportées à la configuration de capture de données modifiées ou à la liste d’instances capturées.  
  
-   Garde les procédures stockées détenues par **sysadmin**qui permettent de créer et de maintenir les instances de capture de données modifiées. Elles sont semblables aux procédures système utilisées pour l'implémentation de la fonctionnalité de capture de données modifiées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="creating-the-msxdbcdc-database"></a>Création de la base de données MSXDBCDC  
 Une base de données MSXDBCDC doit être créée avant de définir le service de capture de données modifiées Oracle. Vous ne pouvez créer qu'une seule base de données MSXDBCDC sur une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La base de données MSXDBCDC est créée lorsque vous préparez une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la capture de données modifiées Oracle. Cela peut être accompli en utilisant la console de configuration du service de capture de données modifiées Oracle ou en exécutant un script de création qui est généré par la console de configuration du service de capture de données modifiées.  
  
 Le propriétaire de cette base de données est l'administrateur de service de capture de données modifiées Oracle, qui peut contrôler toutes les instances Oracle CDC hébergées dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Voir aussi :**  
  
 [Guide pratique pour préparer SQL Server pour CDC](../../integration-services/change-data-capture/how-to-prepare-sql-server-for-cdc.md)  
  
### <a name="the-msxdbcdc-database-tables"></a>Tables de la base de données MSXDBCDC  
 Cette section décrit les tables suivantes dans la base de données MSXDBCDC.  
  
-   [dbo.xdbcdc_trace](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxdbcdc_trace)  
  
-   [dbo.xdbcdc_databases](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxdbcdc_databases)  
  
-   [dbo.xdbcdc_services](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxdbcdc_services)  
  
###  <a name="BKMK_dboxdbcdc_trace"></a> dbo.xdbcdc_trace  
 Cette table stocke les informations de trace pour le service de capture de données modifiées Oracle. Les informations stockées dans cette table incluent les modifications notables d'état et les enregistrements de trace.  
  
 Le service de capture de données modifiées Oracle écrit des enregistrements d'erreur et certains enregistrements des informations dans le journal des événements Windows et dans la table de trace. Dans certains cas, la table de trace n'est pas accessible, auquel cas les informations d'erreur sont accessibles à partir du journal des événements.  
  
 La section suivante décrit les éléments inclus dans la table **dbo.xdbcdc_trace** .  
  
|Élément|Description|  
|----------|-----------------|  
|TIMESTAMP|Horodateur UTC exact de l'enregistrement de trace.|  
|type|Contient l'une des valeurs suivantes :<br /><br /> erreur<br /><br /> INFO<br /><br /> trace|  
|node|Nom du nœud sur lequel l'enregistrement a été écrit.|  
|status|Code d'état utilisé par la table d'état.|  
|sub_status|Code se sous-état utilisé par la table d'état.|  
|status_message|Message d'état utilisé par la table d'état.|  
|source|Nom du composant de capture de données modifiées Oracle qui a généré l'enregistrement de trace.|  
|text_data|Données texte supplémentaires pour les cas où l'erreur ou l'enregistrement de trace contient une charge utile textuelle.|  
|binary_data|Données binaires supplémentaires pour les cas où l'erreur ou l'enregistrement de trace contient une charge utile binaire.|  
  
 L'instance Oracle CDC supprime les anciennes lignes de table de trace en fonction de la stratégie de rétention de tables de modifications.  
  
###  <a name="BKMK_dboxdbcdc_databases"></a> dbo.xdbcdc_databases  
 Cette table contient les noms de services de capture de données modifiées pour les bases de données de capture de données modifiées Oracle dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] active. Chaque base de données correspond à une instance Oracle CDC. Le service de capture de données modifiées Oracle utilise cette table pour déterminer quelles instances démarrer ou arrêter et quelles instances reconfigurer.  
  
 Le tableau suivant décrit les éléments inclus dans la table **dbo.xdbcdc_databases** .  
  
|Élément|Description|  
|----------|-----------------|  
|NAME|Nom de la base de données Oracle dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|config_version|Horodateur (UTC) pour la dernière modification de la table **xdbcdc_config** de base de données CDC correspondante ou horodateur (UTC) pour la ligne actuelle dans cette table.<br /><br /> Le déclencheur UPDATE applique la valeur GETUTCDATE() pour cet élément. **config_version** permet au service de capture de données modifiées d’identifier l’instance de capture de données modifiées qui doit être examinée pour la modification de configuration ou pour l’activation/désactivation.|  
|cdc_service_name|Cet élément détermine le service de capture de données modifiées Oracle qui gère la base de données Oracle sélectionnée.|  
|activé|Indique si l'instance Oracle CDC est activée (1) ou désactivée (0). Lorsque le service de capture de données modifiées Oracle démarre, seules les instances marquées comme étant activées (1) sont démarrées.<br /><br /> **Remarque**: Une instance Oracle CDC peut être désactivée en raison d’une erreur qui n’est pas renouvelable. Dans ce cas, l'instance doit être redémarrée manuellement une fois l'erreur corrigée.|  
  
###  <a name="BKMK_dboxdbcdc_services"></a> dbo.xdbcdc_services  
 Ce tableau répertorie les services de capture de données modifiées associés à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hôte. Cette table est utilisée par la console du concepteur CDC pour déterminer la liste des services de capture de données modifiées configurés pour l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale. Elle est également utilisée par le service de capture de données modifiées pour garantir qu'un seul service Windows en cours de exécution gère un nom donné de service de capture de données modifiées Oracle.  
  
 La section suivante décrit les éléments d’état de capture qui sont inclus dans la table **dbo.xdbcdc_databases** .  
  
|Élément|Description|  
|----------|-----------------|  
|cdc_service_name|Nom du service de capture de données modifiées Oracle (nom de service Windows).|  
|cdc_service_sql_login|Nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée par le service de capture de données modifiées Oracle pour se connecter à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un nouvel utilisateur SQL nommé cdc_service est créé et associé à ce nom de connexion et est ensuite ajouté en tant que membre des rôles de base de données fixes db_ddladmin, db_datareader et db_datawriter pour chaque base de données CDC gérée par le service.|  
|ref_count|Cet élément compte le nombre d'ordinateurs où le même service de capture de données modifiées Oracle est installé. Il est augmenté à chaque ajout de service de capture de données modifiées Oracle portant le même nom, et il est réduit lorsque ce service est supprimé. Lorsque le compteur atteint zéro, cette ligne est supprimée.|  
|active_service_node|Nom du nœud Windows qui gère actuellement le service de capture de données modifiées. Lorsque le service est arrêté correctement, cette colonne a la valeur Null, ce qui indique qu'il ne s'agit plus d'un service actif.|  
|active_service_heartbeat|Cet élément suit le service de capture de données modifiées actuel pour déterminer s'il est encore actif.<br /><br /> Cet élément est mis à jour avec l'horodateur UTC de la base de données active du service de capture de données modifiées actif à intervalles réguliers. L'intervalle par défaut est de 30 secondes ; cependant, vous pouvez le configurer.<br /><br /> Lorsqu'un service de capture de données modifiées en attente détecte que la pulsation n'a pas été mise à jour après que l'intervalle configuré est passé, le service en attente tente d'assumer le rôle de service de capture de données modifiées actif.|  
|options|Cet élément spécifie les options secondaires, telles que suivi ou paramétrage. Il est enregistré au format **name[=value][; ]**. La chaîne d'options utilise la même sémantique que la chaîne de connexion ODBC. Si l'option est booléenne (avec une valeur oui/non), la valeur peut inclure le nom uniquement.<br /><br /> Les valeurs possibles de trace sont les suivantes :<br /><br /> **true**<br /><br /> **actif**<br /><br /> **false**<br /><br /> **inactif**<br /><br /> **\<nom_classe>[,nom_classe>]**<br /><br /> <br /><br /> La valeur par défaut est **false**.<br /><br /> **service_heartbeat_interval** est l’intervalle de temps (en secondes) pour que le service mette à jour la colonne active_service_heartbeat. La valeur par défaut est **30**. La valeur maximale est **3600**.<br /><br /> **service_config_polling_interval** est la fréquence d’interrogation (en secondes) pour que le service de capture de données modifiées vérifie les modifications de configuration. La valeur par défaut est **30**. La valeur maximale est **3600**.<br /><br /> **sql_command_timeout** est le délai d’attente de commande qui fonctionne avec le serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur par défaut est **1**. La valeur maximale est **3600**.|  
||  
  
### <a name="the-msxdbcdc-database-stored-procedures"></a>Procédures stockées de base de données MSXDBCDC  
 Cette section décrit les procédures stockées suivantes dans la base de données MSXDBCDC.  
  
-   [dbo.xcbcdc_reset_db(Database Name)](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxcbcdc_reset_db)  
  
-   [dbo.xdbcdc_disable_db(dbname)](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxdbcdc_disable_db)  
  
-   [dbo.xcbcdc_add_service (svcname,sqlusr)](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxcbcdc_add_service)  
  
-   [dbo.xdbcdc_start(dbname)](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxdbcdc_start)  
  
-   [dbo.xdbcdc_stop(dbname)](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_dboxdbcdc_stop)  
  
###  <a name="BKMK_dboxcbcdc_reset_db"></a> dbo.xcbcdc_reset_db(Database Name)  
 Cette procédure efface les données d'une instance Oracle CDC. Mode d'utilisation  
  
-   Pour redémarrer la capture de données en ignorant les anciennes données, par exemple après récupération de la base de données source ou après inactivité lorsque les journaux des transactions Oracle ne sont pas disponibles.  
  
-   Lorsqu'il existe une altération de l'état de capture de données modifiées (surtout dans les données cdc.*tables).  
  
 La procédure **dbo.xcbcdc_reset_db** effectue les tâches suivantes :  
  
-   Elle arrête l’instance de capture de données modifiées (si elle est active).  
  
-   Elle tronque les tables de modifications, la table **cdc_lsn_mapping** et la table **cdc_ddl_history** .  
  
-   Elle supprime la table **cdc_xdbcdc_state** .  
  
-   Elle supprime la colonne start_lsn pour chaque ligne de **cdc_change_table**.  
  
 Pour utiliser la procédure **dbo.xcbcdc_reset_db** , l’utilisateur doit être membre du rôle de base de données **db_owner** pour la base de données d’instance de capture de données modifiées nommée ou membre du rôle serveur fixe **sysadmin** ou **serveradmin** .  
  
 Pour plus d’informations sur les tables de capture de données modifiées, consultez *Bases de données CDC* dans le système d’aide de la console du concepteur CDC.  
  
###  <a name="BKMK_dboxdbcdc_disable_db"></a> dbo.xdbcdc_disable_db(dbname)  
 La procédure **dbo.xcbcdc_disable_db** effectue la tâche suivante :  
  
-   Elle supprime l’entrée pour la base de données CDC sélectionnée dans la table MSXDBCDC.xdbcdc_databases.  
  
 Pour utiliser la procédure **dbo.xcbcdc_disable_db** , l’utilisateur doit être membre du rôle de base de données **db_owner** pour l’instance de capture de données modifiées nommée ou membre du rôle serveur fixe **sysadmin** ou **serveradmin** .  
  
 Pour plus d'informations sur les tables de capture de données modifiées, consultez Bases de données CDC dans le système d'aide de la console du concepteur CDC.  
  
###  <a name="BKMK_dboxcbcdc_add_service"></a> dbo.xcbcdc_add_service (svcname,sqlusr)  
 La procédure **dbo.xcbcdc_add_service** ajoute une entrée à la table **MSXDBCDC.xdbcdc_services** et ajoute un incrément de un à la colonne ref_count pour le nom du service dans la table **MSXDBCDC.xdbcdc_services** . Quand **ref_count** a la valeur 0, elle supprime la ligne.  
  
 Pour utiliser la procédure **dbo.xcbcdc_add_service\<nom_service, nom_utilisateur>**, l’utilisateur doit être membre du rôle de base de données **db_owner** pour la base de données d’instanceCDC nommée ou membre du rôle serveur fixe **sysadmin** ou **serveradmin**.  
  
###  <a name="BKMK_dboxdbcdc_start"></a> dbo.xdbcdc_start(dbname)  
 La procédure **dbo.xdbcdc_start** envoie une demande de démarrage au service de capture de données modifiées qui gère l’instance de capture de données modifiées sélectionnée pour démarrer le traitement des modifications.  
  
 Pour utiliser la procédure **dbo.xcdcdc_start** , l’utilisateur doit être membre du rôle de base de données **db_owner** pour la base de données CDC ou être membre des rôles **sysadmin** ou **serveradmin** pour l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
###  <a name="BKMK_dboxdbcdc_stop"></a> dbo.xdbcdc_stop(dbname)  
 La procédure **dbo.xdbcdc_stop** envoie une demande d’arrêt au service de capture de données modifiées qui gère l’instance de capture de données modifiées sélectionnée pour arrêter le traitement des modifications.  
  
 Pour utiliser la procédure **dbo.xcdcdc_stop** , l’utilisateur doit être membre du rôle de base de données **db_owner** pour la base de données CDC ou être membre des rôles **sysadmin** ou **serveradmin** pour l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="BKMK_CDCdatabase"></a> Bases de données CDC  
 Chaque instance Oracle CDC utilisée dans un service de capture de données modifiées est associée à une base de données spécifique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelée base de données CDC. Cette base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est hébergée dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associée au service de capture de données modifiées Oracle.  
  
 La base de données CDC contient un schéma cdc spécial. Le service de capture de données modifiées Oracle utilise ce schéma avec des noms de tables ayant le préfixe **xdbcdc_**. Ce schéma est utilisé à des fins de sécurité et de cohérence.  
  
 L'instance Oracle CDC et les bases de données CDC sont créées à l'aide de la console du concepteur de capture de données modifiées Oracle. Pour plus d'informations sur les bases de données CDC, consultez la documentation fournie avec votre installation de la console du concepteur CDC Oracle.  
  
##  <a name="BKMK_CommandConfigCDC"></a> Utilisation de la ligne de commande pour configurer le service de capture de données modifiées  
 Vous pouvez exécuter le programme Service de capture de données modifiées Oracle (xdbcdcsvc.exe) à partir de la ligne de commande. Ce programme est un fichier exécutable 32 bits et 64 bits natif Windows.  
  
 **Voir aussi**  
  
 [Guide pratique pour utiliser l’interface de ligne de commande du service CDC](../../integration-services/change-data-capture/how-to-use-the-cdc-service-command-line-interface.md)  
  
### <a name="service-program-commands"></a>Commandes du programme de service  
 Cette section décrit les commandes suivantes utilisées pour configurer le service CDC.  
  
-   [Config](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_config)  
  
-   [Créer](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_create)  
  
-   [Supprimer](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_delete)  
  
###  <a name="BKMK_config"></a> Config  
 Utilisez `Config` pour mettre à jour une configuration de service de capture de données modifiées Oracle à partir d'un script. La commande peut être utilisée pour mettre à jour uniquement les parties spécifiques de la configuration du service de capture de données modifiées (par exemple, uniquement la chaîne de connexion sans connaître le mot de passe de la clé asymétrique). La commande doit être exécutée par un administrateur de l'ordinateur. Voici un exemple de commande `Config` .  
  
```  
"<path>xdbcdcsvc.exe" config  
     <cdc-service-name>  
     [connect= <sql-server-connection-string>]  
     [key= <asym-key-password>]  
     [svcacct= <windows-account> <windows-password>]  
     [sqlacct= <sql-username> <sql-password>]  
  
```  
  
 Où :  
  
 **cdc-service-name** est le nom du service de capture de données modifiées (CDC) à mettre à jour. Il s’agit d’un paramètre obligatoire.  
  
 **sql-server-connection-string** est la chaîne de connexion à mettre à jour. Si la chaîne de connexion contient des espaces ou des guillemets, elle doit être incluse entre des guillemets doubles ("). Les guillemets incorporés sont placés dans une séquence d'échappement en doublant les guillemets.  
  
 **asym-key-password** est le mot de passe à mettre à jour.  
  
 **windows-account**, **windows-password** sont des informations d’identification du compte Windows pour le service mis à jour.  
  
 **sql-username**, **sql-password** sont les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mises à jour. Si sqlacct a un nom d'utilisateur vide et un mot de passe vide, le service de capture de données modifiées Oracle se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant l'authentification Windows.  
  
 **Remarque**: Un paramètre qui contient des espaces ou des guillemets doubles doit être inclus entre guillemets doubles ("). Les guillemets doubles incorporés doivent être doublés (par exemple, pour utiliser **"A#B" D** comme mot de passe, entrez **""A#B"" D"**).  
  
###  <a name="BKMK_create"></a> Créer  
 Utilisez `Create` pour créer un service de capture de données modifiées Oracle à partir d'un script. La commande doit être exécutée par un administrateur de l'ordinateur. Voici un exemple de commande `Create` :  
  
```  
"<path>xdbcdcsvc.exe" create  
     <cdc-service-name>  
     [connect= "<sql-server-connection-string>"]  
     [key= <asym-key-password>]  
     [svcacct <windows-account> <windows-password>]  
     [sqlacct <sql-username> <sql-password>]  
```  
  
 Où :  
  
 **cdc-service-name** est le nom du service nouvellement créé. S'il existe déjà un service portant ce nom, le programme retourne une erreur. Vous ne devez pas utiliser des noms longs ou des noms comportant des espaces. Les caractères "/" et "\\" ne sont pas valides dans un nom de service. Il s’agit d’un paramètre obligatoire.  
  
 **sql-server-connection-string** est la chaîne de connexion à utiliser pour se connecter à l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associée au nouveau service de capture de données modifiées Oracle.  
  
 **asym-key-password** est le mot de passe qui protège la clé asymétrique utilisée pour stocker les informations d’identification pour l’exploration des données de journaux de la base de données source.  
  
 **windows-account**, **windows-password** sont le nom et le mot de passe du compte associés au service de capture de données modifiées Oracle créé.  
  
 **sql-username**, **sql-password** sont le nom et le mot de passe du compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisés pour se connecter à l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si ces deux paramètres sont vides, le service de capture de données modifiées pour Oracle se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant l'authentification Windows.  
  
 **Remarque**: Un paramètre qui contient des espaces ou des guillemets doubles doit être inclus entre guillemets doubles ("). Les guillemets doubles incorporés doivent être doublés (par exemple, pour utiliser **"A#B" D** comme mot de passe, entrez **""A#B"" D"**).  
  
###  <a name="BKMK_delete"></a> Supprimer  
 Utilisez `Delete` pour supprimer correctement le service de capture de données modifiées Oracle à partir d'un script. Cette commande doit être exécutée par un administrateur de l'ordinateur. Voici un exemple de commande `Delete` .  
  
```  
"<path>xdbcdcsvc.exe" delete  
    <cdc-service-name>  
  
```  
  
 Où :  
  
 **cdc-service-name** est le nom du service de capture de données modifiées (CDC) à supprimer.  
  
 **Remarque**: Un paramètre qui contient des espaces ou des guillemets doubles doit être inclus entre guillemets doubles ("). Les guillemets doubles incorporés doivent être doublés (par exemple, pour utiliser **"A#B" D** comme mot de passe, entrez **""A#B"" D"**).  
  
## <a name="see-also"></a> Voir aussi  
 [Procédure : utiliser l'interface de ligne de commande du service de capture de données modifiées](../../integration-services/change-data-capture/how-to-use-the-cdc-service-command-line-interface.md)   
 [Guide pratique pour préparer SQL Server pour CDC](../../integration-services/change-data-capture/how-to-prepare-sql-server-for-cdc.md)  
  
  
