---
title: Générer des scripts (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: scripting
ms.reviewer: mathoma
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9711c617-3c68-4e5a-aea3-befc64d51524
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6d3d77c77be6e16e6d4deb5fd968774717230f88
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="generate-scripts-sql-server-management-studio"></a>Générer des scripts (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit deux mécanismes pour générer des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] . Vous pouvez créer des scripts pour plusieurs objets à l’aide de **l’Assistant Générer et publier des scripts**. Vous pouvez également générer un script pour des objets individuels ou plusieurs objets à l'aide du menu **Script en tant que** dans l' **Explorateur d'objets**.  

Pour obtenir un tutoriel détaillé sur les scripts de différents objets à l’aide de SQL Server Management Studio (SSMS), consultez [Tutoriel : Écriture de scripts dans SSMS](https://docs.microsoft.com/en-us/sql/ssms/tutorials/scripting-ssms).

  
## <a name="before-you-begin"></a>Avant de commencer  
 Choisissez le mécanisme qui correspond le mieux à vos besoins.  
  
###  <a name="GenPubScriptWiz"></a> Assistant Générer et publier des scripts  
 Utilisez l' **Assistant Générer et publier des scripts** pour créer un script [!INCLUDE[tsql](../../includes/tsql-md.md)] pour plusieurs objets. L'Assistant génère un script de tous les objets d'une base de données, ou un sous-ensemble des objets que vous sélectionnez. L'Assistant propose de nombreuses options pour vos scripts et vous permet notamment d'inclure ou non des autorisations, un classement et des contraintes. Pour obtenir des instructions sur l'utilisation de l'Assistant, consultez [Generate and Publish Scripts Wizard](../../relational-databases/scripting/generate-and-publish-scripts-wizard.md).  
  
###  <a name="OEScriptAsMenu"></a> Menu Script en tant que de l'Explorateur d'objets  
 Vous pouvez utiliser le menu **« Script en tant que » de l’Explorateur d’objets** pour générer le script d’un objet unique, de plusieurs objets ou de plusieurs instructions pour un objet unique. Vous pouvez choisir un type de scripts parmi plusieurs ; par exemple pour créer, modifier ou supprimer l'objet. Vous pouvez enregistrer le script dans une fenêtre Éditeur de requête, dans un fichier ou dans le Presse-papiers. Le script est créé au format Unicode.  
  
##  <a name="ScriptSingleObject"></a> Pour générer un script d'un objet unique  
 **Pour générer un script d'un objet unique**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, puis développez la base de données contenant l'objet pour lequel un script doit être généré.  
  
3.  Développez la catégorie de l'objet. Par exemple, développez le nœud **Tables** ou **Vues** .  
  
4.  Cliquez avec le bouton droit sur l’objet et pointez sur **Générer un script de \<type d’objet> en tant que**. Par exemple, pointez sur **Générer un script de la table en tant que**.  
  
5.  Pointez sur le type de script, tel que **Create to** ou **Alter to**.  
  
6.  Sélectionnez l'emplacement dans lequel vous voulez enregistrer le script, tel que **Nouvelle fenêtre d'éditeur de requête** ou **Presse-papiers**.  

    ![Générer un script de la table](media/generate-scripts-sql-server-management-studio/scripttable.png)
  
  
 Vous pouvez utiliser le volet **Détails de l’Explorateur d’objets** afin de générer un script pour plusieurs objets appartenant à la même catégorie.  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, puis développez la base de données contenant les objets pour lequel un script doit être généré.  
  
3.  Développez le nœud de catégorie des types d'objet pour lesquels vous voulez générer un script, tel que le nœud **Tables** .  
  
4.  Ouvrez le volet **Détails de l'Explorateur d'objets** en appuyant sur **F7**, ou en ouvrant le menu **Affichage** et en sélectionnant **Détails de l'Explorateur d'objets**.  
  
5.  Cliquez avec le bouton gauche sur l'un des objets pour lesquels vous voulez générer un script.  
  
6.  Appuyez sur Crtl + clic gauche sur le deuxième objet pour lequel vous voulez générer un script.  
  
7.  Cliquez avec le bouton droit sur l’un des objets sélectionnés et sélectionnez **Générer un script de \<type d’objet> en tant que**.  

    ![Explorateur d’objets](media/generate-scripts-sql-server-management-studio/objectexplorerdetails.png)
  
  
