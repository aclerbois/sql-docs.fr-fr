---
title: Niveaux de gravité des erreurs du moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- user-defined error messages [SQL Server]
- severity levels [SQL Server]
- retrieving error severity
- errors [SQL Server], severity
- TRY...CATCH [SQL Server]
ms.assetid: 3e7f5925-6edd-42e1-bf17-f7deb03993a7
caps.latest.revision: 21
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7d5c5a6ddee7f8d5e9b734651fa7722df56349db
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="database-engine-error-severities"></a>Niveaux de gravité des erreurs du moteur de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lorsqu'une erreur est déclenchée par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], la gravité de l'erreur indique le type de problème rencontré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="levels-of-severity"></a>Niveaux de gravité  
 Le tableau suivant répertorie et décrit les niveaux de gravité des erreurs déclenchées par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
|Niveau de gravité|Description|  
|--------------------|-----------------|  
|0-9|Messages qui retournent des informations d'état ou qui signalent des erreurs sans gravité. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne déclenche pas d'erreurs système dont les niveaux de gravité sont compris entre 0 et 9.|  
|10|Messages qui retournent des informations d'état ou qui signalent des erreurs sans gravité. Pour des raisons de compatibilité, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] convertit le niveau de gravité 10 en 0 avant de retourner les informations d'erreur à l'application appelante.|  
|11-16|Indique les erreurs pouvant être corrigées par l'utilisateur.|  
|11|Indique que l'objet ou l'entité spécifique n'existe pas.|  
|12|Niveau de gravité particulier pour les requêtes qui n'utilisent pas de verrouillage en raison d'indicateurs de requête spéciaux. Dans certains cas, les opérations de lecture effectuées par ces instructions peuvent produire des données incohérentes, car les verrouillages ne sont pas effectués pour garantir la cohérence.|  
|13|Indique des erreurs de blocage de transaction.|  
|14|Indique des erreurs liées à la sécurité, par exemple le refus d'une autorisation.|  
|15|Indique des erreurs de syntaxe dans la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|16|Indique des erreurs générales qui peuvent être corrigées par l'utilisateur.|  
|17-19|Indique des erreurs logicielles qui ne peuvent pas être corrigées par l'utilisateur. Informez votre administrateur système de l'existence du problème.|  
|17|Indique que l'instruction a obligé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à s'exécuter avec des ressources insuffisantes (mémoire, verrous ou espace disque de la base de données) ou à dépasser une limite fixée par l'administrateur système.|  
|18|Indique l'existence d'un problème logiciel avec le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ; toutefois, l'instruction s'exécute intégralement et la connexion à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est conservée. L'administrateur système doit être informé chaque fois qu'un message s'affiche avec le niveau de gravité 18.|  
|19|Indique qu'une limite non configurable du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est dépassée et que le traitement est terminé. Les messages d'erreur dont le niveau de gravité est supérieur ou égal à 19 arrêtent l'exécution du traitement actuel. Les erreurs dont le niveau de gravité est 19 se produisent rarement ; elles doivent être résolues par l'administrateur système ou votre support technique. Contactez votre administrateur système en cas de déclenchement d'un message dont le niveau de gravité est 19. Les messages d’erreur dont le niveau de gravité est compris entre 19 et 25 sont inscrits dans le journal des erreurs.|  
|20-24|Indique l'existence de problèmes système et d'erreurs irrécupérables, ce qui signifie que la tâche du [!INCLUDE[ssDE](../../includes/ssde-md.md)] exécutant une instruction ou un traitement n'est plus en cours d'exécution. La tâche enregistre des informations sur ce qui s'est produit, puis se termine. Dans la plupart des cas, la connexion de l'application à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] se termine également. Si cela se produit, selon la nature du problème, l'application risque de ne pas pouvoir se reconnecter.<br /><br /> Les messages d'erreur de ce niveau peuvent affecter tous les processus qui accèdent aux données de la même base de données et indiquer la détérioration d'une base de données ou d'un objet. Les messages d'erreur dont le niveau de gravité est compris entre 19 et 24 sont inscrits dans le journal des erreurs.|  
|20|Indique qu'une instruction a rencontré un problème. Dans la mesure où le problème affecte uniquement la tâche actuelle, il est improbable que la base de données elle-même soit endommagée.|  
|21|Indique qu'un problème a été rencontré et qu'il affecte toutes les tâches de la base de données actuelle ; toutefois, il est improbable que la base de données elle-même soit endommagée.|  
|22|Indique que la table ou l'index spécifié dans le message a été endommagé à la suite d'un problème logiciel ou matériel.<br /><br /> Les erreurs dont le niveau de gravité est 22 se produisent rarement. Si ce type d'erreur se produit, exécutez DBCC CHECKDB pour déterminer si d'autres objets de la base de données sont également endommagés. Il est possible que le problème provienne uniquement du cache des tampons et non du disque lui-même. Dans ce cas, le redémarrage de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] permet de corriger le problème. Pour pouvoir continuer à travailler, vous devez vous reconnecter à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]; dans le cas contraire, utilisez DBCC pour corriger le problème. Dans certains cas, vous pouvez être amené à restaurer la base de données.<br /><br /> Si le redémarrage de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne permet pas de corriger le problème, cela signifie que ce dernier provient du disque. Dans certains cas, il peut être résolu en détruisant l'objet spécifié dans le message d'erreur. Par exemple, si le message indique que l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] a trouvé une ligne de longueur 0 dans un index non-cluster, supprimez l'index et reconstruisez-le.|  
|23|Indique que l'intégrité de la totalité de la base de données est douteuse en raison d'un problème matériel ou logiciel.<br /><br /> Les erreurs dont le niveau de gravité est 23 se produisent rarement. Si ce type d'erreur se produit, exécutez DBCC CHECKDB pour déterminer l'étendue des dommages. Il est possible que le problème provienne uniquement du cache et non du disque lui-même. Dans ce cas, le redémarrage de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] permet de corriger le problème. Pour pouvoir continuer à travailler, vous devez vous reconnecter à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]; dans le cas contraire, utilisez DBCC pour corriger le problème. Dans certains cas, vous pouvez être amené à restaurer la base de données.|  
|24|Indique une défaillance du support. L'administrateur système peut être obligé de restaurer la base de données. Vous pouvez également être amené à contacter le fournisseur de votre matériel.|  
  
## <a name="user-defined-error-message-severity"></a>Niveau de gravité des messages d'erreur définis par l'utilisateur  
 **sp_addmessage** permet d’ajouter à l’affichage catalogue **sys.messages** des messages d’erreur définis par l’utilisateur et dont les niveaux de gravité vont de 1 à 25. Ces messages d'erreur définis par l'utilisateur peuvent être utilisés par RAISERROR. Pour plus d’informations, consultez [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md).  
  
 RAISERROR permet de générer des messages d'erreur définis par l'utilisateur et dont les niveaux de gravité vont de 1 à 25. RAISERROR peut faire référence à un message d’erreur défini par l’utilisateur et stocké dans l’affichage catalogue **sys.messages** ou générer un message de manière dynamique. Lors de l’utilisation du message d’erreur défini par l’utilisateur dans **sys.messages** au cours de la génération d’une erreur, le niveau de gravité spécifié par RAISERROR remplace celui spécifié dans **sys.messages**. Pour plus d’informations, consultez [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md).  
  
## <a name="error-severity-and-trycatch"></a>Gravité d'erreur et TRY…CATCH  
 Une construction TRY…CATCH intercepte toutes les erreurs d'exécution dont le niveau de gravité est supérieur à 10 et qui ne mettent pas fin à la connexion de base de données.  
  
 Les erreurs dont le niveau de gravité est compris entre 0 et 10 sont des messages d'information. Elles ne provoquent pas de saut d'exécution du bloc CATCH d'une construction TRY…CATCH.  
  
 Les erreurs (dont le niveau est généralement compris entre 20 et 25) qui mettent fin à la connexion de base de données ne sont pas gérées par le bloc CATCH, car l'exécution est annulée lorsque la connexion est terminée.  
  
 Pour plus d’informations, consultez [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md).  
  
## <a name="retrieving-error-severity"></a>extraction de la gravité des erreurs  
 La fonction système ERROR_SEVERITY permet de récupérer le niveau de gravité de l'erreur qui a causé l'exécution du bloc CATCH d'une construction TRY…CATCH. ERROR_SEVERITY retourne NULL s'il est appelé en dehors de la portée d'un bloc CATCH. Pour plus d’informations, consultez [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Présentation des erreurs du moteur de base de données](../../relational-databases/errors-events/understanding-database-engine-errors.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  
