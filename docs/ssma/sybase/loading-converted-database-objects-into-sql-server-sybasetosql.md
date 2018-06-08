---
title: Chargement converti objets base de données dans SQL Server (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Loading Converted Database Objects
ms.assetid: 4c59256f-99a8-4351-9559-a455813dbd06
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: c3c5f0d28224afce206949ae9011c6777458325e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="loading-converted-database-objects-into-sql-server-sybasetosql"></a>Chargement converti objets base de données dans SQL Server (SybaseToSQL)
Après avoir converti les objets de base de données Sybase Adaptive Server Enterprise (ASE) à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous pouvez charger les objets de base de données qui en résulte dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous pouvez avoir créer les objets SSMA, ou vous pouvez les objets de script et exécuter les scripts vous-même. SSMA vous permet également de mettre à jour des métadonnées de la cible avec le contenu réel de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de la base de données SQL Azure.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Choix entre synchronisation et de Scripts  
Si vous souhaitez charger les objets de base de données convertie en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure sans modification, vous pouvez avoir SSMA directement de créer ou de recréer les objets de base de données. Cette méthode est rapide et facile, mais ne permettent pas de personnaliser la [!INCLUDE[tsql](../../includes/tsql_md.md)] code qui définit la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de SQL Azure, autres que les procédures stockées.  
  
Si vous souhaitez modifier le [!INCLUDE[tsql](../../includes/tsql_md.md)] qui est utilisé pour créer les objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, ou si vous souhaitez mieux contrôler quand et comment les objets sont créés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, utilisez SSMA pour créer [!INCLUDE[tsql](../../includes/tsql_md.md)] des scripts. Vous pouvez ensuite modifier ces scripts, créer chaque objet séparément et même utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure Agent pour planifier la création de ces objets.  
  
## <a name="using-ssma-to-load-objects-into-sql-server-or-sql-azure"></a>À l’aide de SSMA pour charger des objets dans SQL Server ou SQL Azure  
SSMA permet de créer [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de base de données SQL Azure, vous sélectionnez les objets de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Explorateur de métadonnées SQL Azure, puis de synchroniser les objets avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, comme indiqué dans la procédure suivante. Par défaut, si les objets existent déjà dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure et si les métadonnées SSMA a certaines modifications locales ou les mises à jour de la définition de ces objets très, SSMA modifie les définitions d’objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous pouvez modifier le comportement par défaut en modifiant **les paramètres de projet**.  
  
> [!NOTE]  
> Vous pouvez sélectionner existant [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de base de données SQL Azure qui n’ont pas été converties à partir de bases de données ASE. Toutefois, ces objets ne seront pas recréées ou modifiés par SSMA.  
  
**Pour synchroniser des objets avec SQL Server ou SQL Azure**  
  
1.  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Explorateur de métadonnées SQL Azure, développez haut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou nœud de SQL Azure, puis développez **bases de données**.  
  
2.  Sélectionnez les objets à traiter :  
  
    -   Pour synchroniser une base de données complète, sélectionnez la case à cocher en regard du nom de la base de données.  
  
    -   Pour synchroniser ou omettre des objets individuels ou des catégories d’objets, activez ou désactivez la case à cocher en regard de l’objet ou un dossier.  
  
3.  Après avoir sélectionné les objets à traiter dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Explorateur de métadonnées SQL Azure, avec le bouton droit **bases de données**, puis cliquez sur **synchroniser avec la base de données**.  
  
    Vous pouvez également synchroniser des objets individuels ou des catégories d’objets en cliquant sur l’objet ou son dossier parent, puis en cliquant **synchroniser avec la base de données**.  
  
    Après cela, SSMA affichera le **synchroniser avec la base de données** boîte de dialogue, où vous pouvez consulter les deux groupes d’éléments. Sur le côté gauche, SSMA affiche les objets de base de données sélectionnée représentées dans une arborescence. Sur le côté droit, vous pouvez voir une arborescence représentant les mêmes objets dans les métadonnées SSMA. Vous pouvez développer l’arborescence en cliquant sur la droite ou la gauche bouton « + ». La direction de la synchronisation est indiquée dans la colonne Action placée entre les deux arbres.  
  
    Un signe de l’action peut être dans trois états :  
  
    -   Une flèche gauche signifie que le contenu de métadonnées sera enregistré dans la base de données (la valeur par défaut).  
  
    -   Une flèche droite signifie le contenu de la base de données remplace les métadonnées SSMA.  
  
    -   Une croix signifie qu'aucune action à entreprendre.  
  
Cliquez sur le signe de l’action Modifier l’état. Synchronisation réelle sera effectuée lorsque vous cliquez sur **OK** bouton de la **synchroniser avec la base de données** boîte de dialogue.  
  
## <a name="scripting-objects"></a>Objets de script  
Si vous souhaitez enregistrer [!INCLUDE[tsql](../../includes/tsql_md.md)] définitions des objets de base de données convertie, ou si vous souhaitez modifier les définitions d’objet et exécuter des scripts vous-même, vous pouvez enregistrer les définitions d’objet dans la base de données convertie [!INCLUDE[tsql](../../includes/tsql_md.md)] des scripts.  
  
**Pour enregistrer les objets sous forme de scripts**  
  
1.  Après avoir sélectionné les objets à enregistrer dans un script, cliquez sur **bases de données**, puis sélectionnez **enregistrer en tant que Script**.  
  
    Vous pouvez également écrire un script des objets individuels ou des catégories d’objets en cliquant sur l’objet ou son conteneur, puis en sélectionnant **enregistrer le Script**.  
  
2.  Dans le **enregistrer en tant que** boîte de dialogue, recherchez le dossier où vous souhaitez enregistrer le script, entrez un nom de fichier dans le **nom de fichier** zone, puis cliquez sur **OK**.  
  
    SSMA permet d’ajouter l’extension de nom de fichier .sql.  
  
### <a name="modifying-scripts"></a>Modification des Scripts  
Après avoir enregistré le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou définitions d’objets SQL Azure en tant qu’un ou plusieurs scripts, vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] pour afficher et modifier les scripts.  
  
**Pour modifier un script**  
  
1.  Sur le [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] **fichier** menu, pointez sur **ouvrir**, puis cliquez sur **fichier**.  
  
2.  Dans le **ouvrir** boîte de dialogue, accédez à et sélectionnez votre fichier de script, puis cliquez sur **OK**.  
  
3.  Modifier et le fichier de script à l’aide de l’éditeur de requête.  
  
    Pour plus d’informations sur l’éditeur de requête, consultez « Éditeur commodité commandes et fonctionnalités » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
4.  Pour enregistrer le script, dans le menu fichier, sélectionnez **enregistrer**.  
  
### <a name="running-scripts"></a>Exécution de Scripts  
Vous pouvez exécuter un script ou des instructions individuelles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)].  
  
**Pour exécuter un script**  
  
1.  Sur le [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] **fichier** menu, pointez sur **ouvrir**, puis cliquez sur **fichier**.  
  
2.  Dans le **ouvrir** boîte de dialogue, accédez à et sélectionnez votre fichier de script, puis cliquez sur **OK**.  
  
3.  Pour exécuter le script complet, appuyez sur la **F5** clé.  
  
4.  Pour exécuter un ensemble d’instructions, les instructions select dans la fenêtre d’éditeur de requête, puis appuyez sur la **F5** clé.  
  
Pour plus d’informations sur l’utilisation de l’éditeur de requête à exécuter des scripts, consultez «[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] [!INCLUDE[tsql](../../includes/tsql_md.md)] requête » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
Vous pouvez également exécuter des scripts à partir de la ligne de commande à l’aide de la **sqlcmd** utilitaire et à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Agent. Pour plus d’informations sur **sqlcmd**, consultez la section « utilitaire sqlcmd » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne. Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent, voir « Automatisation des tâches d’administration ([!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent) » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="securing-objects-in-sql-server"></a>Sécurisation des objets dans SQL Server  
Une fois que vous avez chargé les objets de base de données convertie en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez accorder et refuser des autorisations sur ces objets. Il est judicieux d’effectuer cette opération avant la migration de données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations sur la sécurisation des objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], consultez « Sécurité considérations relatives à des bases de données et base de données des Applications » dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration consiste à [migration de données Sybase ASE dans SQL Server / SQL Azure(SybaseToSQL)](http://msdn.microsoft.com/en-us/54a39f5e-9250-4387-a3ae-eae47c799811).  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de Sybase ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
