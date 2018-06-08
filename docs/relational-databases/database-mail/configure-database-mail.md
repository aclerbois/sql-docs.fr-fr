---
title: Configuration de la Messagerie de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: database-mail
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlimail.profileandaccountmanagement.f1
- sql13.swb.sqlimail.newaccount.f1
- sql13.swb.dbmail. manageprofilesecurity.profileview.f1
- sql13.swb.sqlimail.manageexistingprofile.f1
- sql13.swb.sqlimail.addaccounttoprofile.f1
- sql13.swb.dbmail.manageexistingaccount.f1
- sql13.swb.sqlimail.manageprofilesecurity.profileview.f1
- sql13.swb.sqlimail.welcome.f1
- sql13.swb.sqlimail.manageprofilesecurity.principalview.f1
- sql13.swb.sqlimail.newsqlimailaccount.f1
- sql13.swb.sqlimail.selectconfiguration.f1
- sql13.swb.dbmail.completewizard.f1
- sql13.swb.dbmail.sendtestemail.test.f1
- sql13.swb.sqlimail.newprofile.f1
- sql13.swb.dbmail.addaccounttoprofile.f1
- sql13.swb.dbmail.newprofile.f1
- sql13.swb.sqlimail.manageexistingaccount.f1
- sql13.swb.dbmail.welcome.f1
- sql13.swb.dbmail.newaccount.f1
- sql13.swb.dbmail.profileandaccountmanagement.f1
- sql13.swb.dbmail.selectconfiguration.f1
- sql13.swb.dbmail.sendtestemail.f1
- sql13.swb.sqlimail.completewizard.f1
- sql13.swb.dbmail.configuresystem.f1
- sql13.swb.sqlimail.configuresystem.f1
- sql13.swb.dbmail.newsqlimailaccount.f1
- sql13.swb.dbmail.manageexistingprofile.f1
- sql13.swb.dbmail.manageprofilesecurity.principalview.f1
ms.assetid: 7edc21d4-ccf3-42a9-84c0-3f70333efce6
caps.latest.revision: 13
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b1df9d91458211d66722ac1b844e5d938690acc3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-database-mail"></a>Configuration de la Messagerie de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment activer et configurer la messagerie de base de données à l'aide de l'Assistant Configuration de la messagerie de base de données, et créer un script de configuration de la messagerie de base de données à l'aide de modèles.  
  
-   **Avant de commencer :**  [Limitations et restrictions](#Restrictions), [Sécurité](#Security)  
  
-   **Pour configurer la messagerie de base de données, utilisez :**  [Assistant Configuration de la messagerie de base de données](#DBWizard), [Utilisation de modèles](#Template)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 Pour activer la messagerie de base de données sur ce serveur, utilisez l’option **Messagerie de base de données XPs** . Pour plus d’informations, consultez la rubrique de référence [Messagerie de base de données XPs (option de configuration de serveur)](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) .  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
 L'activation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker dans une base de données requiert un verrou de base de données. Si Service Broker a été désactivé dans **msdb**, pour activer la messagerie de base de données, arrêtez d'abord l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin que Service Broker puisse obtenir le verrou nécessaire.  
  
###  <a name="Security"></a> Sécurité  
 Pour configurer la messagerie de base de données vous devez être membre du rôle serveur fixe **sysadmin** . Pour envoyer un message de messagerie de base de données, vous devez être membre du rôle de base de données **DatabaseMailUserRole** de la base de données **msdb** .  
  
##  <a name="DBWizard"></a> Utilisation de l'Assistant Configuration de la messagerie de base de données  
 **Pour configurer la messagerie de base de données à l'aide d'un Assistant**  
  
1.  Dans l'Explorateur d'objets, développez le nœud de l'instance dont vous souhaitez configurer la messagerie de base de données.  
  
2.  Développez le nœud **Gestion** .  
  
3.  Cliquez avec le bouton droit sur **Messagerie de base de données**, puis cliquez sur **Configurer la messagerie de base de données**.  
  
4.  Renseignez les boîtes de dialogue de l'Assistant  
  
    -   [Page d'accueil](#Welcome)  
  
    -   [Page Sélectionner une tâche de configuration](#ConfigTask)  
  
    -   [Page Nouveau compte](#NewAccount)  
  
    -   [Page Gérer le compte existant](#ExistingAccount)  
  
    -   [Page Nouveau profil](#NewProfile)  
  
    -   [Page Gérer le profil existant](#ExistingProfile)  
  
    -   [Page Ajouter un compte au profil](#AddAccount)  
  
    -   [Page Gérer les comptes et profils](#AccountsProfiles)  
  
    -   [Gérer la sécurité des profils, onglet Public](#ProfileSecurityPublic)  
  
    -   [Gérer la sécurité des profils, onglet Privé](#ProfileSecurityPrivate)  
  
    -   [Page Configurer les paramètres du système](#SystemParameters)  
  
    -   [Page Fin de l'Assistant](#CompleteWizard)  
  
    -   [Page Envoyer un message électronique de test](#TestEmail)  
  
###  <a name="Welcome"></a> Page d'accueil  
 Cette page décrit les étapes de configuration de la messagerie de base de données.  
  
 **Ne plus afficher cette page** - activez cette option pour ignorer cette page d'accueil.  
  
 **Suivant** - passe à la page **Sélectionner une tâche de configuration** .  
  
 **Annuler** - termine l'Assistant sans configurer la messagerie de base de données  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="ConfigTask"></a> Sélectionner une tâche de configuration  
 Chaque fois que vous utilisez l'Assistant, utilisez la page **Sélectionner une tâche de configuration** pour indiquer quelle tâche vous allez réaliser. Si vous changez d'avis avant d'avoir terminé l'Assistant, utilisez le bouton **Précédent** pour retourner à cette page et sélectionner une autre tâche.  
  
> [!NOTE]  
>  Si la messagerie de base de données n'a pas été activée, vous recevrez le message : **Le composant de messagerie de base de données n'est pas disponible.  Voulez-vous activer ce composant ?** Répondre **Oui**équivaut à activer la messagerie de base de données à l’aide de l’option [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) de la procédure stockée système **sp_configure** .  
  
 **Configurer la messagerie de base de données en effectuant les tâches suivantes**  
 Permet d'exécuter toutes les tâches nécessaires pour configurer la messagerie de base de données pour la première fois. Cette option inclut l'ensemble des trois autres options.  
  
 **Gérer les comptes et profils de messagerie de base de données**  
 Permet de créer de nouveaux comptes et profils de messagerie de base de données ou de consulter, changer ou supprimer des comptes et profils existants.  
  
 **Gérer la sécurité des profils**  
 Permet de configurer les utilisateurs qui ont accès aux profils de la messagerie de base de données.  
  
 **Afficher ou modifier les paramètres du système**  
 Permet de configurer les paramètres système de la messagerie de base de données, par exemple la taille maximale des fichiers de pièces jointes.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="NewAccount"></a> Page Nouveau compte  
 Utilisez cette page pour créer un nouveau compte de messagerie de base de données. Ce type de compte contient les informations d’envoi de messages électroniques à un serveur SMTP.  
  
 Un compte de messagerie de base de données contient les informations utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l’envoi du courrier électronique à un serveur SMTP. Chacun contient des informations propres à un serveur de messagerie particulier.  
  
 Un compte de messagerie de base de données est utilisé uniquement pour la messagerie de base de données. Un compte de messagerie de base de données ne correspond pas à un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à un compte Microsoft Windows. Le courrier de la messagerie de base de données peut être envoyé avec les informations d'identification de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], d'autres informations d'identification que vous fournissez, ou de façon anonyme. Lorsque vous avez recours à l'authentification de base, le nom d'utilisateur et le mot de passe associés au compte de messagerie de base de données sont utilisés uniquement pour l'authentification auprès du serveur de messagerie. Un compte ne doit pas nécessairement correspondre à un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à un utilisateur de l'ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Nom du compte**  
 Tapez le nom du nouveau compte.  
  
 **Description**  
 Tapez une description du compte. Cette description est facultative.  
  
 **Adresse de messagerie**  
 Tapez l'adresse de messagerie du compte. C'est à partir de cette adresse qu'est envoyé le courrier électronique. Par exemple, un compte du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut envoyer des e-mails à partir de l’adresse SqlAgent@Adventure-Works.com.  
  
 **Nom complet**  
 Tapez le nom à afficher sur les messages électroniques envoyés à partir de ce compte. Le nom affiché est facultatif. C'est le nom qui sera affiché dans les messages envoyés à partir de ce compte. Par exemple, un compte de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut afficher le nom « Messagerie automatisée de SQL Server Agent » sur les messages électroniques.  
  
 **Répondre au courrier**  
 Tapez l'adresse de messagerie à utiliser pour les réponses aux messages électroniques envoyés à partir de ce compte. L'adresse Répondre au courrier est facultative. Par exemple, les réponses envoyées vers un compte du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peuvent être adressées à l’administrateur de base de données, dont l’adresse est danw@Adventure-Works.com.  
  
 **Nom du serveur**  
 Tapez le nom ou l'adresse IP du serveur SMTP utilisé par le compte pour envoyer du courrier électronique. En général, son format est semblable à celui-ci : **smtp.***<votre_société>***.com**. Pour plus d'informations, contactez l'administrateur de messagerie.  
  
 **Numéro de port**  
 Tapez le numéro de port du serveur SMTP de ce compte. La plupart des serveurs SMTP utilisent le port 25.  
  
 **Ce serveur demande une connexion sécurisée (SSL)**  
 Chiffre la communication au moyen de Secure Sockets Layer.  
  
 **Authentification Windows à l'aide d'informations d'identification du service de moteur de bases de données**  
 La connexion au serveur SMTP s'effectue avec les informations d'identification configurées pour le service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] .  
  
 **Authentification de base**  
 Spécifiez le nom d'utilisateur et le mot de passe demandés par le serveur SMTP.  
  
 **Nom d'utilisateur**  
 Tapez le nom d'utilisateur que la messagerie de base de données utilise pour se connecter au serveur SMTP. Ce nom est obligatoire si le serveur SMTP nécessite une authentification de base.  
  
 **Mot de passe**  
 Tapez le mot de passe que la messagerie de base de données utilise pour se connecter au serveur SMTP. Ce mot de passe est obligatoire si le serveur SMTP nécessite une authentification de base.  
  
 **Confirmer le mot de passe**  
 Tapez une seconde fois le mot de passe pour le valider. Ce mot de passe est obligatoire si le serveur SMTP nécessite une authentification de base.  
  
 **Authentification anonyme**  
 Le message est envoyé au serveur SMTP sans informations d'identification. Utilisez cette option lorsque le serveur SMTP ne nécessite pas d'authentification.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="ExistingAccount"></a> Page Gérer le compte existant  
 Utilisez cette page pour gérer un compte de messagerie de base de données existant.  
  
 **Nom du compte**  
 Sélectionnez le compte à afficher, mettre à jour ou supprimer.  
  
 **Supprimer**  
 Supprime le compte sélectionné. Vous devez supprimer ce compte des profils associés ou supprimer ces profils avant de supprimer le compte sélectionné.  
  
 **Description**  
 Permet d'afficher ou de modifier la description du compte. Cette description est facultative.  
  
 **Adresse de messagerie**  
 Affichez ou modifiez l'adresse de messagerie du compte. C'est à partir de cette adresse qu'est envoyé le courrier électronique. Par exemple, un compte du service SQL Server Agent Microsoft peut envoyer du courrier électronique à partir de l’adresse **SqlAgent@Adventure-Works.com**.  
  
 **Nom complet**  
 Affichez ou modifiez le nom qui apparaîtra sur le courrier électronique envoyé à partir de ce compte. Le nom affiché est facultatif. C'est le nom qui sera affiché dans les messages envoyés à partir de ce compte. Par exemple, un compte du service SQL Server Agent peut afficher le nom **SQL Server Agent Automated Mailer** sur les messages électroniques.  
  
 **Répondre au courrier**  
 Affichez ou modifiez l'adresse de messagerie qui sera utilisée pour répondre au courrier électronique envoyé à partir de ce compte. L'adresse Répondre au courrier est facultative. Par exemple, les réponses envoyées vers un compte du service SQL Server Agent peuvent être adressées à l’administrateur de base de données, dont l’adresse est **danw@Adventure-Works.com**.  
  
 **Nom du serveur**  
 Affichez ou modifiez le nom du serveur SMTP utilisé par le compte pour envoyer du courrier électronique. En général, son format est semblable à celui-ci : **smtp.<votre_société>.com**. Pour plus d'informations, contactez l'administrateur de messagerie.  
  
 **Numéro de port**  
 Affichez ou modifiez le numéro de port du serveur FTP de ce compte. La plupart des serveurs SMTP utilisent le port 25.  
  
 **Ce serveur demande une connexion sécurisée (SSL)**  
 Chiffre la communication au moyen de Secure Sockets Layer.  
  
 **Authentification Windows à l'aide d'informations d'identification du service de moteur de bases de données**  
 La connexion au serveur SMTP s'effectue avec les informations d'identification configurées pour le service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] .  
  
 **Authentification de base**  
 Spécifiez le nom d'utilisateur et le mot de passe demandés par le serveur SMTP.  
  
 **User name**  
 Affichez ou modifiez le nom d'utilisateur que la messagerie de base de données utilise pour se connecter au serveur SMTP. Ce nom est obligatoire si le serveur SMTP nécessite une authentification de base.  
  
 **Mot de passe**  
 Modifiez le mot de passe que la messagerie de base de données utilise pour se connecter au serveur SMTP. Ce mot de passe est obligatoire si le serveur SMTP nécessite une authentification de base.  
  
 **Confirmer le mot de passe**  
 Tapez une seconde fois le mot de passe pour le valider. Ce mot de passe est obligatoire si le serveur SMTP nécessite une authentification de base.  
  
 **Authentification anonyme**  
 Le message est envoyé au serveur SMTP sans informations d'identification. Utilisez cette option lorsque le serveur SMTP ne nécessite pas d'authentification.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="NewProfile"></a> Page Nouveau profil  
 Utilisez cette page pour créer un profil de messagerie de base de données. Un profil de messagerie de base de données est une collection de comptes de messagerie de base de données. Les profils améliorent la fiabilité lorsqu'un serveur de messagerie électronique est inaccessible, grâce à la fourniture d'autres comptes de messagerie de base de données. Au moins un compte de messagerie de base de données est nécessaire. Pour plus d'informations sur la définition des priorités des comptes de messagerie de base de données dans le profil, consultez [Create a Database Mail Profile](../../relational-databases/database-mail/create-a-database-mail-profile.md).  
  
 Utilisez les boutons **Monter** et **Descendre** pour modifier l'ordre d'utilisation des comptes de messagerie de base de données. Cet ordre est déterminé par une valeur nommée numéro de séquence. **Monter** diminue le numéro de séquence et **Descendre** l'augmente. Le numéro de séquence détermine l'ordre d'utilisation des comptes de messagerie de base de données dans le profil. Pour un nouveau message électronique, la messagerie de base de données démarre avec le compte dont le numéro de séquence est le plus petit. Si ce compte échoue, la messagerie de base de données utilise le compte qui possède le numéro de séquence plus élevé suivant, et ainsi de suite jusqu'à ce qu'elle envoie le message correctement ou que le compte qui possède le numéro de séquence le plus élevé échoue. Si le compte possédant le numéro de séquence le plus élevé échoue, la messagerie de base de données tente d'envoyer le message pendant la durée configurée dans le paramètre **AccountRetryDelay** de la messagerie de base de données. Elle tente ensuite de nouveau le processus d'envoi du message en commençant par le numéro de séquence le plus petit. Utilisez le paramètre **AccountRetryAttempts** de la messagerie de base de données pour configurer le nombre de fois que le processus de messagerie externe tente d’envoyer le message à l’aide de chacun des comptes du profil spécifié. Vous pouvez configurer les paramètres **AccountRetryDelay** et **AccountRetryAttempts** dans la page **Configurer les paramètres du système** de l'Assistant Configuration de la messagerie de base de données.  
  
 **Nom de profil**  
 Tapez le nom du nouveau profil. Le profil est créé avec ce nom. N'utilisez pas le nom d'un profil existant.  
  
 **Description**  
 Tapez une description du profil. Cette description est facultative.  
  
 **Comptes SMTP**  
 Choisissez un ou plusieurs comptes pour le profil. La priorité définit l'ordre d'utilisation des comptes par la messagerie de base de données. Si aucun compte n'est répertorié, vous devez cliquer sur **Ajouter** pour poursuivre et ajouter un nouveau compte SMTP.  
  
 **Ajouter**  
 Ajoutez un compte au profil.  
  
 **Supprimer**  
 Supprimez le compte sélectionné de la liste.  
  
 **Monter**  
 Augmentez la priorité du compte sélectionné.  
  
 **Descendre**  
 Réduisez la priorité du compte sélectionné.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="ExistingProfile"></a> Page Gérer le profil existant  
 Utilisez cette page pour gérer un profil existant de la messagerie de base de données. Un profil de messagerie de base de données est une collection de comptes de messagerie de base de données. Les profils améliorent la fiabilité lorsqu'un serveur de messagerie électronique est inaccessible, grâce à la fourniture d'autres comptes de messagerie de base de données. Au moins un compte de messagerie de base de données est nécessaire. Pour plus d'informations sur la définition des priorités des comptes de messagerie de base de données dans le profil, consultez [Create a Database Mail Profile](../../relational-databases/database-mail/create-a-database-mail-profile.md).  
  
 Utilisez les boutons **Monter** et **Descendre** pour modifier l'ordre d'utilisation des comptes de messagerie de base de données. Cet ordre est déterminé par une valeur nommée numéro de séquence. **Monter** diminue le numéro de séquence et **Descendre** l'augmente. Le numéro de séquence détermine l'ordre d'utilisation des comptes de messagerie de base de données dans le profil. Pour un nouveau message électronique, la messagerie de base de données démarre avec le compte dont le numéro de séquence est le plus petit. Si ce compte échoue, la messagerie de base de données utilise le compte qui possède le numéro de séquence plus élevé suivant, et ainsi de suite jusqu'à ce qu'elle envoie le message correctement ou que le compte qui possède le numéro de séquence le plus élevé échoue. Si le compte possédant le numéro de séquence le plus élevé échoue, la messagerie de base de données tente d'envoyer le message pendant la durée configurée dans le paramètre **AccountRetryDelay** de la messagerie de base de données. Elle tente ensuite de nouveau le processus d'envoi du message en commençant par le numéro de séquence le plus petit. Utilisez le paramètre **AccountRetryAttempts** de la messagerie de base de données pour configurer le nombre de fois que le processus de messagerie externe tente d’envoyer le message à l’aide de chacun des comptes du profil spécifié. Vous pouvez configurer les paramètres **AccountRetryDelay** et **AccountRetryAttempts** dans la page **Configurer les paramètres du système** de l'Assistant Configuration de la messagerie de base de données.  
  
 **Nom de profil**  
 Sélectionnez le nom du profil à gérer.  
  
 **Supprimer**  
 Supprimez le profil sélectionné. Vous serez invité à sélectionner **Oui** pour supprimer le profil sélectionné et pour annuler les messages non envoyés, ou pour sélectionner **Non** afin de supprimer le profil sélectionné uniquement en l'absence de tout message non envoyé  
  
 **Description**  
 Affichez ou modifiez la description du profil sélectionné. Cette description est facultative.  
  
 **Comptes SMTP**  
 Choisissez un ou plusieurs comptes pour le profil. La priorité de basculement définit l'ordre dans lequel la messagerie de base de données utilise le compte en cas de basculement.  
  
 **Ajouter**  
 Ajoutez un compte au profil.  
  
 **Supprimer**  
 Supprimez le compte sélectionné de la liste.  
  
 **Monter**  
 Augmentez la priorité de basculement du compte sélectionné.  
  
 **Descendre**  
 Réduisez la priorité de basculement du compte sélectionné.  
  
 **Priorité**  
 Affichez la priorité actuelle de basculement du compte.  
  
 **Nom du compte**  
 Affichez le nom du compte.  
  
 **E-mail Address**  
 Affichez l'adresse de messagerie du compte.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="AddAccount"></a> Add Account to Profile Page  
 Utilisez cette page pour choisir le compte à ajouter au profil. Choisissez un compte existant dans la zone **Nom du compte** ou cliquez sur **Nouveau compte**.  
  
 **Nom du compte**  
 Sélectionnez le nom du compte à ajouter au profil.  
  
 **Adresse de messagerie**  
 Affichez l'adresse de messagerie du compte sélectionné. Vous ne pouvez pas modifier l'adresse de messagerie dans cette page. Pour modifier l’adresse de messagerie du compte, revenez à la page principale de l’Assistant et sélectionnez l’option **Gérer les comptes et les profils de messagerie de base de données** .  
  
 **Nom du serveur**  
 Affichez le nom de serveur de messagerie du compte sélectionné. Vous ne pouvez pas modifier le nom du serveur dans cette page. Pour modifier le nom de serveur pour le compte, revenez à la page principale de l'Assistant et sélectionnez l'option **Gérer les comptes et les profils de messagerie de base de données** .  
  
 **Nouveau compte**  
 Créez un nouveau compte.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="AccountsProfiles"></a> Page Gérer les comptes et profils  
 Utilisez cette page pour choisir une tâche de gestion d'un profil ou d'un compte.  
  
 **Créer un nouveau compte**  
 Créez un nouveau compte.  
  
 **Afficher, modifier ou supprimer un compte existant**  
 Gérez ou supprimez un compte existant.  
  
 **Créer un nouveau profil**  
 Créez un nouveau profil.  
  
 **Afficher, modifier ou supprimer un profil existant. Vous pouvez également gérer les comptes associés au profil.**  
 Mettez à jour ou supprimez un profil existant. Cette option permet également de gérer les comptes associés au profil.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="ProfileSecurityPublic"></a> Gérer la sécurité des profils, onglet Public  
 Utilisez cette page pour configurer un profil public.  
  
 Les profils sont soit publics soit privés. Un profil privé n'est accessible qu'à des utilisateurs ou des rôles spécifiques. Un profil public permet à tout utilisateur ou rôle ayant accès à la base de données hôte de messagerie (**msdb**) d’envoyer des messages en utilisant ce profil.  
  
 Un profil peut être un profil par défaut. Dans ce cas, les utilisateurs ou les rôles peuvent envoyer des messages électroniques avec ce profil, sans le spécifier explicitement. Si l'utilisateur ou le rôle qui envoie ce message possède un profil privé par défaut, la messagerie de base de données utilise ce profil. Si l’utilisateur ou le rôle ne possède pas de profil privé par défaut, **sp_send_dbmail** utilise le profil public par défaut pour la base de données **msdb** . S’il n’existe ni profil privé par défaut pour l’utilisateur ou le rôle, ni profil public par défaut pour la base de données, **sp_send_dbmail** renvoie une erreur. Un seul profil peut être marqué comme profil par défaut.  
  
 **Public**  
 Sélectionnez cette option pour rendre public le profil spécifié.  
  
 **Profile Name**  
 Affiche le nom du profil.  
  
 **Profil par défaut**  
 Sélectionnez cette option pour faire du profil spécifié le profil par défaut.  
  
 **Afficher uniquement les profils publics existants**  
 Sélectionnez cette option pour n'afficher que les profils publics présents dans la base de données spécifiée.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="ProfileSecurityPrivate"></a> Gérer la sécurité des profils, onglet Privé  
 Utilisez cette page pour configurer un profil privé.  
  
 Les profils sont soit publics soit privés. Un profil privé n'est accessible qu'à des utilisateurs ou des rôles spécifiques. Un profil public permet à tout utilisateur ou rôle ayant accès à la base de données hôte de messagerie (**msdb**) d’envoyer des messages en utilisant ce profil.  
  
 Un profil peut être un profil par défaut. Dans ce cas, les utilisateurs ou les rôles peuvent envoyer des messages électroniques avec ce profil, sans le spécifier explicitement. Si l'utilisateur ou le rôle qui envoie ce message possède un profil privé par défaut, la messagerie de base de données utilise ce profil. Si l’utilisateur ou le rôle ne possède pas de profil privé par défaut, **sp_send_dbmail** utilise le profil public par défaut pour la base de données **msdb** . S’il n’existe ni profil privé par défaut pour l’utilisateur ou le rôle, ni profil public par défaut pour la base de données, **sp_send_dbmail** renvoie une erreur.  
  
 **User name**  
 Pour sélectionner le nom d’un utilisateur ou d’un rôle dans la base de données **msdb** .  
  
 **Accès**  
 Pour choisir si l'utilisateur ou le rôle a accès au profil spécifié.  
  
 **Nom de profil**  
 Permet d'afficher le nom du profil.  
  
 **Est le profil par défaut**  
 Permet d'indiquer si ce profil représente le profil par défaut de l'utilisateur ou du rôle. Chaque utilisateur ou rôle ne peut avoir qu'un profil par défaut.  
  
 **Afficher uniquement les profils privés existants pour cet utilisateur**  
 Sélectionnez cette option pour n'afficher que les profils auxquels l'utilisateur ou le rôle spécifié a accès.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="SystemParameters"></a> Configurer les paramètres du système  
 Utilisez cette page pour spécifier les paramètres système de la messagerie de base de données. Affichez les paramètres du système et la valeur actuelle de chaque paramètre. Sélectionnez un paramètre pour afficher une brève description dans le volet d'informations.  
  
 **Tentatives de reprises de comptes**  
 Nombre de fois où le processus de messagerie externe tente d'envoyer le message électronique à l'aide de chaque compte présent dans le profil spécifié.  
  
 **Délai entre reprises de comptes (secondes)**  
 Durée, en secondes, qu'attend le processus de messagerie externe après son essai d'envoi d'un message avec tous les comptes du profil, avant d'essayer de nouveau tous les comptes.  
  
 **Taille de fichier maximale (octets)**  
 Taille maximale d'une pièce jointe, en octets.  
  
 **Extensions de fichiers joints interdites**  
 Liste séparée par des virgules des extensions qui ne peuvent pas être envoyées en pièces jointes dans un message électronique. Cliquez sur le bouton Parcourir (**...**) pour ajouter d’autres extensions.  
  
 **Durée de vie minimale de l'exécutable de la messagerie de base de données (secondes)**  
 Durée minimale (en secondes) pendant laquelle le processus de messagerie externe reste actif. Le processus demeure actif aussi longtemps qu'il reste des messages dans la file d'attente de la messagerie de base de données. Ce paramètre spécifie le délai pendant lequel le processus demeure actif s'il n'y a pas de messages à traiter.  
  
 **Niveau de journalisation**  
 Spécifiez quels messages sont enregistrés dans le journal de la messagerie de base de données. Les valeurs possibles sont :  
  
-   Normal : ne journalise que les erreurs  
  
-   Étendu : journalise les messages d'information, d'avertissement et d'erreur  
  
-   Commentaires : journalise les messages d'information, d'avertissement et d'erreur, les messages de réussite et les autres messages internes. Utilisez la journalisation commentée pour le dépannage.  
  
 La valeur par défaut est Étendu.  
  
 **Réinitialiser tout**  
 Sélectionnez cette option pour réinitialiser les valeurs de la page à leur valeur par défaut.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="CompleteWizard"></a> Page Fin de l'Assistant  
 Cette page vous permet de passer en revue les actions que doit effectuer l' **Assistant Configuration de la messagerie de base de données** . Aucune modification n'est apportée, tant que vous ne terminez pas l'Assistant.  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
###  <a name="TestEmail"></a> Send Test E-Mail Page  
 Utilisez la page **Envoyer un message électronique de test à partir de***<nom_instance>* pour envoyer un e-mail en utilisant le profil de messagerie de base de données spécifié. Seuls les membres du rôle serveur fixe **sysadmin** peuvent envoyer des messages électroniques de test avec cette page.  
  
 **Profil de messagerie de base de données**  
 Sélectionnez un profil de messagerie de base de données dans la liste. Ce champ est obligatoire. Si aucun profil ne s'affiche, soit il n'en existe pas, soit vous n'avez pas d'autorisation sur ces profils. Utilisez l' **Assistant Configuration de la messagerie de base de données** pour créer et configurer des profils. Si aucun profil n'est répertorié, utilisez l'Assistant Configuration de la messagerie de base de données pour créer un profil pour votre propre usage.  
  
 **Pour**  
 Adresse électronique des destinataires des messages. Au moins un destinataire est requis.  
  
 **Objet**  
 Ligne d'objet du message électronique de test. Changez l'objet par défaut pour mieux identifier votre message à des fins de dépannage.  
  
 **Corps**  
 Corps du message électronique de test. Changez l'objet par défaut pour mieux identifier votre message à des fins de dépannage.  
  
 La boîte de dialogue **Message électronique de test de la messagerie de base de données** confirme que la messagerie de base de données a tenté d’envoyer le message et fournit l’ID **mailitem_id** du message de test. Assurez-vous auprès du destinataire que le message lui est bien parvenu. Normalement, un message arrive en quelques minutes, mais il peut être retardé en raison de la lenteur du réseau, d'une surcharge de messages sur le serveur de messagerie ou d'une indisponibilité temporaire du serveur. Utilisez **mailitem_id** pour le dépannage.  
  
 **Message envoyé**  
 **mailitem_id** du message électronique de test.  
  
 **Dépanner**  
 Cliquez sur cette option pour ouvrir la rubrique de la documentation en ligne [Dépannage de la messagerie de base de données](http://msdn.microsoft.com/library/ms188663.aspx).  
  
 [Assistant Configuration de la messagerie de base de données](#DBWizard)  
  
##  <a name="Template"></a> Utilisation de modèles  
 **Pour créer un script de configuration de la messagerie de base de données**  
  
1.  Dans le menu **Affichage** , sélectionnez **Explorateur de modèles**.  
  
2.  Dans la fenêtre **Explorateur de modèles** , développez le dossier **Messagerie de base de données** .  
  
3.  Double-cliquez sur **Configuration simple de la messagerie de base de données**. Le modèle s'ouvre dans une nouvelle fenêtre de requête.  
  
4.  Dans le menu **Requête** , sélectionnez **Spécifier les valeurs des paramètres du modèle**. La fenêtre **Remplacer les paramètres de modèle** s'ouvre.  
  
5.  Tapez les valeurs des **profile_name**, **account_name**, **SMTP_servername**, **email_address**et **display_name**. SQL Server Management Studio renseigne le modèle avec les valeurs que vous fournissez.  
  
6.  Exécutez le script pour créer la configuration.  
  
7.  Le script n'accorde l'accès au profil à aucun utilisateur de base de données. En conséquence, par défaut, seuls les membres du rôle de sécurité fixe **sysadmin** peuvent utiliser le profil. Pour plus d’informations sur l’octroi d’accès aux profils, consultez [sysmail_add_principalprofile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-principalprofile-sp-transact-sql.md)  
  
  
