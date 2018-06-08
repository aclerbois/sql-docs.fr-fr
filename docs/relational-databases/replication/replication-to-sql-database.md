---
title: Réplication vers une base de données SQL | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Database replication
- replication, SQL Database
ms.assetid: e8484da7-495f-4dac-b38e-bcdc4691f9fa
caps.latest.revision: 15
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5e3099ebb18883a99b5815b3db3e40f178b22234
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-to-sql-database"></a>Réplication vers une base de données SQL
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Vous pouvez configurer une réplication de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
 
 ### <a name="supported-configurations"></a>**Configurations prises en charge :**  
 -  La [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée localement ou une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée dans une machine virtuelle Azure dans le cloud. Pour plus d’informations, consultez [Vue d’ensemble de SQL Server sur les machines virtuelles Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/).  
 - [!INCLUDE[ssSDS](../../includes/sssds-md.md)] doit être un abonné par émission de données d’un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
 -  La base de données de distribution et les agents de réplication ne peut pas être placés sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
 - Seules les réplications d’instantané et les réplications transactionnelles monodirectionnelles sont prises en charge. Les réplications transactionnelles d’égal à égal et les réplications de fusion ne sont pas prises en charge.  
 
 ## <a name="versions"></a>Versions  
 - Le serveur de publication et le serveur de distribution doivent exécuter l’une des versions suivantes ou une version ultérieure :  
   
 -   [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)]  
 
 -   [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
   
 -   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP1 CU3  
   
 -   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] RTM CU10  
   
 -   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 CU8  
   
 -   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] attendu pour SP3  
   
 - Une tentative de configuration de la réplication à l’aide d’une version antérieure peut entraîner les erreurs MSSQL_REPL20084 (Le processus n’a pas pu se connecter à l’abonné.) et MSSQL_REPL40532 (Impossible d’ouvrir le serveur \<nom> requis pour la connexion. La connexion a échoué.).  
 
 - l’abonné [!INCLUDE[ssSDS](../../includes/sssds-md.md)] doit exécuter la version V12 ou une version ultérieure et peut être situé dans n’importe quelle région.  
   
 - Pour utiliser toutes les fonctionnalités de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vous devez utiliser les dernières versions de [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) et [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx).  
   
 ## <a name="remarks"></a>Notes   
 - La réplication peut être configurée à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou en exécutant des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sur le serveur de publication. Vous ne pouvez pas configurer la réplication à l’aide du portail [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .  
   
 - La réplication peut uniquement utiliser des connexions d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
   
 - La table répliquée doit disposer d’une clé primaire.  
   
 - Vous devez disposer d’un abonnement Azure et de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] V12.  
   
 - Une seule publication sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut prendre en charge à la fois des abonnés [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (instance exécutée localement et instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée dans une machine virtuelle Azure).  
   
 - Les opérations de gestion, de surveillance et de dépannage de la réplication doivent être effectuées à partir de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]exécutée localement.  
   
 - Seuls les abonnements par émission de données à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] sont pris en charge.  
   
 - Seul `@subscriber_type = 0` est pris en charge dans **sp_addsubscription** pour la base de données SQL.  
   
 - [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ne prend pas en charge la réplication bidirectionnelle, immédiate, pouvant être mise à jour ou d’égal à égal.      
   
 ## <a name="replication-architecture"></a>Architecture de réplication  
 ![replication-to-sql-database](../../relational-databases/replication/media/replication-to-sql-database.png "replication-to-sql-database")  
   
 ## <a name="scenarios"></a>Scénarios  
   
 ### <a name="typical-replication-scenario"></a>Scénario de réplication classique  
   
 1.  Créez une publication de réplication transactionnelle sur une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale.  
   
 2.  Dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale, utilisez l’ **Assistant Nouvel abonnement** ou des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] pour créer un abonnement par émission de données à [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
   
 3.  Le jeu de données initial est généralement un instantané créé par l’Agent d’instantané puis distribué et appliqué par l’Agent de distribution. Le jeu de données initial peut être fourni par une sauvegarde ou un autre moyen, par exemple [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
   
 #### <a name="data-migration-scenario"></a>Scénario de migration de données  
   
 1.  Utilisez la réplication transactionnelle pour répliquer des données depuis une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale vers [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
   
 2.  Redirigez les applications clientes ou de couche intermédiaire pour mettre à jour la copie [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .  
   
 3.  Arrêtez la mise à jour de la version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la table et supprimez la publication.  
   
 ## <a name="limitations"></a>Limitations  
  Les options suivantes ne sont pas prises en charge pour les abonnements [!INCLUDE[ssSDS](../../includes/sssds-md.md)] :  
   
 -   Copier l’association de groupes de fichiers  
   
 -   Copier les schémas de partition de table  
   
 -   Copier les schémas de partition d’index  
   
 -   Copier les statistiques définies par l’utilisateur  
   
 -   Copier les liaisons par défaut  
   
 -   Copier les liaisons de règle  
   
 -   Copier les index de texte intégral  
   
 -   Copier le schéma XSD XML  
   
 -   Copier les index XML  
   
 -   Copier les autorisations  
   
 -   Copier les index spatiaux  
   
 -   Copier les index filtrés  
   
 -   Copier l’attribut de compression de données  
   
 -   Copier l’attribut de colonne éparse  
   
 -   Convertir filestream en types de données MAX  
   
 -   Convertir hierarchyId en types de données MAX  
   
 -   Convertir le type spatial en types de données MAX  
   
 -   Copier les propriétés étendues  
   
 -   Copier les autorisations  
   
### <a name="limitations-to-be-determined"></a>Limitations à déterminer 
 
 -   Copier le classement  
   
 -   Exécution de la procédure stockée dans une transaction sérialisée  
   
 ## <a name="examples"></a>Exemples  
  Créer une publication et un abonnement par émission de données. Pour plus d'informations, consultez :  
   
 -   [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)  
   
 -   [Créer un abonnement par extraction](../../relational-databases/replication/create-a-push-subscription.md) en utilisant le nom de serveur logique [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] comme abonné (par exemple, **N’azuresqldbdns.database.windows.net’**) et le nom [!INCLUDE[ssSDS](../../includes/sssds-md.md)] comme base de données de destination (par exemple, **AdventureWorks**).  
   
 ## <a name="see-also"></a> Voir aussi  
 - [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 - [Créer un abonnement par émission (push)](../../relational-databases/replication/create-a-push-subscription.md)   
 - [Types de réplication](../../relational-databases/replication/types-of-replication.md)   
 - [Surveillance &#40;réplication&#41;](../../relational-databases/replication/monitor/monitoring-replication.md)   
 - [Initialiser un abonnement](../../relational-databases/replication/initialize-a-subscription.md)  
