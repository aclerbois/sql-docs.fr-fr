---
title: Limiter les tailles de fichier et de table de trace | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: sql-trace
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- maximum file size for traces
- traces [SQL Server], file size
- size [SQL Server], tables
- file rollover option [SQL Server]
- size [SQL Server], files
ms.assetid: 88c31b02-f44c-4a14-be8b-437f2097de12
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 48134da68c8125408b2616ca6298b78ab0aa3ba2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="limit-trace-file-and-table-sizes"></a>Limiter les tailles de fichier et de table de trace
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les résultats de Trace SQL ont des tailles variables selon les classes d’événements incluses dans la trace et leur mode d’utilisation par [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Si vous tracez des classes d'événements qui se produisent fréquemment, vous pouvez minimiser la quantité de données collectées par la trace en définissant la taille de fichier maximale ou le nombre maximal de lignes. La spécification d'une taille de fichier maximale et/ou d'un nombre maximal de lignes empêche le fichier de trace ou la table de trace de croître au-delà de la limite spécifiée.  
  
> [!NOTE]  
>  Si vous enregistrez les données de trace dans un fichier existant, vous pouvez ajouter les données au fichier ou écraser le fichier. Si vous choisissez l'ajout au fichier et que la taille du fichier de trace est déjà supérieure ou égale à la taille de fichier maximale spécifiée, vous en êtes averti et pouvez alors augmenter la taille maximale du fichier ou indiquer un autre fichier. Il en est de même pour les tables de trace.  
  
## <a name="maximum-file-size"></a>Taille de fichier maximale  
 Une trace pour laquelle la taille de fichier maximale est spécifiée arrête d'enregistrer les informations de trace dans le fichier dès que cette taille est atteinte. Cette option permet de grouper les événements en fichiers plus petits, plus gérables. Elle permet en outre d'améliorer la fiabilité des traces sans assistance car la trace s'arrête dès que la taille de fichier maximale est atteinte. Vous pouvez définir la taille de fichier maximale des traces créées au moyen de procédures stockées Transact-SQL ou à l'aide du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
 L'option de taille de fichier maximale est limitée à 1 gigaoctet (Go). La taille de fichier maximale par défaut est de 5 mégaoctets (Mo).  
  
### <a name="enabling-file-rollover"></a>Activation de la substitution de fichier  
 L'option de substitution de fichier entraîne la fermeture par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du fichier actif et la création d'un nouveau fichier lorsque la taille de fichier maximale est atteinte. Le nouveau fichier porte le même nom que l'ancien mais un nombre entier est ajouté au nom pour indiquer son rang. Par exemple, si le fichier de trace d'origine se nomme nomfichier_1.trc, le fichier de trace suivant est nomfichier_2.trc, etc. Si le nom affecté à un nouveau fichier de substitution est déjà utilisé par un fichier existant, ce dernier est remplacé sauf s'il est en lecture seule. L’option de substitution de fichier est activée par défaut lors de l’enregistrement de données de trace dans un fichier.  
  
> [!NOTE]  
>  Lorsque l’option de substitution de fichier est activée, la trace se poursuit jusqu'à ce qu'elle soit arrêtée par un autre moyen. Pour arrêter la trace lorsque la taille de fichier maximale est atteinte, vous devez désactiver l’option de substitution de fichier.  
  
 **Pour définir une taille maximale de fichier de trace**  
  
 [Définir la taille maximale d’un fichier de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-a-maximum-file-size-for-a-trace-file-sql-server-profiler.md)  
  
## <a name="maximum-number-of-rows"></a>Nombre maximal de lignes  
 Une trace pour laquelle un nombre maximal de lignes est défini arrête d'enregistrer les informations de trace dans une table dès que le nombre maximal de lignes est atteint. Comme chaque événement correspond à une ligne, ce paramètre définit une limite quant au nombre d'événements pouvant être collectés. La définition du nombre maximal de lignes simplifie l'exécution de traces sans surveillance. Par exemple, vous pouvez automatiquement démarrer une trace qui enregistre les données de trace dans une table et s'arrête si le fichier devient trop volumineux.  
  
 Lorsque le nombre maximal de lignes est spécifié et que cette valeur a été atteinte, la trace se poursuit tant que [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] fonctionne, mais les informations de trace ne sont plus enregistrées. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] continue à afficher les résultats de la trace jusqu’à ce que celle-ci s’arrête.  
  
 **Pour définir un nombre maximal de lignes pour une trace**  
  
 [Définir la taille maximale d’une table de trace &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-a-maximum-table-size-for-a-trace-table-sql-server-profiler.md)  
  
## <a name="see-also"></a> Voir aussi  
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)  
  
  
