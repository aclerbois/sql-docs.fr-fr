---
title: Déploiement de packages hérités (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: packages
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.packageconfigurationorganizer.f1
- sql13.dts.configwizard.finishdtsconfiguration.f1
- sql13.dts.configwizard.selectobjects.f1
- sql13.dts.configwizard.selecconfigtype.f1
- sql13.dts.configwizard.welcome.f1
- sql13.dts.deploymentwizard.welcome.f1
- sql13.dts.deploymentwizard.confirminstallation.f1
- sql13.dts.deploymentwizard.deploydtspackages.f1
- sql13.dts.deploymentwizard.finish.f1
- sql13.dts.deploymentwizard.configurepackages.f1
- sql13.dts.deploymentwizard.selectinstfolder.f1
- sql13.dts.deploymentwizard.packagevalidation.f1
- sql13.dts.deploymentwizard.specifytargetsqlserver.f1
helpviewer_keywords:
- Integration Services packages, deploying
- deploying packages [Integration Services]
- SQL Server Integration Services packages, deploying
- deploying packages [Integration Services], about deploying packages
- packages [Integration Services], deploying
- SSIS packages, deploying
ms.assetid: 0f5fc7be-e37e-4ecd-ba99-697c8ae3436f
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 778901dae6c0504d84eb7cb93667d0ac4024e9ac
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="legacy-package-deployment-ssis"></a>Déploiement de packages hérités (SSIS)
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend des outils et des assistants qui facilitent le déploiement de packages de l’ordinateur de développement au serveur de production ou à d’autres ordinateurs.  
  
 Ce processus de déploiement de package se déroule en quatre étapes :  
  
1.  La première étape est facultative et implique la création de configurations de package qui mettent à jour les propriétés des éléments de package au moment de l'exécution. Les configurations sont automatiquement incluses lorsque vous déployez les packages.  
  
2.  La deuxième étape consiste à générer le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] afin de créer un utilitaire de déploiement de package. L'utilitaire de déploiement du projet contient les packages à déployer.  
  
3.  La troisième étape consiste à copier, sur l'ordinateur cible, le dossier de déploiement créé lors de la génération du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
4.  La quatrième étape consiste à exécuter l’Assistant Installation de package sur l’ordinateur cible pour installer les packages sur le système de fichiers ou une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="package-configurations"></a>Configurations du package
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit des configurations de package avec lesquelles vous pouvez mettre à jour les valeurs des propriétés au moment de l'exécution.  
  
> **REMARQUE :** des configurations sont disponibles pour le modèle de déploiement de package. Les paramètres sont utilisés à la place des configurations pour le modèle de déploiement de projet. Le modèle de déploiement de projet vous permet de déployer des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d'informations sur les modèles de déploiement, consultez [Deployment of Projects and Packages](https://msdn.microsoft.com/library/hh213290.aspx).   
  
 Une configuration est une paire propriété/valeur que vous ajoutez à un package terminé. En règle générale, vous devez créer un package, définir des propriétés sur les objets du package lors du développement de ce dernier, puis ajouter la configuration au package. Lors de son exécution, le package extrait les nouvelles valeurs de la propriété à partir de la configuration. Par exemple, lorsque vous utilisez une configuration, vous pouvez modifier la chaîne de connexion d'un gestionnaire de connexions ou mettre à jour la valeur d'une variable.  
  
 Les configurations de package offrent les avantages suivants :  
  
-   Les configurations facilitent le déplacement des packages d'un environnement de développement vers un environnement de production. Par exemple, une configuration peut mettre à jour le chemin d'accès d'un fichier source ou bien modifier le nom d'une base de données ou d'un serveur.  
  
-   Les configurations sont utiles lorsque vous déployez des packages sur de nombreux serveurs différents. Par exemple, une variable dans la configuration de chaque package déployé peut contenir une valeur d'espace disque différente, et si l'espace disque disponible ne correspond pas à cette valeur, le package ne s'exécute pas.  
  
-   Les configurations rendent ces packages plus souples. Par exemple, une configuration peut mettre à jour la valeur d'une variable utilisée dans une expression de propriété.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge différentes méthodes de stockage des configurations de package, telles que les fichiers XML, les tables d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les variables d'environnement et de package.  
  
 Chaque configuration est une paire propriété/valeur. Le fichier de configuration XML et les types de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent inclure plusieurs configurations.  
  
 Les configurations sont incluses lorsque vous créez un utilitaire de déploiement de package pour l'installation des packages. Lorsque vous installez les packages, les configurations peuvent être mises à jour lors d'une étape de l'installation du package.  
  
### <a name="understanding-how-package-configurations-are-applied-at-run-time"></a>Fonctionnement de l'application des configurations de package au moment de l'exécution  
 Quand vous utilisez l’utilitaire d’invite de commandes **dtexec** (dtexec.exe) pour exécuter un package déployé, l’utilitaire applique les configurations de package à deux reprises. L'utilitaire applique les configurations avant et après avoir appliqué les options que vous avez spécifiées sur la ligne de commande.  
  
 Lorsque l'utilitaire charge et exécute le package, des événements se produisent dans l'ordre suivant :  
  
1.  L’utilitaire **dtexec** charge le package.  
  
2.  L'utilitaire applique les configurations spécifiées dans le package au moment de la conception et dans l'ordre spécifié dans le package. (La seule exception concerne les configurations des variables de package parent. L'utilitaire applique ces configurations une seule fois et ultérieurement au cours du processus).  
  
3.  L'utilitaire applique ensuite les options que vous avez spécifiées sur la ligne de commande.  
  
4.  L'utilitaire recharge ensuite les configurations spécifiées dans le package au moment du design et dans l'ordre spécifié dans le package. (Une fois encore, la seule exception à cette règle concerne les configurations des variables de package parent). L'utilitaire se base sur les options de ligne de commande spécifiées pour recharger les configurations. Par conséquent, des valeurs différentes peuvent être rechargées à partir d'un emplacement distinct.  
  
5.  L'utilitaire applique les configurations des variables de package parent.  
  
6.  L'utilitaire exécute le package.  
  
 La façon dont l’utilitaire **dtexec** applique les configurations affecte les options de ligne de commande suivantes :  
  
-   Vous pouvez utiliser l’option **/Connection** ou **/Set** à l’exécution pour charger les configurations de package à partir d’un emplacement autre que celui que vous avez spécifié au moment de la conception.  
  
-   Vous pouvez utiliser l’option **/ConfigFile** pour charger des configurations supplémentaires que vous n’aviez pas spécifiées au moment de la conception.  
  
 Toutefois, ces options de ligne de commande sont soumises à quelques restrictions :  
  
-   Vous ne pouvez pas utiliser l’option **/Set** ou **/Connection** pour remplacer des valeurs uniques qui sont également définies par une configuration.  
  
-   Vous ne pouvez pas utiliser l’option **/ConfigFile** pour charger des configurations qui remplacent celles que vous aviez spécifiées au moment de la conception.  
  
 Pour plus d’informations sur ces options, ainsi que sur les différences de comportement de ces options entre [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] et les versions antérieures, consultez [Changements de comportement des fonctionnalités Integration Services dans SQL Server 2016](http://msdn.microsoft.com/library/611d22fa-5ac7-485e-9a40-7131e852f794).  
  
### <a name="package-configuration-types"></a>Types de configuration de package  
 Le tableau suivant décrit les types de configuration de package.  
  
|Type|Description|  
|----------|-----------------|  
|Fichier de configuration XML|Un fichier XML contient les configurations. Le fichier XML peut inclure plusieurs configurations.|  
|Variable d'environnement|Une variable d'environnement contient la configuration.|  
|Entrée de Registre|Une entrée de Registre contient la configuration.|  
|Variable de package parent|Une variable dans le package contient la configuration. Ce type de configuration est généralement utilisé pour mettre à jour les propriétés dans les packages enfants.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table|Une table d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient la configuration. La table peut inclure plusieurs configurations.|  
  
#### <a name="xml-configuration-files"></a>Fichiers de configuration XML  
 Si vous sélectionnez le type de configuration **Fichier de configuration XML** , vous pouvez créer un nouveau fichier de configuration, réutiliser un fichier existant et ajouter de nouvelles configurations, ou réutiliser un fichier existant en remplaçant son contenu.  
  
 Un fichier de configuration XML est composé de deux sections :  
  
-   Un en-tête qui contient des informations sur le fichier de configuration. Cet élément comprend des attributs tels que la date de création du fichier et le nom de la personne qui a généré le fichier.  
  
-   Les éléments de configuration qui contiennent des informations sur chaque configuration. Cet élément comprend des attributs tels que le chemin d'accès de la propriété et la valeur configurée de la propriété.  
  
 Le code XML suivant présente la syntaxe d'un fichier de configuration XML. Cet exemple montre une configuration de la propriété Valeur d’une variable de type entier nommée `MyVar`.  
  
```xml
\<?xml version="1.0"?>  
<DTSConfiguration>  
   <DTSConfigurationHeading>  
      <DTSConfigurationFileInfo  
          GeneratedBy="DomainName\UserName"  
          GeneratedFromPackageName="Package"  
          GeneratedFromPackageID="{2AF06766-817A-4E28-9878-0DE37A150648}"  
          GeneratedDate="2/01/2005 5:58:09 PM"/>  
   </DTSConfigurationHeading>  
   <Configuration ConfiguredType="Property" Path="\Package.Variables[User::MyVar].Value" ValueType="Int32">  
      <ConfiguredValue>0</ConfiguredValue>  
   </Configuration>  
</DTSConfiguration>  
  
```  
  
#### <a name="registry-entry"></a>Entrée de Registre  
 Si vous souhaitez utiliser une entrée de Registre pour stocker la configuration, vous pouvez utiliser une clé existante ou créer une nouvelle clé dans HKEY_CURRENT_USER. La clé de Registre que vous utilisez doit contenir une valeur nommée **Value**. Cette valeur peut être de type DWORD ou une chaîne.  
  
 Si vous sélectionnez le type de configuration **Entrée de Registre** , tapez le nom de la clé de Registre dans la zone d'entrée de Registre. Le format est \<clé de Registre>. Si vous souhaitez utiliser une clé de Registre qui n’est pas à la racine de HKEY_CURRENT_USER, utilisez le format \<Clé de Registre\clé de Registre\\...> pour identifier la clé. Par exemple, pour utiliser la clé MyPackage située dans SSISPackages, tapez **SSISPackages\MyPackage**.  
  
#### <a name="sql-server"></a>SQL Server  
 Si vous sélectionnez le type de configuration **SQL Server** , vous spécifiez la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous voulez stocker les configurations. Vous pouvez enregistrer les configurations dans une table existante ou créer une nouvelle table dans la base de données spécifiée.  
  
 L'instruction SQL suivante montre l'instruction CREATE TABLE par défaut fournie par l'Assistant Configuration de package.  
  
```sql
CREATE TABLE [dbo].[SSIS Configurations]  
(  
ConfigurationFilter NVARCHAR(255) NOT NULL,  
ConfiguredValue NVARCHAR(255) NULL,  
PackagePath NVARCHAR(255) NOT NULL,  
ConfiguredValueType NVARCHAR(20) NOT NULL  
)  
  
```  
  
 Le nom que vous donnez à la configuration est la valeur stockée dans la colonne **ConfigurationFilter** .  
  
### <a name="direct-and-indirect-configurations"></a>Configurations directes et indirectes  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit des configurations directes et indirectes. Si vous spécifiez directement les configurations, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un lien direct entre l'élément de configuration et la propriété de l'objet package. Les configurations directes sont un meilleur choix lorsque l'emplacement de la source ne change pas. Par exemple, si vous êtes sûr que tous les déploiements dans le package utilisent le même chemin d'accès de fichier, vous pouvez spécifier un fichier de configuration XML.  
  
 Les configurations indirectes utilisent des variables d'environnement. Au lieu de spécifier directement le paramètre de configuration, la configuration pointe vers une variable d'environnement, qui contient à son tour la valeur de configuration. L'utilisation de configurations indirectes est un meilleur choix lorsque l'emplacement de la configuration peut changer pour chaque déploiement d'un package.  

## <a name="create-package-configurations"></a>Créer des configurations de package
  Vous pouvez créer des configurations de package à l’aide de la boîte de dialogue **Bibliothèque des configurations du package** et de l’Assistant Configuration de package. Pour accéder à ces outils, cliquez sur **Configurations du package** dans le menu **SSIS** de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
  
 **REMARQUES :**
>Vous pouvez également accéder à la **Bibliothèque des configurations du package** , en cliquant sur le bouton de sélection (points de suspension) en regard de la propriété **Configuration** . La propriété Configuration apparaît dans la fenêtre des propriétés du package.  
  
>Des configurations sont disponibles pour le modèle de déploiement de package. Les paramètres sont utilisés à la place des configurations pour le modèle de déploiement de projet. Le modèle de déploiement de projet vous permet de déployer des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d'informations sur les modèles de déploiement, consultez [Deployment of Projects and Packages](https://msdn.microsoft.com/library/hh213290.aspx).    
  
>Dans la boîte de dialogue **Bibliothèque des configurations du package** , vous pouvez permettre aux packages d'utiliser des configurations, ajouter et supprimer des configurations et définir l'ordre souhaité dans lequel sont chargées les configurations. 
 
>Lorsque les configurations du package sont chargées dans l'ordre souhaité, le chargement est effectué du haut vers le bas de la liste affichée dans la boîte de dialogue **Bibliothèques des configurations du package** . Toutefois, au moment de l'exécution, il se peut que les configurations de package ne soient pas chargées dans l'ordre souhaité. Les configurations de package parent sont notamment chargées après les configurations d'autres types.  
  
>Si plusieurs configurations définissent la même propriété d'objet, la dernière valeur chargée est utilisée à l'exécution.  
  
 À partir de la boîte de dialogue **Bibliothèque des configurations du package** , vous pouvez exécuter l'Assistant Configuration de package, qui vous guide à travers les étapes de création d'une configuration. Pour exécuter l'Assistant Configuration de package, ajoutez une nouvelle configuration dans la boîte de dialogue **Bibliothèque des configurations du package** ou modifiez une configuration existante. Sur les pages de l'Assistant, vous choisissez un type de configuration, vous choisissez si vous souhaitez accéder directement à la configuration ou utiliser des variables d'environnement, et vous sélectionnez les propriétés à enregistrer dans la configuration.  
  
 L'exemple suivant illustre les propriétés cibles d'une variable et d'un package telles qu'elles apparaissent dans la page Fin de l'Assistant de l'Assistant Configuration de package :  
  
 \Package.Variables[User::TodaysDate].Properties[RaiseChangedEvent]  
  
 \Package.Properties[MaximumErrorCount]  
  
 \Package.Properties[LoggingMode]  
  
 \Package.Properties[LocaleID]  
  
 \Package\My SQL Task.Variables[User::varTableName].Properties[Value]  
  
 Dans cet exemple, la configuration met à jour ces propriétés :  
  
-   Propriété RaiseChangedEvent de la variable définie par l’utilisateur `TodaysDate`  
  
-   Propriétés MaximumErrorCount, LoggingMode et LocaleID du package  
  
-   Propriété Value de la variable définie par l’utilisateur `varTableName`, dans la portée de la tâche My SQL Task  
  
 La syntaxe « \Package » représente la racine, et les points (.) séparent les objets qui définissent le chemin d'accès de la propriété mise à jour par la configuration. Les noms de variables et de propriétés figurent entre crochets. Le terme Package est toujours utilisé dans la configuration, quel que soit le nom du package ; toutefois, tous les autres objets indiqués dans le chemin d'accès portent leur nom défini par l'utilisateur.  
  
 Une fois l'Assistant terminé, la nouvelle configuration est ajoutée à la liste des configurations dans la boîte de dialogue **Bibliothèque des configurations du package** .  
  
> **REMARQUE :** La dernière page de l’Assistant Configuration de package, Fin de l’Assistant, répertorie les propriétés cibles dans la configuration. Si vous souhaitez mettre à jour des propriétés pendant l’exécution de packages à l’aide de l’utilitaire d’invite de commandes **dtexec** , vous pouvez générer les chaînes qui représentent les chemins d’accès aux propriétés en exécutant l’Assistant Configuration de package, puis les copier et les coller dans la fenêtre d’invite de commandes pour les utiliser avec l’option set de **dtexec**.  
  
 Le tableau suivant décrit les colonnes dans la liste des configurations de la boîte de dialogue **Bibliothèque des configurations du package** .  
  
|colonne|Description|  
|------------|-----------------|  
|**Nom de la configuration**|Le nom de la configuration.|  
|**Type de configuration**|Le type de la configuration.|  
|**Chaîne de configuration**|L'emplacement de la configuration. L'emplacement peut être un chemin d'accès, une variable d'environnement, une clé de Registre, un nom de variable d'un package parent ou une table d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Objet cible**|Le nom de l'objet dont une propriété a une configuration. Si la configuration est un fichier de configuration XML, la colonne est vide, car la configuration peut mettre à jour plusieurs objets.|  
|**Propriété cible**|Nom de la propriété. Si la configuration écrit dans un fichier de configuration XML ou dans une table SQL Server, la colonne est vide puisque la configuration peut mettre à jour plusieurs objets.|  
  
### <a name="to-create-a-package-configuration"></a>Pour créer une configuration de package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l'onglet **Flux de contrôle**, **Flux de données**, **Gestionnaire d'événements**ou **Explorateur de package** .  
  
4.  Dans le menu **SSIS** , cliquez sur **Configurations du package**.  
  
5.  Dans la boîte de dialogue **Bibliothèque des configurations du package** , sélectionnez **Activer les configurations du package**et cliquez sur **Ajouter**.  
  
6.  Sur la page d'accueil de l'Assistant Configuration de package, cliquez sur **Suivant**.  
  
7.  Sur la page Sélectionner le type de configuration, spécifiez le type de configuration, puis définissez les propriétés se rapportant au type de configuration. Pour plus d’informations, voir [Package Configuration Wizard UI Reference](../../integration-services/packages/package-configuration-wizard-ui-reference.md).  
  
8.  Dans la page Sélectionner les propriétés à exporter, sélectionnez les propriétés des objets de package à inclure dans la configuration. Si le type de configuration ne prend en charge qu'une seule propriété, le titre de cette page de l'Assistant est Sélectionner la propriété cible. Pour plus d’informations, voir [Package Configuration Wizard UI Reference](../../integration-services/packages/package-configuration-wizard-ui-reference.md).  
  
    > **REMARQUE :** Seuls les types de configuration **Fichier de configuration XML** et **SQL Server** prennent en charge l’inclusion de plusieurs propriétés dans une configuration.  
  
9. Dans la page Fin de l'Assistant, tapez le nom de la configuration, puis cliquez sur **Terminer**.  
  
10. Affichez la configuration dans la boîte de dialogue **Bibliothèque des configurations du package** .  
  
11. Cliquez sur **Fermer**.  

## <a name="package-configurations-organizer"></a>Bibliothèque des configurations du package
  Utilisez la boîte de dialogue **Bibliothèque des configurations du package** pour activer les configurations du package, afficher une liste des configurations du package actuel et spécifier l'ordre de chargement de préférence des configurations.  
  
> **REMARQUE :** des configurations sont disponibles pour le modèle de déploiement de package. Les paramètres sont utilisés à la place des configurations pour le modèle de déploiement de projet. Le modèle de déploiement de projet vous permet de déployer des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d'informations sur les modèles de déploiement, consultez [Deployment of Projects and Packages](https://msdn.microsoft.com/library/hh213290.aspx).    
  
 Si plusieurs configurations mettent à jour la même propriété, les valeurs des configurations du bas de la liste de configuration remplacent les valeurs des configurations du haut de la liste. La dernière valeur chargée dans la propriété est la valeur utilisée à l'exécution du package. De plus, si le package utilise une combinaison de configuration directe (par exemple, un fichier de configuration XML) et de configuration indirecte (par exemple, une variable d'environnement), la configuration indirecte qui pointe vers l'emplacement de la configuration directe doit figurer plus haut dans la liste.  
  
> **REMARQUE :** lorsque les configurations du package sont chargées dans l’ordre souhaité, le chargement est effectué du haut vers le bas de la liste affichée dans la boîte de dialogue **Bibliothèques des configurations du package** . Toutefois, au moment de l'exécution, il se peut que les configurations de package ne soient pas chargées dans l'ordre souhaité. Les configurations de package parent sont notamment chargées après les configurations d'autres types.  
  
 Les configurations de package mettent à jour les valeurs des propriétés des objets de package au moment de l'exécution. Lorsqu'un package est chargé, les valeurs des configurations remplacent les valeurs définies lors du développement du package. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge différents types de configuration. Par exemple, vous pouvez utiliser un fichier XML pouvant contenir plusieurs configurations, ou une variable d'environnement qui contient une seule configuration. Pour plus d'informations, consultez [Package Configurations](../../integration-services/packages/package-configurations.md).  
  
### <a name="options"></a>Options  
 **Activer les configurations du package**  
 Sélectionnez cette option pour utiliser les configurations avec le package.  
  
 **Nom de la configuration**  
 Affichez le nom de la configuration.  
  
 **Type de configuration**  
 Affichez le type de l'emplacement où sont stockées les configurations.  
  
 **Chaîne de configuration**  
 Affichez l'emplacement où sont stockées les valeurs de configuration. L'emplacement peut être le chemin d'accès d'un fichier, le nom d'une variable d'environnement, le nom d'une variable de package parent, une clé de Registre ou le nom d'une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Objet cible**  
 Affiche le nom de l'objet mis à jour par la configuration. Si la configuration est un fichier de configuration XML ou une table SQL Server, la colonne est vide puisque la configuration peut inclure plusieurs objets.  
  
 **Propriété cible**  
 Affichez le nom de la propriété modifiée par la configuration. Cette colonne est vide si le type de configuration prend en charge plusieurs configurations.  
  
 **Ajouter**  
 Ajoutez une configuration à l'aide de l'Assistant Configuration de package.  
  
 **Modifier**  
 Modifiez une configuration existante en réexécutant l'Assistant Configuration de package.  
  
 **Supprimer**  
 Sélectionnez une configuration, puis cliquez sur **Supprimer**.  
  
 **Flèches**  
 Sélectionnez une configuration et utilisez les flèches vers le haut et vers le bas pour la déplacer vers le haut ou vers le bas de la liste. Les configurations sont chargées dans l'ordre dans lequel elles apparaissent dans la liste.  

## <a name="package-configuration-wizard-ui-reference"></a>Référence de l'interface utilisateur de l'Assistant Configuration de package
  **L’Assistant Configuration de package** vous permet de créer des configurations chargées de mettre à jour les propriétés d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ainsi que les objets qui s’y rattachent au moment de l’exécution. Cet Assistant s’exécute quand vous ajoutez une nouvelle configuration ou modifiez une configuration existante dans la boîte de dialogue **Bibliothèque des configurations du package** . Pour ouvrir la boîte de dialogue **Bibliothèque des configurations du package** , sélectionnez **Configurations du package** dans le menu **SSIS** de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Pour plus d’informations, consultez [Créer des configurations de package](../../integration-services/packages/create-package-configurations.md).  
  
> **REMARQUE :** des configurations sont disponibles pour le modèle de déploiement de package. Les paramètres sont utilisés à la place des configurations pour le modèle de déploiement de projet. Le modèle de déploiement de projet vous permet de déployer des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d'informations sur les modèles de déploiement, consultez [Deployment of Projects and Packages](https://msdn.microsoft.com/library/hh213290.aspx).  
  
 Les sections suivantes décrivent les pages de l'Assistant.  
  
### <a name="welcome-to-the-package-configuration-wizard-page"></a>Page Assistant Configuration de package  
 **L’Assistant de configuration de SSIS** vous permet de créer des configurations chargées de mettre à jour les propriétés d’un package ainsi que les objets qui s’y rattachent au moment de l’exécution du programme.  
  
#### <a name="options"></a>Options  
 **Ne plus afficher cette page**  
 Option permettant d'ignorer cette page d'accueil la prochaine fois que vous ouvrez l'Assistant.  
  
 **Suivant**  
 Permet de passer à la page suivante de l’Assistant.  
  
### <a name="select-configuration-type-page"></a>Page Sélectionner le type de configuration  
 La page **Sélectionner le type de configuration** permet de spécifier le type de configuration à créer.  
  
 Si vous souhaitez obtenir des informations supplémentaires pour déterminer quel type de configuration utiliser, consultez [Configurations de package](../../integration-services/packages/package-configurations.md).  
  
#### <a name="static-options"></a>Options statiques  
 **Type de configuration**  
 Sélectionnez le type de source dans lequel stocker la configuration, à l'aide des options suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Fichier de configuration XML**|Stocke la configuration sous forme de fichier XML. Si cette valeur est sélectionnée, les options dynamiques s’affichent dans la section **Type de configuration**.|  
|**Variable d'environnement**|Stocke la configuration dans une des variables d'environnement. Si cette valeur est sélectionnée, les options dynamiques s’affichent dans la section **Type de configuration**.|  
|**Entrée de Registre**|Stocke la configuration dans le Registre. Si cette valeur est sélectionnée, les options dynamiques s’affichent dans la section **Type de configuration**.|  
|**Variable de package parent**|Stocke la configuration en tant que variable dans le package qui contient la tâche.  Si cette valeur est sélectionnée, les options dynamiques s’affichent dans la section **Type de configuration**.|  
|**SQL Server**|Stocke la configuration dans une table de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si cette valeur est sélectionnée, les options dynamiques s’affichent dans la section **Type de configuration**.|  
  
 **Suivant**  
 Affiche la page suivante de l'Assistant.  
  
#### <a name="dynamic-options"></a>Options dynamiques  
  
##### <a name="configuration-type-option--xml-configuration-file"></a>Option Type de configuration = Fichier de configuration XML  
 **Spécifier directement les paramètres de configuration**  
 Permet de spécifier directement les paramètres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Nom du fichier de configuration**|Tapez le chemin d'accès du fichier de configuration généré par l'Assistant.|  
|**Parcourir**|La boîte de dialogue **Sélectionner l’emplacement du fichier de configuration** permet de spécifier le chemin d’accès au fichier de configuration généré par l’Assistant. Si le fichier n'existe pas, l'Assistant le crée.|  
  
 **L'emplacement de la configuration est stocké dans une variable d'environnement**  
 Permet de spécifier la variable d’environnement dans laquelle stocker la configuration.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Variable d'environnement**|Permet de sélectionner une variable d'environnement dans la liste.|  
  
##### <a name="configuration-type-option--environment-variable"></a>Option Type de configuration = Variable d'environnement  
 **Variable d'environnement**  
 Permet de sélectionner la variable d'environnement qui contient les informations de configuration.  
  
##### <a name="configuration-type-option--registry-entry"></a>Option Type de configuration = Entrée de Registre  
 **Spécifier directement les paramètres de configuration**  
 Permet de spécifier directement les paramètres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée de Registre**|Tapez la clé de Registre qui contient les informations de configuration. Le format est \<clé de Registre>.<br /><br /> La clé de Registre doit déjà exister dans HKEY_CURRENT_USER et porter une valeur nommée Value. Cette valeur peut être de type DWORD ou une chaîne.<br /><br /> Si vous souhaitez utiliser une clé de Registre qui n’est pas à la racine de HKEY_CURRENT_USER, utilisez le format \<Clé de Registre\clé de Registre\\...> pour identifier la clé.|  
  
 **L'emplacement de la configuration est stocké dans une variable d'environnement**  
 Permet de spécifier la variable d’environnement dans laquelle stocker la configuration.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Variable d'environnement**|Permet de sélectionner une variable d'environnement dans la liste.|  
  
##### <a name="configuration-type-option--parent-package-variable"></a>Option Type de configuration = Variable de package parent  
 **Spécifier directement les paramètres de configuration**  
 Permet de spécifier directement les paramètres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Variable parent**|Permet de spécifier la variable dans le package parent qui contient les informations de configuration.|  
  
 **L'emplacement de la configuration est stocké dans une variable d'environnement**  
 Permet de spécifier la variable d’environnement dans laquelle stocker la configuration.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Variable d'environnement**|Permet de sélectionner une variable d'environnement dans la liste.|  
  
##### <a name="configuration-type-options--sql-server"></a>Options Type de configuration = SQL Server  
 **Spécifier directement les paramètres de configuration**  
 Permet de spécifier directement les paramètres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion**|Permet de sélectionner une connexion dans la liste ou de cliquer sur **Nouvelle** pour créer une connexion.|  
|**Table de configuration**|Permet de sélectionner une table existante ou de cliquer sur **Nouvelle** pour écrire une instruction SQL qui crée une table.|  
|**Filtre de la configuration**|Permet de sélectionner le nom d'une configuration existante ou de taper un nouveau nom.<br /><br /> Un grand nombre de configurations SQL Server peuvent être stockées dans la même table et chacune d'entre elles peut inclure plusieurs éléments de configuration.<br /><br /> La valeur définie par l'utilisateur est stockée dans la table pour identifier les éléments de configuration qui appartiennent à une configuration particulière.|  
  
 **L'emplacement de la configuration est stocké dans une variable d'environnement**  
 Permet de spécifier la variable d'environnement dans laquelle stocker la configuration.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Variable d'environnement**|Permet de sélectionner une variable d'environnement dans la liste.|  
  
### <a name="select-objects-to-export-page"></a>Page Sélectionner des objets à exporter  
 Utilisez la page **Sélectionner la propriété cible ou Sélectionner les propriétés à exporter** pour définir les propriétés des objets que contient la configuration. La sélection de plusieurs propriétés n'est possible que si vous sélectionnez le type de configuration XML.  
  
#### <a name="options"></a>Options  
 **Objets**  
 Développe la hiérarchie du package et sélectionne les propriétés à exporter.  
  
 **Attributs de la propriété**  
 Affiche les attributs d'une propriété.  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
### <a name="completing-the-wizard-page"></a>Page Fin de l’Assistant  
 Utilisez la page **Fin de l’Assistant** pour nommer les paramètres de configuration et d’affichage utilisés par l’Assistant pour créer la configuration. Quand l’Assistant est terminé, la **Bibliothèque des configurations du package** affiche la liste de toutes les configurations du package.  
  
#### <a name="options"></a>Options  
 **Nom de la configuration**  
 Tapez le nom de la configuration.  
  
 **Aperçu**  
 Affiche les paramètres utilisés par l'Assistant pour créer la configuration.  
  
 **Terminer**  
 Crée la configuration et quitte **l’Assistant Configuration de package**.  

## <a name="child"></a> Utiliser les valeurs des variables et des paramètres dans un package enfant
  Cette procédure explique comment créer une configuration de package qui utilise le type de configuration de variable parent. Ce type de configuration active un package enfant exécuté à partir d'un package parent pour accéder à une variable dans le parent.  
  
> [!NOTE]  
>  Vous pouvez également passer les valeurs à un package enfant en configurant la tâche d'exécution afin de mapper les variables ou paramètres du package parent, ou les paramètres du projet, aux paramètres du package enfant. Pour plus d’informations, consultez [Tâche d’exécution de package](../../integration-services/control-flow/execute-package-task.md).  
  
 Il n'est pas nécessaire de créer la variable dans le package parent avant de créer la configuration de package dans le package enfant. Vous pouvez ajouter la variable au package parent à tout moment, mais vous devez utiliser le nom exact de la variable parent dans la configuration du package. Cependant, avant que vous puissiez créer une configuration de variable parent, la configuration doit pouvoir mettre à jour une variable existante dans le package enfant. Pour plus d’informations sur l’ajout et la configuration de variables, consultez [Ajouter, supprimer, modifier l’étendue de la variable définie par l’utilisateur dans un package](http://msdn.microsoft.com/library/cbf40c7f-3c8a-48cd-aefa-8b37faf8b40e).  
  
 La portée de la variable dans le package parent utilisé dans une configuration de variable parent peut être définie à la tâche d'exécution de package, au conteneur qui détient la tâche ou au package. Si plusieurs variables portant le même nom sont définies dans un package, la variable la plus proche en portée de la tâche d'exécution de package est employée. La portée la plus proche de la tâche d'exécution de package est la tâche proprement dite.  
  
### <a name="to-add-a-variable-to-a-parent-package"></a>Pour ajouter une variable à un package parent  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package auquel vous voulez ajouter une variable à transmettre à un package enfant.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , pour définir la portée de la variable, effectuez l'une des opérations suivantes :  
  
    -   Pour définir la portée du package, cliquez n’importe où sur l’aire de conception de l’onglet **Flux de contrôle** .  
  
    -   Pour définir la portée à un conteneur parent de la tâche d'exécution de package, cliquez sur le conteneur.  
  
    -   Pour définir la portée de la tâche d'exécution de package, cliquez sur la tâche.  
  
4.  Ajoutez et configurez une variable.  
  
    > [!NOTE]  
    >  Sélectionnez un type de données compatible avec les données que la variable stockera.  
  
5.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
### <a name="to-add-a-variable-to-a-child-package"></a>Pour ajouter une variable à un package enfant  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient le package auquel vous souhaitez ajouter une configuration de variable parent.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , pour définir la portée au package, cliquez n’importe où sur la zone de conception de l’onglet **Flux de contrôle** .  
  
4.  Ajoutez et configurez une variable.  
  
    > [!NOTE]  
    >  Sélectionnez un type de données compatible avec les données que la variable stockera.  
  
5.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="create-a-deployment-utility"></a>Créer un utilitaire de déploiement
  La première étape de déploiement des packages consiste à créer un utilitaire de déploiement pour un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . L'utilitaire de déploiement est un dossier qui contient les fichiers dont vous avez besoin pour déployer les packages dans un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un serveur différent. L'utilitaire de déploiement est créé sur l'ordinateur sur lequel le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est stocké.  
  
 Vous créez un utilitaire de déploiement de packages pour un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en configurant d'abord le processus de création pour créer un utilitaire de déploiement de packages, puis en créant le projet. Lorsque vous créez le projet, tous les packages et configurations de package dans le projet sont automatiquement inclus. Pour déployer des fichiers supplémentaires tels que le fichier Lisez-moi avec le projet, placez les fichiers dans le dossier **Divers** du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Lorsque le projet est généré, ces fichiers sont automatiquement inclus.  
  
 Vous pouvez configurer chaque déploiement de projet différemment. Avant de générer le projet et de créer l'utilitaire de déploiement de packages, vous pouvez définir les propriétés sur l'utilitaire de déploiement pour personnaliser la façon dont les packages du projet seront déployés. Par exemple, vous pouvez spécifier si les configurations de package peuvent être mises à jour lorsque le projet est déployé. Pour accéder aux propriétés d’un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , cliquez avec le bouton droit sur le projet, puis cliquez sur **Propriétés**.  
  
 Le tableau suivant récapitule les propriétés de l'utilitaire de déploiement.  
  
|Propriété|Description|  
|--------------|-----------------|  
|AllowConfigurationChange|Une valeur qui spécifie si les configurations peuvent être mises à jour lors du déploiement.|  
|CreateDeploymentUtility|Une valeur qui spécifie si un utilitaire de déploiement de packages est créé lorsque le projet est généré. Cette propriété doit être **True** pour créer un utilitaire de déploiement.|  
|DeploymentOutputPath|L'emplacement, relatif au projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , de l'utilitaire de déploiement.|  
  
 Quand vous générez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], un fichier manifeste, \<nom_projet>.SSISDeploymentManifest.xml et des copies des packages du projet et des dépendances de package sont créés et ajoutés dans le dossier bin\Deployment au sein du projet ou à l’emplacement spécifié dans la propriété DeploymentOutputPath. Le fichier manifeste répertorie les packages, les configurations de package et tous les divers autres fichiers du projet.  
  
 Le contenu du dossier de déploiement est actualisé chaque fois que vous générez le projet. Cela signifie que tout fichier enregistré dans ce dossier et qui n'est pas copié de nouveau dans le dossier par le processus de construction sera supprimé. Par exemple, les fichiers de configuration de package enregistrés dans les dossiers de déploiement seront supprimés.  
  
### <a name="to-create-a-package-deployment-utility"></a>Pour créer un utilitaire de déploiement de package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez la solution qui contient le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour lequel vous voulez créer un utilitaire de déploiement de package.  
  
2.  Cliquez avec le bouton droit sur le projet et cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **\<Pages de propriétés de <nom_projet**, cliquez sur **Utilitaire de déploiement**.  
  
4.  Pour mettre à jour les configurations de package quand les packages sont déployés, affectez à **AllowConfigurationChanges** la valeur **True**.  
  
5.  Affectez à **CreateDeploymentUtility** la valeur **True**.  
  
6.  Vous pouvez au besoin mettre à jour l’emplacement de l’utilitaire de déploiement en modifiant la propriété **DeploymentOutputPath** .  
  
7.  Cliquez sur **OK**.  
  
8.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis cliquez sur **Générer**.  
  
9. Affichez la progression de la build et les erreurs de build dans la fenêtre **Sortie** .  

## <a name="deploy-packages-by-using-the-deployment-utility"></a>Déployer des packages à l’aide de l’utilitaire de déploiement
  Après avoir créé un utilitaire de déploiement pour installer les packages d’un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur différent de celui sur lequel cet utilitaire a été créé, vous devez d’abord copier le dossier de déploiement vers l’ordinateur de destination.  
  
 Le chemin du dossier de déploiement est spécifié dans la propriété DeploymentOutputPath du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour lequel vous avez créé l’utilitaire de déploiement. Le chemin d’accès par défaut est bin\Deployment, lié au projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [Créer un utilitaire de déploiement](../../integration-services/packages/create-a-deployment-utility.md).  
  
 Vous utilisez l'Assistant Installation de package pour installer les packages. Pour lancer l'Assistant, double-cliquez sur l'utilitaire de déploiement après avoir copié le dossier de déploiement sur le serveur. Le fichier est nommé \<nom_projet>.SSISDeploymentManifest et se trouve dans le dossier de déploiement sur l’ordinateur de destination.  
  
> [!NOTE]  
>  En fonction de la version du package que vous déployez, une erreur risque de se produire si différentes version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées côte à côte. Cette erreur vient du fait que l'extension de nom de fichier .SSISDeploymentManifest est la même pour toutes les versions d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si vous double-cliquez sur le fichier, cela entraîne l’appel du programme d’installation (dtsinstall.exe) correspondant à la version installée en dernier d’ [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], laquelle risque d’être différente de la version du fichier de l’utilitaire de déploiement. Pour contourner ce problème, exécutez la version appropriée de dtsinstall.exe à partir de la ligne de commande, puis indiquez le chemin d'accès du fichier de l'utilitaire de déploiement.  
  
 L'Assistant Installation de package vous accompagne dans le processus d'installation des packages sur le système de fichiers et sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez configurer l'installation de plusieurs manières :  
  
-   Choix du type d'emplacement et emplacement d'installation des packages.  
  
-   Choix de l'emplacement d'installation des dépendances de package.  
  
-   Validation des packages après leur installation sur le serveur cible.  
  
 Les dépendances basées sur les fichiers pour les packages sont toujours installées sur le système de fichiers. Si vous installez un package sur le système de fichiers, les dépendances sont installées dans le même dossier que celui indiqué pour le package. Si vous installez un package sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez spécifier le dossier dans lequel les dépendances basées sur les fichiers seront stockées.  
  
 Si le package comprend des configurations que vous voulez modifier pour qu'elles soient utilisées sur l'ordinateur de destination, vous pouvez mettre à jour les valeurs des propriétés à l'aide de l'Assistant.  
  
 Outre l’installation des packages à l’aide de l’Assistant Installation de package, vous pouvez copier et déplacer des packages à l’aide de l’utilitaire d’invite de commandes **dtutil** . Pour plus d’informations, consultez [dtutil Utility](../../integration-services/dtutil-utility.md).  
  
### <a name="to-deploy-packages-to-an-instance-of-sql-server"></a>Pour déployer des packages sur une instance de SQL Server  
  
1.  Ouvrez le dossier de déploiement sur l'ordinateur cible.  
  
2.  Double-cliquez sur le fichier manifeste, \<nom_projet>.SSISDeploymentManifest, pour démarrer l’Assistant Installation de package.  
  
3.  Dans la page **Déployer les packages SSIS** , sélectionnez l’option **Déploiement sur SQL Server** .  
  
4.  Vous pouvez, si vous le souhaitez, sélectionner **Valider les packages après l’installation** pour valider les packages après qu’ils ont été installés sur le serveur cible.  
  
5.  Dans la page **Spécifier le serveur SQL cible** , spécifiez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle les packages doivent être installés et sélectionnez un mode d’authentification. Si vous sélectionnez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez fournir un nom d’utilisateur et un mot de passe.  
  
6.  Dans la page **Sélectionner le dossier d’installation** , spécifiez le dossier du système de fichiers dans lequel installer les dépendances du package.  
  
7.  Si le package comprend des configurations, vous pouvez les modifier en mettant à jour des valeurs dans la liste **Valeur** dans la page Configurer les packages.  
  
8.  Si vous avez choisi de valider les packages après l'installation, affichez les résultats de validation des packages déployés.  

## <a name="redeployment-of-packages"></a>Redéploiement de packages
  Après qu'un projet a été déployé, vous pouvez avoir besoin de mettre à jour ou d'étendre les fonctionnalités du package, puis de redéployer le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient les packages mis à jour. Au cours du processus de redéploiement des packages, vous devez vérifier les propriétés de configuration incluses dans l'utilitaire de déploiement. Par exemple, vous pouvez décider de ne pas autoriser les modifications de configuration après le redéploiement du package.  
  
### <a name="process-for-redeployment"></a>Processus de redéploiement  
 Après avoir mis à jour les packages, vous recréez le projet, vous copiez le dossier de déploiement sur l'ordinateur cible et vous réexécutez l'Assistant Installation de package.  
  
 Si vous ne mettez à jour que quelques packages dans le projet, vous ne souhaitez pas forcément redéployer le projet tout entier. Pour ne déployer que quelques packages, vous pouvez créer un nouveau projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ajouter les packages mis à jour au nouveau projet, puis créer et déployer le projet. Les configurations de package sont automatiquement copiées avec le package lorsque vous l'ajoutez à un autre projet.  

## <a name="package-installation-wizard-ui-reference"></a>Référence de l'interface utilisateur de l'Assistant Installation de package
  **L’Assistant Installation de package** permet de déployer un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , dont les packages et les divers fichiers qu’il contient, ainsi que les dépendances de package éventuelles.  
  
 Avant de déployer des packages, vous pouvez créer des configurations, puis déployer ces dernières avec les packages. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise les configurations pour mettre à jour dynamiquement les propriétés des packages et les objets de package au moment de l’exécution. Par exemple, il est possible de définir dynamiquement à l'exécution la chaîne de connexion d'une connexion OLE DB en fournissant une configuration qui mappe une valeur avec la propriété contenant la chaîne de connexion.  
  
 Vous ne pouvez pas exécuter l'Assistant Installation de package tant que vous n'avez pas généré un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et créé un utilitaire de déploiement. Pour plus d’informations, consultez [Déployer des packages à l’aide de l’utilitaire de déploiement](../../integration-services/packages/deploy-packages-by-using-the-deployment-utility.md).  
  
 Les sections suivantes décrivent les pages de l’Assistant.  
  
### <a name="welcome-to-the-package-installation-wizard-page"></a>Page Assistant Installation de package  
 **L’Assistant Installation de package** vous permet de déployer un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour lequel vous avez créé un utilitaire de déploiement de package.  
  
 **Ne plus afficher cette page de démarrage**  
 Ignorer la page de démarrage à la prochaine exécution de l'Assistant.  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
 **Terminer**  
 Passez directement à la page Fin de l'Assistant Installation de package. Utilisez cette option si vous êtes revenu en arrière dans les pages de l'Assistant pour examiner vos choix et si vous avez spécifié toutes les options obligatoires.  
  
### <a name="configure-packages-page"></a>Page Configurer les packages  
 Utilisez la page **Configurer les packages** pour modifier la configuration des packages.  
  
#### <a name="options"></a>Options  
 **Fichier de configuration**  
 Modifie le contenu d'un fichier de configuration sélectionné dans la liste.  
  
 **Related Topics:** [Create Package Configurations](../../integration-services/packages/create-package-configurations.md)  
  
 **Chemin d'accès**  
 Affiche le chemin d'accès de la propriété à configurer.  
  
 **Type**  
 Affiche le type des données de la propriété.  
  
 **Value**  
 Spécifiez la valeur de la configuration.  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
 **Terminer**  
 Passez directement à la page Fin de l'Assistant Installation de package. Utilisez cette option si vous êtes revenu en arrière dans les pages de l'Assistant pour examiner vos choix et si vous avez spécifié toutes les options obligatoires.  
  
### <a name="confirm-installation-page"></a>Page Confirmer l'installation  
 La page **Confirmer l’installation** permet de lancer l’installation de packages, de visualiser l’état et les informations que l’Assistant utilise pour installer les fichiers du projet spécifié.  
  
 **Suivant**  
 Installe les packages et leurs dépendances et passe à la page suivante de l'Assistant une fois l'installation terminée.  
  
 **État**  
 Affiche la progression de l'installation du package.  
  
 **Terminer**  
 Accédez à la page Fin de l'Assistant Installation de package. Utilisez cette option si vous êtes revenu en arrière dans les pages de l'Assistant pour revoir vos choix et si vous avez spécifié toutes les options indispensables.  
  
### <a name="deploy-ssis-packages-page"></a>Page Déployer les packages SSIS  
 La page **Déployer les packages SSIS** permet d’indiquer à quel emplacement les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et leurs dépendances doivent être installés.  
  
#### <a name="options"></a>Options  
 **Déploiement sur le système de fichiers**  
 Déploie les packages et leurs dépendances dans un dossier du système de fichiers.  
  
 **Déploiement sur SQL Server**  
 Déploie les packages et leurs dépendances dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez cette option si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] partage des packages entre serveurs. Toutes les dépendances de package sont installées dans le dossier spécifié du système de fichiers.  
  
 **Valider les packages après l'installation**  
 Indique si les packages doivent être validés après installation.  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
 **Terminer**  
 Passez directement à la page Fin de l'Assistant Installation de package. Utilisez cette option si vous êtes revenu en arrière dans les pages de l'Assistant pour examiner vos choix et si vous avez spécifié toutes les options obligatoires.  
  
### <a name="packages-validation-page"></a>Page Validation des packages  
 La page **Validation des packages** permet d’afficher et de suivre la progression de la validation des packages et les résultats de cette validation.  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
### <a name="select-installation-folder-page"></a>Page Sélectionner le dossier d'installation  
 La page **Sélectionner le dossier d’installation** permet de définir le dossier du système de fichiers dans lequel les packages et leurs dépendances sont installés.  
  
#### <a name="options"></a>Options  
 **Dossier**  
 Définissez le chemin d'accès et le dossier de copie du package et de ses dépendances.  
  
 **Parcourir**  
 Accédez au dossier cible via la boîte de dialogue **Rechercher un dossier** .  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
 **Terminer**  
 Passez directement à la page Fin de l'Assistant Installation de package. Utilisez cette option si vous voulez revenir dans les pages de l'Assistant pour vérifier vos choix et si vous avez défini toutes les options nécessaires.  
  
### <a name="specify-target-sql-server-page"></a>Page Spécifier le serveur SQL cible  
 La page **Spécifier le serveur SQL cible** permet de spécifier les options de déploiement du package dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="options"></a>Options  
 **Nom du serveur**  
 Permet de spécifier le nom du serveur pour lequel doit s'effectuer le déploiement des packages.  
  
 **Utiliser l'authentification Windows**  
 Permet d'indiquer si la méthode d'authentification Windows doit être utilisée pour ouvrir une session sur le serveur. L'authentification Windows est recommandée pour renforcer la sécurité.  
  
 **Utiliser l'authentification SQL Server**  
 Permet d'indiquer si la méthode d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être utilisée pour ouvrir une session sur le serveur. Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez fournir un nom d’utilisateur et un mot de passe.  
  
 **User name**  
 Indique un nom d'utilisateur.  
  
 **Mot de passe**  
 Permet de spécifier un mot de passe.  
  
 **Chemin d'accès au package**  
 Spécifiez le nom du dossier logique ou entrez « / » pour le dossier par défaut.  
  
 Pour sélectionner le dossier dans la boîte de dialogue **Package SSIS** , cliquez sur Parcourir (...). La boîte de dialogue ne fournit toutefois pas de moyen de sélectionner le dossier par défaut. Si vous souhaitez utiliser le dossier par défaut, vous devez entrer « / » dans la zone de texte.  
  
> [!NOTE]  
>  Si vous n'entrez pas de chemin d'accès de package valide, le message d'erreur suivant apparaît : « Un ou plusieurs arguments ne sont pas valides ».  
  
 **Se fier au serveur pour le chiffrement**  
 Permet d’utiliser les fonctionnalités de sécurité du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour sécuriser les packages.  
  
 **Suivant**  
 Permet de passer à la page suivante de l'Assistant.  
  
 **Terminer**  
 Passez directement à la page Fin de l'Assistant Installation de package. Utilisez cette option si vous êtes revenu en arrière dans les pages de l'Assistant pour examiner vos choix et si vous avez spécifié toutes les options obligatoires.  
  
### <a name="finish-the-package-installation-page"></a>Page Fin de l'Assistant Installation de package  
 La page **Fin de l’Assistant Installation de package** permet d’obtenir un résumé des résultats de l’installation des packages. Cette page fournit des détails comme le nom du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployé, les packages installés, les fichiers de configuration et l’emplacement de l’installation.  
  
 **Terminer**  
 Pour quitter l’Assistant, cliquez sur **Terminer**.  

