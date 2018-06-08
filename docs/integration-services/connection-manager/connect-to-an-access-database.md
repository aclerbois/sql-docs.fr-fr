---
title: Établir une connexion à une base de données Access | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Access [Integration Services]
- Access databases [Integration Services]
ms.assetid: 229fbd46-ef6a-4609-a4cc-d80d52c33cf1
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c8e0811b894e96c4ac7b11ef377765aa6b56cdbf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-an-access-database"></a>Établir une connexion à une base de données Access
  Connecter un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à une source de données Microsoft Office Access requiert un gestionnaire de connexions OLE DB et un fournisseur de données. Le fournisseur de données que vous utilisez dépend de la version d’Access qui a été utilisée pour créer la source de données :  
  
-   Pour Access 2003 et les versions antérieures, le package nécessite le fournisseur OLE DB pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet.  
  
-   Pour Access 2007, le package requiert le fournisseur OLE DB pour le moteur de base de données Microsoft Office Access 12.0.  
  
 Vous pouvez créer un gestionnaire de connexions OLE DB et sélectionner le fournisseur de données correspondant à partir de la zone Gestionnaires de connexions dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou à partir de l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Sur un ordinateur 64 bits, vous devez exécuter les packages qui se connectent à des sources de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access en mode 32 bits. Les deux fournisseurs OLE DB pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet et pour le moteur de base de données Microsoft Office Access 12.0 sont disponibles uniquement dans des versions 32 bits.  

## <a name="connectivity-components-for-microsoft-excel-and-access-files"></a>Composants de connectivité des fichiers Microsoft Excel et Microsoft Access
  
Vous devrez peut-être télécharger les composants de connectivité pour les fichiers Microsoft Office s’ils ne sont pas déjà installés. Téléchargez la dernière version des composants de connectivité pour les fichiers Excel et Access ici : [Microsoft Access Database Engine 2016 Redistributable](https://www.microsoft.com/download/details.aspx?id=54920).
  
La dernière version des composants peut ouvrir des fichiers créés dans des versions antérieures d’Access.

Si la version 32 bits de Microsoft Office est installée sur l’ordinateur, vous devez installer la version 32 bits des composants et vérifier que vous exécutez le package en mode 32 bits.

Si vous avez un abonnement Office 365, veillez à télécharger Access Database Engine 2016 Redistributable et non Microsoft Access 2016 Runtime. Lorsque vous exécutez le programme d’installation, il est possible qu’un message d’erreur s’affiche indiquant que vous ne pouvez pas installer le téléchargement côte à côte avec les composants Office « Démarrer en un clic ». Pour contourner ce message d’erreur, exécutez l’installation en mode silencieux en ouvrant une fenêtre d’invite de commandes et en exécutant le fichier .EXE que vous avez téléchargé avec l’option `/quiet`. Exemple :

`C:\Users\<user name>\Downloads\AccessDatabaseEngine.exe /quiet`
  
## <a name="connecting-to-a-data-source-in-access-2003-or-earlier-format"></a>Connexion à une source de données dans Access 2003 ou un format antérieur  
  
### <a name="to-create-an-access-connection-manager-from-the-connection-managers-area"></a>Pour créer un gestionnaire de connexions Access à partir de la zone Gestionnaires de connexions  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package.  
  
2.  Cliquez avec le bouton droit dans la zone **Gestionnaires de connexions** et choisissez **Nouvelle connexion OLE DB**.  
  
3.  Dans la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** , cliquez sur **Nouveau**.  
  
     Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
4.  Dans la boîte de dialogue **Gestionnaire de connexions** , pour **Fournisseur**, sélectionnez **Fournisseur OLE DB pour Microsoft Jet 4.0**, puis configurez le gestionnaire de connexions comme il convient.  
  
### <a name="to-create-an-access-connection-from-the-sql-server-import-and-export-wizard"></a>Pour créer une connexion à Access à partir de l'Assistant Importation et exportation SQL Server  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], démarrez l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Dans la page **Choisir une source de données** , pour **Source de données**, sélectionnez **Microsoft Access**, puis configurez la connexion à Access.  
  
     Quand vous sélectionnez **Microsoft Access** comme **Source de données**, l’Assistant crée automatiquement le gestionnaire de connexions OLE DB nécessaire avec le fournisseur de données correct. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
## <a name="connecting-to-a-data-source-in-access-2007-format"></a>Connexion à une source de données dans le format Access 2007  
 Pour accéder à une source de données Access 2007, le gestionnaire de connexions OLE DB requiert le fournisseur OLE DB pour le moteur de base de données Microsoft Office Access 12.0. Ce fournisseur est installé automatiquement avec la version 2007 de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office System. Si le système Office 2007 n'est pas installé sur l'ordinateur sur lequel [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] s'exécute, vous devez installer le fournisseur séparément. Pour installer le fournisseur OLE DB pour le moteur de base de données Microsoft Office Access 12.0, téléchargez et installez les composants fournis dans la page web [Pilote d’Office System 2007 : composants de connectivité des données](http://go.microsoft.com/fwlink/?LinkId=98155).  
  
### <a name="to-create-an-ole-db-connection-manager-from-the-connection-managers-area"></a>Pour créer un gestionnaire de connexions OLE DB à partir de la zone Gestionnaires de connexions  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package.  
  
2.  Cliquez avec le bouton droit dans la zone **Gestionnaires de connexions** et choisissez **Nouvelle connexion OLE DB**.  
  
3.  Dans la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** , cliquez sur **Nouveau**.  
  
     Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
4.  Dans la boîte de dialogue **Gestionnaire de connexions** , pour **Fournisseur**, sélectionnez **Fournisseur OLE DB du moteur de base de données Microsoft Office Access 12.0**, puis configurez le gestionnaire de connexions comme il convient.  
  
    > [!NOTE]  
    >  Pour établir une connexion à une source de données qui utilise Access 2007, vous ne pouvez pas sélectionner **Fournisseur OLE DB pour Microsoft Jet 4.0** comme **Source de données**.  
  
### <a name="to-create-an-ole-db-connection-from-the-sql-server-import-and-export-wizard"></a>Pour créer une connexion OLE DB à partir de l'Assistant Importation et exportation SQL Server  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], démarrez l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Dans la page **Choisir une source de données** , pour **Source de données**, sélectionnez **Fournisseur OLE DB du moteur de base de données Microsoft Office Access 12.0**, puis configurez la connexion comme il convient.  
  
    > [!NOTE]  
    >  Pour établir une connexion à une source de données qui utilise Access 2007, vous ne pouvez pas sélectionner **Fournisseur OLE DB pour Microsoft Jet 4.0** comme **Source de données**.  
  
     Quand vous sélectionnez **Fournisseur OLE DB du moteur de base de données Microsoft Office Access 12.0** comme **Source de données**, l’Assistant crée automatiquement le gestionnaire de connexions OLE DB nécessaire avec le fournisseur de données correct. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Établir une connexion à un classeur Excel](../../integration-services/connection-manager/connect-to-an-excel-workbook.md)  
  
  
