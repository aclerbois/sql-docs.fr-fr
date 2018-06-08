---
title: Modèle de sécurité de l’Agent de réplication | Microsoft Docs
ms.custom: ''
ms.date: 04/26/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Snapshot Agent, security
- agents [SQL Server replication], security
- Distribution Agent, security
- logins [SQL Server replication], permissions for agents
- security [SQL Server replication], agent security model
- Log Reader Agent, security
- Queue Reader Agent, security
- Merge Agent, security
- replication [SQL Server], agents and profiles
ms.assetid: 6d09fc8d-843a-4a7a-9812-f093d99d8192
caps.latest.revision: 72
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 162d6cb864228db80a8d7b7e7fdf0d14acd44799
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-agent-security-model"></a>Modèle de sécurité de l'Agent de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le modèle de sécurité de l'Agent de réplication permet un contrôle fin des comptes sous lesquels les agents de réplication s'exécutent et établissent des connexions : un compte distinct peut être spécifié pour chaque agent. Pour plus d’informations sur la manière de spécifier des comptes, consultez [Gérer les connexions et les mots de passe dans la réplication](../../../relational-databases/replication/security/manage-logins-and-passwords-in-replication.md).  
  
> [!IMPORTANT]  
>  Lorsqu'un membre du rôle serveur fixe **sysadmin** configure la réplication, les agents de réplication peuvent être configurés pour emprunter l'identité du compte de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Il suffit de ne pas spécifier de nom de connexion et de mot de passe pour un Agent de réplication ; toutefois, cette méthode n'est pas recommandée. Pour des raisons de sécurité, il est plutôt recommandé de spécifier un compte pour chaque agent, disposant des autorisations minimales décrites dans la section « Autorisations requises par les agents » plus loin dans cette rubrique.  
  
 Comme tous les exécutables, les agents de réplication sont exécutés dans le contexte d'un compte Windows. Les agents établissent des connexions de sécurité intégrée Windows en utilisant ces comptes. Le compte sous lequel l'agent s'exécute dépend de la manière dont l'agent est démarré :  
  
-   Démarrage de l'agent à partir d'un travail de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (par défaut) : lorsqu'un travail de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sert à démarrer un agent de réplication, l'agent s'exécute dans le contexte d'un compte que vous spécifiez lors de la configuration de la réplication. Pour plus d'informations sur l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et la réplication, consultez la section « Sécurité de l'Agent sous l'Agent SQL Server » plus loin dans cette rubrique. Pour plus d’informations sur les autorisations requises pour le compte sous lequel s’exécute l’Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Configurer l’Agent SQL Server](http://msdn.microsoft.com/library/2e361a62-9e92-4fcd-80d7-d6960f127900).  
  
-   Démarrage de l'agent à partir d'une ligne de commande MS-DOS, directement ou via un script : l'agent s'exécute dans le contexte du compte de l'utilisateur exécutant l'agent sur la ligne de commande.  
  
-   Démarrage de l'agent à partir d'une application qui utilise des objets RMO (Replication Management Objects) ou un contrôle ActiveX : l'agent s'exécute dans le contexte de l'application appelant les objets RMO ou le contrôle ActiveX.  
  
    > [!NOTE]  
    >  Les contrôles ActiveX sont déconseillés.  
  
 Il est recommandé d'établir les connexions dans le contexte de sécurité intégrée Windows. Pour des besoins de compatibilité descendante, la sécurité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut également être utilisée. Pour plus d'informations sur les meilleures pratiques, consultez [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="permissions-that-are-required-by-agents"></a>Autorisations requises par les agents  
 Les comptes sous lesquels les agents s'exécutent et établissent des connexions nécessitent différentes autorisations. Ces autorisations sont décrites dans le tableau suivant. Il est recommandé d'exécuter chaque agent sous un compte Windows différent et d'accorder au compte uniquement les autorisations nécessaires. Pour obtenir des informations sur la liste d’accès à la publication (PAL, Publication Access List), qui concerne un certain nombre d’agents, consultez [Sécuriser le serveur de publication](../../../relational-databases/replication/security/secure-the-publisher.md).  
  
> [!NOTE]  
>  La fonctionnalité Contrôle de compte d'utilisateur dans certains systèmes d'exploitation Windows peut empêcher l'accès administratif au partage de fichiers d'instantanés. Vous devez donc octroyer explicitement des autorisations sur le partage de fichiers d'instantanés aux comptes Windows qui sont utilisés par l'Agent d'instantané, l'Agent de distribution et l'Agent de fusion. Vous devez effectuer cette opération même si les comptes Windows sont membres du groupe Administrateurs. Pour plus d’informations, consultez [Sécuriser le dossier d’instantanés](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
|Agent|Autorisations|  
|-----------|-----------------|  
|Agent d'instantané|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter au serveur de distribution. Ce compte doit obligatoirement :<br /><br /> - être au moins membre du rôle de base de données fixe **db_owner** dans la base de données de distribution ;<br /><br /> - disposer des autorisations de lecture, écriture et modification sur le partage de fichiers d’instantanés.<br /><br /> <br /><br /> Notez que le compte utilisé pour *se connecter* au serveur de publication doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données de publication.|  
|l'Agent de lecture du journal ;|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter au serveur de distribution. Ce compte doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données de distribution.<br /><br /> Le compte utilisé pour se connecter au serveur de publication doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données de publication.<br /><br /> Lors de la sélection des options **sync_type** , *replication support only*, *initialize with backup*ou *initialize from lsn*, l'Agent de lecture du journal doit s'exécuter après l'exécution de **sp_addsubscription**, afin que les scripts d'installation soient écrits dans la base de données de distribution. L'Agent de lecture du journal doit s'exécuter sous un compte membre du rôle serveur fixe **sysadmin** . Lorsque l'option **sync_type** a la valeur *Automatic*, aucune action particulière de l'Agent de lecture du journal n'est requise.|  
|Agent de distribution pour un abonnement par envoi de données (push)|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter au serveur de distribution. Ce compte doit obligatoirement :<br /><br /> - être au moins membre du rôle de base de données fixe **db_owner** dans la base de données de distribution ;<br /><br /> - être membre de la liste d’accès à la publication (PAL) ;<br /><br /> - avoir les autorisations de lecture sur le partage des fichiers d’instantanés ;<br /><br /> - avoir les autorisations de lecture sur le répertoire d’installation du fournisseur OLE DB pour l’Abonné si l’abonnement est destiné à un Abonné non-SQL Server.<br /><br /> - Lors de la réplication des données LOB, l’agent de distribution doit avoir des autorisations en écriture sur la réplication **C:\Program Files\Microsoft SQL Server\XX\COMfolder** , où XX représente l’ID d’instance.<br /><br /> <br /><br /> Notez que le compte employé pour *se connecter* à l’Abonné doit au moins être membre du rôle de base de données fixe **db_owner** dans la base de données d’abonnement, ou bien disposer d’autorisations équivalentes si l’abonnement s’adresse à un Abonné non-SQL Server.<br /><br /> Notez également que, lorsque vous utilisez `-subscriptionstreams >= 2` sur l’agent de distribution, vous devez également accorder l’autorisation **View Server State** sur les abonnés pour détecter les blocages.|  
|Agent de distribution pour un abonnement par extraction de données (pull)|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter à l'Abonné. Ce compte doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement. Le compte servant à se connecter au serveur de distribution doit obligatoirement :<br /><br /> - être membre de la liste d’accès à la publication (PAL) ;<br /><br /> - avoir les autorisations de lecture sur le partage des fichiers d’instantanés ;<br /><br /> - Lors de la réplication des données LOB, l’agent de distribution doit avoir des autorisations en écriture sur la réplication **C:\Program Files\Microsoft SQL Server\XX\COMfolder** , où XX représente l’ID d’instance.<br /><br /> <br /><br /> Notez que, lorsque vous utilisez `-subscriptionstreams >= 2` sur l’agent de distribution, vous devez également accorder l’autorisation **View Server State** sur les abonnés pour détecter les blocages.|  
|Agent de fusion pour un abonnement par envoi de données (push)|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter au serveur de publication et au serveur de distribution. Ce compte doit obligatoirement :<br /><br /> - Être au moins membre du rôle de base de données fixe **db_owner** dans la base de données de distribution<br /><br /> - Être membre de la liste d’accès à la publication (PAL).<br /><br /> - Être un compte de connexion associé à un utilisateur ayant des autorisations de lecture-écriture dans la base de données de publication.<br /><br /> - Avoir les autorisations de lecture sur le partage des fichiers d’instantanés.<br /><br /> <br /><br /> Notez que le compte utilisé pour *se connecter* à l’Abonné doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement.|  
|Agent de fusion pour un abonnement par extraction de données (pull)|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter à l'Abonné. Ce compte doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement. Le compte servant à se connecter au serveur de publication et au serveur de distribution doit obligatoirement :<br /><br /> - Être membre de la liste d’accès à la publication (PAL).<br /><br /> - Être un compte de connexion associé à un utilisateur ayant des autorisations de lecture-écriture dans la base de données de publication.<br /><br /> - Être un compte de connexion associé à un utilisateur enregistré dans la base de données de distribution. L'utilisateur peut être un **Guest**<br /><br /> - Avoir les autorisations de lecture sur le partage des fichiers d’instantanés|  
|Agent de lecture de la file d'attente|Le compte Windows sous lequel s'exécute l'agent est utilisé pour se connecter au serveur de distribution. Ce compte doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données de distribution.<br /><br /> Le compte utilisé pour se connecter au serveur de publication doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données de publication.<br /><br /> Le compte utilisé pour se connecter à l'Abonné doit être au minimum membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement.|  
  
## <a name="agent-security-under-sql-server-agent"></a>Sécurité de l'Agent dans l'Agent SQL Server  
 Lorsque vous configurez la réplication à l'aide de procédures [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou RMO, un travail de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est créé par défaut pour chaque agent. Les agents s'exécutent ensuite dans le contexte d'une étape de travail, qu'ils s'exécutent en continu, dans le cadre d'une planification ou à la demande. Vous pouvez afficher ces travaux dans le dossier **Jobs** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Le tableau suivant donne la liste des noms des travaux.  
  
|Agent|Nom du travail|  
|-----------|--------------|  
|Agent d'instantané|**\<ServeurPublication>-\<BasededonnéesPublication>-\<Publication>-\<entier>**|  
|Agent d'instantané pour une partition de publication de fusion|**Dyn_\<ServeurPublication>-\<BasededonnéesPublication>-\<Publication>-\<GUID>**|  
|l'Agent de lecture du journal ;|**\<ServeurPublication>-\<BasededonnéesPublication>-\<entier>**|  
|Agent de fusion pour les abonnements extraits|**\<Serveur_Publication>-\<Base_de_données_Publication>-\<Publication>-\<Abonné>-\<Base_de_données_Abonnement>-\<entier>**|  
|Agent de fusion pour abonnements par envoi de données (push)|**\<ServeurPublication>-\<BasededonnéesPublication>-\<Publication>-\<Abonné>-\<entier>**|  
|Agent de distribution pour abonnements par envoi de données (push)|**\<ServeurPublication>-\<BasededonnéesPublication>-\<Publication>-\<Abonné>-\<entier>***|  
|Agent de distribution pour abonnements par extraction de données (pull)|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<GUID>***\*|  
|Agent de distribution pour les abonnements par envoi de données aux Abonnés non SQL Server|**\<ServeurPublication>-\<BasededonnéesPublication>-\<Publication>-\<Abonné>-\<entier>**|  
|Agent de lecture de la file d'attente|**[\<Distributeur>].\<entier>**|  
  
 \*Pour les abonnements par envoi de données aux publications Oracle, le nom du travail est **\<ServeurPublication>-\<ServeurPublication**> au lieu de **\<ServeurPublication>-\<BasededonnéesPublication>**.  
  
 \*\*Pour les abonnements par extraction de données aux publications Oracle, le nom du travail est **\<ServeurPublication>-\<BasededonnéesDistribution**> au lieu de **\<ServeurPublication>-\<BasededonnéesPublication>**.  
  
 Lorsque vous configurez la réplication, vous spécifiez les comptes sous lesquels les agents doivent être exécutés. Cependant, toutes les étapes de travail s'exécutent dans le contexte de sécurité d'un *proxy*; la réplication effectue donc les mappages suivants en interne pour les comptes d'agent spécifiés :  
  
-   Le compte est d'abord mappé à des informations d'identification à l'aide de l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] [CREATE CREDENTIAL](../../../t-sql/statements/create-credential-transact-sql.md) . Les proxys de l'Agent[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilisent les informations d'identification pour stocker les informations relatives aux comptes d'utilisateur Windows.  
  
-   La procédure stockée [sp_add_proxy](../../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md) est appelée et les informations d'identification sont utilisées pour créer un proxy.  
  
> [!NOTE]  
>  Ces informations sont fournies pour vous aider à comprendre les conséquences de l'exécution d'agents dans le contexte de sécurité approprié. Il ne devrait pas être nécessaire d'interagir directement avec les informations d'identification ou les proxys qui ont été créés.  
  
## <a name="see-also"></a> Voir aussi  
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Sécurité et protection &#40;Réplication&#41;](../../../relational-databases/replication/security/security-and-protection-replication.md)   
 [Sécuriser le dossier d’instantanés](../../../relational-databases/replication/security/secure-the-snapshot-folder.md)  
  
  
