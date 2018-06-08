---
title: Gérer des connexions dans la liste d’accès à la publication | Microsoft Docs
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
helpviewer_keywords:
- logins [SQL Server replication], publication access list
- publications [SQL Server replication], publication access lists
- publication access list (PAL)
- PAL (publication access list)
- logins [SQL Server replication], managing
ms.assetid: fceb216b-0b18-4e3b-8ae0-13e35920dcbc
caps.latest.revision: 45
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 317dc0b140cc7da9ad54664eea9f00bb71715051
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-logins-in-the-publication-access-list"></a>Gérer des connexions dans la liste d'accès à la publication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment gérer des connexions dans la liste d'accès à la publication dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. L'accès à une publication est contrôlé par la liste d'accès à la publication. Les connexions et les groupes peuvent être ajoutés et supprimés de la liste d'accès à la publication.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Conditions préalables](#Prerequisites)  
  
-   **Pour gérer les connexions dans la liste d'accès à la publication, à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez associer la connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à un utilisateur de base de données dans la base de données de publication avant d'ajouter cette connexion à la liste d'accès à la publication.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 La liste d’accès à la publication dans la page **Liste d’accès à la publication** de la boîte de dialogue **Propriétés de la publication - \<Publication>** sert à gérer les connexions. Pour plus d’informations sur la façon d’accéder à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-manage-logins-in-the-pal"></a>Pour gérer des noms de connexion dans la liste d'accès à la publication  
  
1.  Dans la page **Liste d’accès à la publication** de la boîte de dialogue **Propriétés de la publication - \<Publication>**, utilisez les boutons **Ajouter**, **Supprimer** et **Supprimer tout** pour ajouter et supprimer des connexions et des groupes dans la liste d’accès à la publication. Ne supprimez pas **distributor_admin** de la liste d'accès à la publication. Ce compte est utilisé par la réplication.  
  
    > [!NOTE]  
    >  En cas d'utilisation d'un serveur de distribution distant, les comptes de la liste d'accès à la publication doivent être disponibles à la fois auprès du serveur de publication et auprès du serveur de distribution. Le compte doit être un compte de domaine ou un compte local défini sur les deux serveurs. Les mots de passe associés aux deux connexions doivent être identiques.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-groups-and-logins-that-belong-to-the-pal"></a>Pour afficher les groupes et les connexions qui figurent dans la liste d'accès à la publication  
  
1.  Exécutez [sp_help_publication_access](../../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez le nom de la publication pour **@publication**. Cela affiche des informations sur les groupes et les connexions qui figurent dans la liste d'accès à la publication.  
  
#### <a name="to-add-groups-and-logins-to-the-pal"></a>Pour ajouter des groupes et des connexions dans la liste d'accès à la publication  
  
1.  Exécutez [sp_grant_publication_access](../../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez le nom de la publication pour **@publication**et le nom de la connexion ou du groupe à ajouter pour **@login**.  
  
#### <a name="to-remove-groups-and-logins-from-the-pal"></a>Pour supprimer des groupes et des connexions de la liste d'accès à la publication  
  
1.  Exécutez [sp_revoke_publication_access](../../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez le nom de la publication pour **@publication**et le nom de la connexion ou du groupe à ajouter pour **@login**.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer des connexions dans la liste d'accès à la publication](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md)   
 [Modèle de sécurité de l’Agent de réplication](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Sécuriser une topologie de réplication](../../../relational-databases/replication/security/secure-a-replication-topology.md)   
 [Sécuriser le serveur de publication](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
  
