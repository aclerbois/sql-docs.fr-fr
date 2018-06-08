---
title: Ajouter des SQLRUserGroup en tant qu’un utilisateur de base de données (SQL Server Machine Learning) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 6f0877f04b35475dd4d1403390447adad0c3936c
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="add-sqlrusergroup-as-a-database-user"></a>Ajouter SQLRUserGroup en tant qu’un utilisateur de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article explique comment donner au groupe de comptes de travail utilisés par les services d’apprentissage machine dans SQL Server les autorisations requises pour se connecter à la base de données et exécuter des travaux R ou Python pour le compte de l’utilisateur.

## <a name="what-is-sqlrusergroup"></a>Qu’est SQLRUserGroup ?

Lors de l’installation de [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] ou [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)], nouveaux comptes d’utilisateur Windows sont créés pour prendre en charge l’exécution de R ou Python de tâches sous le jeton de sécurité de le [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] service.

Vous pouvez afficher ces comptes dans le groupe d’utilisateurs Windows **SQLRUserGroup**. Par défaut, 20 comptes de travail sont créés, ce qui est généralement plus que suffisant pour l’apprentissage en cours d’exécution de travaux.

Lorsqu’un utilisateur envoie un script à partir d’un client externe, d’apprentissage [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Active un compte de travail disponible, il est mappé à l’identité de l’utilisateur appelant et exécute le script de la part de l’utilisateur. Ce nouveau service du moteur de base de données prend en charge l’exécution sécurisée de scripts externes, appelé *l’authentification implicite*.

Toutefois, si vous devez exécuter des scripts R ou Python à partir d’un client de science des données distantes, et vous utilisez l’authentification Windows, vous devez attribuer ces comptes de travailleur autorisé à se connecter à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance à votre place.

## <a name="add-sqlrusergroup-as-a-sql-server-login"></a>Ajouter des SQLRUserGroup en tant qu’une connexion SQL Server

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans l’Explorateur d’objets, développez **Sécurité**, cliquez avec le bouton droit sur **Connexions**, puis sélectionnez **Nouvelle connexion**.

2. Dans le **nouvelle connexion** boîte de dialogue, sélectionnez **recherche**. (Ne tapez pas quoi que ce soit dans la zone encore.)
    
     ![Cliquez sur Rechercher pour ajouter le nouveau compte de connexion pour l’apprentissage](media/implied-auth-login1.png "cliquez sur Rechercher pour ajouter le nouveau compte de connexion pour l’apprentissage")

3. Dans le **sélectionner utilisateur ou groupe** , cliquez sur le **Types d’objet** bouton.

     ![Rechercher des types d’objets pour ajouter le nouveau compte de connexion pour l’apprentissage](media/implied-auth-login2.png "recherche les types d’objets pour ajouter le nouveau compte de connexion pour l’apprentissage")

4. Dans le **les Types d’objets** boîte de dialogue, sélectionnez **groupes**. Désactivez toutes les autres cases à cocher.

     ![Sélectionnez les groupes dans la boîte de dialogue Types d’objet](media/implied-auth-login3.png "sélectionner des groupes dans la boîte de dialogue Types d’objet")

4. Cliquez sur **avancé**, vérifiez que l’emplacement de recherche est l’ordinateur actuel et puis cliquez sur **Rechercher maintenant**.

     ![Cliquez sur Rechercher maintenant pour obtenir la liste des groupes](media/implied-auth-login4.png "cliquez sur Rechercher pour obtenir la liste des groupes")

5. Faites défiler la liste des comptes de groupe sur le serveur jusqu'à ce que vous trouviez une commençant par `SQLRUserGroup`.
    
    + Le nom du groupe associé avec le service Launchpad pour le _instance par défaut_ est toujours **SQLRUserGroup**, indépendamment de si vous avez installé R, Python ou les deux. Sélectionner ce compte pour l’instance par défaut uniquement.
    + Si vous utilisez un _instance nommée_, le nom d’instance est ajouté au nom du nom de groupe de travail par défaut, `SQLRUserGroup`. Par conséquent, si votre instance est nommée « MLTEST », le nom du groupe utilisateur par défaut pour cette instance serait **SQLRUserGroupMLTest**.
 
     ![Exemple de groupes sur le serveur](media/implied-auth-login5.png "exemple de groupes sur le serveur")
   
5. Cliquez sur **OK** pour fermer la boîte de dialogue Recherche avancée.

    > [!IMPORTANT]
    > Veillez à ce que vous avez sélectionné le compte approprié pour l’instance. Chaque instance peut utiliser uniquement son propre service Launchpad et le groupe créé pour ce service. Les instances ne peuvent pas partager un compte de service ou de travail Launchpad.

6. Cliquez sur **OK** pour fermer la **sélectionner utilisateur ou groupe** boîte de dialogue.

7. Dans le **nouvelle connexion** boîte de dialogue, cliquez sur **OK**. Par défaut, la connexion est affectée au rôle **public** et est autorisée à se connecter au moteur de base de données.

## <a name="change-the-number-of-worker-accounts-in-sqlrusergroup"></a>Modifier le nombre de comptes de travail dans SQLRUserGroup

Si vous avez l’intention de faire un usage intensif de l’apprentissage, vous pouvez augmenter le nombre de comptes utilisés pour exécuter des scripts externes, comme décrit dans cet article : 

+ [Modifier le pool de comptes d’utilisateur pour l’apprentissage](modify-the-user-account-pool-for-sql-server-r-services.md)

Par défaut, 20 comptes sont créés, qui prend en charge 20 sessions simultanées. Tâches parallélisées n’utilisent pas des comptes supplémentaires. Par exemple, si un utilisateur exécute une tâche de calcul de score qui utilise le traitement parallèle, le même compte de processus de travail est réutilisé pour tous les threads.
