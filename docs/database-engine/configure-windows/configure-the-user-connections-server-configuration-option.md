---
title: Configurer l’option de configuration de serveur user connections | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- simultaneous connections [SQL Server]
- user connections option [SQL Server]
- users [SQL Server], simultaneous connections
- maximum number of simultaneous user connections
- connections [SQL Server], simultaneous
ms.assetid: 53beee6e-59fe-4276-9abb-8f1cec2a3508
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 8e06f9c776811fab5dc2a8f8f9748dde1402e0f7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-the-user-connections-server-configuration-option"></a>Configurer l'option de configuration de serveur user connections
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment définir l'option de configuration de serveur **user connections** dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **user connections** spécifie le nombre maximal de connexions utilisateur simultanées autorisées sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nombre réel de connexions utilisateur autorisées dépend également de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée, ainsi que des limites de vos applications et de votre matériel. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise un maximum de 32 767 connexions utilisateur. L’option **user connections** est dynamique (auto-configurable). Ainsi, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique automatiquement le nombre maximal de connexions utilisateur en fonction des besoins, jusqu’à la valeur maximale autorisée. Par exemple, si seuls 10 utilisateurs sont connectés, 10 objets connexion utilisateur sont alloués. Dans la plupart des cas, il est inutile de modifier la valeur de cette option. La valeur par défaut est zéro, ce qui signifie que le nombre maximal (32 767) de connexions utilisateur est autorisé.  
  
 Pour déterminer le nombre maximal de connexions utilisateur autorisé par votre système, vous pouvez exécuter [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) ou interroger l’affichage catalogue [sys.configuration](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) .  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option user connections, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l'option user connections](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   L'option **user connections** vous aide à éviter de surcharger le serveur avec de trop nombreuses connexions simultanées. Pour estimer le nombre de connexions à définir, prenez comme base la configuration de votre système et les besoins des utilisateurs. Par exemple, pour un système comptant de nombreux utilisateurs, chaque utilisateur n'a pas obligatoirement besoin d'une connexion unique. Les connexions peuvent être partagées par plusieurs utilisateurs. Les utilisateurs qui exécutent des applications OLE DB ont besoin d’une connexion pour chaque objet de connexion ouvert. Les utilisateurs qui exécutent des applications ODBC (Open Database Connectivity) ont besoin d’une connexion pour chaque descripteur de connexion actif dans l’application. Les utilisateurs qui exécutent des applications de type bibliothèque de base de données (DB-Library) ont besoin d’une connexion pour chaque processus démarré qui appelle la fonction **dbopen** de la bibliothèque de base de données.  
  
    > [!IMPORTANT]  
    >  Si vous devez utiliser cette option, n'attribuez pas une valeur trop élevée, car chaque connexion possède un espace réservé, qu'elle soit utilisée ou non. Si vous dépassez le nombre maximal de connexions utilisateur, un message d'erreur s'affiche et vous devez attendre qu'une autre connexion soit disponible pour pouvoir vous connecter.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-user-connections-option"></a>Pour configurer l'option user connections  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur, puis cliquez sur **Propriétés**.  
  
2.  Cliquez sur le nœud **Connexions** .  
  
3.  Sous **Connexions**, dans la zone **Nombre maximal de connexions simultanées** , tapez ou sélectionnez une valeur comprise entre 0 et 32 767 pour définir le nombre maximal d'utilisateurs autorisés à se connecter simultanément à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-user-connections-option"></a>Pour configurer l'option user connections  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `user connections` la valeur `325` utilisateurs.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'user connections', 325 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="FollowUp"></a> Suivi : Après avoir configuré l'option user connections  
 Le serveur doit être redémarré pour que le paramètre puisse être effet.  
  
## <a name="see-also"></a> Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
