---
title: Générer de fichiers de vidage pour l’exécution des packages | Microsoft Docs
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 61ef1731-cb3a-4afb-b4a4-059b04aeade0
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1053f9e1714ac5003f02ac66ff19e148660d918b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="generating-dump-files-for-package-execution"></a>Générer de fichiers de vidage pour l'exécution des packages
  Dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouvez créer des fichiers de vidage du débogage qui fourniront des informations sur l'exécution d'un package. Les informations contenues dans ces fichiers peuvent vous aider à résoudre des problèmes d’exécution du package.  
  
> **REMARQUE !** Les fichiers de vidage de débogage peuvent contenir des informations sensibles. Pour protéger les informations sensibles, vous pouvez utiliser une liste de contrôle d'accès (ACL) afin de restreindre l'accès aux fichiers ou copier les fichiers dans un dossier avec accès limité. Par exemple, nous vous recommandons de supprimer toutes les informations sensibles ou confidentielles avant d'envoyer vos fichiers de débogage aux services de support technique de [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 Lorsque vous déployez un projet sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous pouvez créer des fichiers de vidage qui fournissent des informations sur l'exécution des packages contenus dans le projet. Lorsque le processus ISServerExec.exe se termine, les fichiers de vidage sont créés. Vous pouvez spécifier qu'un fichier de vidage doit être créé lorsque des erreurs se produisent durant l'exécution du package. Pour ce faire, sélectionnez l'option **Vider en cas d'erreurs** dans la boîte de dialogue **Exécuter le package** . Vous pouvez également utiliser les procédures stockées suivantes :  
  
-   [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
  
     Appelez cette procédure stockée pour configurer un fichier de vidage à créer lorsqu'une erreur ou un événement se produit, et lorsque des événements spécifiques se produisent, au cours de l'exécution d'un package.  
  
-   [catalog.create_execution_dump](../../integration-services/system-stored-procedures/catalog-create-execution-dump.md)  
  
     Appelez cette procédure stockée pour obliger un package à interrompre son exécution et créer un fichier de vidage.  
  
 Si vous déployez des packages à l’aide du modèle de déploiement de package, vous créez les fichiers de vidage du débogage à l’aide de l’utilitaire **dtexec** ou de l’utilitaire **dtutil** pour spécifier une option de vidage du débogage sur la ligne de commande. Pour plus d'informations, consultez [dtexec Utility](../../integration-services/packages/dtexec-utility.md) et [dtutil Utility](../../integration-services/dtutil-utility.md). Pour plus d’informations sur les modèles de déploiement de package, consultez [Déploiement de projets et de packages Integration Services (SSIS)](https://msdn.microsoft.com/library/hh213290.aspx) et [Déploiement de packages hérités &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).   
  
## <a name="debug-dump-file-format"></a>Format des fichiers de vidage du débogage  
 Lorsque vous spécifiez une option de vidage de débogage, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée les fichiers de vidage de débogage suivants :  
  
-   Un fichier de vidage de débogage .mdmp. Il s'agit d'un fichier binaire.  
  
-   Fichier de vidage de débogage .tmp. Il s'agit d'un fichier au format texte.  
  
 Par défaut, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stocke ces fichiers dans le dossier *\<lecteur>:* \Program Files\Microsoft SQL Server\110\Shared\ErrorDumps.  
  
 Le tableau ci-dessous décrit uniquement certaines sections du fichier .tmp. Le fichier .tmp inclut des données supplémentaires qui ne sont pas répertoriées dans ce tableau.  
  
|Type d'informations|Description| Exemple|  
|-------------------------|-----------------|-------------|  
|Environnement|Version de système d'exploitation, données d'utilisation de la mémoire, ID de processus et nom d'image de processus. Les informations d'environnement se trouvent au début du fichier .tmp.|# Vidage texte SSIS effectué le 13/09/2007 à 13:50:34<br /><br /> # PID 4120<br /><br /> #Nom de l'image [C:\Program Files\Microsoft SQL Server\110\DTS\Binn\DTExec.exe]<br /><br /> # OS major=6 minor=0 build=6000<br /><br /> # Exécution sur 2 processeurs amd64 sous WOW64<br /><br /> # Mémoire : 58 % utilisé. Physical: 845M/2044M  Paging: 2404M/4095M (avail/total)|  
|Chemin d'accès et version des bibliothèques de liens dynamiques (DLL)|Chemin d'accès et numéro de version de chaque DLL que le système charge pendant le traitement d'un package.|# Module chargé : c:\bb\Sql\DTS\src\bin\debug\i386\DTExec.exe (10.0.1069.5)<br /><br /> # Module chargé : C:\Windows\SysWOW64\ntdll.dll (6.0.6000.16386)<br /><br /> # Module chargé : C:\Windows\syswow64\kernel32.dll (6.0.6000.16386)|  
|Messages récents|Messages récents émis par le système. Inclut l'heure, le type, la description et l'ID de thread de chaque message.|[M:1]   Ring buffer entry:              (*pRecord)<br /><br /> [D:2]      <<\<CRingBufferLogging::RingBufferLoggingRecord>>> ( @ 0282F1A8 )<br /><br /> [E:3]         Time Stamp: 2007-09-13 13:50:32.786      (szTimeStamp)<br /><br /> [E:3]         Thread ID: 2368           (ThreadID)<br /><br /> [E:3]         Event Name: OnError                        (EventName)<br /><br /> [E:3]         Source Name:                (SourceName)<br /><br /> [E:3]         Source ID:                        (SourceID)<br /><br /> [E:3]         Execution ID:                 (ExecutionGUID)<br /><br /> [E:3]         Data Code: -1073446879              (DataCode)<br /><br /> [E:3]         Description : Le composant est manquant, n’est pas enregistré, ne peut pas être mis à niveau ou des interfaces obligatoires sont manquantes. Informations de contact de ce composant : «  ».|  
  
## <a name="related-information"></a>Informations connexes  
 [Boîte de dialogue d'exécution de package](../../integration-services/packages/run-integration-services-ssis-packages.md#execute_package_dialog)  
  
  
