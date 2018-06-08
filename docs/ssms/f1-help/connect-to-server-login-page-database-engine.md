---
title: Se connecter au serveur (page Connexion) — Moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-f1
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttosqlserver.login.f1
ms.assetid: e08cfbc3-bed5-4401-a13b-1c66d902fe32
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: dd237091b0839caa0db90637b9338704a217faab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-server-login-page-database-engine"></a>Se connecter au serveur (page Connexion) — Moteur de base de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Utilisez cet onglet pour afficher ou spécifier les options de connexion à [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]. Dans la plupart des cas, vous pouvez vous connecter en entrant le nom d’ordinateur du serveur de base de données dans la zone **Nom du serveur** et en cliquant sur **Se connecter**. Si vous vous connectez à une instance nommée, utilisez le nom d’ordinateur suivi d’une barre oblique inverse, puis du nom de l’instance. Par exemple, `mycomputer\myinstance`. Si vous vous connectez à [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)], utilisez le nom d’ordinateur suivi de **\sqlexpress**.  
  
De nombreux facteurs affectent votre capacité à vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour obtenir de l’aide, consultez les ressources suivantes :  
- [Leçon 1 du didacticiel : Connexion au moteur de base de données](../../relational-databases/lesson-1-connecting-to-the-database-engine.md)  
- [Résoudre les problèmes de connexion au moteur de base de données SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)  
- [Résolution des erreurs de connectivité à SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)    
  
> [!NOTE]  
> Pour vous connecter avec l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] doit être configuré en mode d'authentification Windows et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] . Pour plus d’informations sur la détermination et la modification du mode d’authentification, consultez [Procédure : modification du mode d’authentification du serveur](http://msdn.microsoft.com/en-us/79babcf8-19fd-4495-b8eb-453dc575cac0).  
  
## <a name="options"></a>Options  
**Type de serveur**  
Lorsque vous inscrivez un serveur depuis l'Explorateur d'objets, sélectionnez le type de serveur pour vous connecter à : [!INCLUDE[ssDE](../../includes/ssde_md.md)], Analysis Services, Reporting Services ou Integration Services. Le reste de la boîte de dialogue affiche uniquement les options s'appliquant au type de serveur sélectionné. Lors de l’inscription d’un serveur de la liste Serveurs inscrits, la zone **Type de serveur** est en lecture seule et indique le type de serveur affiché dans le composant Serveurs inscrits. Pour inscrire un autre type de serveur, sélectionnez [!INCLUDE[ssDE](../../includes/ssde_md.md)], Analysis Services, Reporting Services ou Integration Services dans la barre d'outils Serveurs inscrits avant de commencer l'inscription d'un nouveau serveur.  
  
Quand vous vous connectez à une instance du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] par le biais de [!INCLUDE[ssSDSfull](../../includes/sssdsfull_md.md)], vous devez utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et spécifier une base de données dans la boîte de dialogue **Se connecter au serveur** , sous l’onglet **Propriétés de connexion** . Vérifiez que vous avez coché la case **Chiffrer la connexion** .  
  
Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] se connecte à **master**. Si vous spécifiez une base de données utilisateur, vous voyez uniquement la base de données et ses objets dans l’Explorateur d'objets. Si vous vous connectez à **master**, vous pouvez voir toutes les bases de données. Pour plus d’informations, consultez [Présentation de la base de données SQL Azure Windows](http://go.microsoft.com/fwlink/?LinkId=163948).  
  
**Nom du serveur**  
Sélectionnez l'instance de serveur à laquelle se connecter. La dernière instance de serveur à laquelle une connexion a été établie est affichée par défaut.  
  
**Authentification**  
La version actuelle de SSMS propose cinq modes d’authentification lorsque vous vous connectez à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)]. Si votre boîte de dialogue d’authentification ne correspond pas à la liste suivante, téléchargez la version la plus récente de SSMS à partir de [Télécharger SQL Server Management Studio (SSMS)](../download-sql-server-management-studio-ssms.md).     
  
Quand vous vous connectez à une instance du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] par le biais de [!INCLUDE[ssSDS](../../includes/sssds_md.md)], vous devez utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et spécifier une base de données dans la boîte de dialogue **Se connecter au serveur** , sous l’onglet **Propriétés de connexion** . Vérifiez que vous avez coché la case **Chiffrer la connexion** .  
  
Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] se connecte à **master**. Si vous spécifiez une base de données utilisateur quand vous vous connectez à [!INCLUDE[ssSDS](../../includes/sssds_md.md)], vous voyez uniquement cette base de données et ses objets dans l’Explorateur d’objets. Si vous vous connectez à **master**, vous pouvez voir toutes les bases de données. Pour plus d’informations, consultez [Présentation de la base de données SQL Azure Windows](http://go.microsoft.com/fwlink/?LinkId=163948).  
  
  > **Authentification Windows**  
[!INCLUDE[msCoName](../../includes/msconame_md.md)] Le mode d’authentification Windows permet à l’utilisateur de se connecter au moyen d’un compte d’utilisateur Windows.  
  
  > **Authentification SQL Server**  
Quand un utilisateur se connecte avec un nom d’accès et un mot de passe spécifiés à partir d’une connexion non autorisée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] réalise l’authentification en vérifiant si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] a été défini et si le mot de passe spécifié correspond à celui enregistré précédemment. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ne possède pas de compte de connexion, l'authentification échoue et un message d'erreur est envoyé à l'utilisateur. Lorsque c'est possible, utilisez l'authentification Windows.  
  
  > **Active Directory - Authentification universelle avec prise en charge de MFA**  
Active Directory - Authentification universelle avec prise en charge de MFA est un processus interactif qui prend en charge Azure Multi-Factor Authentication (MFA). Azure MFA contribue à protéger l’accès aux données et aux applications tout en répondant à la volonté des utilisateur d’avoir un processus d’authentification simple. Il fournit une authentification renforcée avec un éventail d’options de vérification simples (appel téléphonique, SMS, cartes à puce avec code confidentiel ou notification d’application mobile), ce qui permet aux utilisateurs de choisir la méthode qu’ils préfèrent. Quand le compte d’utilisateur est configuré pour MFA, le processus d’authentification interactif demande une plus grande intervention de l’utilisateur par le biais de boîtes de dialogue contextuelles, d’utilisation de carte à puce, et ainsi de suite. Quand le compte d’utilisateur est configuré pour MFA, l’utilisateur doit sélectionner Authentification universelle Azure pour se connecter. Si le compte d’utilisateur ne nécessite pas MFA, l’utilisateur peut toujours utiliser les deux autres options d’authentification Azure Active Directory. Pour plus d’informations, consultez [Prise en charge SSMS pour Azure AD MFA avec SQL Database et SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/). Si nécessaire, vous pouvez changer le domaine qui authentifie la connexion, en cliquant sur **Options**, en sélectionnant l’onglet **Propriétés de connexion** et en renseignant la zone **Nom du domaine AD ou ID de locataire** .  

  > **Active Directory - Authentification par mot de passe**  
L’authentification Azure Active Directory est un mécanisme de connexion à [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssSDSfull](../../includes/sssdsfull_md.md)] à l’aide d’identités dans Azure Active Directory (Azure AD).  Utilisez cette méthode pour vous connecter à [!INCLUDE[ssSDS](../../includes/sssds_md.md)] si vous êtes connecté à Windows avec les informations d’identification d’un domaine qui n’est pas fédéré avec Azure, ou si vous utilisez l’authentification Azure AD avec Azure AD basée sur le domaine client ou initial. Pour plus d’informations, voir [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/).  
  
  > **Active Directory - Authentification intégrée**  
L’authentification Azure Active Directory est un mécanisme de connexion à [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssSDSfull](../../includes/sssdsfull_md.md)] à l’aide d’identités dans Azure Active Directory (Azure AD). Utilisez cette méthode pour vous connecter à [!INCLUDE[ssSDS](../../includes/sssds_md.md)] si vous êtes connecté à Windows avec vos informations d’identification Azure Active Directory à partir d’un domaine fédéré. Pour plus d’informations, voir [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/).  
  
**User name**  
Nom d’utilisateur Windows avec lequel vous connecter. Cette option est disponible uniquement si vous avez choisi la connexion avec **l’Authentification par mot de passe Active Directory**. Il est en lecture seule lorsque vous sélectionnez l’authentification **Authentification Windows** ou **Active Directory - Authentification intégrée**.  
  
**Connexion**  
Entrez le nom d'accès avec lequel se connecter. Cette option est disponible uniquement si vous avez choisi la connexion avec l’Authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Authentification par mot de passe Active Directory.  
  
**Mot de passe**  
Entrez le mot de passe utilisé avec la connexion. Cette option est modifiable uniquement si vous avez choisi la connexion avec l’Authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Authentification par mot de passe Active Directory.  
  
**Mémoriser le mot de passe**  
Cliquez sur cette option pour que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] enregistre le mot de passe entré. Cette option n’est affichée que si vous avez choisi la connexion avec l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
**Se connecter**  
Cliquez sur le bouton pour vous connecter au serveur.  
  
**Options**  
Cliquez pour afficher les onglets **Propriétés de connexion** et **Paramètres de connexion supplémentaires**.  
   
  
