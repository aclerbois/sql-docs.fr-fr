---
title: Connexion à SQL Server (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- connecting to SQL Server 2008, SQL Server permission
- connecting to SQL Server 2008, synchronization
ms.assetid: 08233267-693e-46e6-9ca3-3a3dfd3d2be7
caps.latest.revision: 18
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d946b7c6d5fed230fab2daed139421c43d814566
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-sql-server-mysqltosql"></a>Connexion à SQL Server (MySQLToSQL)
Pour migrer des bases de données MySQL vers SQL Server, vous devez vous connecter à l’instance cible de SQL Server. Lorsque vous vous connectez, SSMA Obtient les métadonnées relatives à toutes les bases de données dans l’instance de SQL Server et affiche les métadonnées de la base de données dans l’Explorateur de métadonnées SQL Server. SSMA stocke les informations de l’instance de SQL Server que vous êtes connecté à, mais ne stockez pas les mots de passe.  
  
Votre connexion à SQL Server reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter à SQL Server si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu'à ce que le chargement des objets de base de données dans SQL Server et de migrer les données.  
  
Métadonnées relatives à l’instance de SQL Server ne sont pas synchronisée automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans l’Explorateur de métadonnées SQL Server, vous devez manuellement mettre à jour les métadonnées de SQL Server. Pour plus d’informations, consultez la section « Synchronisation des métadonnées SQL Server » plus loin dans cette rubrique.  
  
## <a name="required-sql-server-permissions"></a>Autorisations requises de SQL Server  
Le compte qui est utilisé pour se connecter à SQL Server nécessite des autorisations différentes selon les actions réalisées par le compte :  
  
-   Pour convertir des objets de MySQL vers [!INCLUDE[tsql](../../includes/tsql_md.md)] syntaxe, pour mettre à jour les métadonnées à partir de SQL Server, ou pour enregistrer la syntaxe converti pour les scripts, le compte doit avoir l’autorisation de se connecter à l’instance de SQL Server.  
  
-   Pour charger des objets de base de données dans SQL Server, la spécification de l’autorisation minimale est l’appartenance à la **db_owner** rôle de base de données dans la base de données cible.  
  
## <a name="establishing-a-sql-server-connection"></a>L’établissement d’une connexion SQL Server  
Avant de convertir des objets de base de données MySQL en syntaxe SQL, vous devez établir une connexion à l’instance de SQL Server sur lequel vous souhaitez migrer les bases de données ou la base de données MySQL.  
  
Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données où les objets et les données seront migrées. Vous pouvez personnaliser ce mappage au niveau du schéma de MySQL après la connexion à SQL Server. Pour plus d’informations, consultez [mappage de bases de données MySQL pour les schémas SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
> [!IMPORTANT]  
> Avant d’essayer de se connecter à SQL Server, assurez-vous que l’instance de SQL Server est en cours d’exécution et peut accepter des connexions.  
  
**Pour se connecter à SQL Server**  
  
1.  Sur le **fichier** menu, sélectionnez **se connecter à SQL Server** (cette option est activée après la création d’un projet).  
  
    Si vous êtes jamais connecté à SQL Server, le nom de la commande sera **se reconnecter à SQL Server**.  
  
2.  Dans la boîte de dialogue de connexion, entrez ou sélectionnez le nom de l’instance de SQL Server.  
  
    -   Si vous vous connectez à l’instance par défaut sur l’ordinateur local, vous pouvez entrer **localhost** ou un point (**.**).  
  
    -   Si vous vous connectez à l’instance par défaut sur un autre ordinateur, entrez le nom de l’ordinateur.  
  
    -   Si vous vous connectez à une instance nommée sur un autre ordinateur, entrez le nom d’ordinateur suivi d’une barre oblique inverse, puis le nom d’instance, tels que MyServer\MyInstance.  
  
3.  Si votre instance de SQL Server est configuré pour accepter les connexions sur un port non défini par défaut, entrez le numéro de port qui est utilisé pour les connexions SQL Server dans le **port du serveur** boîte. L’instance par défaut de SQL Server, le numéro de port par défaut est 1433. Pour les instances nommées, SSMA va tenter d’obtenir le numéro de port à partir du Service SQL Server Browser.  
  
4.  Dans le **authentification** , sélectionnez le type d’authentification à utiliser pour la connexion. Pour utiliser le compte Windows actuel, sélectionnez **l’authentification Windows**. Pour utiliser une connexion SQL Server, sélectionnez l’authentification SQL Server, puis indiquez le nom de connexion et un mot de passe.  
  
5.  Pour une connexion sécurisée, les deux contrôles sont ajoutés, le **chiffrer la connexion** et **TrustServerCertificate** cases à cocher. Uniquement lorsque **chiffrer la connexion** est activée, le **TrustServerCertificate** case à cocher est visible. Lorsque **chiffrer la connexion** est vérifiée (true) et **TrustServerCertificate** est désactivée (false), il valide le certificat SSL SQL Server. La validation du certificat de serveur est une partie de la négociation SSL qui garantit qu'il s'agit du serveur correct avec lequel établir une connexion. Pour ce faire, un certificat doit être installé sur le côté client, ainsi que sur le côté serveur.  
  
6.  Cliquez sur connecter.  
  
**Compatibilité de version supérieur**  
  
Il est autorisé à se connecter/se reconnecter à des versions ultérieures de SQL Server.  
  
1.  Vous serez en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet créé est [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005.  
  
2.  Vous serez en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet créé est [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008, mais il ne peut pas se connecter à des versions inférieures c'est-à-dire [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005.  
  
3.  Vous serez en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet créé est [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012.  
  
4.  Vous serez en mesure de se connecter uniquement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 lorsque le projet créé est [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014.  
  
5.  Compatibilité des versions plus élevée n’est pas valide pour « SQL Azure ».  
  
||||||||  
|-|-|-|-|-|-|-|  
|**TYPE et VERSION du serveur cible du projet**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005<br /> (Version : 9.x)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008<br /> (Version : 10.x)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012<br />(Version:11.x)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014<br />(Version:12.x)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016<br />(Version:13.x)|SQL Azure|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005|Oui|Oui|Oui|Oui|Oui||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008||Oui|Oui|Oui|Oui||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012|||Oui|Oui|Oui||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]2014||||Oui|Oui||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]2016|||||Oui||  
|SQL Azure||||||Oui|  
  
> [!IMPORTANT]  
> Conversion des objets de base de données est effectuée selon le type de projet, mais pas conformément à la version de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] connecté à. Dans le cas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] projet 2005, Conversion est effectuée en tant que par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005 même si vous êtes connecté à une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] (SQL Server 2008/SQL Server 2012/SQL Server 2014/SQL Server 2016).  
  
## <a name="synchronizing-sql-server-metadata"></a>Synchronisation des métadonnées du serveur SQL  
Les métadonnées sur les bases de données SQL Server ne sont pas automatiquement mis à jour. Les métadonnées dans l’Explorateur de métadonnées SQL Server sont un instantané des métadonnées lorsque vous tout d’abord connecté à SQL Server, ou la dernière fois que vous avez manuellement les métadonnées mises à jour. Vous pouvez mettre à jour manuellement les métadonnées pour toutes les bases de données, ou pour tout de la base de données unique ou d’un objet de base de données.  
  
**Pour synchroniser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté à SQL Server.  
  
2.  Dans l’Explorateur de métadonnées SQL Server, sélectionnez la case à cocher en regard de la base de données ou le schéma de base de données que vous souhaitez mettre à jour.  
  
    Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case en regard de bases de données.  
  
3.  Cliquez sur les bases de données, ou l’individuel de la base de données ou le schéma de base de données, puis sélectionnez **synchroniser avec la base de données**.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage entre les schémas de MySQL et des bases de données SQL Server et des schémas, consultez [mappage de bases de données MySQL pour les schémas SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
-   Pour personnaliser les options de configuration pour les projets, consultez [définition des Options de projet &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md)  
  
-   Pour personnaliser le mappage des types de données source et cible, consultez [MySQL de mappage et les Types de données SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
-   Si vous n’avez pas à effectuer ces tâches, vous pouvez convertir les définitions d’objets de base de données MySQL dans des définitions d’objets SQL Server. Pour plus d’informations, consultez [conversion des bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
