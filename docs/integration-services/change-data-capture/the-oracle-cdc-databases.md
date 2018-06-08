---
title: Bases de données Oracle CDC | Microsoft Docs
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
ms.assetid: a96486e9-f79b-4b24-bfaf-56203dd0e435
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bf36c117d636579d0f2048b67cd903eca224cc3d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-oracle-cdc-databases"></a>Bases de données de capture de données modifiées Oracle
  Une instance Oracle CDC est associée à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le même nom sur l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible. Cette base de données est appelée base de données de capture de données modifiées Oracle (ou base de données CDC).  
  
 La base de données CDC est créée et configurée à l'aide de la console du concepteur de capture de données modifiées Oracle et contient les éléments suivants :  
  
-   Un schéma `cdc` créé en activant la base de données pour SQL Server CDC.  
  
-   Un ensemble de tables **cdc.xdbcdc_xxxx** utilisées par l’instance Oracle CDC.  
  
-   Un ensemble de tables miroir vides avec les définitions des tables capturées dans la base de données Oracle source.  
  
-   Un ensemble de tables de modifications et de fonctions de modification d'accès qui sont générées par le mécanisme SQL Server CDC et sont identiques à celles utilisées dans SQL Server CDC standard qui n'est pas fourni par Oracle.  
  
 Le schéma `cdc` n’est initialement accessible qu’aux membres du rôle de base de données fixe **dbowner** . L'accès aux tables de modifications et aux fonctions de modification est déterminé par le même modèle de sécurité que celui de SQL Server CDC. Pour plus d’informations sur le modèle de sécurité, consultez [Modèle de sécurité](http://go.microsoft.com/fwlink/?LinkId=231151).  
  
## <a name="creating-the-cdc-database"></a>Création de la base de données CDC  
 Dans la plupart des cas, la base de données CDC est créée à l'aide de la console du concepteur CDC, mais elle peut également être créée à l'aide d'un script de déploiement de capture de données modifiées qui est généré à l'aide de la console du concepteur CDC. L'administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut modifier les paramètres de la base de données si nécessaire (pour des éléments tels que le stockage, la sécurité ou la disponibilité).  
  
 Pour plus d’informations sur l’utilisation de la console du concepteur CDC pour créer des tables de base de données et les scripts nécessaires, consultez [Utiliser l’Assistant Nouvelle instance](../../integration-services/change-data-capture/use-the-new-instance-wizard.md).  
  
## <a name="cdc-database-user-roles"></a>Rôles de l'utilisateur de base de données CDC  
 Quand une base de données CDC est créée et activée pour la capture de données modifiées, un utilisateur de base de données appelé **cdc_service** est créé dans la base de données CDC et est associé à la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec laquelle le service de capture de données modifiées Oracle a été configuré. Cet utilisateur est membre des rôles de base de données **db_datareader**, **db_datawriter**et **db_ddladmin** . Si la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est également associée à l’utilisateur `dbo` , l’utilisateur **cdc_service** n’est pas créé.  
  
 Cette attribution de rôle permet au service de capture de données modifiées Oracle de mettre à jour les tables dans le schéma `cdc` avec des données capturées et des informations de contrôle.  
  
 Lorsqu'une base de données CDC est créée et des tables Oracle de source CDC sont installées, le propriétaire de la base de données CDC peut accorder l'autorisation SELECT des tables miroir et définir des rôles de régulation SQL Server CDC pour contrôler l'accès aux données modifiées.  
  
## <a name="mirror-tables"></a>Tables miroir  
 Pour chaque table capturée, \<schema-name>.\<table-name>, dans la base de données source Oracle, une table vide similaire est créée dans la base de données CDC, avec les mêmes noms de schéma et de table. Les tables sources Oracle avec le nom de schéma `cdc` (ne respectant pas la casse) ne peuvent pas être capturées, car le schéma `cdc` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est réservé pour SQL Server CDC.  
  
 Les tables miroir sont vides ; aucune donnée n'est stockée dans ces dernières. Elles servent à activer l'infrastructure standard SQL Server CDC utilisée par l'instance Oracle CDC. Pour empêcher l'insertion ou la mise à jour des données dans les tables miroir, toutes les opérations UPDATE, DELETE et INSERT sont refusées pour le rôle PUBLIC. Cela garantit qu'elles ne peuvent pas être modifiées.  
  
## <a name="access-to-change-data"></a>Accès aux données modifiées  
 En raison du modèle de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour accéder aux données modifiées associées à une instance de capture, l'utilisateur doit disposer de l'accès `select` à toutes les colonnes capturées de la table miroir associée (les autorisations d'accès aux tables d'origine Oracle ne permettent pas d'accéder aux tables de modifications dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Pour plus d’informations sur le modèle de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Modèle de sécurité](http://go.microsoft.com/fwlink/?LinkId=231151).  
  
 De plus, si un rôle de régulation est spécifié lors de la création de l'instance de capture, l'appelant doit également être membre du rôle de régulation spécifié. Les autres fonctions de capture de données modifiées générales pour accéder aux métadonnées sont accessibles à tous les utilisateurs de base de données par le biais du rôle PUBLIC, bien que l'accès aux métadonnées retournées soit en général également régulé par le biais de l'accès choisi aux tables sources sous-jacentes et par l'appartenance aux rôles de régulation définis.  
  
 Les données modifiées peuvent être lues en appelant les fonctions table spéciales générées par le composant SQL Server CDC lorsqu'une instance de capture est créée. Pour plus d’informations sur cette fonction, consultez [Fonctions de capture de données modifiées (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=231152).  
  
 L'accès aux données CDC via le composant source [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] CDC est soumis aux mêmes règles.  
  
## <a name="the-cdc-database-tables"></a>Tables de base de données CDC  
 Cette section décrit les tables suivantes dans la base de données CDC.  
  
-   [Tables de modifications (_CT)](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_Change_Tables_CT)  
  
-   [cdc.lsn_time_mapping](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdclsn_time_mapping)  
  
-   [cdc.xdbcdc_config](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_config)  
  
-   [cdc.xdbcdc_state](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_state)  
  
-   [cdc.xdbcdc_trace](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_trace)  
  
-   [cdc.xdbcdc_staged_transactions](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_staged_transactions)  
  
###  <a name="BKMK_Change_Tables_CT"></a> Tables de modifications (_CT)  
 Les tables de modifications sont créées à partir des tables miroir. Elles contiennent les données modifiées qui sont capturées dans la base de données Oracle. Les tables sont nommées en fonction de la convention suivante :  
  
 **[cdc]. [\<instance_de_capture >_CT]**  
  
 Lorsque la capture est initialement activée pour la table `<schema-name>.<table-name>`, le nom par défaut de l'instance de capture est `<schema-name>_<table-name>`. Par exemple, le nom par défaut de l'instance de capture pour la table Oracle HR.EMPLOYEES est HR_EMPLOYEES et la table de modifications associée est [cdc]. [HR_EMPLOYEES_CT].  
  
 Les tables de capture sont écrites par l'instance Oracle CDC. Elles sont lues à l'aide de fonctions table spéciales générées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque l'instance de capture est créée. Par exemple, `fn_cdc_get_all_changes_HR_EMPLOYEES`. Pour plus d’informations sur ces fonctions CDC, consultez [Fonctions de capture de données modifiées (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=231152).  
  
###  <a name="BKMK_cdclsn_time_mapping"></a> cdc.lsn_time_mapping  
 La table **[cdc].[lsn_time_mapping]** est générée par le composant SQL Server CDC. Son utilisation dans le cas de capture de données modifiées Oracle est différente de son utilisation normale.  
  
 Pour la capture de données modifiées Oracle, les valeurs LSN stockées dans cette table reposent sur la valeur SCN (System Change Number) Oracle associée à la modification. Les 6 premiers octets de la valeur LSN constituent le numéro SCN Oracle d'origine.  
  
 En outre, lors de l'utilisation d'Oracle CDC, les colonnes de temps (`tran_begin_time` et `tran_end_time`) stockent l'heure UTC de la modification plutôt que l'heure locale, comme avec SQL Server CDC standard. Cela garantit que les modifications de l'heure d'été n'affectent pas les données stockées dans lsn_time_mapping.  
  
###  <a name="BKMK_cdcxdbcdc_config"></a> cdc.xdbcdc_config  
 Cette table contient les données de configuration de l'instance Oracle CDC. Elle est mise à jour à l'aide de la console du concepteur CDC. Cette table a une seule ligne.  
  
 Le tableau suivant décrit les colonnes de la table **cdc.xdbcdc_config** .  
  
|Élément|Description|  
|----------|-----------------|  
|version|Effectue le suivi de la version de la configuration de l'instance CDC. Elle est mise à jour chaque fois que la table est mise à jour et chaque fois qu'une nouvelle instance de capture est ajoutée ou qu'une instance de capture existante est supprimée.|  
|connect_string|Chaîne de connexion Oracle. Voici un exemple de base :<br /><br /> `<server>:<port>/<instance>` (par exemple, `erp.contoso.com:1521/orcl`).<br /><br /> La chaîne de connexion peut également spécifier un descripteur de connexion Oracle Net, par exemple, `(DESCRIPTION=(ADDRESS=(PROTOCOL=tcp) (HOST=erp.contoso.com) (PORT=1521)) (CONNECT_DATA=(SERVICE_NAME=orcl)))`.<br /><br /> Si vous utilisez un serveur d'annuaire ou des noms TNS, la chaîne de connexion peut être le nom de la connexion.<br /><br /> Pour plus d’informations sur les chaînes de connexion Oracle, consultez [http://go.microsoft.com/fwlink/?LinkId=231153](http://go.microsoft.com/fwlink/?LinkId=231153) pour obtenir des informations détaillées sur les chaînes de connexion de base de données Oracle pour le client Oracle Instant qui est utilisée par le service de capture des changements de données Oracle.|  
|use_windows_authentication|Valeur booléenne qui peut être :<br /><br /> **0**: un nom d’utilisateur et un mot de passe Oracle sont fournis pour l’authentification (valeur par défaut) ;<br /><br /> **1**: l’authentification Windows est utilisée pour la connexion à la base de données Oracle. Vous ne pouvez utiliser cette option que si la base de données Oracle est configurée pour utiliser l'authentification Windows.|  
|username|Nom de l'utilisateur de la base de données Oracle d'exploration de données de journaux. Requis uniquement si **use_windows_authentication = 0**.|  
|password|Mot de passe de l'utilisateur de la base de données Oracle d'exploration de données de journaux. Requis uniquement si **use_windows_authentication = 0**.|  
|transaction_staging_timeout|Durée, en secondes, pendant laquelle une transaction Oracle non enregistrée est gardée en mémoire avant d’être écrite dans la table **cdc.xdbcdc_staged_transactions** . La valeur par défaut est 120 secondes.|  
|memory_limit|Quantité de mémoire limite, en Mo, qui permet de mettre en cache des données en mémoire. Un paramètre inférieur provoque l’écriture de davantage de transactions dans la table **cdc.xdbcdc_staged_transactions** . La valeur par défaut est 50 Mo.|  
|options|Liste des options au format name[=value][; ] - elle sert à spécifier des options secondaires (par exemple, traçage, paramétrage). Consultez le tableau ci-dessous pour obtenir une description des options disponibles.|  
  
 Le tableau suivant décrit les options disponibles.  
  
|Nom   |Valeur par défaut|Min|Max|Statique|Description|  
|----------|-------------|---------|---------|------------|-----------------|  
|trace|False|-|-|False|Les valeurs disponibles sont :<br /><br /> True<br /><br /> False<br /><br /> actif<br /><br /> inactif|  
|cdc_update_state_interval|10| 1|120|False|Taille (en kilo-octets) des segments de mémoire alloués pour une transaction (une transaction peut allouer plusieurs segments). Consultez la colonne memory_limit dans la table [cdc.xdbcdc_config](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_config) .|  
|target_max_batched_transactions|100| 1|1000|True|Nombre maximal de transactions Oracle qui peuvent être traitées comme une transaction avec mise à jour de tables SQL Server CT.|  
|target_idle_lsn_update_interval|10|0| 1|False|Intervalle (en secondes) de mise à jour de la table **lsn_time_mapping** quand les tables capturées n’ont aucune activité.|  
|trace_retention_period|24| 1|24*31|False|Durée (en heures pour conserver les messages dans la table de trace).|  
|sql_reconnect_interval|2|2|3600|False|Délai (en secondes) qui doit s'écouler avant la reconnexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cet intervalle est utilisé en plus du délai d'attente de connexion du client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|sql_reconnect_limit|-1|-1|-1|False|Nombre maximal de reconnexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La valeur par défaut -1 signifie que le processus tente de se reconnecter jusqu'à ce qu'il s'arrête.|  
|cdc_restart_limit|6|-1|3600|False|Dans la plupart des cas, le service de capture de données modifiées redémarre automatiquement une instance CDC qui s'est terminée de façon anormale. Cette propriété définit après combien d'échecs par heure le service cesse de redémarrer l'instance. La valeur -1 signifie que l'instance doit toujours être redémarrée.<br /><br /> Le service retourne pour redémarrer l'instance après toute mise à jour de la table de configuration.|  
|cdc_memory_report|0|0|1000|False|Si la valeur du paramètre a été modifiée, l'instance CDC imprime son rapport mémoire sur la table de trace.|  
|target_command_timeout|600| 1|3600|False|Délai d'attente de commande utilisé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|source_character_set|-|-|-|True|Peut être défini sur un encodage Oracle spécifique pour être utilisé à la place de la page de codes de la base de données Oracle. Cela peut être utile lorsque l'encodage réel que les données caractères utilisent est différent de celui exprimé par la page de codes de la base de données Oracle.|  
|source_error_retry_interval|30| 1|3600|False|Utilisé avant reprise en cas de plusieurs erreurs, telles qu'une erreur de connexion ou un échec temporaire de synchronisation entre les tables système.|  
|source_prefetch_size|100| 1|10000|True|Taille du lot de prérécupération.|  
|source_max_tables_in_query|100| 1|10000|True|Nombre maximal de tables dans la clause WHERE avant le basculement vers la lecture du journal Oracle sans filtrage de table.|  
|source_read_retry_interval|2| 1|3600|False|Durée pendant laquelle la source attend avant d'essayer de lire les journaux des transactions Oracle sur EOF.|  
|source_reconnect_interval|30| 1|3600|False|Durée d'attente (en secondes) avant une nouvelle tentative de connexion à la base de données source.|  
|source_reconnect_limit|-1|-1||False|Nombre maximal de reconnexions à la base de données source. La valeur par défaut -1 signifie que le processus tente de se reconnecter jusqu’à ce qu’il soit arrêté.|  
|source_command_timeout|30| 1|3600|False|Délai de connexion utilisé avec Oracle.|  
|source_connection_timeout|30| 1|3600|False|Délai de connexion utilisé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|trace_data_errors|True|-|-|False|Propriété booléenne. **True** indique l’enregistrement des erreurs de conversion de données et de troncation.|  
|CDC_stop_on_breaking_schema_changes|False|-|-|False|Propriété booléenne. **True** indique l’arrêt quand une modification avec rupture du schéma est détectée.<br /><br /> **False** indique la suppression de la table miroir et de l’instance de capture.|  
|source_oracle_home||-|-|False|Peut être défini sur un chemin racine Oracle spécifique ou un nom de dossier racine Oracle que l'instance de capture de données modifiées utilisera pour se connecter à Oracle.|  
  
###  <a name="BKMK_cdcxdbcdc_state"></a> cdc.xdbcdc_state  
 Cette table contient des informations sur l'état persistant de l'instance Oracle CDC. L'état de capture est utilisé dans des scénarios de récupération et de basculement et à des fins de contrôle d'intégrité.  
  
 Le tableau suivant décrit les colonnes de la table **cdc.xdbcdc_state** .  
  
|Élément|Description|  
|----------|-----------------|  
|status|Code d'état actuel de l'instance Oracle CDC active. Décrit l'état actuel de l'instance CDC.|  
|sub_status|État de deuxième niveau qui fournit des informations supplémentaires sur l'état actuel.|  
|active|Valeur booléenne qui peut être :<br /><br /> **0**: le processus d’instance Oracle CDC n’est pas actif.<br /><br /> **1**: le processus d’instance Oracle CDC est actif.|  
|erreur|Valeur booléenne qui peut être :<br /><br /> **0**: le processus d’instance Oracle CDC n’est pas dans un état d’erreur.<br /><br /> **1**: l’instance Oracle CDC est dans un état d’erreur.|  
|status_message|Chaîne qui fournit une description de l'erreur ou de l'état.|  
|TIMESTAMP|Horodateur avec l'heure (UTC) à laquelle l'état de capture a été mis à jour pour la dernière fois.|  
|active_capture_node|Nom de l'hôte (l'hôte peut être un nœud dans un cluster) qui exécute actuellement le service de capture de données modifiées Oracle et l'instance Oracle CDC (qui traite les journaux des transactions Oracle).|  
|last_transaction_timestamp|Horodateur avec l'heure (UTC) à laquelle la dernière transaction a été écrite dans les tables de modifications.|  
|last_change_timestamp|Horodateur avec l'heure (UTC) à laquelle l'enregistrement de modification le plus récent a été lu dans le journal des transactions Oracle source. Cet horodateur aide à identifier la latence actuelle du processus de capture de données modifiées.|  
|transaction_log_head_cn|Numéro de modification (CN) le plus récent lu dans le journal des transactions Oracle.|  
|transaction_log_tail_cn|Numéro de modification (CN) dans le journal des transactions Oracle où l'instance Oracle CDC se repositionne en cas de redémarrage ou de récupération.|  
|current_cn|Numéro de modification (CN) le plus récent (CN) connu dans la base de données source.|  
|software_version|Version interne du service de capture de données modifiées Oracle.|  
|completed_transactions|Nombre de transactions traitées depuis la dernière réinitialisation CDC.|  
|written_changes|Nombre d'enregistrements de modification écrits dans les tables de modifications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|read_changes|Nombre d'enregistrements de modification lus dans le journal des transactions Oracle source.|  
|staged_transactions|Nombre de transactions actives intermédiaires dans la table **cdc.xdbcdc_staged_transactions** .|  
  
###  <a name="BKMK_cdcxdbcdc_trace"></a> cdc.xdbcdc_trace  
 Cette table contient des informations sur le fonctionnement de l'instance de capture de données modifiées. Les informations stockées dans cette table incluent les enregistrements d'erreur, les modifications notables d'état et les enregistrements de trace. Les informations d’erreur sont également écrites dans le journal des événements Windows pour garantir que les informations sont disponibles si la table **cdc.xcbcdc_trace** n’est pas disponible.  
  
 Le tableau suivant décrit les colonnes de la table cdc.xdbcdc_trace :  
  
|Élément|Description|  
|----------|-----------------|  
|TIMESTAMP|Horodateur UTC exact de l'enregistrement de trace.|  
|type|Contient l'une des valeurs suivantes :<br /><br /> erreur<br /><br /> INFO<br /><br /> trace|  
|node|Nom du nœud sur lequel l'enregistrement a été écrit.|  
|status|Code d'état utilisé par la table d'état.|  
|sub_status|Code de sous-état utilisé par la table d'état.|  
|status_message|Message d'état utilisé par la table d'état.|  
|données|Informations supplémentaires pour les cas où l'erreur ou l'enregistrement de trace contient une charge utile (par exemple, un enregistrement de journal endommagé).|  
  
###  <a name="BKMK_cdcxdbcdc_staged_transactions"></a> cdc.xdbcdc_staged_transactions  
 Cette table stocke les enregistrements de modification des transactions de grande taille ou longues jusqu'à ce que l'événement de validation ou de restauration de transaction soit capturé. Le service de capture de données modifiées Oracle classe les enregistrements de journal capturés par heure de validation de transaction, puis par ordre chronologique pour chaque transaction. Les enregistrements de journaux pour la même transaction sont stockés en mémoire jusqu'à ce que la transaction se termine, puis sont écrits dans la table de modifications cible ou ignorés (en cas de restauration). Comme il existe une quantité limitée de mémoire disponible, les grandes transactions sont écrites dans la table **cdc.xdbcdc_staged_transactions** jusqu’à ce que la transaction soit terminée. Les transactions sont également écrites dans la table de mise en lots lorsqu'elles s'exécutent longtemps. Par conséquent, lorsque l'instance Oracle CDC est redémarrée, les anciennes modifications n'ont pas besoin d'être relues dans les journaux des transactions Oracle.  
  
 Le tableau suivant décrit les colonnes de la table **cdc.xdbcdc_staged_transactions** .  
  
|Élément|Description|  
|----------|-----------------|  
|transaction_id|Identificateur unique de la transaction intermédiaire.|  
|seq_num|Numéro de la ligne **xcbcdc_staged_transactions** pour la transaction active (commence par 0).|  
|data_start_cn|Numéro de modification (CN) pour la première modification des données de cette ligne.|  
|data_end_cn|Numéro de modification (CN) pour la dernière modification des données de cette ligne.|  
|données|Modifications intermédiaires pour la transaction sous forme d'un objet BLOB.|  
  
## <a name="see-also"></a> Voir aussi  
 [Concepteur de capture de données modifiées pour Oracle par Attunity](../../integration-services/change-data-capture/change-data-capture-designer-for-oracle-by-attunity.md)  
  
  
