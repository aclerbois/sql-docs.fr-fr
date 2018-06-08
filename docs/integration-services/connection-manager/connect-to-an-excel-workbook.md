---
title: Établir une connexion à un classeur Excel | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Excel [Integration Services]
ms.assetid: d9746318-3669-4ce2-bbb0-4a1bd471c9dd
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 62c9d7b4de0b764ff7208b0fa077ec12b58dec60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-an-excel-workbook"></a>Établir une connexion à un classeur Excel
  Pour connecter un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à un classeur Microsoft Office Excel, vous devez utiliser un gestionnaire de connexions Excel.  
  
 Vous pouvez créer ces gestionnaires de connexions à partir de la zone Gestionnaires de connexions dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou à partir de l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
 
## <a name="connectivity-components-for-microsoft-excel-and-access-files"></a>Composants de connectivité des fichiers Microsoft Excel et Microsoft Access
  
Vous devrez peut-être télécharger les composants de connectivité pour les fichiers Microsoft Office s’ils ne sont pas déjà installés. Téléchargez la dernière version des composants de connectivité pour les fichiers Excel et Access ici : [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920).
  
La dernière version des composants peut ouvrir les fichiers créés dans des versions antérieures d’Excel.

Si la version 32 bits de Microsoft Office est installée sur l’ordinateur, vous devez installer la version 32 bits des composants et vérifier que vous exécutez le package en mode 32 bits.

Si vous avez un abonnement Office 365, veillez à télécharger Access Database Engine 2016 Redistributable et non Microsoft Access 2016 Runtime. Lorsque vous exécutez le programme d’installation, il est possible qu’un message d’erreur s’affiche indiquant que vous ne pouvez pas installer le téléchargement côte à côte avec les composants Office « Démarrer en un clic ». Pour contourner ce message d’erreur et installer les composants avec succès, exécutez l’installation en mode silencieux en ouvrant une fenêtre d’invite de commandes et en exécutant le fichier .EXE que vous avez téléchargé avec l’option `/quiet`. Exemple :

`C:\Users\<user name>\Downloads\AccessDatabaseEngine.exe /quiet`

## <a name="create-an-excel-connection-manager"></a>Créer un gestionnaire de connexions Excel

### <a name="to-create-an-excel-connection-manager-from-the-connection-managers-area"></a>Pour créer un gestionnaire de connexions Excel à partir de la zone Gestionnaires de connexions  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package.  
  
2.  Cliquez avec le bouton droit dans la zone **Gestionnaires de connexions** et sélectionnez **Nouvelle connexion**.  
  
3.  Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS** , sélectionnez **Excel**, puis configurez le gestionnaire de connexions.  
  
     Pour plus d’informations sur les options de configuration qui sont disponibles pour ce gestionnaire de connexions, consultez [Éditeur du gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager-editor.md).  
  
### <a name="to-create-an-excel-connection-from-the-sql-server-import-and-export-wizard"></a>Pour créer une connexion à Excel à partir de l'Assistant Importation et Exportation SQL Server  
  
1.  Démarrez la version 32 bits de l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Dans la page **Choisir une source de données** , pour **Source de données**, sélectionnez **Microsoft Excel**, puis configurez la connexion à Excel.  
  
     Pour plus d’informations sur les options de configuration qui sont disponibles pour ce type de connexion, consultez [Éditeur du gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager-editor.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Établir une connexion à une base de données Access](../../integration-services/connection-manager/connect-to-an-access-database.md)  
  
  
