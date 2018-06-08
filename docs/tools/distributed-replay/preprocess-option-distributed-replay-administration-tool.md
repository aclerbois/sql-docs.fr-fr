---
title: Option preprocess (outil d’administration Distributed Replay) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: distributed-replay
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9b5012fd-233e-4a25-a2e1-585c63b70502
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 9a4a0cfe264cc5f67c5fe2f21590ffcd6abf63d0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="preprocess-option-distributed-replay-administration-tool"></a>Option preprocess (outil d'administration Distributed Replay)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L’outil d’administration [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay **DReplay.exe**est un outil en ligne de commande que vous pouvez utiliser pour communiquer avec Distributed Replay Controller. Cette rubrique décrit l’option de ligne de commande **preprocess** et la syntaxe correspondante.  
  
 L’option **preprocess** initialise l’étape de prétraitement. Lors de cette étape, le contrôleur prépare les données de trace d'entrée pour la relecture sur le serveur cible.  
  
 ![Icône de lien vers une rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien vers une rubrique") Pour plus d’informations sur les conventions de syntaxe utilisées par l’outil d’administration, consultez [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dreplay preprocess [-m controller] -i input_trace_file  
    -d controller_working_dir [-c config_file] [-f status_interval]  
```  
  
#### <a name="parameters"></a>Paramètres  
 **-m** *controller*  
 Spécifie le nom de l'ordinateur du contrôleur. Vous pouvez utiliser «`localhost`» ou «`.`» pour désigner l'ordinateur local.  
  
 Si le paramètre **-m** n’est pas spécifié, l’ordinateur local est utilisé.  
  
 **-i** *input_trace_file*  
 Spécifie le chemin complet du fichier de trace d’entrée sur le contrôleur, tel que `D:\Mytrace.trc`. Le paramètre **-i** est obligatoire.  
  
 Si des fichiers de substitution se trouvent dans le même répertoire, ils seront chargés et utilisés automatiquement. Les fichiers doivent suivre la convention d’affectation des noms de substitution de fichier, par exemple : `Mytrace.trc`, `Mytrace_1.trc`, `Mytrace_2.trc`, `Mytrace_3.trc`, etc. `Mytrace_n.trc`.  
  
> [!NOTE]  
>  Si vous utilisez l'outil d'administration sur un autre ordinateur que le contrôleur, vous devrez copier les fichiers de trace d'entrée vers le contrôleur afin qu'un chemin d'accès local puisse être utilisé pour ce paramètre.  
  
 **-d** *controller_working_dir*  
 Spécifie le répertoire du contrôleur où sera stocké le fichier intermédiaire. Le paramètre **-d** est obligatoire.  
  
 Les conditions suivantes s'appliquent :  
  
-   Le répertoire doit résider sur le contrôleur.  
  
-   Vous devez spécifier le chemin complet, en commençant par une lettre de lecteur (par exemple, `c:\WorkingDir`).  
  
-   Le chemin d'accès ne doit pas se terminer par une barre oblique inverse «`\`».  
  
-   Les chemins d'accès UNC ne sont pas pris en charge.  
  
 **-c** *config_file*  
 C'est le chemin d'accès complet au fichier de configuration de prétraitement ; il est utilisé pour spécifier l'emplacement du fichier de configuration de prétraitement en cas de stockage dans un autre emplacement. Ce paramètre peut être un chemin d'accès UNC, ou peut résider localement sur l'ordinateur où vous exécutez l'outil d'administration.  
  
 Le paramètre **-c** n’est pas obligatoire si aucun filtrage n’est exigé, ou si vous ne voulez pas modifier la durée d’inactivité maximale.  
  
 Sans le paramètre **-c** , le fichier de configuration de prétraitement par défaut `DReplay.exe.preprocess.config`est utilisé.  
  
 **-f** *status_interval*  
 Spécifie la fréquence (en secondes) d'affichage des messages d'état.  
  
 Si **-f** n’est pas spécifié, l’intervalle par défaut est de 30 secondes.  
  
## <a name="examples"></a>Exemples  
 Dans cet exemple, l'étape de prétraitement est initialisée avec tous les paramètres par défaut. La valeur `localhost` indique que le service contrôleur s'exécute sur le même ordinateur que l'outil d'administration. Le paramètre *input_trace_file* spécifie l’emplacement des données de trace d’entrée ( `c:\mytrace.trc`). Étant donné qu’aucun filtre de fichier de trace n’est impliqué, le paramètre **-c** doit être spécifié.  
  
```  
dreplay preprocess –m localhost -i c:\mytrace.trc -d c:\WorkingDir  
```  
  
 Dans cet exemple, l'étape de prétraitement est initialisée et un fichier de configuration de prétraitement modifié est spécifié. Contrairement à l’exemple précédent, le paramètre **-c** est utilisé pour pointer sur le fichier de configuration modifié, si vous l’avez stocké dans un emplacement différent. Exemple :  
  
```  
dreplay preprocess –m localhost -i c:\mytrace.trc -d c:\WorkingDir -c c:\DReplay.exe.preprocess.config  
```  
  
 Dans le fichier de configuration de prétraitement modifié, une condition de filtre est ajoutée qui exclut les sessions système pendant la relecture distribuée. Le filtre est ajouté en modifiant l'élément `<PreprocessModifiers>` dans le fichier de configuration de prétraitement, `DReplay.exe.preprocess.config`.  
  
 L'exemple suivant montre le fichier de configuration modifié :  
  
```  
<?xml version='1.0'?>  
<Options>  
    <PreprocessModifiers>  
        <IncSystemSession>No</IncSystemSession>  
        <MaxIdleTime>-1</MaxIdleTime>  
    </PreprocessModifiers>  
</Options>  
```  
  
## <a name="permissions"></a>Autorisations  
 Vous devez exécuter l'outil d'administration en tant qu'utilisateur interactif, comme un utilisateur local ou un compte d'utilisateur de domaine. Pour utiliser un compte d'utilisateur local, l'outil d'administration et le contrôleur doivent s'exécuter sur le même ordinateur.  
  
 Pour plus d’informations, voir [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Préparer les données de trace d'entrée](../../tools/distributed-replay/prepare-the-input-trace-data.md)   
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Configurer Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)  
  
  
