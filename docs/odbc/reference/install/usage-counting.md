---
title: Utilisation de l’inventaire | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- usage counts [ODBC]
- file usage counts [ODBC]
- installing ODBC components [ODBC], usage counts
- subkeys [ODBC], usage counts
ms.assetid: 0678aee9-8256-463c-89dd-77b1a0dfdd60
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 51d1acca922245a4b57ecd30428e0b4079327e29
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="usage-counting"></a>Utilisation de l’inventaire
> [!NOTE]  
>  ODBC à partir de Windows XP et Windows Server 2003, est inclus dans le système d’exploitation Windows. Vous devez explicitement uniquement installer ODBC dans les versions antérieures de Windows.  
  
 Deux types de compteurs d’utilisation sont conservées dans le Registre pour chaque composant : un nombre d’utilisations de composant et un ou plusieurs compteurs d’utilisation de fichier facultatif. Le programme d’installation permet du décompte d’utilisation de composant DLL mettre à jour les entrées de Registre. Il est stocké dans la valeur UsageCount sous le niveau principal ODBC, le pilote et les sous-clés de convertisseur. Pour le format de la valeur UsageCount et plus d’informations sur ces sous-clés, consultez [les entrées de Registre pour les composants ODBC](../../../odbc/reference/install/registry-entries-for-odbc-components.md).  
  
 Lors de l’installation d’un composant, le programme d’installation DLL crée une sous-clé pour celui-ci et les définit les données pour la valeur UsageCount dans cette sous-clé à 1. Lorsque le composant est installé à nouveau, le programme d’installation DLL incrémente le décompte d’utilisation. Lorsque le composant est supprimé, la décrémente DLL du programme d’installation le décompte d’utilisation. Si le nombre d’utilisations tombe à 0, le programme d’installation DLL supprime la sous-clé pour le composant.  
  
> [!CAUTION]  
>  Une application ne supprimez pas physiquement les fichiers du Gestionnaire de pilotes lorsque le nombre d’utilisations de composant et le nombre d’utilisations de fichier atteint zéro.  
  
 Déterminer les nombres doit réellement être copié ou d’un fichier supprimé en tant que l’utilisation du fichier et non pour incrémenter ou décrémenter le décompte d’utilisation. Ceci est important, car les composants ODBC et, par conséquent, les fichiers de composants ODBC, partagés et peuvent être installés ou supprimés par une variété d’applications. L’application peut supprimer les fichiers de pilote et de traduction si le nombre d’utilisations de composant et le nombre d’utilisations de fichier atteint zéro. Les fichiers du Gestionnaire de pilotes ne doit pas, être supprimés lorsque le nombre d’utilisations de composant et le nombre d’utilisations de fichier ont atteint zéro, car ces fichiers peuvent être utilisés par d’autres applications qui n’ont pas incrémenté le décompte d’utilisation de fichier.  
  
> [!NOTE]  
>  Nombre de fichiers d’utilisation est facultatifs dans Microsoft® WindowsNT®/Windows 2000.  
  
 Nombre de fichiers d’utilisation est conservées par le programme d’installation après avoir appelé **SQLInstallDriverManager**, **SQLInstallDriverEx**, **SQLInstallTranslatorEx**, **SQLRemoveDriverManager**, **SQLRemoveDriver**, ou **SQLRemoveTranslator**.  
  
 Lors de l’installation d’un composant, le programme d’installation ou le programme d’installation DLL crée une valeur sous la clé suivante pour chaque fichier dans ce composant n’est pas déjà sur le système :  
  
> [!NOTE]  
>  HKEY_LOCAL_MACHINE  
>   
>  LOGICIEL  
>   
>  Microsoft  
>   
>  Windows  
>   
>  CurrentVersion  
>   
>  SharedDlls  
  
 Il définit les données pour les valeurs 1 et copie le fichier dans le système. Lorsque le composant est installé à nouveau, le programme d’installation ou le programme d’installation DLL incrémente le nombre d’utilisations. Lorsque le composant est supprimé, le décrémente le programme d’installation programme d’installation ou de programme DLL la fréquence d’utilisation. Si n’importe quel nombre d’utilisations tombe à 0, le programme d’installation DLL ou le programme d’installation supprime la valeur pour le fichier et, si le composant est un pilote ou un traducteur, supprime le fichier. Les fichiers du Gestionnaire de pilotes ne doivent pas être supprimés.  
  
 Le format de la valeur de nombre de l’utilisation de fichier est illustré dans le tableau suivant.  
  
|Nom|Type de données|data|  
|----------|---------------|----------|  
|*chemin d’accès intégral*|REG_DWORD|*nombre*|  
  
 Par exemple, supposons qu’un pilote pour Informix utilise les fichiers Infrmx32.dll et Infrmx32.hlp et supposons que ce pilote a été installé à deux reprises. Les valeurs sous la sous-clé SharedDlls pour le pilote Informix serait comme suit :  
  
```  
C:\WINDOWS\SYSTEM32\INFRMX32.DLL : REG_DWORD : 0x2  
C:\WINDOWS\SYSTEM32\INFRMX32.HLP : REG_DWORD : 0x2  
```
