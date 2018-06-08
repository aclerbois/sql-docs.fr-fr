---
title: Créer et configurer un groupe de disponibilité pour SQL Server sur Linux | Documents Microsoft
description: Ce didacticiel montre comment créer et configurer des groupes de disponibilité pour SQL Server sur Linux.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 12/11/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 549b34aa918f65da20c3398e1d38491841b5c6fc
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>Créer et configurer un groupe de disponibilité pour SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce didacticiel décrit comment créer et configurer un groupe de disponibilité (AG) pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux. Contrairement aux [!INCLUDE[sssql15-md](../includes/sssql15-md.md)] et précédemment sur Windows, vous pouvez activer groupes de disponibilité avec ou sans créer au préalable le cluster STIMULATEUR sous-jacent. Intégration avec le cluster, si nécessaire, n’est effectuée qu’ultérieurement.

Le didacticiel comprend les tâches suivantes :
 
> [!div class="checklist"]
> * Activer les groupes de disponibilité.
> * Créer des certificats et des points de terminaison groupe de disponibilité.
> * Utilisez [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) ou de Transact-SQL pour créer un groupe de disponibilité.
> * Créer le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] connexion et des autorisations pour STIMULATEUR.
> * Créer des ressources du groupe de disponibilité dans un cluster STIMULATEUR (type externe uniquement).

## <a name="prerequisite"></a>Condition préalable
- Déployer le cluster à haute disponibilité STIMULATEUR comme décrit dans [déployer un cluster STIMULATEUR pour SQL Server sur Linux](sql-server-linux-deploy-pacemaker-cluster.md).


## <a name="enable-the-availability-groups-feature"></a>Activer la fonctionnalité de groupes de disponibilité

Contrairement sur Windows, vous ne pouvez pas utiliser PowerShell ou [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Configuration Manager pour activer la disponibilité regroupe les fonctionnalité (AG). Sous Linux, vous devez utiliser `mssql-conf` pour activer la fonctionnalité. Il existe deux façons d’activer la fonctionnalité de groupes de disponibilité : utiliser le `mssql-conf` utilitaire ou modifier le `mssql.conf` fichier manuellement.

> [!IMPORTANT]
> La fonctionnalité de groupe de disponibilité doit être activée pour les réplicas de configuration uniquement, même sur [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)].

### <a name="use-the-mssql-conf-utility"></a>Utilisez l’utilitaire mssql-conf

À l’invite, exécutez les éléments suivants :

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>Modifiez le fichier mssql.conf

Vous pouvez également modifier le `mssql.conf` fichier, situé sous le `/var/opt/mssql` dossier, ajoutez les lignes suivantes :

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-includessnoversion-mdincludesssnoversion-mdmd"></a>Redémarrage [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Après avoir activé les groupes de disponibilité, comme sur Windows, vous devez redémarrer [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Cela peut se faire par le texte suivant :

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>Créer les points de terminaison groupe de disponibilité et les certificats

Un groupe de disponibilité utilise des points de terminaison TCP pour la communication. Sous Linux, points de terminaison pour un groupe de disponibilité sont uniquement prises en charge si les certificats sont utilisés pour l’authentification. Cela signifie que le certificat à partir d’une instance doit être restauré sur toutes les autres instances seront réplicas participant dans le même groupe de disponibilité. Le processus de certificat est requis même pour un réplica de configuration. 

Création de points de terminaison et la restauration des certificats n’est possible que via Transact-SQL. Vous pouvez utiliser non -[!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]-générée également les certificats. Vous devez également un processus pour gérer et remplacer les certificats qui expirent.

> [!IMPORTANT]
> Si vous envisagez d’utiliser le [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] Assistant pour créer le groupe de disponibilité, vous devez créer et restaurer les certificats à l’aide de Transact-SQL sur Linux.

Pour connaître la syntaxe complète sur les options disponibles pour les différentes commandes (telles que la sécurité supplémentaire), consultez :

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CRÉER LE CERTIFICAT](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> Bien que vous allez créer un groupe de disponibilité, le type de point de terminaison utilise *FOR DATABASE_MIRRORING*, car certains aspects sous-jacente ont été partagés qu’une seule fois avec cette fonctionnalité maintenant déconseillé.

Cet exemple crée des certificats pour une configuration de trois nœuds. Les noms d’instance sont LinAGN1, LinAGN2 et LinAGN3.

1.  Exécutez la commande suivante sur LinAGN1 pour créer la clé principale, le certificat et le point de terminaison, ainsi que le certificat de sauvegarde. Pour cet exemple, le port TCP 5022 par défaut est utilisé pour le point de terminaison.
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  Procédez de même LinAGN2 :
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  Pour finir, effectuez la même séquence sur LinAGN3 :
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  À l’aide de `scp` ou un autre utilitaire, copiez les sauvegardes du certificat sur chaque nœud devant faire partie du groupe de disponibilité.
    
    Pour cet exemple :
    
    - Copiez LinAGN1_Cert.cer LinAGN2 et LinAGN3
    - Copiez LinAGN2_Cert.cer dans LinAGN1 et LinAGN3.
    - Copiez LinAGN3_Cert.cer dans LinAGN1 et LinAGN2.
    
5.  Modifier la propriété et le groupe associé aux fichiers de certificat copié à `mssql`.
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  Créer les connexions de niveau de l’instance et les utilisateurs associés à LinAGN2 et LinAGN3 sur LinAGN1.
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  Restaurer LinAGN2_Cert et LinAGN3_Cert sur LinAGN1. Certificats des autres réplicas est un aspect important de communication de groupe de disponibilité et de sécurité.
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  Créer les connexions de niveau de l’instance et les utilisateurs associés à LinAGN1 et LinAGN3 sur LinAGN2.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10.  Restaurer LinAGN1_Cert et LinAGN3_Cert sur LinAGN2. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
11.  Grant the logins associated with LinAG1 and LinAGN3 permission to connect to the endpoint on LinAGN2.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12.  Créer les connexions de niveau de l’instance et les utilisateurs associés à LinAGN1 et LinAGN2 sur LinAGN3.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13.  Restaurer LinAGN1_Cert et LinAGN2_Cert sur LinAGN3. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
14.  Grant the logins associated with LinAG1 and LinAGN2 permission to connect to the endpoint on LinAGN3.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>Créer le groupe de disponibilité

Cette section explique comment utiliser [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) ou Transact-SQL pour créer le groupe de disponibilité pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

### <a name="use-includessmanstudiofull-mdincludesssmanstudiofull-mdmd"></a>Utilisez [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)].

Cette section montre comment créer un groupe de disponibilité avec un type de cluster d’externes à l’aide de SSMS avec l’Assistant Nouveau groupe de disponibilité.

1.  Dans SSMS, développez **haute disponibilité Always On**, cliquez droit **groupes de disponibilité**, puis sélectionnez **Assistant Nouveau groupe de disponibilité**.

2.  Dans la boîte de dialogue d’Introduction, cliquez sur **suivant**.

3.  Dans la boîte de dialogue spécifier les Options de disponibilité groupe, entrez un nom pour le groupe de disponibilité et sélectionnez un type de cluster externe ou NONE dans la liste déroulante. Externe doit être utilisé lorsque STIMULATEUR sera déployé. None est pour les scénarios spécialisées, telles que de lecture de montée en charge. L’option de détection de l’intégrité au niveau de base de données est facultative. Pour plus d’informations sur cette option, consultez [option de basculement de détection de l’intégrité au niveau disponibilité groupe base de données](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). Cliquez sur **Suivant**.

    ![](./media/sql-server-linux-create-availability-group/image3.png)

4.  Dans la boîte de dialogue Sélectionner les bases de données, sélectionnez les bases de données qui seront incluses dans le groupe de disponibilité. Chaque base de données doit avoir une sauvegarde complète avant de pouvoir l’ajouter à un groupe de disponibilité. Cliquez sur **Suivant**.

5.  Dans la boîte de dialogue spécifier les réplicas, cliquez sur **ajouter un réplica**.

6.  Dans la connexion à la boîte de dialogue serveur, entrez le nom de l’instance Linux [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] qui sera le réplica secondaire et les informations d’identification pour se connecter. Cliquez sur **Se connecter**.

7.  Répétez les deux étapes précédentes pour l’instance qui contient un réplica de configuration ou un autre réplica secondaire.

8.  Les trois instances doivent figurer dans la boîte de dialogue spécifier les réplicas. Si vous utilisez un type de cluster d’externes, pour le réplica secondaire qui sera true secondaire, assurez-vous que le Mode de disponibilité correspond à celle du réplica principal et le mode de basculement est défini en mode externe. Pour le réplica de configuration uniquement, sélectionnez un mode de disponibilité de Configuration uniquement.

    L’exemple suivant montre un groupe de disponibilité avec deux réplicas, un type de cluster d’externe et un réplica de configuration.

    ![](./media/sql-server-linux-create-availability-group/image4.png)

    L’exemple suivant montre un groupe de disponibilité avec deux réplicas, un type de cluster aucune et un réplica de configuration.

    ![](./media/sql-server-linux-create-availability-group/image5.png)

9.  Si vous souhaitez modifier les préférences de sauvegarde, cliquez sur l’onglet Préférences de sauvegarde. Pour plus d’informations sur les préférences de sauvegarde avec les groupes de disponibilité, consultez [configurer la sauvegarde sur les réplicas de disponibilité](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).

10. Si à l’aide de copies secondaires lisibles ou la création d’un groupe de disponibilité avec un cluster de type None pour la lecture à l’échelle, vous pouvez créer un écouteur en sélectionnant l’onglet de l’écouteur. Un écouteur peut également être ajouté plus tard. Pour créer un écouteur, choisissez l’option **créer un écouteur de groupe de disponibilité** et entrez un nom, un port TCP/IP et s’il faut utiliser une adresse IP DHCP statique ou attribuée automatiquement. Souvenez-vous que pour un groupe de disponibilité avec un type de cluster None, l’adresse IP doit être statique et définissez à l’adresse du principal.

    ![](./media/sql-server-linux-create-availability-group/image6.png)

11. Si un écouteur est créé pour les scénarios lisibles, SSMS 17,3 ou version ultérieure permet la création de routage en lecture seule dans l’Assistant. Il peut également être ajoutée plus tard via SSMS ou Transact-SQL. Pour ajouter le routage en lecture seule maintenant :

    A.  Sélectionnez l’onglet routage en lecture seule.

    B.  Entrez l’URL pour les réplicas en lecture seule. Ces URL est semblables aux points de terminaison, à ceci près qu’ils utilisent le port de l’instance, pas le point de terminaison.

    c.  Sélectionnez chaque URL et à partir du bas, sélectionnez les réplicas lisibles. Pour une sélection multiple, maintenez la touche MAJ ou cliquez et faites glisser.

12. Cliquez sur **Suivant**.

13. Choisissez comment l’ou les réplicas secondaire sont initialisé. La valeur par défaut consiste à utiliser [l’amorçage automatique](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md), ce qui nécessite le même chemin d’accès sur tous les serveurs participant dans le groupe de disponibilité. Vous pouvez également laisser l’Assistant faire une copie de sauvegarde et restaurer (la deuxième option) ; il avoir une jointure si vous avez manuellement sauvegardée, copiée et restaurée de la base de données sur l’ou les réplicas (troisième option) ; ou les ajouter ultérieurement la base de données (la dernière option). Comme avec les certificats, si vous êtes manuellement les sauvegardes et les copier, les autorisations sur les fichiers de sauvegarde doit être définie sur l’ou les autres réplicas. Cliquez sur **Suivant**.

14. Si tous les éléments ne sont fourni comme une réussite, recherchez dans la boîte de dialogue Validation. Des avertissements sont acceptables et pas fatale, comme si vous ne créez pas un écouteur. Cliquez sur **Suivant**.

15. Dans la boîte de dialogue Résumé, cliquez sur **Terminer**. Le processus pour créer le groupe de disponibilité commence.

16. Lors de la création du groupe de disponibilité est terminée, cliquez sur **fermer** sur les résultats. Vous pouvez maintenant voir le groupe de disponibilité sur les réplicas dans les vues de gestion dynamique, ainsi que sous le dossier de haute disponibilité Always On dans SSMS.

### <a name="use-transact-sql"></a>Utilisation de Transact-SQL

Cette section présente des exemples de création d’un groupe de disponibilité à l’aide de Transact-SQL. L’écouteur et le routage en lecture seule peuvent être configurées après avoir créé le groupe de disponibilité. Le groupe de disponibilité peut être modifié avec `ALTER AVAILABILITY GROUP`, mais la modification du type de cluster ne peut pas être effectuée [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Si vous n’aviez pas l’intention de créer un groupe de disponibilité avec un type de cluster d’externes, vous devez supprimer et recréer avec un type de cluster None. Vous trouverez plus d’informations et d’autres options aux adresses suivantes :

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)
-   [ALTER AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [Configurer le routage en lecture seule pour un groupe de disponibilité (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [Créer ou configurer un écouteur de groupe de disponibilité (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one--two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>Réplicas exemple 1 : 2 avec un réplica de la configuration uniquement (type de cluster externe)

Cet exemple montre comment créer un groupe de disponibilité de deux réplicas qui utilise un réplica de configuration.

1.  Exécuter sur le nœud qui sera le réplica principal qui contient la copie en lecture/écriture complète des bases de données. Cet exemple utilise l’amorçage automatique.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  Dans une fenêtre de requête connectée à l’autre réplica, exécutez la commande suivante pour joindre le réplica pour le groupe de disponibilité et de lancer le processus d’amorçage à partir du principal au réplica secondaire.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Dans une fenêtre de requête connectée au réplica seule configuration, joignez-le au groupe de disponibilité.
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two--three-replicas-with-read-only-routing-external-cluster-type"></a>Réplicas exemple deux, trois avec (type de cluster externe) de routage en lecture seule

Cet exemple illustre trois complète réplicas et comment en lecture seule de routage peuvent être configurés dans le cadre de la création initiale du groupe de disponibilité.

1.  Exécuter sur le nœud qui sera le réplica principal qui contient la copie en lecture/écriture complète des bases de données. Cet exemple utilise l’amorçage automatique.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    Choses à noter concernant cette configuration :
    
    - *AGName* est le nom du groupe de disponibilité.
    - *DBName* est le nom de la base de données qui est utilisée avec le groupe de disponibilité. Il peut également être une liste de noms séparés par des virgules.
    - *ListenerName* est un nom qui est différent de celle des serveurs/nœuds sous-jacents. Il est enregistré dans DNS avec *IPAddress*.
    - *IPAddress* est une adresse IP qui est associée avec *ListenerName*. Il est également unique et pas le même que les serveurs/nœuds. Applications et les utilisateurs finaux utilisent soit *ListenerName* ou *IPAddress* pour se connecter pour le groupe de disponibilité.
    - *Masque de sous-réseau* est le masque de sous-réseau *IPAddress*; par exemple, 255.255.255.0.

2.  Dans une fenêtre de requête connectée à l’autre réplica, exécutez la commande suivante pour joindre le réplica pour le groupe de disponibilité et de lancer le processus d’amorçage à partir du principal au réplica secondaire.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Répétez l’étape 2 pour le troisième réplica.

#### <a name="example-three--two-replicas-with-read-only-routing-none-cluster-type"></a>Réplicas exemple trois : deux avec le routage en lecture seule (aucun type de cluster)

Cet exemple illustre la création d’une configuration de deux réplicas à l’aide d’un type de cluster None. Il est utilisé pour le scénario de lecture de l’échelle où aucun basculement n’est attendue. Cette opération crée le port d’écoute est réellement le réplica principal, ainsi que le routage en lecture seule, à l’aide de la fonctionnalité de tourniquet (Round Robin).

1.  Exécuter sur le nœud qui sera le réplica principal qui contient la copie en lecture/écriture complète des bases de données. Cet exemple utilise l’amorçage automatique.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    Où
    - *AGName* est le nom du groupe de disponibilité.
    - *DBName* est le nom de la base de données qui est utilisée avec le groupe de disponibilité. Il peut également être une liste de noms séparés par des virgules.
    - *PortOfEndpoint* est le numéro de port utilisé par le point de terminaison créé.
    - *PortOfInstance* est le numéro de port utilisé par l’instance de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].
    - *ListenerName* est un nom qui est différente de celle d’un des réplicas sous-jacent, mais n’est pas réellement utilisé.
    - *PrimaryReplicaIPAddress* est l’adresse IP du réplica principal.
    - *Masque de sous-réseau* est le masque de sous-réseau *IPAddress*. Par exemple, 255.255.255.0.
    
2.  Joignez le réplica secondaire pour le groupe de disponibilité et de lancer l’amorçage automatique.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-includessnoversion-mdincludesssnoversion-mdmd-login-and-permissions-for-pacemaker"></a>Créer le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] connexion et des autorisations pour STIMULATEUR

Un cluster de haute disponibilité de STIMULATEUR sous-jacent [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux a besoin d’accéder à la [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] instance, ainsi que des autorisations sur le groupe de disponibilité lui-même. Les étapes suivantes créent la connexion et les autorisations associées, ainsi que d’un fichier qui indique à STIMULATEUR comment se connecter à [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

1.  Dans une fenêtre de requête connectée vers le premier réplica, exécutez la commande suivante :

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD '<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  Sur le nœud 1, entrez la commande 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    L’éditeur Emacs s’ouvre.
    
3.  Entrez les deux lignes suivantes dans l’éditeur :

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  Maintenez la touche CTRL enfoncée et appuyez sur X, puis C pour quitter et enregistrer le fichier.

5.  Execute 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    Pour verrouiller le fichier.

6.  Répétez les étapes 1 à 5 sur les autres serveurs qui serviront de réplicas.

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>Créer des ressources du groupe de la disponibilité du cluster STIMULATEUR (externes uniquement)

Après une disponibilité groupe est créé dans [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], les ressources correspondantes doivent être créés dans STIMULATEUR, lorsqu’un type de cluster d’externes est spécifié. Il existe deux ressources associées à un groupe de disponibilité : le groupe de disponibilité et une adresse IP. Configuration de la ressource d’adresse IP est facultative si vous n’utilisez pas la fonctionnalité de l’écouteur, mais il êtes recommandé.

La ressource de groupe de disponibilité est créée est un type spécial de ressource appelée un clone. La ressource de groupe de disponibilité possède essentiellement des copies sur chaque nœud et il existe une ressource de contrôle appelée principal. Le maître est associé avec le serveur qui héberge le réplica principal. Les réplicas secondaires (standard ou configuration uniquement) sont considérés comme des instances subordonnées et peut être promues en maître dans un basculement.

1.  Créer la ressource de groupe de disponibilité avec la syntaxe suivante :

    **Red Hat Enterprise Linux (RHEL) et Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> --master meta notify=true
    ```

    >[!NOTE]
    >Sur RHEL 7.4, vous pouvez rencontrer un avertissement à l’aide de--principale. Pour éviter cela, utilisez `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> master notify=true`
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    où *NameForAGResource* est le nom unique donné à cette ressource de cluster pour le groupe de disponibilité, et *AGName* est le nom du groupe de disponibilité qui a été créé.
 
2.  Créer la ressource d’adresse IP pour le groupe de disponibilité qui sera associé à la fonctionnalité de l’écouteur.

    **RHEL et Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    où *NameForIPResource* est le nom unique pour la ressource IP, et *IPAddress* est l’adresse IP affectée à la ressource. SLES, vous devez également fournir le masque de réseau. Par exemple, 255.255.255.0 avoir la valeur 24 pour *masque de réseau.*
    
3.  Pour vous assurer que l’adresse IP et la ressource de groupe de disponibilité sont en cours d’exécution sur le même nœud, une contrainte de colocalisation doit être configurée.

    **RHEL et Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    où *NameForIPResource* est le nom de la ressource IP, *NameForAGResource* est le nom de la ressource de groupe de disponibilité et SLES, *NameForConstraint* est le nom de la contrainte.

4.  Créer un contrainte pour s’assurer que la ressource de groupe de disponibilité est disponible et en cours d’exécution avant l’adresse IP. Alors que la contrainte de colocalisation implique une contrainte de classement, cela l’applique.

    **RHEL et Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    où *NameForIPResource* est le nom de la ressource IP, *NameForAGResource* est le nom de la ressource de groupe de disponibilité et SLES, *NameForConstraint* est le nom de la contrainte.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à créer et configurer un groupe de disponibilité pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux. Vous avez appris à : 
> [!div class="checklist"]
> * Activer les groupes de disponibilité.
> * Points de terminaison de créer le groupe de disponibilité et les certificats.
> * Utilisez [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) ou de Transact-SQL pour créer un groupe de disponibilité.
> * Créer le [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] connexion et des autorisations pour STIMULATEUR.
> * Créer des ressources du groupe de disponibilité dans un cluster STIMULATEUR.

Pour la plupart des tâches d’administration AG, y compris les mises à niveau et le basculement, consultez :

> [!div class="nextstepaction"]
> [Utiliser le groupe de disponibilité haute disponibilité pour SQL Server sur Linux](sql-server-linux-availability-group-failover-ha.md)

