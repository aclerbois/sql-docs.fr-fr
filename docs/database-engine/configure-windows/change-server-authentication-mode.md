---
title: Changer le mode d’authentification du serveur | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- sa account
- authentication [SQL Server], changing modes
- server authentication mode [SQL Server]
- modifying server authentication mode
ms.assetid: 79babcf8-19fd-4495-b8eb-453dc575cac0
caps.latest.revision: 31
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 4c6aecb4ca6f75f7089efd7931455f34f1bc9915
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-server-authentication-mode"></a>Changer le mode d'authentification du serveur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment modifier le mode d'authentification du serveur dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Au cours de l’installation, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] est paramétré sur **Mode d’authentification Windows** ou sur **Mode d’authentification SQL Server et Windows**. Après l'installation, vous pouvez modifier le mode d'authentification à tout moment.  
  
 Si vous sélectionnez le **Mode d’authentification Windows** au cours de l’installation, la connexion sa est désactivée et un mot de passe est attribué par le programme d’installation. Si vous remplacez ensuite le mode d’authentification par **Mode d’authentification SQL Server et Windows**, la connexion sa reste désactivée. Pour utiliser la connexion sa, utilisez l’instruction ALTER LOGIN de façon à activer la connexion et attribuer un nouveau mot de passe. La connexion sa au serveur est possible uniquement via l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour modifier le mode d'authentification du serveur, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Security"></a> Sécurité  
 Le compte sa est un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bien connu qui est souvent la cible d'utilisateurs malveillants. N'activez pas le compte sa à moins que votre application l'exige. Il est très important que vous utilisiez un mot de passe fort pour la connexion sa.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-change-security-authentication-mode"></a>Pour modifier le mode d'authentification de sécurité  
  
1.  Dans l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , cliquez avec le bouton droit sur le serveur, puis sélectionnez **Propriétés**.  
  
2.  Dans la page **Sécurité** , sous **Authentification du serveur**, sélectionnez le nouveau mode d'authentification du serveur, puis cliquez sur **OK**.  
  
3.  Dans la boîte de dialogue [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , cliquez sur **OK** pour accepter le message indiquant la nécessité de redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre serveur, puis cliquez sur **Redémarrer**. S'il est en cours d'exécution, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit également être redémarré.  
  
#### <a name="to-enable-the-sa-login"></a>Pour activer la connexion sa  
  
1.  Dans l’Explorateur d’objets, développez **Sécurité**, puis Connexions, cliquez avec le bouton droit sur **sa**, puis sélectionnez **Propriétés**.  
  
2.  Sur la page **Général** , vous devrez peut-être créer et confirmer un mot de passe pour la connexion.  
  
3.  Sur la page **État** , dans la section **Connexion** , cliquez sur **Activée**, puis sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour activer la connexion sa**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L’exemple suivant active la connexion sa et définit un nouveau mot de passe.  
  
    ```  
    ALTER LOGIN sa ENABLE ;  
    GO  
    ALTER LOGIN sa WITH PASSWORD = '<enterStrongPasswordHere>' ;  
    GO  
  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Mots de passe forts](../../relational-databases/security/strong-passwords.md)   
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [Se connecter à SQL Server lorsque les administrateurs système n’y ont plus accès](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)  
  
  
