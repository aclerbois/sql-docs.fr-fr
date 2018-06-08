---
title: Chiffrement TDE (Transparent Data Encryption) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption
- database encryption key, about
- TDE
- database encryption key
- TDE, about
- Transparent Data Encryption, about
- encryption [SQL Server], transparent data encryption
ms.assetid: c75d0d4b-4008-4e71-9a9d-cee2a566bd3b
caps.latest.revision: 75
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 270c13eeb22078db8ae2ca1b65fd049d969f174a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transparent-data-encryption-tde"></a>Chiffrement transparent des données (TDE)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/bb934049(SQL.120).aspx).

  Le*chiffrement transparent des données* (TDE) chiffre les fichiers de données de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]et [!INCLUDE[ssSDWfull](../../../includes/sssdwfull-md.md)] (processus appelé chiffrement des données au repos). Vous pouvez prendre plusieurs précautions pour mieux sécuriser la base de données comme par exemple concevoir un système sécurisé, chiffrer les ressources confidentielles et créer un pare-feu autour des serveurs de base de données. Toutefois, dans un scénario où le support physique (tel que des lecteurs ou des bandes de sauvegarde) est dérobé, une personne malveillante peut simplement restaurer ou attacher la base de données et parcourir les données. Une solution consiste à chiffrer les données sensibles dans la base de données et à protéger les clés utilisées pour chiffrer les données avec un certificat. Cela empêche toute personne qui ne dispose pas des clés d'utiliser les données, mais ce type de protection doit être planifié à l'avance.  
  
 Le chiffrement transparent des données effectue le chiffrement et le déchiffrement d'E/S en temps réel des données et des fichiers journaux. Le chiffrement utilise une clé de chiffrement de base de données (DEK), stockée dans l'enregistrement de démarrage de base de données pour être disponible pendant la récupération. La clé de chiffrement de base de données est une clé symétrique sécurisée à l'aide d'un certificat stocké dans la base de données MASTER du serveur ou une clé asymétrique protégée par un module EKM. Le chiffrement transparent des données protège les données « au repos », autrement dit les fichiers de données et les fichiers journaux. Il permet de se conformer à de nombreuses lois, règles et instructions établies dans différents secteurs professionnels. Cela permet aux développeurs de logiciels de chiffrer des données à l'aide des algorithmes de chiffrement AES et 3DES sans modifier les applications existantes.  
  
> [!IMPORTANT]  
>  Le chiffrement transparent des données ne permet pas le chiffrement via des canaux de communication. Pour plus d’informations sur le chiffrement des données sur les canaux de communication, consultez [Activer les connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
>   
>  **Rubriques connexes :**  
>   
>  -   [Chiffrement transparent des données avec Azure SQL Database](../../../relational-databases/security/encryption/transparent-data-encryption-azure-sql.md)  
> -   [Prise en main du chiffrement transparent des données (TDE) sur SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-encryption-tde-tsql/)  
> -   [Déplacer une base de données protégée par le chiffrement transparent des données vers un autre serveur SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
> -   [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)   
> -   [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)
> - [Blog sur la sécurité SQL Server : chiffrement transparent des données (avec FAQ)](https://blogs.msdn.microsoft.com/sqlsecurity/2016/10/05/feature-spotlight-transparent-data-encryption-tde/)
 
  
## <a name="about-tde"></a>À propos du chiffrement transparent des données  
 Le chiffrement du fichier de base de données est effectué au niveau de la page. Les pages d'une base de données chiffrée sont chiffrées avant d'être écrites sur le disque et déchiffrées lorsqu'elles sont lues en mémoire. Le chiffrement transparent des données n'augmente pas la taille de la base de données chiffrée.  
  
 **Informations applicables à [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]**  
  
 Quand vous utilisez le chiffrement transparent des données avec [!INCLUDE[sqldbesa](../../../includes/sqldbesa-md.md)] V12, le certificat de niveau serveur stocké dans la base de données master est automatiquement créé par [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. Pour déplacer une base de données avec chiffrement transparent des données sur [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] , vous devez déchiffrer la base de données, la déplacer, puis réactiver le chiffrement transparent des données sur la [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]de destination. Pour obtenir des instructions pas à pas relatives au chiffrement transparent des données dans [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], consultez [Chiffrement transparent des données avec Azure SQL Database](../../../relational-databases/security/encryption/transparent-data-encryption-azure-sql.md).  
  
 **Informations applicables à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**  
  
 Une fois sécurisée, la base de données peut être restaurée à l'aide du certificat approprié. Pour plus d'informations sur les certificats, consultez [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
 Lorsque vous activez le chiffrement transparent des données, vous devez immédiatement sauvegarder le certificat et la clé privée associée au certificat. Dans l'éventualité où le certificat ne serait plus disponible ou que vous deviez restaurer ou attacher la base de données sur un autre serveur, vous devez disposer de sauvegardes du certificat et de la clé privée, sans quoi vous ne pourrez pas ouvrir la base de données. Le certificat de chiffrement doit être conservé même si le chiffrement transparent des données n'est plus activé sur la base de données. Même si la base de données n'est pas chiffrée, des parties du journal des transactions peuvent toujours être protégées, et le certificat peut être nécessaire pour certaines opérations tant que la sauvegarde complète de la base de données n'a pas été effectuée. Un certificat qui a dépassé sa date d'expiration peut toujours être utilisé pour chiffrer et déchiffrer les données avec le chiffrement transparent des données.  
  
 **Hiérarchie de chiffrement**  
  
 L'illustration ci-dessous montre l'architecture du chiffrement TDE. Seuls les éléments de niveau base de données (la clé de chiffrement de base de données et les parties ALTER DATABASE) sont configurables par l'utilisateur lors de l'utilisation du chiffrement transparent des données sur la [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
 ![Affiche la hiérarchie décrite dans cette rubrique.] (../../../relational-databases/security/encryption/media/tde-architecture.gif "Affiche la hiérarchie décrite dans cette rubrique.")  
  
## <a name="using-transparent-data-encryption"></a>Utilisation du chiffrement transparent des données  
 Pour utiliser le chiffrement transparent des données, procédez comme suit :  
  
**S'applique à**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Créez une clé principale.  
  
-   Créez ou obtenez un certificat protégé par la clé principale.  
  
-   Créez une clé de chiffrement de base de données et protégez-la à l'aide du certificat.  
  
-   Configurez la base de données pour qu'elle utilise le chiffrement.  
  
 L'exemple ci-dessous illustre le chiffrement et le déchiffrement de la base de données `AdventureWorks2012` à l'aide d'un certificat installé sur le serveur nommé `MyServerCert`.  
  
```sql  
USE master;  
GO  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';  
go  
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';  
go  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_128  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
ALTER DATABASE AdventureWorks2012  
SET ENCRYPTION ON;  
GO  
```  
  
 Les opérations de chiffrement et de déchiffrement sont planifiées sur des threads d'arrière-plan par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Vous pouvez consulter l'état de ces opérations à l'aide des affichages catalogue et des vues de gestion dynamique mentionnés dans la liste fournie plus loin dans cette rubrique.  
  
> [!CAUTION]  
>  Les fichiers de sauvegarde des bases de données pour lesquelles le chiffrement transparent des données est activé sont également chiffrés à l'aide de la clé de chiffrement de base de données. En conséquence, lorsque vous restaurez ces sauvegardes, le certificat qui protège la clé de chiffrement de base de données doit être disponible. Cela signifie qu'en plus de sauvegarder la base de données, vous devez vous assurer que vous conservez des sauvegardes des certificats du serveur pour empêcher toute perte de données. Une perte de données interviendra si le certificat n'est plus disponible. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
## <a name="commands-and-functions"></a>Commandes et fonctions  
 Les certificats TDE doivent être chiffrés par la clé principale de base de données pour être acceptés par les instructions suivantes. S'ils sont chiffrés uniquement par mot de passe, les instructions les refuseront en tant que chiffreurs.  
  
> [!IMPORTANT]  
>  Si des certificats ayant été utilisés par le chiffrement transparent des données sont modifiés de sorte qu'ils soient protégés par mot de passe, la base de données ne sera plus disponible après un redémarrage.  
  
 Le tableau suivant fournit des liens et des explications pour les commandes et les fonctions TDE.  
  
|Commande ou fonction|Fonction|  
|-------------------------|-------------|  
|[CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)|Crée une clé permettant de chiffrer une base de données.|  
|[ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-encryption-key-transact-sql.md)|Modifie la clé qui permet de chiffrer une base de données.|  
|[DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-database-encryption-key-transact-sql.md)|Supprime la clé qui était utilisée pour chiffrer une base de données.|  
|[ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql-set-options.md)|Présente l'option **ALTER DATABASE** qui est utilisée pour activer le chiffrement transparent des données.|  
  
## <a name="catalog-views-and-dynamic-management-views"></a>Affichages catalogue et vues de gestion dynamique  
 Le tableau suivant indique les affichages catalogue et les vues de gestion dynamique du chiffrement transparent des données.  
  
|Affichage catalogue ou vue de gestion dynamique|Fonction|  
|---------------------------------------------|-------------|  
|[sys.databases &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Affichage catalogue qui affiche des informations sur la base de données.|  
|[sys.certificates &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|Affichage catalogue qui indique les certificats inclus dans une base de données.|  
|[sys.dm_database_encryption_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)|Vue de gestion dynamique qui fournit des informations sur les clés de chiffrement utilisées dans une base de données et sur l'état de chiffrement d'une base de données.|  
  
## <a name="permissions"></a>Autorisations  
 Chaque fonctionnalité et commande TDE requiert des autorisations individuelles, décrites dans les tableaux précédents.  
  
 La consultation des métadonnées impliquées dans le chiffrement transparent des données requiert l'autorisation VIEW DEFINITION sur le certificat.  
  
## <a name="considerations"></a>Observations  
 Lorsqu'une analyse de rechiffrement est en cours pour une opération de chiffrement de la base de données, les opérations de maintenance sur la base de données sont désactivées. Vous pouvez utiliser le paramètre de mode utilisateur unique de la base de données pour effectuer l'opération de maintenance. Pour plus d’informations, consultez [Définir une base de données en mode mono-utilisateur](../../../relational-databases/databases/set-a-database-to-single-user-mode.md).  
  
 Vous pouvez déterminer l'état de chiffrement de la base de données en utilisant la vue de gestion dynamique sys.dm_database_encryption_keys. Pour plus d’informations, consultez la section « Affichages catalogue et vues de gestion dynamique » plus haut dans cette rubrique.  
  
 Dans le chiffrement transparent des données, tous les fichiers et groupes de fichiers de la base de données sont chiffrés. Si des groupes de fichiers de la base de données sont marqués comme READ ONLY, l'opération de chiffrement de la base de données échoue.  
  
 Si une base de données est utilisée dans la mise en miroir de bases de données ou la copie des journaux de transaction, les deux bases de données seront chiffrées. Les transactions du journal sont chiffrées lorsqu'elles sont envoyées dans l'intervalle.  
  
> [!IMPORTANT]  
>  Les index de recherche en texte intégral sont chiffrés dès lors qu’une base de données est définie pour le chiffrement. Les index de recherche en texte intégral créés avant SQL Server 2008 sont importés dans la base de données durant la mise à niveau vers SQL Server 2008 ou version supérieure pour ensuite être chiffrés par le chiffrement transparent des données.  

> [!TIP]  
> Pour surveiller les changements de l’état de TDE d’une base de données, utilisez SQL Server Audit ou l’audit SQL Database. Pour SQL Server, le chiffrement TDE est suivi sous le groupe d’actions d’audit DATABASE_CHANGE_GROUP qui se trouve dans [Actions et groupes d’actions SQL Server Audit ](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).
  
### <a name="restrictions"></a>Restrictions  
 Les opérations suivantes ne sont pas autorisées au cours du chiffrement initial de la base de données, de la modification d'une clé ou du déchiffrement de la base de données :  
  
-   Suppression d'un fichier d'un groupe de fichiers dans la base de données  
  
-   Suppression de la base de données  
  
-   Mise hors connexion de la base de données  
  
-   Détachement d'une base de données  
  
-   Passage d'une base de données ou d'un groupe de fichiers à l'état READ ONLY  
  
 Les opérations suivantes ne sont pas autorisées durant les instructions CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY ou ALTER DATABASE...SET ENCRYPTION :  
  
-   Suppression d'un fichier d'un groupe de fichiers dans la base de données  
  
-   Suppression de la base de données  
  
-   Mise hors connexion de la base de données  
  
-   Détachement d'une base de données  
  
-   Passage d'une base de données ou d'un groupe de fichiers à l'état READ ONLY  
  
-   Utilisation d'une commande ALTER DATABASE  
  
-   Démarrage d'une base de données ou d'une sauvegarde de fichiers de base de données  
  
-   Démarrage d'une base de données ou d'une restauration de fichiers de base de données  
  
-   Création d'un instantané  
  
 Les opérations ou conditions suivantes empêchent l'exécution des instructions CREATE DATABASE ENCRYPTION KEY, ALTER DATABASE ENCRYPTION KEY, DROP DATABASE ENCRYPTION KEY ou ALTER DATABASE...SET ENCRYPTION :  
  
-   La base de données est en lecture seule ou a des groupes de fichiers en lecture seule.  
  
-   Une commande ALTER DATABASE est en cours d'exécution.  
  
-   Une sauvegarde de données est en cours d'exécution.  
  
-   La base de données est dans une condition hors connexion ou de restauration.  
  
-   Un instantané est en cours.  
  
-   Tâches de maintenance de base de données.  
  
 Lors de la création de fichiers de base de données, l'initialisation instantanée des fichiers n'est pas disponible lorsque le chiffrement transparent des données est activé.  
  
 Afin de chiffrer la clé de chiffrement de base de données avec une clé asymétrique, la clé asymétrique doit résider sur un fournisseur de gestion de clés extensible.  
  
### <a name="transparent-data-encryption-and-transaction-logs"></a>Chiffrement transparent des données et journaux de transactions  
 L'activation du chiffrement transparent des données sur une base de données a pour effet de « réinitialiser » la partie restante du journal des transactions virtuel pour imposer le journal des transactions virtuel suivant. Cela garantit qu'aucun texte en clair n'est conservé dans les journaux des transactions après que la base de données a été définie pour le chiffrement. Vous pouvez rechercher l'état du chiffrement des fichiers journaux en consultant la colonne `encryption_state` dans la vue `sys.dm_database_encryption_keys`, comme dans l'exemple suivant :  
  
```  
USE AdventureWorks2012;  
GO  
/* The value 3 represents an encrypted state   
   on the database and transaction logs. */  
SELECT *  
FROM sys.dm_database_encryption_keys  
WHERE encryption_state = 3;  
GO  
```  
  
 Pour plus d’informations sur l’architecture des fichiers journaux [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Journal des transactions &#40;SQL Server&#41;](../../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Toutes les données écrites dans le journal des transactions avant une modification de la clé de chiffrement de base de données seront chiffrées à l'aide de la clé de chiffrement de base de données précédente.  
  
 Lorsque la clé de chiffrement d'une base de données a été modifiée deux fois, une sauvegarde de fichier journal doit être effectuée pour rendre possible une nouvelle modification de la clé de chiffrement.  
  
### <a name="transparent-data-encryption-and-the-tempdb-system-database"></a>Chiffrement transparent des données et base de données système tempdb  
 La base de données système tempdb est chiffrée si toute autre base de données sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est chiffrée à l'aide du chiffrement transparent des données. Cela peut avoir un impact sur les performances des bases de données non chiffrées situées sur la même instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la base de données système tempdb, consultez [Base de données tempdb](../../../relational-databases/databases/tempdb-database.md).  
  
### <a name="transparent-data-encryption-and-replication"></a>Chiffrement transparent des données et réplication  
 La réplication ne réplique pas automatiquement sous forme chiffrée les données d'une base sur laquelle le chiffrement transparent des données est activé. Vous devez activer séparément le chiffrement transparent des données si vous souhaitez protéger les bases de données de distribution et d'abonné. La réplication d'instantané, ainsi que la distribution initiale de données pour la réplication transactionnelle et la réplication de fusion, peut stocker des données dans des fichiers intermédiaires non chiffrés, par exemple, des fichiers bcp.  Au cours de la réplication transactionnelle ou de la réplication de fusion, le chiffrement peut être activé pour protéger le canal de communication. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
### <a name="transparent-data-encryption-and-filestream-data"></a>Chiffrement transparent des données et données FILESTREAM  
 Les données FILESTREAM ne sont pas chiffrées même si le chiffrement transparent des données est activé.  
  
## <a name="transparent-data-encryption-and-buffer-pool-extension"></a>Chiffrement transparent des données et Extension du Pool de mémoires tampons  
 Les fichiers associés à l'extension du pool de mémoires tampons ne sont pas chiffrés lorsque la base de données est chiffrée à l'aide du chiffrement transparent des données. Vous devez utiliser les outils de chiffrement au niveau du système de fichiers tels que Bitlocker ou EFS pour les fichiers associés à l'extension du pool de mémoires tampons.  
  
## <a name="transparent-data-encryption-and-in-memory-oltp"></a>Chiffrement transparent des données et OLTP en mémoire  
 Le chiffrement transparent des données (TDE) peut être activé sur une base de données contenant des objets de l'OLTP en mémoire. Dans [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] et [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] , les enregistrements de journal et les données de l’OLTP en mémoire sont chiffrés si le chiffrement transparent des données (TDE) est activé. Dans [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] , les enregistrements de journal de l’OLTP en mémoire sont chiffrés si le chiffrement transparent des données (TDE) est activé, mais les fichiers dans le groupe de fichiers MEMORY_OPTIMIZED_DATA ne sont pas chiffrés.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Déplacer une base de données protégée par le chiffrement transparent des données vers un autre serveur SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)  
 [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
## <a name="related-content"></a>Contenu associé  
 [Chiffrement transparent des données avec Azure SQL Database](../../../relational-databases/security/encryption/transparent-data-encryption-azure-sql.md)  
 [Prise en main du chiffrement transparent des données (TDE) sur SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-encryption-tde-tsql/)  
 [Chiffrement SQL Server](../../../relational-databases/security/encryption/sql-server-encryption.md)  
 [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  
   
## <a name="see-also"></a> Voir aussi  
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [FILESTREAM &#40;SQL Server&#41;](../../../relational-databases/blob/filestream-sql-server.md)  
  
  
