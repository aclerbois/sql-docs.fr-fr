---
title: Sauvegarde, restauration et synchronisation de bases de données (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 09a1312c37cfa47a0eb7cac909b388035d6352b2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="backing-up-restoring-and-synchronizing-databases-xmla"></a>Sauvegarde, restauration et synchronisation de bases de données (XMLA)
  XML for Analysis propose trois commandes qui permettent de sauvegarder, restaurer et synchroniser des bases de données :  
  
-   Le [sauvegarde](../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md) commande sauvegarde un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide de la base de données un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] le fichier de sauvegarde (.abf), comme décrit dans la section, [sauvegarde des bases de données](#backing_up_databases).  
  
-   Le [restaurer](../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md) commande restaure un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de base de données à partir d’un fichier .abf, comme décrit dans la section, [restaurer les bases de données](#restoring_databases).  
  
-   Le [synchroniser](../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md) commande synchronise un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de base de données avec les données et les métadonnées d’une autre base de données, comme décrit dans la section, [synchroniser les bases de données](#synchronizing_databases).  
  
##  <a name="backing_up_databases"></a> Sauvegarde des bases de données  
 Comme mentionné précédemment, le **sauvegarde** commande sauvegarde une [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données à un fichier de sauvegarde. Le **sauvegarde** commande possède plusieurs propriétés qui permettent de spécifier la base de données à sauvegarder, le fichier de sauvegarde à utiliser, la sauvegarde des définitions de sécurité et les partitions distantes à sauvegarder.  
  
> [!IMPORTANT]  
>  Le compte de service Analysis Services doit avoir l'autorisation d'écrire dans l'emplacement de sauvegarde spécifié pour chaque fichier. Par ailleurs, l'utilisateur doit avoir l'un des rôles suivants : rôle d'administrateur sur l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou membre d'un rôle de base de données avec les autorisations de contrôle total (Administrateur) sur la base de données à sauvegarder.  
  
### <a name="specifying-the-database-and-backup-file"></a>Spécification de la base de données et du fichier de sauvegarde  
 Pour spécifier la base de données à sauvegarder, vous définissez la [objet](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) propriété de la **sauvegarde** commande. Le **objet** propriété doit contenir un identificateur d’objet pour une base de données ou une erreur se produit.  
  
 Pour spécifier le fichier qui doit être créé et utilisé par le processus de sauvegarde, vous définissez la [fichier](../../analysis-services/xmla/xml-elements-properties/file-element-xmla.md) propriété de la **sauvegarde** commande. Le **fichier** propriété doit être définie sur un nom de chemin d’accès et de fichier UNC pour le fichier de sauvegarde doit être créé.  
  
 En plus de spécifier le fichier à utiliser pour la sauvegarde, vous pouvez définir les options suivantes pour ce même fichier :  
  
-   Si vous définissez la [AllowOverwrite](../../analysis-services/xmla/xml-elements-properties/allowoverwrite-element-xmla.md) sur true, le **sauvegarde** commande remplace le fichier de sauvegarde si le fichier spécifié existe déjà. Si vous définissez la **AllowOverwrite** propriété sur false, une erreur se produit si le fichier de sauvegarde spécifié existe déjà.  
  
-   Si vous définissez la [ApplyCompression](../../analysis-services/xmla/xml-elements-properties/applycompression-element-xmla.md) sur true, le fichier de sauvegarde est compressé après avoir créé le fichier.  
  
-   Si vous définissez la [mot de passe](../../analysis-services/xmla/xml-elements-properties/password-element-xmla.md) propriété une valeur non vide, le fichier de sauvegarde est chiffrée à l’aide du mot de passe spécifié.  
  
    > [!IMPORTANT]  
    >  Si **ApplyCompression** et **mot de passe** propriétés ne sont pas spécifiées, le fichier de sauvegarde stocke les noms d’utilisateur et mots de passe qui sont contenus dans les chaînes de connexion en texte clair. Les données stockées en texte en clair peuvent être récupérées. Pour une sécurité accrue, utilisez le **ApplyCompression** et **mot de passe** paramètres à la fois à compresser et chiffrer le fichier de sauvegarde.  
  
### <a name="backing-up-security-settings"></a>Sauvegarde des paramètres de sécurité  
 Le [sécurité](../../analysis-services/xmla/xml-elements-properties/security-element-xmla.md) propriété détermine si le **sauvegarde** commande sauvegarde les définitions de sécurité, telles que les rôles et les autorisations, définies sur une [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données. Le **sécurité** propriété détermine également si le fichier de sauvegarde inclut les comptes d’utilisateur Windows et groupes définis en tant que membres des définitions de sécurité.  
  
 La valeur de la **sécurité** propriété est limitée à une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*skipMembership*|Inclut les définitions de sécurité dans le fichier de sauvegarde mais exclut les informations d'appartenance.|  
|*CopyAll*|Inclut les définitions de sécurité et les informations d'appartenance dans le fichier de sauvegarde.|  
|*IgnoreSecurity*|Exclut les définitions de sécurité du fichier de sauvegarde.|  
  
### <a name="backing-up-remote-partitions"></a>Sauvegarde de partitions distantes  
 Pour sauvegarder des partitions distantes dans le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données, vous définissez la [BackupRemotePartitions](../../analysis-services/xmla/xml-elements-properties/backupremotepartitions-element-xmla.md) propriété de la **sauvegarde** commande sur true. Ce paramètre entraîne la **sauvegarde** commande pour créer un fichier de sauvegarde à distance pour chaque source de données distante qui est utilisé pour stocker les partitions distantes pour la base de données.  
  
 Pour chaque source de données distante à sauvegarder, vous pouvez spécifier le fichier de sauvegarde correspondant en incluant un [emplacement](../../analysis-services/xmla/xml-elements-properties/location-element-xmla.md) élément dans le [emplacements](../../analysis-services/xmla/xml-elements-properties/locations-element-xmla.md) propriété de la **sauvegarde** commande. Le **emplacement** élément doit avoir son **fichier** le nom de fichier et le chemin UNC du fichier de sauvegarde à distance, la valeur de propriété et sa [DataSourceID](../../analysis-services/xmla/xml-elements-properties/datasourceid-element-xmla.md) propriété définie à l’identificateur de la source de données distante définie dans la base de données.  
  
##  <a name="restoring_databases"></a> Restauration des bases de données  
 Le **restaurer** commande restaure un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données à partir d’un fichier de sauvegarde. Le **restaurer** commande possède plusieurs propriétés qui permettent de spécifier la base de données à restaurer, le fichier de sauvegarde à utiliser, comment restaurer des définitions de sécurité, les partitions distantes à stocker et objets réadressage relationnelle ROLAP (OLAP).  
  
> [!IMPORTANT]  
>  Pour chaque fichier de sauvegarde, l'utilisateur qui exécute la commande de restauration doit avoir l'autorisation de lire à partir de l'emplacement de sauvegarde spécifié pour chaque fichier. Pour restaurer une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui n'est pas installée sur le serveur, l'utilisateur doit également être un membre du rôle serveur pour cette instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Pour remplacer une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], l'utilisateur doit avoir l'un des rôles suivants : être membre du rôle serveur pour l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou membre d'un rôle de base de données avec les autorisations de contrôle total (Administrateur) sur la base de données à restaurer.  
  
> [!NOTE]  
>  Après la restauration d'une base de données existante, l'utilisateur qui a restauré la base de données peut perdre l'accès à la base de données restaurée. Cette perte d'accès peut se produire si, au moment de la sauvegarde, l'utilisateur n'était pas un membre du rôle de serveur ou un membre du rôle de base de données avec les autorisations de contrôle total (Administrateur).  
  
### <a name="specifying-the-database-and-backup-file"></a>Spécification de la base de données et du fichier de sauvegarde  
 Le **DatabaseName** propriété de la **restaurer** commande doit contenir un identificateur d’objet pour une base de données ou une erreur se produit. Si la base de données spécifié existe déjà, le **AllowOverwrite** propriété détermine si la base de données existante est remplacée. Si le **AllowOverwrite** est définie sur false et la base de données spécifié existe déjà, une erreur se produit.  
  
 Vous devez définir le **fichier** propriété de la **restaurer** commande UNC chemin d’accès et le nom du fichier de sauvegarde à restaurer la base de données spécifié. Vous pouvez également définir le **mot de passe** propriété pour le fichier de sauvegarde spécifié. Si le **mot de passe** est définie sur n’importe quelle valeur non vide, le fichier de sauvegarde est déchiffré à l’aide du mot de passe spécifié. Si le fichier de sauvegarde n'a pas été chiffré ou si le mot de passe spécifié ne correspond pas au mot de passe utilisé pour chiffrer le fichier de sauvegarde, une erreur se produit.  
  
### <a name="restoring-security-settings"></a>Restauration des paramètres de sécurité  
 Le **sécurité** propriété détermine si le **restaurer** commande restaure les définitions de sécurité, notamment les rôles et autorisations, définies dans un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données. Le **sécurité** propriété détermine également si le **restaurer** commande inclut les comptes d’utilisateur Windows et groupes définis en tant que membres des définitions de sécurité dans le cadre du processus de restauration.  
  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*skipMembership*|Inclut les définitions de sécurité dans la base de données mais exclut les informations d'appartenance.|  
|*CopyAll*|Inclut les définitions de sécurité et les informations d'appartenance dans la base de données.|  
|*IgnoreSecurity*|Exclut les définitions de sécurité de la base de données.|  
  
### <a name="restoring-remote-partitions"></a>Restaration de partitions distantes  
 Pour chaque fichier de sauvegarde distant créé lors d’une précédente **sauvegarde** de commande, vous pouvez restaurer sa partition distante associée en incluant un **emplacement** élément dans le **emplacements** propriété de la **restaurer** commande. Le [DataSourceType](../../analysis-services/xmla/xml-elements-properties/datasourcetype-element-xmla.md) propriété pour chaque **emplacement** élément doit être exclu ou explicitement la valeur *distant*.  
  
 Pour chaque spécifié **emplacement** élément, le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance contacte la source de données distante spécifiée dans le **DataSourceID** propriété à restaurer les partitions définies dans le fichier de sauvegarde distant spécifié dans le **fichier** propriété. Outre la **DataSourceID** et **fichier** propriétés, les propriétés suivantes sont disponibles pour chaque **emplacement** élément utilisé pour restaurer une partition distante :  
  
-   Pour remplacer la chaîne de connexion pour la source de données distante spécifiée dans **DataSourceID**, vous pouvez définir le **ConnectionString** propriété de la **emplacement** élément à une chaîne de connexion différents. Le **restaurer** commande utilise la chaîne de connexion qui est contenue dans le **ConnectionString** propriété. Si **ConnectionString** n’est pas spécifié, le **restaurer** commande utilise la chaîne de connexion stockée dans le fichier de sauvegarde pour la source de données distante spécifiée. Vous pouvez utiliser la **ConnectionString** paramètre pour déplacer une partition distante à une autre instance distante. Toutefois, vous ne pouvez pas utiliser le **ConnectionString** paramètre afin de restaurer une partition distante à la même instance qui contient la base de données restaurée. En d’autres termes, vous ne pouvez pas utiliser le **ConnectionString** propriété pour qu’une partition distante dans une partition locale.  
  
-   Pour chaque dossier d’origine utilisé pour stocker les partitions distantes sur la source de données distante, vous pouvez spécifier un [dossier](../../analysis-services/xmla/xml-elements-properties/folder-element-xmla.md) élément pour indiquer le nouveau dossier dans lequel restaurer les partitions distantes stockées dans le dossier d’origine. Si un **dossier** élément n’est pas spécifié, le **restaurer** commande utilise les dossiers d’origine spécifiés pour les partitions distantes qui sont contenues dans le fichier de sauvegarde à distance.  
  
### <a name="relocating-rolap-objects"></a>Déplacement d'objets ROLAP  
 Le **restaurer** commande ne peut pas restaurer des agrégations ou les données pour les objets qui utilisent le stockage ROLAP, car ces informations sont stockées dans les tables sur une source de données relationnelle sous-jacente. Toutefois, les métadonnées d'objets ROLAP peuvent être restaurées. Pour restaurer les métadonnées d’un objet ROLAP, les **restaurer** commande recrée la structure des tables d’une source de données relationnelle.  
  
 Vous pouvez utiliser la **emplacement** élément dans une **restaurer** commande pour déplacer des objets ROLAP. Pour chaque **emplacement** élément utilisé pour déplacer une source de données, le **DataSourceType** propriété doit être définie explicitement sur *Local*. Vous devez également définir le **ConnectionString** propriété de la **emplacement** élément à la chaîne de connexion du nouvel emplacement. Lors de la restauration, le **restaurer** commande remplace la chaîne de connexion pour la source de données identifiée par le **DataSourceID** propriété de la **emplacement** élément avec la valeur de la **ConnectionString** propriété de la **emplacement** élément.  
  
##  <a name="synchronizing_databases"></a> Synchronisation de bases de données  
 Le **synchroniser** commande synchronise les données et métadonnées d’un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données avec une autre base de données. Le **synchroniser** commande possède plusieurs propriétés qui vous permettent de spécifier la base de données source, la synchronisation des définitions de sécurité, les partitions distantes à synchroniser et la synchronisation des objets ROLAP.  
  
> [!NOTE]  
>  Le **synchroniser** commande peut être exécutée uniquement par les administrateurs de serveur et les administrateurs de base de données. Les bases de données source et de destination doivent avoir le même niveau de compatibilité de base de données.  
  
### <a name="specifying-the-source-database"></a>Spécification de la base de données source  
 Le [Source](../../analysis-services/xmla/xml-elements-properties/source-element-xmla.md) propriété de la **synchroniser** commande contient deux propriétés, **ConnectionString** et **objet**. Le **ConnectionString** propriété contient la chaîne de connexion de l’instance qui contient la base de données source et le **objet** propriété contient l’identificateur d’objet pour la base de données source.  
  
 La base de données de destination est la base de données en cours de la session dans laquelle le **synchroniser** commande s’exécute.  
  
 Si le **ApplyCompression** propriété de la **synchroniser** commande est définie sur true, les informations envoyées à partir de la source de base de données à la base de données de destination est compressée avant d’être envoyés.  
  
### <a name="synchronizing-security-settings"></a>Synchronisation des paramètres de sécurité  
 Le [SynchronizeSecurity](../../analysis-services/xmla/xml-elements-properties/synchronizesecurity-element-xmla.md) propriété détermine si le **synchroniser** commande synchronise les définitions de sécurité, notamment les rôles et autorisations, définies dans la base de données source. Le **SynchronizeSecurity** propriété détermine également si le **synchroniser des** commande inclut les comptes d’utilisateur Windows et groupes définis en tant que membres des définitions de sécurité.  
  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*skipMembership*|Inclut les définitions de sécurité dans la base de données de destination mais exclut les informations d'appartenance.|  
|*CopyAll*|Inclut les définitions de sécurité et les informations d'appartenance dans la base de données de destination.|  
|*IgnoreSecurity*|Exclut les définitions de sécurité de la base de données de destination.|  
  
### <a name="synchronizing-remote-partitions"></a>Synchronisation de partitions distantes  
 Pour chaque source de données distante qui existe sur la base de données source, vous pouvez synchroniser chaque partition distante associée en incluant un **emplacement** élément dans le **emplacements** propriété de la **synchroniser** commande. Pour chaque **emplacement** élément, le **DataSourceType** propriété doit être exclue ou explicitement la valeur *distant*.  
  
 Pour définir et se connecter à une source de données distante dans la base de données de destination, le **synchroniser** commande utilise la chaîne de connexion définie dans le **ConnectionString** propriété de la **emplacement** élément. Le **synchroniser** commande puis utilise le **DataSourceID** propriété de la **emplacement** élément pour identifier les partitions distantes à synchroniser. Le **synchroniser**commande synchronise les partitions distantes sur la source de données distante spécifiée dans le **DataSourceID** propriété sur la base de données source avec la source de données distante spécifiée dans le **DataSourceID** propriété sur la base de données de destination.  
  
 Pour chaque dossier d’origine utilisé pour stocker les partitions distantes sur la source de données distante sur la base de données source, vous pouvez également spécifier un **dossier** élément dans le **emplacement** élément. Le **dossier** élément indique le nouveau dossier pour la base de données de destination dans lequel vous souhaitez synchroniser les partitions distantes stockées dans le dossier d’origine de la source de données distante. Si un **dossier** élément n’est pas spécifié, la commande Synchronize utilise les dossiers d’origine spécifiés pour les partitions distantes qui sont contenues dans la base de données source.  
  
### <a name="synchronizing-rolap-objects"></a>Synchronisation d'objets ROLAP  
 Le **synchroniser** commande ne peut pas synchroniser les agrégations ou les données pour les objets qui utilisent le stockage ROLAP, car ces informations sont stockées dans les tables sur une source de données relationnelle sous-jacente. Toutefois, les métadonnées d'objets ROLAP peuvent être synchronisées. Pour synchroniser les métadonnées, le **synchroniser** commande recrée la structure des tables d’une source de données relationnelle.  
  
 Vous pouvez utiliser la **emplacement** élément dans une commande Synchronize pour synchroniser des objets ROLAP. Pour chaque **emplacement** élément utilisé pour déplacer une source de données, le **DataSourceType** propriété doit être définie explicitement sur *Local*. . Vous devez également définir le **ConnectionString** propriété de la **emplacement** élément à la chaîne de connexion du nouvel emplacement. Pendant la synchronisation, le **synchroniser** commande remplace la chaîne de connexion pour la source de données identifiée par le **DataSourceID** propriété de la **emplacement** élément avec la valeur de la **ConnectionString** propriété de la **emplacement** élément.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément Backup & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md)   
 [Restaurer l’élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md)   
 [Synchroniser, élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md)   
 [Sauvegarde et restauration de bases de données Analysis Services](../../analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases.md)  
  
  
