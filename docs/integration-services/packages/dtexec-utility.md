---
title: Utilitaire dtexec | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: packages
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7b6867fa-1039-49b3-90fb-85b84678a612
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 19120f136913925721b61aacd59364f787ac0816
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dtexec-utility"></a>Utilitaire dtexec
  L’utilitaire d’invite de commandes **dtexec** permet de configurer et d’exécuter des packages [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . L’utilitaire **dtexec** donne accès à toutes les fonctions de configuration et d’exécution de packages, telles que les paramètres, les connexions, les propriétés, les variables, la journalisation et les indicateurs de progression. L’utilitaire **dtexec** vous permet de charger des packages à partir des sources suivantes : le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , un fichier projet .ispac, une base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le Magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] et le système de fichiers.  
  
> **REMARQUE :** lorsque vous utilisez la version actuelle de l’utilitaire **dtexec** pour exécuter un package créé par une version antérieure de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], l’utilitaire met temporairement à niveau le package au format de package actuel. En revanche, vous ne pouvez pas vous servir de l’utilitaire **dtexec** pour enregistrer le package mis à niveau. Pour plus d’informations sur la mise à niveau permanente d’un package vers la version actuelle, voir [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
 Cette rubrique comprend les sections suivantes :  
  
-   [Serveur et fichier projet Integration Services](#server)  
  
-   [Considérations concernant l'installation sur les ordinateurs 64 bits](#bit)  
  
-   [Remarques concernant les ordinateurs dotés d'installations côte à côte](#side)  
  
-   [Phases d'exécution](#phases)  
  
-   [Codes de sortie retournés](#exit)  
  
-   [Règles de la syntaxe](#syntaxRules)  
  
-   [Utilisation de dtexec à partir de l'invite xp_cmdshell](#cmdshell)  
  
-   [Syntaxe](#syntax)  
  
-   [Paramètres](#parameter)  
  
-   [Notes](#remark)  
  
-   [Exemples](#example)  
  
##  <a name="server"></a> Serveur et fichier projet Integration Services  
 Quand vous utilisez **dtexec** pour exécuter des packages sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], **dtexec** appelle les procédures stockées [catalog.create_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md), [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) et [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) pour créer une exécution, définir les valeurs des paramètres et démarrer l’exécution. Tous les journaux des exécutions peuvent être consultés à partir du serveur dans les vues associées ou via les rapports standard disponibles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations sur les rapports, consultez [Rapports du serveur Integration Services](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  
  
 Voici un exemple d'exécution de package sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
```  
DTExec /ISSERVER "\SSISDB\folderB\Integration Services Project17\Package.dtsx" /SERVER "." /Envreference 2 /Par "$Project::ProjectParameter(Int32)";1 /Par "Parameter(Int32)";21 /Par "CM.sqlcldb2.SSIS_repro.InitialCatalog";ssisdb /Par "$ServerOption::SYNCHRONIZED(Boolean)";True  
```  
  
 Quand vous utilisez **dtexec** pour exécuter un package à partir du fichier projet .ispac, les options associées /Proj[ect] et /Pack[age] permettent de spécifier le chemin du projet et le nom de flux du package. Lorsque vous convertissez un projet en modèle de déploiement de projet en exécutant l' **Assistant Conversion de projet Integration Services** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l'Assistant génère un fichier projet .ispac. Pour plus d’informations, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 Vous pouvez utiliser **dtexec** avec des outils de planification tiers pour planifier les packages qui sont déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
##  <a name="bit"></a> Considérations concernant l'installation sur les ordinateurs 64 bits  
 Sur un ordinateur 64 bits, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] installe une version 64 bits de l’utilitaire **dtexec** (dtexec.exe). Si vous devez exécuter certains packages en mode 32 bits, installez la version 32 bits de l’utilitaire **dtexec** . Pour installer la version 32 bits de l’utilitaire **dtexec** , vous devez sélectionner Outils clients ou [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pendant l’installation.  
  
 Par défaut, un ordinateur 64 bits qui dispose à la fois des versions 64 bits et 32 bits d'une invite de commandes [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] doit pouvoir exécuter la version 32 bits. La version 32 bits s'exécute car le chemin d'accès au répertoire de la version 32 bits apparaît dans la variable d'environnement PATH avant le chemin d'accès au répertoire de la version 64 bits. (En général, le chemin du répertoire 32 bits est *\<lecteur>*:\Program Files(x86)\Microsoft SQL Server\110\DTS\Binn, tandis que le chemin du répertoire 64 bits est *\<lecteur>*:\Program Files\Microsoft SQL Server\110\DTS\Binn.)  
  
> **REMARQUE :** si vous utilisez SQL Server Agent pour exécuter l’utilitaire, il utilise automatiquement la version 64 bits de ce dernier. SQL Server Agent utilise le Registre, et non la variable d'environnement PATH, pour localiser le fichier exécutable correct de l'utilitaire.  
  
 Pour vous assurer que vous exécutez la version 64 bits de l'utilitaire à l'invite de commandes, vous pouvez effectuer l'une des actions suivantes :  
  
-   Ouvrez une fenêtre d’invite de commandes, indiquez le répertoire qui contient la version 64 bits de l’utilitaire (*\<lecteur>*:\Program Files\Microsoft SQL Server\110\DTS\Binn), puis exécutez celui-ci à partir de cet emplacement.  
  
-   À l’invite de commandes, exécutez l’utilitaire en entrant le chemin complet (*\<lecteur>*:\Program Files\Microsoft SQL Server\110\DTS\Binn) de la version 64 bits de l’utilitaire.  
  
-   Modifiez de manière définitive l’ordre des chemins dans la variable d’environnement PATH en plaçant le chemin 64 bits (*\<lecteur>*:\Program Files\Microsoft SQL Server\110\DTS\Binn) avant le chemin 32 bits (*\<lecteur>*:\Program Files(x86)\Microsoft SQL Server\110\DTS\Binn) dans la variable.  
  
##  <a name="side"></a> Remarques concernant les ordinateurs dotés d'installations côte à côte  
 Lousque [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] est installé sur un oudinateur sur lequel [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] ou [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] est installé, plusieurs versions de l'utilitaire **dtexec** sont installées.  
  
 Pour être certain d’exécuter la version appropriée de l’utilitaire, à l’invite de commandes, exécutez l’utilitaire en entrant le chemin complet (*\<lecteur>*:\Program Files\Microsoft SQL Server\\<version\>\DTS\Binn).  
  
##  <a name="phases"></a> Phases d'exécution  
 L'utilitaire s'exécute en quatre phases. Ces phases sont les suivantes :  
  
1.  Phase de source de commande : l'invite de commandes lit la liste des options et arguments spécifiés. Toutes les phases suivantes sont ignorées si une option **/?** ou **/HELP** est rencontrée.  
  
2.  Phase de chargement de package : le package spécifié par l’option **/SQL**, **/FILE**ou **/DTS** est chargé.  
  
3.  Phase de configuration : les options sont traitées dans l'ordre suivant :  
  
    -   Les options qui définissent les indicateurs, les variables et les propriétés de package.  
  
    -   Les options qui vérifient les numéros de version et de construction du package.  
  
    -   Les options qui configurent le comportement de l'utilitaire au moment de l'exécution, par exemple la génération de rapports.  
  
4.  Phase de validation et d’exécution : le package est exécuté ou validé sans exécution si l’option **/VALIDATE** a été spécifiée.  
  
##  <a name="exit"></a> Codes de sortie retournés  
 **Code de sortie retourné par l'utilitaire dtexec**  
  
 Quand un package s’exécute, **dtexec** peut retourner un code de sortie. Le code de sortie est utilisé pour remplir la variable ERRORLEVEL, dont la valeur peut être testée dans des instructions conditionnelles ou la logique dans un fichier de commandes. Le tableau suivant répertorie les valeurs que l’utilitaire **dtexec** peut définir lors de la sortie.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|L'exécution du package a réussi.|  
| 1|Le package a échoué.|  
|3|Le package a été annulé par l'utilisateur.|  
|4|L'utilitaire n'a pas pu localiser le package demandé. Le package est introuvable.|  
|5|L'utilitaire n'a pas pu localiser le package demandé. Le package ne peut pas être chargé.|  
|6|L'utilitaire a rencontré une erreur interne, ou des erreurs syntaxiques ou sémantiques dans la ligne de commande.|  
  
##  <a name="syntaxRules"></a> Règles de la syntaxe  
 **Règles de syntaxe de l'utilitaire**  
  
 Toutes les options doivent commencer par une barre oblique (/) ou par un signe moins (-). Les options qui sont présentées ici commencent par une barre oblique (/), mais le signe moins (-) peut être utilisé à la place.  
  
 Un argument doit être placé entre guillemets s'il contient un espace. Si l'argument n'est pas placé entre guillemets, il ne peut pas contenir d'espaces.  
  
 Des guillemets doublés à l'intérieur des chaînes entre guillemets représentent des guillemets simples placés en mode d'échappement.  
  
 Les options et les arguments ne respectent pas la casse, à l'exception des mots de passe.  
  
##  <a name="cmdshell"></a> Utilisation de dtexec à partir de l'invite xp_cmdshell  
 **Utilisation de dtexec à partir de l'invite xp_cmdshell**  
  
 Vous pouvez exécuter dtexec à partir de l’invite **xp_cmdshell** . L'exemple suivant indique comment exécuter un package nommé UpsertData.dtsx et ignorer le code de retour :  
  
```  
EXEC xp_cmdshell 'dtexec /f "C:\UpsertData.dtsx"'  
```  
  
 L'exemple suivant indique comment exécuter le même package et capturer le code de retour :  
  
```  
DECLARE @returncode int  
EXEC @returncode = xp_cmdshell 'dtexec /f "C:\UpsertData.dtsx"'  
```  
  
> **IMPORTANT** Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’option **xp_cmdshell** est désactivée par défaut sur les nouvelles installations. Elle peut être activée en exécutant la procédure stockée système **sp_configure** . Pour plus d’informations, consultez [xp_cmdshell (option de configuration de serveur)](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
##  <a name="syntax"></a> Syntaxe  
  
```  
dtexec /option [value] [/option [value]]...  
```  
  
##  <a name="parameter"></a> Paramètres  
  
-   **/?** [*option_name*] : (Facultatif). Affiche les options d’invite de commandes ou l’aide pour l’argument *option_name* spécifié, puis ferme l’utilitaire.  
  
     Si vous spécifiez un argument *option_name*, **dtexec** ouvre la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et affiche la rubrique relative à l’utilitaire dtexec.  
  
-   **/Ca[llerInfo]**: (Facultatif). Spécifie des informations supplémentaires pour l'exécution d'un package. Lorsque vous exécutez un package à l'aide de SQL Server Agent, l'agent définit cet argument afin d'indiquer que l'exécution du package est appelée par SQL Server Agent. Ce paramètre est ignoré lorsque l’utilitaire **dtexec** est exécuté à partir de la ligne de commande.  
  
-   **/CheckF[ile]** *filespec*: (Facultatif). Affecte à la propriété **CheckpointFileName** du package le chemin d'accès et le fichier spécifiés dans *filespec*. Ce fichier est utilisé lors du redémarrage du package. Si cette option est spécifiée et si aucune valeur n'est fournie pour le nom de fichier, le **CheckpointFileName** pour le package est défini à une chaîne vide. Si cette option n'est pas spécifiée, les valeurs dans le package sont conservées.  
  
-   **/CheckP[ointing]** *{on\off}* : (Facultatif). Définit une valeur qui détermine si le package utilise des points de contrôle pendant l'exécution du package. La valeur **on** spécifie qu’un package ayant échoué doit être réexécuté. Lorsque le package ayant échoué est relancé, le moteur d'exécution utilise le fichier de point de contrôle pour redémarrer le package à partir du point d'échec.  
  
     La valeur par défaut est « on » si l'option est déclarée sans valeur. L'exécution du package échoue si la valeur « on » est choisie et si le fichier de point de contrôle demeure introuvable. Si cette option n'est pas spécifiée, la valeur définie dans le package est conservée. Pour plus d'informations, consultez [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
     L’option **/CheckPointing on** de dtexec revient à définir la propriété **SaveCheckpoints** du package sur True, et la propriété **CheckpointUsage** sur Always.  
  
-   **/Com[mandFile]** *filespec*: (Facultatif). Spécifie les options de commande qui s’exécutent avec **dtexec**. Le fichier spécifié dans *filespec* est ouvert et les options du fichier sont lues jusqu'à ce que EOF soit rencontré dans le fichier. *filespec* est un fichier texte. L'argument *filespec* spécifie le nom et le chemin du fichier de commandes associé à l'exécution du package.  
  
-   **/Conf[igFile]** *filespec*: (Facultatif). Spécifie un fichier de configuration dont des valeurs sont extraites. Avec cette option, vous pouvez définir une configuration d'exécution qui est différente de celle qui a été spécifiée au moment de la conception du package. Vous pouvez stocker différents paramètres de configuration dans le fichier de configuration XML et charger ensuite les paramètres avant l’exécution du package à l’aide de l’option **/ConfigFile** .  
  
     Vous pouvez utiliser l’option **/ConfigFile** pour charger, au moment de l’exécution, des configurations supplémentaires que vous n’aviez pas spécifiées au moment de la conception. Cependant, vous ne pouvez pas utiliser l’option **/ConfigFile** pour remplacer des valeurs configurées que vous aviez aussi spécifiées au moment de la conception. Pour comprendre le fonctionnement de l'application des configurations de package, consultez [Package Configurations](../../integration-services/packages/package-configurations.md).  
  
-   **/Conn[ection]** *id_or_name;connection_string [[;id_or_name;connection_string]…]*: (Facultatif). Spécifie que le gestionnaire de connexions avec le nom ou le GUID indiqué est fourni dans le package et spécifie une chaîne de connexion valide.  
  
     Cette option exige la spécification des deux paramètres : le nom ou le GUID du gestionnaire de connexions doit être fourni dans l’argument *id_or_name*, et une chaîne de connexion valide doit être spécifiée dans l’argument *connection_string*. Pour plus d’informations, consultez [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
     Au moment de l’exécution, vous pouvez utiliser l’option **/Connection** pour charger des configurations de package à partir d’un emplacement autre que celui que vous avez spécifié au moment de la conception. Les valeurs de ces configurations remplacent alors les valeurs spécifiées à l'origine. Cependant, vous ne pouvez utiliser l’option **/Connection** que pour des configurations qui utilisent un gestionnaire de connexions (par exemple les configurations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ). Pour comprendre comment les configurations de package sont appliquées, consultez [Configurations de package](../../integration-services/packages/package-configurations.md) et [Changements de comportement des fonctionnalités Integration Services dans SQL Server 2016](http://msdn.microsoft.com/library/611d22fa-5ac7-485e-9a40-7131e852f794).  
  
-   **/Cons[oleLog]** [[*displayoptions*];[*list_options*;*src_name_or_guid*]...] : (Facultatif). Affiche les entrées de journal spécifiées sur la console pendant l'exécution du package. Si cette option est omise, aucune entrée de journal ne s'affiche sur la console. Si l'option est spécifiée sans paramètre de restriction d'affichage, toutes les entrées du journal s'affichent. Pour limiter les entrées qui s’affichent dans la console, vous pouvez spécifier les colonnes à inclure à l’aide du paramètre *displayoptions* , puis limiter les types d’entrée de journal à l’aide du paramètre *list_options* .  
  
    > **REMARQUE :**  quand vous exécutez un package sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en utilisant le paramètre **/ISSERVER** , la sortie de la console est limitée et la plupart des options **/Cons[oleLog]** ne sont pas applicables. Tous les journaux des exécutions peuvent être consultés à partir du serveur dans les vues associées ou via les rapports standard disponibles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations sur les rapports, consultez [Rapports du serveur Integration Services](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  
  
     Les valeurs *displayoptions* sont les suivantes :  
  
    -   N (Nom)  
  
    -   C (Ordinateur)  
  
    -   O (Opérateur)  
  
    -   S (Nom de source)  
  
    -   G (GUID de source)  
  
    -   X (GUID d'exécution)  
  
    -   M (Message)  
  
    -   T (Heure de début et de fin)  
  
     Les valeurs de *list_options* sont les suivantes :  
  
    -   *I* - Spécifie la liste d’inclusion. Seuls les noms ou GUID de source qui sont spécifiés sont consignés.  
  
    -   *E* - Spécifie la liste d’exclusion. Les noms ou GUID de source qui sont spécifiés ne sont pas consignés.  
  
    -   Le paramètre *src_name_or_guid* spécifié pour l’inclusion ou l’exclusion est un nom d’événement, un nom de source ou un GUID de source.  
  
     Si vous utilisez plusieurs options **/ConsoleLog** dans la même invite de commandes, celles-ci interagissent de la manière suivante :  
  
    -   L'ordre d'occurrence n'a aucun effet.  
  
    -   Si aucune liste d'inclusion n'est présente sur la ligne de commande, des listes d'exclusion sont appliquées sur tous les types d'entrée de journal.  
  
    -   Si des listes d'inclusion sont présentes sur la ligne de commande, les listes d'exclusion sont appliquées sur l'union de toutes les listes d'inclusion.  
  
     Pour afficher plusieurs exemples de l’option **/ConsoleLog** , consultez la section **Notes** .  
  
--   **/D [ts]** *package_path* (Facultatif). Charge un package à partir du magasin de packages SSIS. Les packages stockés dans le Magasin de packages SSIS sont déployés à l'aide du modèle de déploiement de package hérité. Pour exécuter les packages qui sont déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l’aide du modèle de déploiement de projet, utilisez l’option **/ISServer**. Pour plus d'informations sur les modèles de déploiement de package et de projet, consultez [Deployment of Projects and Packages](https://msdn.microsoft.com/library/hh213290.aspx).  
  
     The *package_path* argument specifies the relative path of the [!INCLUDE[ssIS](../../includes/ssis-md.md)] package, starting at the root of the SSIS Package Store, and includes the name of the [!INCLUDE[ssIS](../../includes/ssis-md.md)] package. If the path or file name specified in the *package_path* argument contains a space, you must put quotation marks around the *package_path* argument.  
  
     The **/DTS** option cannot be used together with the **/File** or **/SQL** option. If multiple options are specified, **dtexec** fails.  
  
-   **/De [crypt]**  *password*: (Facultatif). Définit le mot de passe de déchiffrement qui est utilisé lorsque vous chargez un package avec chiffrement de mot de passe.  
  
-   (Facultatif) Crée les fichiers de vidage du débogage, .mdmp et .tmp, lorsqu'un ou plusieurs événements spécifiés se produisent pendant l'exécution du package. L'argument *error code* spécifie le type de code d'événement (erreur, avertissement ou information) qui déclenchera la création, par le système, des fichiers de vidage du débogage. Pour spécifier plusieurs codes d’événement, séparez chaque argument *error code* par un point-virgule (;). N'incluez pas de guillemets avec l'argument *error code* .  
  
     L'exemple suivant génère les fichiers de vidage du débogage lorsqu'une erreur DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER se produit.  
  
    ```  
    /Dump 0xC020801C  
    ```  
  
     **/Dump** *error code* : par défaut, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stocke les fichiers de vidage du débogage dans le dossier*\<lecteur>*:\Program Files\Microsoft SQL Server\110\Shared\ErrorDumps.  
  
    > **REMARQUE :** les fichiers de vidage du débogage peuvent contenir des informations sensibles. Utilisez une liste de contrôle d'accès (ACL, Access Control List) pour restreindre l'accès aux fichiers ou copiez ces derniers dans un dossier avec accès restreint. Par exemple, nous vous recommandons de supprimer toutes les informations sensibles ou confidentielles avant d'envoyer vos fichiers de débogage aux services de support technique de Microsoft.  
  
     Pour appliquer cette option à tous les packages exécutés par l’utilitaire **dtexec** , ajoutez une valeur REG_SZ **DumpOnCodes** à la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\110\SSIS\Setup\DtsPath. La valeur de données dans **DumpOnCodes** spécifie le ou les codes d’erreur qui déclencheront la création, par le système, des fichiers de vidage du débogage. Plusieurs codes d'erreur doivent être séparés par un point-virgule (;).  
  
     Si vous ajoutez une valeur **DumpOnCodes** à la clé de Registre et utilisez l’option **/Dump** , le système crée des fichiers de vidage du débogage qui sont basés sur les deux paramètres.  
  
     Pour plus d'informations sur les fichiers de vidage du débogage, consultez [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
-   **/DumpOnError**: (Facultatif) Crée les fichiers de vidage du débogage, .mdmp et .tmp, quand une erreur se produit pendant l’exécution du package.  
  
     Par défaut, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stocke les fichiers de vidage du débogage dans le dossier, *\<lecteur>*:\Program Files\Microsoft SQL Server\110\Shared\ErrorDumps.  
  
    > **REMARQUE :** les fichiers de vidage du débogage peuvent contenir des informations sensibles. Utilisez une liste de contrôle d'accès (ACL, Access Control List) pour restreindre l'accès aux fichiers ou copiez ces derniers dans un dossier avec accès restreint. Par exemple, nous vous recommandons de supprimer toutes les informations sensibles ou confidentielles avant d'envoyer vos fichiers de débogage aux services de support technique de Microsoft.  
  
     Pour appliquer cette option à tous les packages exécutés par l’utilitaire **dtexec** , ajoutez une valeur REG_DWORD **DumpOnError** à la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\110\SSIS\Setup\DtsPath. La valeur REG_DWORD **DumpOnError** détermine si l’option **/DumpOnError** doit être utilisée avec l’utilitaire **dtexec** :  
  
    -   Une valeur de données différente de zéro indique que le système crée des fichiers de vidage du débogage quand une erreur se produit, que vous utilisiez ou non l’option **/DumpOnError** avec l’utilitaire **dtexec** .  
  
    -   Une valeur zéro indique que le système ne crée les fichiers de vidage du débogage que si vous utilisez l’option **DumpOnError** avec l’utilitaire **dtexec** .  
  
     Pour plus d'informations sur les fichiers de vidage du débogage, consultez [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
-   **/ Env [Reference]** *environment reference ID*: (Facultatif). Spécifie la référence environnementale (ID) utilisée par l'exécution du package, dans le cas d'un package déployé sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les paramètres configurés pour la liaison aux variables utilisent les valeurs des variables contenues dans l'environnement.  
  
     L’option **/Env[Reference]** s’utilise avec les options **/ISServer** et **/Server** .  
  
     Ce paramètre est utilisé par SQL Server Agent.  
  --   **/F[ile]** *filespec* (Facultatif). Charge un package qui est enregistré dans le système de fichiers. Les packages enregistrés dans le système de fichiers sont déployés à l'aide du modèle de déploiement de package hérité. Pour exécuter les packages qui sont déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l’aide du modèle de déploiement de projet, utilisez l’option **/ISServer** . Pour plus d'informations sur les modèles de déploiement de package et de projet, consultez [Deployment of Projects and Packages](deploy-integration-services-ssis-projects-and-packages.md).  

  L'argument *filespec* spécifie le chemin d'accès et le nom de fichier du package. Vous pouvez spécifier le chemin d'accès en tant que chemin UNC (Universal Naming Convention) ou chemin d'accès local. Si le chemin d'accès ou le nom de fichier spécifié dans l'argument *filespec* contient un espace, vous devez placer l'argument *filespec* entre guillemets.  
  
     L’option **/File** ne peut pas être utilisée avec l’option **/DTS** ou **/SQL** . Si plusieurs options sont spécifiées, **dtexec** échoue.  
  
-   **/H[elp]** [*option_name*] : (Facultatif). Affiche l’aide pour les options ou l’argument *option_name* spécifié, puis ferme l’utilitaire.  
  
     Si vous spécifiez un argument *option_name* , **dtexec** ouvre la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et affiche la rubrique relative à l’utilitaire dtexec.  
  
-   **/ISServer** *packagepath*: (Facultatif). Exécute un package déployé sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . L’argument *PackagePath* spécifie le nom de fichier et le chemin complet du package déployé sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Si le chemin d'accès ou le nom de fichier spécifié dans l'argument *PackagePath* contient un espace, vous devez placer l'argument *PackagePath* entre guillemets.  
  
     Le format du package est le suivant :  
  
    ```  
    \<catalog name>\<folder name>\<project name>\package file name  
    ```  
  
     L’option **/Server** est utilisée avec l’option **/ISSERVER** . Seule l'authentification Windows peut exécuter un package sur le serveur SSIS. L'accès au package s'effectue via l'utilisateur Windows actuel. Si l’option /Server est omise, l’instance locale par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisée.  
  
     L’option **/ISSERVER** ne peut pas être utilisée avec l’option **/DTS**, **/SQL** ou **/File** . Si plusieurs options sont spécifiées, dtexec échoue.  
  
     Ce paramètre est utilisé par SQL Server Agent.  
  
-   **/L[ogger]** *classid_orprogid;configstring*: (Facultatif). Associe un ou plusieurs modules fournisseurs d'informations avec l'exécution d'un package [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Le paramètre *classid_orprogid* spécifie le module fournisseur d’informations et peut être spécifié en tant que GUID de classe. L'argument *configstring* correspond à la chaîne utilisée pour configurer le module fournisseur d'informations.  
  
     La liste suivante affiche les modules fournisseurs d'informations disponibles :  
  
    -   Fichier texte :  
  
        -   ProgID : DTS.LogProviderTextFile.1  
  
        -   ClassID : {59B2C6A5-663F-4C20-8863-C83F9B72E2EB}  
  
    -   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]:  
  
        -   ProgID : DTS.LogProviderSQLProfiler.1  
  
        -   ClassID : {5C0B8D21-E9AA-462E-BA34-30FF5F7A42A1}  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
        -   ProgID : DTS.LogProviderSQLServer.1  
  
        -   ClassID : {6AA833A1-E4B2-4431-831B-DE695049DC61}  
  
    -   Journaux d'événements Windows :  
  
        -   ProgID : DTS.LogProviderEventLog.1  
  
        -   ClassID : {97634F75-1DC7-4F1F-8A4C-DAF0E13AAA22}  
  
    -   Fichier XML :  
  
        -   ProgID : DTS.LogProviderXMLFile.1  
  
        -   ClassID : {AFED6884-619C-484F-9A09-F42D56E1A7EA}  
  
-   **/M[axConcurrent]** *concurrent_executables*: (Facultatif). Spécifie le nombre d'exécutables que le package peut exécuter simultanément. La valeur spécifiée doit être un entier non négatif ou -1. La valeur -1 signifie que [!INCLUDE[ssIS](../../includes/ssis-md.md)] permet l’exécution simultanée d’un nombre maximal d’exécutables égal au nombre total de processeurs présents sur l’ordinateur exécutant le package, plus deux.  
  
-   **/Pack[age]** *PackageName*: (Facultatif). Spécifie le package à exécuter. Ce paramètre est principalement utilisé lorsque vous exécutez le package à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
-   **/P[assword]** *password*: (Facultatif). Permet la récupération d'un package protégé par l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est utilisée avec l’option **/User** . Si l’option **/Password** est omise et que l’option **/User** est utilisée, un mot de passe vide est employé. La valeur *password* peut être mise entre guillemets.  
  
    > **IMPORTANT** [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
-   **/Par[ameter]** [$Package:: | $Project:: | $ServerOption::] *parameter_name* [(data_type)]; *literal_value*: (Facultatif). Spécifie les valeurs de paramètres. Plusieurs options **/Parameter** peuvent être spécifiées. Les types de données sont des TypeCodes CLR définis en tant que chaînes. Pour un paramètre qui n'est pas une chaîne, le type de données est spécifié entre parenthèses, après le nom du paramètre.  
  
     L’option **/Parameter** peut être utilisée uniquement avec l’option **/ISServer** .  
  
     Utilisez les préfixes $Package, $Project et $ServerOption pour indiquer un paramètre de package, un paramètre de projet et un paramètre d'option de serveur, respectivement. Le type de paramètre par défaut est package.  
  
     Voici un exemple qui illustre l'exécution d'un package et la spécification de myvalue pour le paramètre de projet (myparam) et de la valeur entière 12 pour le paramètre de package (anotherparam).  
  
     `Dtexec /isserver “SSISDB\MyFolder\MyProject\MyPackage.dtsx” /server “.” /parameter $Project::myparam;myvalue /parameter anotherparam(int32);12`  
  
     Vous pouvez également définir les propriétés du Gestionnaire de connexions à l'aide des paramètres. Utilisez le préfixe CM pour désigner un paramètre du Gestionnaire de connexions.  
  
     Dans l'exemple suivant, la propriété InitialCatalog du Gestionnaire de connexions SourceServer a la valeur `ssisdb`.  
  
    ```  
    /parameter CM.SourceServer.InitialCatalog;ssisdb  
    ```  
  
     Dans l'exemple suivant, la propriété ServerName du Gestionnaire de connexions SourceServer est définie sur un point (.) pour indiquer le serveur local.  
  
    ```  
    /parameter CM.SourceServer.ServerName;.  
    ```  
  
-   **/Proj[ect]** *ProjectFile*: (Facultatif). Spécifie le projet à partir duquel le package à exécuter doit être récupéré. L'argument *ProjectFile* spécifie le nom du fichier .ispac. Ce paramètre est principalement utilisé lorsque vous exécutez le package à partir de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
-   **/Rem** *comment*: (Facultatif). Inclut des commentaires sur l'invite de commandes ou dans des fichiers de commandes. Cet argument est facultatif. La valeur *comment* est une chaîne à mettre entre guillemets, ou qui ne contient aucun espace. Si aucun argument n'est spécifié, une ligne vide est insérée. Les valeurs*comment* sont rejetées pendant la phase source de la commande.  
  
-   **/Rep[orting]** *level* [*;event_guid_or_name*[*;event_guid_or_name*[...]] : (Facultatif). Spécifie les types de message à produire. Les options de rapport disponibles pour *level* sont les suivantes :  
  
     **N** : pas de rapport.  
  
     **E** : les erreurs sont signalées.  
  
     **W** : les avertissements sont inclus dans le rapport.  
  
     **I** : les messages d'information sont inclus dans le rapport.  
  
     **C** : les événements personnalisés sont inclus dans le rapport.  
  
     **D** : les événements de la tâche de flux de données sont inclus dans le rapport.  
  
     **P** : l'avancement est inclus dans le rapport.  
  
     **V** : rapport exhaustif.  
  
     Les arguments de V et N sont mutuellement exclusifs à tous les autres arguments ; ils doivent être spécifiés seuls. Si l’option **/Reporting** n’est pas spécifiée, le niveau par défaut est **E** (erreurs), **W** (avertissements) et **P** (progression).  
  
     Tous les événements sont précédés d'un horodateur dans le format « AA/MM/JJ HH:MM:SS », ainsi que d'un GUID ou d'un nom convivial, si disponible.  
  
     Le paramètre facultatif *event_guid_or_name* correspond à une liste d’exceptions pour les modules fournisseurs d’informations. L'exception spécifie les événements qui ne sont pas journalisés et qui sinon auraient pu l'être.  
  
     Vous n'avez pas besoin d'exclure un événement si celui-ci n'est pas habituellement journalisé par défaut.  
  
-   **/Res[tart]** {*deny | force | ifPossible*} : (Facultatif). Spécifie une nouvelle valeur pour la propriété <xref:Microsoft.SqlServer.Dts.Runtime.Package.CheckpointUsage%2A> du package. La signification des paramètres est la suivante :  
  
     *Deny* : attribue à <xref:Microsoft.SqlServer.Dts.Runtime.Package.CheckpointUsage%2A> la valeur <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DTSCheckpointUsage.DTSCU_NEVER>.  
  
     *Force* : attribue à <xref:Microsoft.SqlServer.Dts.Runtime.Package.CheckpointUsage%2A> la valeur <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DTSCheckpointUsage.DTSCU_ALWAYS>.  
  
     *ifPossible* : attribue à <xref:Microsoft.SqlServer.Dts.Runtime.Package.CheckpointUsage%2A> la valeur <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.DTSCheckpointUsage.DTSCU_IFEXISTS>.  
  
     La valeur par défaut **force** est utilisée si aucune valeur n’est spécifiée.  
  
-   **/Set** [$Sensitive::]*propertyPath;value*: (Facultatif). Remplace la configuration d'un paramètre, d'une variable, d'une propriété, d'un conteneur, d'un module fournisseur d'informations, de l'énumérateur Foreach ou de la connexion dans un package. Quand cette option est utilisée, **/Set** modifie l’argument *propertyPath* et lui attribue la valeur spécifiée. Vous pouvez spécifier plusieurs options **/Set** .  
  
     Outre l’utilisation de l’option **/Set** avec l’option **/F[ile]** , vous pouvez aussi utiliser l’option **/Set** avec l’option **/ISServer** ou **/Project** . Quand vous utilisez l’option **/Set** avec l’option **/Project**, **/Set** définit les valeurs de paramètre. Quand vous utilisez l’option **/Set** avec l’option **/ISServer**, **/Set** définit les substitutions de propriété. En outre, quand vous utilisez l’option **/Set** avec l’option **/ISServer**, vous pouvez utiliser le préfixe facultatif $Sensitive pour indiquer que la propriété doit être traitée comme sensible sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous pouvez déterminer la valeur de *propertyPath* en exécutant l'Assistant Configuration de package. Les chemins pour les éléments que vous sélectionnez sont affichés dans la page **Fin de l'Assistant** ; ils peuvent être copiés et collés. Si vous utilisez l'Assistant uniquement dans ce but, vous pouvez le fermer après la copie des chemins.  
  
     Voici un exemple qui illustre l'exécution d'un package enregistré dans le système de fichiers et la spécification d'une nouvelle valeur pour une variable :  
  
     `dtexec /f mypackage.dtsx /set \package.variables[myvariable].Value;myvalue`  
  
     L'exemple suivant permet d'exécuter un package à partir du fichier projet .ispac et de définir les paramètres de package et de projet.  
  
     `/Project c:\project.ispac /Package Package1.dtsx /SET \Package.Variables[$Package::Parameter];1 /SET \Package.Variables[$Project::Parameter];1`  
  
     Vous pouvez utiliser l’option **/Set** pour modifier l’emplacement de chargement des configurations de package. Cependant, vous ne pouvez pas utiliser l’option **/Set** pour remplacer une valeur spécifiée par une configuration au moment de la conception. Pour comprendre comment les configurations de package sont appliquées, consultez [Configurations de package](../../integration-services/packages/package-configurations.md) et [Changements de comportement des fonctionnalités Integration Services dans SQL Server 2016](http://msdn.microsoft.com/library/611d22fa-5ac7-485e-9a40-7131e852f794).  
  
-   **/Ser[ver]** *server*: (Facultatif). Quand l’option **/SQL** ou **/DTS** est spécifiée, cette option spécifie le nom du serveur à partir duquel le package est récupéré. Si vous omettez l’option **/Server** et que l’option **/SQL** ou **/DTS** est spécifiée, l’exécution du package est tentée sur le serveur local. La valeur de *server_instance* peut être mise entre guillemets.  
  
     L’option **Ser[ver]** est obligatoire quand l’option **/ISServer** est spécifiée.  
  
--   **/SQ[L]** *package_path* : charge un package qui est stocké dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dans la base de données **msdb**. Les packages stockés dans la base de données **msdb** sont déployés à l'aide du modèle de déploiement de package. Pour exécuter les packages qui sont déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l’aide du modèle de déploiement de projet, utilisez l’option **/ISServer** . Pour plus d'informations sur les modèles de déploiement de package et de projet, consultez [Deployment of Projects and Packages](https://msdn.microsoft.com/library/hh213290.aspx).   
  
     The *package_path* argument specifies the name of the package to retrieve. If folders are included in the path, they are terminated with backslashes ("\\"). The *package_path* value can be quoted. If the path or file name specified in the *package_path* argument contains a space, you must put quotation marks around the *package_path* argument.  
  
     You can use the **/User**, **/Password**, and **/Server** options together with the **/SQL** option.  
  
     If you omit the **/User** option, Windows Authentication is used to access the package. If you use the **/User** option, the **/User** login name specified is associated with [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentication.  
  
     The **/Password** option is used only together with the **/User** option. If you use the **/Password** option, the package is accessed with the user name and password information provided. If you omit the **/Password** option, a blank password is used.  
  
    > **IMPORTANT!!** [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
     If the **/Server** option is omitted, the default local instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] is assumed.  
  
     The **/SQL** option cannot be used together with the **/DTS** or **/File** option. If multiple options are specified, **dtexec** fails.  
  
-   **/Su[m]**: (Facultatif). Affiche un compteur incrémentiel qui contient le nombre de lignes qui seront reçues par le prochain composant.  
  
-   **/U[ser]** *user_name*: (Facultatif). Permet la récupération d'un package protégé par l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est utilisée uniquement quand l’option **/SQL** est spécifiée. La valeur de *user_name* peut être mise entre guillemets.  
  
    > **IMPORTANT**  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
-   **/Va[lidate]**: (Facultatif). Arrête l'exécution du package après la phase de validation, sans exécuter le package. Pendant la validation, l’utilisation de l’option **/WarnAsError** amène **dtexec** à traiter un avertissement comme une erreur. Par conséquent, le package échoue si un avertissement se produit pendant la validation.  
  
-   **/VerifyB[uild]** *major*[*;minor*[*;build*]] : (Facultatif). Vérifie le numéro de build d'un package par rapport aux numéros de build qui ont été spécifiés pendant la phase de vérification dans les arguments *major*, *minor*et *build* . En cas de non-concordance, le package ne s'exécute pas.  
  
     Les valeurs sont des entiers longs. L'argument peut prendre trois formes, avec une valeur toujours obligatoire pour *major* :  
  
    -   *major*  
  
    -   *major*;*minor*  
  
    -   *major*; *minor*; *build*  
  
-   **/VerifyP[ackageID]** *packageID*: (Facultatif). Vérifie le GUID du package à exécuter en le comparant à la valeur spécifiée dans l’argument *package_id* .  
  
-   **/VerifyS[igned]**: (Facultatif). Demande à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de vérifier la signature numérique du package. Si le package n'est pas signé ou si la signature n'est pas valide, le package échoue. Pour plus d’informations, consultez [Identifier la source de packages à l’aide de signatures numériques](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
    > **IMPORTANT** Lorsqu'il est configuré pour vérifier la signature du package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vérifie seulement si la signature numérique est présente, si elle est valide et si elle provient d'une source approuvée. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ne vérifie pas si le package a été modifié.  
  
    > **REMARQUE :** la valeur de Registre **BlockedSignatureStates** facultative peut spécifier un paramètre qui est plus restrictif que l’option de signature numérique définie dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou à la ligne de commande **dtexec** . Dans cette situation, le paramètre du Registre plus restrictif a priorité sur les autres paramètres.  
  
-   **/VerifyV[ersionID]** *versionID*: (Facultatif). Vérifie le GUID de version d’un package à exécuter en le comparant à la valeur spécifiée dans l’argument *version_id* pendant la phase de validation du package.  
  
-   **/VLog** *[Filespec]*: (Facultatif). Écrit tous les événements de package Integration Services dans les modules fournisseurs d'informations activés lors de la conception du package. Pour qu'Integration Services active un module fournisseur d'informations pour les fichiers texte et écrive les événements de journal dans un fichier texte spécifié, incluez un chemin d'accès et un nom de fichier en tant que paramètre *Filespec* .  
  
     Si vous n'incluez pas le paramètre *Filespec* , Integration Services n'activera pas de module fournisseur d'informations pour les fichiers texte. Il écrira uniquement les événements de journal dans les modules fournisseurs d'informations activés lors de la conception du package.  
  
-   **/W[arnAsError]**: (Facultatif). Le package considère un avertissement comme une erreur ; par conséquent, le package échoue si un avertissement se produit pendant la validation. Si aucun avertissement ne se produit pendant la validation et que l’option **/Validate** n’est pas spécifiée, le package est exécuté.  
  
-   **/X86**: (Facultatif). Provoque l'exécution du package par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode 32 bits sur un ordinateur 64 bits. Cette option est définie par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque les conditions suivantes sont vraies :  
  
    -   Le type d'étape de travail correspond à un **Package SQL Server Integration Services**.  
  
    -   L'option **Utiliser le runtime 32 bits** sous l'onglet **Options d'exécution** de la boîte de dialogue **Nouvelle étape du travail** est sélectionnée.  
  
     Vous pouvez également définir cette option pour une étape de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant des procédures stockées ou des objets SMO (SQL Server Management Objects) pour créer le travail par programmation.  
  
     Cette option est utilisée uniquement par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est ignorée si vous exécutez l’utilitaire **dtexec** à l’invite de commandes.  
  
##  <a name="remark"></a> Notes  
 L'ordre dans lequel vous spécifiez des options de commande peut influencer le mode d'exécution du package :  
  
-   Les options sont traitées dans l'ordre de leur occurrence sur la ligne de commande. Les fichiers de commandes sont lus dans leur ordre d'occurrence sur la ligne de commande. Les commandes du fichier de commandes sont également traitées dans leur ordre d'apparition.  
  
-   Si la même option, le même paramètre ou la même variable apparaît plusieurs fois dans la même instruction de ligne de commande, la dernière instance de l'option est prioritaire.  
  
-   Les options **/Set** et **/ConfigFile** sont traitées dans l’ordre dans lequel elles sont rencontrées.  
  
##  <a name="example"></a> Exemples  
 Les exemples suivants montrent comment utiliser l’utilitaire d’invite de commandes **dtexec** pour configurer et exécuter des packages [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 **Exécution des packages**  
  
 Pour exécuter un package [!INCLUDE[ssIS](../../includes/ssis-md.md)] enregistré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de l'authentification Windows, utilisez le code suivant :  
  
```  
dtexec /sq pkgOne /ser productionServer  
```  
  
 Pour exécuter un package [!INCLUDE[ssIS](../../includes/ssis-md.md)] enregistré dans le dossier File System, dans le magasin de packages SSIS, utilisez le code suivant :  
  
```  
dtexec /dts "\File System\MyPackage"  
```  
  
 Pour valider un package qui utilise l'authentification Windows et est enregistré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans exécuter le package, utilisez le code suivant :  
  
```  
dtexec /sq pkgOne /ser productionServer /va  
```  
  
 Pour exécuter un [!INCLUDE[ssIS](../../includes/ssis-md.md)] qui est enregistré dans le système de fichiers, utilisez le code suivant :  
  
```  
dtexec /f "c:\pkgOne.dtsx"   
```  
  
 Pour exécuter un package [!INCLUDE[ssIS](../../includes/ssis-md.md)] qui est enregistré dans le système de fichiers et pour spécifier des options de journalisation, utilisez le code suivant :  
  
```  
dtexec /f "c:\pkgOne.dtsx" /l "DTS.LogProviderTextFile;c:\log.txt"  
```  
  
 Pour exécuter un package qui emploie l'authentification Windows et est enregistré dans l'instance locale par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis vérifier la version avant son exécution, utilisez le code suivant :  
  
```  
dtexec /sq pkgOne /verifyv {c200e360-38c5-11c5-11ce-ae62-08002b2b79ef}  
```  
  
 Pour exécuter un package [!INCLUDE[ssIS](../../includes/ssis-md.md)] qui est enregistré dans le système de fichiers et configuré de façon externe, utilisez le code suivant :  
  
```  
dtexec /f "c:\pkgOne.dtsx" /conf "c:\pkgOneConfig.cfg"  
```  
  
> **REMARQUE :** les arguments *package_path* ou *filespec* des options /SQL, /DTS ou /FILE doivent être mis entre guillemets si le chemin ou le nom du fichier contient un espace. Si l'argument n'est pas placé entre guillemets, il ne peut pas contenir d'espaces.  
  
 **Option du journal**  
  
 S’il existe trois types d’entrée de journal A, B et C, l’option **ConsoleLog** suivante sans paramètre affiche les trois types de journaux avec tous les champs :  
  
```  
/CONSOLELOG  
```  
  
 L'option suivante affiche tous les types de journaux, mais avec uniquement les colonnes Nom et Message :  
  
```  
/CONSOLELOG NM  
```  
  
 L'option suivante affiche toutes les colonnes, mais seulement pour le type A d'entrée de journal :  
  
```  
/CONSOLELOG I;LogEntryTypeA  
```  
  
 L'option suivante affiche uniquement le type A d'entrée de journal, avec les colonnes Nom et Message :  
  
```  
/CONSOLELOG NM;I;LogEntryTypeA  
```  
  
 L'option suivante affiche toutes les entrées de journal pour les types A et B d'entrée de journal :  
  
```  
/CONSOLELOG I;LogEntryTypeA;LogEntryTypeB  
```  
  
 Vous pouvez obtenir les mêmes résultats en utilisant plusieurs options **ConsoleLog** :  
  
```  
/CONSOLELOG I;LogEntryTypeA /CONSOLELOG I;LogEntryTypeB  
```  
  
 Si l’option **ConsoleLog** est utilisée sans paramètre, tous les champs sont affichés. Si vous ajoutez un paramètre *list_options* , l’expression suivante affiche uniquement le type A d’entrée de journal, avec tous les champs :  
  
```  
/CONSOLELOG NM;I;LogEntryTypeA /CONSOLELOG  
```  
  
 L'expression suivante affiche toutes les entrées de journal, à l'exception du type A d'entrée de journal ; elle affiche les types B et C d'entrée de journal :  
  
```  
/CONSOLELOG E;LogEntryTypeA  
```  
  
 L’exemple suivant permet d’obtenir les mêmes résultats en utilisant plusieurs options **ConsoleLog** et une seule exclusion :  
  
```  
/CONSOLELOG E;LogEntryTypeA /CONSOLELOG  
/CONSOLELOG E;LogEntryTypeA /CONSOLELOG E;LogEntryTypeA  
/CONSOLELOG E;LogEntryTypeA;LogEntryTypeA  
```  
  
 L'exemple suivant n'affiche aucun message de journalisation car lorsqu'un type de fichier journal figure à la fois dans les listes d'inclusion et d'exclusion, il est exclu.  
  
```  
/CONSOLELOG E;LogEntryTypeA /CONSOLELOG I;LogEntryTypeA  
```  
  
 **Option SET**  
  
 L’exemple suivant montre comment utiliser l’option **/SET** , qui permet de modifier la valeur de toute variable ou propriété du package quand vous démarrez le package à partir de la ligne de commande.  
  
```  
/SET \package\DataFlowTask.Variables[User::MyVariable].Value;newValue  
```  
  
 **Option Project**  
  
 L’exemple suivant montre comment utiliser les options **/Project** et **/Package** .  
  
```  
/Project c:\project.ispac /Package Package1.dtsx  
```  
  
 L’exemple suivant montre comment utiliser les options **/Project** et **/Package** et comment définir les paramètres de package et de projet.  
  
```  
/Project c:\project.ispac /Package Package1.dtsx /SET \Package.Variables[$Package::Parameter];1 /SET \Package.Variables[$Project::Parameter];1  
  
```  
  
 **Option ISServer**  
  
 L’exemple suivant montre comment utiliser l’option **/ISServer** .  
  
```  
dtexec /isserver "\SSISDB\MyFolder\MyProject\MyPackage.dtsx" /server "."  
```  
  
 L’exemple suivant montre comment utiliser l’option **/ISServer** et comment définir les paramètres relatifs au projet et au gestionnaire de connexions.  
  
```  
/Server localhost /ISServer “\SSISDB\MyFolder\Integration Services Project1\Package.dtsx” /Par "$Project::ProjectParameter(Int32)";1 /Par "CM.SourceServer.InitialCatalog";SourceDB  
  
```  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [Exit Codes, DTEXEC, and SSIS Catalog](http://www.mattmasson.com/2012/02/exit-codes-dtexec-and-ssis-catalog/), sur www.mattmasson.com.  
  
  
