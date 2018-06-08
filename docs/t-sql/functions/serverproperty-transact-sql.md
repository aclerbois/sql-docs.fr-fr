---
title: SERVERPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SERVERPROPERTY_TSQL
- SERVERPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SERVERPROPERTY function
- server instance property information [SQL Server]
- IsHadrEnabled server property
- instances of SQL Server, property information
- server properties [SQL Server]
ms.assetid: 11e166fa-3dd2-42d8-ac4b-04f18c612c4a
caps.latest.revision: 128
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a7dcb6e3ceb0ecbfb4f36b38b2e68916793d9ddc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="serverproperty-transact-sql"></a>SERVERPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des informations de propriété relatives à l'instance du serveur.  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SERVERPROPERTY ( 'propertyname' )  
```  
  
## <a name="arguments"></a>Arguments  
 *propertyname*  
 Expression contenant les informations de propriétés à retourner pour le serveur. *propertyname* peut avoir l’une des valeurs suivantes.  
  
|Propriété|Valeurs retournées|  
|--------------|---------------------|  
|BuildClrVersion|Version du CLR (Common Language Runtime) [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui a été utilisée lors de la génération de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **nvarchar(128)**|  
|Classement|Nom du classement par défaut pour le serveur.<br /><br /> NULL = Entrée non valide ou erreur.<br /><br /> Type de données de base : **nvarchar(128)**|  
|CollationID|ID du classement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Type de données de base : **int**|  
|ComparisonStyle|Style de comparaison Windows du classement.<br /><br /> Type de données de base : **int**|  
|ComputerNamePhysicalNetBIOS|Nom NetBIOS de l'ordinateur local sur lequel l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.<br /><br /> Pour une instance cluster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un cluster de basculement, cette valeur change étant donné que l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bascule sur d'autres nœuds du cluster de basculement.<br /><br /> Sur une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette valeur reste constante et retourne la même valeur que la propriété MachineName.<br /><br /> **Remarque :** Si l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se trouve dans un cluster de basculement et si vous voulez obtenir le nom de l’instance en cluster de basculement, utilisez la propriété MachineName.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **nvarchar(128)**|  
|Édition|Édition du produit installée de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez la valeur de cette propriété pour déterminer des fonctionnalités et des limites, telles que les [limites de capacité de calcul des éditions SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md). Les versions 64 bits du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ajoutent la mention (64 bits) à la version.<br /><br /> Retourne les informations suivantes :<br /><br /> « Enterprise Edition »<br /><br /> « Enterprise Edition : contrat de licence selon le nombre de cœurs »<br /><br /> « Enterprise Evaluation Edition »<br /><br /> « Business Intelligence Edition »<br /><br /> « Developer Edition »<br /><br /> « Express Edition »<br /><br /> « Express Edition with Advanced Services »<br /><br /> « Standard Edition »<br /><br /> « Web Edition »<br /><br /> « SQL Azure » indique [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ou [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> Type de données de base : **nvarchar(128)**|  
|EditionID|EditionID représente l'édition de produit installée de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez la valeur de cette propriété pour déterminer des fonctionnalités et des limites, telles que les [limites de capacité de calcul des éditions SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).<br /><br /> 1804890536 = Enterprise<br /><br /> 1872460670 = Enterprise Edition : contrat de licence selon le nombre de cœurs<br /><br /> 610778273 = Enterprise Evaluation<br /><br /> 284895786 = Business Intelligence<br /><br /> -2117995310 = Developer<br /><br /> -1592396055 = Express<br /><br /> -133711905 = Express with Advanced Services<br /><br /> -1534726760 = Standard<br /><br /> 1293598313 = Web<br /><br /> 1674378470 = SQL Database et SQL Data Warehouse<br /><br /> Type de données de base : **bigint**|  
|EngineEdition|Édition du [!INCLUDE[ssDE](../../includes/ssde-md.md)] de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installée sur le serveur.<br /><br /> 1 = Personal ou Desktop Engine (non disponible dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures)<br /><br /> 2 = Standard (valeur retournée pour Standard, Web et Business Intelligence)<br /><br /> 3 = Enterprise (valeur retournée pour les éditions Evaluation, Developer et les deux éditions Enterprise.)<br /><br /> 4 = Express (valeur retournée pour Express, Express with Tools et Express with Advanced Services)<br /><br /> 5 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<br /><br /> 6 - [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 8 = Managed Instance<br /><br /> Type de données de base : **int**|  
|HadrManagerStatus|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Indique si le gestionnaire [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] a démarré.<br /><br /> 0 = Non démarré, en attente de communication<br /><br /> 1 = Démarré et en cours d'exécution<br /><br /> 2 = Non démarré et en état d'échec<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.|  
|InstanceDefaultDataPath|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Nom du chemin par défaut jusqu’aux fichiers de données d’instance.|  
|InstanceDefaultLogPath|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Nom du chemin par défaut jusqu’aux fichiers journaux d’instance.|  
|InstanceName|Nom de l'instance à laquelle l'utilisateur est connecté.<br /><br /> Retourne la valeur NULL si le nom de l'instance est celui de l'instance par défaut, et en cas d'entrée incorrecte ou d'erreur.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **nvarchar(128)**|  
|IsAdvancedAnalyticsInstalled|Retourne 1 si la fonctionnalité Analyse avancée a été installée pendant l’installation ; 0 si la fonctionnalité Analyse avancée n’a pas été installée.|  
|IsClustered|L'instance de serveur est configurée dans un cluster de basculement.<br /><br /> 1 = Ordonné en clusters<br /><br /> 0 = Non cluster<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**|  
|IsFullTextInstalled|Les composants d'indexation sémantique et de texte intégral sont installés sur l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Les composants d'indexation sémantique et de texte intégral sont installés.<br /><br /> 0 = Les composants d'indexation sémantique et de texte intégral ne sont pas installés.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**|  
|IsHadrEnabled|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] est activé sur cette instance de serveur.<br /><br /> 0 = La fonctionnalité [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] est désactivée.<br /><br /> 1 = La fonctionnalité [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] est activée.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**<br /><br /> Pour les réplicas de disponibilité à créer et exécuter sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le service [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] doit être activé sur l'instance de serveur. Pour plus d’informations, consultez [Activer et désactiver les groupes de disponibilité AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).<br /><br /> **Remarque :** La propriété IsHadrEnabled se rapporte uniquement aux [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. D'autres fonctionnalités haute disponibilité ou de récupération d'urgence, telles que la mise en miroir de bases de données ou la copie des journaux de transaction, ne sont pas affectées par cette propriété du serveur.|  
|IsIntegratedSecurityOnly|Le serveur fonctionne en mode de sécurité intégrée.<br /><br /> 1 = Sécurité intégrée (authentification Windows)<br /><br /> 0 = Sécurité non intégrée. (Authentification Windows et authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].)<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**|  
|IsLocalDB|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Le serveur est une instance de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.|  
|IsPolybaseInstalled|**S'applique à**: [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Indique si la fonctionnalité PolyBase est installée sur l’instance de serveur.<br /><br /> 0 = Polybase n’est pas installée.<br /><br /> 1 = Polybase est installée.<br /><br /> Type de données de base : **int**|  
|IsSingleUser|Le serveur est en mode mono-utilisateur.<br /><br /> 1 = Utilisateur unique<br /><br /> 0 = Utilisateur non unique<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**|  
|IsXTPSupported|**S’applique à** : SQL Server ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[ssSDS](../../includes/sssds-md.md)].<br /><br /> Le serveur prend en charge OLTP en mémoire.<br /><br /> 1 = Le serveur prend en charge OLTP en mémoire.<br /><br /> 0 = Le serveur ne prend pas en charge OLTP en mémoire.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**|  
|LCID|Identificateur des paramètres régionaux (LCID) Windows du classement.<br /><br /> Type de données de base : **int**|  
|LicenseType|Inutilisé. Les informations de licence ne sont pas conservées ou ne sont pas gérées par le produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Retourne toujours DISABLED.<br /><br /> Type de données de base : **nvarchar(128)**|  
|MachineName|Nom de l'ordinateur Windows sur lequel s'exécute l'instance du serveur.<br /><br /> Dans le cas d'une instance en cluster, instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécutant sur un serveur virtuel sous Microsoft Cluster Service, le nom du serveur virtuel est retourné.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **nvarchar(128)**|  
|NumLicenses|Inutilisé. Les informations de licence ne sont pas conservées ou ne sont pas gérées par le produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Retourne toujours la valeur Null.<br /><br /> Type de données de base : **int**|  
|ProcessID|ID de processus du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. ProcessID permet d'identifier le fichier sqlservr.exe qui appartient à cette instance.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.<br /><br /> Type de données de base : **int**|  
|ProductBuild|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] depuis octobre 2015.<br /><br /> Numéro de build.|  
|ProductBuildType|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Type de build de la build actuelle.<br /><br /> Retourne l'une des valeurs suivantes :<br /><br /> OD = Version à la demande pour un client spécifique.<br /><br /> GDR = Correctif logiciel grand public publié via Windows Update.<br /><br /> NULL<br />= Non applicable.|  
|ProductLevel|Niveau de la version de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Retourne l'une des valeurs suivantes :<br /><br /> « 'RTM » = Version d'origine<br /><br /> 'SP*n*' = version du Service Pack<br /><br /> 'CTP*n*', = version CTP (Community Technology Preview)<br /><br /> Type de données de base : **nvarchar(128)**|  
|ProductMajorVersion|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Version principale.|  
|ProductMinorVersion|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Version mineure.|  
|ProductUpdateLevel|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Niveau de mise à jour de la build actuelle. CU indique une mise à jour cumulative.<br /><br /> Retourne l'une des valeurs suivantes :<br /><br /> CU*n* = mise à jour cumulative<br /><br /> NULL<br />= Non applicable.|  
|ProductUpdateReference|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à la version actuelle dans les mises à jour depuis fin 2015.<br /><br /> Article de la Base de connaissances pour cette version.|  
|ProductVersion|Version de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sous la forme de « *major.minor.build.revision* ».<br /><br /> Type de données de base : **nvarchar(128)**|  
|ResourceLastUpdateDateTime|Retourne la date et l'heure de la dernière mise à jour de la base de données des ressources.<br /><br /> Type de données de base : **datetime**|  
|ResourceVersion|Retourne la base de données des ressources de versions.<br /><br /> Type de données de base : **nvarchar(128)**|  
|ServerName|Informations relatives à l'instance et au serveur Windows, associées à une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = Entrée non valide ou erreur.<br /><br /> Type de données de base : **nvarchar(128)**|  
|SqlCharSet|ID du jeu de caractères SQL provenant de l'ID de classement<br /><br /> Type de données de base : **tinyint**|  
|SqlCharSetName|Nom du jeu de caractères SQL provenant du classement<br /><br /> Type de données de base : **nvarchar(128)**|  
|SqlSortOrder|ID d'ordre de tri SQL provenant du classement<br /><br /> Type de données de base : **tinyint**|  
|SqlSortOrderName|Nom de l'ordre de tri SQL provenant du classement.<br /><br /> Type de données de base : **nvarchar(128)**|  
|FilestreamShareName|Nom du partage utilisé par FILESTREAM.<br /><br /> NULL = Entrée non valide, non applicable, ou erreur.|  
|FilestreamConfiguredLevel|Niveau configuré d'accès de FILESTREAM. Pour plus d’informations, consultez [Niveau d’accès filestream](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).|  
|FilestreamEffectiveLevel|Niveau effectif d'accès de FILESTREAM. Cette valeur peut être différente de FilestreamConfiguredLevel si le niveau a changé ou si un redémarrage de l'instance ou de l'ordinateur est en attente. Pour plus d’informations, consultez [Niveau d’accès filestream](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).|  
  
## <a name="return-types"></a>Types de retour  
 **sql_variant**  
  
## <a name="remarks"></a>Notes   
  
### <a name="servername-property"></a>Propriété ServerName  
 La propriété `ServerName` de la fonction `SERVERPROPERTY` et [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) retournent des informations similaires. La propriété `ServerName` fournit le serveur et le nom de l'instance Windows qui constituent ensemble l'instance de serveur unique. [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) fournit le nom du serveur local actuellement configuré.  
  
 La propriété `ServerName` et [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) retournent les mêmes informations si le nom de serveur par défaut au moment de l’installation n’a pas été changé. Le nom de serveur local peut être configuré en exécutant la commande suivante :  
  
```  
EXEC sp_dropserver 'current_server_name';  
GO  
EXEC sp_addserver 'new_server_name', 'local';  
GO  
```  
  
 Si le nom par défaut du serveur local a été modifié lors de l’installation, [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) retourne le nouveau nom.  
  
### <a name="version-properties"></a>Propriétés de version  
 La fonction `SERVERPROPERTY` retourne des propriétés individuelles qui sont en rapport avec les informations de version, alors que la fonction [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md) combine la sortie en une seule chaîne. Si votre application requiert des chaînes de propriété individuelles, vous pouvez utiliser la fonction `SERVERPROPERTY` pour les retourner au lieu d’analyser les résultats de [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md).  

> [!NOTE]  
> Nous savons que les propriétés de version retournées par SERVERPROPERTY pour Azure SQL Database sont incorrectes. La version du moteur de base de données SQL Server exécutée par Azure SQL Database est toujours ultérieure à la version locale de SQL Server, et inclut les derniers correctifs de sécurité. Cela signifie que le niveau du correctif est toujours égal ou supérieur à celui de la version locale de SQL Server, et que les dernières fonctionnalités disponibles dans SQL Server sont disponibles dans Azure SQL Database.
>
> Pour déterminer l’édition du moteur par programmation, utilisez SELECT SERVERPROPERTY('EngineEdition'). Cette requête retourne « 5 » pour les bases de données autonomes et « 8 » pour les instances gérées dans Azure SQL Database. 
>
> Nous mettrons à jour la documentation lorsque ce problème aura été résolu.

## <a name="permissions"></a>Autorisations

Tous les utilisateurs peuvent interroger les propriétés du serveur. 
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise la fonction `SERVERPROPERTY` dans une instruction `SELECT` pour retourner des informations sur l’instance actuelle de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].   
  
```  
SELECT  
  SERVERPROPERTY('MachineName') AS ComputerName,
  SERVERPROPERTY('ServerName') AS InstanceName,  
  SERVERPROPERTY('Edition') AS Edition,
  SERVERPROPERTY('ProductVersion') AS ProductVersion,  
  SERVERPROPERTY('ProductLevel') AS ProductLevel;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  
  
  
