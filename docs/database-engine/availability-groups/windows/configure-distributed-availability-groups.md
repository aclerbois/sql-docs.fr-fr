---
title: Configurer un groupe de disponibilité distribué (Groupe de disponibilité AlwaysOn) | Microsoft Docs
ms.custom: ''
ms.date: 08/17/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f7c7acc5-a350-4a17-95e1-e689c78a0900
caps.latest.revision: 28
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6dd177d3094f50cd226ed5613ded8fc0d76e6891
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769065"
---
# <a name="configure-distributed-availability-group"></a>Configurer un groupe de disponibilité distribué  
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Pour créer un groupe de disponibilité distribué, vous devez créer un groupe de disponibilité et un écouteur sur chaque cluster de basculement Windows Server. Vous combinez ensuite ces groupes de disponibilité dans un groupe de disponibilité distribué. Les étapes suivantes fournissent un exemple de base dans Transact-SQL. Cet exemple ne couvre pas tous les détails de la création des groupes de disponibilité et des écouteurs. Son but est de mettre en évidence les exigences principales. 

Pour obtenir une présentation technique des groupes de disponibilité distribués, consultez [Groupes de disponibilité distribués](distributed-availability-groups.md).   

## <a name="prerequisites"></a>Conditions préalables requises

### <a name="set-the-endpoint-listeners-to-listen-to-all-ip-addresses"></a>Définir les écouteurs de point de terminaison pour écouter toutes les adresses IP

Vérifiez que les points de terminaison peuvent communiquer entre les différents groupes de disponibilité du groupe de disponibilité distribué. Si un groupe de disponibilité est défini sur un réseau spécifique sur le point de terminaison, le groupe de disponibilité distribué ne fonctionne pas correctement. Sur chaque serveur qui héberge un réplica dans le groupe de disponibilité distribué, configurez l’écouteur sur `LISTENER_IP = ALL`. 

#### <a name="create-a-listener-to-listen-to-all-ip-addresses"></a>Créer un écouteur pour écouter toutes les adresses IP

Par exemple, le script suivant crée un point de terminaison d’écouteur sur le port TCP 5022 qui écoute sur toutes les adresses IP.  

```sql
CREATE ENDPOINT [aodns-hadr] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)
FOR DATA_MIRRORING (
   ROLE = ALL, 
   AUTHENTICATION = WINDOWS NEGOTIATE,
   ENCRYPTION = REQUIRED ALGORITHM AES
)
GO
```

#### <a name="alter-a-listener-to-listen-to-all-ip-addresses"></a>Modifier un écouteur pour écouter toutes les adresses IP

Par exemple, le script suivant modifie un point de terminaison d’écouteur pour qu’il écoute sur toutes les adresses IP.  

```sql
ALTER ENDPOINT [aodns-hadr] 
    AS TCP (LISTENER_IP = ALL)
GO
```

## <a name="create-first-availability-group"></a>Créer un premier groupe de disponibilité

### <a name="create-the-primary-availability-group-on-the-first-cluster"></a>Créer le groupe de disponibilité principal sur le premier cluster  
Créez un groupe de disponibilité sur le premier cluster WSFC.   Dans cet exemple, le groupe de disponibilité est nommé `ag1` pour la base de données `db1`.      
  
```sql  
CREATE AVAILABILITY GROUP [ag1]   
FOR DATABASE db1   
REPLICA ON N'server1' WITH (ENDPOINT_URL = N'TCP://server1.contoso.com:5022',  
    FAILOVER_MODE = AUTOMATIC,  
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC),   
N'server2' WITH (ENDPOINT_URL = N'TCP://server2.contoso.com:5022',   
    FAILOVER_MODE = AUTOMATIC,   
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC);   
GO  
  
```  
  
>[!NOTE]
>L’exemple précédent utilise un amorçage direct, où **SEEDING_MODE** a la valeur **AUTOMATIC** pour les réplicas et le groupe de disponibilité distribué. Cette configuration définit les réplicas secondaires et le groupe de disponibilité secondaire pour qu’ils soient renseignés automatiquement sans qu’une sauvegarde manuelle et une restauration de base de données primaire soient nécessaires.  
  
### <a name="join-the-secondary-replicas-to-the-primary-availability-group"></a>Joindre les réplicas secondaires au groupe de disponibilité principal  
Les réplicas secondaires doivent être joints au groupe de disponibilité **ALTER AVAILABILITY GROUP** avec l’option **JOIN** . Étant donné que l’amorçage direct est utilisé dans cet exemple, vous devez également appeler  **ALTER AVAILABILITY GROUP** avec l’option **GRANT CREATE ANY DATABASE** . Ainsi, le groupe de disponibilité peut créer la base de données et commencer l’amorçage automatique à partir du réplica principal.  
  
Dans cet exemple, les commandes suivantes sont exécutées sur le réplica secondaire `server2`pour rejoindre le groupe de disponibilité `ag1` . Le groupe de disponibilité est ensuite autorisé à créer des bases de données sur le réplica secondaire.  
  
```sql  
ALTER AVAILABILITY GROUP [ag1] JOIN   
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE  
GO  
```  

>[!NOTE]
>Quand le groupe de disponibilité crée une base de données sur un réplica secondaire, il définit le propriétaire de la base de données en tant que compte qui a exécuté l’instruction `ALTER AVAILABILITY GROUP` pour accorder l’autorisation de créer une base de données. Pour plus d’informations, consultez [ Octroyer l’autorisation de créer une base de données sur un réplica secondaire au groupe de disponibilité](automatic-seeding-secondary-replicas.md#grantCreate).
  
### <a name="create-a-listener-for-the-primary-availability-group"></a>Créer un écouteur pour le groupe de disponibilité principal  

Ajoutez ensuite un écouteur pour le groupe de disponibilité principal sur le premier cluster WSFC. Dans cet exemple, l’écouteur est nommé `ag1-listener`. Pour obtenir des instructions détaillées sur la création d’un écouteur, consultez [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
```sql
ALTER AVAILABILITY GROUP [ag1]    
    ADD LISTENER 'ag1-listener' ( 
        WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , 
        PORT = 60173);    
GO  
```  
  

## <a name="create-second-availability-group"></a>Créer un second groupe de disponibilité  
 Puis, sur le deuxième cluster WSFC, créez un deuxième groupe de disponibilité `ag2`. Dans ce cas, la base de données n’est pas spécifiée, car elle est amorcée automatiquement à partir du groupe de disponibilité principal.  
  
```sql  
CREATE AVAILABILITY GROUP [ag2]   
FOR   
REPLICA ON N'server3' WITH (ENDPOINT_URL = N'TCP://server3.contoso.com:5022',   
    FAILOVER_MODE = MANUAL,   
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC),   
N'server4' WITH (ENDPOINT_URL = N'TCP://server4.contoso.com:5022',   
    FAILOVER_MODE = MANUAL,   
    AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,   
    BACKUP_PRIORITY = 50,   
    SECONDARY_ROLE(ALLOW_CONNECTIONS = NO),   
    SEEDING_MODE = AUTOMATIC);   
GO  
```  
  
> [!NOTE]  
> Le groupe de disponibilité secondaire doit utiliser le même point de terminaison de mise en miroir de bases de données (le port 5022 dans l’exemple). Sinon, la réplication s’arrête après un basculement local.  
  
### <a name="join-the-secondary-replicas-to-the-secondary-availability-group"></a>Joindre les réplicas secondaires au groupe de disponibilité secondaire  
 Dans cet exemple, les commandes suivantes sont exécutées sur le réplica secondaire `server4`pour rejoindre le groupe de disponibilité `ag2` . Le groupe de disponibilité est ensuite autorisé à créer des bases de données sur le réplica secondaire pour prendre en charge l’amorçage direct.  
  
```sql  
ALTER AVAILABILITY GROUP [ag2] JOIN   
ALTER AVAILABILITY GROUP [ag2] GRANT CREATE ANY DATABASE  
GO  
```  
  
### <a name="create-a-listener-for--the-secondary-availability-group"></a>Créer un écouteur pour le groupe de disponibilité secondaire  
 Ajoutez ensuite un écouteur au groupe de disponibilité secondaire sur le deuxième cluster WSFC. Dans cet exemple, l’écouteur est nommé `ag2-listener`. Pour obtenir des instructions détaillées sur la création d’un écouteur, consultez [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
```  
ALTER AVAILABILITY GROUP [ag2]    
    ADD LISTENER 'ag2-listener' ( WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , PORT = 60173);    
GO  
```  
  
## <a name="create-distributed-availability-group-on-first-cluster"></a>Créer un groupe de disponibilité distribué sur le premier cluster  
 Sur le premier cluster WSFC, créez un groupe de disponibilité distribué (nommé `distributedag` dans cet exemple). Utilisez la commande **CREATE AVAILABILITY GROUP** avec l’option **DISTRIBUTED** . Le paramètre **AVAILABILITY GROUP ON** spécifie les groupes de disponibilité membres `ag1` et `ag2`.  
  
```sql  
CREATE AVAILABILITY GROUP [distributedag]  
   WITH (DISTRIBUTED)   
   AVAILABILITY GROUP ON  
      'ag1' WITH    
      (   
         LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      ),   
      'ag2' WITH    
      (   
         LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      );    
GO   
```  
  
> [!NOTE]  
>  **LISTENER_URL** spécifie l’écouteur pour chaque groupe de disponibilité, ainsi que le point de terminaison de mise en miroir de bases de données du groupe de disponibilité. Dans cet exemple, il s’agit du port `5022` (et non du port `60173` qui a permis de créer l’écouteur). Si vous utilisez un équilibreur de charge, par exemple dans Azure, [ajoutez une règle d’équilibrage de charge pour le port du groupe de disponibilité distribué](http://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#add-load-balancing-rule-for-distributed-availability-group). Ajoutez la règle pour le port d’écoute, en plus du port de l’instance SQL Server. 
  
## <a name="join-distributed-availability-group-on-second-cluster"></a>Joindre un groupe de disponibilité distribué sur le second cluster  
 Joignez ensuite le groupe de disponibilité distribué au deuxième cluster WSFC.  
  
```sql  
ALTER AVAILABILITY GROUP [distributedag]   
   JOIN   
   AVAILABILITY GROUP ON  
      'ag1' WITH    
      (   
         LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      ),   
      'ag2' WITH    
      (   
         LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC   
      );    
GO  
```  

## <a name="failover"></a>Joindre la base de données sur le réplica secondaire du deuxième groupe de disponibilité
Quand la base de données qui se trouve sur le réplica secondaire du deuxième groupe de disponibilité est en restauration, vous devez la joindre manuellement au groupe de disponibilité.

```sql  
ALTER DATABASE [db1] SET HADR AVAILABILITY GROUP = [ag2];   
```  
  
## <a name="failover"></a> Basculer vers un groupe de disponibilité secondaire  
Seul le basculement manuel est pris en charge pour l’instant. L’instruction Transact-SQL suivante bascule le groupe de disponibilité distribué nommé `distributedag` :  


1. Définissez le mode de disponibilité sur validation synchrone pour les deux groupes de disponibilité. 
    
      ```sql  
      ALTER AVAILABILITY GROUP [distributedag] 
      MODIFY 
      AVAILABILITY GROUP ON
      'ag1' WITH 
         ( 
          LISTENER_URL = 'tcp://ag1-listener.contoso.com:5022',  
          AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
          FAILOVER_MODE = MANUAL, 
          SEEDING_MODE = MANUAL 
          ), 
      'ag2' WITH  
        ( 
        LISTENER_URL = 'tcp://ag2-listener.contoso.com:5022', 
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
        FAILOVER_MODE = MANUAL, 
        SEEDING_MODE = MANUAL 
        );  
       
      ```  
   >[!NOTE]
   >De même que pour les groupes de disponibilité normaux, l’état de synchronisation entre deux réplicas de groupe de disponibilité faisant partie d’un groupe de disponibilité distribué dépend du mode de disponibilité des deux réplicas. Par exemple, pour obtenir une validation synchrone, le groupe de disponibilité principal et le groupe de disponibilité secondaire doivent tous deux être configurés avec le mode de disponibilité synchronous_commit.  


1. Attendez que l’état du groupe de disponibilité distribuée soit défini sur `SYNCHRONIZED`. Exécutez la requête suivante sur le serveur SQL Server qui héberge le réplica principal du groupe de disponibilité principal. 
    
      ```sql  
      SELECT ag.name
             , drs.database_id
             , drs.group_id
             , drs.replica_id
             , drs.synchronization_state_desc
             , drs.end_of_log_lsn 
        FROM sys.dm_hadr_database_replica_states drs,
        sys.availability_groups ag
          WHERE drs.group_id = ag.group_id;      
      ```  

    Continuez une fois que le groupe de disponibilité **synchronization_state_desc** est `SYNCHRONIZED`. Si **synchronization_state_desc** n’est pas `SYNCHRONIZED`, exécutez la commande toutes les cinq secondes jusqu’à ce qu’il change. Ne continuez pas jusqu’à **synchronization_state_desc** = `SYNCHRONIZED`. 

1. Sur le serveur SQL Server hébergeant le réplica principal pour le groupe de disponibilité principal, affectez la valeur `SECONDARY` au rôle du groupe de disponibilité distribué. 

    ```sql
    ALTER AVAILABILITY GROUP distributedag SET (ROLE = SECONDARY); 
    ```  

    À ce stade, le groupe de disponibilité distribué n’est pas disponible.

1. Testez la disponibilité du basculement. Exécutez la requête suivante :

    ```sql
    SELECT ag.name, 
        drs.database_id, 
        drs.group_id, 
        drs.replica_id, 
        drs.synchronization_state_desc, 
        drs.end_of_log_lsn 
    FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
    WHERE drs.group_id = ag.group_id; 
    ```  
    Le groupe de disponibilité est prêt pour le basculement quand **synchronization_state_desc** est `SYNCHRONIZED` et que **end_of_log_lsn** est identique pour les deux groupes de disponibilité. 

1. Basculez du groupe de disponibilité principal vers le groupe de disponibilité secondaire. Exécutez la commande suivante sur le serveur SQL Server qui héberge le réplica principal du groupe de disponibilité secondaire. 

    ```sql
    ALTER AVAILABILITY GROUP distributedag FORCE_FAILOVER_ALLOW_DATA_LOSS; 
    ```  

    Après cette étape, le groupe de disponibilité distribué est disponible.
      
Après avoir effectué les étapes ci-dessus, le groupe de disponibilité distribué bascule sans perte de données. Si les groupes de disponibilité sont à une distance géographique qui provoque des temps de latence, Microsoft vous recommande de définir le mode de disponibilité sur ASYNCHRONOUS_COMMIT. 
  
## <a name="remove-a-distributed-availability-group"></a>Supprimer un groupe de disponibilité distribué  
 L’instruction Transact-SQL suivante supprime un groupe de disponibilité distribué nommé `distributedag`:  
  
```sql  
DROP AVAILABILITY GROUP [distributedag]  
```  

## <a name="create-distributed-availability-group-on-failover-cluster-instances"></a>Créer un groupe de disponibilité distribué sur des instances de cluster de basculement

Vous pouvez créer un groupe de disponibilité distribué à l’aide d’un groupe de disponibilité sur une instance de cluster de basculement (FCI). Dans ce cas, vous n’avez pas besoin d’écouteur de groupe de disponibilité. Utilisez le nom de réseau virtuel pour le réplica principal de l’instance FCI. L’exemple suivant montre un groupe de disponibilité distribué appelé SQLFCIDAG. Un des groupes de disponibilité est SQLFCIAG. SQLFCIAG a deux réplicas FCI. Le VNN pour le réplica FCI principal est SQLFCIAG-1, et celui du réplica FCI secondaire est SQLFCIAG-2. Le groupe de disponibilité distribué inclut également SQLAG-DR pour la récupération d’urgence.

![Groupe de disponibilité distribué Always On](../../../database-engine/availability-groups/windows/media/always-on-availability-group-distributed.png)

 
 
 Le DDL suivant crée ce groupe de disponibilité distribué. 

```sql  
CREATE AVAILABILITY GROUP [SQLFCIDAG]  
   WITH (DISTRIBUTED)   
   AVAILABILITY GROUP ON  
  'SQLFCIAG' WITH    
    (   
        LISTENER_URL = 'tcp://SQLFCIAG-1.contoso.com:5022',    
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC
      ),   
  'SQLAG-DR' WITH    
       (   
         LISTENER_URL = 'tcp://SQLAG-DR.contoso.com:5022',   
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,   
         FAILOVER_MODE = MANUAL,   
         SEEDING_MODE = AUTOMATIC
      );   
```  

L’URL de l’écouteur est le VNN de l’instance FCI principale.

## <a name="manually-fail-over-fci-in-distributed-availability-group"></a>Basculer manuellement FCI dans le groupe de disponibilité distribué

Pour basculer manuellement le groupe de disponibilité FCI, mettez à jour le groupe de disponibilité distribué de façon à refléter la modification de l’URL de l’écouteur. Par exemple, exécutez le DDL suivant sur le groupe de disponibilité principal et le groupe de disponibilité secondaire de SQLFCIAG :

```sql  
ALTER AVAILABILITY GROUP [SQLFCIDAG]  
   MODIFY AVAILABILITY GROUP ON  
 'SQLFCIAG' WITH    
    (   
        LISTENER_URL = 'tcp://SQLFCIAG-2.contoso.com:5022'
    )
```  
  
## <a name="next-steps"></a>Étapes suivantes

 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  
