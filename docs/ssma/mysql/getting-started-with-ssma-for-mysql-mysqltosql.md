---
title: Prise en main de SSMA pour MySQL (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Getting started, MySQL metadata explorer
- Getting started, SQL Server or SQL Azure metadata explorer
- Getting started,Installing and licensing
ms.assetid: 8ebfa061-be6f-4a07-923f-8dc832a82f70
caps.latest.revision: 19
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 191ff0505de357b4a76579e2797ceaeb636faf1a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-ssma-for-mysql-mysqltosql"></a>Prise en main de SSMA pour MySQL (MySQLToSQL)
SQL Server Migration Assistant (SSMA) pour MySQL vous permet de rapidement convertir des schémas de base de données MySQL en schémas SQL Server ou de la base de données SQL Azure, téléchargez les schémas qui en résulte dans SQL Server ou une base de données SQL Azure et migrer des données de MySQL vers SQL Server ou de la base de données SQL Azure.  
  
Cette rubrique présente le processus d’installation et vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>L’installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur pouvant accéder à la base de données MySQL source et de l’instance cible de SQL Server ou de la base de données SQL Azure. Ensuite, installez les fournisseurs de MySQL (MySQL 5.1 le pilote ODBC (approuvé)) sur l’ordinateur qui exécute le programme Client de SSMA. Pour obtenir des instructions d’installation, consultez [l’installation de SSMA pour MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur **SQL Server Migration Assistant pour MySQL**, puis cliquez sur **SQL Server Migration Assistant pour MySQL**.  
  
## <a name="ssma-for-mysql-user-interface"></a>SSMA pour l'interface d'utilisateur MySQL  
Une fois SSMA est installé et sous licence, vous pouvez utiliser SSMA pour migrer des bases de données MySQL vers SQL Server ou de la base de données SQL Azure. Cela permet de vous familiariser avec l’interface utilisateur SSMA avant de commencer. Le diagramme suivant montre l’interface utilisateur de SSMA, y compris les explorateurs de métadonnées, métadonnées, barres d’outils, sortie et volet Liste d’erreur :  
  
![SSMA pour Interface utilisateur graphique de MySql](../../ssma/mysql/media/ssmaformysqlgui.gif "SSMA pour l’Interface utilisateur graphique MySql")  
  
Pour démarrer une migration, vous devez :  
  
1.  Créez un projet.  
  
2.  Se connecter à une base de données MySQL.  
  
3.  Après une connexion réussie, les schémas de MySQL seront affiche dans l’Explorateur de métadonnées MySQL. Les objets avec le bouton droit dans l’Explorateur de métadonnées MySQL pour effectuer des tâches telles que créent des rapports qui évaluent les conversions de la base de données SQL Server/Azure SQL.  
  
Vous pouvez également effectuer ces tâches en utilisant les menus et barres d’outils.  
  
Vous devez également vous connecter à une instance de SQL Server. Après une connexion réussie, une hiérarchie de bases de données SQL Server s’affiche dans l’Explorateur de métadonnées SQL Server. Après la conversion des schémas de MySQL en schémas SQL Server, sélectionnez ces schémas convertis dans l’Explorateur de métadonnées SQL Server, puis synchronisez les schémas avec SQL Server.  
  
Vous devez vous connecter à la base de données SQL Azure si vous avez sélectionné la base de données SQL Azure à partir de la migration vers la liste déroulante dans la boîte de dialogue Nouveau projet. Après une connexion réussie, une hiérarchie de bases de données de base de données SQL Azure s’affiche dans l’Explorateur de métadonnées de base de données SQL Azure. Après la conversion de schémas de MySQL pour les schémas de base de données SQL Azure, sélectionnez ces schémas convertis dans l’Explorateur de métadonnées de base de données SQL Azure, puis synchronisez les schémas avec la base de données SQL Azure.  
  
Une fois que vous synchronisez des schémas convertis avec SQL Server ou de la base de données SQL Azure, vous pouvez revenir à l’Explorateur de métadonnées MySQL et migrer des données à partir des schémas de MySQL vers les bases de données SQL Server ou de la base de données SQL Azure.  
  
Pour plus d’informations sur ces tâches et comment les exécuter, consultez [migration de bases de données MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md).  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées pour parcourir et d’effectuer des actions sur les bases de données SQL Server et MySQL.  
  
### <a name="mysql-metadata-explorer"></a>Explorateur de métadonnées MySQL  
Explorateur de métadonnées MySQL affiche des informations sur les schémas de MySQL. À l’aide de l’Explorateur de métadonnées MySQL, vous pouvez effectuer les tâches suivantes :  
  
-   Parcourir les objets dans chaque schéma.  
  
-   Sélectionner les objets pour la conversion et puis convertir les objets à la syntaxe de SQL Server. Pour plus d’informations, consultez [conversion des bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
-   Sélectionner des tables de la migration des données, puis migrer les données à partir de ces tables vers SQL Server. Pour plus d’informations, consultez [migration des données de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
### <a name="sql-server-or-azure-sql-db-metadata-explorer"></a>SQL Server ou l’Explorateur de métadonnées de base de données SQL Azure  
SQL Server ou l’Explorateur de métadonnées de base de données SQL Azure affiche des informations sur une instance de SQL Server ou de la base de données SQL Azure. Lorsque vous vous connectez à une instance de SQL Server ou de la base de données SQL Azure, SSMA récupère les métadonnées relatives à cette instance et le stocke dans le fichier projet.  
  
Vous pouvez utiliser l’Explorateur de métadonnées pour sélectionner des objets de base de données MySQL convertis et puis de synchroniser ces objets avec l’instance de SQL Server ou de la base de données SQL Azure.  
  
Pour plus d’informations, consultez [synchronisation (MySQL vers SQL Server / base de données SQL Azure)](http://msdn.microsoft.com/en-us/ac993a6d-0283-4823-8793-6b217677dfa3)  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées MySQL, neuf onglets seront affiche : **Table**, **SQL**, **le mappage de Type**, **données**, **paramètres**, **mappage du jeu de caractères**, **Modes SQL**, **propriétés**, et **rapport**. Le **rapport** onglet contient des informations uniquement après avoir créé un rapport qui contient l’objet sélectionné. Si vous sélectionnez une table dans l’Explorateur de métadonnées SQL Server, les trois onglets seront affiche : **Table**, **SQL** et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées MySQL, vous pouvez modifier les mappages de type, le mappage de jeu de caractères, les Modes SQL. Pour convertir les mappages de type modifié ou de mappage de jeu de caractères ou de Modes SQL, apporter des modifications avant de convertir des schémas.  
  
-   Dans l’Explorateur de métadonnées SQL Server, vous pouvez modifier les propriétés de table et d’index sur l’onglet de Table. Pour voir ces modifications dans SQL Server, effectuez ces modifications avant de charger les schémas dans SQL Server.  
  
Modifications apportées dans l’Explorateur de métadonnées sont répercutées dans les métadonnées de projet, pas dans les bases de données source ou cible.  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA a deux barres d’outils : une barre d’outils du projet et une barre d’outils de migration.  
  
### <a name="the-project-toolbar"></a>La barre d’outils du projet  
La barre d’outils du projet contient des boutons pour l’utilisation de projets, la connexion à MySQL et se connecter à SQL Server ou de la base de données SQL Azure. Ces boutons ressemblent aux commandes sur le **fichier** menu.  
  
### <a name="migration-toolbar"></a>Barre d’outils de migration  
Le tableau suivant affiche les commandes de la barre d’outils de la migration :  
  
|||  
|-|-|  
|**Bouton**|**Fonction**|  
|**Créer des rapports**|Convertit les objets sélectionnés de MySQL pour les objets SQL Server ou de la base de données SQL Azure et crée ensuite un rapport qui indique la réussite de la conversion a.<br /><br />Cette commande est désactivée, sauf si les objets sont sélectionnés dans l’Explorateur de métadonnées MySQL.|  
|**Convertir le schéma**|Convertit les objets sélectionnés de MySQL pour les objets SQL Server ou de la base de données SQL Azure.<br /><br />Cette commande est désactivée, sauf si les objets sont sélectionnés dans l’Explorateur de métadonnées MySQL.|  
|**Migrer des données**|Migre les données à partir de la base de données MySQL vers SQL Server ou de la base de données SQL Azure. Avant d’exécuter cette commande, vous devez convertir les schémas MySQL en schémas SQL Server ou de la base de données SQL Azure et puis charger les objets dans SQL Server ou de la base de données SQL Azure.<br /><br />Cette commande est désactivée, sauf si les objets sont sélectionnés dans l’Explorateur de métadonnées MySQL.|  
|**Arrêter**|Arrête le processus en cours.|  
  
### <a name="menus"></a>Menus  
Le tableau suivant présente les menus SSMA.  
  
|||  
|-|-|  
|**Menu**|**Description**|  
|**Fichier**|Contient des commandes pour travailler avec des projets, de se connecter à MySQL et de se connecter à SQL Server ou de la base de données SQL Azure.|  
|**Modifier**|Contient des commandes pour la recherche et l’utilisation de texte dans les pages de détails. Pour ouvrir **gérer les signets** boîte de dialogue, dans le menu Edition cliquez sur Gérer les signets. Dans la boîte de dialogue, vous verrez une liste des signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Affichage**|Contient le **synchroniser les explorateurs de métadonnées** commande. Qui synchronise les objets entre l’Explorateur de métadonnées MySQL et SQL Server ou Explorateur de métadonnées de base de données SQL Azure. Contient également des commandes pour afficher et masquer le **sortie** et **liste d’erreurs** volets et une option **dispositions** pour gérer les mises en page.|  
|**Outils**|Contient des commandes pour créer des rapports, convertir le schéma, Actualiser à partir de la base de données, migrer des objets et des données et enregistrer en tant que Script. Permet également d’accéder à la **des paramètres globaux, des paramètres de projet par défaut** et **les paramètres de projet** boîtes de dialogue.|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la **sur** boîte de dialogue.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volets de sortie et liste erreur  
Le **vue** menu fournit des commandes pour activer/désactiver la visibilité du volet sortie et le volet de la liste d’erreurs :  
  
-   Le volet de résultats affiche les messages d’état à partir de SSMA pendant la conversion de l’objet, la synchronisation de l’objet et la migration des données.  
  
-   Le volet de la liste d’erreurs affiche des messages d’information, d’avertissement et erreur dans une liste pouvant être triée.  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’Interface utilisateur &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
[Migration de données MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
