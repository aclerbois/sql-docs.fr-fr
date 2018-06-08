---
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
ms.assetid: d949e540-9517-4bca-8117-ad8358848baa
caps.latest.revision: 47
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 82f3df3e82ae7be0c8df9e12e29afbffdb43f92d
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un groupe de charges de travail du gouverneur de ressources et l'associe à un pool de ressources du gouverneur de ressources. Resource Governor n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE WORKLOAD GROUP group_name  
[ WITH  
    ( [ IMPORTANCE = { LOW | MEDIUM | HIGH } ]  
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]  
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]  
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]   
      [ [ , ] MAX_DOP = value ]  
      [ [ , ] GROUP_MAX_REQUESTS = value ] )  
 ]  
[ USING {   
    [ pool_name | "default" ]    
    [ [ , ] EXTERNAL external_pool_name | "default" ] ]   
    } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 *group_name*  
 Nom défini par l'utilisateur pour le groupe de charges de travail. *group_name* est alphanumérique, peut contenir jusqu’à 128 caractères, doit être unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 IMPORTANCE = { LOW | **MEDIUM** | HIGH }  
 Spécifie l'importance relative d'une demande dans le groupe de charges de travail. Le paramètre Importance peut avoir les valeurs suivantes, MEDIUM étant la valeur par défaut :  
  
-   LOW  
-   MEDIUM (valeur par défaut)    
-   HIGH  
  
> [!NOTE]  
> En interne, chaque paramètre d'importance est stocké sous la forme d'un nombre utilisé pour les calculs.  
  
 Le paramètre IMPORTANCE est local par rapport au pool de ressources : les groupes de charges de travail d'importance différente à l'intérieur du même pool de ressources s'affectent mutuellement, mais n'affectent pas les groupes de charges de travail dans un autre pool de ressources.  
  
 REQUEST_MAX_MEMORY_GRANT_PERCENT =*value*  
 Spécifie la quantité de mémoire maximale qu'une requête unique peut prendre du pool. Ce pourcentage est relatif à la taille du pool de ressources spécifiée par MAX_MEMORY_PERCENT.  
  
> [!NOTE]  
> La quantité spécifiée fait uniquement référence à la mémoire allouée à l'exécution de la requête.  
  
 *value* doit être égal à 0 ou un entier positif. La plage autorisée pour *value* est comprise entre 0 et 100. La valeur par défaut de *value* est 25.  
  
 Notez les points suivants :  
  
-   L’affectation de la valeur 0 à *value* empêche l’exécution de requêtes avec les opérations SORT et HASH JOIN dans les groupes de charges de travail définis par l’utilisateur.  
  
-   Il est déconseillé d’affecter à *value* une valeur supérieure à 70, car le serveur risque de ne pas pouvoir mettre de côté suffisamment de mémoire disponible si d’autres requêtes simultanées s’exécutent. Cela risque de provoquer l'erreur de délai d'attente de requête 8645.  
  
> [!NOTE]  
>  Si les besoins en mémoire de la requête dépassent la limite spécifiée par ce paramètre, le serveur effectue les opérations suivantes :  
>   
>  Pour les groupes de charges de travail définis par l'utilisateur, le serveur essaie de réduire le degré de parallélisme de la requête jusqu'à ce que ses besoins en mémoire tombent sous la limite ou jusqu'à ce que le degré de parallélisme soit égal à 1. Si les besoins en mémoire de la requête sont encore supérieurs à la limite, l'erreur 8657 se produit.  
>   
>  Pour les groupes de charges de travail internes et par défaut, le serveur autorise la requête à obtenir la mémoire requise.  
>   
>  Sachez toutefois que dans les deux cas, l'erreur de délai d'attente 8645 se produit si le serveur dispose d'une mémoire physique insuffisante.  
  
 REQUEST_MAX_CPU_TIME_SEC =*value*  
 Spécifie la quantité maximale de temps processeur, en secondes, qu'une demande peut utiliser. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value* est 0, ce qui signifie illimité.  
  
> [!NOTE]  
> Par défaut, Resource Governor n’empêche pas une demande de continuer si le temps maximal est dépassé. Toutefois, un événement sera généré. Pour plus d’informations, consultez [Classe d’événements CPU Threshold Exceeded](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).  

> [!IMPORTANT]
> À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]SP2 et de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3, quand [l’indicateur de trace 2422](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) est utilisé, Resource Governor abandonne une demande en cas de dépassement de la durée maximale. 
  
 REQUEST_MEMORY_GRANT_TIMEOUT_SEC =*value*  
 Spécifie la durée maximale, en secondes, pendant laquelle une requête peut attendre qu’une allocation de mémoire (mémoire tampon de travail) devienne disponible.  
  
> [!NOTE]  
>  Une requête n'échoue pas toujours lorsque le délai d'expiration d'allocation mémoire est atteint. Une requête échoue seulement si le nombre de requêtes exécutées simultanément est trop élevé. Autrement, la requête risque d'obtenir uniquement l'allocation mémoire minimale, d'où une dégradation des performances.  
  
 *value* doit être égal à 0 ou un entier positif. Le paramètre par défaut de *value*, 0, utilise un calcul interne basé sur le coût de requête pour déterminer le délai maximal.  
  
 MAX_DOP =*value*  
 Spécifie le degré maximal de parallélisme (DOP) pour les demandes parallèles. *value* doit être égal à 0 ou un entier positif. La plage autorisée pour *value* est comprise entre 0 et 64. Le paramètre par défaut de *value*, 0, utilise le paramètre global. MAX_DOP est géré comme suit :  
  
-   MAX_DOP en tant qu'indicateur de requête est effectif tant qu'il ne dépasse pas le groupe de charges de travail MAX_DOP. Si la valeur d'indicateur de requête MAXDOP dépasse la valeur configurée avec le gouverneur de ressources, le moteur de base de données utilise la valeur MAXDOP du gouverneur de ressources.  
  
-   MAX_DOP en tant qu'indicateur de requête remplace toujours l'option « max degree of parallelism » de sp_configure.  
  
-   Le groupe de charge de travail MAX_DOP remplace le degré maximal de parallélisme sp_configure.  
  
-   Si la requête est marquée comme étant en série au moment de la compilation, elle ne peut être reconvertie en requête parallèle au moment de l'exécution, indépendamment du groupe de charge de travail ou du paramètre sp_configure.  
  
-   Une fois le degré maximal de parallélisme (DOP) configuré, il ne peut être diminué que sous la sollicitation de l'allocation de mémoire. La reconfiguration du groupe de charges de travail n'est pas visible lors de l'attente dans la file d'attente d'allocation de mémoire.  
  
 GROUP_MAX_REQUESTS =*value*  
 Spécifie le nombre maximal de demandes simultanées autorisées à s'exécuter dans le groupe de charges de travail. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value*, 0, autorise un nombre illimité de demandes. Lorsque le nombre maximal de requêtes est atteint, un utilisateur de ce groupe peut se connecter, mais est placé dans un état d'attente jusqu'à ce que le nombre de requêtes simultanées soit inférieur à la valeur spécifiée.  
  
 USING { *pool_name* | **"default"** }  
 Associe le groupe de charge de travail au pool de ressources défini par l’utilisateur identifié par *pool_name*. Cette opération revient en fait à placer le groupe de charges de travail dans le pool de ressources. Si *pool_name* n’est pas fourni ou si l’argument USING n’est pas utilisé, le groupe de charge de travail est placé dans le pool par défaut Resource Governor prédéfini.  
  
 "default" est un mot réservé et doit être placé entre des guillemets doubles ("") ou des crochets ([]) lorsqu'il est utilisé avec l'argument USING.  
  
> [!NOTE]  
>  Les groupes de charges de travail et les pools de ressources prédéfinis utilisent tous des noms minuscules, tels que "default". Ce facteur doit être pris en considération pour les serveurs qui utilisent un classement qui respecte la casse. Les serveurs avec un classement qui ne respecte pas la casse, tel que SQL_Latin1_General_CP1_CI_AS, traitent "default" et "Default" comme identiques.  
  
 EXTERNAL external_pool_name | “default“  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).  
  
 Le groupe de charge de travail peut spécifier un pool de ressources externes. Vous pouvez définir un groupe de charge de travail et l’associer à 2 pools :  
  
-   Un pool de ressources pour les charges de travail et les requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Un pool de ressources externes pour les processus externes. Pour plus d’informations, consultez [sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).  
  
## <a name="remarks"></a>Notes   
 REQUEST_MEMORY_GRANT_PERCENT : la création d'index est autorisée à utiliser une mémoire d'espace de travail supérieure à celle qui lui a été initialement allouée, afin d'améliorer les performances. Cette gestion spéciale est prise en charge par le gouverneur de ressources dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Toutefois, l'allocation initiale et toute allocation de mémoire supplémentaire sont limitées par les paramètres du pool de ressources et du groupe de charges de travail.  
  
 **Création d’un index sur une table partitionnée**  
  
 La mémoire consommée par la création d'index sur une table partitionnée non alignée est proportionnelle au nombre de partitions impliquées. Si la mémoire totale requise dépasse la limite par requête (REQUEST_MAX_MEMORY_GRANT_PERCENT) imposée par le paramètre du groupe de charges de travail du Gouverneur de ressources, cette création d'index peut ne pas s'exécuter. Étant donné que le groupe de charges de travail « default » permet à une requête de dépasser la limite par requête avec la mémoire minimale, l'utilisateur peut être en mesure d'exécuter la même création d'index dans le groupe de charges de travail « default », si le pool de ressources « default » possède assez de mémoire totale configurée pour exécuter cette requête.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment créer un groupe de charges de travail appelé `newReports`. Ce pool utilise les paramètres par défaut du gouverneur de ressources et se trouve dans le pool par défaut du gouverneur de ressources. L'exemple spécifie le pool `default`, mais cela n'est pas obligatoire.  
  
```  
CREATE WORKLOAD GROUP newReports  
    USING "default" ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  

