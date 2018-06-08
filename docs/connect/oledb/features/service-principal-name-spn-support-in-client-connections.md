---
title: Prise en charge du nom de Principal du service (SPN) dans les connexions clientes | Documents Microsoft
description: Prise en charge du nom de Principal du service (SPN) dans les connexions clientes
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, SPNs
- OLE DB, SPNs
- SPNs [SQL Server]
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: c119c14cbd0441d22b8e97f8d168d10d652e86d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="service-principal-name-spn-support-in-client-connections"></a>Prise en charge des noms de principaux du service (SPN) dans les connexions clientes
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  À partir de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], prise en charge des noms principaux de service (SPN) a été étendu pour permettre l’authentification mutuelle entre tous les protocoles. Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], noms principaux de service ont été uniquement pris en charge pour Kerberos sur TCP lorsque la valeur par défaut SPN pour le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] instance a été inscrit auprès d’Active Directory.  
  
 Noms principaux de service sont utilisés par le protocole d’authentification pour déterminer le compte dans lequel un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de l’instance s’exécute. Si le compte de l'instance est connu, l'authentification Kerberos peut être utilisée pour fournir une authentification mutuelle par le client et le serveur. Si le compte de l'instance n'est pas connu, l'authentification NTLM, qui fournit uniquement une authentification du client par le serveur, est utilisée. Actuellement, pilote OLE DB pour SQL Server effectue la recherche d’authentification faisant dériver le SPN des propriétés de connexion réseau et le nom d’instance. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tentent d’inscrire les SPN au démarrage, ou ils peuvent également être inscrits manuellement. Toutefois, l'inscription échoue si les droits d'accès sont insuffisants pour le compte qui essaie d'inscrire les noms principaux de service.  
  
 Les comptes de domaine et d'ordinateur sont inscrits automatiquement dans Active Directory. Ils peuvent être utilisés comme noms principaux de service ; par ailleurs, les administrateurs peuvent définir leurs propres noms principaux de service. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permet de sécuriser l’authentification plus gérable et plus fiable en autorisant les clients à spécifier directement le SPN à utiliser.  
  
> [!NOTE]  
>  Un nom principal de service spécifié par une application cliente est utilisé uniquement lorsqu'une connexion est établie avec la sécurité intégrée Windows.  
  
> [!TIP]  
>  **[!INCLUDE[msCoName](../../../includes/msconame-md.md)] pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** est un outil de diagnostic qui permet de dépanner les problèmes de connexion que rencontre Kerberos avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Gestionnaire de configuration de Microsoft Kerberos pour SQL Server](http://www.microsoft.com/download/details.aspx?id=39046).  
  
 Pour plus d'informations sur Kerberos, consultez les articles suivants :  
  
-   [Supplément technique de Kerberos pour Windows](http://go.microsoft.com/fwlink/?LinkId=101449)  
  
-   [Microsoft Kerberos](http://go.microsoft.com/fwlink/?LinkID=100758)  
  
## <a name="usage"></a>Utilisation  
 Le tableau suivant décrit les scénarios les plus courants dans lesquels les applications clientes peuvent activer l'authentification sécurisée.  
  
|Scénario| Description|  
|--------------|-----------------|  
|Une application héritée ne spécifie pas de nom principal de service.|Ce scénario de compatibilité garantit l'absence de tout changement de comportement dans les applications développées pour les versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si aucun nom principal de service n'est spécifié, l'application s'appuie sur les noms principaux de service générés et n'a aucune connaissance de la méthode d'authentification utilisée.|  
|Une application cliente à l’aide de la version actuelle du pilote OLE DB pour SQL Server spécifie un nom principal de service dans la chaîne de connexion comme un compte d’utilisateur ou d’ordinateur de domaine, un nom principal de service spécifique à l’instance ou sous forme de chaîne définie par l’utilisateur.|Le mot clé **ServerSPN** peut être utilisé dans une chaîne de fournisseur, une chaîne d'initialisation ou une chaîne de connexion afin d'effectuer les opérations suivantes :<br /><br /> -Spécifiez le compte utilisé par le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] instance pour une connexion. Cela simplifie l'accès à l'authentification Kerberos. Si un centre de distribution de clés Kerberos (KDC) est présent et si le compte approprié est spécifié, l'utilisation de l'authentification Kerberos est plus probable que celle de l'authentification NTLM. Le centre de distribution de clés se trouve habituellement sur le même ordinateur que le contrôleur de domaine.<br /><br /> -Spécifier un SPN pour le compte de service pour rechercher le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] instance. Pour chaque [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deux par défaut noms principaux de service sont générés et peut être utilisé à cet effet, l’instance. Toutefois, il n'est pas certain que ces clés soient présentes dans Active Directory ; par conséquent, dans cette situation, l'authentification Kerberos n'est pas garantie.<br /><br /> -Spécifier un SPN qui permet de rechercher le compte de service pour le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] instance. Il peut s'agir de n'importe quelle chaîne définie par l'utilisateur et mappée au compte de service. Dans ce cas, la clé doit être inscrite manuellement dans le centre de distribution de clés et doit répondre aux exigences relatives à un nom principal de service défini par l'utilisateur.<br /><br /> Le mot clé **FailoverPartnerSPN** peut être utilisé pour spécifier le nom principal de service du serveur partenaire de basculement. La plage des valeurs de comptes et de clés Active Directory est identique à celle des valeurs que vous pouvez spécifier pour le serveur principal.|  
|Une application OLE DB spécifie un nom principal de service en tant que propriété d'initialisation d'une source de données pour le serveur principal ou pour un partenaire de basculement.|La propriété de connexion **SSPROP_INIT_SERVER_SPN** du jeu de propriétés **DBPROPSET_SQLSERVERDBINIT** peut être utilisée pour spécifier le nom principal de service d'une connexion.<br /><br /> La propriété de connexion **SSPROP_INIT_FAILOVER_PARTNER_SPN** dans **DBPROPSET_SQLSERVERDBINIT** peut être utilisée pour spécifier le nom principal de service du serveur partenaire de basculement.|   
|L'utilisateur spécifie un nom principal de service pour un serveur ou un serveur partenaire de basculement dans une boîte de dialogue **Liaison de données** ou **Connexion** OLE DB.|Le nom principal de service peut être spécifié dans une boîte de dialogue **Liaison de données** ou **Connexion** .|   
|Une application OLE DB détermine la méthode d'authentification utilisée pour établir une connexion.|Lorsqu'une connexion est ouverte avec succès, une application peut interroger la propriété de connexion **SSPROP_AUTHENTICATION_METHOD** dans le jeu de propriétés **DBPROPSET_SQLSERVERDATASOURCEINFO** pour déterminer la méthode d'authentification utilisée. Les valeurs incluent **NTLM** et **Kerberos**mais ne se limitent pas à ces dernières.|  
  
## <a name="failover"></a>Basculement  
 Les noms principaux de service ne sont pas stockés dans le cache de basculement et par conséquent ne peuvent pas être passés entre les connexions. Les noms principaux de service sont utilisés lors de toutes les tentatives de connexion au principal et au serveur partenaire lorsque cela est spécifié dans la chaîne de connexion ou les attributs de connexion.  
  
## <a name="connection-pooling"></a>Regroupement de connexions  
 Les applications doivent prendre en compte le fait que la spécification de noms principaux de service dans une partie (au lieu de la totalité) des chaînes de connexion peut contribuer à la fragmentation du regroupement.  
  
 Les applications peuvent spécifier par programme des noms principaux de service en tant qu'attributs de connexion, au lieu de spécifier des mots clés de chaînes de connexion. Cela peut permettre de gérer la fragmentation du regroupement de connexions.  
  
 Les applications doivent prendre en compte le fait que les noms principaux de service dans les chaînes de connexion peuvent être substitués en définissant les attributs de connexion correspondants ; toutefois, les chaînes de connexion utilisées par le regroupement de connexions tirent parti des valeurs de chaînes de connexion à des fins de regroupement.  
  
## <a name="down-level-server-behavior"></a>Comportement de serveur de bas niveau  
 Le nouveau comportement de connexion est implémenté par le client ; par conséquent, il n'est pas spécifique à une version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="linked-servers-and-delegation"></a>Serveurs liés et délégation  
 Lorsque des serveurs liés sont créés, le **@provstr** paramètre de [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) peut être utilisé pour spécifier le serveur et le partenaire de basculement noms principaux de service. Les avantages de ce choix sont les mêmes que pour la spécification de noms principaux de service dans les chaînes de connexion des clients : davantage de simplicité et de fiabilité pour l'établissement des connexions qui utilisent l'authentification Kerberos.  
  
 La délégation avec des serveurs liés requiert l'authentification Kerberos.  
  
## <a name="management-aspects-of-spns-specified-by-applications"></a>Aspects liés à la gestion des noms principaux de service spécifiés par les applications  
 Au moment de choisir s'il faut spécifier des noms principaux de service dans une application (via les chaînes de connexion) ou par programme via les propriétés de connexion (au lieu de recourir aux noms principaux de service par défaut générés par le fournisseur), prenez en considération les facteurs suivants :  
  
-   Sécurité : est-ce que le nom principal de service spécifié divulgue des informations protégées ?  
  
-   Fiabilité : Pour activer l’utilisation de noms principaux de service par défaut, le compte de service dans lequel le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s’exécute d’instance doit disposer des privilèges suffisants pour mettre à jour d’Active Directory sur le KDC.  
  
-   Commodité et transparence de l'emplacement : comment les noms principaux de service d'une application sont-ils affectés si la base de données correspondante est déplacée vers une autre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ? Cela s'applique à la fois au serveur principal et à son partenaire de basculement, si vous utilisez la mise en miroir de bases de données. Si une modification du serveur signifie une modification des noms principaux de service, comment cela affecte-t-il les applications ? Est-ce que les modifications sont gérées ?  
  
## <a name="specifying-the-spn"></a>Spécification du nom principal de service  
 Vous pouvez spécifier un nom principal de service dans les boîtes de dialogue et dans le code. Cette section explique comment vous pouvez spécifier un nom principal de service.  
  
 La longueur maximale d'un nom principal de service est 260 caractères.  
  
 La syntaxe que les noms principaux de service utilisent dans la chaîne de connexion ou les attributs de connexion est la suivante :  
  
|Syntaxe| Description|  
|------------|-----------------|  
|MSSQLSvc/*fqdn*|Nom principal de service par défaut, généré par le fournisseur, pour une instance par défaut lorsqu'un autre protocole que TCP est utilisé.<br /><br /> *fqdn* est un nom de domaine complet.|  
|MSSQLSvc/*fqdn*:*port*|Nom principal de service par défaut, généré par le fournisseur, lorsque le protocole TCP est utilisé.<br /><br /> *port* est un numéro de port TCP.|  
|MSSQLSvc/*fqdn*:*InstanceName*|Nom principal de service par défaut, généré par le fournisseur, pour une instance nommée lorsqu'un autre protocole que TCP est utilisé.<br /><br /> *InstanceName* est un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nom de l’instance.|  
|HOST/*fqdn*<br /><br /> HOST/*MachineName*|Nom principal de service mappé aux comptes d'ordinateur intégrés qui sont inscrits automatiquement par Windows.|  
|*Username*@*Domain*|Spécification directe d'un compte de domaine.<br /><br /> *Username* est un nom de compte d'utilisateur Windows.<br /><br /> *Domain* est un nom de domaine ou nom de domaine complet Windows.|  
|*MachineName*$@*Domain*|Spécification directe d'un compte d'ordinateur.<br /><br /> (Si le serveur auquel vous vous connectez s'exécute sous les comptes LOCAL SYSTEM ou NETWORK SERVICE, pour obtenir l'authentification Kerberos, **ServerSPN** peut être au format *MachineName*$@*Domain* .)|  
|*KDCKey*/*MachineName*|Nom principal de service spécifié par l'utilisateur.<br /><br /> *KDCKey* est une chaîne alphanumérique conforme aux règles d'une clé du centre de distribution de clés.|  
  
## <a name="ole-db-syntax-supporting-spns"></a>OLE DB syntaxe prise en charge des noms principaux de service  
 Pour plus d'informations spécifiques à la syntaxe, consultez les rubriques suivantes :  
  
-   [Noms de Principal du service &#40;SPN&#41; dans les connexions clientes &#40;OLE DB&#41;](../../oledb/ole-db/service-principal-names-spns-in-client-connections-ole-db.md)  
  
 Pour plus d'informations sur les exemples d'applications qui illustrent cette fonctionnalité, consultez [Exemples de programmabilité des données SQL Server](http://msftdpprodsamples.codeplex.com/).  
  
## <a name="see-also"></a>Voir aussi  
 [Pilote de base de données OLE pour les fonctionnalités SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Inscrire un nom de principal du service pour les connexions Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
  
