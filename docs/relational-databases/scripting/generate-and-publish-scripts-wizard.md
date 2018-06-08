---
title: Assistant Générer et publier des scripts | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql9.swb.generatescriptswizard.chooseviews.f1
- sql13.swb.generatescriptswizard.manageproviders.f1
- sql9.swb.generatescriptswizard.scriptwizarddescription.f1
- sql9.swb.generatescriptswizard.choosedefaults.f1
- sql13.swb.generatescriptswizard.summarypage.f1
- sql13.swb.generatescriptswizard.providerconfiguration.f1
- sql9.swb.generatescriptswizard.chooseuddt.f1
- sql9.swb.generatescriptswizard.chooserules.f1
- sql13.swb.generatescriptswizard.introduction.f1
- sql13.swb.generatescriptswizard.setscriptingoptions.f1
- sql13.swb.generatescriptswizard.saveorpublishscripts.f1
- sql9.swb.generatescriptswizard.progress.f1
- sql9.swb.generatescriptswizard.chooseobjects.f1
- sql9.swb.generatescriptswizard.welcome.f1
- sql9.swb.generatescriptswizard.scriptfileoption.f1
- sql13.swb.generatescriptswizard.chooseobjects.f1
- sql13.swb.generatescriptswizard.advancedpublishingoptions.f1
- sql9.swb.generatescriptswizard.selectdatabase.f1
- sql9.swb.generatescriptswizard.choosetables.f1
- sql9.swb.generatescriptswizard.choosestoredprocedures.f1
- sql9.swb.generatescriptswizard.chooseobjecttypes.f1
- sql13.swb.generatescriptswizard.advancedscriptingoptions.f1
- sql9.swb.generatescriptswizard.choosescriptoptions.f1
- sql9.swb.generatescriptswizard.chooseudf.f1
helpviewer_keywords:
- databases [SQL Server], publishing
- publishing databases
- scripts [SQL Server], generating
- scripts [SQL Server], publishing
- databases [SQL Server], generating scripts
- Publish Database Wizard
ms.assetid: 5ee520ba-ec7e-4199-a441-189e9e264b37
caps.latest.revision: 45
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d755a56c4ee302ed0f249bac303b082b9031a239
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="generate-and-publish-scripts-wizard"></a>Assistant Générer et publier des scripts
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Vous pouvez utiliser l’ **Assistant Générer et publier des scripts** pour créer des scripts afin de transférer une base de données d’une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ou de [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]vers une autre. Vous pouvez générer des scripts pour une base de données sur une instance du moteur de base de données dans votre réseau local ou à partir de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Les scripts générés peuvent être exécutés sur une autre instance du moteur de base de données ou [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Vous pouvez également utiliser l'Assistant pour publier directement le contenu d'une base de données sur un service Web créé à l'aide des Services de publication de base de données. Vous pouvez créer des scripts pour une base de données entière ou les limiter à des objets spécifiques.  

Pour un tutoriel plus détaillé sur l’utilisation de l’Assistant Générer et publier des scripts, consultez [Tutoriel : Générer et publier des scripts](https://docs.microsoft.com/en-us/sql/ssms/tutorials/scripting-ssms#script-database-using-generate-scripts-option).


  
## <a name="before-you-begin"></a>Avant de commencer  
 Les bases de données source et cible peuvent se trouver sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]ou sur une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui exécute [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou une version ultérieure.  
  
###  <a name="PubHostSvc"></a> Publication sur un service hébergé  
 Outre pour la création de scripts, l' **Assistant Générer et publier des scripts** peut être utilisé pour publier une base de données sur un type spécifique de service Web SQL Server hébergé. Le SQL Server Hosting Toolkit fournit des Services de publication de base de données en tant que projet source partagé sur CodePlex. Le projet des Services de publication de base de données peut être utilisé par les fournisseurs d'hébergement Web pour générer un ensemble de services Web permettant aux clients de déployer facilement des bases de données sur le service Web. Pour plus d'informations sur le téléchargement du SQL Server Hosting Toolkit, consultez [SQL Server Database Publishing Services](http://go.microsoft.com/fwlink/?LinkId=142025)(en anglais).  
  
 Pour publier une base de données sur un service d'hébergement Web, sélectionnez l'option **Publier sur le service Web** dans la page de **Définir les options de script** de l'Assistant.  
  
###  <a name="Permissions"></a> Permissions  
 L'autorisation minimale pour publier une base de données est l'appartenance au rôle de base de données fixe db_ddladmin sur la base de données d'origine. L'autorisation minimale pour publier un script de base de données sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au fournisseur d'hébergement est l'appartenance au rôle de base de données fixe db_ddladmin sur la base de données cible.  
  
 L'utilisateur doit fournir également un nom d'utilisateur et un mot de passe pour accéder à son compte de fournisseur d'hébergement pour publier avec l'Assistant. La base de données cible doit être créée au fournisseur d'hébergement avant de publier la base de données source. La publication remplace les objets dans cette base de données existante.  
  
##  <a name="GenPubScriptWiz"></a> Utilisation de l'Assistant Générer et publier des scripts  
 **Pour générer ou publier un script**  
  
1.  Dans l' **Explorateur d'objets**, développez le nœud pour l'instance qui contient la base de données devant faire l'objet d'un script.  
  
2.  Pointez sur **Tâches**, puis cliquez sur **Générer des scripts**.  

    ![Assistant Générer des scripts](media/generate-and-publish-scripts-wizard/generatescripts.png)
  
3.  Renseignez les boîtes de dialogue de l'Assistant :  
  
    -   [Page Introduction](#Introduction)  
    -   [Page Sélectionner les objets](#ChooseObjects)   
    -   [Page Définir les options de script](#SetScriptOpt)  
    -   [Page Options de script avancées](#AdvScriptOpt)  
    -   [Page Gérer les fournisseurs](#MgProviders)   
    -   [Page Options de publication avancées](#AdvPubOpts)  
    -   [Page Configuration du fournisseur](#ProvConfig)  
    -   [Page Résumé](#Summary)   
    -   [Page Enregistrer ou publier des scripts](#SavePubScripts)  
  
###  <a name="Introduction"></a> Page Introduction  
 Cette page décrit la procédure de création ou de publication d'un script.  
  
 **Ne plus afficher cette page** - Permet d’ignorer cette page la prochaine fois que vous démarrez l’ **Assistant Générer et publier des scripts**.  
  
  ![Page Introduction](media/generate-and-publish-scripts-wizard/intro.png)
  
###  <a name="ChooseObjects"></a> Page Sélectionner les objets  
 Utilisez cette page pour choisir les objets que vous souhaitez inclure dans les scripts générés par cet Assistant. Dans la page suivante de l'Assistant, vous aurez la possibilité d'enregistrer ces scripts à l'emplacement de votre choix ou de les utiliser pour publier des objets de base de données sur un fournisseur d'hébergement Web distant sur lequel les [Services de publication de base de données SQL Server](http://go.microsoft.com/fwlink/?LinkId=142025)sont installés.  
  
 **Générer un script pour la base de données entière** - Cliquez sur cette option pour générer des scripts pour tous les objets de la base de données et pour inclure un script pour la base de données elle-même. 

   ![Générer un script de la base de données entière](media/generate-and-publish-scripts-wizard/scriptall.png) 
  
 **Sélectionner des objets de base de données spécifiques** - Cliquez sur cette option pour limiter l’action de l’Assistant à la génération de scripts uniquement pour les objets de la base de données que vous choisissez :  
  
-   **Objets de base de données** - Sélectionnez au moins un objet à inclure dans le script.  
  
-   **Sélectionner tout** - Coche toutes les cases disponibles.  
  
-   **Désélectionner tout** - Décoche toutes les cases. Vous devez ensuite activer au moins un objet de base de données pour poursuivre.  

   ![Sélectionner les objets à inclure dans le script](media/generate-and-publish-scripts-wizard/scriptspecificobjects.png)
  
###  <a name="SetScriptOpt"></a> Page Définir les options de script  
 Utilisez cette page pour spécifier si vous souhaitez que l'Assistant enregistre les scripts à l'emplacement de votre choix ou les utilise pour publier des objets de base de données sur un fournisseur d'hébergement Web distant. Pour publier, vous devez avoir accès à un service Web installé à l'aide des Database Publishing Services.  
  
 **Options** - Si vous souhaitez que l’Assistant enregistre les scripts à l’emplacement de votre choix, sélectionnez **Enregistrer les scripts à un emplacement spécifique**. Vous pourrez exécuter les scripts ultérieurement sur une instance du moteur de base de données ou sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Si vous souhaitez que l'Assistant publie vos objets de base de données sur un fournisseur d'hébergement Web distant, sélectionnez **Publier sur le service Web**.  
  
 **Enregistrer les scripts à un emplacement spécifique** - Permet d’enregistrer un ou plusieurs fichiers de script Transact-SQL à un emplacement que vous spécifiez.  

  ![Enregistrer](media/generate-and-publish-scripts-wizard/save.png)   
  
-   **Enregistrer dans le fichier** - Permet d’enregistrer le script dans un ou plusieurs fichiers .sql. Cliquez sur le bouton**Parcourir**pour spécifier le nom et l’emplacement du fichier. Activez la case à cocher **Remplacer le fichier existant** pour remplacer le fichier s'il en existe déjà un du même nom. Cliquez sur **Fichier unique** ou **Fichier unique par objet** pour spécifier comment les scripts doivent être générés. Cliquez sur **Texte Unicode** ou **Texte ANSI** pour spécifier le type de texte qui doit être utilisé dans le script.  
  
-   **Enregistrer dans le Presse-papiers** - Permet d’enregistrer le script Transact-SQL dans le Presse-papiers.  
  
-   **Enregistrer dans une nouvelle fenêtre de requête** - Permet de générer le script dans une fenêtre de l’Éditeur de requête du moteur de base de données. Si aucune fenêtre d'éditeur n'est ouverte, une nouvelle fenêtre d'éditeur s'ouvre comme cible du script.  
  
 **Publier sur le service Web** - Permet de publier les objets que vous avez sélectionnés sur un service d’hébergement web distant pour lequel vous avez configuré un fournisseur.  
  
-   **Gérer les fournisseurs** - Permet d’afficher la boîte de dialogue **Gérer les fournisseurs** . Utilisez la boîte de dialogue **Gérer les fournisseurs** pour ajouter, modifier et supprimer des fournisseurs d'hébergement. Chaque fournisseur spécifie les informations de connexion à un service d'hébergement Web, ainsi que les bases de données cibles sur ce service.  
  
-   **Avancé** - Permet d’afficher la boîte de dialogue **Options de publication avancées** dans laquelle vous pouvez sélectionner des options avancées pour publier des scripts.  
  
-   **Fournisseur** - Permet de sélectionner le fournisseur qui spécifie les informations de connexion au service d’hébergement web qui héberge la base de données sur laquelle vous souhaitez publier les objets que vous avez sélectionnés. Vous devez avoir au moins un fournisseur dans la boîte de dialogue **Gérer les fournisseurs** pour pouvoir sélectionner un fournisseur.  
  
-   **Base de données cible** - Permet de sélectionner la base de données cible sur laquelle vous souhaitez publier les objets que vous avez sélectionnés. Vous devez sélectionner un fournisseur avant de sélectionner une base de données cible.  
  
###  <a name="AdvScriptOpt"></a> Page Options de script avancées  
 Utilisez cette page pour spécifier la façon dont vous souhaitez que cet Assistant génère des scripts. De nombreuses options sont disponibles. Les options sont grisées si elles ne sont pas prises en charge par la version de SQL Server ou [!INCLUDE[ssSDS](../../includes/sssds-md.md)] spécifiée dans **Type de moteur de base de données**.  

![Options avancées](media/generate-and-publish-scripts-wizard/advanced.png)
  
 **Options** - Spécifiez les options avancées en sélectionnant une valeur dans la liste des paramètres disponibles située à droite de chaque option.  
  
 **Général** - Les options suivantes s'appliquent au script entier.  
  
-   **Remplissage ANSI** - Inclut **ANSI PADDING ON** dans le script. La valeur par défaut est **True**.  
  
-   **Ajouter au fichier** - Lorsque cette option a la valeur **True**, ce script est ajouté au bas d’un script existant, spécifié sur la page **Définir les options de script** . Lorsqu'elle a la valeur **False**, le nouveau script remplace un script précédent. La valeur par défaut est **False**.  
  
-   **Continuer l’exécution du script en cas d’erreur** - Lorsque cette option a la valeur **True**, l’exécution du script s’arrête en cas d’erreur. Lorsqu'elle a la valeur **False**, l'exécution du script continue. La valeur par défaut est **False**.  
  
-   **Convertir les UDDT en types de base** - Lorsque cette option a la valeur **True**, les types de données définis par l’utilisateur (UDDT) sont convertis en types de données de base sous-jacents, ceux-là même qui ont été utilisés pour les créer. Utilisez **True** lorsque l'UDDT n'existe pas dans la base de données où le script s'exécutera. Lorsque cette option a la valeur **False**, les UDDT sont utilisés. La valeur par défaut est **False**.  
  
-   **Générer un script pour les objets dépendants** - Génère un script pour tout objet dont la présence est requise lorsque le script de l’objet sélectionné est exécuté. La valeur par défaut est **True**.  
  
-   **Inclure des en-têtes descriptifs** - Lorsque cette option a la valeur **True**, des commentaires descriptifs sont ajoutés au script afin de le séparer en sections pour chaque objet. La valeur par défaut est **False**.  
  
-   **Inclure IF NOT EXISTS** - Lorsque cette option a la valeur **True**, le script inclut une instruction pour vérifier si l’objet existe déjà dans la base de données et il ne tente pas de créer l’objet si celui-ci existe déjà. La valeur par défaut est **False**.  
  
-   **Inclure des noms de contraintes système** - Lorsque cette option a la valeur **False**, la valeur par défaut des contraintes nommées automatiquement sur la base de données d’origine est renommée automatiquement sur la base de données cible. Lorsqu'elle a la valeur **True**, les contraintes ont le même nom sur les bases de données cible et d'origine.  
  
-   **Inclure des instructions non prises en charge** - Si la valeur est **False**, le script ne contient pas d’instructions pour les objets non pris en charge par la version du serveur ou le type de moteur sélectionné. Si la valeur est **True**, le script contient les objets non pris en charge. Chaque instruction concernant un objet non pris en charge sera accompagnée d'un commentaire stipulant que l'instruction doit être modifiée avant que le script puisse être exécuté sur la version de SQL Server ou le type de moteur sélectionné. La valeur par défaut est **False**.  
  
-   **Noms d’objet de qualification de schéma** - Inclut le nom de schéma dans le nom des objets créés. La valeur par défaut est **True**.  
  
-   **Liaisons de scripts** - Génère un script pour lier les objets de règle et les objets par défaut. La valeur par défaut est **False**. Pour plus d’informations, consultez [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md) et [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md).  
  
-   **Classement de script** - Inclut des informations de classement dans le script. La valeur par défaut est **False**. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
-   **Valeurs de script par défaut** - Inclut les objets par défaut utilisés pour définir les valeurs par défaut dans les colonnes de table. La valeur par défaut est **True**. Pour plus d’informations, consultez [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md).  
  
-   **Générer un script de création/suppression (DROP/CREATE)** - Avec **Générer un script de création (CREATE)**, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont incluses pour créer les objets. Avec **Générer un script de suppression (DROP)**, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont incluses pour supprimer les objets. Avec **Générer un script de création/suppression (DROP/CREATE)**, l'instruction de suppression [!INCLUDE[tsql](../../includes/tsql-md.md)] est incluse dans le script, suivie de l'instruction de création, pour chaque objet faisant l'objet d'un script. La valeur par défaut est **Générer un script de création (CREATE)**.  
  
-   **Générer un script pour les propriétés étendues** - Inclut les propriétés étendues dans le script, si l’objet en possède. La valeur par défaut est **True**.  
  
-   **Générer un script pour le type de moteur** - Crée un script qui peut être exécuté sur le type sélectionné de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ou d’une instance du moteur de base de données SQL Server. Les objets non pris en charge sur le type spécifié ne sont pas inclus dans le script. La valeur par défaut est le type du serveur d'origine.  
  
-   **Générer un script pour la version du serveur** - Crée un script qui peut être exécuté sur la version sélectionnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fonctionnalités qui sont nouvelles dans une version ne peuvent pas faire l'objet d'un script pour les versions antérieures. La valeur par défaut est la version du serveur d'origine.  
  
-   **Générer un script pour les connexions** - Lorsque l’objet pour lequel générer le script est un utilisateur de base de données, cette option crée les connexions desquelles l’utilisateur dépend. La valeur par défaut est **False**.  
  
-   **Générer un script pour les autorisations au niveau objet** - Inclut des scripts pour définir l’autorisation sur les objets de la base de données. La valeur par défaut est **False**.  
  
-   **Générer un script pour les statistiques** - Quand cette option a la valeur **Générer un script pour les statistiques**, elle inclut l’instruction **CREATE STATISTICS** pour recréer des statistiques sur l’objet. L'option **Générer un script des statistiques et des histogrammes** crée également des informations d'histogramme. La valeur par défaut est **Ne pas générer de script des statistiques**. Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
-   **Script USE DATABASE** - Ajoute l’instruction **USE DATABASE** au script. Pour vous assurer que les objets de base de données sont créés dans la base de données correcte, incluez l'instruction **USE DATABASE** . Lorsqu'il est prévu que le script soit utilisé dans une base de données différente, sélectionnez **False** afin d'omettre l'instruction **USE DATABASE** . La valeur par défaut est **True**. Pour plus d’informations, consultez [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).  
  
-   **Types de données à inclure dans le script** - Sélectionne ce qui doit être inclus dans le script : **Données seulement**, **Schéma uniquement** ou les deux. La valeur par défaut est **Schéma uniquement**.  
  
 **Options de table/vue** - Les options suivantes s’appliquent uniquement aux scripts des tables et des vues.  
  
-   **Générer le script de suivi des modifications** - Génère le script de suivi des modifications s’il est activé sur la base de données d’origine ou sur des tables dans la base de données d’origine. La valeur par défaut est **False**. Pour plus d’informations, consultez [À propos du suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md).  
  
-   **Générer un script pour les contraintes de validation** – Ajoute des contraintes **CHECK** au script. La valeur par défaut est **True**. Les contraintes**CHECK** exigent que les données entrées dans une table satisfassent à certaines conditions spécifiées. Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
-   **Générer un script des options de compression de données** - Génère un script des options de compression de données si elles sont configurées sur la base de données d’origine ou sur des tables dans la base de données d’origine. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md). La valeur par défaut est **False**.  
  
-   **Générer un script pour les clés étrangères** - Ajoute des clés étrangères au script. La valeur par défaut est **True**. Les clés étrangères indiquent et garantissent les relations entre les tables.  
  
-   **Générer un script pour les index de recherche en texte intégral** - Génère un script pour la création d’index de recherche en texte intégral. La valeur par défaut est **False**.  
  
-   **Générer un script pour les index** - Génère un script pour la création d’index. La valeur par défaut est **True**. Les index permettent de trouver rapidement des données.  
  
-   **Générer un script pour les clés primaires** - Génère un script pour la création de clés primaires sur les tables. La valeur par défaut est **True**. Les clés primaires identifient de manière unique chaque ligne d'une table.  
  
-   **Générer un script pour les déclencheurs** - Génère un script pour la création de déclencheurs DML sur les tables. La valeur par défaut est **False**. Un déclencheur DML est une action programmée pour s'exécuter lorsqu'un événement de langage de manipulation de données (DML, Data Manipulation Language) se produit sur le serveur de base de données. Pour plus d'informations, consultez [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
-   **Générer un script pour les clés uniques** - Génère un script pour la création de clés uniques sur les tables. Les clés uniques empêchent l'entrée de données dupliquées. La valeur par défaut est **True**. Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
###  <a name="MgProviders"></a> Page Gérer les fournisseurs  
 Utilisez cette boîte de dialogue pour afficher, ajouter, modifier, supprimer ou tester les connexions des fournisseurs d'hébergement. Un fournisseur d'hébergement spécifie les informations de connexion pour un service Web créé à l'aide du projet de Service de publication de base de données dans le SQL Server Hosting Toolkit sur CodePlex.  
  
 **Fournisseurs configurés** - Répertorie le nom et l’adresse du service **Web** de chaque fournisseur d’hébergement enregistré.  
  
 **Nouveau** - Ouvre la boîte de dialogue **Configuration de fournisseur pour nouveau fournisseur** pour ajouter un nouveau fournisseur d’hébergement.  
  
 **Modifier** - Ouvre la boîte de dialogue **Configuration de fournisseur** correspondante pour modifier un fournisseur d’hébergement existant.  
  
 **Supprimer** - Supprime le fournisseur d’hébergement sélectionné.  
  
 **Test** - Teste la connexion à un service d’hébergement à l’aide des informations du fournisseur sélectionné.  
  
 **OK** - Enregistre toutes les modifications que vous avez apportées dans la boîte de dialogue **Fournisseur d’hébergement** .  
  
 **Annuler** - Annule toutes les modifications que vous avez apportées dans la boîte de dialogue **Fournisseur d’hébergement** .  
  
###  <a name="AdvPubOpts"></a> Page Options de publication avancées  
 Utilisez cette page pour spécifier la façon dont vous souhaitez que cet Assistant publie une base de données. De nombreuses options sont disponibles. Les options sont grisées si elles ne sont pas prises en charge par la version de SQL Server ou [!INCLUDE[ssSDS](../../includes/sssds-md.md)] spécifiée dans **Type de moteur de base de données**.  

  ![Publication avancée](media/generate-and-publish-scripts-wizard/advancedpublish.png)
  
 **Options** - Spécifiez les options avancées en sélectionnant une valeur dans la liste des paramètres disponibles située à droite de chaque option.  
  
 **Général** - Les options suivantes s'appliquent à la publication entière.  
  
1.  **Convertir les UDDT en types de base** - Lorsque cette option a la valeur **True**, les types de données définis par l’utilisateur (UDDT) sont convertis en types de données de base sous-jacents, ceux-là même qui ont été utilisés pour les créer. Utilisez **True** lorsque l'UDDT n'existe pas dans la base de données où le script s'exécutera. Lorsque cette option a la valeur **False**, les UDDT sont utilisés. La valeur par défaut est **False**.  
  
2.  **Publier le classement** - Inclut les informations de classement pour les colonnes de table. La valeur par défaut est **False**. Pour plus d’informations, consultez [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
3.  **Publication par défaut** - Inclut les objets par défaut utilisés pour définir les valeurs par défaut dans les colonnes de table. La valeur par défaut est **True**. Pour plus d’informations, consultez [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md).  
  
4.  **Publier les objets dépendants** - Publie tout objet dont la présence est requise lorsque le script de l’objet sélectionné est exécuté. La valeur par défaut est **True**.  
  
5.  **Publier les propriétés étendues** - Inclut les propriétés étendues dans le script envoyé au fournisseur pour la publication, si l’objet a des propriétés étendues. La valeur par défaut est **True**.  
  
6.  **Publier pour la version du serveur** - Crée un script envoyé au fournisseur distant pour la publication d’une façon qui peut être exécutée sur la version sélectionnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fonctionnalités qui sont nouvelles dans une version ne peuvent pas faire l'objet d'un script pour les versions antérieures. La valeur par défaut est la version du serveur d'origine.  
  
7.  **Publier les autorisations au niveau objet** - Inclut les autorisations sur les objets sélectionnés dans la base de données. La valeur par défaut est **False**.  
  
8.  **Publier les statistiques** - Quand cette option a la valeur **Publier les statistiques**, inclut l’instruction **CREATE STATISTICS** pour recréer des statistiques sur l’objet. L'option **Publier les statistiques et les histogrammes** crée également des informations d'histogramme. La valeur par défaut est **Ne pas publier les statistiques**. Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
9. **Publier les options Vardecimal** - Active le format de table **vardecimal** sur la table de base de données cible quand il est activé sur la table de base de données d’origine. La valeur par défaut est **True**.  
  
10. **Noms d’objet de qualification de schéma** - Inclut le nom de schéma dans le nom des objets créés. La valeur par défaut est **True**.  
  
11. **Liaisons de scripts** - Inclut la liaison pour les objets de règle et les objets par défaut dans le script envoyé au fournisseur pour la publication. La valeur par défaut est **True**. Pour plus d’informations, consultez [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md) et [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md).  
  
12. **Types de données à publier** - Sélectionne ce qui doit être inclus dans le script : **Données seulement**, **Schéma uniquement** ou les deux. La valeur par défaut est **Schéma et données**.  
  
 **Options de publication** - Spécifie s'il faut utiliser des transactions lors de la publication au fournisseur de serveur Web.  
  
1.  **Publier à l’aide d’une transaction** - Utilise les transactions lors de la publication sur un fournisseur d’hébergement web distant. Si la base de données cible ne peut pas terminer la publication, les transactions sont restaurées. La valeur par défaut est **True**.  
  
 **Options de table/vue** - Les options suivantes s’appliquent uniquement aux tables et aux vues.  
  
1.  **Publier les contraintes CHECK** - Inclut la création de contraintes **CHECK** dans le processus de publication. La valeur par défaut est **True**. Les contraintes**CHECK** exigent que les données entrées dans une table satisfassent à certaines conditions spécifiées. Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
2.  **Publier les clés étrangères** - Inclut la création de clés étrangères dans le processus de publication. La valeur par défaut est **True**. Les clés étrangères indiquent et garantissent les relations entre les tables. Pour plus d'informations, consultez [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md).  
  
3.  **Publier les index de recherche en texte intégral** - Génère un script pour la création d’index de recherche en texte intégral. La valeur par défaut est **False**.  
  
4.  **Publier les index** - Inclut les index sur les tables dans le processus de publication. La valeur par défaut est **True**. Les index permettent de trouver rapidement des données.  
  
5.  **Publier les clés primaires** - Inclut la création de clés primaires dans le processus de publication. La valeur par défaut est **True**. Les clés primaires identifient de manière unique chaque ligne d'une table. Pour plus d'informations, consultez [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md).  
  
6.  **Publier les déclencheurs** - Inclut la création de déclencheurs DML dans le processus de publication. La valeur par défaut est **True**. Un déclencheur DML est une action programmée pour s'exécuter lorsqu'un événement de langage de manipulation de données (DML, Data Manipulation Language) se produit sur le serveur de base de données. Pour plus d'informations, consultez [DML Triggers](../../relational-databases/triggers/dml-triggers.md).  
  
7.  **Publier les clés uniques** - Inclut la création de clés uniques sur les tables dans le processus de publication. Les clés uniques empêchent l'entrée de données dupliquées. La valeur par défaut est **True**. Pour plus d’informations, consultez [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
8.  **Publier le suivi des modifications** - Inclut le suivi des modifications dans le processus de publication, s’il est activé sur la base de données d’origine ou sur des tables dans la base de données d’origine. La valeur par défaut est **False**. Pour plus d’informations, consultez [À propos du suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md).  
  
9. **Publier les options de compression de données** - Inclut les options de compression de données dans le processus de publication, si elles sont configurées sur la base de données d’origine ou sur des tables dans la base de données d’origine. La valeur par défaut est **True**. Pour plus d’informations, consultez [Data Compression](../../relational-databases/data-compression/data-compression.md).  
  
###  <a name="ProvConfig"></a> Page Configuration du fournisseur  
 Utilisez cette boîte de dialogue pour afficher ou modifier des paramètres de fournisseur d'hébergement. Cette boîte de dialogue permet d'effectuer les opérations suivantes :  
  
-   afficher, ajouter ou modifier les informations de connexion d'un fournisseur d'hébergement ;  
  
-   afficher, ajouter, modifier ou supprimer une base de données pour une connexion de fournisseur ;  
  
-   configurer automatiquement des bases de données pour un fournisseur d'hébergement.  
  
 Un fournisseur d'hébergement spécifie les informations de connexion pour un service Web créé à l'aide du projet de Service de publication de base de données dans le SQL Server Hosting Toolkit sur CodePlex.  
  
 **Nom** - Nom du fournisseur d’hébergement.  
  
 **Adresse du service Web** - Adresse HTTPS du service d’hébergement.  
  
 **Authentification du service Web** - Nom d’utilisateur et mot de passe nécessaires pour ouvrir une session sur le service d’hébergement.  
  
 **Enregistrer le mot de passe** - Permet de chiffrer et d’enregistrer le mot de passe sur votre ordinateur local.  
  
 **Bases de données disponibles** - Les bases de données configurées pour les fournisseurs d’hébergement sont répertoriées par ordre croissant au format suivant : *nom_serveur*.*nom_base_de_données*.  
  
 **Nouveau** - Permet d’ouvrir la boîte de dialogue de configuration de la **Base de données** et d’ajouter une nouvelle base de données.  
  
 **Modifier** - Permet d’ouvrir la boîte de dialogue de configuration de la **Base de données** pour la base de données sélectionnée.  
  
 **Supprimer** - Supprime la base de données sélectionnée.  
  
 **Par défaut** - Permet de sélectionner la base de données comme base de données par défaut.  
  
 **OK** - Enregistre toutes les modifications que vous avez apportées dans cette boîte de dialogue et revient à l’Assistant.  
  
 **Annuler** - Annule toutes les modifications que vous avez apportées dans cette boîte de dialogue et revient à l’Assistant.  
  
###  <a name="Summary"></a> Page Résumé  
 Cette page résume les options que vous avez sélectionnées dans cet Assistant. Pour modifier une option, cliquez sur **Précédent**. Pour commencer à générer des scripts qui seront enregistrés ou publiés, cliquez sur **Suivant**.  
  
 **Vérifier vos sélections** - Affiche les choix que vous avez effectués dans chaque page de l’Assistant. Développez un nœud pour voir les options sélectionnées pour la page correspondante.  
  
###  <a name="SavePubScripts"></a> Page Enregistrer ou publier des scripts  
 Utilisez cette page pour contrôler la progression de l'Assistant.  
  
 **Détails** - Affiche la colonne **Action** pour voir la progression de l’Assistant. Après avoir généré les scripts, l'Assistant les enregistre dans un fichier ou les utilise pour la publication sur un service Web, selon vos sélections. Lorsque chacune de ces étapes est terminée, cliquez sur la valeur dans la colonne **Résultat** pour afficher le résultat de l'étape correspondante.  
  
 **Enregistrer le rapport** - Enregistre les résultats de la progression de l’Assistant dans un fichier.  
  
 **Annuler** - Ferme l’Assistant avant la fin du traitement ou si une erreur se produit.  
  
 **Terminer** - Ferme l’Assistant une fois le traitement terminé ou si une erreur se produit.  
 
## <a name="generating-scripts-on-azure-sql-data-warehouse"></a>Génération de scripts dans Azure SQL Data Warehouse  

Si la syntaxe générée lors de l’utilisation de « Script comme » ne ressemble pas à la syntaxe [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] ou si vous recevez un message d’erreur, vous devrez peut-être définir vos options de script dans SQL Server Management Studio sur [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].  

### <a name="how-to-set-default-scripting-options-to-sql-data-warehouse"></a>Comment définir les options de script par défaut sur SQL Data Warehouse  

Pour générer des scripts sur des objets avec la syntaxe [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] , définissez l’option de script par défaut sur [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] comme suit :  

1. Cliquez sur **outils** , puis **Options**.  
2. Sous **Options de script générales** , définissez :  
    1. Script pour le type de moteur de base de données : **Base de données SQL Microsoft Azure**.  
    2. Script pour l’édition de moteur de base de données : **Édition Microsoft Azure SQL Data Warehouse**.  
3. Cliquez sur **OK**.

### <a name="how-to-generate-scripts-for-sql-data-warehouse-when-it-is-not-the-default-scripting-option"></a>Comment générer des scripts pour SQL Data Warehouse quand il ne s’agit pas de l’option de script par défaut  

Si vous définissez [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] en tant qu’option de script par défaut, comme indiqué ci-dessus, vous pouvez ignorer ces instructions. Mais, si vous choisissez d’utiliser d’autres options de script par défaut, vous risquez de rencontrer une erreur. Pour éviter les erreurs, procédez comme suit pour générer et publier des scripts pour [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]:  

1. Cliquez avec le bouton droit sur votre base de données SQL Data Warehouse.  
2. Sélectionnez **Générer des scripts**.  
3. Sélectionnez les objets pour lesquels vous voulez générer des scripts.  
4. Dans **Options de script**, cliquez sur **Avancé**. Sous **Général** , définissez :  
    1. Script pour le type de moteur de base de données : **Base de données SQL Microsoft Azure**.  
    2. Script pour l’édition de moteur de base de données : **Édition Microsoft Azure SQL Data Warehouse**.  
5. Cliquez sur **Enregistrer ou publier des scripts** , puis sur **Terminer**.  

Les options définies à l’étape 4 ne seront pas mémorisées. Si vous préférez qu’elles le soient, suivez les instructions données dans **Comment définir les options de script par défaut sur SQL Data Warehouse**.  
  
## <a name="see-also"></a> Voir aussi  
 [Installation de SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)   
 [Copier des bases de données sur d’autres serveurs](../../relational-databases/databases/copy-databases-to-other-servers.md)  
  
  
