---
title: Gestionnaire de connexions OLE DB | Microsoft Docs
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
f1_keywords:
- sql13.dts.designer.oledbconnection.f1
helpviewer_keywords:
- OLE DB connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], OLE DB
- connections [Integration Services], OLE DB
ms.assetid: 91e3622e-4b1a-439a-80c7-a00b90d66979
caps.latest.revision: 59
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a65995d0e4f36cd1e330c01345834d1bca68b948
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-connection-manager"></a>Gestionnaire de connexions OLE DB
  Un gestionnaire de connexions OLE DB permet à un package de se connecter à une source de données à l'aide d'un fournisseur OLE DB. Par exemple, un gestionnaire de connexions OLE DB qui se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
    
> [!NOTE]    
>  Le fournisseur OLEDB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 11.0 ne prend pas en charge les mots clés de la nouvelle chaîne de connexion (MultiSubnetFailover=True) pour le clustering de basculement de sous-réseaux multiples. Pour plus d’informations, consultez les [Notes de publication de SQL Server](http://go.microsoft.com/fwlink/?LinkId=247824) et la publication de blog [Basculement de sous-réseaux multiples Always On et SSIS](http://www.mattmasson.com/2012/03/alwayson-multi-subnet-failover-and-ssis/)sur www.mattmasson.com.    
    
> [!NOTE]    
>  Si la source de données est [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, elle requiert un fournisseur de données différent des versions antérieures d’Excel ou d’Access. Pour plus d’informations, consultez [Établir une connexion à un classeur Excel](../../integration-services/connection-manager/connect-to-an-excel-workbook.md) et [Établir une connexion à une base de données Access](../../integration-services/connection-manager/connect-to-an-access-database.md).    
    
 Plusieurs tâches et composants de flux de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilisent un gestionnaire de connexions OLE DB. Ainsi, la source et la destination OLE DB utilisent ce gestionnaire de connexions pour extraire et charger des données, tandis que la tâche d’exécution SQL utilise ce gestionnaire pour se connecter à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d’exécuter des requêtes.    
    
 Le gestionnaire de connexions OLE DB est également utilisé pour accéder à des sources de données OLE DB dans des tâches personnalisées écrites dans du code non géré utilisant un langage comme C++.    
    
 Quand vous ajoutez un gestionnaire de connexions OLE DB à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera résolu en une connexion OLE DB au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connections** sur le package.    
    
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **OLEDB**.    
    
 Le gestionnaire de connexions OLE DB peut être configuré de plusieurs manières :    
    
-   Indiquez une chaîne de connexion spécifique configurée pour répondre aux besoins du fournisseur sélectionné.    
    
-   Selon le fournisseur, incluez le nom de la source de données à laquelle se connecter.    
    
-   Fournissez les informations d'identification de sécurité nécessaires selon le fournisseur sélectionné.    
    
-   Indiquez si la connexion créée à partir du gestionnaire de connexions est conservée au moment de l'exécution.    
    
## <a name="logging"></a>Journalisation    
 Vous pouvez consigner les appels que le gestionnaire de connexions OLE DB effectue vers des fournisseurs de données externes. Cette fonctionnalité de journalisation permet de résoudre des problèmes liés aux connexions que le gestionnaire de connexions OLE DB établit avec des sources de données externes. Pour consigner les appels que le gestionnaire de connexions OLE DB effectue vers des fournisseurs de données externes, activez la journalisation de package et sélectionnez l’événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).    
    
## <a name="configuration-of-the-oledb-connection-manager"></a>Configuration du gestionnaire de connexions OLEDB    
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation. Pour plus d’informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md). Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez la documentation de la classe **T:Microsoft.SqlServer.Dts.Runtime.ConnectionManager** dans le Guide du développeur.    
    
## <a name="related-content"></a>Contenu associé    
    
-   Article Wiki, [SSIS with Oracle Connectors](http://go.microsoft.com/fwlink/?LinkId=220670) (SSIS avec connecteurs Oracle) sur social.technet.microsoft.com.    
    
-   Article technique, [Connection Strings for OLE DB Providers](http://go.microsoft.com/fwlink/?LinkId=220744)(Chaînes de connexion pour les fournisseurs OLE DB), sur carlprothman.net.    
    
## <a name="configure-ole-db-connection-manager"></a>Configurer le gestionnaire de connexions OLE DB
  La boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** vous permet d’ajouter une connexion à une source de données, qui peut être une nouvelle connexion ou la copie d’une connexion existante.  
  
> [!NOTE]  
>  Si la source de données est [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, la source de données requiert un gestionnaire de connexions différent des versions antérieures d'Excel. Pour plus d’informations, consultez [Établir une connexion à un classeur Excel](../../integration-services/connection-manager/connect-to-an-excel-workbook.md).  
>   
>  Si la source de données est [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, la source de données requiert un fournisseur OLE DB différent des versions antérieures d’Access. Pour plus d’informations, consultez [Établir une connexion à une base de données Access](../../integration-services/connection-manager/connect-to-an-access-database.md).  
  
 Pour en savoir plus sur le gestionnaire de connexions OLE DB, consultez [Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Connexions de données**  
 Sélectionnez une connexion de données OLE DB existante dans la liste.  
  
 **Propriétés des connexions de données**  
 Permet d'afficher les propriétés et les valeurs relatives à la connexion de données OLE DB sélectionnée.  
  
 **Nouveau**  
 Créez une connexion de données OLE DB à l’aide de la boîte de dialogue **Gestionnaire de connexions** .  
  
 **Supprimer**  
 Sélectionnez une connexion de données, puis supprimez-la à l’aide du bouton **Supprimer** .  
  
## <a name="see-also"></a> Voir aussi    
 [Source OLE DB](../../integration-services/data-flow/ole-db-source.md)     
 [Destination OLE DB](../../integration-services/data-flow/ole-db-destination.md)     
 [Exécution de requêtes SQL, tâche](../../integration-services/control-flow/execute-sql-task.md)     
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)    
    
  
