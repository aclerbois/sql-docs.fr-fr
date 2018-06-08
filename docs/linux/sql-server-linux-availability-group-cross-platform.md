---
title: Configurer SQL Server toujours sur le groupe de disponibilité sur Windows et Linux | Documents Microsoft
description: Configurer le groupe de disponibilité SQL Server avec des réplicas sur Windows et Linux.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 01/31/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: ''
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 9ccd5cb5fdc7d5c5bc6ff62b203bee0bbce6e5e8
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-sql-server-always-on-availability-group-on-windows-and-linux-cross-platform"></a>Configurer SQL Server groupe de disponibilité AlwaysOn sur Windows et Linux (multiplateforme)

[!INCLUDE[tsql-appliesto-sslinux-only](../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Cet article explique les étapes pour créer un toujours sur disponibilité Group (AG) avec un réplica sur un serveur Windows et l’autre réplica sur un serveur Linux. Cette configuration est inter-plateformes, car les réplicas sont sur différents systèmes d’exploitation. Utilisez cette configuration pour la migration à partir d’une plateforme à l’autre ou la récupération d’urgence (DR). Cette configuration ne prend pas en charge la haute disponibilité, car il n’existe aucune solution de cluster pour gérer une configuration inter-plateformes. 

![Hybride None](./media/sql-server-linux-availability-group-overview/image1.png)

Avant de continuer, vous devez être familiarisé avec l’installation et la configuration pour les instances de SQL Server sur Windows et Linux. 

## <a name="scenario"></a>Scénario

Dans ce scénario, deux serveurs se trouvent sur différents systèmes d’exploitation. Un serveur Windows Server 2016 nommé `WinSQLInstance` héberge le réplica principal. Un serveur Linux nommé `LinuxSQLInstance` héberger le réplica secondaire.

## <a name="configure-the-ag"></a>Configurer le groupe de disponibilité 

Les étapes pour créer le groupe de disponibilité sont les mêmes que les étapes pour créer un groupe de disponibilité pour les charges de travail en lecture à l’échelle. Le type de cluster du groupe de disponibilité est NONE, car il n’existe aucun gestionnaire du cluster. 

   >[!NOTE]
   >Pour les scripts dans cet article, les crochets pointus `<` et `>` identifient des valeurs que vous devez remplacer pour votre environnement. Les crochets pointus eux-mêmes ne sont pas requises pour les scripts. 

1. Installer SQL Server 2017 sur Windows Server 2016, activer les groupes de disponibilité AlwaysOn à partir du Gestionnaire de Configuration SQL Server et définir le mode d’authentification mixte. 

   >[!TIP]
   >Si vous validez cette solution dans Azure, placez les deux serveurs dans le même groupe de disponibilité défini pour s’assurer qu’ils sont séparés dans le centre de données. 

   **Activer les groupes de disponibilité**

   Pour obtenir des instructions, consultez [activer et désactiver les groupes de disponibilité AlwaysOn (SQL Server)](../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).

   ![Activer les groupes de disponibilité](./media/sql-server-linux-availability-group-cross-platform/1-sqlserver-configuration-manager.png)

   Gestionnaire de Configuration SQL Server indique que l’ordinateur n’est pas un nœud dans un cluster de basculement. 

   Après avoir activé les groupes de disponibilité, redémarrez SQL Server.

   **Définition de l’authentification en mode mixte**

   Pour obtenir des instructions, consultez [modifier le mode d’authentification serveur](../database-engine/configure-windows/change-server-authentication-mode.md#SSMSProcedure).

1. Installer SQL Server 2017 sur Linux. Pour obtenir des instructions, consultez [installer SQL Server](sql-server-linux-setup.md). Activer `hadr` via mssql-conf.

   Pour activer `hadr` via conf mssql à partir d’une invite de commandes, exécutez la commande suivante :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
   ```

   Après avoir activé `hadr`, redémarrez l’instance de SQL Server.  

   L’illustration suivante montre cette étape terminée.

   ![Activer Linux de groupes de disponibilité](./media/sql-server-linux-availability-group-cross-platform/2-sqlserver-linux-set-hadr.png)

1. Configurer le fichier hosts sur les deux serveurs ou d’inscrire les noms de serveur DNS.

1. Ouvrir des ports de pare-feu pour TPC 1433 et 5022 sur Windows et Linux.

1. Sur le réplica principal, créez une connexion de base de données et le mot de passe.

   ```sql
   CREATE LOGIN dbm_login WITH PASSWORD = '<C0m9L3xP@55w0rd!>';
   CREATE USER dbm_user FOR LOGIN dbm_login;
   GO
   ```

1. Sur le réplica principal, créez une clé principale et le certificat, puis sauvegardez le certificat avec une clé privée.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<C0m9L3xP@55w0rd!>';
   CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';
   BACKUP CERTIFICATE dbm_certificate
   TO FILE = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\dbm_certificate.cer'
   WITH PRIVATE KEY (
           FILE = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\dbm_certificate.pvk',
           ENCRYPTION BY PASSWORD = '<C0m9L3xP@55w0rd!>'
       );
   GO
   ```

1. Copiez le certificat et la clé privée pour le serveur Linux (réplica secondaire) au `/var/opt/mssql/data`. Vous pouvez utiliser `pscp` pour copier les fichiers vers le serveur Linux. 

1. Définir le groupe et le propriétaire de la clé privée et le certificat à `mssql:mssql`.

   Le script suivant définit le groupe et la possession des fichiers. 

   ```bash
   sudo chown mssql:mssql /var/opt/mssql/data/dbm_certificate.pvk
   sudo chown mssql:mssql /var/opt/mssql/data/dbm_certificate.cer
   ```

   Dans le diagramme suivant, la propriété et groupe sont correctement définies pour le certificat et la clé.

   ![Activer Linux de groupes de disponibilité](./media/sql-server-linux-availability-group-cross-platform/3-cert-key-owner-group.png)


1. Sur le réplica secondaire, créez une connexion de base de données et le mot de passe et créer une clé principale.

   ```sql
   CREATE LOGIN dbm_login WITH PASSWORD = '<C0m9L3xP@55w0rd!>';
   CREATE USER dbm_user FOR LOGIN dbm_login;
   GO
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<M@st3rKeyP@55w0rD!>'
   GO
   ```

1. Sur le réplica secondaire, restaurez le certificat que vous avez copié dans `/var/opt/mssql/data`. 

   ```sql
   CREATE CERTIFICATE dbm_certificate   
       AUTHORIZATION dbm_user
       FROM FILE = '/var/opt/mssql/data/dbm_certificate.cer'
       WITH PRIVATE KEY (
       FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
       DECRYPTION BY PASSWORD = '<C0m9L3xP@55w0rd!>'
   )
   GO
   ```

1. Sur le réplica principal, créez un point de terminaison.

   ```sql
   CREATE ENDPOINT [Hadr_endpoint]
       AS TCP (LISTENER_IP = (0.0.0.0), LISTENER_PORT = 5022)
       FOR DATA_MIRRORING (
           ROLE = ALL,
           AUTHENTICATION = CERTIFICATE dbm_certificate,
           ENCRYPTION = REQUIRED ALGORITHM AES
           );
   ALTER ENDPOINT [Hadr_endpoint] STATE = STARTED;
   GRANT CONNECT ON ENDPOINT::[Hadr_endpoint] TO [dbm_login]
   GO
   ```

   >[!IMPORTANT]
   >Le pare-feu doit être ouvert pour le port d’écoute le port TCP. Dans le script précédent, le port est 5022. Utilisez n’importe quel port TCP disponible. 

1. Sur le réplica secondaire, créez le point de terminaison. Répétez le script précédent sur le réplica secondaire pour créer le point de terminaison. 

1. Sur le réplica principal, créez le groupe de disponibilité avec `CLUSTER_TYPE = NONE`. L’exemple de script utilise `SEEDING_MODE = AUTOMATIC` pour créer le groupe de disponibilité. 

   >[!NOTE]
   >Lorsque l’instance de Windows de SQL Server utilise des chemins d’accès pour les fichiers journaux et de données, l’amorçage échoue à l’instance de Linux de SQL Server, car ces chemins d’accès n’existent pas sur le réplica secondaire automatique. Pour utiliser le script suivant pour un groupe de disponibilité inter-plateformes, la base de données requiert le même chemin d’accès pour les données et fichiers journaux sur le serveur Windows. Vous pouvez également mettre à jour le script pour définir `SEEDING_MODE = MANUAL` ensuite sauvegarder et restaurer la base de données avec `NORECOVERY` pour amorcer la base de données. 
   >
   >Ce comportement s’applique aux images Azure Marketplace. 
   >
   >Pour plus d’informations sur l’amorçage automatique, consultez [l’amorçage automatique - disposition du disque](../database-engine/availability-groups/windows/automatic-seeding-secondary-replicas.md#disklayout). 

   Avant d’exécuter le script, mettez à jour les valeurs de vos groupes de disponibilité.

      * Remplacez `<WinSQLInstance>` avec le nom du serveur de l’instance de SQL Server du réplica principal.

      * Remplacez `<LinuxSQLInstance>` avec le nom du serveur de l’instance de SQL Server du réplica secondaire. 

   Pour créer le groupe de disponibilité, mettre à jour les valeurs et exécutez le script sur le réplica principal.  

   ```sql
   CREATE AVAILABILITY GROUP [ag1]
       WITH (CLUSTER_TYPE = NONE)
       FOR REPLICA ON
           N'<WinSQLInstance>' 
        WITH (
           ENDPOINT_URL = N'tcp://<WinSQLInstance>:5022',
           AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            SEEDING_MODE = AUTOMATIC,
            FAILOVER_MODE = MANUAL,
           SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
           N'<LinuxSQLInstance>' 
       WITH (
            ENDPOINT_URL = N'tcp://<LinuxSQLInstance>:5022',
           AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
           SEEDING_MODE = AUTOMATIC,
           FAILOVER_MODE = MANUAL,
           SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
           )
   GO
   ```
   
   Pour plus d’informations, consultez [créer le groupe de disponibilité (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md).

1. Sur le réplica secondaire, joignez le groupe de disponibilité.

   ```sql
   ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE)
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE
   GO
   ```

1. Créer une base de données pour le groupe de disponibilité. L’exemple montre comment utilise une base de données nommée `<TestDB>`. Si vous utilisez l’amorçage automatique, définissez le même chemin d’accès pour les données et les fichiers journaux. 

   Avant d’exécuter le script, mettez à jour les valeurs de votre base de données.

      * Remplacez `<TestDB>` par le nom de votre base de données.

      * Remplacez `<F:\Path>` avec le chemin d’accès pour votre base de données et les fichiers journaux. Utilisez le même chemin d’accès pour les fichiers journaux et de base de données. 

      Vous pouvez également utiliser les chemins d’accès par défaut. 

    Pour créer votre base de données, exécutez le script. 

   ```sql
   CREATE DATABASE [<TestDB>]
      CONTAINMENT = NONE
     ON  PRIMARY ( NAME = N'<TestDB>', FILENAME = N'<F:\Path>\<TestDB>.mdf')
     LOG ON ( NAME = N'<TestDB>_log', FILENAME = N'<F:\Path>\<TestDB>_log.ldf')
   GO
   ```

1. Effectuer une sauvegarde complète de la base de données. 

1. Si vous n’utilisez pas l’amorçage automatique, restaurez la base de données sur le serveur réplica secondaire (Linux). [Migrer une base de données SQL Server à partir de Windows et Linux à l’aide de la sauvegarde et restauration](sql-server-linux-migrate-restore-database.md). Restaurer la base de données `WITH NORECOVERY` sur le réplica secondaire. 

1. Ajouter la base de données pour le groupe de disponibilité. Mettre à jour de l’exemple de script. Remplacez `<TestDB>` par le nom de votre base de données. Sur le réplica principal, exécutez la requête SQL pour ajouter la base de données pour le groupe de disponibilité.

   ```sql
   ALTER AG [ag1] ADD DATABASE <TestDB>
   GO
   ```

1. Vérifiez que la base de données est rempli de mise en route sur le réplica secondaire. 

## <a name="fail-over-the-primary-replica"></a>Basculer le réplica principal

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

Cet article passé en revue les étapes pour créer un groupe de disponibilité inter-plateformes pour prendre en charge les charges de travail de migration ou en lecture à l’échelle. Il peut être utilisé pour la récupération d’urgence manuelle. Il explique également comment basculer le groupe de disponibilité. Un groupe de disponibilité inter-plateformes utilise le type de cluster `NONE` et ne prend pas en charge la haute disponibilité, car il n’existe aucun outil entre-plates-formes de cluster. 

## <a name="next-steps"></a>Étapes suivantes

[Vue d’ensemble des groupes de disponibilité Always On](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)

[Principes fondamentaux de disponibilité de SQL Server pour les déploiements de Linux](sql-server-linux-ha-basics.md)
