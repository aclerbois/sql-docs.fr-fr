---
title: Filtres de lignes paramétrés | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], dynamic filters
- merge replication [SQL Server replication], dynamic filters
- parameterized filters [SQL Server replication]
- filters [SQL Server replication], dynamic
- merge replication parameterized filters [SQL Server replication]
- publications [SQL Server replication], parameterized filters
- parameterized filters [SQL Server replication], about parameterized filters
- filters [SQL Server replication], parameterized
- dynamic filters [SQL Server replication]
ms.assetid: b48a6825-068f-47c8-afdc-c83540da4639
caps.latest.revision: 69
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7cf73ea957e5a918618b1172b65a905439831689
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameterized-filters---parameterized-row-filters"></a>Filtres paramétrés - Filtres de lignes paramétrés
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les filtres de lignes paramétrables permettent l'envoi de différentes partitions de données à divers Abonnés sans qu'il soit nécessaire de créer plusieurs publications (les filtres paramétrés étaient précédemment désignés par le terme « filtres dynamiques » dans les versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]). Une partition est un sous-ensemble de lignes d'une table ; selon les paramètres sélectionnés lors de la création d'un filtre de lignes paramétrable, chaque ligne d'une table publiée peut appartenir à une seule partition (ce qui donne des partitions qui ne se chevauchent pas), ou à deux ou plusieurs partitions (auquel cas, elles se chevauchent).  
  
 Les partitions qui ne se chevauchent pas peuvent être partagées entre plusieurs abonnements ou limitées à un seul d'entre eux. Les paramètres qui contrôlent le comportement de la partition sont décrits dans la section « Utilisation des options de filtrage appropriées » plus loin dans cette rubrique. Avec ces paramètres, vous pouvez adapter le filtrage paramétré aux besoins des applications et de performance. En général, les partitions qui se chevauchent offrent davantage de souplesse, tandis que les partitions qui ne se chevauchent pas et sont répliquées sur un seul abonnement offrent de meilleures performances.  
  
 Les filtres paramétrés sont utilisés sur une seule table et généralement combinés à des filtres de jointure afin d'étendre le filtrage aux tables connexes. Pour plus d’informations, voir [Join Filters](../../../relational-databases/replication/merge/join-filters.md).  
  
 Pour définir ou modifier un filtre de lignes paramétré, consultez [Define and Modify a Parameterized Row Filter for a Merge Article](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
## <a name="how-parameterized-filters-work"></a>Fonctionnement des filtres paramétrés  
 Un filtre de lignes paramétré utilise une clause WHERE pour sélectionner les données appropriées à publier. Plutôt que de spécifier une valeur littérale dans la clause (comme dans un filtre de lignes statique), vous spécifiez l'une des deux fonctions système suivantes ou les deux : SUSER_SNAME() et HOST_NAME(). Il est également possible d'utiliser des fonctions définies par l'utilisateur mais elles doivent inclure SUSER_SNAME() ou HOST_NAME() dans le corps de la fonction ou évaluer l'une de ces fonctions système (par exemple, `MyUDF(SUSER_SNAME()`). Si une fonction définie par l'utilisateur inclut SUSER_SNAME() ou HOST_NAME() dans le corps de la fonction, vous ne pouvez pas passer de paramètres à la fonction.  
  
 Les fonctions système SUSER_SNAME() et HOST_NAME() ne sont pas propres à la réplication de fusion mais elles sont utilisées par celle-ci pour le filtrage paramétré :  
  
-   SUSER_SNAME() retourne des informations de connexion sur les connexions établies avec une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Lorsqu'elle est utilisée dans un filtre paramétré, elle retourne le nom de connexion utilisé par l'Agent de fusion pour se connecter au serveur de publication (spécifié lorsque vous créez un abonnement).  
  
-   HOST_NAME() retourne le nom de l'ordinateur qui se connecte à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Lorsqu'elle est utilisée dans un filtre paramétré, elle retourne par défaut le nom de l'ordinateur exécutant l'Agent de fusion. Pour les abonnements extraits, il s'agit du nom du serveur de publication tandis que pour les abonnements envoyés, c'est celui du serveur de distribution.  
  
     Il est également possible de remplacer cette fonction par une valeur autre que le nom du serveur de publication ou de distribution. En général les applications remplacent cette fonction par des valeurs plus explicites, telles qu'un nom de vendeur ou un ID de vendeur. Pour plus d'informations, consultez la section « Substitution de la valeur de HOST_NAME() » dans cette rubrique.  
  
 La valeur retournée par la fonction système est comparée à une colonne spécifiée dans la table que vous filtrez et les données appropriées sont téléchargées vers l'Abonné. Cette comparaison est effectuée au moment de l'initialisation de l'abonnement (dès lors, seules les données pertinentes sont contenues dans l'instantané initial) et à chaque synchronisation de l'abonnement. Par défaut, si une modification apportée sur le serveur de publication enlève une ligne d’une partition, la ligne est supprimée sur l’abonné (ce comportement est contrôlé à l’aide du paramètre **@allow_partition_realignment** de [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)).  
  
> [!NOTE]  
>  Lors des comparaisons effectuées pour les filtres paramétrés, le classement de la base de données est toujours utilisé. Si, par exemple, le classement de la base de données ne respecte pas la casse mais que le classement de la table ou de la colonne le fait, la comparaison ne tiendra pas compte de la casse.  
  
### <a name="filtering-with-susersname"></a>Filtrage avec SUSER_SNAME()  
 Prenons l'exemple de la **table Employee** dans la base de données exemple [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] Cette table comprend la colonne **LoginID**qui contient le nom de connexion de chaque employé sous la forme «*domaine\connexion*». Pour filtrer la table afin que les employés reçoivent uniquement les données qui les concernent, spécifiez la clause de filtre suivante :  
  
```  
LoginID = SUSER_SNAME()  
```  
  
 Par exemple la valeur d'un des employés est « adventure-works\john5 ». Lorsque l'Agent de fusion se connecte au serveur de publication, il utilise le nom de connexion spécifié lors de la création de l'abonnement (dans ce cas-ci, adventure-works\john5). L'Agent de fusion compare ensuite la valeur retournée par SUSER_SNAME() aux valeurs de la table et ne télécharge que la ligne qui contient la valeur « adventure-works\john5 » dans la colonne **LoginID** .  
  
### <a name="filtering-with-hostname"></a>Filtrage avec HOST_NAME()  
 Prenons l'exemple de la table **HumanResources.Employee** . Supposons que cette table contient une colonne **ComputerName** avec le nom de l'ordinateur de chaque employé désigné sous la forme «*nom_typeordinateur*». Pour filtrer la table afin que les employés reçoivent uniquement les données qui les concernent, spécifiez la clause de filtre suivante :  
  
```  
ComputerName = HOST_NAME()  
```  
  
 Par exemple, la valeur correspondant à l'un des employés peut être « john5_laptop ». Lorsque l'Agent de fusion se connecte au serveur de publication, il compare la valeur retournée par SUSER_SNAME() aux valeurs de la table et ne télécharge que la ligne contenant la valeur « john5_laptop » dans la colonne **ComputerName** .  
  
 Il est également possible de combiner les fonctions dans un filtre. Si, par exemple, vous souhaitez qu'un employé ne reçoive les données que s'il se connecte avec son nom de connexion sur son ordinateur, la clause du filtre peut se présenter comme suit :  
  
```  
LoginID = SUSER_SNAME() AND ComputerName = HOST_NAME()  
```  
  
 À moins que vous substituiez la valeur de HOST_NAME(), le filtrage avec HOST_NAME() est, en général, uniquement utilisé avec les abonnements extraits. La valeur retournée par la fonction est le nom de l'ordinateur exécutant l'Agent de fusion. Pour les abonnements extraits, la valeur est différente pour chaque abonnement alors que la valeur est identique pour les abonnements envoyés (tous les Agents de fusion s'exécutent sur le serveur de distribution dans le cas des abonnements envoyés).  
  
> [!IMPORTANT]  
>  La valeur de la fonction HOST_NAME() peut être substituée ; par conséquent, il n'est pas possible d'utiliser des filtres qui incluent HOST_NAME() pour contrôler l'accès aux partitions de données. Pour contrôler cet accès, utilisez SUSER_SNAME(), SUSER_SNAME() en combinaison avec HOST_NAME() ou employez des filtres de lignes statiques.  
  
#### <a name="overriding-the-hostname-value"></a>Substitution de la valeur de HOST_NAME()  
 Comme indiqué ci-dessus, HOST_NAME() retourne, par défaut, le nom de l'ordinateur qui se connecte à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Lors de l'utilisation de filtres paramétrés, il est fréquent de remplacer cette valeur par une autre valeur au moment où vous créez l'abonnement. La fonction HOST_NAME() retourne alors la valeur spécifiée à la place du nom de l'ordinateur.  
  
> [!NOTE]  
>  Si vous remplacez la valeur de HOST_NAME(), tous les appels de la fonction HOST_NAME() retourneront la valeur que vous avez spécifiée. Assurez-vous que les autres applications ne dépendent pas de la fonction HOST_NAME() avec retour du nom de l'ordinateur.  
  
 Prenons l'exemple de la table **HumanResources.Employee** . Cette table comprend la colonne **EmployeeID**. Pour filtrer la table afin que chaque employé reçoive uniquement les données qui le concerne, spécifiez la clause de filtre :  
  
 `EmployeeID = CONVERT(int,HOST_NAME())`  
  
 Par exemple, l'employée Pamela Ansman-Wolfe possède l'ID d'employé 280. Spécifiez la valeur de l'ID d'employé (280 dans notre exemple) comme valeur de HOST_NAME() lorsque vous créez un abonnement pour celle-ci. Lorsque l'Agent de fusion se connecte au serveur de publication, il compare la valeur retournée par HOST_NAME() aux valeurs de la table et ne télécharge que la ligne contenant la valeur 280 dans la colonne **EmployeeID** .  
  
> [!IMPORTANT]  
>  Du fait que la fonction HOST_NAME() retourne une valeur de type **nchar** , vous devez utiliser CONVERT si la colonne dans la clause de filtre affiche un type de données numérique comme dans l'exemple ci-dessus. Pour des raisons de performances, nous vous recommandons de ne pas appliquer de fonctions aux noms de colonnes dans les clauses de filtre de lignes paramétrables, telles que `CONVERT(nchar,EmployeeID) = HOST_NAME()`. En revanche, il est conseillé d'adopter l'approche illustrée dans l'exemple : `EmployeeID = CONVERT(int,HOST_NAME())`. Cette clause peut être utilisée pour le paramètre **@subset_filterclause** de [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)mais elle ne peut généralement pas être utilisée dans l'Assistant Nouvelle publication (l'Assistant exécute la clause de filtre pour la valider mais l'opération échoue car le nom de l'ordinateur ne peut pas être converti en un type **int**). Si vous utilisez l'Assistant Nouvelle publication, nous vous recommandons de spécifier `CONVERT(nchar,EmployeeID) = HOST_NAME()` dans l'Assistant, puis d'utiliser ensuite [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) pour modifier la clause en `EmployeeID = CONVERT(int,HOST_NAME())` avant de créer un instantané de la publication.  
  
 **Pour substituer la valeur de HOST_NAME()**  
  
 Utilisez l'une des méthodes suivantes pour substituer la valeur de HOST_NAME() :  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] : spécifiez une valeur dans la page **Valeurs de HOST\_NAME\(\)** de l’Assistant Nouvel abonnement. Pour plus d’informations sur la création d’abonnements, consultez [S’abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md).  
  
-   Programmation [!INCLUDE[tsql](../../../includes/tsql-md.md)] de réplication : spécifiez une valeur pour le paramètre **@hostname** de [sp_addmergesubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md) (pour les abonnements par émission de données) ou de [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) (pour les abonnements par extraction).  
  
-   Agent de fusion : spécifiez une valeur pour le paramètre **-Hostname** dans la ligne de commande ou par l’intermédiaire d’un profil d’Agent. Pour plus d'informations sur l'Agent de fusion, consultez [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md). Pour plus d'informations sur les profils d'Agent, consultez [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
## <a name="initializing-a-subscription-to-a-publication-with-parameterized-filters"></a>Initialisation d'un abonnement à une publication avec des filtres paramétrés  
 Lorsque vous utilisez des filtres de lignes paramétrés dans les publications de fusion, la réplication initialise chaque abonnement avec un instantané en deux parties. Pour plus d’informations, voir [Instantanés des publications de fusion avec des filtres paramétrés](../../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md).  
  
## <a name="using-the-appropriate-filtering-options"></a>Utilisation des options de filtrage appropriées  
 Vous contrôlez essentiellement deux processus lorsque vous utilisez les filtres paramétrés :  
  
-   le mode de traitement des filtres par réplication de fusion, qui est contrôlé par l'un des deux paramètres de publication : **use partition groups** et **keep partition changes**.  
  
-   le mode de partage des données entre Abonnés, lequel est identifié par le paramètre d'article **partition options**.  
  
 Pour définir des options de filtrage, consultez [Optimize Parameterized Row Filters](../../../relational-databases/replication/publish/optimize-parameterized-row-filters.md).  
  
### <a name="setting-use-partition-groups-and-keep-partition-changes"></a>Définition de « use partition groups » et « keep partition changes »  
 Les options **use partition groups** et **keep partition changes** améliorent les performances de synchronisation des publications avec des articles filtrés en stockant des métadonnées supplémentaires dans la base de données de publication. C'est l'option **use partition groups** qui améliore le plus les performances grâce à l'utilisation de la fonctionnalité de partitions précalculées. Par défaut, cette option est définie à **true** si les articles de votre publication respectent un ensemble de conditions. Pour plus d’informations sur ces exigences, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Si vos articles ne répondent pas aux conditions d'utilisation requises pour l'emploi de partitions précalculées, l'option **keep partition changes** a la valeur **true**.  
  
### <a name="setting-partition-options"></a>Définition de « partition options »  
 Vous spécifiez une valeur pour la propriété **partition options** lors de la création d'un article, selon la façon dont les données de la table filtrée seront partagées par les Abonnés. Vous pouvez attribuer une à quatre valeurs à l'aide de [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)et de la boîte de dialogue **Propriétés de l'article** . La propriété peut avoir deux valeurs différentes lorsque vous la définissez dans les boîtes de dialogue **Ajouter un filtre** ou **Modifier le filtre** , auxquelles vous accédez à partir de l'Assistant Nouvelle publication et la boîte de dialogue **Propriétés de la publication** . Le tableau suivant récapitule les valeurs disponibles :  
  
|Description|Valeur dans Ajouter un filtre et Modifier un filtre|Valeur dans Propriétés de l'article|Valeur dans les procédures stockées|  
|-----------------|-----------------------------------------|---------------------------------|--------------------------------|  
|Les données dans les partitions se chevauchent et l'Abonné peut mettre à jour des colonnes référencées dans un filtre paramétré.|**Une ligne de cette table ira à plusieurs abonnements**|**Chevauchement**|**0**|  
|Les données dans les partitions se chevauchent et l'Abonné ne peut pas mettre à jour les colonnes référencées dans un filtre paramétré.|N/A*|**Chevauchement ; refus des modifications de données hors partition**|**1**|  
|Les données des partitions ne se chevauchent pas et les données sont partagées entre les abonnements. L'Abonné ne peut pas mettre à jour les colonnes référencées dans un filtre paramétré.|N/A*|**Non-chevauchement ; partage entre les abonnements**|**2**|  
|Les données des partitions ne se chevauchent pas et il n'existe qu'un seul abonnement par partition. L’Abonné ne peut pas mettre à jour les colonnes référencées dans un filtre paramétré.**|**Filtre paramétré créant des partitions qui ne se chevauchent pas, avec un seul abonnement par partition**|**Non-chevauchement ; abonnement unique**|**3**|  
  
 \*Si l’option de filtrage sous-jacente a la valeur **0**, **1** ou**2**, les boîtes de dialogue **Ajouter un filtre** et **Modifier le filtre** affichent le message suivant : **Une ligne de cette table ira à plusieurs abonnements**.  
  
 ** Si vous spécifiez cette option, il ne peut y avoir qu’un seul abonnement pour chaque partition de données de cet article. Si un deuxième abonnement est créé dans lequel le critère de filtrage du nouvel abonnement produit la même partition que l'abonnement existant, ce dernier est supprimé.  
  
> [!IMPORTANT]  
>  La valeur de **partition options** doit être définie en fonction du partage des données entre Abonnés. Si, par exemple, vous spécifiez une partition qui ne se chevauche pas avec un seul abonnement par partition mais que les données sont ensuite mises à jour sur un autre Abonné, l'Agent de fusion peut échouer au cours de la synchronisation et un problème de non convergence peut survenir.  
  
#### <a name="selecting-the-appropriate-partition-option"></a>Sélection de l'option de partition adéquate  
 Les partitions qui ne se chevauchent pas sont utilisées en conjonction avec les partitions précalculées pour améliorer les performances dans les cas où certaines limitations fonctionnelles sont admises. Les partitions précalculées accélèrent les téléchargements vers les Abonnés mais ralentissent les téléchargements à partir de ceux-ci. Les partitions qui ne se chevauchent pas réduisent les coûts de téléchargement associés aux partitions précalculées. Le gain de performances lié au non-chevauchement des partitions est plus flagrant lorsque les filtres paramétrés et les filtres de jointure utilisés sont plus complexes.  
  
 Prenez les scénarios suivants en considération lorsque vous décidez de l'option de partition à utiliser dans une publication.  
  
-   [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] possède une force de vente itinérante dont chaque membre est responsable de clients de la zone d'un code postal donné. En cas de déplacement d'un client d'un secteur de vente vers un autre, l'application exige que le code postal soit mis à jour pour que le client soit attribué à un autre commercial. Le filtre paramétré est basé sur le code postal du client et la mise à jour entraîne la suppression du code postal de la partition d'un commercial et son insertion dans la partition d'un autre commercial. Cela exige la présence de partitions qui se chevauchent avec la possibilité de mettre à jour les colonnes référencées dans un filtre paramétré. Cette option offre une plus grande souplesse mais peut s'avérer moins performante que les partitions qui ne se chevauchent pas.  
  
-   Une agence pour l'emploi possède des données qu'elle transmet à ses diverses antennes régionales. Les données ne se chevauchent pas ; chaque ligne de la table hébergée sur le serveur du siège de l'agence est incluse dans une seule partition mais cette dernière est envoyée à plusieurs antennes régionales. L'utilisation de l'option de partitions qui ne se chevauchent pas avec des partitions partagées entre les abonnements convient parfaitement, car elle offre de meilleures performances que les partitions qui se chevauchent tout en répondant aux exigences de l'application.  
  
-   Si vous disposez de partitions qui ne se chevauchent pas et qu'un seul abonnement reçoit et met à jour les données d'une partition, les performances y gagnent encore. Il s'agit d'un scénario classique pour les systèmes de points de vente et les applications de force de vente dans lesquelles les données sont principalement collectées sur l'Abonné et téléchargées vers le serveur de publication. Prenons l'exemple d'une table **Package** dans une application de livraison : au fur et à mesure du chargement des colis dans le camion, l'état des colis change dans la table **Package** et la modification est répliquée vers le siège de la société. Les camionneurs ne mettent pas à jour l'état du même colis sur deux camions différents de sorte que la table **Package** est un bon exemple de partition qui ne se chevauche pas avec un seul abonnement par partition.  
  
#### <a name="considerations-for-nonoverlapping-partitions"></a>Considérations en matière de partitions qui ne se chevauchent pas  
 Les éléments suivants doivent être pris en compte lors de l'utilisation de partitions qui ne se chevauchent pas.  
  
##### <a name="general-considerations"></a>Considérations générales  
  
-   La publication doit utiliser des partitions précalculées.  
  
-   Une ligne ne doit appartenir qu'à une seule partition.  
  
-   Les articles ne peuvent pas faire partie d'un enregistrement logique.  
  
-   D'autres partenaires de synchronisation ne sont pas pris en charge (cette fonctionnalité est déconseillée).  
  
-   L'Abonné ne peut pas mettre à jour les colonnes référencées dans un filtre paramétré.  
  
-   Si une insertion sur un Abonné n'appartient pas à la partition, elle n'est pas supprimée. En revanche, elle ne sera pas répliquée vers les autres Abonnés.  
  
-   Dans certains cas de partitions qui se chevauchent, les plages d'identité sont modifiées lorsque l'Agent de fusion insère des données. Avec des partitions qui ne se chevauchent pas, les plages peuvent uniquement être modifiées au cours des insertions effectuées par un utilisateur autorisé à modifier les plages d'identité dans la base de données d'abonnement. L'utilisateur doit être soit propriétaire de la table, soit membre du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** ou du rôle de base de données fixe **db_ddladmin** .  
  
##### <a name="additional-considerations-for-nonoverlapping-partitions-with-a-single-subscription-per-partition"></a>Autres considérations relatives aux partitions qui ne se chevauchent pas avec un seul abonnement par partition  
  
-   Les articles ne peuvent exister que dans une seule publication et ne peuvent pas être republiés.  
  
-   La publication doit autoriser les Abonnés à initialiser le processus d'instantané. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md).  
  
##### <a name="additional-considerations-for-join-filters"></a>Autres considérations sur les filtres de jointure  
  
-   Dans une hiérarchie de filtres de jointure, un article avec chevauchement de partitions peut apparaître au dessus d'un article sans chevauchement de partitions. En d'autres termes, un article parent doit utiliser des partitions qui ne se chevauchent pas si l'article enfant en possède. Pour plus d'informations sur les filtres de jointure, consultez [Join Filters](../../../relational-databases/replication/merge/join-filters.md).  
  
-   Un filtre de jointure dans lequel la partition sans chevauchement est un enfant doit avoir la propriété **join unique key** affectée de la valeur 1. Pour plus d’informations, voir [Join Filters](../../../relational-databases/replication/merge/join-filters.md).  
  
-   L'article ne doit avoir qu'un seul filtre paramétré ou filtre de jointure. Posséder un filtre paramétré et être le parent dans un filtre de jointure est autorisé. Posséder un filtre paramétré et être l'enfant dans un filtre de jointure n'est pas autorisé. Posséder plusieurs filtres de jointures est également interdit.  
  
-   Si deux tables d'un serveur de publication possèdent une relation de filtre de jointure et que la table enfant contient des lignes qui ne correspondent à aucune ligne de la table parent, une insertion de la ligne parente manquante n'entraîne pas le téléchargement des lignes liées vers l'Abonné (les lignes seraient téléchargées avec des partitions qui se chevauchent). Si, par exemple, la table **SalesOrderDetail** comprend des lignes sans correspondance dans la table **SalesOrderHeader** et que vous insérez la ligne manquante dans **SalesOrderHeader**, la ligne est téléchargée sur l'Abonné mais les lignes correspondantes de la table **SalesOrderDetail** ne le sont pas.  
  
## <a name="see-also"></a> Voir aussi  
 [Bonnes pratiques concernant les filtres de lignes basés sur le temps](../../../relational-databases/replication/merge/best-practices-for-time-based-row-filters.md)   
 [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Filtrer des données publiées en vue de la réplication de fusion](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
