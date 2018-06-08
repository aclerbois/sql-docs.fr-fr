---
title: Sécurité de l’Agent de distribution | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.security.DA.f1
helpviewer_keywords:
- Distribution Agent Security dialog box
ms.assetid: de40cc21-2e58-4464-9be7-b5b90c925e9b
caps.latest.revision: 25
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 3dd07937e02644043078dd82c98e14143b6ae241
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="distribution-agent-security"></a>Sécurité de l'Agent de distribution
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Sécurité de l'Agent de distribution** permet de spécifier le compte Windows sous lequel s'exécute l'Agent de distribution. Cet agent s'exécute généralement sur le serveur de distribution pour les abonnements par envoi de données et sur l'Abonné pour les abonnements par extraction. Le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows est également baptisé *compte de processus*du fait que le processus agent s'exécute sous ce compte. La boîte de dialogue propose des options supplémentaires en fonction de la façon d'y accéder :  
  
-   Si vous accédez à la boîte de dialogue à partir de l'Assistant Nouvel abonnement, elle permet de spécifier le contexte dans lequel l'Agent de distribution établit les connexions avec l'Abonné (pour les abonnements par envoi de données) ou avec le serveur de distribution (pour les abonnements par extraction). La connexion peut avoir lieu en empruntant l'identité du compte Windows ou dans le contexte d'un compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous spécifiez.  
  
-   Si vous accédez à la boîte de dialogue à partir de la boîte de dialogue **Propriétés de l'abonnement** , spécifiez le contexte dans lequel l'Agent de distribution établit les connexions en cliquant sur le bouton des propriétés (**...**) dans **Connexion de l'Abonné** ou sur la ligne **Connexion du serveur de distribution** de cette boîte de dialogue. Pour plus d’informations sur l’accès à la boîte de dialogue **Propriétés de l’abonnement**, consultez [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md) et la procédure indiquant comment [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
 Tous les comptes doivent être valides, le mot de passe correct étant spécifié pour chaque compte. Les comptes et les mots de passe ne sont pas validés tant qu'un agent ne s'exécute pas.  
  
## <a name="options"></a>Options  
 **Process Account**  
 Entrez un compte Windows sous lequel l'Agent de distribution s'exécute :  
  
-   Pour les abonnements par envoi de données, le compte doit :  
  
    -   être au moins un membre du rôle de base de données fixe **db_owner** dans la base de données de distribution ;  
  
    -   être membre de la liste d'accès à la publication (PAL) ;  
  
    -   avoir les autorisations de lecture sur le partage des fichiers d'instantanés.  
  
    -   avoir les autorisations de lecture dans le répertoire d'installation du fournisseur OLE DB de l'Abonné si l'abonnement concerne un Abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Pour les abonnements par extraction, le compte doit être au moins un membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement.  
  
 Des autorisations supplémentaires sont indispensables si l'identité du compte de processus est empruntée lorsque les connexions sont établies. Voir les paragraphes **Se connecter au serveur de distribution** et **Connexion à l'Abonné** ci-dessous.  
  
 Le**compte de processus** ne peut être indiqué à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]pour des abonnements par extraction, car l'Agent de distribution ne s'exécute pas sur des instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].  
  
 **Mot de passe** et **Confirmer le mot de passe**  
 Entrez le mot de passe du compte Windows.  
  
 **Se connecter au serveur de distribution**  
 Pour les abonnements par envoi de données, les connexions au serveur de distribution ont toujours lieu en empruntant l'identité du compte spécifié dans la zone de texte **Compte de processus** .  
  
 Pour les abonnements par extraction, choisissez si l'Agent de distribution doit établir les connexions avec le serveur de distribution en empruntant l'identité du compte spécifié dans la zone de texte **Compte de processus** ou en utilisant un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous choisissez d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un nom de connexion et un mot de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Il est recommandé de choisir d'emprunter l'identité du compte Windows plutôt que d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le compte Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour la connexion doit :  
  
-   être membre de la liste d'accès à la publication (PAL) ;  
  
-   avoir les autorisations de lecture sur le partage des fichiers d'instantanés.  
  
 **Connexion à l'Abonné**  
 Pour les abonnements par extraction, les connexions à l'Abonné ont toujours lieu en empruntant l'identité du compte spécifié dans la zone de texte **Compte de processus** .  
  
 Pour les abonnements par envoi de données, les options sont différentes pour les Abonnés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Pour les Abonnés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : choisissez si l'Agent de distribution doit établir les connexions avec l'Abonné en empruntant l'identité du compte spécifié dans la zone de texte **Compte de processus** ou en utilisant un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous choisissez d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un nom de connexion et un mot de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  Il est recommandé de choisir d'emprunter l'identité du compte Windows plutôt que d'utiliser un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Le compte Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour la connexion avec l'abonné doit être au moins un membre du rôle de base de données fixe **db_owner** dans la base de données d'abonnement.  
  
-   Pour les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , spécifiez la connexion avec la base de données qui doit être utilisée lorsque l'Agent de distribution se connecte à l'Abonné. La connexion doit avoir les autorisations suffisantes pour créer des objets dans la base de données d'abonnement. Pour plus d’informations sur la configuration d’abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  
  
 **Options de connexion supplémentaires**  
 Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] uniquement. Spécifiez les options de connexion de l'Abonné sous forme d'une chaîne de connexion (Oracle n'exige pas d'option supplémentaire). Chaque option doit être délimitée par un point-virgule. Trouvez ci-dessous un exemple d'une chaîne de connexion IBM DB2 (les sauts de lignes facilitent la lecture) :  
  
```  
Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
Persist Security Info=False;Connection Pooling=True;  
```  
  
 La plupart des options de cette chaîne sont spécifiques du serveur DB2 que vous configurez, mais vous devez attribuer à l'option **Traiter les données binaires comme des caractères** la valeur **False**. Une valeur est exigée de façon que l'option **Catalogue initial** identifie la base de données d'abonnement. Pour plus d’informations, voir [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer les comptes de connexion et les mots de passe dans la réplication](../../relational-databases/replication/security/manage-logins-and-passwords-in-replication.md)   
 [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Présentation des Agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
