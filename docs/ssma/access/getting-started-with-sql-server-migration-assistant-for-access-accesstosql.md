---
title: Prise en main SQL Server Migration Assistant pour Access | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- error list pane
- getting started
- menus
- metadata explorers
- output pane
- toolbars
- user interface
- user interface overview
ms.assetid: 462a731f-08f1-44e1-9eeb-4deac6d2f6c5
caps.latest.revision: 24
author: Shamikg
ms.author: Shamikg
manager: murato
ms.openlocfilehash: 450057d8167423b07a729a4bd05c65e67a533b37
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-sql-server-migration-assistant-for-access-accesstosql"></a>Prise en main de SQL Server Migration Assistant pour Access (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour l’accès vous permet de convertir rapidement aux objets de base de données Access à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou télécharger des objets de base de données SQL Azure, les objets résultants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure, et migrer les données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Si nécessaire, vous pouvez également lier des tables de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou sur des tables de base de données SQL Azure afin que vous puissiez continuer à utiliser vos applications front-end Access existantes avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure.  
  
Cette rubrique présente le processus d’installation et vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>L’installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur qui peut accéder à la fois les bases de données à migrer et l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Pour obtenir des instructions d’installation, consultez [installation Assistant Migration SQL Server pour l’accès &#40;AccessToSQL&#41;](../../ssma/access/installing-sql-server-migration-assistant-for-access-accesstosql.md).  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, pointez sur **Assistant Migration SQL Server pour l’accès**, puis sélectionnez **Assistant Migration SQL Server pour l’accès**.  
  
## <a name="using-ssma"></a>À l’aide de SSMA  
Après l’installation de SSMA, elle permet de vous familiariser avec l’interface utilisateur SSMA avant d’utiliser l’outil pour migrer des bases de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. L’interface utilisateur SSMA, y compris les explorateurs de métadonnées, métadonnées, barres d’outils, sortie et volet Liste d’erreur sont affichés dans le diagramme suivant :  
  
![SSMA pour Interface utilisateur graphique accès](../../ssma/access/media/ssmaforaccessgui.gif "SSMA pour Interface utilisateur graphique d’accès")  
  
Pour démarrer une migration, créez un nouveau projet, puis ajoutez les bases de données Access à l’Explorateur de métadonnées d’accès. Vous pouvez ensuite cliquer sur les objets dans l’Explorateur de métadonnées de l’accès pour effectuer des tâches telles que :
- Exportation d’un inventaire des objets de base de données Access à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure.
- Création de rapports qui évaluent les conversions [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure.
- Conversion de schémas d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou les schémas de base de données SQL Azure.

Vous pouvez également effectuer ces tâches en utilisant les menus et barres d’outils.  
  
Vous devez également vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Après une connexion réussie, une hiérarchie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données s’affiche dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées. Après la conversion de schémas d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schémas, vous pouvez sélectionner ces schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, puis charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Si vous avez sélectionné la base de données SQL Azure à partir de la migration vers la liste déroulante dans la boîte de dialogue Nouveau projet, vous devez vous connecter à la base de données SQL Azure. Après une connexion réussie, une hiérarchie de bases de données de base de données SQL Azure s’affiche dans l’Explorateur de métadonnées de base de données SQL Azure. Après avoir converti les schémas d’accès aux schémas de base de données SQL Azure, vous pouvez sélectionner ces schémas convertis dans l’Explorateur de métadonnées de base de données SQL Azure et puis charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Après avoir chargé des schémas convertis en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de la base de données SQL Azure, vous pouvez revenir à l’Explorateur de métadonnées d’accès et migrer des données à partir de bases de données Access dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou bases de données de base de données SQL Azure. Si nécessaire, vous pouvez également lier des tables de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou les tables de base de données SQL Azure.  
  
Pour plus d’informations sur ces tâches et leur exécution, consultez les rubriques suivantes :  
  
-   [Préparation pour la Migration des bases de données Access](http://msdn.microsoft.com/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)  
  
-   [Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
-   [Liaison des Applications d’accès à SQL Server](http://msdn.microsoft.com/82374ad2-7737-4164-a489-13261ba393d4)  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées que vous pouvez utiliser pour parcourir et d’effectuer des actions sur l’accès et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou bases de données de base de données SQL Azure.  
  
#### <a name="access-metadata-explorer"></a>Accédez à l’Explorateur de métadonnées  
Explorateur de métadonnées accès affiche des informations sur les bases de données Access qui ont été ajoutés au projet. Lorsque vous ajoutez une base de données Access, SSMA récupère les métadonnées relatives à cette base de données, les métadonnées qui sont disponibles dans l’Explorateur de métadonnées d’accès.  
  
Vous pouvez utiliser l’Explorateur de métadonnées d’accès pour effectuer les tâches suivantes :  
  
-   Parcourir les tables dans chaque base de données Access.  
  
-   Sélectionner les objets pour la conversion et convertir les objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe. Pour plus d’informations, consultez [convertir des objets de base de données Access](http://msdn.microsoft.com/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c).  
  
-   Sélectionner des objets pour la migration des données et de migrer les données à partir de ces objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations, consultez [migration d’accéder aux données dans SQL Server](http://msdn.microsoft.com/f3b18af7-1af0-499d-a00d-a0af94895625).  
  
-   La liaison et la dissociation d’accès et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] tables.  
  
#### <a name="sql-server-or-azure-sql-db-metadata-explorer"></a>SQL Server ou l’Explorateur de métadonnées de base de données SQL Azure  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de l’Explorateur de métadonnées de base de données SQL Azure affiche des informations sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de la base de données SQL Azure, SSMA récupère les métadonnées relatives à cette instance et le stocke dans le fichier projet.  
  
Vous pouvez utiliser SQL Server ou l’Explorateur de métadonnées de base de données SQL Azure pour sélectionner des objets de base de données Access convertis et charger (synchronisation de) ces objets dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure.  
  
Pour plus d’informations, consultez [le chargement des objets de base de données convertie dans SQL Server](http://msdn.microsoft.com/4e854eee-b10c-4f0b-9d9e-d92416e6f2ba).  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées de l’accès, quatre onglets s’affichent : **Table**, **le mappage de Type**, **propriétés**, et **données**. Si vous sélectionnez une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, trois onglets s’affichent : **Table**, **SQL**, et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées de l’accès, vous pouvez modifier les mappages de type. Veillez à effectuer ces modifications avant de créer des rapports ou convertir des schémas.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’Explorateur de métadonnées, vous pouvez modifier les propriétés de table et d’index sur la **Table** onglet. Effectuer ces modifications avant de charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations, consultez [convertir des objets de base de données Access](http://msdn.microsoft.com/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c).  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA a deux barres d’outils : une barre d’outils du projet et une barre d’outils de migration.  
  
#### <a name="the-project-toolbar"></a>La barre d’outils du projet  
La barre d’outils du projet contient des boutons pour l’utilisation de projets, l’ajout de fichiers de base de données Access et la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Ces boutons ressemblent aux commandes sur le **fichier** menu.  
  
#### <a name="the-migration-toolbar"></a>La barre d’outils de migration  
La barre d’outils de migration comprend les commandes suivantes :  
  
|Bouton|Fonction|  
|----------|------------|  
|**Convertir, charger et migrer**|Convertit des bases de données Access, charge les objets convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure, et migre les données en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou Azure SQL DB, tout en une seule étape.|  
|**Créer des rapports**|Convertit le schéma sélectionné accès [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une syntaxe de base de données SQL Azure, puis crée un rapport qui indique la réussite de la conversion a.<br /><br />Cette commande est disponible uniquement lorsque les objets sont sélectionnés dans l’Explorateur de métadonnées d’accès.|  
|**Convertir le schéma**|Convertit le schéma sélectionné accès [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des schémas de base de données SQL Azure.<br /><br />Cette commande est disponible uniquement lorsque les objets sont sélectionnés dans l’Explorateur de métadonnées d’accès.|  
|**Migrer des données**|Migre les données à partir de la base de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Avant d’exécuter cette commande, vous devez convertir les schémas d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des schémas de base de données SQL Azure, puis charger les objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure.<br /><br />Cette commande est disponible uniquement lorsque les objets sont sélectionnés dans l’Explorateur de métadonnées d’accès.|  
|**Arrêter**|Arrête le processus en cours, telles que la conversion d’objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une syntaxe de base de données SQL Azure.|  
  
### <a name="menus"></a>Menus  
SSMA contient les menus suivants :  
  
|Menu| Description|  
|--------|---------------|  
|**Fichier**|Contient des commandes pour l’Assistant de Migration, utilisation de projets, ajout et suppression des fichiers de base de données Access et la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure.|  
|**Modifier**|Contient des commandes pour la recherche et utilisation du texte dans les pages Détails, telles que la copie [!INCLUDE[tsql](../../includes/tsql_md.md)] dans le volet SQL. Pour ouvrir la **gérer les signets** boîte de dialogue, dans le menu Edition, cliquez sur Gérer les signets. Dans la boîte de dialogue, vous verrez une liste des signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Affichage**|Contient le **synchroniser les explorateurs de métadonnées** commande. Cette opération synchronise les objets entre l’Explorateur de métadonnées d’accès et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Explorateur de métadonnées de base de données SQL Azure. Contient également des commandes pour afficher et masquer le **sortie** et **liste d’erreurs** volets et une option **dispositions** pour gérer les mises en page.|  
|**Outils**|Contient des commandes pour créer des rapports, exporter des données, migrer des objets et des données, liez des tables et fournit des boîtes de dialogue accès à global et les paramètres de projet.|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la **sur** boîte de dialogue.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volets de sortie et liste d’erreurs  
Le **vue** menu fournit des commandes pour activer/désactiver la visibilité du volet sortie et le volet de la liste d’erreurs :  
  
-   Le volet de résultats affiche les messages d’état à partir de SSMA pendant la conversion de l’objet, la synchronisation de l’objet et la migration des données.  
  
-   Le volet de la liste d’erreurs affiche l’erreur, d’avertissement et messages d’information dans une liste que vous pouvez trier.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
