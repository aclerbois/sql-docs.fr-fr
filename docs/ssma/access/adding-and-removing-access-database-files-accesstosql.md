---
title: Ajout et suppression de l’accès de fichiers (AccessToSQL) base de données | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
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
- Access databases
- adding Access files
- adding and removing Access databases
- browsing Access metadata
- browsing metadata, Access databases
- connecting, Access databases
- databases
- files, adding and removing
- finding Access databases
- finding database files
- locating database files
- metadata
- metadata, browsing
- metadata, refreshing
- refreshing metadata
- removing Access files
- scanning for database files
- searching for database files
ms.assetid: e944c740-4c8a-4bc1-b0ed-be57bc06dced
caps.latest.revision: 15
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: e2fbd70bf6427b7f5dbe694f4abd2fed2f28b4b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="adding-and-removing-access-database-files-accesstosql"></a>Ajout et suppression de fichiers de base de données Access (AccessToSQL)
Pour migrer des données d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez ajouter une ou plusieurs bases de données Access vers le projet SSMA. Ces bases de données doivent être Access 97 ou version ultérieure. Si vous avez des bases de données à partir d’une version antérieure de l’accès, vous devez convertir les bases de données vers une version plus récente. Pour cela, en ouvrant et en enregistrant les bases de données dans Access 97 ou une version ultérieure avant de les ajouter à SSMA.  
  
## <a name="what-happens-when-you-add-access-database-files"></a>Que se passe-t-il lorsque vous ajoutez des fichiers de base de données Access ?  
Lorsque vous ajoutez une base de données de l’accès à un projet SSMA, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées dans le fichier projet. Ces métadonnées décrivent la base de données et ses objets. SSMA utilise les métadonnées lorsqu’il convertit des objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou la syntaxe SQL Azure, et lorsqu’il migre les données pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous pouvez parcourir ces métadonnées dans l’Explorateur de métadonnées d’accès et passez en revue les propriétés des objets de base de données individuels.  
  
> [!NOTE]  
> Une base de données Access peut être divisé en plusieurs fichiers : une base de données principale qui contient des tables et des bases de données frontales qui contiennent des requêtes, formulaires, rapports, macros, modules et raccourcis. Si vous souhaitez migrer une base de données de fractionnement à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, ajouter la base de données frontaux SSMA.  
  
## <a name="permissions-that-are-required-by-ssma"></a>Autorisations requises par SSMA  
Pour migrer une base de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, le groupe d’utilisateurs et l’utilisateur Admin doivent avoir des autorisations d’administration. Pour plus d’informations sur la migration des bases de données avec la protection d’un groupe de travail, consultez [préparation de bases de données Access pour la Migration](http://msdn.microsoft.com/en-us/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)  
  
## <a name="selecting-databases-to-add"></a>Sélectionner les bases de données à ajouter  
Si vous souhaitez ajouter un ou plusieurs bases de données à un projet SSMA, et les fichiers sont dans un emplacement connu, vous pouvez ajouter les fichiers à l’aide de la procédure suivante.  
  
**Pour ajouter des fichiers de base de données individuels**  
  
1.  Sur le **fichier** menu, cliquez sur **ajouter les bases de données**.  
  
2.  Dans le **ouvrir** boîte de dialogue, recherchez le dossier qui contient l’ou les fichiers de base de données.  
  
3.  Sélectionnez les fichiers que vous souhaitez ajouter, puis cliquez sur **ouvrir**.  
  
## <a name="finding-databases-to-add"></a>Recherche des bases de données à ajouter  
Si vous souhaitez ajouter plusieurs bases de données Access à partir de dossiers différents à un projet SSMA ou que vous souhaitez ajouter un seul fichier, mais que le fichier est introuvable, vous pouvez suivre ces étapes pour rechercher un des autres fichiers et de les ajouter au projet.  
  
**Pour rechercher et ajouter des bases de données**  
  
1.  Sur le **fichier** menu, cliquez sur **trouver les bases de données**.  
  
2.  Dans l’Assistant trouver les bases de données, entrez le nom du lecteur, chemin d’accès de fichier ou le chemin d’accès UNC que vous souhaitez rechercher. Sinon, cliquez sur **Parcourir** pour localiser le dossier de disque ou réseau.  
  
3.  Cliquez sur **ajouter** pour ajouter l’emplacement dans la liste.  
  
    Répétez les deux étapes précédentes pour ajouter des emplacements de recherche.  
  
4.  Si vous le souhaitez, ajoutez les critères de recherche pour affiner la liste des bases de données qui sont retournées.  
  
    > [!IMPORTANT]  
    > Le **tout ou partie du nom de fichier** zone de texte ne prend pas en charge les caractères génériques.  
  
5.  Cliquez sur **analyse**.  
  
    La page d’analyse s’affiche. Elle indique les bases de données qui ont été trouvés et la progression de la recherche. Pour arrêter la recherche, cliquez sur **arrêter**.  
  
6.  Dans la page Sélectionner les fichiers, sélectionnez les bases de données que vous souhaitez ajouter au projet.  
  
    Vous pouvez utiliser la **sélectionner tout** et **Effacer tout** boutons en haut de la liste pour sélectionner ou effacer toutes les bases de données. Vous maintenez la touche CTRL ENFONCÉE pour sélectionner plusieurs bases de données, ou maintenez la touche MAJ pour sélectionner une plage de bases de données.  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page de confirmation, cliquez sur **Terminer**.  
  
## <a name="browsing-access-metadata"></a>Exploration des métadonnées de l’accès  
Après avoir ajouté une base de données de l’accès à un projet, les métadonnées de projet apparaissent dans l’Explorateur de métadonnées de l’accès. Vous pouvez parcourir la hiérarchie des bases de données et les objets de base de données dans l’Explorateur.  
  
**Pour parcourir les métadonnées**  
  
1.  Dans l’Explorateur de métadonnées de l’accès, développez **accès à la métabase**, puis développez **bases de données**.  
  
2.  Développez la base de données que vous souhaitez examiner, puis cliquez sur **requêtes**.  
  
    Notez que la liste des requêtes. Si vous sélectionnez une requête, un **SQL** onglet et un **propriétés** onglet s’affiche dans le volet droit.  
  
3.  Développez **Tables** , puis sélectionnez une table.  
  
    Remarquez que quatre onglets s’affichent : **Table**, **le mappage de Type**, **propriétés**, et **données**.  
  
4.  Développez une table, **clés**, puis sélectionnez une clé.  
  
    Les propriétés de clé apparaissent dans le volet droit.  
  
5.  Développez **index**, puis sélectionnez un index.  
  
    Les propriétés d’index s’affichent dans le volet droit.  
  
## <a name="refreshing-databases"></a>L’actualisation des bases de données  
Si une base de données Access change après avoir ajouté le fichier, vous pouvez mettre à jour les métadonnées à partir de la base de données Access.  
  
**Pour mettre à jour les métadonnées de l’accès**  
  
-   Dans l’Explorateur de métadonnées de l’accès, avec le bouton droit de la base de données, puis **Actualiser à partir de la base de données**.  
  
## <a name="removing-databases"></a>Suppression de bases de données  
Vous pouvez supprimer une base de données Access à partir d’un projet en suivant ces étapes.  
  
**Pour supprimer une base de données à partir d’un projet**  
  
1.  Dans l’Explorateur de métadonnées de l’accès, développez **accès à la métabase**, puis développez **bases de données**.  
  
2.  Avec le bouton droit de la base de données, puis sélectionnez **supprimer la base de données**.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration consiste à [se connecter à SQL Server](http://msdn.microsoft.com/en-us/bb8c4bde-cfc2-4636-92ae-5dd24abe9536).  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
[Création et gestion de projets](http://msdn.microsoft.com/en-us/f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7)  
  
