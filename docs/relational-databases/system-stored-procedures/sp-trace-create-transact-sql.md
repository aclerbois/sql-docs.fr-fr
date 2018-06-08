---
title: sp_trace_create (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_trace_create_TSQL
- sp_trace_create
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_create
ms.assetid: f3a43597-4c5a-4520-bcab-becdbbf81d2e
caps.latest.revision: 38
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 06588729794b9a5b62b82e0576f955536687f57e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptracecreate-transact-sql"></a>sp_trace_create (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une définition de trace. La nouvelle trace est à l'état arrêté.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des événements étendus.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_trace_create [ @traceid = ] trace_id OUTPUT   
          , [ @options = ] option_value   
          , [ @tracefile = ] 'trace_file'   
     [ , [ @maxfilesize = ] max_file_size ]  
     [ , [ @stoptime = ] 'stop_time' ]  
     [ , [ @filecount = ] 'max_rollover_files' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@traceid=** ] *trace_id*  
 Est le numéro attribué par [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la nouvelle trace. Toute entrée fournie par l'utilisateur est ignorée. *l’argument trace_id* est **int**, avec NULL comme valeur par défaut. L’utilisateur emploie le *trace_id* valeur pour identifier, modifier et contrôler la trace définie par cette procédure stockée.  
  
 [  **@options=** ] *argument option_value*  
 Spécifie les options définies pour la trace. *l’argument option_value* est **int**, sans valeur par défaut. Les utilisateurs peuvent choisir une combinaison de ces options en spécifiant la valeur totale des options choisies. Par exemple, pour activer les deux options TRACE_FILE_ROLLOVER et SHUTDOWN_ON_ERROR, spécifiez **6** pour *argument option_value*.  
  
 Le tableau suivant répertorie les options, leur description et leurs valeurs.  
  
|Nom d'option|Valeur d'option| Description|  
|-----------------|------------------|-----------------|  
|TRACE_FILE_ROLLOVER|**2**|Spécifie que lorsque la *max_file_size* est atteinte, l’actuel fichier de trace est fermé et un nouveau fichier est créé. Tous les nouveaux enregistrements sont écrits dans le nouveau fichier. Le nouveau fichier porte le même nom que l'ancien, mais un nombre entier est ajouté pour indiquer son ordre dans la séquence. Par exemple, si le fichier de trace d'origine se nomme filename.trc, le fichier de trace suivant se nommera filename_1.trc, le fichier de trace suivant se nommera, filename_2.trc, etc.<br /><br /> Au fur et à mesure que les fichiers de trace sont créés, la valeur du nombre entier ajouté au nom de fichier augmente de façon séquentielle.<br /><br /> SQL Server utilise la valeur par défaut *max_file_size* (5 Mo) si cette option est spécifiée sans spécifier de valeur pour *max_file_size*.|  
|SHUTDOWN_ON_ERROR|**4**|Indique que si la trace ne peut pas être écrite dans le fichier pour une raison quelconque, SQL Server doit s'arrêter. Cette option est utile lors de l'exécution de traces d'audit de sécurité.|  
|TRACE_PRODUCE_BLACKBOX|**8**|Indique que le serveur doit enregistrer les derniers 5 Mo d'informations de trace qu'il génère. TRACE_PRODUCE_BLACKBOX est incompatible avec toutes les autres options.|  
  
 [  **@tracefile=** ] *'**trace_file**'*  
 Indique l'emplacement et le nom du fichier dans lequel la trace doit être écrite. *trace_file* est **nvarchar(245)** sans valeur par défaut. *trace_file* peut être un répertoire local (par exemple, N 'C:\MSSQL\Trace\trace.trc') ou un chemin UNC vers un partage ou un chemin d’accès (N'\\\\*nom_serveur*\\*nom_partage* \\ *Active*\trace.trc').  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Permet d’ajouter un **.trc** extension à tous les noms de fichier de trace. Si l’option TRACE_FILE_ROLLOVER et *max_file_size* sont spécifiés, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un nouveau fichier de trace lorsque le fichier de trace d’origine a atteint sa taille maximale. Le nouveau fichier porte le même nom que le fichier d’origine, mais _*n* est ajouté pour indiquer son rang, en commençant par **1**. Par exemple, si le premier fichier de trace est nommé **filename.trc**, le deuxième fichier de trace est nommé **filename_1.trc**.  
  
 Si vous utilisez l'option TRACE_FILE_ROLLOVER, il est préférable d'éviter les traits de soulignement dans le nom du fichier initial. Si vous utilisez les traits de soulignement, vous obtenez :  
  
-   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] n’effectue pas automatiquement de charge, ou vous invite à charger les fichiers de substitution (si une de ces options de substitution de fichier est configurée).  
  
-   La fonction fn_trace_gettable ne charge pas les fichiers de substitution (selon le *number_files* argument) où le nom du fichier d’origine se termine avec un trait de soulignement et une valeur numérique. Cela ne s'applique pas au trait de soulignement et au nombre qui sont automatiquement ajoutés lors du remplacement d'un fichier.  
  
> [!NOTE]  
>  Une autre solution consiste à renommer les fichiers de trace pour supprimer les traits de soulignement dans le fichier d'origine. Par exemple, si le fichier d’origine se nomme **my_trace.trc**, et le fichier de substitution nommé **my_trace_1.trc**, vous pouvez renommer les fichiers **mytrace.trc** et **mytrace_1.trc** avant d’ouvrir les fichiers dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
 *trace_file* ne peut pas être spécifié lorsque l’option TRACE_PRODUCE_BLACKBOX est utilisée.  
  
 [  **@maxfilesize=** ] *max_file_size*  
 Indique la taille maximale en mégaoctets (Mo) d'un fichier de trace. *max_file_size* est **bigint**, avec la valeur par défaut **5**.  
  
 Si ce paramètre est spécifié sans l’option TRACE_FILE_ROLLOVER, la trace s’arrête l’enregistrement dans le fichier lorsque l’espace disque utilisé dépasse la quantité spécifiée par *max_file_size*.  
  
 [  **@stoptime=** ] **'***heure_fin***'**  
 Indique la date et l'heure d'arrêt de la trace. *heure_fin* est **datetime**, avec NULL comme valeur par défaut. Si la valeur est NULL, la trace continue de s'exécuter jusqu'à ce qu'elle soit arrêtée manuellement ou que le serveur s'arrête.  
  
 Si les deux *heure_fin* et *max_file_size* sont spécifiés, et TRACE_FILE_ROLLOVER n’est pas spécifié, la trace s’arrête lorsque l’heure d’arrêt ou une taille de fichier maximale est atteinte. Si *heure_fin*, *max_file_size*et TRACE_FILE_ROLLOVER sont spécifiés, la trace s’arrête à l’heure d’arrêt spécifiée, en supposant la trace ne remplit pas le lecteur.  
  
 [  **@filecount=** ] **'***max_rollover_files***'**  
 Spécifie le nombre maximal de fichiers de trace à conserver avec le même nom de fichier de base. *MAX_ROLLOVER_FILES* est **int**, supérieure à 1. Ce paramètre est valide uniquement si l'option TRACE_FILE_ROLLOVER est spécifiée. Lorsque *max_rollover_files* est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de limiter plus de *max_rollover_files* fichiers de trace en supprimant le fichier le plus ancien avant d’ouvrir un nouveau fichier de trace. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assure le suivi de l'ancienneté des fichiers de trace en ajoutant un numéro au nom de fichier de base.  
  
 Par exemple, lorsque le *trace_file* paramètre est spécifié en tant que « c:\mytrace », un fichier portant le nom « c:\mytrace_123.trc » est antérieur à un fichier portant le nom « c:\mytrace_124.trc ». Si *max_rollover_files* est défini sur 2, SQL Server supprime le fichier « c:\mytrace_123.trc » avant de créer le fichier de trace « c:\mytrace_125.trc ».  
  
 Notez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'effectue qu'une seule tentative de suppression pour chaque fichier et qu'il ne peut pas supprimer un fichier en cours d'utilisation par un autre processus. Par conséquent, si une autre application utilise des fichiers de trace tandis que la trace s'exécute, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut laisser ces fichiers dans le système de fichiers.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Le tableau suivant décrit les valeurs de code que les utilisateurs peuvent recevoir à la fin de l'exécution de la procédure stockée.  
  
|Code de retour| Description|  
|-----------------|-----------------|  
|0|Aucune erreur.|  
|1|Erreur inconnue.|  
|10|Options non valides. Renvoyé lorsque les options spécifiées sont incompatibles.|  
|12|Fichier non créé.|  
|13|Mémoire insuffisante. Renvoyé lorsqu'il n'y a pas assez de mémoire pour exécuter l'action spécifiée.|  
|14|Heure d'arrêt non valide. Renvoyé lorsque l'heure d'arrêt spécifiée est déjà dépassée.|  
|15|Paramètres incorrects. Renvoyé lorsque l'utilisateur a fourni des paramètres incompatibles.|  
  
## <a name="remarks"></a>Notes  
 **sp_trace_create** est un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procédure stockée qui effectue la plupart des actions précédemment exécutées par **xp_trace_\***  procédures stockées disponibles dans les versions antérieures de SQL Server. Utilisez **sp_trace_create** au lieu de :  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_setqueuecreateinfo**  
  
-   **xp_trace_setqueuedestination**  
  
 **sp_trace_create** crée uniquement une définition de trace. Cette procédure stockée ne peut pas être utilisée pour démarrer ou modifier une trace.  
  
 Les paramètres de Trace de SQL toutes les procédures stockées (**sp_trace_xx**) sont de type strict. Si ces paramètres ne sont pas appelés avec des types de données appropriés pour les paramètres d'entrée tels qu'ils sont spécifiés dans la description de l'argument, la procédure stockée renvoie une erreur.  
  
 Pour **sp_trace_create**, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service doit avoir l’autorisation d’écriture sur le dossier de fichiers de trace. Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service n’est pas un administrateur sur l’ordinateur où se trouve le fichier de trace, vous devez accorder explicitement des autorisations d’écriture pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service.  
  
> [!NOTE]  
>  Vous pouvez charger automatiquement le fichier de trace créé avec **sp_trace_create** dans une table à l’aide de la **fn_trace_gettable** fonction système. Pour plus d’informations sur l’utilisation de cette fonction système, consultez [sys.fn_trace_gettable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md).  
  
 Pour obtenir un exemple d’utilisation de procédures stockées de trace, consultez [Créer une trace &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
 **TRACE_PRODUCE_BLACKBOX** présente les caractéristiques suivantes :  
  
-   C'est une trace de substitution. La valeur par défaut *file_count* est 2 mais peut être remplacée par l’utilisateur à l’aide de *filecount* option.  
  
-   La valeur par défaut *file_size* comme avec d’autres traces est de 5 Mo et peut être modifiée.  
  
-   Aucun nom de fichier ne peut être spécifié. Le fichier sera enregistré sous : **N'%SQLDIR%\MSSQL\DATA\blackbox.trc'**  
  
-   Seuls les événements suivants et leurs colonnes sont contenus dans la trace :  
  
    -   **Démarrage de RPC**  
  
    -   **Démarrage de lot**  
  
    -   **Exception**  
  
    -   **Attention**  
  
-   Des événements ou des colonnes ne peuvent pas être ajoutés ou supprimés dans cette trace.  
  
-   Il n'est pas possible de spécifier des filtres pour cette trace.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation ALTER TRACE.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_generateevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [Trace SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
