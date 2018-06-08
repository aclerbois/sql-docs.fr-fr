---
title: Utilitaire dtutil | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- verifying packages
- checking packages
- moving packages
- packages [Integration Services], command line options
- command prompt [Integration Services]
- SQL Server Integration Services packages, command line options
- copying packages
- existence testing [Integration Services]
- Integration Services packages, command line options
- SSIS packages, command line options
- deleting packages
- dtutil utility
- removing packages
- relocating packages
ms.assetid: 6c7975ff-acec-4e6e-82e5-a641e3a98afe
caps.latest.revision: 114
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a3978535dd221b4df0534b1e559d688d14741168
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dtutil-utility"></a>dtutil (utilitaire)
  L’utilitaire d’invite de commandes **dtutil** sert à gérer les packages [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Cet utilitaire peut copier, déplacer ou supprimer un package, ou en vérifier l'existence. Ces actions peuvent être effectuées sur n'importe quel package [!INCLUDE[ssIS](../includes/ssis-md.md)] stocké à l'un des trois emplacements suivants : une base de données [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , le magasin de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] et le système de fichiers. Si l'utilitaire accède à un package stocké dans **msdb**, il peut être nécessaire d'indiquer un nom d'utilisateur et un mot de passe à l'invite de commandes. Si l'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , il est nécessaire d'indiquer un nom d'utilisateur et un mot de passe à l'invite de commandes. Si le nom d'utilisateur est manquant, **dtutil** essaie de se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant l'authentification Windows. Le type de stockage du package est identifié par les options **/SQL**, **/FILE**et **/DTS** .  
  
 L'utilitaire d'invite de commandes **dtutil** ne prend pas en charge l'utilisation de fichiers de commandes ni la redirection.  
  
 L'utilitaire d'invite de commandes **dtutil** comprend les fonctionnalités suivantes :  
  
-   Des remarques présentes à l'invite de commandes, qui documentent automatiquement l'action en cours de réalisation et qui en facilitent la compréhension  
  
-   Une protection contre l'écrasement, qui sollicite votre confirmation avant une opération de copie ou de déplacement de packages susceptible de remplacer un package existant  
  
-   Une aide de la console, qui fournit des informations sur les options de commande de **dtutil**.  
  
> [!NOTE]  
>  Nombre des opérations qui sont réalisées par l'utilitaire dtutil peuvent également être réalisées visuellement dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] lorsque vous êtes connecté à une instance de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Pour plus d’informations, consultez [Gestion de packages &#40;Service SSIS&#41;](../integration-services/service/package-management-ssis-service.md).  
  
 Les options peuvent être entrées dans n'importe quel ordre. Le caractère (« | ») est l’opérateur **OR** et sert à afficher les valeurs possibles. Vous devez choisir l'une des options qui sont délimitées par le caractère **OR** .  
  
 Toutes les options doivent commencer par une barre oblique (/) ou par un signe moins (-). Toutefois, n'insérez pas d'espace entre la barre oblique ou le signe moins et le texte de l'option ; sinon, la commande échoue.  
  
 Les arguments doivent être des chaînes entre guillemets ou ne contenir aucun espace.  
  
 Les guillemets doubles à l'intérieur des chaînes mises entre guillemets simples représentent des guillemets simples en mode échappement.  
  
 Les options et les arguments, à l'exception des mots de passe, ne respectent pas la casse.  
  
 **Considérations concernant l'installation sur les ordinateurs 64 bits**  
  
 Sur un ordinateur 64 bits, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] installe une version 64 bits de l’utilitaire **dtexec** (dtexec.exe) et de l’utilitaire **dtutil** (dtutil.exe). Pour installer des versions 32 bits de ces outils [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , vous devez sélectionner Outils clients ou [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] lors de l'installation.  
  
 Par défaut, un ordinateur 64 bits qui dispose à la fois des versions 64 bits et 32 bits d'une invite de commandes [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] doit pouvoir exécuter la version 32 bits. La version 32 bits s'exécute car le chemin d'accès au répertoire de la version 32 bits apparaît dans la variable d'environnement PATH avant le chemin d'accès au répertoire de la version 64 bits. (En général, le chemin du répertoire 32 bits est *\<lecteur>*:\Program Files(x86)\Microsoft SQL Server\130\DTS\Binn, tandis que le chemin du répertoire 64 bits est *\<lecteur>*:\Program Files\Microsoft SQL Server\130\DTS\Binn.)  
  
> [!NOTE]  
>  Si vous utilisez SQL Server Agent pour exécuter l'utilitaire, il utilise automatiquement la version 64 bits de ce dernier. SQL Server Agent utilise le Registre, et non la variable d'environnement PATH, pour localiser le fichier exécutable correct de l'utilitaire.  
  
 Pour vous assurer que vous exécutez la version 64 bits de l'utilitaire à l'invite de commandes, vous pouvez effectuer l'une des actions suivantes :  
  
-   Ouvrez une fenêtre d’invite de commandes, indiquez le répertoire qui contient la version 64 bits de l’utilitaire *(\<lecteur>*:\Program Files\Microsoft SQL Server\130\DTS\Binn), puis exécutez celui-ci à partir de cet emplacement.  
  
-   À l’invite de commandes, exécutez l’utilitaire en entrant le chemin complet (*\<lecteur>*:\Program Files\Microsoft SQL Server\130\DTS\Binn) de la version 64 bits de l’utilitaire.  
  
-   Modifiez de manière définitive l’ordre des chemins dans la variable d’environnement PATH en plaçant le chemin 64 bits (*\<lecteur>*:\Program Files\Microsoft SQL Server\130\DTS\Binn) avant le chemin 32 bits (*\<lecteur>*:\Program Files(x86)\Microsoft SQL Server\130\DTS\Binn) dans la variable.  
  
## <a name="syntax"></a>Syntaxe  
  
```dos
dtutil /option [value] [/option [value]]...  
```  
  
#### <a name="parameters"></a>Paramètres  
  
|Option|Description|  
|------------|-----------------|  
|/?|Affiche les options d'invite de commandes.|  
|/C[opy] *location;destinationPathandPackageName*|Spécifie une copie sur un package [!INCLUDE[ssIS](../includes/ssis-md.md)] . Pour utiliser ce paramètre, vous devez tout d’abord spécifier l’emplacement du package à l’aide de l’option **/FI**, **/SQ**ou **/DT** . Spécifiez ensuite le nom du package de destination de l'emplacement de destination. L’argument *destinationPathandPackageName* spécifie sur quel emplacement le package [!INCLUDE[ssIS](../includes/ssis-md.md)] est copié. Si l'emplacement de destination *location* est **SQL**, les arguments *DestUser*, *DestPassword* et *DestServer* doivent être également spécifiés dans la commande.<br /><br /> Lorsque l'action **Copy** rencontre un package existant à l'emplacement de destination, **dtutil** invite l'utilisateur à confirmer la suppression du package. La réponse **Y** remplace le package et la réponse **N** termine le programme. Lorsque la commande inclut l'argument *Quiet* , aucune invite n'apparaît et tout package existant est remplacé.|  
|/Dec[rypt] *password*|(Facultatif). Définit le mot de passe de déchiffrement qui est utilisé lorsque vous chargez un package avec chiffrement de mot de passe.|  
|/Del[ete]|Supprime le package spécifié par l'option *SQL*, *DTS* ou *FILE* . Si **dtutil** ne peut pas supprimer le package, le programme prend fin.|  
|/DestP[assword] *password*|Spécifie le mot de passe utilisé avec l'option SQL pour se connecter à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de destination avec l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Une erreur est générée si *DESTPASSWORD* est spécifié dans une ligne de commande qui n'inclut pas l'option *DTSUSER* .<br /><br /> Remarque : [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)].|  
|/DestS[erver] *server_instance*|Spécifie le nom du serveur utilisé avec une action qui entraîne l'enregistrement d'une destination dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Cette valeur sert à identifier un serveur non local ou autre que le serveur par défaut lors de l’enregistrement d’un package [!INCLUDE[ssIS](../includes/ssis-md.md)] . La spécification de *DESTSERVER* dans une ligne de commande sans action associée à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]constitue une erreur. Des actions telles que *SIGN SQL*, *COPY SQL*ou *MOVE SQL* représentent des commandes appropriées à combiner avec cette option.<br /><br /> Un nom d'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut être spécifié en ajoutant à la suite du nom du serveur une barre oblique et le nom de l'instance.|  
|/DestU[ser] *username*|Spécifie le nom d’utilisateur qui est employé avec les options *SIGN SQL*, *COPY SQL*et *MOVE SQL* pour se connecter à une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La spécification de *DESTUSER* dans une ligne de commande qui n'inclut pas l'option *SIGN SQL*, *COPY SQL*ou *MOVE SQL* constitue une erreur.|  
|/Dump *process ID*|(Facultatif) Entraîne la suspension du processus spécifié, de l’utilitaire **dtexec** ou du processus **dtsDebugHost.exe** , ainsi que la création des fichiers de vidage de débogage, .mdmp et .tmp.<br /><br /> Remarque : Pour utiliser l’option **/Dump**, le droit d’utilisateur Déboguer les programmes doit vous être assigné (SeDebugPrivilege).<br /><br /> Pour trouver le *process ID* du processus que vous souhaitez suspendre, utilisez le Gestionnaire des tâches de Windows.<br /><br /> Par défaut, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] stocke les fichiers de vidage du débogage dans le dossier *\<lecteur>*:\Program Files\Microsoft SQL Server\130\Shared\ErrorDumps.<br /><br /> Pour plus d’informations sur l’utilitaire **dtexec** et le processus **dtsDebugHost.exe** , consultez [Utilitaire dtexec](../integration-services/packages/dtexec-utility.md) et [Génération, déploiement et débogage d’objets personnalisés](../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).<br /><br /> Pour plus d’informations sur les fichiers de vidage du débogage, consultez [Générer des fichiers de vidage pour l’exécution des packages](../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).<br /><br /> Remarque : Les fichiers de vidage du débogage peuvent contenir des informations sensibles. Utilisez une liste de contrôle d'accès (ACL, Access Control List) pour restreindre l'accès aux fichiers ou copiez ces derniers dans un dossier avec accès restreint.|  
|/DT[S] *filespec*|Spécifie que le package [!INCLUDE[ssIS](../includes/ssis-md.md)] à traiter se trouve dans le magasin de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] . L’argument *filespec* doit inclure le chemin du dossier en commençant à la racine du magasin de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] . Par défaut, les noms des dossiers racine dans le fichier de configuration sont "MSDB" et "Système de fichiers". Les chemins d'accès qui contiennent un espace doivent être placés entre guillemets.<br /><br /> Si l'option DT[S] est spécifiée sur la même ligne de commande que l'une des options suivantes, une erreur DTEXEC_DTEXECERROR est retournée :<br /><br /> **FILE**<br /><br /> **SQL**<br /><br /> **SOURCEUSER**<br /><br /> **SOURCEPASSWORD**<br /><br /> **SOURCESERVER**|  
|/En[crypt] *{SQL &#124; FILE}; Path;ProtectionLevel[;password]*|(Facultatif). Chiffre le package chargé avec le niveau de protection et le mot de passe spécifiés, puis l'enregistre à l'emplacement spécifié dans *Path*. Le *ProtectionLevel* détermine si un mot de passe est requis.<br /><br /> *SQL* - Path est le nom du package de destination.<br /><br /> *FILE* - Path est le chemin complet et le nom de fichier du package.<br /><br /> *DTS* - Cette option n’est actuellement pas prise en charge.<br /><br /> Options*ProtectionLevel* :<br /><br /> Niveau 0 : retire les informations sensibles.<br /><br /> Niveau 1 : les informations sensibles sont chiffrées en utilisant des informations d'identification de l'utilisateur local.<br /><br /> Niveau 2 : les informations sensibles sont chiffrées à l'aide du mot de passe requis.<br /><br /> Niveau 3 : le package est chiffré à l'aide du mot de passe requis.<br /><br /> Niveau 4 : le package est chiffré à l'aide des informations d'identification de l'utilisateur local.<br /><br /> Un package de niveau 5 utilise le chiffrement de stockage [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .|  
|/Ex[ists]|(Facultatif). Sert à déterminer si un package existe. **dtutil** tente de localiser le package spécifié à l'aide des options *SQL*, *DTS* ou *FILE* . Si **dtutil** ne peut pas localiser le package spécifié, une erreur DTEXEC_DTEXECERROR est retournée.|  
|/FC[reate] {*SQL* &#124; *DTS*};*ParentFolderPath;NewFolderName*|(Facultatif). Crée un nouveau dossier portant le nom que vous avez spécifié dans *NewFolderName*. L'emplacement du nouveau dossier est indiqué par *ParentFolderPath*.|  
|/FDe[lete] {*SQL* &#124; *DTS*}[;*ParentFolderPath;FolderName]*|(Facultatif). Supprime de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou [!INCLUDE[ssIS](../includes/ssis-md.md)] le dossier qui a été spécifié par le nom dans *FolderName*. L'emplacement du dossier à supprimer est indiqué par *ParentFolderPath*.|  
|/FDi[rectory] {*SQL* &#124; *DTS*};*FolderPath[;S]*|(Facultatif). Répertorie le contenu, dossiers et packages, dans un dossier sur [!INCLUDE[ssIS](../includes/ssis-md.md)] ou [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le paramètre facultatif *FolderPath* spécifie le dossier dont vous voulez afficher le contenu. Le paramètre facultatif *S* spécifie que vous voulez afficher la liste du contenu des sous-dossiers du dossier indiqué dans *FolderPath*.|  
|/FE[xists ] {*SQL* &#124; *DTS*};*FolderPath*|(Facultatif). Vérifie si le dossier spécifié existe sur [!INCLUDE[ssIS](../includes/ssis-md.md)] ou [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le paramètre *FolderPath* représente le chemin et le nom de dossier à vérifier.|  
|/Fi[le] *filespec*|Cette option spécifie que le package [!INCLUDE[ssIS](../includes/ssis-md.md)] à traiter se trouve dans le système de fichiers. La valeur *filespec* peut être fournie sous la forme d’un chemin UNC (Universal Naming Convention) ou d’un chemin local.<br /><br /> Si l’option *File* est spécifiée sur la même ligne de commande que l’une des options suivantes, une erreur DTEXEC_DTEXECERROR est retournée :<br /><br /> **DTS**<br /><br /> **SQL**<br /><br /> **SOURCEUSER**<br /><br /> **SOURCEPASSWORD**<br /><br /> **SOURCESERVER**|  
|/FR[ename] {*SQL* &#124; *DTS*} [;*ParentFolderPath; OldFolderName;NewFolderName]*|(Facultatif). Renomme un dossier sur [!INCLUDE[ssIS](../includes/ssis-md.md)] ou [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. *ParentFolderPath* constitue l'emplacement du dossier à renommer. *OldFolderName* correspond au nom actuel du dossier tandis que *NewFolderName* représente le nouveau nom à attribuer au dossier.|  
|/H[elp] *option*|Affiche une aide contextuelle qui indique les options **dtutil** et décrit leur utilisation. Cet argument est facultatif. Si l'argument est inclus, le texte d'aide contient des informations détaillées sur l'option spécifiée. L'exemple suivant affiche une aide pour toutes les options :<br /><br /> `dtutil /H`<br /><br /> Les deux exemples suivants illustrent l’utilisation de l’option */H* pour afficher une aide étendue sur une option spécifique, l’option */Q [uiet]* , dans cet exemple :<br /><br /> `dtutil /Help Quiet`<br /><br /> `dtutil /H Q`|  
|/I[DRegenerate]|Crée un nouvel identificateur global unique (GUID) pour le package et met à jour la propriété de l'ID du package. Lorsqu'un package est copié, l'ID du package ne change pas ; ainsi les fichiers journaux contiennent le même GUID pour les deux packages. Cette action crée un nouveau GUID pour le package nouvellement copié, afin de le distinguer de l'original.|  
|/M[ove] {*SQL* &#124; *File* &#124; *DTS*}; *pathandname*|Spécifie un déplacement sur un package [!INCLUDE[ssIS](../includes/ssis-md.md)] . Pour utiliser ce paramètre, vous devez tout d’abord spécifier l’emplacement du package à l’aide de l’option **/FI**, **/SQ**ou **/DT** . Spécifiez ensuite l'action **Move** . Cette action nécessite deux arguments, séparés par un point-virgule :<br /><br /> L'argument de destination peut spécifier *SQL*, *FILE*ou *DTS*. Une destination *SQL* peut comporter les options *DESTUSER*, *DESTPASSWORD*et *DESTSERVER* .<br /><br /> L’argument *pathandname* spécifie l’emplacement du package : *SQL* utilise le chemin et le nom du package, *FILE* utilise un UNC ou un chemin local tandis que *DTS* utilise un emplacement relatif à la racine du magasin de packages [!INCLUDE[ssIS](../includes/ssis-md.md)] . Lorsque la destination est *FILE* ou *DTS*, l'argument path n'inclut pas le nom du fichier. Il utilise plutôt le nom du package à l'emplacement spécifié comme le nom de fichier.<br /><br /> <br /><br /> Lorsque l'action **MOVE** rencontre un package existant à l'emplacement de destination, **dtutil** vous invite à confirmer le remplacement du package. La réponse **Y** remplace le package et la réponse **N** termine le programme. Lorsque la commande inclut l'option *QUIET* , aucune invite n'apparaît et tout package existant est remplacé.|  
|/Q[uiet]|Arrête les invites de confirmation pouvant apparaître lorsqu'une commande incluant l'option **COPY**, **MOVE**ou **SIGN** est exécutée. Ces invites apparaissent si un package portant le même nom que le package spécifié existe déjà sur l'ordinateur de destination ou si le package spécifié est déjà signé.|  
|/R[emark] *text*|Ajoute un commentaire à la ligne de commande. L'argument de commentaire est facultatif. Si le texte du commentaire inclut les espaces, il est mis entre guillemets. Vous pouvez inclure plusieurs options REM dans une ligne de commande.|  
|/Si[gn] {*SQL* &#124; *File* &#124; *DTS*}; *path*; *hash*|Signe un package [!INCLUDE[ssIS](../includes/ssis-md.md)] . Cette action utilise trois arguments requis, séparés par des point-virgules ; destination, path et hash :<br /><br /> L'argument de destination peut spécifier *SQL*, *FILE*ou *DTS*. Une destination SQL peut comporter les options *DESTUSER*, *DESTPASSWORD* et *DESTSERVER* .<br /><br /> L'argument path spécifie l'emplacement du package à traiter.<br /><br /> L'argument hash spécifie un identificateur de certificat exprimé en tant que chaîne hexadécimale de longueur variable.<br /><br /> Pour plus d’informations, consultez [Identifier la source de packages à l’aide de signatures numériques](../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).<br /><br /> <br /><br /> **\*\* Important \*\*** Lorsqu’il est configuré pour vérifier la signature du package, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] vérifie seulement si la signature numérique est présente, si elle est valide et si elle provient d’une source approuvée. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] ne vérifie *pas* si le package a été modifié.|  
|/SourceP[assword] *password*|Spécifie le mot de passe utilisé avec les options *SQL* et *SOURCEUSER* pour permettre la récupération d'un package [!INCLUDE[ssIS](../includes/ssis-md.md)] qui est stocké dans une base de données, sur une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La spécification de *SOURCEPASSWORD* dans une ligne de commande qui n'inclut pas l'option **SOURCEUSER** constitue une erreur.<br /><br /> Remarque : [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]|  
|/SourceS[erver] *server_instance*|Spécifie le nom du serveur qui est utilisé avec l'option **SQL** pour permettre l'extraction d'un package [!INCLUDE[ssIS](../includes/ssis-md.md)] qui est stocké dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La spécification de *SOURCESERVER* dans une ligne de commande qui n'inclut pas l'option *SIGN SQL*, *COPY* *SQL*ou *MOVE* *SQL* constitue une erreur.<br /><br /> Un nom d'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut être spécifié en ajoutant à la suite du nom du serveur une barre oblique et le nom de l'instance.|  
|/SourceU[ser] *username*|Spécifie le nom d'utilisateur qui est employé avec les options *SOURCESERVER* pour permettre l'extraction d'un package [!INCLUDE[ssIS](../includes/ssis-md.md)] , d'un package [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] stocké dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . La spécification de *SOURCEUSER* dans une ligne de commande qui n'inclut pas l'option *SIGN SQL*, *COPY SQL*ou *MOVE SQL* constitue une erreur.<br /><br /> Remarque : [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]|  
|/SQ[L] *package_path*|Spécifie l'emplacement d'un package [!INCLUDE[ssIS](../includes/ssis-md.md)] . Cette option indique que le package est stocké dans la base de données **msdb** . L’argument *package_path* spécifie le chemin et le nom du package [!INCLUDE[ssIS](../includes/ssis-md.md)] . Les noms de dossier se terminent par des barres obliques inverses.<br /><br /> Si l’option *SQL* est spécifiée sur la même ligne de commande que l’une des options suivantes, une erreur DTEXEC_DTEXECERROR est retournée :<br /><br /> *DTS*<br /><br /> *FILE*<br /><br /> L'option *SQL* peut être accompagnée de zéro ou d'une instance des options suivantes :<br /><br /> *SOURCEUSER*<br /><br /> *SOURCEPASSWORD*<br /><br /> *SOURCESERVER*<br /><br /> <br /><br /> Si *SOURCEUSERNAME* n'est pas inclus, l'authentification Windows est utilisée pour accéder au package. *SOURCEPASSWORD* est autorisé uniquement si *SOURCEUSER* est présent. Si l'option *SOURCEPASSWORD* n'est pas incluse, un mot de passe vide est employé.<br /><br /> **\*\* Important \*\*** [!INCLUDE[ssNoteStrongPass](../includes/ssnotestrongpass-md.md)]|  
  
## <a name="dtutil-exit-codes"></a>Codes de sortie dtutil  
 L'utilitaire**dtutil** définit un code de sortie qui vous alerte lorsque des erreurs de syntaxe sont détectées, des arguments incorrects sont employés ou des combinaisons non valides d'options sont spécifiées. Dans le cas contraire, l'utilitaire signale « L'opération s'est terminée avec succès ». Le tableau ci-dessous répertorie les valeurs que l'utilitaire **dtutil** peut définir lors de sa fermeture.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|L'utilitaire s'est exécuté avec succès.|  
| 1|L'utilitaire a échoué.|  
|4|L'utilitaire ne peut pas localiser le package demandé.|  
|5|L'utilitaire ne peut pas charger le package demandé.|  
|6|L'utilitaire ne peut pas résoudre la ligne de commande car elle comporte des erreurs syntaxiques ou sémantiques.|  
  
## <a name="remarks"></a>Notes   
 Vous ne pouvez pas utiliser de fichiers de commandes ou une redirection avec **dtutil**.  
  
 L'ordre des options dans la ligne de commande n'est pas significatif.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants présentent de façon détaillée des scénarios classiques d'utilisation de ligne de commande.  
  
### <a name="copy-examples"></a>Exemples de copie  
 Pour copier dans le magasin de packages SSIS un package stocké au sein de la base de données **msdb** , elle-même présente sur une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification Windows, employez la syntaxe suivante :  
  
```dos
dtutil /SQL srcPackage /COPY DTS;destFolder\destPackage   
```  
  
 Pour copier un package d'un emplacement sur le système de fichiers dans un autre emplacement et renommer la copie, utilisez la syntaxe suivante :  
  
```dos
dtutil /FILE c:\myPackages\mypackage.dtsx /COPY FILE;c:\myTestPackages\mynewpackage.dtsx  
```  
  
 Pour copier un package sur le système de fichiers local dans une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] hébergée sur un autre ordinateur, utilisez la syntaxe suivante :  
  
```dos
dtutil /FILE c:\sourcepkg.dtsx /DestServer <servername> /COPY SQL;destpkgname  
```  
  
 Dans la mesure où les options */DestU[ser]* et */DestP[assword]* n’ont pas été utilisées, l’utilisation de l’authentification Windows est implicite.  
  
 Pour créer un ID destiné à un package nouvellement copié, utilisez la syntaxe suivante :  
  
```dos
dtutil /I /FILE copiedpkg.dtsx   
```  
  
 Pour créer un ID destiné à tous les packages d'un dossier spécifique, utilisez la syntaxe suivante :  
  
```dos
for %%f in (C:\test\SSISPackages\*.dtsx) do dtutil.exe /I /FILE %%f  
```  
  
 Utilisez un signe de pourcentage unique (%) lorsque vous tapez la commande à partir de l'invite et utilisez un signe de pourcentage double (%%) si la commande est utilisée à l'intérieur d'un fichier de commandes.  
  
### <a name="delete-examples"></a>Exemples de suppression  
 Pour supprimer un package stocké dans la base de données **msdb** sur une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification Windows, employez la syntaxe suivante :  
  
```dos
dtutil /SQL delPackage /DELETE  
```  
  
 Pour supprimer un package stocké dans la base de données **msdb** sur une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , employez la syntaxe suivante :  
  
```dos
dtutil /SQL delPackage /SOURCEUSER srcUserName /SOURCEPASSWORD #8nGs*w7F /DELETE  
```  
  
> [!NOTE]  
>  Pour supprimer un package d'un serveur nommé, incluez l'option **SOURCESERVER** et son argument. Vous pouvez spécifier un serveur uniquement par le biais de l'option *SQL* .  
  
 Pour supprimer un package stocké dans le magasin des packages SSIS, utilisez la syntaxe suivante :  
  
```dos
dtutil /DTS delPackage.dtsx /DELETE  
```  
  
 Pour supprimer un package stocké dans le système de fichiers, utilisez la syntaxe suivante :  
  
```dos
dtutil /FILE c:\delPackage.dtsx /DELETE  
```  
  
### <a name="exists-examples"></a>Exemples d'Exists  
 Pour déterminer si un package existe dans la base de données **msdb** sur une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification Windows, employez la syntaxe suivante :  
  
```dos
dtutil /SQL srcPackage /EXISTS  
```  
  
 Pour déterminer si un package existe dans la base de données **msdb** sur une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , employez la syntaxe suivante :  
  
```dos
dtutil /SQL srcPackage /SOURCEUSER srcUserName /SOURCEPASSWORD *hY$d56b /EXISTS  
```  
  
> [!NOTE]  
>  Pour déterminer si un package existe sur un serveur nommé, incluez l'option **SOURCESERVER** et son argument. Vous pouvez uniquement spécifier un serveur en utilisant l'option SQL.  
  
 Pour déterminer si un package existe dans le magasin de packages local, utilisez la syntaxe suivante :  
  
```dos
dtutil /DTS srcPackage.dtsx /EXISTS  
```  
  
 Pour déterminer si un package existe dans le système de fichiers local, utilisez la syntaxe suivante :  
  
```dos
dtutil /FILE c:\srcPackage.dtsx /EXISTS  
```  
  
### <a name="move-examples"></a>Exemples de Move  
 Pour déplacer un package stocké dans le magasin de packages SSIS vers la base de données **msdb** , elle-même située sur une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification Windows, employez la syntaxe suivante :  
  
```dos
dtutil /DTS srcPackage.dtsx /MOVE SQL;destPackage  
```  
  
 Pour déplacer un package stocké dans la base de données **msdb** , elle-même située sur une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vers la base de données **msdb** située sur une autre instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , employez la syntaxe suivante :  
  
```dos
dtutil /SQL srcPackage /SOURCEUSER srcUserName /SOURCEPASSWORD $Hj45jhd@X /MOVE SQL;destPackage /DESTUSER destUserName /DESTPASSWORD !38dsFH@v  
```  
  
> [!NOTE]  
>  Pour déplacer un package d'un serveur nommé à un autre, incluez les options **SOURCES** et **DESTS** , ainsi que leurs arguments. Vous ne pouvez spécifier des serveurs qu'à l'aide de l'option *SQL* .  
  
 Pour déplacer un package stocké dans le magasin des packages SSIS, employez la syntaxe suivante :  
  
```dos
dtutil /DTS srcPackage.dtsx /MOVE DTS;destPackage.dtsx  
```  
  
 Pour déplacer un package qui est stocké dans le système de fichiers, employez la syntaxe suivante :  
  
```dos
dtutil /FILE c:\srcPackage.dtsx /MOVE FILE;c:\destPackage.dtsx  
```  
  
### <a name="sign-examples"></a>Exemples de Sign  
 Pour signer un package qui est stocké dans une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur une instance locale de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui utilise l'authentification Windows, utilisez la syntaxe suivante :  
  
```dos
dtutil /FILE srcPackage.dtsx /SIGN FILE;destpkg.dtsx;1767832648918a9d989fdac9819873a91f919  
```  
  
 Pour obtenir des informations concernant votre certificat, utilisez **CertMgr**. Le code de hachage peut être affiché dans l'utilitaire **CertMgr** en sélectionnant le certificat, puis en cliquant sur **Afficher** pour afficher les propriétés. L'onglet **Détails** fournit des informations supplémentaires sur le certificat. La propriété **Thumbprint** est utilisée comme valeur de hachage, sans les espaces.  
  
> [!NOTE]  
>  Le hachage utilisé dans l'exemple ci-dessus n'est pas un hachage véritable.  
  
 Pour plus d'informations, consultez la section « CertMgr » de l'article (en anglais) « [Signing and Checking Code with Authenticode](http://go.microsoft.com/fwlink/?LinkId=78100)» (signature et vérification du code à l'aide d'Authenticode).  
  
### <a name="encrypt-examples"></a>Exemples d'Encrypt  
 L'exemple suivant chiffre le fichier PackageToEncrypt.dtsx dans le fichier EncryptedPackage.dts en utilisant un chiffrement de package complet, avec un mot de passe. Le mot de passe qui est utilisé pour le chiffrement est *EncPswd*.  
  
```dos
dtutil /FILE PackageToEncrypt.dtsx /ENCRYPT file;EncryptedPackage.dtsx;3;EncPswd  
```  
  
## <a name="see-also"></a> Voir aussi  
[Exécuter des packages Integration Services (SSIS)](../integration-services/packages/run-integration-services-ssis-packages.md)  
  
  
