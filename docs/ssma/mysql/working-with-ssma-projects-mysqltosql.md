---
title: Utilisation de projets SSMA (MySQLToSQL) | Documents Microsoft
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
- Working with SSMA projects, create new project
- Working with SSMA projects, customize settings
- Working with SSMA projects, Open project
- Working with SSMA projects, Save project
ms.assetid: 9e4394e9-f177-41d9-839e-5d53a9c9b840
caps.latest.revision: 20
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 6f0cd5bf46696dc6acb928b7bfba4a3896f1503d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-ssma-projects-mysqltosql"></a>Utilisation de projets SSMA (MySQLToSQL)
Pour migrer des bases de données MySQL vers SQL Server ou SQL Azure, vous devez d’abord créer un projet SSMA. Le projet est un fichier qui contient les informations suivantes :  
  
-   Métadonnées sur les bases de données MySQL que vous souhaitez migrer vers SQL Server ou SQL Azure.  
  
-   Métadonnées relatives à l’instance cible de SQL Server ou SQL Azure qui recevra les objets migrés et les données.  
  
-   Informations de connexion SQL Server ou SQL Azure.  
  
-   Paramètres du projet.  
  
Lorsque vous ouvrez un projet, il est déconnecté de MySQL et SQL Server ou SQL Azure. Qui vous permet de travailler en mode hors connexion. Pour plus d’informations sur la reconnexion à SQL Server, consultez [connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="reviewing-default-project-settings"></a>Examen des paramètres de projet par défaut  
SSMA contient plusieurs paramètres de conversion et le chargement de base de données, de migration des données et synchronisation de SSMA avec MySQL et SQL Server ou SQL Azure. Les paramètres par défaut sont adaptés à de nombreux utilisateurs. Toutefois, avant de créer un nouveau projet SSMA, vous devez examiner les paramètres. Si nécessaire, vous pouvez modifier les paramètres par défaut qui seront utilisés pour tous les nouveaux projets.  
  
##### <a name="to-review-default-project-settings"></a>Pour passer en revue les paramètres de projet par défaut  
  
1.  Sélectionnez **les paramètres de projet par défaut** à partir de la **outils** menu.  
  
2.  Sélectionnez le type de projet dans **Version cible de la Migration** la liste déroulante pour les paramètres doivent être affichés / modifié, puis cliquez sur **général** onglet.  
  
3.  Dans le volet gauche, cliquez sur **Conversion**.  
  
4.  Dans le volet droit, vérifiez et modifiez les paramètres selon vos besoins. Pour plus d’informations sur ces paramètres, consultez [les paramètres de projet &#40;Conversion&#41; &#40;MySQLToSQL&#41; ](../../ssma/mysql/project-settings-conversion-mysqltosql.md) .  
  
5.  Répétez les étapes 1 à 3 pour les pages de la Migration, la synchronisation, SQL Azure, l’interface graphique utilisateur et le mappage de Type.  
  
-   Pour plus d’informations sur les paramètres de migration, consultez [les paramètres de projet &#40;Migration&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md).  
  
-   Pour plus d’informations sur les paramètres de synchronisation à SQL Server, consultez [les paramètres de projet &#40;synchronisation&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md).  
  
-   Pour plus d’informations sur les paramètres de l’interface graphique utilisateur, consultez [paramètres de projet (GUI) (SSMA commun)](http://msdn.microsoft.com/en-us/cf06baf1-8714-48a3-95dc-781f6ca53693).  
  
-   Pour plus d’informations sur les paramètres de mappage de type de données, consultez [les paramètres de projet &#40;mappage de Type&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
-   Pour plus d’informations sur les paramètres de SQL Azure, consultez [les paramètres de projet &#40;base de données SQL Azure&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md).  
  
> [!NOTE]  
> Les paramètres de SQL Azure seront affichera uniquement lorsque vous sélectionnez **la Migration vers SQL Azure** lors de la création d’un projet.  
  
## <a name="creating-new-projects"></a>Création de projets  
Pour migrer des données à partir de bases de données MySQL vers SQL Server ou SQL Azure, vous devez créer un projet.  
  
##### <a name="to-create-a-new-project"></a>Pour créer un nouveau projet  
  
1.  Sélectionnez **nouveau projet** à partir de la **fichier** menu. La boîte de dialogue **Nouveau projet** s'affiche. Dans le menu **Fichier**, sélectionnez **Nouveau projet**. La boîte de dialogue **Nouveau projet** s'affiche.  
  
2.  Dans la zone **Nom** , tapez le nom de votre projet.  
  
3.  Dans le **emplacement** zone, entrez ou sélectionnez un dossier pour le projet.  
  
4.  Dans le **à la Migration** liste déroulante, sélectionnez la version de la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilisé pour la migration. Les options disponibles sont :  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014  
  
    -   Base de données SQL Azure  
  
Puis cliquez sur **OK**  
  
SSMA crée le fichier de projet.  
  
## <a name="customizing-project-settings"></a>Personnalisation des paramètres de projet  
Outre la définition de la valeur par défaut des paramètres de projet qui s’appliquent à tous les nouveaux projets SSMA, vous peuvent également personnaliser les paramètres pour chaque projet. Pour plus d’informations, consultez [définition des Options de projet &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).  
  
Lorsque vous personnalisez les mappages de types de données entre les bases de données source et cible, vous pouvez définir des mappages pour le projet, la base de données ou le niveau de l’objet. Pour plus d’informations, consultez [MySQL de mappage et les Types de données SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).  
  
## <a name="saving-projects"></a>L’enregistrement de projets  
La fonctionnalité de l’enregistrement des projets permet à l’utilisateur essentiellement à enregistrer les paramètres du projet et, éventuellement, les métadonnées de la base de données dans le fichier de projet SSMA.  
  
##### <a name="to-save-a-project"></a>Pour enregistrer un projet  
  
-   Sur le **fichier** menu, sélectionnez **enregistrer** projet.  
  
Si les bases de données au sein du projet ont été modifiées ou n’ont pas été converties, SSMA vous invitera à charger et enregistrer les métadonnées. Le chargement et l’enregistrement des métadonnées vous permettent de travailler hors connexion. Il vous permet également d’envoyer un fichier projet à d’autres personnes, tels que les membres du support technique. Si vous êtes invité à enregistrer les métadonnées, procédez comme suit :  
  
1.  Pour chaque base de données qui présente l’état de **métadonnées manquantes**, activez la case à cocher en regard du nom de la base de données. L’enregistrement des métadonnées peut prendre plusieurs minutes. Si vous ne souhaitez pas enregistrer les métadonnées à ce stade, ne sélectionnez pas les cases à cocher.  
  
2.  Cliquez sur **Enregistrer**.  
  
SSMA analysera les schémas de MySQL et enregistrer les métadonnées dans le fichier projet.  
  
## <a name="opening-projects"></a>Ouverture de projets  
Lorsque vous ouvrez un projet, il est déconnecté de MySQL et à partir de SQL Server ou SQL Azure. Cela vous permet de travailler hors connexion. Pour mettre à jour les métadonnées, charger des objets de base de données dans SQL Server ou SQL Azure. Pour migrer des données, vous devez vous reconnecter à SQL Server ou SQL Azure.  
  
##### <a name="to-open-a-project"></a>Pour ouvrir un projet  
  
1.  Utilisez une des procédures suivantes :  
  
    1.  Sur le **fichier** menu, pointez sur **projets récents**.  
  
    2.  Sélectionnez le projet que vous souhaitez ouvrir.  
  
    3.  Sur le **fichier** menu, sélectionnez **ouvrir le projet**, recherchez le fichier de projet .m2ssproj, sélectionnez le fichier, puis cliquez sur **ouvrir**.  
  
2.  Se reconnecter à MySQL, sur le **fichier** menu, sélectionnez **se reconnecter à MySQL**.  
  
3.  Se reconnecter à SQL Server, sur le **fichier** menu, sélectionnez **se reconnecter à SQL Server**.  
  
4.  Se reconnecter à SQL Azure, sur le **fichier** menu, sélectionnez **se reconnecter à SQL Azure.**  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration est [se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
[Connexion à la base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
  
