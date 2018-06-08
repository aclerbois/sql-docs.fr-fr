---
title: Affecter des rôles DQS aux utilisateurs | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: install-windows
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: afb445b5-bdbe-4bfe-844f-344766cdc2b2
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f2034bc27c747649932bae6d7348cbc006afdfb0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="grant-dqs-roles-to-users"></a>Affecter des rôles DQS aux utilisateurs

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment créer des connexions SQL selon un principal Windows, et accorde ces rôles de [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) sur la base de données DQS_MAIN.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
  
-   Vous devez avoir terminé l'installation du [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] en exécutant le fichier DQSInstaller.exe. Pour plus d’informations, consultez [Exécuter DQSInstaller.exe pour terminer l’installation du serveur DQS](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
-   Votre compte d'utilisateur Windows doit être un membre du rôle serveur fixe sysadmin approprié (tel que securityadmin, serveradmin ou sysadmin) pour créer une connexion SQL et lui accorder des rôles DQS.  
  
### <a name="to-create-sql-login-and-grant-dqs-roles"></a>Pour créer une connexion SQL et accorder des rôles DQS  
  
1.  Démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], développez votre instance de SQL Server, puis développez **Sécurité**.  
  
3.  Cliquez avec le bouton droit sur le dossier **Sécurité** , pointez sur **Nouveau**, puis cliquez sur **Connexion**.  
  
4.  Dans la boîte de dialogue **Nouvelle connexion** , spécifiez le nom d'un utilisateur Windows dans la zone **Nom de connexion** , spécifiez le type d'authentification **Authentification Windows**et cliquez sur **Rechercher** pour valider l'utilisateur.  
  
5.  Une fois l'utilisateur validé, dans le volet de gauche, cliquez sur **Mappage de l'utilisateur** .  
  
6.  Dans le volet droit, cochez la case sous la colonne **Mappage** pour la base de données **DQS_MAIN** , puis cochez la case **dqs_administrator**, **dqs_kb_editor**ou **dqs_kb_operator** dans le volet **Appartenance au rôle de base de données : DQS_MAIN** , selon le niveau d’accès nécessaire pour l’utilisateur. Pour plus d'informations sur les trois rôles DQS, consultez [DQS Security](../../data-quality-services/dqs-security.md).  
  
7.  Dans la boîte de dialogue **Nouvelle connexion** , cliquez sur **OK** pour appliquer les modifications.  
  
    > [!NOTE]  
    >  Si vous accordez le rôle **dqs_administrator** à un utilisateur, appliquez les modifications, puis revérifiez les autorisations de l’utilisateur ; les deux autres cases de rôles DQS (**dq_kb_editor** et **dqs_kb_operator**) sont aussi cochées.  
  
## <a name="next-steps"></a>Next Steps  
 Essayez d'ouvrir une session sur le [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] à l'aide du compte d'utilisateur Windows pour lequel vous venez de créer une connexion SQL et d'accorder un rôle DQS.  
  
## <a name="see-also"></a> Voir aussi  
 [Installer Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Créer un compte de connexion](../../relational-databases/security/authentication-access/create-a-login.md)  
  
  
