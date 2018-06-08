---
title: Création d’étendues de procédures stockées | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: extended-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- warnings [SQL Server]
- extended stored procedures [SQL Server], debugging
- extended stored procedures [SQL Server], creating
- messages [SQL Server], extended stored procedures
ms.assetid: 9f7c0cdb-6d88-44c0-b049-29953ae75717
caps.latest.revision: 38
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 21d29aa0ceb7ba16216db3f52e18379f55b775dd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-extended-stored-procedures"></a>Création de procédures stockées étendues
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Une procédure stockée étendue est une fonction avec un prototype :  
  
 SRVRETCODE *xp_extendedProcName* **(** SRVPROC  **\*) ;**  
  
 L'utilisation du préfixe xp_ est facultative. Les noms de procédures stockées étendues respectent la casse lorsqu'ils sont référencés dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], indépendamment de la page de codes/de l'ordre de tri sur le serveur. Lorsque vous générez une DLL :  
  
-   Si un point d'entrée est nécessaire, écrivez une fonction DllMain.  
  
     Cette fonction est facultative ; si vous ne la fournissez pas dans le code source, le compilateur lie sa propre version, qui ne fait rien d'autre que de retourner TRUE. Si vous fournissez une fonction DllMain, le système d'exploitation appelle cette fonction lorsqu'un thread ou un processus est attaché à la DLL ou détaché de cette dernière.  
  
-   Toutes les fonctions appelées hors de la DLL (toutes les procédures stockées étendues Efunctions) doivent être exportées.  
  
     Vous pouvez exporter une fonction en répertoriant son nom dans la section EXPORTS d’un fichier .def, ou vous pouvez préfixer le nom de fonction dans le code source avec __declspec (dllexport), une extension du compilateur Microsoft (Notez que \__declspec() commence par deux traits de soulignement).  
  
 Ces fichiers sont requis pour la création d'une DLL de procédure stockée étendue.  
  
|Fichier| Description|  
|----------|-----------------|  
|Srv.h|Fichier d'en-tête de l'API de procédure stockée étendue|  
|Opends60.lib|Bibliothèque d'importation pour Opends60.dll|  
  
 Pour créer une DLL de procédure stockée étendue, créez un projet de type bibliothèque de liens dynamiques. Pour plus d'informations sur la création d'une DLL, consultez la documentation relative à l'environnement de développement.  
  
 Il est fortement recommandé que toutes les DLL de procédures stockées étendues implémentent et exportent la fonction suivante :  
  
```  
__declspec(dllexport) ULONG __GetXpVersion()  
{  
   return ODS_VERSION;  
}  
```  
  
> [!NOTE]  
>  __declspec(dllexport) est une extension du compilateur spécifique à Microsoft. Si votre compilateur ne prend pas en charge cette directive, vous devez exporter cette fonction dans votre fichier DEF sous la section EXPORTS.  
  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est démarré avec la trace flag - T260 ou si un utilisateur disposant des privilèges d’administrateur système exécute DBCC TRACEON (260), et si la stockée étendue DLL de procédure ne prend pas en charge __GetXpVersion(), un message d’avertissement (erreur 8131 : procédure stockée étendue DLL '%' n’exporte pas \__GetXpVersion().) est écrit dans le journal des erreurs. (Notez que \__GetXpVersion() commence par deux traits de soulignement.)  
  
 Si la DLL de procédure stockée étendue exporte __GetXpVersion(), mais que la version retournée par la fonction est antérieure à celle requise par le serveur, un message d'avertissement indiquant la version retournée par la fonction et la version attendue par le serveur est écrit dans le journal des erreurs. Si vous obtenez ce message, vous retournez une valeur incorrecte de \__GetXpVersion(), ou si vous compilez avec une version antérieure de srv.h.  
  
> [!NOTE]  
>  SetErrorMode (fonction [!INCLUDE[msCoName](../../includes/msconame-md.md)] Win32) ne doit pas être appelé dans les procédures stockées étendues.  
  
 Il est recommandé que toute procédure stockée étendue dont l'exécution est longue, appelle périodiquement srv_got_attention afin que la procédure puisse se terminer si la connexion est arrêtée ou si le lot est abandonné.  
  
 Pour déboguer une DLL de procédure stockée étendue, copiez-la vers le répertoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Binn. Pour spécifier le fichier exécutable pour la session de débogage, entrez le chemin d’accès et le nom de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le fichier exécutable (par exemple, C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\MSSQL\Binn\Sqlservr.exe). Pour plus d’informations sur les arguments sqlservr, consultez [Application sqlservr](../../tools/sqlservr-application.md).  
  
## <a name="see-also"></a>Voir aussi  
 [srv_got_attention &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-got-attention-extended-stored-procedure-api.md)  
  
  
