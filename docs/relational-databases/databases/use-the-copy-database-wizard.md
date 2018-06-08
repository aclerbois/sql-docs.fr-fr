---
title: Utiliser l’Assistant Copie de base de données | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.cdw.packageconfiguration.f1
- sql13.swb.cdw.schedule.f1
- sql13.swb.cdw.transfermethod.f1
- sql13.swb.cdw.databases.f1
- sql13.swb.cdw.destinationconfiguration.f1
- sql13.swb.cdw.destination.f1
- sql13.swb.cdw.locationofsourcedbfiles.f1
- sql13.swb.cdw.source.f1
- sql13.swb.cdw.selectdatabaseobjects.f1
- sql13.swb.cdw.complete.f1
- sql13.swb.cdw.welcome.f1
helpviewer_keywords:
- Copy Database Wizard
- starting Copy Database Wizard
ms.assetid: 7a999fc7-0a26-4a0d-9eeb-db6fc794f3cb
caps.latest.revision: 64
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 298bc4c6f485f89b24e43536644dcca02d22dbf9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-the-copy-database-wizard"></a>Utiliser l'Assistant Copie de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
L’Assistant Copie de base de données déplace ou copie des bases de données et certains objets serveur facilement à partir d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  vers une autre instance, sans temps d’arrêt du serveur. À l'aide de cet Assistant, vous pouvez effectuer les opérations suivantes : 
  
-   Choisir un serveur source et un serveur de destination.  
  
-   Sélectionner les bases de données à déplacer ou copier.  
  
-   Spécifier l’emplacement des fichiers des bases de données.  
  
-   Copier les connexions au serveur de destination.  
  
-   Copier d'autres objets de support, travaux, procédures stockées définies par l'utilisateur et messages d'erreur.  
  
-   Programmer l’heure à laquelle déplacer ou copier les bases de données.  
  

  
##  <a name="Restrictions"></a> Limitations et restrictions  
  
-   L'Assistant Copie de base de données n'est pas disponible dans l'édition Express.  
  
-   L’Assistant Copie de base de données ne peut pas être utilisé pour copier ou déplacer les bases de données :
  
    -   système ;
  
    -   marquées pour la réplication ;
  
    -   marquées comme Inaccessible, Chargement, Déconnecté, Récupération, Suspect ou en Mode urgence ;
    
    -   qui ont des fichiers journaux ou de données stockés dans Microsoft Azure Storage.
  
-   Une base de données ne peut pas être déplacée ni copiée vers une version antérieure de SQL Server.
  
-   Si vous sélectionnez l'option **Déplacer** , l'Assistant supprime automatiquement la base de données source après avoir déplacé la base de données. L'Assistant Copie de base de données ne supprime pas une base de données source si vous sélectionnez l'option **Copier** .  De plus, les objets serveur sélectionnés sont copiés plutôt que déplacés vers la destination ; la base de données est le seul objet qui est réellement déplacé.
  
-   Si vous utilisez la méthode [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Object pour déplacer le catalogue de texte intégral, vous devez de nouveau remplir l’index après le déplacement.  
  
-   La méthode de **détachement et d’attachement** permet de détacher la base de données, de déplacer ou copier les fichiers .mdf, .ndf et .ldf de la base de données, puis de rattacher la base de données à son nouvel emplacement. En cas d’utilisation de la méthode de **détachement et d’attachement** , les sessions actives ne peuvent pas être attachées à la base de données en cours de déplacement ou de copie, ceci afin d’éviter une perte ou une incohérence des données. Dans le cas de la méthode [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Object, les sessions actives sont autorisées car la base de données n'est jamais placée en mode hors connexion.  

-    Le transfert de travaux SQL Server Agent qui référencent des bases de données qui n’existent pas déjà sur le serveur de destination entraîne l’échec de l’opération entière.  L’Assistant tente de créer un travail SQL Server Agent avant de créer la base de données.  Solution de contournement :
     1. Créez une base de données shell sur le serveur de destination portant le même nom que la base de données à copier ou à déplacer.  Consultez [Créer une base de données](../../relational-databases/databases/create-a-database.md).
     
     2. Dans la page **Configurer la base de données de destination** , sélectionnez **Supprimer les bases de données portant le même nom sur le serveur de destination, puis poursuivre le transfert de base de données en remplaçant les fichiers de base de données existants**.

> **IMPORTANT** La méthode d’ **attachement et de détachement** attribue la propriété de la base de données source et de destination à la connexion exécutant l’ **Assistant Copie de base de données**.  Consultez [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md) pour modifier la propriété d’une base de données.
  
  
##  <a name="Prerequisites"></a> Conditions préalables  
-   Vérifiez que SQL Server Agent est démarré sur le serveur de destination.  

-   Vérifiez que les répertoires de fichiers journaux et de données sur le serveur source sont accessibles à partir du serveur de destination.

-   Sous la méthode de **détachement et d’attachement** , un proxy SQL Server Agent pour le sous-système SSIS doit exister sur le serveur de destination avec les informations d’identification qui peuvent accéder au système de fichiers des serveurs source et de destination. Pour plus d’informations sur les proxys, consultez [Créer un proxy SQL Server Agent](~/ssms/agent/create-a-sql-server-agent-proxy.md).

> **IMPORTANT** Sous la méthode de **détachement et d’attachement** , le processus de copie ou de déplacement échoue si aucun compte proxy Integration Services n’est utilisé.  Sous certaines circonstances, la base de données source n’est pas à nouveau attachée au serveur source et toutes les autorisations de sécurité NTFS sont supprimées des fichiers journaux et de données.  Dans ce cas, accédez à vos fichiers, ré-appliquez les autorisations appropriées, puis attachez à nouveau la base de données à votre instance de SQL Server.
  
##  <a name="Recommendations"></a> Recommandations  
  
-   Pour garantir des performances optimales de la base de données mise à niveau, exécutez [sp_updatestats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) (mise à jour des statistiques) sur la base de données mise à niveau.  
  
-   Lorsque vous déplacez ou copiez une base de données sur une autre instance de serveur et il est possible que vous deviez recréer sur cette autre instance de serveur une partie ou l’ensemble des métadonnées de la base de données, telles que les connexions et les travaux, afin de garantir la cohérence pour les utilisateurs et les applications. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  

  
###  <a name="Permissions"></a> Permissions  
 Vous devez être membre du rôle de serveur fixe **sysadmin** sur le serveur source et sur le serveur de destination.  
  
##  <a name="Overview"></a> Pages de l’Assistant Copie de base de données 
Lancez l’ **Assistant Copie de base de données** dans SQL Server Management Studio à partir de l’ **Explorateur d’objets** et développez **Bases de données**.  Ensuite, cliquez avec le bouton droit sur une base de données, pointez vers **Tâches**et cliquez sur **Copier la base de données**.  Si la page d’accueil **Bienvenue dans l’Assistant Copie de base de données** s’affiche, cliquez sur **Suivant**.


### <a name="select-a-source-server"></a>Sélectionner un serveur source
Utilisé pour spécifier le serveur sur lequel se trouve la base de données à déplacer ou copier, puis pour entrer les informations de connexion.  Une fois la méthode d'authentification sélectionnée et les informations de connexion entrées, cliquez sur **Suivant** afin d'établir la connexion au serveur source.  Cette connexion reste active tout au long de la session.

-    **Serveur source**  
Utilisé pour identifier le nom du serveur sur lequel se trouvent les bases de données à déplacer ou copier.  Entrez manuellement ou cliquez sur le bouton de sélection pour accéder au serveur voulu.  La version du serveur doit être au moins SQL Server 2005.

-    **Utiliser l'authentification Windows**  
Permet à un utilisateur de se connecter par le biais d’un compte d’utilisateur Microsoft Windows.

-    **Utiliser l’authentification SQL Server**  
Permet à un utilisateur de se connecter en fournissant un nom d’utilisateur et un mot de passe d’authentification SQL Server.

     -    **User name**  
Utilisé pour entrer le nom d’utilisateur avec lequel se connecter. Cette option est disponible uniquement si vous avez choisi de vous connecter via l’ **authentification SQL Server**.

     -    **Mot de passe**  
Utilisé pour entrer le mot de passe du compte de connexion. Cette option est disponible uniquement si vous avez choisi de vous connecter via l’ **authentification SQL Server**.

###  <a name="select-a-destination-server"></a>Sélectionner un serveur de destination
Utilisé pour spécifier le serveur sur lequel la base de données est déplacée ou copiée.  Si vous spécifiez la même instance de serveur pour le serveur source et le serveur de destination, la base de données est copiée.  Dans ce cas, vous devrez renommer la base de données à une étape ultérieure de l'Assistant.  Le nom de la base de données source ne peut être utilisé pour la base de données copiée ou déplacée que s'il n'y a pas de conflits de nom sur le serveur de destination.  En cas de conflits de noms, vous devez les résoudre manuellement sur le serveur de destination avant de pouvoir y utiliser le nom de la base de données de source.
  
-    **Serveur de destination**  
Utilisé pour identifier le nom du serveur sur lequel se trouvent les bases de données à déplacer ou copier.  Entrez manuellement ou cliquez sur le bouton de sélection pour accéder au serveur voulu.  La version du serveur doit être au moins SQL Server 2005.

     >**REMARQUE :** vous pouvez utiliser une destination qui est un serveur cluster ; l’Assistant Copie de base de données s’assurera que vous sélectionnez uniquement des lecteurs partagés sur un serveur de destination cluster.

-    **Utiliser l'authentification Windows**  
Permet à un utilisateur de se connecter par le biais d’un compte d’utilisateur Microsoft Windows.

-    **Utiliser l’authentification SQL Server**  
Permet à un utilisateur de se connecter en fournissant un nom d’utilisateur et un mot de passe d’authentification SQL Server.

     -    **User name**  
Utilisé pour entrer le nom d’utilisateur avec lequel se connecter. Cette option est disponible uniquement si vous avez choisi de vous connecter via l’ **authentification SQL Server**.

     -    **Mot de passe**  
Utilisé pour entrer le mot de passe du compte de connexion. Cette option est disponible uniquement si vous avez choisi de vous connecter via l’ **authentification SQL Server**.

###  <a name="select-the-transfer-method"></a>Sélectionner la méthode de transfert
 
-    **Utiliser la méthode de détachement et d'attachement**  
Détachez la base de données du serveur source, copiez les fichiers de base de données (.mdf, .ndf et .ldf) sur le serveur de destination, puis attachez la base de données au serveur de destination. Cette méthode est généralement la plus rapide, car le travail principal consiste à lire le disque source et à écrire sur le disque de destination. Aucune logique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est requise pour créer des objets au sein de la base de données ou pour créer des structures de stockage de données. Cependant, cette méthode peut être plus lente si la base de données contient une quantité importante d'espace alloué, mais inutilisé. Par exemple, une nouvelle base de données pratiquement vide qui est créée en allouant 100 Mo copie la totalité des 100 Mo, même si seulement 5 Mo sont utilisés.

     > **REMARQUE** Cette méthode ne permet pas aux utilisateurs d’avoir accès à la base de données pendant le transfert.

     -    **En cas d'échec, rattachez la base de données source.**  
     Lorsqu'une base de données est copiée, les fichiers de la base de données d'origine sont toujours rattachés au serveur source. Utilisez cette case pour rattacher les fichiers d'origine à la base de données source si un déplacement de base de données ne peut pas être achevé. 
  
-    **Utiliser la méthode de transfert SQL Management Object**  
     Cette méthode lit la définition de chaque objet de base de données dans la base de données source et crée chaque objet dans la base de données de destination. Elle transfère ensuite les données des tables sources vers les tables de destination en recréant les index et les métadonnées.
  
     > [!NOTE]
     >  Les utilisateurs peuvent continuer à accéder à la base de données pendant le transfert. 
  
###  <a name="select-database"></a>Sélectionner une base de données
Sélectionnez les bases de données à déplacer ou copier depuis le serveur source vers le serveur de destination.  Consultez [Limitations et Restrictions](#Restrictions) en haut de la rubrique.  
  
-    **Déplacer**  
Déplace la base de données vers le serveur de destination.

-    **Copier**  
Copie la base de données sur le serveur de destination.

-    **Source**  
Permet d'afficher les bases de données existant déjà sur le serveur de destination.

-    **État**  
Affiche diverses informations de la base de données source.

-    **Actualiser**  
Permet d'actualiser la liste des bases de données.
  
### <a name="configure-destination-database"></a>Configurer la base de données de destination
Modifiez le nom de la base de données si besoin et spécifiez l’emplacement ainsi que les noms des fichiers de base de données.  Cette page s'affiche une seule fois, chaque fois qu'une base de données est déplacée ou copiée.

-    **Base de données source**  
Nom de la base de données source.  La zone de texte n’est pas modifiable.

-    **Base de données de destination**  
Nom de la base de données de destination à créer. Modifiez-le selon vos besoins.

-    **Fichiers de la base de données de destination :**  
     
     -    **Nom du fichier**  
Nom du fichier de la base de données de destination à créer. Modifiez-le selon vos besoins.

     -    **Taille (Mo)**  
Taille du fichier de la base de données de destination en mégaoctets.

     -    **Dossier de destination**  
Dossier sur le serveur de destination servant à héberger le fichier de la base de données de destination. Modifiez-le selon vos besoins.

     -    **État**  
État

-    **Si la base de données de destination existe déjà :**  
     Déterminer l’action à entreprendre si la base de données de destination déjà existe.

     -    **Arrêter le transfert si une base de données ou un fichier portant le même nom existe à l'emplacement de destination.**

     -    **Supprimer les bases de données portant le même nom sur le serveur de destination, puis poursuivre le transfert de base de données en remplaçant les fichiers de base de données existants.**

###  <a name="select-server-objects"></a>Sélectionner des objets serveur 
Cette page est disponible uniquement lorsque la source et la destination correspondent à des serveurs différents.  
  
-    **Objets connexes disponibles**  
Répertorie les objets disponibles pour le transfert vers le serveur de destination.  Pour inclure un objet, cliquez sur son nom dans la zone **Objets connexes disponibles** , puis cliquez sur le bouton **>>** pour déplacer l’objet vers la zone **Objets connexes sélectionnés** . 

-    **Objets connexes sélectionnés**  
Répertorie les objets qui seront transférés sur le serveur de destination.  Pour exclure un objet, cliquez sur son nom dans la zone **Objets connexes sélectionnés** , puis cliquez sur le bouton **<\<** pour déplacer l’objet vers la zone **Objets connexes disponibles** .  Par défaut, tous les objets de chaque type sélectionné sont transférés. Pour choisir des objets de tout type, cliquez sur le bouton de sélection (...) en regard d'un type d'objet dans la zone **Objets connexes sélectionnés** .  Cela permet d'afficher une boîte de dialogue dans laquelle vous pouvez sélectionner des objets individuels.

-    **Liste des objets serveur** 

      -    **Connexions (sélectionnées par défaut)** 
  
     -    **travaux de l'Agent SQL Server**  
  
     -    **Messages d'erreur définis par l'utilisateur**  
  
     -    **Points de terminaison**  
  
     -    **Catalogue de texte intégral** 
  
     -    **Package SSIS**  
     
     -    **Procédures stockées de la base de données master** 
          >**REMARQUE** Les procédures stockées étendues et leurs DLL associées ne peuvent faire l’objet d’une copie automatique.  
    
  
###   <a name="location-of-source-database-files"></a>Emplacement des fichiers de la base de données source
Cette page est disponible uniquement lorsque la source et la destination correspondent à des serveurs différents.  Spécifiez un partage de système de fichiers qui contient les fichiers de base de données sur le serveur source.
  
-    **Sauvegarde de la base de données**  
     Affiche le nom de chaque base de données en cours de déplacement.  
  
-    **Emplacement du dossier**  
Emplacement du dossier des fichiers de base de données sur le serveur source.
Par exemple : `C:\Program Files\Microsoft SQL Server\MSSQL110.MSSQLSERVER\MSSQL\DATA`.

  
-    **Partage de fichier sur le serveur source**  
Partage de fichiers contenant les fichiers de base de données sur le serveur source.  Entrez manuellement le partage ou cliquez sur le bouton de sélection pour accéder au partage.
Par exemple : `\\server_name\C$\Program Files\Microsoft SQL Server\MSSQL110.MSSQLSERVER\MSSQL\Data`.

### <a name="configure-the-package"></a>Configurer le package
L’Assistant Copie de base de données crée un package SSIS pour transférer la base de données.

-    **Emplacement du package**  
Affiche l’emplacement où le package SSIS sera écrit.

-    **Nom du package**  
Un nom par défaut du package SSIS est créé. Modifiez-le selon vos besoins.

-    **Options du journal**  
Précisez si les informations du journal doivent être stockées dans le journal des événements Windows ou dans un fichier texte.

-    **Chemin d'accès au fichier journal des erreurs**  
Cette option est disponible uniquement si l'option Enregistrement dans un fichier texte est sélectionnée.  Fournissez un chemin d'accès pour l'emplacement du fichier journal. 

### <a name="schedule-the-package"></a>Planifier le package
Spécifiez à quel moment vous voulez que l’opération de déplacement ou de copie démarre.  Si vous n’êtes pas administrateur système, vous devez spécifier un compte proxy SQL Server Agent qui a accès au sous-système d’exécution du package Integration Services (SSIS).

> **IMPORTANT** Un compte proxy Integration Services doit être utilisé sous la méthode de **détachement et d’attachement** .  

-    **Exécuter immédiatement**  
     Le package SSIS s’exécute une fois l’Assistant terminé.
  
-    **Planification**  
     Le package SSIS s’exécute selon une planification. 
  
     -    **Modifier la planification**   
Ouvrez la boîte de dialogue **Nouvelle planification du travail** .  Configurez selon vos besoins.  Lorsque vous avez terminé, cliquez sur **OK** .


-    **Compte proxy Integration Services** Sélectionnez un compte proxy disponible dans la liste déroulante.  Pour planifier le transfert, l’utilisateur doit avoir à sa disposition au moins un compte proxy disposant des autorisations nécessaires pour accéder au **sous-système d’exécution du package SSIS**.

        Pour créer un compte proxy pour l’exécution du package SSIS, dans l’ **Explorateur d’objets**, développez **SQL Server Agent**, puis **Proxys**, cliquez avec le bouton droit sur **Exécution du package SSIS**, puis cliquez sur **Nouveau proxy**.

### <a name="complete-the-wizard"></a>Terminer l’Assistant
Affiche un résumé des options sélectionnées.  Cliquez sur **Précédent** pour modifier une option.  Cliquez sur **Terminer** pour créer le package SSIS. La page **Exécution de l’opération** permet de surveiller les informations d’état relatives à l’exécution de l’ **Assistant Copie de base de données**.

-    **Action**  
 Liste chaque action en cours de réalisation.

-    **État**  
 Indique si l’action a réussi ou échoué dans sa globalité.

-    **Boîte de**  
Affiche les messages retournés par chaque étape.

##  <a name="Examples"></a> Exemples
### <a name="common-steps"></a>**Étapes courantes** 
Que vous choisissiez **Déplacer** ou **Copier**, **Détacher et attacher** ou **SMO**, les cinq étapes répertoriées ci-dessous sont les mêmes.  Par souci de concision, ces étapes ne sont répertoriées ici qu’une seule fois et tous les exemples commencent à l’ **Étape 6**.

1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur la base de données souhaitée, pointez sur **Tâches**, puis cliquez sur **Copier la base de données**.

3.  Si la page d’accueil **Bienvenue dans l’Assistant Copie de base de données** s’affiche, cliquez sur **Suivant**.

4.  Page**Sélectionner un serveur source** : spécifiez le serveur avec la base de données à déplacer ou copier.  Sélectionnez la méthode d'authentification.  Si vous choisissez **Utiliser l’authentification SQL Server** , vous devez entrer vos informations d’identification de connexion.  Cliquez sur **Suivant** pour établir la connexion au serveur source.  Cette connexion reste active tout au long de la session.

5.  Page**Sélectionner un serveur de destination** : spécifiez le serveur sur lequel la base de données sera déplacée ou copiée.  Sélectionnez la méthode d'authentification.  Si vous choisissez **Utiliser l’authentification SQL Server** , vous devez entrer vos informations d’identification de connexion.  Cliquez sur **Suivant** pour établir la connexion au serveur source.  Cette connexion reste active tout au long de la session.

     > **REMARQUE** Vous pouvez lancer l’Assistant Copie de base de données à partir d’une base de données.  Vous pouvez utiliser l’Assistant Copie de base de données à partir du serveur source ou de destination.
  
### <a name="a--move-database-using-detach-and-attach-method-to-an-instance-on-a-different-physical-server--a-login-and-sql-server-agent-job-will-be-moved-as-well"></a>**A.  Déplacez la base de données en utilisant la méthode de détachement et d’attachement vers une instance située sur un autre serveur physique.  Une connexion et un travail SQL Server Agent sont également déplacés.**  
L’exemple suivant déplace la base de données `Sales` , une connexion Windows nommée `contoso\Jennie` et un travail SQL Server Agent nommé `Jennie’s Report` depuis une instance 2008 de SQL Server sur `Server1` vers une instance 2016 de SQL Server sur `Server2`.  `Jennie’s Report` utilise la base de données `Sales` .  `Sales` n’existe pas déjà sur le serveur de destination `Server2`.  `Server1` sera réaffecté à une autre équipe après le déplacement de la base de données.
  
6.  Comme indiqué dans [Limitations et Restrictions](#Restrictions)ci-dessus, une base de données shell devra être créée sur le serveur de destination lors du transfert d’un travail SQL Server Agent qui référence une base de données qui n’existe pas déjà sur le serveur de destination.  Créez une base de données shell appelée `Sales` sur le serveur de destination. 

7.  De retour dans l’ **Assistant**, page **Sélectionner la méthode de transfert** : vérifiez et conservez les valeurs par défaut.  Cliquez sur **Suivant**.
  
8.  Page**Sélectionner les bases de données** : cochez la case **Déplacer** pour la base de données souhaitée, `Sales`.  Cliquez sur **Suivant**.
  
9.  Page**Configurer la base de données de Destination** : l’ **Assistant** a déterminé que `Sales` existe déjà sur le serveur de destination, tel qu’elle a été créée à l’ **étape 6** ci-dessus et a ajouté `_new` au nom de la **base de données de destination** .  Supprimez `_new` de la zone de texte **Base de données de destination** .  Si vous le souhaitez, modifiez le **nom de fichier**et le **dossier de destination**.  Sélectionnez **Supprimer les bases de données portant le même nom sur le serveur de destination, puis poursuivre le transfert de base de données en remplaçant les fichiers de base de données existants**.  Cliquez sur **Suivant**.
  
10. Page**Sélectionner des objets serveur** : dans le panneau **Objets connexes sélectionnés** , cliquez sur le bouton de sélection **Connexions des noms d’objets**.  Sous **Options de copie** , sélectionnez **Copier uniquement les connexions sélectionnées**.  Cochez la case **Afficher toutes les connexions serveur**.  Cochez la case **Connexion** pour `contoso\Jennie`.  Cliquez sur **OK**.  Dans le panneau **Objets connexes disponibles** , sélectionnez **Travaux SQL Server Agent** , puis cliquez sur le bouton **>** .  Dans le panneau **Objets connexes sélectionnés** , cliquez sur le bouton de sélection **Travaux SQL Server Agent**.  Sous **Options de copie** , sélectionnez **Copier uniquement les travaux sélectionnés**.  Cochez la case pour `Jennie’s Report`.  Cliquez sur **OK**.  Cliquez sur **Suivant**.  
  
11. Page**Emplacement des fichiers de base de données source** : cliquez sur le bouton de sélection **Partage de fichiers sur le serveur source** et accédez à l’emplacement du dossier donné.  Par exemple, pour l’emplacement du dossier `D:\MSSQL13.MSSQLSERVER\MSSQL\DATA` , utilisez `\\Server1\D$\MSSQL13.MSSQLSERVER\MSSQL\DATA` pour **Partage de fichier sur le serveur source**.  Cliquez sur **Suivant**.
  
12. Page**Configurer le package** : dans la zone de texte **Nom du package** , entrez `SalesFromServer1toServer2_Move`.  Cochez la case **Enregistrer les journaux de transfert ?** .  Dans la liste déroulante **Options de journalisation** , sélectionnez **Fichier texte**.  Notez le **chemin du fichier journal des erreurs**; modifiez-le selon vos besoins.  Cliquez sur **Suivant**.  
  
     > **REMARQUE** Le **chemin du fichier journal des erreurs** est le chemin sur le serveur de destination.
  
13. Page**Planifier le package** : sélectionnez le proxy approprié dans la liste déroulante **Compte proxy Integration Services** .  Cliquez sur **Suivant**.

14. Page**Terminer l’Assistant** : passez en revue la synthèse des options sélectionnées.  Cliquez sur **Précédent** pour modifier une option.  Cliquez sur **Terminer** pour exécuter la tâche.  Au cours du transfert, la page **Exécution de l’opération** permet de surveiller les informations d’état relatives à l’exécution de l’ **Assistant**.

15. Page**Exécution de l’opération** : si l’opération a réussi, cliquez sur **Fermer**.  Si l’opération n’aboutit pas, passez en revue le journal des erreurs et cliquez éventuellement sur **Précédent** pour examiner les étapes antérieures.  Sinon, cliquez sur **Fermer**.
  
16. **Étapes postérieures au déplacement** Envisagez l’exécution des instructions T-SQL suivantes sur le nouvel hôte, `Server2`:
  
     ~~~ tsql 
     ALTER AUTHORIZATION ON DATABASE::Sales TO sa;

     ALTER DATABASE Sales 
     SET COMPATIBILITY_LEVEL = 130;

     USE Sales
     GO

     EXEC sp_updatestats;
     ~~~
 
17. **Nettoyage consécutif au déplacement**  
Étant donné que `Server1` sera déplacé vers une autre équipe et que l’opération de **déplacement** ne sera pas répétée, pensez à effectuer les étapes suivantes :
     -    Suppression du package SSIS `SalesFromServer1toServer2_Move` sur `Server2`.
     -    Suppression du travail SQL Server Agent `SalesFromServer1toServer2_Move` sur `Server2`.
     -    Suppression du travail SQL Server Agent `Jennie’s Report` sur `Server1`.
     -    Suppression de la connexion `contoso\Jennie` sur `Server1`.


### <a name="b-----copy-database-using-detach-and-attach-method-to-the-same-instance-and-set-recurring-schedule"></a>**B.     Copiez la base de données à l’aide de la méthode de détachement et d’attachement vers la même instance et définissez une planification périodique.**  
Dans cet exemple, la base de données `Sales` est copiée et créée en tant que `SalesCopy` sur la même instance.  Par la suite, `SalesCopy`est recréée toutes les semaines.

6.  Page**Sélectionner une méthode de transfert** : vérifiez et conservez les valeurs par défaut.  Cliquez sur **Suivant**.

7.  Page**Sélectionner les bases de données** : cochez la case **Copier** pour la base de données `Sales` .  Cliquez sur **Suivant**.

8.  Page**Configurer la base de données de Destination** : remplacez le nom de la **base de données de destination** par `SalesCopy`.  Si vous le souhaitez, modifiez le **nom de fichier**et le **dossier de destination**.  Sélectionnez **Supprimer les bases de données portant le même nom sur le serveur de destination, puis poursuivre le transfert de base de données en remplaçant les fichiers de base de données existants**.  Cliquez sur **Suivant**.

9.  Page**Configurer le package** : dans la zone de texte **Nom du package** , entrez `SalesCopy Weekly Refresh`.  Cochez la case **Enregistrer les journaux de transfert ?** .  Cliquez sur **Suivant**.

10. Page**Planifier le package** : cliquez sur la case d’option **Planification** , puis cliquez sur le bouton **Modifier la planification** . 
 
    1. Page**Nouvelle planification du travail** : dans la zone **Nom** , entrez `Weekly on Sunday`. 
          
    2. Cliquez sur **OK**.

11. Sélectionnez le proxy approprié dans la liste déroulante **Compte proxy Integration Services** .  Cliquez sur **Suivant**.

12. Page**Terminer l’Assistant** : passez en revue la synthèse des options sélectionnées.  Cliquez sur **Précédent** pour modifier une option.  Cliquez sur **Terminer** pour exécuter la tâche.  Pendant la création du package, la page **Exécution de l’opération** permet de surveiller les informations d’état relatives à l’exécution de l’ **Assistant**.

13. Page**Exécution de l’opération** : si l’opération a réussi, cliquez sur **Fermer**.  Si l’opération n’aboutit pas, passez en revue le journal des erreurs et cliquez éventuellement sur **Précédent** pour examiner les étapes antérieures.  Sinon, cliquez sur **Fermer**.

14. Démarrez manuellement le travail SQL Server Agent nouvellement créé `SalesCopy weekly refresh`.  Passez en revue l’historique des travaux et vérifiez que `SalesCopy` existe maintenant sur l’instance.

  
##  <a name="FollowUp"></a> Suivi : après la mise à niveau d’une base de données  
 Après avoir utilisé l'Assistant Copie de base de données pour mettre à niveau une base de données d'une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la base de données est immédiatement disponible et est automatiquement mise à niveau. Si la base de données comprend des index de recherche en texte intégral, la mise à niveau les importe, les réinitialise ou les reconstruit, selon le paramètre de la propriété de serveur **Option de mise à niveau des index de recherche en texte intégral** . Si l’option de mise à niveau est définie sur **Importer** ou **Reconstruire**, les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l’option de mise à niveau est **Importer**, si un catalogue de texte intégral n’est pas disponible, les index de recherche en texte intégral associés sont reconstruits. Pour plus d’informations sur l’affichage ou la modification du paramètre de la propriété **Option de mise à niveau des index de recherche en texte intégral** , consultez [Gérer et surveiller la recherche en texte intégral pour une instance de serveur](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).  
  
 Si le niveau de compatibilité d'une base de données utilisateur est à 100 ou supérieur avant la mise à niveau, il reste le même après la mise à niveau. Si le niveau de compatibilité était à 90 dans la base de données mise à niveau, le niveau de compatibilité est défini à 100, ce qui correspond au niveau de compatibilité le plus bas pris en charge dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
 
 ## <a name="Post"></a> Considérations liées à l’après-copie/déplacement
 Pensez à effectuer les étapes suivantes après une **copie** ou un **déplacement**:
-    Modifiez la propriété des bases de données quand vous utilisez la méthode de détachement et d’attachement.
-    Supprimez les objets serveur sur le serveur source après un **déplacement**.
-    Supprimez le package SSIS créé par l’Assistant sur le serveur de destination.
-    Supprimez le travail SQL Server Agent créé par l’Assistant sur le serveur de destination.

  
## <a name="more-information"></a>Informations complémentaires 
 [Mettre à niveau une base de données avec la méthode de détachement et d’attachement &#40;Transact-SQL&#41;](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md)   
 [Créer un proxy SQL Server Agent](http://msdn.microsoft.com/library/142e0c55-a8b9-4669-be49-b9dc602d5988)  
  
  

