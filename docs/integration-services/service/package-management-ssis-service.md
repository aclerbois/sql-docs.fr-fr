---
title: Gestion de packages (Service SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: service
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtsserver.importpackage.f1
- sql13.dts.dtsserver.exportpackage.f1
helpviewer_keywords:
- SQL Server Integration Services packages, managing
- packages [Integration Services], managing
- Integration Services packages, managing
- storing packages
- Stored Packages folder
- current packages
- Running Packages folder
- status information [Integration Services]
- SSIS packages, managing
- storage [Integration Services]
- monitoring [Integration Services], packages
- Integration Services service, package management
- services [Integration Services], package management
ms.assetid: 0261ed9e-3b01-4e37-a9d4-d039c41029b6
caps.latest.revision: 59
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 09aa087d02f97e4c4c989ceb5186b79a123500cb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="package-management-ssis-service"></a>Gestion de packages (Service SSIS)
  La gestion de packages inclut la surveillance, la gestion, l’importation et l’exportation de packages.  
 
 ## <a name="package-store"></a>Magasin de packages  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit deux dossiers de premier niveau pour l’accès aux packages : 
 - **Exécution des packages** 
 - **Packages stockés**

 Le dossier **Exécution des packages** répertorie les packages en cours d'exécution sur le serveur. Le dossier **Packages stockés** répertorie les packages enregistrés dans le magasin de packages. Ce sont les seuls packages gérés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Le magasin de packages peut comprendre la base de données msdb et/ou les dossiers du système de fichiers répertoriés dans le fichier de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Le fichier de configuration indique la base de données msdb et les dossiers du système de fichiers à gérer. Il est possible que vous disposiez également de packages stockés ailleurs dans le système de fichiers qui ne sont pas gérés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Les packages que vous enregistrez dans msdb sont stockés dans une table nommée sysssispackages. Quand vous enregistrez des packages dans msdb, vous pouvez les regrouper dans des dossiers logiques. L’utilisation de dossiers logiques vous permet d’organiser les packages selon leur finalité ou bien de les filtrer dans la table sysssispackages. Créez des dossiers logiques dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Par défaut, tous les dossiers logiques que vous ajoutez à la base de données msdb sont inclus automatiquement dans le magasin de packages.  
  
 Les dossiers logiques que vous créez sont représentés sous forme de lignes dans la table sysssispackagefolders de msdb. Les colonnes folderid et parentfolderid de sysssispackagefolders définissent l’arborescence des dossiers. Les dossiers logiques racines de msdb correspondent aux lignes de sysssispackagefolders ayant des valeurs Null dans la colonne parentfolderid. Pour plus d’informations, consultez [sysssispackages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysssispackages-transact-sql.md) et [sysssispackagefolders (Transact-SQL&)](../../relational-databases/system-tables/sysssispackagefolders-transact-sql.md).  
  
 Lorsque vous ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puis que vous vous connectez à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], les dossiers msdb gérés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] apparaissent dans le dossier Packages stockés. Si le fichier de configuration spécifie des dossiers de système de fichiers racines, le dossier Packages stockés répertorie également les packages enregistrés dans le système de fichiers dans ces mêmes dossiers et dans tous les sous-dossiers.  
  
 Vous pouvez stocker des packages dans n'importe quel dossier de système de fichiers mais ces dossiers ne seront pas consignés dans la liste des sous-dossiers du dossier **Packages stockés** , à moins que vous n'ajoutiez le dossier à la liste des dossiers dans le fichier de configuration du magasin de packages. Pour plus d’informations sur le fichier de configuration, consultez [Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
 Le dossier **Exécution des packages** ne contient aucun sous-dossier et n'est pas extensible.  
  
 Par défaut, le dossier **Packages stockés** contient deux dossiers : **File System** et **MSDB**. Le dossier **File System** répertorie les packages enregistrés dans le système de fichiers. Le fichier de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] indique l'emplacement de ces fichiers. Le dossier par défaut est le dossier Packages, situé dans %Program Files%\Microsoft SQL Server\100\DTS. Le dossier **MSDB** répertorie les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enregistrés dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb sur le serveur. La table sysssispackages contient les packages enregistrés dans la base de données msdb.  
  
 Pour visualiser la liste des packages stockés dans le magasin de packages, vous devez ouvrir [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et vous connecter à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="monitor-running-packages"></a>Surveiller les packages en cours d’exécution  
 Le dossier **Exécution des packages** répertorie les packages en cours d’exécution. Pour afficher des informations relatives aux packages en cours d'exécution sur la page **Résumé** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez sur le dossier **Exécution des packages** . Des informations telles que la durée d'exécution des packages en cours d'exécution figurent sur la page **Résumé** . Vous pouvez également actualiser l'affichage du dossier pour obtenir les informations les plus récentes.  
  
 Pour afficher des informations sur un seul package en cours d'exécution sur la page **Résumé** , cliquez sur le package. La page **Résumé** affiche des informations telles que la version et la description du package.  
  
Pour arrêter un package en cours d’exécution dans le dossier **Exécution des packages**, cliquez avec le bouton droit sur le package, puis cliquez sur **Arrêter**.  
  
## <a name="view-packages-in-ssms"></a>Afficher les packages dans SSMS
    
 Cette procédure explique comment se connecter à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et visualiser la liste des packages gérés par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="to-connect-to-integration-services"></a>Pour se connecter à Integration Services  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, puis sur **Microsoft SQL Server 2005**et cliquez sur **SQL Server Management Studio**.  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** , sélectionnez **Integration Services** dans la liste **Type de serveur** , indiquez un nom de serveur dans la zone **Nom du serveur** , puis cliquez sur **Se connecter**.  
  
    > [!IMPORTANT]  
    >  Si vous ne pouvez pas vous connecter à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], il est probable que le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ne soit pas en cours d'exécution. Pour connaître l'état du service, cliquez sur **Démarrer**, pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**. Dans le volet gauche, cliquez sur **Services SQL Server**. Dans le volet droit, recherchez le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Démarrez le service, il n'est pas déjà en cours d'exécution.  
  
     [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] s'ouvre. Par défaut, la fenêtre Explorateur d'objets est ouverte et positionnée dans l'angle inférieur gauche de SQL Server Management Studio. Si l'Explorateur d'objets n'est pas ouvert, dans le menu **Affichage** , cliquez sur **Explorateur d'objets** .  
  
### <a name="to-view-the-packages-that-integration-services-service-manages"></a>Pour visualiser les packages gérés par le service Integration Services  
  
1.  Dans l'Explorateur d'objets, développez le dossier Packages stockés.  
  
2.  Développez les sous-dossiers du dossier Packages stockés afin d'afficher les packages.  

## <a name="import-and-export-packages"></a>Importer et exporter des packages
 
 Les packages peuvent être enregistrés dans la table sysssispackages de la base de données msdb de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans le système de fichiers.  
  
 Le magasin de packages, qui est le stockage logique que le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contrôle et gère, peut inclure la base de données msdb et les dossiers du système de fichiers spécifiés dans le fichier de configuration pour le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Vous pouvez importer et exporter des packages entre les types de stockage suivants :  
  
-   Les dossiers du système de fichiers n'importe où dans le système de fichiers.  
  
-   Les dossiers dans le magasin de packages SSIS. Les deux dossiers par défaut sont appelés « Système de fichiers » et « MSDB ».  
  
-   La base de données msdb [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vous donne la possibilité d'importer et d'exporter des packages, et ce faisant, de modifier le format et l'emplacement de stockage des packages. Les fonctionnalités d’importation et d’exportation vous permettent d’ajouter des packages au système de fichiers, au magasin de packages ou à la base de données msdb, et de copier des packages d’un format de stockage vers un autre. Par exemple, les packages enregistrés dans msdb peuvent être copiés dans le système de fichiers et vice versa.  
  
 Vous pouvez aussi copier un package dans un format différent à l’aide de l’utilitaire d’invite de commandes **dtutil** (dtutil.exe). Pour plus d’informations, consultez [dtutil Utility](../../integration-services/dtutil-utility.md).  
  
 Vous pouvez importer ou exporter un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] depuis ou vers les emplacements suivants :  
  
-   Vous pouvez importer un package stocké dans une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dans le système de fichiers ou dans le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Le package importé est enregistré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans un dossier dans le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
-   Vous pouvez exporter un package stocké dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dans le système de fichiers ou dans le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] vers un format ou un emplacement de stockage différent.  
  
 Toutefois, il existe des restrictions relatives à l'importation et à l'exportation d'un package entre des versions différentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Vous pouvez importer dans une instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]des packages provenant d'une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], mais vous ne pouvez pas exporter de packages vers une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
-   Dans une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], vous ne pouvez pas importer de packages provenant d'une instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], ni exporter de packages vers une telle instance.  
  
 Les procédures ci-dessous montrent comment utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour importer et exporter un package.  
  
### <a name="to-import-a-package-by-using-sql-server-management-studio"></a>Pour importer un package à l'aide de SQL Server Management Studio  
  
1.  Cliquez sur **Démarrer**, pointez sur **Microsoft** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **SQL Server Management Studio**.  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** , définissez les options suivantes :  
  
    -   Dans la zone **Type de serveur** , sélectionnez **Integration Services**.  
  
    -   Dans la zone **Nom du serveur**, indiquez le nom du serveur ou cliquez sur **\<Parcourir**, puis recherchez le serveur à utiliser.  
  
3.  Si l'Explorateur d'objets n'est pas ouvert, dans le menu **Affichage** , cliquez sur **Explorateur d'objets**.  
  
4.  Dans l'Explorateur d'objets, développez le dossier **Packages stockés** .  
  
5.  Développez les sous-dossiers afin de rechercher celui dans lequel vous souhaitez importer un package.  
  
6.  Cliquez avec le bouton droit sur le dossier, puis cliquez sur **Importer un package**. Effectuez ensuite l'une des opérations suivantes :  
  
    -   Pour importer à partir d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez l'option **SQL Server** , puis spécifiez le serveur et le mode d'authentification. Si vous sélectionnez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , indiquez un nom d'utilisateur et un mot de passe.  
  
         Cliquez sur le bouton Parcourir **(...)**, sélectionnez le package à importer, puis cliquez sur **OK**.  
  
    -   Pour importer à partir d'un système de fichiers, sélectionnez l'option **Système de fichiers** .  
  
         Cliquez sur le bouton Parcourir **(...)**, sélectionnez le package à importer, puis cliquez sur **Ouvrir**.  
  
    -   Pour importer à partir du magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] , sélectionnez l'option **Magasin de packages SSIS** et spécifiez le serveur.  
  
         Cliquez sur le bouton Parcourir **(...)**, sélectionnez le package à importer, puis cliquez sur **OK**.  
  
7.  Si vous le souhaitez, mettez à jour le nom du package.  
  
8.  Pour mettre à jour le niveau de protection du package, cliquez sur le bouton Parcourir **(…)** et sélectionnez un niveau de protection différent dans la boîte de dialogue **Niveau de protection du package** . Si l'option **Chiffrer les données sensibles avec un mot de passe** ou **Chiffrer toutes les données avec un mot de passe** est sélectionnée, tapez un mot de passe et confirmez-le.  
  
9. Cliquez sur **OK** pour terminer l'importation.  
  
### <a name="to-export-a-package-by-using-sql-server-management-studio"></a>Pour exporter un package à l'aide de SQL Server Management Studio  
  
1.  Cliquez sur **Démarrer**, pointez sur **Microsoft** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **SQL Server Management Studio**.  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** , définissez les options suivantes :  
  
    -   Dans la zone **Type de serveur** , sélectionnez **Integration Services**.  
  
    -   Dans la zone **Nom du serveur**, indiquez le nom du serveur ou cliquez sur **\<Parcourir**, puis recherchez le serveur à utiliser.  
  
3.  Si l'Explorateur d'objets n'est pas ouvert, dans le menu **Affichage** , cliquez sur **Explorateur d'objets**.  
  
4.  Dans l'Explorateur d'objets, développez le dossier **Packages stockés** .  
  
5.  Développez les sous-dossiers pour localiser le package à exporter.  
  
6.  Cliquez avec le bouton droit sur le package, cliquez sur **Exporter**, puis effectuez l’une des opérations suivantes :  
  
    -   Pour exporter un package vers une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez l'option **SQL Server** , puis indiquez le serveur et sélectionnez le mode d'authentification. Si vous sélectionnez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , indiquez un nom d'utilisateur et un mot de passe.  
  
         Cliquez sur le bouton Parcourir **(…)**, puis développez le dossier **Packages SSIS** pour rechercher le dossier dans lequel enregistrer le package. Si vous le souhaitez, mettez à jour le nom par défaut du package, puis cliquez sur **OK**.  
  
    -   Pour exporter un package vers le système de fichiers, sélectionnez l'option **Système de fichiers** .  
  
         Cliquez sur le bouton Parcourir **(…)** pour rechercher le dossier dans lequel exporter le package, tapez le nom du fichier de package, puis cliquez sur **Enregistrer**.  
  
    -   Pour exporter un package vers le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] , sélectionnez l'option **Magasin de packages SSIS** , puis indiquez le serveur.  
  
         Cliquez sur le bouton Parcourir **(…)**, développez le dossier **Packages SSIS** , puis sélectionnez le dossier dans lequel enregistrer le package. Si vous le souhaitez, entrez un nouveau nom pour le package dans la zone de texte **Nom du package** . [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
7.  Pour mettre à jour le niveau de protection du package, cliquez sur le bouton Parcourir **(…)** et sélectionnez un niveau de protection différent dans la boîte de dialogue **Niveau de protection du package**. Si l'option **Chiffrer les données sensibles avec un mot de passe** ou **Chiffrer toutes les données avec un mot de passe** est sélectionnée, tapez un mot de passe et confirmez-le.  
  
8.  Cliquez sur **OK** pour terminer l'exportation.  

## <a name="import-package-dialog-box-ui-reference"></a>Référence de l'interface utilisateur de la boîte de dialogue Importer un package
  Utilisez la boîte de dialogue **Importer un package** , disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], pour importer un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et pour définir ou modifier son niveau de protection.  
  
### <a name="options"></a>Options  
 **Emplacement du package**  
 Sélectionnez le type d'emplacement de stockage dans lequel importer le package. Les options suivantes sont disponibles :  
  
 **SQL Server**  
  
 **Système de fichiers**  
  
 **Magasin de packages SSIS**  
  
 **Server**  
 Tapez le nom d’un serveur ou sélectionnez-en un dans la liste.  
  
 **Authentification**  
 Sélectionnez l'authentification Windows ou l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est disponible uniquement si l’emplacement de stockage est [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Dans la mesure du possible, utilisez l’authentification Windows.  
  
 **Type d'authentification**  
 Sélectionnez un type d'authentification.  
  
 **User name**  
 Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un nom d’utilisateur.  
  
 **Mot de passe**  
 Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un mot de passe.  
  
 **Chemin d'accès au package**  
 Tapez le chemin du package ou cliquez sur le bouton Parcourir **(…)** pour rechercher le package.  
  
 **Nom du package**  
 Éventuellement, renommez le package. Par défaut, son nom est celui du package à importer.  
  
 **Niveau de protection**  
 Cliquez sur le bouton Parcourir **(…)** et, dans la boîte de dialogue **Niveau de protection du package** , mettez à jour le niveau de protection. Pour plus d’informations, consultez [Boîte de dialogue Niveau de protection du package](../../integration-services/security/access-control-for-sensitive-data-in-packages.md#protection_dialog).  

## <a name="export-package-dialog-box-ui-reference"></a>Référence de l'interface utilisateur de la boîte de dialogue Exporter un package
  Utilisez la boîte de dialogue **Exporter un package** , disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], pour exporter un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vers un autre emplacement et éventuellement modifier son niveau de protection.  
  
### <a name="options"></a>Options  
 **Emplacement du package**  
 Sélectionnez le type de stockage vers lequel exporter le package. Les options suivantes sont disponibles :  
  
 **SQL Server**  
  
 **File System**  
  
 **Magasin de packages SSIS**  
  
 **Server**  
 Tapez le nom d’un serveur ou sélectionnez-en un dans la liste.  
  
 **Authentification**  
 Sélectionnez l'authentification Windows ou l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est disponible uniquement si l’emplacement de stockage est [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Dans la mesure du possible, utilisez l’authentification Windows.  
  
 **Type d'authentification**  
 Sélectionnez un type d'authentification.  
  
 **User name**  
 Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un nom d’utilisateur.  
  
 **Mot de passe**  
 Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez un mot de passe.  
  
 **Chemin d'accès au package**  
 Tapez le chemin du package ou cliquez sur le bouton Parcourir **(…)** pour rechercher le dossier dans lequel stocker le package.  
  
 **Niveau de protection**  
 Cliquez sur le bouton Parcourir **(…)** et mettez à jour le niveau de protection dans la boîte de dialogue **Niveau de protection du package** . Pour plus d’informations, consultez [Boîte de dialogue Niveau de protection du package](../../integration-services/security/access-control-for-sensitive-data-in-packages.md#protection_dialog).  

## <a name="back-up-and-restore-packages"></a>Sauvegarder et restaurer les packages
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Les packages peuvent être enregistrés dans le système de fichiers ou dans msdb, une base de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les packages enregistrés dans msdb peuvent être sauvegardés et restaurés à l’aide des fonctionnalités de sauvegarde et de restauration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour plus d’informations sur la sauvegarde et la restauration de la base de données msdb, cliquez sur l’une des rubriques suivantes :  
  
-   [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
-   [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend l’utilitaire d’invite de commandes **dtutil** (dtutil.exec) qui permet de gérer les packages. Pour plus d’informations, consultez [dtutil Utility](../../integration-services/dtutil-utility.md).  
  
### <a name="configuration-files"></a>Fichiers de configuration  
 Tous les fichiers de configuration inclus dans les packages sont stockés dans le système de fichiers. Ces fichiers ne sont pas sauvegardés en même temps que la base de données msdb. Vous devez donc vérifier que les fichiers de configuration sont sauvegardés régulièrement dans le cadre de votre plan de sécurisation des packages enregistrés dans msdb. Pour inclure les configurations dans la sauvegarde de la base de données msdb, vous devez envisager d’utiliser le type de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la place des configurations basées sur les fichiers.  
  
### <a name="packages-stored-in-the-file-system"></a>Packages stockés dans le système de fichiers  
 La sauvegarde des packages enregistrés dans le système de fichiers doit être incluse dans le plan de sauvegarde du système de fichiers du serveur. Le ficher de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , dont le nom par défaut est MsDtsSrvr.ini.xml, donne la liste des dossiers sur le serveur contrôlé par le service. Vous devez vous assurer que ces dossiers sont sauvegardés. En outre, les packages peuvent être stockés dans d'autres dossiers sur le serveur et vous devez vous assurer que ces dossiers sont inclus dans la sauvegarde.  

## <a name="see-also"></a> Voir aussi  
 [Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md)  
  
  
