---
title: Utilitaire tablediff | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: tabledif
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- comparing data
- tablediff utility
- tables [SQL Server replication]
- table comparisons [SQL Server]
- command prompt utilities [SQL Server], tablediff
- troubleshooting [SQL Server replication], non-convergence
- non-convergence [SQL Server]
ms.assetid: 3c3cb865-7a4d-4d66-98f2-5935e28929fc
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: daf978c77bca856ebc380c95bdca7185bcdee836
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tablediff-utility"></a>tablediff (utilitaire)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  L’utilitaire **tablediff** sert à comparer les données dans deux tables et à identifier une non-convergence. Il est particulièrement utile pour résoudre des problèmes de non-convergence dans une topologie de réplication. Cet utilitaire peut être employé à partir de l'invite de commandes ou dans un fichier de commandes pour effectuer les tâches suivantes :  
  
-   Une comparaison ligne par ligne entre une table source dans une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] agissant comme serveur de publication de réplication et la table de destination dans une ou plusieurs instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] agissant comme abonnés de réplication.  
  
-   Comparaison rapide se limitant à la comparaison du nombre de lignes et du schéma.  
  
-   Comparaisons au niveau des colonnes.  
  
-   Génération d'un script [!INCLUDE[tsql](../includes/tsql-md.md)] pour corriger les différences sur le serveur de destination afin de mettre en convergence les tables source et de destination.  
  
-   Consignation des résultats dans un fichier de sortie ou dans une table de la base de données de destination.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
tablediff   
[ -? ] |   
{  
        -sourceserver source_server_name[\instance_name]  
        -sourcedatabase source_database  
        -sourcetable source_table_name   
    [ -sourceschema source_schema_name ]  
    [ -sourcepassword source_password ]  
    [ -sourceuser source_login ]  
    [ -sourcelocked ]  
        -destinationserver destination_server_name[\instance_name]  
        -destinationdatabase subscription_database   
        -destinationtable destination_table   
    [ -destinationschema destination_schema_name ]  
    [ -destinationpassword destination_password ]  
    [ -destinationuser destination_login ]  
    [ -destinationlocked ]  
    [ -b large_object_bytes ]   
    [ -bf number_of_statements ]   
    [ -c ]   
    [ -dt ]   
    [ -et table_name ]   
    [ -f [ file_name ] ]   
    [ -o output_file_name ]   
    [ -q ]   
    [ -rc number_of_retries ]   
    [ -ri retry_interval ]   
    [ -strict ]  
    [ -t connection_timeouts ]   
}  
```  
  
## <a name="arguments"></a>Arguments  
 [ **-?** ]  
 Retour de la liste des paramètres pris en charge.  
  
 **-sourceserver** *source_server_name*[**\\***instance_name*]  
 Nom du serveur source. Spécifiez *source_server_name* pour l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Spécifiez *source_server_name***\\*** instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-sourcedatabase** *source_database*  
 Nom de la base de données source.  
  
 **-sourcetable** *source_table_name*  
 Nom de la table source en cours de vérification.  
  
 **-sourceschema** *source_schema_name*  
 Propriétaire du schéma de la table source. Par défaut, le propriétaire de la table est supposé être dbo.  
  
 **-sourcepassword** *source_password*  
 Mot de passe de la connexion utilisée pour la connexion au serveur source avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Lorsque cela est possible, fournissez les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 **-sourceuser** *source_login*  
 Connexion employée pour établir la connexion au serveur source avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si *source_login* n’est pas fourni, l’authentification Windows est employée lors de la connexion au serveur source. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-sourcelocked**  
 Durant la comparaison, la table source est verrouillée à l'aide des indicateurs de table TABLOCK et HOLDLOCK.  
  
 **-destinationserver** *destination_server_name*[**\\***instance_name*]  
 Nom du serveur de destination. Spécifiez *destination_server_name* pour l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Spécifiez *destination_server_name***\\*** instance_name* pour une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-destinationdatabase** *subscription_database*  
 Nom de la base de données de destination.  
  
 **-destinationtable** *destination_table*  
 Nom de la table de destination.  
  
 **-destinationschema** *destination_schema_name*  
 Le propriétaire du schéma de la table de destination. Par défaut, le propriétaire de la table est supposé être dbo.  
  
 **-destinationpassword** *destination_password*  
 Mot de passe de la connexion utilisée pour établir une connexion au serveur de destination avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Lorsque cela est possible, fournissez les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 **-destinationuser** *destination_login*  
 Connexion employée pour établir la connexion au serveur de destination avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si *destination_login* n’est pas fourni, l’authentification Windows est employée lors de la connexion au serveur. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-destinationlocked**  
 Durant la comparaison, la table de destination est verrouillée à l'aide des indicateurs de table TABLOCK et HOLDLOCK.  
  
 **-b** *large_object_bytes*  
 Nombre d’octets à comparer pour des colonnes de types de données d’objet volumineuses, notamment **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)** et **varbinary(max)**. *large_object_bytes* a comme valeur par défaut la taille de la colonne. Toutes les données au-dessus de *large_object_bytes* ne sont pas comparées.  
  
 **-bf**  *number_of_statements*  
 Nombre d’instructions [!INCLUDE[tsql](../includes/tsql-md.md)] à écrire dans le fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] actuel lorsque l’option **-f** est utilisée. Quand le nombre d’instructions [!INCLUDE[tsql](../includes/tsql-md.md)] dépasse *number_of_statements*, un fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] est créé.  
  
 **-c**  
 Compare les différences au niveau des colonnes.  
  
 **-dt**  
 Supprime la table de résultats spécifiée par *table_name*si la table existe déjà.  
  
 **-et** *table_name*  
 Spécifie le nom de la table de résultats à créer. Si cette table existe déjà, **-DT** doit être utilisé, sinon l’opération échoue.  
  
 **-f** [ *file_name* ]  
 Génère un script [!INCLUDE[tsql](../includes/tsql-md.md)] pour mettre la table sur le serveur de destination en convergence avec la table sur le serveur source. Vous pouvez éventuellement spécifier un nom et un chemin d'accès pour le fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] généré. Si *file_name* n’est pas spécifié, le fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] est créé dans le répertoire où l’utilitaire est exécuté.  
  
 **-o** *output_file_name*  
 Nom complet et chemin d'accès du fichier de sortie.  
  
 **-q**  
 Comparaison rapide se limitant à la comparaison du nombre de lignes et du schéma.  
  
 **-rc** *number_of_retries*  
 Nombre de fois où l'utilitaire retente une opération qui a échoué.  
  
 **-ri**  *retry_interval*  
 Intervalle, en secondes, d'attente entre les tentatives.  
  
 **-strict**  
 Les schémas source et de destination sont comparés de manière stricte.  
  
 **-t** *connection_timeouts*  
 Définit le délai d'attente de connexion, en secondes, pour les connexions au serveur source et au serveur de destination.  
  
## <a name="return-value"></a>Valeur retournée  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Réussi|  
|**1**|Erreur critique|  
|**2**|Tables différentes|  
  
## <a name="remarks"></a>Notes   
 L’utilitaire **tablediff** ne peut pas être utilisé avec des serveurs non-[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Les tables comprenant des colonnes de types de données **sql_variant** ne sont pas prises en charge.  
  
 Par défaut, l’utilitaire **tablediff** prend en charge les mappages de types de données suivants entre les colonnes source et de destination.  
  
|Type de données sources|Type de données de destination|  
|----------------------|---------------------------|  
|**tinyint**|**smallint**, **int**ou **bigint**|  
|**smallint**|**int** ou **bigint**|  
|**Int**|**bigint**|  
|**timestamp**|**varbinary**|  
|**varchar(max)**|**text**|  
|**nvarchar(max)**|**ntext**|  
|**varbinary(max)**|**image**|  
|**texte**|**varchar(max)**|  
|**ntext**|**nvarchar(max)**|  
|**image**|**varbinary(max)**|  
  
 Utilisez l’option **-strict** pour interdire ces mappages et effectuer une validation stricte.  
  
 La table source de la comparaison doit contenir au moins une colonne de clé primaire, d'identité ou ROWGUID. Quand vous utilisez l’option **-strict** , la table de destination doit également contenir une colonne de clé primaire, d’identité ou ROWGUID.  
  
 Le script [!INCLUDE[tsql](../includes/tsql-md.md)] généré pour faire converger la table de destination n'inclut pas les types de données suivants :  
  
-   **varchar(max)**  
  
-   **nvarchar(max)**  
  
-   **varbinary(max)**  
  
-   **timestamp**  
  
-   **xml**  
  
-   **texte**  
  
-   **ntext**  
  
-   **image**  
  
## <a name="permissions"></a>Autorisations  
 Pour comparer les tables, vous avez besoin des autorisations SELECT ALL sur les objets de table comparés.  
  
 Pour utiliser l’option **-et** , vous devez être membre du rôle de base de données fixe db_owner, ou au moins disposer de l’autorisation CREATE TABLE dans la base de données d’abonnement et de l’autorisation ALTER sur le schéma du propriétaire de destination sur le serveur de destination.  
  
 Pour utiliser l’option **-dt** , vous devez être membre du rôle de base de données fixe db_owner ou disposer au moins de l’autorisation ALTER sur le schéma du propriétaire de destination sur le serveur de destination.  
  
 Pour utiliser l’option **-o** ou **-f** , vous devez disposer d’autorisations d’écriture sur l’emplacement du répertoire de fichiers spécifié.  
  
## <a name="see-also"></a> Voir aussi  
 [Comparer des tables répliquées pour identifier les différences &#40;programmation de réplication&#41;](../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)  
  
  
