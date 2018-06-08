---
title: Éditeur de requête du moteur de base de données (SQL Server Management Studio) | Microsoft Docs
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
- sql13.swb.tsqlquery.f1
dev_langs:
- TSQL
helpviewer_keywords:
- Query Editor [Database Engine]
- Transact-SQL Editor See Query Editor [Database Engine]
- Database Engine Query Editor See Query Editor [Database Engine]
- Query Editor [Database Engine], Toolbar
- editors [SQL Server Management Studio], Database Engine Query Editor
- Query Editor [Database Engine], Features
- SQL Server Management Studio [SQL Server], Database Engine Query Editor
ms.assetid: 05cfae9b-96d5-4a35-a098-0bc3a548bcfc
caps.latest.revision: 47
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8a43ec3925304c5d895ebedbf157ebfecf8f82c0
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="database-engine-query-editor-sql-server-management-studio"></a>Éditeur de requête du moteur de base de données (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Utilisez l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour créer et exécuter des scripts contenant des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . L’éditeur prend également en charge l’exécution de scripts qui contiennent des commandes **sqlcmd** .  
  
## <a name="transact-sql-f1-help"></a>Aide sur Transact-SQL via la touche F1  
 L'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] prend en charge votre liaison à la rubrique de référence pour une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifique lorsque vous sélectionnez F1. Pour ce faire, mettez en surbrillance le nom d'une instruction Transact-SQL, puis sélectionnez F1. Le moteur de recherche d'aide recherchera alors une rubrique qui a un attribut d'aide F1 qui correspond à la chaîne que vous avez mise en surbrillance.  
  
 Si le moteur de recherche d'aide ne trouve pas une rubrique avec un mot clé d'aide F1 qui correspond exactement à la chaîne que vous avez mise en surbrillance, cette rubrique s'affiche. Dans ce cas, il existe deux approches pour obtenir l'aide que vous recherchez :  
  
-   Copier et coller la chaîne de l'éditeur que vous avez sélectionnée dans l'onglet de recherche de la documentation en ligne de SQL Server et effectuer une recherche.  
  
-   Ne sélectionner que la partie de l'instruction Transact-SQL susceptible de correspondre à un mot clé d'aide F1 appliqué à une rubrique et sélectionner à nouveau F1. Le moteur de recherche nécessite une correspondance exacte entre la chaîne que vous avez mise en surbrillance et un mot clé d'aide F1 affecté à une rubrique. Si la chaîne que vous avez mise en surbrillance contient des éléments propres à votre environnement, tels que les noms de colonne ou de paramètre, le moteur de recherche n'obtiendra pas de correspondance. Voici quelques exemples de chaînes à sélectionner :  
  
    -   Nom d'une instruction Transact-SQL, comme SELECT, CREATE DATABASE or BEGIN TRANSACTION.  
  
    -   Nom d’une fonction intégrée, comme SERVERPROPERTY ou @@VERSION.  
  
    -   Nom d'une table, ou d'une vue, de procédure stockée système, comme sys.data_spaces ou sp_tableoption.  
  
## <a name="working-with-the-database-engine-query-editor"></a>Utilisation de l'éditeur de requête du moteur de base de données  
 L'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est l'un des quatre éditeurs implémentés dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour obtenir une description de la fonctionnalité implémentée dans l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et des tâches principales que vous pouvez effectuer à l’aide de l’éditeur, consultez [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/query-and-text-editors-sql-server-management-studio.md).  
  
## <a name="sql-editor-toolbar"></a>Barre d'outils Éditeur SQL  
 Quand vous ouvrez l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , la barre d’outils Éditeur SQL s’affiche avec les boutons suivants.  
  
 **Se connecter**  
 Ouvre la boîte de dialogue **Se connecter au serveur** . Utilisez cette boîte de dialogue pour établir une connexion à un serveur.  
  
 **Déconnecter**  
 Déconnecte l'éditeur de requête actuel du serveur.  
  
 **Modifier la connexion**  
 Ouvre la boîte de dialogue **Se connecter au serveur** . Utilisez cette boîte de dialogue pour établir une connexion à un serveur différent.  
  
 **Nouvelle requête avec la connexion actuelle**  
 Ouvre une nouvelle fenêtre de l'éditeur de requête et utilise les informations de connexion de la fenêtre actuelle de l'éditeur de requête.  
  
 **Bases de données disponibles**  
 Permet de se connecter à une autre base de données sur le même serveur.  
  
 **Execute**  
 Exécute le code sélectionné ou, si aucun code n'est sélectionné, exécute la totalité du code figurant dans l'éditeur de requête.  
  
 **Débogage**  
 Active le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ce débogueur prend en charge les actions de débogage comme la définition de points d’arrêt, la surveillance de variables et l’exécution pas à pas du code.  
  
 **Annuler l'exécution de la requête**  
 Envoie une demande d'annulation au serveur. Certaines requêtes ne peuvent pas être annulées immédiatement et doivent attendre une condition d'annulation appropriée. Les opérations de restauration de transactions peuvent prendre un certain temps lorsque celles-ci sont annulées.  
  
 **Analyser**  
 Vérifie la syntaxe du code sélectionné. Si aucun code n'est sélectionné, cette option vérifie la syntaxe de l'ensemble du code contenu dans la fenêtre de l'éditeur de requête.  
  
 **Afficher le plan d'exécution estimé**  
 Demande un plan d’exécution de la requête au processeur de requêtes sans exécuter la requête et affiche le plan dans la fenêtre **Plan d’exécution** . Ce plan utilise les statistiques d'index pour estimer le nombre de lignes qui seront renvoyées pendant chaque partie de l'exécution de la requête. Le plan de requête utilisé dans la pratique peut être différent du plan d'exécution estimé. Cela peut se produire si le nombre de lignes renvoyées diffère sensiblement de l'estimation. À ce moment-là, le processeur de requêtes modifie le plan pour qu'il soit plus efficace.  
  
 **Options de requête**  
 Ouvre la boîte de dialogue **Options de requête** . Utilisez cette boîte de dialogue pour configurer les options par défaut d'exécution des requêtes et d'affichage de leurs résultats.  
  
 **IntelliSense activé**  
 Indique si les fonctionnalités IntelliSense sont disponibles dans l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 **Inclure le plan d'exécution réel**  
 Exécute la requête, puis renvoie les résultats de la requête et le plan d'exécution utilisé pour la requête. Ceux-ci s’affichent sous forme de plan de requête graphique dans la fenêtre **Plan d’exécution** .  
  
 **Inclure les statistiques du client**  
 Insère une fenêtre **Statistiques du client** qui contient des statistiques sur la requête et les paquets réseau, ainsi que le temps écoulé depuis le début de la requête.  
  
 **Résultats dans du texte**  
 Retourne les résultats de la requête dans la fenêtre **Résultats** sous forme de texte.  
  
 **Résultats dans des grilles**  
 Retourne les résultats de la requête dans la fenêtre **Résultats** sous forme d’une ou plusieurs grilles.  
  
 **Résultats dans un fichier**  
 Lors de l’exécution de la requête, la boîte de dialogue **Enregistrer les résultats** s’ouvre. Dans **Enregistrer dans**, sélectionnez le dossier dans lequel vous souhaitez enregistrer le fichier. Dans **Nom de fichier**, tapez le nom du fichier, puis cliquez sur **Enregistrer** pour enregistrer les résultats de la requête en tant que fichier de **Rapport** avec une extension .rpt. Pour accéder à des options avancées, cliquez sur la flèche de déroulement du bouton **Enregistrer** , puis cliquez sur **Enregistrer avec l’encodage**.  
  
 **Commenter la sélection**  
 Transforme la ligne active en commentaire en ajoutant un opérateur de commentaire (--) en début de ligne.  
  
 **Supprimer les marques de commentaire de la sélection**  
 Transforme la ligne active en instruction source active en supprimant l'opérateur de commentaire (--) en début de ligne.  
  
 **Réduire le retrait de ligne**  
 Déplace le texte de la ligne à gauche en supprimant les espaces en début de ligne.  
  
 **Augmenter le retrait de ligne**  
 Déplace le texte de la ligne vers la droite en ajoutant des espaces en début de ligne.  
  
 **Spécifier les valeurs des paramètres du modèle**  
 Ouvre une boîte de dialogue qui vous permet de spécifier les valeurs des paramètres des procédures stockées et des fonctions.  
  
 Vous pouvez également ajouter la barre d’outils Éditeur SQL en sélectionnant le menu **Affichage** , **Barres d’outils**, puis **Éditeur SQL**. Si vous ajoutez la barre d'outils Éditeur SQL alors qu'aucune fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'est ouverte, tous les boutons sont indisponibles.  
  
## <a name="sql-editor-toolbar"></a>Barre d'outils Éditeur SQL  
 Quand une fenêtre de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est ouverte, vous pouvez ajouter la barre d’outils Déboguer en sélectionnant successivement le menu **Affichage** , **Barres d’outils**et **Déboguer**. Si vous ajoutez la barre d'outils Déboguer alors qu'aucune fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'est ouverte, tous les boutons sont indisponibles.  
  
 **Continuer**  
 Exécute le code contenu dans la fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] jusqu'à ce qu'un point d'arrêt soit rencontré.  
  
 **Interrompre tout**  
 Le débogueur interrompt tous les processus auxquels il est associé lorsqu'un arrêt se produit.  
  
 **Arrêter le débogage**  
 Sort la fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] du mode débogage et rétablit le mode d'exécution standard.  
  
 **Afficher l'instruction suivante**  
 Déplace le curseur jusqu'à la prochaine instruction à exécuter.  
  
 **Pas à pas détaillé**  
 L'instruction suivante est exécutée. Si l’instruction appelle une procédure stockée Transact-SQL, une fonction ou un déclencheur, le débogueur affiche une nouvelle fenêtre de l’ **éditeur de requête** contenant le code du module. La fenêtre est en mode débogage, et l'exécution s'arrête à la première instruction dans le module. Vous pouvez ensuite parcourir le module, en définissant par exemple des points d'arrêt ou en exécutant le code pas à pas.  
  
 **Pas à pas principal**  
 L'instruction suivante est exécutée. Si l'instruction appelle une procédure stockée Transact-SQL, une fonction ou un déclencheur, le module est exécuté jusqu'à ce qu'il se termine, et les résultats sont retournés au code appelant. Si vous êtes certain que le module ne comporte aucune erreur, vous pouvez faire un pas à pas principal. L'exécution suspend l'instruction qui suit l'appel au module.  
  
 **Pas à pas sortant**  
 Retourne au niveau d'appel supérieur suivant (fonction, procédure stockée ou déclencheur). L'exécution s'arrête à l'instruction qui suit l'appel à la procédure stockée, à la fonction ou au déclencheur.  
  
 **Windows**  
 Ouvre la fenêtre **Point d’arrêt** ou la fenêtre **Immédiat** .  
  
## <a name="see-also"></a> Voir aussi  
 [Raccourcis clavier dans SQL Server Management Studio](../../tools/sql-server-management-studio/sql-server-management-studio-keyboard-shortcuts.md)  
  
  
