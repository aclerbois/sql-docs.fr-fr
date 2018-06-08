---
title: Utilisateurs de base de données à relation contenant-contenu - Rendre votre base de données portable | Microsoft Docs
ms.custom: ''
ms.date: 03/05/2018
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
- contained database, users
- user [SQL Server], about contained database users
ms.assetid: e57519bb-e7f4-459b-ba2f-fd42865ca91d
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 84870e10dc63e06dc9fafa158caba8d43fe15672
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="contained-database-users---making-your-database-portable"></a>Utilisateurs de base de données à relation contenant-contenu - Rendre votre base de données portable
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Faites appel à des utilisateurs de base de données à relation contenant-contenu pour authentifier les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)] au niveau de la base de données. Une base de données à relation contenant-contenu est une base de données qui est isolée d'autres bases de données et de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/[!INCLUDE[ssSDS](../../includes/sssds-md.md)] (et la base de données MASTER) qui héberge la base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les utilisateurs de base de données pour Windows et l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous utilisez [!INCLUDE[ssSDS](../../includes/sssds-md.md)], associez les utilisateurs de base de données à relation contenant-contenu à des règles de pare-feu au niveau de la base de données. Cette rubrique examine les avantages liés à l'utilisation du modèle de base de données à relation contenant-contenu et les différences qu'il présente par rapport au modèle traditionnel de connexion/utilisateur et aux règles de pare-feu Windows ou au niveau du serveur. Le recours au modèle traditionnel de connexion/utilisateur et aux règles de pare-feu au niveau du serveur peut encore s'avérer nécessaire pour mettre en œuvre une logique métier d'application, des critères de facilité de gestion ou des scénarios spécifiques.  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] travaille actuellement à faire évoluer le service [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et à améliorer les contrats de niveau de service garantis. À l'avenir, vous devrez peut-être basculer vers le modèle d'utilisateurs de base de données à relation contenant-contenu et vers les règles de pare-feu étendues à la base de données pour bénéficier d'un contrat de niveau de service offrant une meilleure disponibilité et d'une fréquence de connexion maximale plus élevée pour une base de données particulière. [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous invite à prendre en compte ces changements dès aujourd'hui.  
  
## <a name="traditional-login-and-user-model"></a>Connexion traditionnelle et modèle utilisateur  
 Dans le modèle traditionnel de connexion, les utilisateurs Windows ou les membres des groupes Windows se connectent à [!INCLUDE[ssDE](../../includes/ssde-md.md)] en fournissant des informations d'identification utilisateur ou groupe authentifiées par Windows. Ou vous pouvez fournir un nom et un mot de passe et vous connecter par le biais de l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans les deux cas, la base de données MASTER doit avoir une connexion qui correspond aux informations d'identification de connexion. Après que [!INCLUDE[ssDE](../../includes/ssde-md.md)] a confirmé les informations d'identification de l'authentification Windows ou a authentifié les informations d'identification de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la connexion tente généralement de se connecter à une base de données utilisateur. Pour vous connecter à une base de données utilisateur, la connexion doit pouvoir être mappée (autrement dit, associée) à un utilisateur de base de données de la base de données utilisateur. La chaîne de connexion peut également spécifier la connexion à une base de données spécifique, facultative dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mais obligatoire dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 L'important est que la connexion (de la base de données MASTER) et l'utilisateur (de la base de données utilisateur) doivent exister et être liés entre eux. Cela signifie que la connexion à la base de données utilisateur a une dépendance à l’égard de la connexion de la base de données MASTER, ce qui limite la capacité de la base de données à être déplacée vers un autre serveur d’hébergement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] . Et si, pour une raison quelconque, une connexion à la base de données MASTER n'est pas disponible (par exemple, un basculement en cours), le temps global de connexion augmente ou la connexion peut expirer. Il peut en résulter une réduction de l'extensibilité de la connexion.  
  
## <a name="contained-database-user-model"></a>Modèle utilisateur de base de données à relation contenant-contenu  
 Dans le modèle utilisateur de la base de données à relation contenant-contenu, la connexion de la base de données MASTER n'est pas présente. Au lieu de cela, le processus d'authentification se produit sur la base de données utilisateur, et l'utilisateur de base de données de la base de données utilisateur n'a pas de connexion associée dans la base de données MASTER. Le modèle utilisateur de base de données à relation contenant-contenu prend en charge l’authentification Windows et l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et il peut être utilisé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Pour vous connecter en tant qu'utilisateur de base de données à relation contenant-contenu, la chaîne de connexion doit toujours contenir un paramètre de la base de données utilisateur afin que [!INCLUDE[ssDE](../../includes/ssde-md.md)] sache quelle base de données est chargée de gérer le processus d'authentification. L'activité de l'utilisateur de base de données à relation contenant-contenu étant limitée à l'authentification de base de données, lorsque vous vous connectez en tant qu' utilisateur de base de données à relation contenant-contenu, le compte d'utilisateur base de données doit être créé indépendamment dans chaque base de données dont l'utilisateur a besoin. Pour modifier les bases de données, les utilisateurs [!INCLUDE[ssSDS](../../includes/sssds-md.md)] doivent créer une connexion. Les utilisateurs de base de données à relation contenant-contenu dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent modifier les bases de données si un même utilisateur est présent dans une autre base de données.  
  
**Azure :** [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] et [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] prennent en charge les identités Azure Active Directory en tant qu’utilisateurs de base de données à relation contenant-contenu. [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] prend en charge les utilisateurs de base de données à relation contenant-contenu utilisant l’authentification [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , à la différence de [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] . Pour plus d’informations, voir [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/). Lorsque vous utilisez l’authentification Azure Active Directory, les connexions à partir de SSMS peut être établies à l’aide de l’authentification Active Directory universelle.  Les administrateurs peuvent configurer une authentification universelle pour exiger l’authentification multifacteur, qui vérifie l’identité à l’aide d’un appel téléphonique, d’un SMS, d’une carte à puce avec code confidentiel ou d’une notification d’application mobile. Pour plus d’informations, consultez [Prise en charge SSMS pour Azure AD MFA avec SQL Database et SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/).  
  
 Pour [!INCLUDE[ssSDS](../../includes/sssds-md.md)] et [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], dans la mesure où le nom de la base de données est toujours requis dans la chaîne de connexion, aucune modification n’est requise sur la chaîne de connexion lorsque vous passez du modèle traditionnel au modèle utilisateur de base de données à relation contenant-contenu. Pour les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le nom de la base de données doit être ajoutée à la chaîne de connexion, s’il n'est pas déjà présent.  
  
> [!IMPORTANT]  
>  Lorsque vous utilisez le modèle traditionnel, les rôles de niveau serveur et les autorisations de niveau serveur peuvent limiter l'accès à toutes les bases de données. Lorsque vous utilisez le modèle de base de données à relation contenant-contenu, les propriétaires et les utilisateurs de base de données ayant l'autorisation ALTER ANY USER peuvent accorder l'accès à la base de données. Cela réduit le contrôle d'accès des connexions serveur dotées de privilèges élevés et étend le contrôle d'accès pour inclure les utilisateurs de base de données à privilèges élevés.  
  
## <a name="firewalls"></a>Pare-feux  
  
### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Les règles de pare-feu Windows s'appliquent à toutes les connexions et ont les mêmes effets sur les connexions (connexions de modèle traditionnel) et les utilisateurs de base de données à relation contenant-contenu. Pour plus d'informations sur le pare-feu Windows, consultez [Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
### <a name="includesssdsincludessssds-mdmd-firewalls"></a>[!INCLUDE[ssSDS](../../includes/sssds-md.md)] Pare-feux  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] permet des règles de pare-feu distinctes pour les connexions au niveau serveur (connexions) et pour les connexions au niveau base de données (utilisateurs de base de données à relation contenant-contenu). Quand vous vous connectez à une base de données utilisateur, les règles de pare-feu au niveau de la base de données sont contrôlées en premier. Si aucune règle n'autorise l'accès à la base de données, les règles de pare-feu au niveau serveur sont contrôlées, ce qui nécessite un accès à la base de données MASTER du serveur logique. L'association de règles de pare-feu au niveau de la base de données et d'utilisateurs de base de données à relation contenant-contenu peut éliminer la nécessité d'accéder à la base de données MASTER du serveur pendant la connexion, d'où un avantage potentiel en matière d'extensibilité de la connexion.  
  
 Pour plus d'informations sur les règles de pare-feu [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , consultez les rubriques suivantes :  
  
-   [Pare-feu Azure SQL Database](http://msdn.microsoft.com/library/azure/ee621782.aspx)  
  
-   [Procédure : configurer les paramètres de pare-feu (Azure SQL Database)](http://msdn.microsoft.com/library/azure/jj553530.aspx)  
  
-   [sp_set_firewall_rule &#40;Base de données Azure SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
  
-   [sp_set_database_firewall_rule &#40;Base de données Azure SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
  
## <a name="syntax-differences"></a>Différences de syntaxe  
  
|Modèle traditionnel|Modèle utilisateur de base de données à relation contenant-contenu|  
|-----------------------|-----------------------------------|  
|Si une connexion est établie à la base de données MASTER :<br /><br /> `CREATE LOGIN login_name  WITH PASSWORD = 'strong_password';`<br /><br /> Ensuite, si une connexion est établie à une base de données utilisateur :<br /><br /> `CREATE USER 'user_name' FOR LOGIN 'login_name';`|Si une connexion est établie à une base de données utilisateur :<br /><br /> `CREATE USER user_name  WITH PASSWORD = 'strong_password';`|  
  
|Modèle traditionnel|Modèle d'utilisateurs de base de données à relation contenant-contenu|  
|-----------------------|-----------------------------------|  
|Pour modifier un mot de passe dans le contexte d'une base de données MASTER :<br /><br /> `ALTER LOGIN login_name  WITH PASSWORD = 'strong_password';`|Pour modifier un mot de passe dans le contexte d'une base de données utilisateur :<br /><br /> `ALTER USER user_name  WITH PASSWORD = 'strong_password';`|  
  
## <a name="remarks"></a>Notes   
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les utilisateurs de base de données à relation contenant-contenu doivent être activés pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [contained database authentication Server Configuration Option](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md).  
  
-   Les connexions et les utilisateurs de base de données à relation contenant-contenu sans chevauchement de noms peuvent coexister dans vos applications.  
  
-   S’il existe une connexion dans la base de données MASTER avec le nom **nom1** et que vous créez un utilisateur de base de données à relation contenant-contenu nommé **nom1**, lorsqu'un nom de base de données est fourni dans la chaîne de connexion, le contexte de l'utilisateur de base de données est choisi à la place du contexte de connexion lors de la connexion à la base de données. Autrement dit, les utilisateurs de base de données à relation contenant-contenu sont prioritaires sur les connexions portant le même nom.  
  
-   Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , le nom de l'utilisateur de base de données à relation contenant-contenu ne peut pas être le même que celui du compte d'administrateur du serveur.  
  
-   Le compte d'administrateur du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ne peut jamais être un utilisateur de base de données à relation contenant-contenu. L'administrateur du serveur dispose d'autorisations suffisantes pour créer et gérer les utilisateurs de base de données à relation contenant-contenu. L'administrateur du serveur peut accorder des autorisations aux utilisateurs de base de données à relation contenant-contenu sur les bases de données utilisateur.  
  
-   Étant donné que les utilisateurs de base de données à relation contenant-contenu sont des principaux au niveau de la base de données, vous devez créer ces utilisateurs dans chaque base de données où vous souhaitez les utiliser. L'identité est limitée à la base de données. En ce sens, elle est indépendante en tous points de l'identité d'un utilisateur possédant un nom et un mot de passe identiques dans une autre base de données située sur le même serveur.  
  
-   Définissez des mots de passe avec un niveau de force semblable à celui des mots de passe utilisés normalement pour les connexions.  
  
## <a name="see-also"></a> Voir aussi  
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)   
 [Meilleures pratiques de sécurité recommandées avec les bases de données à relation contenant-contenu](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/)  
  
  
