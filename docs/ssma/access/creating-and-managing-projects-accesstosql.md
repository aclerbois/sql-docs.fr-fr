---
title: Création et gestion de projets (AccessToSQL) | Documents Microsoft
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
- creating projects
- new projects
- opening projects
- projects
- projects, creating and managing
- saving metadata
- saving projects
ms.assetid: f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7
caps.latest.revision: 22
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 4025227271ed2c73d11aef838ef902e2438b2dce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-and-managing-projects-accesstosql"></a>Création et gestion de projets (AccessToSQL)
Pour migrer des bases de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez d’abord créer un projet SSMA. Le projet est un fichier qui contient des métadonnées sur les bases de données Access que vous souhaitez migrer vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, les métadonnées relatives à l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure qui recevra les données, les objets migrés [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] informations de connexion et les paramètres du projet.  
  
## <a name="reviewing-default-project-settings"></a>Examen des paramètres de projet par défaut  
SSMA contient plusieurs options pour la conversion et la synchronisation des objets de base de données et de conversion des données. Le paramètre par défaut de ces options est approprié pour de nombreux utilisateurs. Toutefois, avant de créer un nouveau projet SSMA, vous devez examiner les options et, si vous le souhaitez, modifiez les paramètres par défaut qui seront utilisés pour tous les nouveaux projets.  
  
**Pour passer en revue les paramètres de projet par défaut**  
  
1.  Sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**.  
  
2.  Sélectionnez le type de projet dans **Version cible de la Migration** la liste déroulante pour les paramètres doivent être affichés / modifié, puis cliquez sur **général** onglet.  
  
3.  Dans le volet gauche, cliquez sur **Conversion**.  
  
4.  Dans le volet droit, passez en revue les options. Pour plus d’informations sur ces options, consultez [paramètres du projet (Conversion)](http://msdn.microsoft.com/en-us/bcebc635-c638-4ddb-924c-b9ccfef86388).  
  
5.  Modifiez les options selon les besoins.  
  
6.  Répétez les étapes précédentes pour le **Migration**, **GUI**, et **le mappage de Type** pages.  
  
    -   Pour plus d’informations sur les options de migration, consultez [paramètres du projet (Migration)](http://msdn.microsoft.com/en-us/4caebc9c-8680-4b99-a8fa-89c43161c95d).  
  
    -   Pour plus d’informations sur les options d’interface utilisateur, consultez [les paramètres de projet (GUI)](http://msdn.microsoft.com/en-us/cf06baf1-8714-48a3-95dc-781f6ca53693).  
  
    -   Pour plus d’informations sur les paramètres de mappage de type de données, consultez [paramètres du projet (Type de mappage)](http://msdn.microsoft.com/en-us/b87b9683-abed-4677-8c50-18bdba704655).  
  
    -   Pour plus d’informations sur les paramètres de SQL Azure, consultez [paramètres du projet (SQL Azure)](http://msdn.microsoft.com/en-us/bbb8a204-d0e4-4f0b-9709-271feb1f136e).  
  
**Remarque** paramètres SQL Azure seront disponibles uniquement lorsque vous sélectionnez la Migration vers SQL Azure lors de la création d’un projet.  
  
## <a name="creating-new-projects"></a>Création de projets  
SSMA démarre sans charger un projet par défaut. Pour migrer des données à partir de bases de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vous devez créer un projet.  
  
**Pour créer un nouveau projet**  
  
1.  Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
  
    La boîte de dialogue **Nouveau projet** s'affiche.  
  
2.  Dans la zone **Nom** , tapez le nom de votre projet.  
  
3.  Dans le **emplacement** zone, entrez ou sélectionnez un dossier pour le projet  
  
4.  Dans la liste déroulante pour la Migration vers le bas, sélectionnez une de SQL Server 2005 / SQL Server 2008 / SQL Server 2012 / SQL Server 2014 / SQL Server 2016 / Azure SQL DB, puis cliquez sur **OK**.  
  
SSMA crée le fichier de projet. Vous pouvez désormais effectuer l’étape suivante de [Ajout d’une ou plusieurs bases de données Access](http://msdn.microsoft.com/en-us/e944c740-4c8a-4bc1-b0ed-be57bc06dced).  
  
## <a name="customizing-project-settings"></a>Personnalisation des paramètres de projet  
Outre la définition des paramètres de projet par défaut, qui s’appliquent à tous les nouveaux projets SSMA, vous pouvez également personnaliser les paramètres pour chaque projet. Pour plus d’informations, consultez [Conversion de paramètre et les Options de Migration](http://msdn.microsoft.com/en-us/0a7304df-2f35-4453-96ef-7ac83dea1167).  
  
Lorsque vous personnalisez les mappages de types de données entre bases de données source et cible, vous pouvez définir des mappages pour le projet, la base de données ou le niveau de l’objet. Pour plus d’informations sur le mappage de type, consultez [Source de mappage et les Types de données cible](http://msdn.microsoft.com/en-us/b362a075-16e7-423f-b63f-e1e9f02844a9).  
  
## <a name="saving-projects"></a>L’enregistrement de projets  
Lorsque vous enregistrez un projet, SSMA conserve les paramètres du projet et éventuellement les métadonnées de la base de données, dans le fichier projet.  
  
**Pour enregistrer un projet**  
  
-   Sur le **fichier** menu, sélectionnez **enregistrer le projet**.  
  
    Si les bases de données au sein du projet ont été modifiées ou n’ont pas été converties, SSMA vous invitera à enregistrer les métadonnées dans le projet. L’enregistrement des métadonnées vous permet de travailler hors connexion. Il vous permet également d’envoyer un fichier de projet complet à d’autres personnes, y compris le personnel de support technique. Si vous êtes invité à enregistrer les métadonnées, procédez comme suit :  
  
    1.  Pour chaque base de données qui présente l’état de **métadonnées manquantes**, activez la case à cocher en regard du nom de la base de données.  
  
        L’enregistrement des métadonnées peut prendre plusieurs minutes. Si vous ne souhaitez pas enregistrer les métadonnées à ce stade, ne sélectionnez pas les cases à cocher.  
  
    2.  Cliquez sur **Enregistrer**.  
  
        SSMA analysera les schémas d’accès et enregistrer les métadonnées dans le fichier projet.  
  
## <a name="opening-projects"></a>Ouverture de projets  
Lorsque vous ouvrez un projet, il est déconnecté de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Cela vous permet de travailler hors connexion. Pour mettre à jour les métadonnées des objets de base de données de charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Pour migrer des données, vous devez vous reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
**Pour ouvrir un projet**  
  
1.  Utilisez une des procédures suivantes :  
  
    -   Sur le **fichier** menu, pointez sur **projets récents**, puis sélectionnez le projet que vous souhaitez ouvrir.  
  
    -   Sur le **fichier** menu, sélectionnez **ouvrir le projet**, recherchez le fichier de projet .a2ssproj, sélectionnez le fichier, puis cliquez sur **ouvrir**.  
  
2.  Pour vous reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], dans le **fichier** menu, sélectionnez **se reconnecter à SQL Server**.  
  
3.  Se reconnecter à SQL Azure, sur le **fichier** menu, sélectionnez **se reconnecter à SQL Azure.**  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration consiste à [ajouter une ou plusieurs bases de données Access](http://msdn.microsoft.com/en-us/e944c740-4c8a-4bc1-b0ed-be57bc06dced).  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
[Ajout et suppression de fichiers de base de données Access](http://msdn.microsoft.com/en-us/e944c740-4c8a-4bc1-b0ed-be57bc06dced)  
  
