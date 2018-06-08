---
title: DBCC CHECKDB (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CHECKDB_TSQL
- DBCC_CHECKDB_TSQL
- DBCC CHECKDB
- CHECKDB
dev_langs:
- TSQL
helpviewer_keywords:
- CHECKDB [DBCC statement]
- database objects [SQL Server], checking
- counting pages
- per-index row counts
- per-table row counts
- DBCC CHECKDB statement
- per-table page counts
- allocation checks
- integrity [SQL Server], database objects
- per-index page counts
- counting rows
- table integrity checks [SQL Server]
- row count accuracy [SQL Server]
- negative counts
- checking database objects
- page count accuracy [SQL Server]
ms.assetid: 2c506167-0b69-49f7-9282-241e411910df
caps.latest.revision: 144
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 7f270fd58e58b7e6c850a520dff4cd37e2ddb4ec
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-checkdb-transact-sql"></a>DBCC CHECKDB (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

Vérifie l'intégrité logique et physique de tous les objets de la base de données spécifiée en effectuant les opérations suivantes :    
    
-   Exécute [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md) sur la base de données.    
-   Exécute [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) sur chaque table et vue de la base de données.    
-   Exécute [DBCC CHECKCATALOG](../../t-sql/database-console-commands/dbcc-checkcatalog-transact-sql.md) sur la base de données.    
-   Valide le contenu de chaque vue indexée dans la base de données.    
-   Valide la cohérence au niveau du lien entre les métadonnées de la table et les répertoires et les fichiers du système de fichiers lors du stockage de données **varbinary(max)** dans le système de fichiers à l’aide de FILESTREAM.    
-   Valide les données [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans la base de données.    
    
Cela signifie que l'exécution des commandes DBCC CHECKALLOC, DBCC CHECKTABLE ou DBCC CHECKCATALOG ne doit pas être distincte de celle de DBCC CHECKDB. Pour plus d'informations sur les vérifications réalisées par ces commandes, consultez les descriptions des commandes.    
 
> [!NOTE]
> DBCC CHECKDB est pris en charge sur les bases de données contenant des tables mémoire optimisées, mais la validation se produit uniquement sur les tables sur disque. Cependant, dans le cadre de la sauvegarde et de la restauration des bases de données, une validation CHECKSUM est effectuée pour les fichiers des groupes de fichiers mémoire optimisés.    
>     
> Étant donné que options de réparation de DBCC ne sont pas disponibles pour les tables mémoire optimisées, vous devez sauvegarder les bases de données régulièrement et tester les sauvegardes. Si des problèmes d'intégrité des données se produisent dans une table mémoire optimisée, vous devez restaurer à partir de la dernière sauvegarde connue et fiable.    

![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Syntaxe    
    
```    
DBCC CHECKDB     
    [ ( database_name | database_id | 0    
        [ , NOINDEX     
        | , { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD } ]    
    ) ]    
    [ WITH     
        {    
            [ ALL_ERRORMSGS ]    
            [ , EXTENDED_LOGICAL_CHECKS ]     
            [ , NO_INFOMSGS ]    
            [ , TABLOCK ]    
            [ , ESTIMATEONLY ]    
            [ , { PHYSICAL_ONLY | DATA_PURITY } ]    
            [ , MAXDOP  = number_of_processors ]    
        }    
    ]    
]    
```    
    
## <a name="arguments"></a>Arguments    
 *database_name* | *database_id* | 0  
 Nom ou ID de la base de données pour laquelle vous exécutez des vérifications d'intégrité. En l'absence de spécification, ou si 0 est spécifié, la base de données actuelle est utilisée. Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
    
NOINDEX  
 Indique qu'il ne faut pas effectuer de vérifications intensives des index non cluster pour les tables utilisateur. Cela diminue la durée d'exécution globale. NOINDEX n'affecte pas les tables système car les vérifications d'intégrité sont toujours effectuées sur leurs index.  
    
REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD  
 Spécifie que DBCC CHECKDB répare les erreurs trouvées. N'utilisez les options REPAIR qu'en dernier recours. La base de données spécifiée   doit être en mode mono-utilisateur pour pouvoir utiliser l'une des options de réparation suivantes.  
    
REPAIR_ALLOW_DATA_LOSS  
 Tente de réparer toutes les erreurs signalées. Ces réparations peuvent entraîner des pertes de données.  
    
> [!WARNING]
> L’option REPAIR_ALLOW_DATA_LOSS est une fonctionnalité prise en charge, mais il ne s’agit pas toujours nécessairement de la meilleure option pour qu’une base de données soit dans un état physiquement cohérent. En cas de réussite, l'option REPAIR_ALLOW_DATA_LOSS peut entraîner une perte de données. En fait, elle peut entraîner une perte de données supérieure à celle que vous pourriez constater si un utilisateur restaurait la base de données à partir de la dernière bonne sauvegarde. 
>
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande toujours d'effectuer une restauration utilisateur à partir de la dernière bonne sauvegarde comme principale méthode pour récupérer suite aux erreurs signalées par DBCC CHECKDB. L'option REPAIR_ALLOW_DATA_LOSS n'est pas une alternative pour la restauration à partir d'une sauvegarde reconnue fiable. Il s'agit d'une option d'urgence de « dernier recours » recommandée uniquement si la restauration à partir d'une sauvegarde n'est pas possible.    
>     
> Certaines erreurs, qui peuvent uniquement être réparées à l'aide de l'option REPAIR_ALLOW_DATA_LOSS, peuvent nécessiter de désallouer une ligne, une page ou une série de pages pour effacer les erreurs. Toutes les données désallouées ne sont plus accessibles ou récupérables par l'utilisateur et le contenu exact des données désallouées ne peut pas être déterminé. Par conséquent, l'intégrité référentielle peut être inexacte après la désallocation de lignes ou de pages, car les contraintes de clé étrangère ne sont pas vérifiées ou conservées dans le cadre de cette opération de réparation. L'utilisateur doit examiner l'intégrité référentielle de sa base de données (à l'aide de DBCC CHECKCONSTRAINTS) après avoir utilisé l'option REPAIR_ALLOW_DATA_LOSS.    
>     
> Avant d'effectuer la réparation, créez des copies physiques des fichiers qui appartiennent à cette base de données. Cela comprend le fichier de données principal (.mdf), les fichiers de données secondaires (.ndf), tous les fichiers de journaux de transactions (.ldf) et autres conteneurs qui forment la base de données, y compris les catalogues de texte intégral, les dossiers de flux de fichiers, les données optimisées en mémoire, etc.    
>     
> Avant d'effectuer la réparation, modifiez l'état de la base de données en mode d'urgence et essayez d'extraire autant d'informations que possible à partir des tables critiques et d'enregistrer ces données.    
    
REPAIR_FAST  
 Conserve la syntaxe pour une compatibilité descendante uniquement. Aucune réparation n'est effectuée.  
    
REPAIR_REBUILD  
 Effectue des réparations qui ne présentent aucun risque de perte de données. Cela peut inclure des réparations rapides, telles que la réparation de lignes manquantes dans des index non-cluster, ainsi que des réparations nécessitant plus de temps, telles que la reconstruction d'un index.  
 Cet argument ne répare pas les erreurs impliquant des données FILESTREAM.  
    
> [!IMPORTANT] 
> Dans la mesure où DBCC CHECKDB avec l'une des options REPAIR est entièrement journalisé et récupérable, [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande toujours d'utiliser CHECKDB avec des options REPAIR dans une transaction (exécutez BEGIN TRANSACTION avant d'exécuter la commande) pour que l'utilisateur puisse confirmer qu'il souhaite accepter les résultats de l'opération. L'utilisateur peut ensuite exécuter COMMIT TRANSACTION pour valider tout le travail effectué par l'opération de réparation. Si l'utilisateur ne souhaite pas accepter les résultats de l'opération, il peut exécuter une instruction ROLLBACK TRANSACTION pour annuler les effets des opérations de réparation.    
>     
> Pour réparer les erreurs, nous vous recommandons d'effectuer une restauration à partir d'une sauvegarde. Les opérations de réparation ne prennent en compte aucune des contraintes qui peuvent exister sur les tables ou entre tables. Si la table spécifiée est impliquée dans une ou plusieurs contraintes, nous vous recommandons d'exécuter DBCC CHECKCONSTRAINTS après une réparation. Si vous devez utiliser REPAIR, exécutez la commande DBCC CHECKDB sans option de réparation afin de déterminer le niveau de réparation à utiliser. Si vous utilisez le niveau REPAIR_ALLOW_DATA_LOSS, nous vous recommandons de sauvegarder la base de données avant d'exécuter la commande DBCC CHECKDB avec cette option.    
    
ALL_ERRORMSGS  
 Affiche toutes les erreurs signalées par objet. Tous les messages d'erreur sont affichés par défaut. La spécification ou non de cette option n'a aucun effet. Les messages d’erreur sont triés par ID d’objet, à l’exception des messages générés à partir de la [base de données tempdb](../../relational-databases/databases/tempdb-database.md).     

EXTENDED_LOGICAL_CHECKS  
 Si le niveau de compatibilité est égal à 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) ou supérieur, effectue des vérifications de cohérence logique sur une vue indexée, des index XML et des index spatiaux, là où il est présent.  
 Pour plus d’informations, consultez *Exécution de vérifications de cohérence logique sur des index* dans la section [Notes](#remarks), plus loin dans cette rubrique.  
    
NO_INFOMSGS  
 Supprime tous les messages d'information.  
    
TABLOCK  
 Génère des verrouillages par DBCC CHECKDB au lieu d'utiliser un instantané de base de données interne. Cette opération comprend un verrou exclusif sur la base de données. TABLOCK accélère l'exécution de DBCC CHECKDB sur une base de données dont la charge est importante, tout en diminuant la concurrence disponible dans cette dernière pendant l'exécution de DBCC CHECKDB.  
    
> [!IMPORTANT] 
> TABLOCK limite les vérifications effectuées ; DBCC CHECKCATALOG n'est pas exécuté sur la base de données et les données [!INCLUDE[ssSB](../../includes/sssb-md.md)] ne sont pas validées.
    
ESTIMATEONLY  
 Affiche une estimation de la quantité d’espace tempdb nécessaire pour exécuter DBCC CHECKDB avec toutes les autres options spécifiées. La vérification de la base de données actuelle n'est pas effectuée.  
    
PHYSICAL_ONLY  
 Limite la nature de la vérification à l'intégrité de la structure physique sur la page et les en-têtes d'enregistrement, et à l'intégrité de la cohérence d'allocation de la base de données. Cette vérification, qui vise à contrôler la cohérence physique de la base de données, inclut par ailleurs la détection des pages endommagées, des échecs de somme de contrôle et des erreurs matérielles courantes, susceptibles de compromettre les données utilisateur.  
 Une exécution complète de DBCC CHECKDB peut prendre beaucoup plus de temps que dans les versions antérieures. Ce problème se produit parce que :  
 -   Les vérifications logiques sont plus complètes.  
 -   Certaines des structures sous-jacentes à vérifier sont plus complexes.  
 -   De nombreuses vérifications nouvelles ont été introduites pour inclure les nouvelles fonctionnalités.  
 Par conséquent, l'utilisation de l'option PHYSICAL_ONLY étant susceptible de réduire considérablement la durée d'exécution de DBCC CHECKDB sur des bases de données volumineuses, elle est recommandée pour une utilisation fréquente sur des systèmes de production. Nous vous recommandons d'effectuer régulièrement une exécution complète de DBCC CHECKDB. La fréquence de ces exécutions dépend de facteurs spécifiques à chaque entreprise et à chaque environnement de production.    
Cet argument implique toujours NO_INFOMSGS et n’est autorisé avec aucune des options de réparation.  
    
> [!WARNING] 
> La spécification de l'option PHYSICAL_ONLY fait que DBCC CHECKDB ignorera toutes les vérifications des données FILESTREAM.
    
DATA_PURITY  
 Génère la vérification de la base de données par DBCC CHECKDB pour les valeurs de colonnes qui ne sont pas valides ou hors limites. Par exemple, DBCC CHECKDB détecte des colonnes dont les dates et les heures sont supérieures ou inférieures à la plage acceptable pour le type de données **datetime**. Cette commande identifie aussi des colonnes de type **decimal** ou numeric approximatif avec des valeurs d’échelle ou de précision qui ne sont pas valides.  
 Les vérifications d'intégrité sur la base colonne-valeur sont activées par défaut et ne nécessitent pas l'option DATA_PURITY. Pour les bases de données mises à niveau à partir des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les vérifications sur la base colonne-valeur ne sont pas activées par défaut tant que la commande DBCC CHECKDB WITH DATA_PURITY n'est pas exécutée sans erreur sur cette base de données. Ensuite, DBCC CHECKDB vérifie l'intégrité sur la base colonne-valeur par défaut. Pour plus d'informations sur les incidences sur CHECKDB suite à une mise à niveau de la base de données à partir de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez la section Notes, plus loin dans cette rubrique.  
    
> [!WARNING]
> Si PHYSICAL_ONLY est spécifié, l'intégrité des colonnes n'est pas vérifiée.
    
 Les erreurs de validation signalées par cette option ne peuvent pas être corrigées à l'aide des options de réparation DBCC. Pour plus d’informations sur la correction manuelle de ces erreurs, consultez l’article 923247 de la Base de connaissances Microsoft : [Dépannage de l’erreur DBCC 2570 dans SQL Server 2005 et versions ultérieures](http://support.microsoft.com/kb/923247).  
    
 MAXDOP  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).  
    
 Remplace l’option de configuration **max degree of parallelism** de **sp_configure** pour l’instruction. MAXDOP peut dépasser la valeur configurée avec sp_configure. Si MAXDOP dépasse la valeur configurée avec Resource Governor, le [!INCLUDE[ssDEnoversion](../../includes/ssDEnoversion_md.md)] utilise la valeur MAXDOP de Resource Governor, décrite dans [ALTER WORKLOAD GROUP](../../t-sql/statements/alter-workload-group-transact-sql.md). Toutes les règles sémantiques utilisées avec l'option de configuration max degree of parallelism sont applicables lorsque vous utilisez l'indicateur de requête MAXDOP. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
 
> [!WARNING] 
> Si MAXDOP est défini avec la valeur zéro, SQL Server choisit le degré maximal de parallélisme à utiliser.    

## <a name="remarks"></a>Notes     
DBCC CHECKDB n'examine pas les index désactivés. Pour plus d’informations sur les index désactivés, consultez [Désactiver les index et contraintes](../../relational-databases/indexes/disable-indexes-and-constraints.md).    

Si un type défini par l'utilisateur est marqué comme étant ordonné par octet, il ne doit y avoir qu'une seule sérialisation du type défini par l'utilisateur. En l'absence de sérialisation cohérente de type défini par l'utilisateur ordonné par octet, l'erreur 2537 est générée à l'exécution de DBCC CHECKDB. Pour plus d’informations, consultez [Configuration requise pour les types définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-requirements.md).    

Étant donné que la [base de données Resource](../../relational-databases/databases/resource-database.md) est modifiable uniquement en mode mono-utilisateur, vous ne pouvez pas y exécuter la commande DBCC CHECKDB directement. Cependant, lors de l’exécution de DBCC CHECKDB sur la [base de données master](../../relational-databases/databases/master-database.md), une deuxième commande CHECKDB est également exécutée en interne sur la base de données Resource. Cela signifie que DBCC CHECKDB peut retourner des résultats supplémentaires. La commande retourne des jeux de résultats supplémentaires lorsqu'aucune option n'est définie ou lorsque l'option PHYSICAL_ONLY ou ESTIMATEONLY est définie.    

À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2, l’exécution de la commande DBCC CHECKDB n’efface **plus** le cache du plan pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Avant [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2, l’exécution de la commande DBCC CHECKDB effaçait le cache du plan. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. 
    
## <a name="performing-logical-consistency-checks-on-indexes"></a>Exécution de vérifications de cohérence logique sur des index    
La vérification de la cohérence logique sur les index varie selon le niveau de compatibilité de la base de données, comme suit :
-   Si le niveau de compatibilité est égal à 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) ou supérieur :    
-   À moins que l'option NOINDEX soit spécifiée, DBCC CHECKDB effectue des vérifications de cohérence physique et logique sur une table individuelle et sur tous ses index non-cluster. Toutefois, seules des vérifications de cohérence physique sont effectuées par défaut sur les index XML, les index spatiaux et les vues indexées.
-   Si WITH EXTENDED_LOGICAL_CHECKS est spécifié, des vérifications logiques sont effectués sur une vue indexée, des index XML et des index spatiaux, là où ils sont présents. Par défaut, les vérifications de cohérence physique sont effectuées avant les vérifications de cohérence logique. Si l'option NOINDEX est également spécifiée, seules les vérifications logiques sont effectuées.
    
Ces vérifications de cohérence logique effectuent une vérification croisée de la table d'index interne de l'objet d'index avec la table utilisateur à laquelle il fait référence. Pour rechercher les lignes excentrées, une requête interne est construite pour effectuer l'intersection complète de la table interne et de la table utilisateur. L'exécution de cette requête peut avoir un effet très important sur les performances et il n'est pas possible de suivre sa progression. Par conséquent, nous vous recommandons de spécifier WITH EXTENDED_LOGICAL_CHECKS seulement si vous soupçonnez des problèmes d'index qui ne sont pas liés à une altération physique ou si les sommes de contrôle au niveau de la page ont été désactivées et que vous soupçonnez un endommagement matériel au niveau des colonnes.
-   Si l'index est un index filtré, DBCC CHECKDB effectue des vérifications de cohérence pour vérifier que les entrées de l'index satisfont le prédicat du filtre.
-   Si le niveau de compatibilité est égal ou inférieur à 90, à moins que l'option NOINDEX soit spécifiée, DBCC CHECKDB effectue à la fois des vérifications de cohérence physique et logique sur une seule table ou vue indexée et sur tous ses index non-cluster et XML. Les index spatiaux ne sont pas pris en charge.  
- À compter de SQL Server 2016, les vérifications supplémentaires sur les colonnes calculées persistantes, les colonnes UDT et les index filtrés ne seront pas exécutées par défaut afin d’éviter les évaluations d’expressions coûteuses. Cette modification réduit considérablement la durée de CHECKDB sur les bases de données contenant ces objets. Cependant, les vérifications de cohérence physique de ces objets sont toujours effectuées. Les évaluations d’expressions ne seront effectuées en plus des vérifications logiques déjà présentes (vue indexée, index XML et index spatiaux) dans le cadre de l’option EXTENDED_LOGICAL_CHECKS que quand l’option EXTENDED_LOGICAL_CHECKS est spécifiée.   
    
**Pour connaître le niveau de compatibilité d’une base de données**
-   [Afficher ou modifier le niveau de compatibilité d’une base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)    
    
## <a name="internal-database-snapshot"></a>Instantané de base de données interne    
DBCC CHECKDB utilise un instantané de base de données interne pour la cohérence transactionnelle nécessaire à la réalisation de ces vérifications. Ceci évite les problèmes de blocage et d'accès simultané lors de l'exécution de ces commandes. Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) et la section Utilisation de l’instantané de base de données interne DBCC dans [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md). Si vous ne pouvez créer aucun instantané ou si TABLOCK est spécifié, la commande DBCC CHECKDB acquiert des verrous pour obtenir la cohérence requise. Dans ce cas, un verrou de base de données exclusif est requis pour effectuer les vérifications d'allocation, tandis que des verrous de table partagés sont nécessaires pour effectuer les vérifications de table.
DBCC CHECKDB échoue quand il est exécuté sur une base de données master s’il n’est pas possible de créer d’instantané de base de données interne.
L’exécution de la commande DBCC CHECKDB sur tempdb ne procède à aucune allocation et ne vérifie aucun catalogue. Elle doit acquérir des verrous de table partagés pour vérifier ces tables. En effet, pour des raisons de performances, les instantanés de base de données ne sont pas disponibles sur tempdb. Cela signifie que la cohérence transactionnelle requise ne peut pas être obtenue.
Dans Microsoft SQL Server 2012 ou une version antérieure de SQL Server, vous pouvez rencontrer des messages d’erreur quand vous exécutez la commande DBCC CHECKDB pour une base de données dont les fichiers sont situés sur un volume au format ReFS. Pour plus d’informations, consultez l’article 2974455 de la Base de connaissances Microsoft : [Comportement de DBCC CHECKDB quand la base de données SQL Server se trouve sur un volume ReFS](https://support.microsoft.com/kb/2974455).    
    
## <a name="checking-and-repairing-filestream-data"></a>Vérification et réparation des données FILESTREAM    
Quand FILESTREAM est activé pour une base de données et une table, vous pouvez éventuellement stocker des objets BLOB(Binary Large Object) **varbinary(max)** dans le système de fichiers. Lorsque vous utilisez DBCC CHECKDB sur une base de données qui stocke des objets BLOB dans le système de fichiers, DBCC vérifie la cohérence au niveau du lien entre le système de fichiers et la base de données.
Par exemple, si une table contient une colonne **varbinary(max)** qui utilise l’attribut FILESTREAM, DBCC CHECKDB vérifiera qu’il existe un mappage un-à-un entre les répertoires et les fichiers du système de fichiers et les lignes, les colonnes et les valeurs de colonne de la table. DBCC CHECKDB peut réparer l'altération si vous spécifiez l'option REPAIR_ALLOW_DATA_LOSS. Pour réparer l'altération FILESTREAM, DBCC supprime toutes les lignes de la table auxquelles il manque des données du système de fichiers.
    
## <a name="best-practices"></a>Bonnes pratiques    
Nous vous recommandons d'utiliser l'option PHYSICAL_ONLY pour une utilisation fréquente sur des systèmes de production. L'utilisation de PHYSICAL_ONLY permet de raccourcir nettement le temps d'exécution de DBCC CHECKDB sur des bases de données volumineuses. Nous vous conseillons également d'exécuter régulièrement DBCC CHECKDB sans option. La fréquence à laquelle vous devez effectuer ces exécutions dépend de chaque activité et de son environnement de production.
    
## <a name="checking-objects-in-parallel"></a>Vérification des objets en parallèle    
DBCC CHECKDB effectue par défaut une vérification parallèle des objets. Le degré de parallélisme est automatiquement défini par le processeur de requêtes. Le degré maximum de parallélisme est configuré de la même manière que les requêtes parallèles. Pour limiter le nombre maximal de processeurs disponibles pour la vérification DBCC, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). La vérification parallèle peut être désactivée à l'aide de l'indicateur de trace 2528. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
    
> [!NOTE]
> Cette fonctionnalité n'est pas disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez les vérifications de cohérence parallèles dans la section Gestion de SGBDR de [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).    
    
## <a name="understanding-dbcc-error-messages"></a>Présentation des messages d'erreur de DBCC    
Une fois la commande DBCC CHECKDB exécutée, un message est consigné dans le journal d'erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la commande DBCC est correctement exécutée, le message indique que l'exécution a réussi, ainsi que la durée d'exécution de la commande. Si la commande DBCC est interrompue avant la fin de la vérification en raison d'une erreur, le message indique que la commande n'a pas abouti, précise une valeur d'état ainsi que la durée d'exécution de la commande. Le tableau suivant répertorie et décrit les valeurs d'état pouvant être incluses dans le message.
    
|État|Description|    
|-----------|-----------------|    
|0|Erreur numéro 8930 générée. Ceci indique une corruption des métadonnées qui a arrêté la commande DBCC.|    
| 1|Erreur numéro 8967 générée. Une erreur DBCC interne s'est produite.|    
|2|Une erreur s'est produite lors de la réparation de la base de données en mode urgence.|    
|3|Ceci indique une corruption des métadonnées qui a arrêté la commande DBCC.|    
|4|Une assertion ou une violation d'accès a été détectée.|    
|5|Une erreur inconnue s'est produite et a arrêté la commande DBCC.|    
    
## <a name="error-reporting"></a>Rapport d'erreurs    
Un fichier de vidage (`SQLDUMP*nnnn*.txt`) est créé dans le répertoire LOG de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chaque fois que DBCC CHECKDB détecte une erreur d’endommagement. Quand les fonctionnalités *Rapport d’erreurs* et de collecte des données *Utilisation de fonctionnalités* sont activées pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce fichier est automatiquement transféré à [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Les données collectées sont utilisées pour améliorer les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Le fichier de vidage contient les résultats de la commande DBCC CHECKDB ainsi que des informations de diagnostic supplémentaires. L’accès est limité au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux membres du rôle sysadmin. Par défaut, le rôle sysadmin contient tous les membres du groupe Windows `BUILTIN\Administrators` et du groupe Administrateurs local. La commande DBCC n'échoue pas si le processus de collecte des données échoue.
    
## <a name="resolving-errors"></a>Résolution des erreurs    
Si des erreurs sont signalées par DBCC CHECKDB, nous vous recommandons de restaurer la base de données à partir de sa sauvegarde plutôt que d'exécuter REPAIR avec une des options correspondantes. En cas d'absence de sauvegarde, la réparation corrige les erreurs détectées. Cette option de réparation est spécifiée à la fin de la liste des erreurs signalées. Néanmoins, la correction des erreurs à l'aide de l'option REPAIR_ALLOW_DATA_LOSS risque de nécessiter que certaines pages, et par conséquent certaines données, soient supprimées.    

Dans de telles circonstances, des valeurs risquent d'être entrées dans la base de données, alors qu'elles ne sont pas valides ou qu'elles sont hors limites, en fonction du type de données de la colonne. DBCC CHECKDB peut détecter des valeurs de colonne non valides pour tous les types de données de colonne. Ainsi, l'exécution de DBCC CHECKDB avec l'option DATA_PURITY sur des bases de données mises à niveau à partir de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risque de révéler des erreurs pré-existantes de valeur-colonne. Comme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas réparer automatiquement ces erreurs, vous devez mettre à jour la valeur de la colonne manuellement. Si CHECKDB détecte une telle erreur, CHECKDB retourne un avertissement, le numéro d'erreur 2570 et des informations pour identifier la ligne adéquate et corriger l'erreur manuellement.    

La réparation peut être effectuée dans une transaction utilisateur pour permettre à celui-ci d'annuler les modifications effectuées. Si des réparations sont restaurées, la base de données contiendra encore des erreurs et il faudra donc la restaurer à partir d'une sauvegarde. Une fois les réparations effectuées, sauvegardez la base de données.
    
## <a name="resolving-errors-in-database-emergency-mode"></a>Résolution des erreurs en mode urgence dans la base de données    
Quand une base de données a été placée en mode urgence à l’aide de l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) et que l’option REPAIR_ALLOW_DATA_LOSS est spécifiée, DBCC CHECKDB peut réaliser certaines réparations particulières sur la base de données. Ces réparations peuvent éventuellement aboutir à la remise en ligne dans un état physiquement cohérent de bases de données irrécupérables en temps normal. Vous devez utiliser ces réparations en dernier recours et uniquement lorsque vous ne pouvez pas restaurer la base de données à partir d'une sauvegarde. Si la base de données est placée en mode urgence, elle est marquée comme READ_ONLY, la journalisation est désactivée et l’accès est limité aux membres du rôle serveur fixe sysadmin.
    
> [!NOTE]
> Vous ne pouvez pas exécuter la commande DBCC CHECKDB en mode urgence dans une transaction utilisateur puis restaurer celle-ci au terme de l'exécution.    
    
Si la base de données est placée en mode urgence et que DBCC CHECKDB est exécutée avec la clause REPAIR_ALLOW_DATA_LOSS, les actions suivantes se produisent :
-   DBCC CHECKDB utilise les pages marquées comme inaccessibles en raison d'erreurs d'E/S ou de somme de contrôle, comme si aucune erreur ne s'était produite. Cette procédure améliore les chances de récupérer les données de la base de données.    
-   DBCC CHECKDB tente de récupérer la base de données au moyen de techniques classiques de récupération basées sur les fichiers journaux.    
-   Si, en raison de la corruption du journal des transactions, la récupération de la base de données échoue, le journal de transactions est reconstruit. La reconstruction du journal des transactions peut nuire à la cohérence transactionnelle.    
    
> [!WARNING]
> L'option REPAIR_ALLOW_DATA_LOSS est une fonctionnalité prise en charge de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutefois, il ne s'agit pas toujours nécessairement de la meilleure option pour mettre une base de données à un état physiquement cohérent. En cas de réussite, l'option REPAIR_ALLOW_DATA_LOSS peut entraîner une perte de données. En fait, elle peut entraîner une perte de données supérieure à celle que vous pourriez constater si un utilisateur restaurait la base de données à partir de la dernière bonne sauvegarde. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande toujours d'effectuer une restauration utilisateur à partir de la dernière bonne sauvegarde comme principale méthode pour récupérer suite aux erreurs signalées par DBCC CHECKDB. L’option REPAIR_ALLOW_DATA_LOSS n’est **pas** une alternative pour une restauration à partir d’une sauvegarde reconnue fiable. Il s'agit d'une option d'urgence de « dernier recours » recommandée uniquement si la restauration à partir d'une sauvegarde n'est pas possible.    
>     
>  Après la reconstruction du journal, il n'existe aucune garantie ACID totale.    
>     
>  Après la reconstruction du journal, DBCC CHECKDB est exécutée automatiquement et signale et corrige les problèmes de cohérence physique.    
>     
>  La cohérence logique des données et les contraintes appliquées par la logique d'entreprise doivent être validées manuellement.    
>     
>  La taille du journal des transactions reste à sa taille par défaut et doit être réajustée manuellement à sa taille récente.    
    
Si la commande DBCC CHECKDB réussit, la base de données est physiquement cohérente et son état prend la valeur ONLINE. Toutefois, la base de données peut contenir une ou plusieurs incohérences transactionnelles. Nous vous recommandons d’exécuter [DBCC CHECKCONSTRAINTS](../../t-sql/database-console-commands/dbcc-checkconstraints-transact-sql.md) pour identifier tout défaut de logique métier et de sauvegarder immédiatement la base de données.
Si la commande DBCC CHECKDB échoue, la base de données ne peut pas être réparée.
    
## <a name="running-dbcc-checkdb-with-repairallowdataloss-in-replicated-databases"></a>Exécution de DBCC CHECKDB avec REPAIR_ALLOW_DATA_LOSS dans des bases de données répliquées    
L'exécution de la commande DBCC CHECKDB avec l'option REPAIR_ALLOW_DATA_LOSS peut avoir des conséquences sur les bases de données utilisateur (bases de données de publication et d'abonnement) ainsi que sur la base de données de distribution utilisée par la réplication. Les bases de données de publication et d'abonnement incluent des tables publiées et des tables de métadonnées de réplication. Sachez que les problèmes suivants peuvent se poser dans ces bases de données :
-   Tables publiées. Il est possible que les actions effectuées par le processus CHECKDB pour réparer les données utilisateur corrompues ne soient pas répliquées :    
-   La réplication de fusion utilise des déclencheurs pour assurer le suivi des modifications apportées aux tables publiées. Si des lignes sont insérées, mises à jour ou supprimées par le processus CHECKDB, les déclencheurs ne sont pas activés : la modification n'est donc pas répliquée.
-   La réplication transactionnelle utilise le journal des transactions pour assurer le suivi des modifications apportées aux tables publiées. L'Agent de lecture du journal place ensuite ces modifications dans la base de données de distribution. Certaines réparations de DBCC, bien qu'elles soient consignées dans le journal, ne peuvent pas être répliquées par l'Agent de lecture du journal. Par exemple, si une page de données est désallouée par le processus CHECKDB, l'Agent de lecture du journal ne la convertit pas en instruction DELETE ; par conséquent, la modification n'est pas répliquée.
-   Tables de métadonnées de réplication. Pour les actions effectuées par le processus CHECKDB afin de réparer les tables de métadonnées de réplication corrompues, la réplication doit être supprimée puis reconfigurée.    
    
Si vous devez exécuter la commande DBCC CHECKDB avec l'option REPAIR_ALLOW_DATA_LOSS sur une base de données utilisateur ou de distribution :
1.  Suspendez le système : arrêtez l'activité sur la base de données et toutes les autres bases de données appartenant à la topologie de réplication, puis tentez de synchroniser tous les nœuds. Pour plus d’informations, consultez [Suspendre une topologie de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).    
1.  Exécutez DBCC CHECKDB.    
1.  Si le rapport DBCC CHECKDB inclut des réparations pour des tables de la base de données de distribution ou des tables de métadonnées de réplication dans une base de données utilisateur, supprimez et reconfigurez la réplication. Pour plus d’informations, consultez [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md).    
1.  Si le rapport DBCC CHECKDB inclut des réparations pour des tables répliquées, procédez à une validation des données pour déterminer s'il existe des différences entre les données de la base de données de publication et celles de la base de données d'abonnement.    
    
## <a name="result-sets"></a>Jeux de résultats    
DBCC CHECKDB retourne le jeu de résultats suivant. Les valeurs risquent de varier, à moins que les options ESTIMATEONLY, PHYSICAL_ONLY ou NO_INFOMSGS soient spécifiées :
    
```
 DBCC results for 'model'.    
    
 Service Broker Msg 9675, Level 10, State 1: Message Types analyzed: 13.    
    
 Service Broker Msg 9676, Level 10, State 1: Service Contracts analyzed: 5.    
    
 Service Broker Msg 9667, Level 10, State 1: Services analyzed: 3.    
    
 Service Broker Msg 9668, Level 10, State 1: Service Queues analyzed: 3.    
    
 Service Broker Msg 9669, Level 10, State 1: Conversation Endpoints analyzed: 0.    
    
 Service Broker Msg 9674, Level 10, State 1: Conversation Groups analyzed: 0.    
    
 Service Broker Msg 9670, Level 10, State 1: Remote Service Bindings analyzed: 0.    
    
 DBCC results for 'sys.sysrowsetcolumns'.    
    
 There are 630 rows in 7 pages for object 'sys.sysrowsetcolumns'.    
    
 DBCC results for 'sys.sysrowsets'.    
    
 There are 97 rows in 1 pages for object 'sys.sysrowsets'.    
    
 DBCC results for 'sysallocunits'.    
    
 There are 195 rows in 3 pages for object 'sysallocunits'.    
    
 There are 0 rows in 0 pages for object "sys.sysasymkeys".    
    
 DBCC results for 'sys.syssqlguides'.    
    
 There are 0 rows in 0 pages for object "sys.syssqlguides".    
    
 DBCC results for 'sys.queue_messages_1977058079'.    
    
 There are 0 rows in 0 pages for object "sys.queue_messages_1977058079".    
    
 DBCC results for 'sys.queue_messages_2009058193'.    
    
 There are 0 rows in 0 pages for object "sys.queue_messages_2009058193".    
    
 DBCC results for 'sys.queue_messages_2041058307'.    
    
 There are 0 rows in 0 pages for object "sys.queue_messages_2041058307".    
    
 CHECKDB found 0 allocation errors and 0 consistency errors in database 'model'.    
    
 DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```

DBCC CHECKDB retourne le jeu de résultats suivant (message) si NO_INFOMSGS est spécifié :
    
```
 The command(s) completed successfully.
 ```
 
DBCC CHECKDB retourne le jeu de résultats suivant si PHYSICAL_ONLY est spécifié :
    
```
 DBCC results for 'model'.    
    
 CHECKDB found 0 allocation errors and 0 consistency errors in database 'master'.  
    
 DBCC execution completed. If DBCC printed error messages, contact your system administrator.
 ```
 
DBCC CHECKDB retourne le jeu de résultats suivant si ESTIMATEONLY est spécifié :
    
```
 Estimated TEMPDB space needed for CHECKALLOC (KB)    
    
 -------------------------------------------------  
    
 13   
    
 (1 row(s) affected)   
    
 Estimated TEMPDB space needed for CHECKTABLES (KB)    
    
 --------------------------------------------------    
    
 57 
    
 (1 row(s) affected)  
    
 DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```
    
## <a name="permissions"></a>Autorisations    
Nécessite l’appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner.
    
## <a name="examples"></a>Exemples    
    
### <a name="a-checking-both-the-current-and-another-database"></a>A. Vérification de la base de données active et d'une autre base de données    
L'exemple suivant exécute `DBCC CHECKDB` pour la base de données actuelle et pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
    
```sql    
-- Check the current database.    
DBCC CHECKDB;    
GO    
-- Check the AdventureWorks2012 database without nonclustered indexes.    
DBCC CHECKDB (AdventureWorks2012, NOINDEX);    
GO    
```    
    
### <a name="b-checking-the-current-database-suppressing-informational-messages"></a>B. Vérification de la base de données actuelle et suppression des messages d'information    
L'exemple suivant vérifie la base de données actuelle et supprime tous les messages d'information.
    
```sql    
DBCC CHECKDB WITH NO_INFOMSGS;    
GO    
```    
    
## <a name="see-also"></a> Voir aussi    
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md)  
[sp_helpdb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)  
[Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  

