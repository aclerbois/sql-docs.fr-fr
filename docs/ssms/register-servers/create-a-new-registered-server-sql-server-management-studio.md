---
title: Créer un serveur inscrit (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-registration
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.general.sqlce.f1
- sql13.swb.registerserver.general.sqlserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], creating new registered servers
ms.assetid: 716ea070-a3b5-4514-9de2-82ce8a96514b
caps.latest.revision: 31
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ee2fa684589fcdf2089162bd9f7f5e243add925e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-new-registered-server-sql-server-management-studio"></a>Créer un nouveau serveur inscrit (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Cette rubrique explique comment enregistrer les informations de connexion relatives aux serveurs auxquels vous accédez fréquemment en inscrivant ceux-ci dans le composant Serveurs inscrits de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Un serveur peut être inscrit avant la connexion ou lors de la connexion depuis l'Explorateur d'objets. Il existe une option de menu qui permet d'inscrire les instances de serveur sur l'ordinateur local.  
  
 Il existe deux types de serveurs inscrits :  
  
-   Groupes de serveurs locaux  
  
     Utilisez des groupes de serveurs locaux pour vous connecter facilement aux serveurs que vous gérez fréquemment. Les serveurs locaux et non locaux sont inscrits dans des groupes de serveurs locaux. Les groupes de serveurs locaux sont uniques à chaque utilisateur. Pour plus d’informations sur la façon de partager des informations de serveur inscrit, consultez [Exporter les informations des serveurs inscrits &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/export-registered-server-information-sql-server-management-studio.md) et [Importer les informations des serveurs inscrits &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/import-registered-server-information-sql-server-management-studio.md).  
  
    > [!NOTE]  
    >  Nous recommandons d'utiliser l'authentification Windows dans la mesure du possible.  
  
-   Serveurs de gestion centralisée  
  
     Les serveurs de gestion centralisée stockent les inscriptions de serveurs dans le serveur de gestion centralisée plutôt que dans le système de fichiers. Les serveurs de gestion centralisée et les serveurs inscrits subordonnés ne peuvent être inscrits que par le biais de l'authentification Windows. Une fois qu'un serveur de gestion centralisée a été inscrit, ses serveurs inscrits associés sont affichés automatiquement. Pour plus d’informations sur les serveurs d’administration centrale, consultez [Administrer plusieurs serveurs à l’aide de serveurs de gestion centralisée](../../relational-databases/administer-multiple-servers-using-central-management-servers.md). Les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ne peuvent pas être désignées comme serveurs de gestion centralisée.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-automatically-register-the-local-server-instances"></a>Pour inscrire automatiquement les instances de serveur local  
  
-   Dans Serveurs inscrits, cliquez avec le bouton droit sur un nœud de l’arborescence Serveurs inscrits, puis cliquez sur **Mettre à jour l’inscription du serveur local**.  
  
#### <a name="to-create-a-new-registered-server"></a>Pour créer un nouveau serveur inscrit  
  
1.  Si l'élément Serveurs inscrits n'est pas visible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le menu **Affichage** , cliquez sur **Serveurs inscrits**.  
  
     **Type de serveur**  
     Quand un serveur est inscrit à partir de Serveurs inscrits, la zone **Type de serveur** est en lecture seule et correspond au type de serveur affiché dans le volet Serveurs inscrits. Pour inscrire un autre type de serveur, cliquez sur **Moteur de base de données**, **Serveur d'analyse**, **Reporting Services**ou **Integration Services** dans la barre d'outils **Serveurs inscrits** avant de commencer à inscrire un nouveau serveur.  
  
     **Nom du serveur**  
     Sélectionnez l’instance de serveur à inscrire au format suivant : *\<nom_serveur>*[\\*\<nom_instance>*].  
  
     **Authentification**  
     Deux modes d'authentification sont disponibles lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **Authentification Windows**  
     Le mode d'authentification Windows permet à l'utilisateur de se connecter au moyen d'un compte d'utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
     **Authentification SQL Server**  
     Quand un utilisateur se connecte avec un nom d’accès et un mot de passe spécifiés à partir d’une connexion non approuvée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réalise lui-même l’authentification en vérifiant si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été défini et si le mot de passe spécifié correspond à celui enregistré précédemment. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne possède pas de compte de connexion, l'authentification échoue et un message d'erreur est envoyé à l'utilisateur.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] Pour plus d’informations, consultez [Choisir un mode d’authentification](../../relational-databases/security/choose-an-authentication-mode.md).  
  
     **User name**  
     Spécifie le nom d'utilisateur actuel avec lequel vous vous connectez. Cette option en lecture seule est disponible uniquement si vous avez choisi de vous connecter via l'authentification Windows. Pour modifier les **Noms d'utilisateurs**, ouvrez une session sur l'ordinateur en tant qu'utilisateur différent.  
  
     **Connexion**  
     Entrez le nom d'accès avec lequel se connecter. Cette option est disponible uniquement si vous avez choisi la connexion avec l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     **Mot de passe**  
     Entrez le mot de passe utilisé avec la connexion. Cette option peut être modifiée uniquement si vous avez choisi de vous connecter via l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     **Mémoriser le mot de passe**  
     Sélectionnez cette option pour que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiffre et stocke le mot de passe entré. Cette option n'est affichée que si vous avez choisi la connexion avec l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  Si vous avez stocké le mot de passe et ne voulez plus le conserver en mémoire, décochez la case, puis cliquez sur **Enregistrer**.  
  
     **Nom du serveur inscrit**  
     Le nom qui doit apparaître dans Serveurs inscrits. Ce nom ne doit pas correspondre à celui de la zone **Nom du serveur** .  
  
     **Description du serveur inscrit**  
     Entrez une description facultative du serveur.  
  
     **Test**  
     Cliquez sur cette option pour tester la connexion au serveur sélectionné dans la zone **Nom du serveur**.  
  
     **Enregistrer**  
     Cliquez sur ce bouton pour enregistrer les paramètres des serveurs inscrits.  
  
## <a name="multiserver-queries"></a>Requêtes multiserveurs  
 La fenêtre Éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] peut se connecter à plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les interroger simultanément. Les résultats retournés par la requête peuvent être fusionnés dans un volet de résultats unique ou retournés dans des volets de résultats distincts. En guise d'option, l'Éditeur de requête peut inclure des colonnes qui fournissent le nom du serveur ayant produit chaque ligne et également la connexion utilisée pour se connecter au serveur ayant fourni chaque ligne. Pour plus d’informations sur la façon d’exécuter des requêtes multiserveurs, consultez [Exécuter des instructions simultanément sur plusieurs serveurs &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/execute-statements-against-multiple-servers-simultaneously.md).  
  
 Pour exécuter des requêtes sur tous les serveurs du groupe de serveurs locaux, cliquez avec le bouton droit sur le groupe de serveurs, pointez sur **Se connecter**, puis cliquez sur **Nouvelle requête**. Lorsque les requêtes sont exécutées dans la nouvelle fenêtre de l'éditeur de requête, elles s'exécutent sur tous serveurs du groupe en utilisant les informations de connexion stockées (y compris le contexte d'authentification utilisateur). Toute connexion d'un serveur inscrit à l'aide de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mais n'enregistrant pas le mot de passe est vouée à l'échec.  
  
 Pour exécuter des requêtes sur tous les serveurs inscrits auprès d’un serveur de gestion centralisée, développez le serveur de gestion centralisée, cliquez avec le bouton droit sur le groupe de serveurs, pointez sur **Se connecter**, puis cliquez sur **Nouvelle requête**. Lorsque les requêtes sont exécutées dans la nouvelle fenêtre Éditeur de requête, elles s'exécutent contre tous les serveurs du groupe de serveurs, à l'aide des informations de connexion stockées et du contexte d'authentification Windows de l'utilisateur.  
  
## <a name="see-also"></a> Voir aussi  
 [Masquer les objets système dans l'Explorateur d'objets](http://msdn.microsoft.com/library/c01d8804-838c-4f75-b78c-80e41e4fffdc)   
 [Exporter les informations des serveurs inscrits &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/export-registered-server-information-sql-server-management-studio.md)   
 [Importer les informations des serveurs inscrits &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/import-registered-server-information-sql-server-management-studio.md)  
  
  
