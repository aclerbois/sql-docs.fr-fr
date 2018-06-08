---
title: DBCC CHECKALLOC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CHECKALLOC_TSQL
- DBCC CHECKALLOC
- DBCC_CHECKALLOC_TSQL
- CHECKALLOC
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKALLOC statement
- checking database space allocation
- database space allocation [SQL Server]
- consistency [SQL Server], disk space allocations
- space allocation [SQL Server]
- allocation checks
- disk space [SQL Server], allocation consistency checks
- space allocation [SQL Server], checking
ms.assetid: bc1218eb-ffff-44ce-8122-6e4fa7d68a79
caps.latest.revision: 76
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 5532f6870e19e830a4d8925fbfca68862493fa6a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-checkalloc-transact-sql"></a>DBCC CHECKALLOC (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Vérifie la cohérence des structures d'allocation de l'espace disque pour une base de données spécifiée.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```
DBCC CHECKALLOC   
[  
    ( database_name | database_id | 0   
      [ , NOINDEX   
      | , { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD } ]  
    )  
    [ WITH   
        {   
          [ ALL_ERRORMSGS ]  
          [ , NO_INFOMSGS ]   
          [ , TABLOCK ]   
          [ , ESTIMATEONLY ]   
        }  
    ]  
]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* | *database_id* | 0   
 Nom ou ID de la base de données sur laquelle l’allocation et l’utilisation des pages doivent être vérifiées.
En l'absence de spécification, ou si 0 est spécifié, la base de données actuelle est utilisée.
Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).

 NOINDEX  
 Indique que la vérification ne doit pas tenir compte des index non-cluster dans les tables utilisateurs.<br>NOINDEX est conservé seulement pour la compatibilité descendante, et n'affecte pas DBCC CHECKALLOC.

 REPAIR_ALLOW_DATA_LOSS \| REPAIR_FAST \| REPAIR_REBUILD  
 Spécifie que DBCC CHECKALLOC répare les erreurs trouvées. *database_name* doit être en mode mono-utilisateur.

 REPAIR_ALLOW_DATA_LOSS  
 Tente de réparer toutes les erreurs trouvées. Ces réparations peuvent entraîner des pertes de données. REPAIR_ALLOW_DATA_LOSS est la seule option qui permet de réparer les erreurs d'allocation.

 REPAIR_FAST  
 La syntaxe n'est conservée que pour la compatibilité descendante. Aucune réparation n'est effectuée.

 REPAIR_REBUILD  
 Non applicable.  
 N'utilisez les options REPAIR qu'en dernier recours. Pour réparer les erreurs, nous vous recommandons d'effectuer une restauration à partir d'une sauvegarde. Les opérations de réparation ne prennent en compte aucune des contraintes qui peuvent exister sur les tables ou entre tables. Si la table spécifiée est impliquée dans une ou plusieurs contraintes, nous vous recommandons d'exécuter DBCC CHECKCONSTRAINTS après une réparation. Si vous devez utiliser REPAIR, exécutez la commande DBCC CHECKDB sans option de réparation afin de déterminer le niveau de réparation à utiliser. Si vous utilisez le niveau REPAIR_ALLOW_DATA_LOSS, nous vous recommandons de sauvegarder la base de données avant d'exécuter la commande DBCC CHECKDB avec cette option.

 par  
 Permet de spécifier des options.

 ALL_ERRORMSGS  
 Affiche tous les messages d'erreur. Tous les messages d'erreur sont affichés par défaut. La spécification ou non de cette option n'a aucun effet.

 NO_INFOMSGS  
 Supprime tous les messages d'information, ainsi que le rapport sur l'espace utilisé.

 TABLOCK  
 Permet à la commande DBCC d'obtenir un verrou de base de données exclusif.

 ESTIMATE ONLY  
 Affiche une estimation de la quantité d’espace tempdb nécessaire pour exécuter DBCC CHECKALLOC quand toutes les autres options sont spécifiées.
  
## <a name="remarks"></a>Notes   
DBCC CHECKALLOC vérifie l'allocation de toutes les pages de la base de données, quel que soit le type de page ou le type d'objet auquel elles appartiennent. Il valide également les différentes structures internes qui sont utilisées pour garder la trace de ces pages et des relations entre elles.
Si NO_INFOMSGS n'est pas spécifié, DBCC CHECKALLOC collecte les informations d'utilisation de l'espace pour tous les objets de la base de données. Ces informations sont imprimées avec les erreurs détectées.
  
> [!NOTE]  
> La fonctionnalité DBCC CHECKALLOC est incluse dans [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) et [DBCC CHECKFILEGROUP](../../t-sql/database-console-commands/dbcc-checkfilegroup-transact-sql.md). Il n'est donc pas nécessaire d'exécuter DBCC CHECKALLOC indépendamment de ces instructions.   DBCC CHECKALLOC ne vérifie pas les données FILESTREAM. FILESTREAM stocke les objets BLOB dans le système de fichiers.  
  
## <a name="internal-database-snapshot"></a>Instantané de base de données interne  
DBCC CHECKALLOC utilise un instantané interne de la base de données pour assurer la cohérence transactionnelle nécessaire pour l'exécution de ces vérifications. S'il n'est pas possible de créer un instantané, ou si TABLOCK est spécifié, DBCC CHECKALLOC tente d'acquérir un verrou exclusif sur la base de données pour obtenir la cohérence nécessaire.
  
> [!NOTE]  
> L’exécution de DBCC CHECKALLOC sur tempdb n’effectue aucune vérification. En effet, pour des raisons de performances, les instantanés de base de données ne sont pas disponibles sur tempdb. Cela signifie que la cohérence transactionnelle requise ne peut pas être obtenue. Arrêtez et démarrez le service MSSQLSERVER pour résoudre les problèmes d’allocation de tempdb. Cette action supprime la base de données tempdb, puis la recrée.  
  
## <a name="understanding-dbcc-error-messages"></a>Présentation des messages d'erreur de DBCC  
Une fois la commande DBCC CHECKALLOC exécutée, un message est consigné dans le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la commande DBCC est correctement exécutée, le message indique que l'exécution a réussi, ainsi que la durée d'exécution de la commande. Si la commande DBCC est interrompue avant la fin de la vérification en raison d’une erreur, le message indique que la commande n’a pas abouti et précise une valeur d’état ainsi que la durée d’exécution de la commande. Le tableau suivant répertorie et décrit les valeurs d'état pouvant être incluses dans le message.
  
|État|Description|  
|---|---|  
|0|Erreur numéro 8930 générée. Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|  
| 1|Erreur numéro 8967 générée. Une erreur DBCC interne s'est produite.|  
|2|Une erreur s'est produite lors de la réparation de la base de données en mode urgence.|  
|3|Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|  
|4|Une assertion ou une violation d'accès a été détectée.|  
|5|Une erreur inconnue s'est produite et a arrêté la commande DBCC.|  
  
## <a name="error-reporting"></a>Rapport d'erreurs  
Un mini-fichier de vidage (SQLDUMP*nnnn*.txt) est créé dans le répertoire LOG de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chaque fois que DBCC CHECKALLOC détecte une erreur d’altération. Lorsque les fonctions de collecte des données d'utilisation des fonctionnalités et de rapport d'erreurs sont activées pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce fichier est automatiquement transféré à [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Les données collectées sont utilisées pour améliorer les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Le fichier de vidage contient les résultats de la commande DBCC CHECKALLOC ainsi que des informations de diagnostic supplémentaires. Ce fichier contient des listes de contrôle d'accès discrétionnaire (DACL, Discretionary Access Control Lists) avec accès restreint. L’accès est limité au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux membres du rôle sysadmin. Par défaut, le rôle sysadmin contient tous les membres du groupe Windows BUILTIN\Administrators et du groupe Administrateurs local. La commande DBCC n'échoue pas si le processus de collecte des données échoue.
  
## <a name="resolving-errors"></a>Résolution des erreurs  
Si DBCC CHECKALLOC signale des erreurs, nous vous recommandons de restaurer la base de données à partir de la sauvegarde de la base de données plutôt que d'exécuter une réparation. S'il n'existe pas de sauvegarde, l'exécution d'une réparation peut corriger les erreurs signalées ; cependant, la correction de ces erreurs risque de nécessiter la suppression de plusieurs pages, et donc de données.
Une réparation peut être exécutée dans une transaction utilisateur. Ceci permet d'annuler les modifications. Si des modifications sont annulées, la base de données contiendra encore des erreurs et il faudra donc la restaurer à partir d'une sauvegarde. Une fois les réparations effectuées, sauvegardez la base de données.
  
## <a name="result-sets"></a>Jeux de résultats  
Les tableaux suivants décrivent les informations retournées par DBCC CHECKALLOC.
  
|Élément|Description|  
|---|---|  
|FirstIAM|À usage interne uniquement|  
|Root|À usage interne uniquement|  
|Dpages|Nombre de pages de données.|  
|Pages utilisées (Pages used)|Pages allouées.|  
|Extensions dédiées (Dedicated extents)|Extensions allouées à l'objet<br /><br /> En cas d'utilisation de pages d'allocation mixtes, certaines pages allouées peuvent n'avoir aucune extension.|  
  
DBCC CHECKALLOC fournit aussi un résumé de l'allocation pour chaque index et partition de chaque fichier. Ce résumé décrit la distribution des données.
  
|Élément|Description|  
|---|---|  
|Pages réservées (Reserved pages)|Pages allouées à l'index et pages inutilisées dans les extensions allouées.|  
|Pages utilisées (Used pages)|Pages allouées et utilisées par l'index.|  
|ID de partition (Partition ID)|À usage interne uniquement|  
|ID d'unité d'allocation|À usage interne uniquement|  
|Données dans la ligne (In-row data)|Les pages contiennent des données de segment de mémoire ou d'index.|  
|Données LOB (LOB data)|Les pages contiennent des données de type **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **text**, **ntext**, **xml** et **image**.|  
|Données de dépassement de ligne (Row-overflow data)|Les pages contiennent des données de colonne de longueur variable qui ont été envoyées hors ligne.|  
  
DBCC CHECKALLOC renvoie le jeu de résultats suivant (les valeurs peuvent varier), sauf lorsque ESTIMATEONLY ou NO_INFOMSGS est spécifié.
  
```
DBCC results for 'master'.  
***************************************************************  
Table sysobjects                Object ID 1.  
Index ID 1         FirstIAM (1:11)   Root (1:12)    Dpages 22.  
    Index ID 1. 24 pages used in 5 dedicated extents.  
Index ID 2         FirstIAM (1:1368)   Root (1:1362)    Dpages 10.  
    Index ID 2. 12 pages used in 2 dedicated extents.  
Index ID 3         FirstIAM (1:1392)   Root (1:1408)    Dpages 4.  
    Index ID 3. 6 pages used in 0 dedicated extents.  
Total number of extents is 7.  
***************************************************************  
'...'  
***************************************************************  
Table spt_server_info                Object ID 1938105945.  
Index ID 1         FirstIAM (1:520)   Root (1:508)    Dpages 1.  
    Index ID 1. 3 pages used in 0 dedicated extents.  
Total number of extents is 0.  
***************************************************************  
Processed 52 entries in sysindexes for database ID 1.  
File 1. Number of extents = 210, used pages = 1126, reserved pages = 1280.  
           File 1 (number of mixed extents = 73, mixed pages = 184).  
    Object ID 1, Index ID 0, data extents 5, pages 24, mixed extent pages 9.  
'...'  
    Object ID 1938105945, Index ID 0, data extents 0, pages 3, mixed extent pages 3.  
Total number of extents = 210, used pages = 1126, reserved pages = 1280 in this database.  
       (number of mixed extents = 73, mixed pages = 184) in this database.  
CHECKALLOC found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC results for 'master'.  
***************************************************************  
Table sys.sysrowsetcolumns                Object ID 4.  
Index ID 1, partition ID 262144, alloc unit ID 262144 (type In-row data). FirstIAM (1:98). Root (1:94). Dpages 7.  
Index ID 1, partition ID 262144, alloc unit ID 262144 (type In-row data). 9 pages used in 1 dedicated extents.  
Index ID 1, partition ID 262144, alloc unit ID 262398 (type Row-overflow data). FirstIAM (0:0). Root (0:0). Dpages 0.  
Index ID 1, partition ID 262144, alloc unit ID 262398 (type Row-overflow data). 0 pages used in 0 dedicated extents.  
Total number of extents is 1.  
...  
***************************************************************  
Processed 201 entries in system catalog for database ID 1.  
File 1. Number of extents = 44, used pages = 300, reserved pages = 345.  
           File 1 (number of mixed extents = 29, mixed pages = 225).  
    Object ID 4, index ID 1, partition ID 262144, alloc unit ID 262144 (type In-row data), data extents 1, pages 9, mixed extent pages 8.  
    Object ID 5, index ID 1, partition ID 327680, alloc unit ID 327680 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
    Object ID 7, index ID 1, partition ID 458752, alloc unit ID 458752 (type In-row data), data extents 0, pages 5, mixed extent pages 5.  
    Object ID 8, index ID 0, partition ID 524288, alloc unit ID 524288 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
    Object ID 13, index ID 1, partition ID 851968, alloc unit ID 851968 (type In-row data), data extents 1, pages 9, mixed extent pages 8.  
    Object ID 15, index ID 1, partition ID 983040, alloc unit ID 983040 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
    Object ID 26, index ID 1, partition ID 281474978414592, alloc unit ID 1703937 (type In-row data), data extents 0, pages 3, mixed extent pages 3.  
    Object ID 27, index ID 1, partition ID 281474978480128, alloc unit ID 1769473 (type In-row data), data extents 0, pages 3, mixed extent pages 3.  
    Object ID 27, index ID 2, partition ID 562949955190784, alloc unit ID 1769474 (type In-row data), index extents 0, pages 3, mixed extent pages 3.  
...  
    Object ID 1179151246, index ID 1, partition ID 72057594038845440, alloc unit ID 13435136 (type In-row data), data extents 2, pages 18, mixed extent pages 8.  
    Object ID 1179151246, index ID 2, partition ID 72057594038910976, alloc unit ID 13566208 (type In-row data), index extents 1, pages 16, mixed extent pages 8.  
    Object ID 1911677858, index ID 0, partition ID 72057594039631872, alloc unit ID 15073536 (type In-row data), data extents 0, pages 2, mixed extent pages 2.  
Total number of extents = 41, used pages = 289, reserved pages = 323 in this database.  
       (number of mixed extents = 27, mixed pages = 211) in this database.  
CHECKALLOC found 0 allocation errors and 0 consistency errors in database 'master'.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
Lorsque ESTIMATEONLY est spécifié, DBCC CHECKALLOC retourne le jeu de résultats suivant.
  
```
Estimated TEMPDB space needed for CHECKALLOC (KB)   
-------------------------------------------------   
34  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner.
  
## <a name="examples"></a>Exemples  
L'exemple suivant exécute `DBCC CHECKALLOC` pour la base de données actuelle et pour la base de données `AdventureWorks2012`.
  
```sql  
-- Check the current database.  
DBCC CHECKALLOC;  
GO  
-- Check the AdventureWorks2012 database.  
DBCC CHECKALLOC (AdventureWorks2012);  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  

