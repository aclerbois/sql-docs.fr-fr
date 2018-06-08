---
title: Le vérificateur de cohérence (DBCC) pour Analysis Services de base de données | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cb131f76c839f446cbdc31dae51e98431bb87902
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="database-consistency-checker-dbcc-for-analysis-services"></a>Vérificateur de cohérence de base de données (DBCC) pour Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  DBCC assure la validation de base de données à la demande pour les bases de données multidimensionnelles et tabulaires sur une instance Analysis Services. Vous pouvez exécuter DBCC dans une fenêtre de requête MDX ou XMLA dans SQL Server Management Studio (SSMS) et tracer la sortie DBCC dans SQL Server Profiler ou des sessions xEvent dans SSMS.  
La commande prend une définition d’objet et retourne un jeu de résultats vide ou des informations d’erreur détaillées si l’objet est endommagé.   Dans cet article, vous allez apprendre à exécuter la commande, à interpréter les résultats et à traiter tous les problèmes qui surviennent.  
  
 Pour les bases de données tabulaires, les vérifications de cohérence par DBCC sont équivalentes à la validation intégrée qui se produit automatiquement chaque fois que vous rechargez, synchronisez ou restaurez une base de données.  En revanche, les vérifications de cohérence pour les bases de données multidimensionnelles n’interviennent que quand vous exécutez DBCC à la demande.  
  
 La plage des contrôles de validation varie suivant le mode, les bases de données tabulaires étant soumises à une plage de contrôles plus étendue.  
 Les caractéristiques d’une charge de travail DBCC varient également en fonction du mode serveur. Les opérations de contrôle sur les bases de données multidimensionnelles impliquent la lecture des données à partir du disque et la construction d’index temporaires en vue d’une comparaison avec les index réels, ensemble d’opérations qui prend beaucoup plus de temps.  
  
 La syntaxe de la commande DBCC utilise les métadonnées d’objets propres au type de base de données que vous voulez vérifier :  
  
-   Les bases de données au niveau de compatibilité 1100 ou 1103 tabulaires antérieures à SQL Server 2016 et multidimensionnelles sont décrites dans des constructions de modélisation multidimensionnelle telles que **cubeID**, **measuregroupID**et **partitionID**.  
  
-   Les métadonnées des nouvelles bases de données de modèle tabulaire au niveau de compatibilité 1200 et supérieur se composent de descripteurs tels que **TableName** et **PartitionName**.  
  
 DBCC pour Analysis Services s’exécute sur toute base de données Analysis Services à n’importe quel niveau de compatibilité, tant que la base de données s’exécute sur une instance de SQL Server 2016. Vérifiez simplement que vous utilisez la syntaxe de commande appropriée pour chaque type de base de données.  
  
> [!NOTE]  
>  Si vous connaissez [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md), vous remarquerez rapidement que la portée de DBCC dans Analysis Services est beaucoup plus restreinte. DBCC dans Analysis Services est une commande unique dont les rapports portent exclusivement sur l’altération des données affectant la base de données ou des objets spécifiques. Si vous avez d’autres tâches à l’esprit, telles que la collecte d’informations, essayez d’utiliser des scripts PowerShell AMO ou XMLA à la place. Pour obtenir des liens vers d’autres informations, consultez [Monitor an Analysis Services Instance](../../analysis-services/instances/monitor-an-analysis-services-instance.md) .  
  
## <a name="permission-requirements"></a>Spécifications relatives aux autorisations  
 Vous devez être administrateur de serveur ou de base de données Analysis Services (membre du rôle serveur) pour exécuter la commande. Pour obtenir des instructions, consultez [Octroyer des autorisations de base de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md) ou [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md).  
  
## <a name="command-syntax"></a>Syntaxe de commande 
 Bases de données tabulaires 1200 et les niveaux de compatibilité plus élevés utilisent des métadonnées tabulaires pour les définitions d’objet. La syntaxe complète de la commande DBCC pour une base de données tabulaire créée à un niveau fonctionnel SQL Server 2016 est illustrée dans l’exemple suivant.  
  
 Principales différences entre les deux syntaxes incluent un espace de noms XMLA plus récent, ne \<objet > élément et aucun \<modèle > élément (il est toujours un seul modèle par base de données).  
  
```  
<DBCC xmlns="http://schemas.microsoft.com/analysisservices/2014/engine">  
     <DatabaseID>MyTabular1200DB_7811b5d8-c407-4203-8793-12e16c3d1b9b</DatabaseID>  
     <TableName>FactSales</TableName>  
     <PartitionName>FactSales 4</PartitionName>  
</DBCC>  
```  
  
 Vous pouvez omettre les objets de niveau inférieur, tels que les noms de table ou de partition, pour vérifier la totalité du schéma.  
  
 Vous pouvez obtenir des noms d’objets et l’élément DatabaseID à partir de Management Studio, par le biais de la page de propriétés de chaque objet.  
  
## <a name="command-syntax-for-multidimensional-and-tabular-110x-databases"></a>Syntaxe de la commande pour les bases de données 110x tabulaires et multidimensionnelles  
 DBCC utilise une syntaxe identique pour les bases de données 1100 et 1103 tabulaires et multidimensionnelles. Vous pouvez exécuter DBCC sur des objets de base de données spécifiques, y compris sur la base de données entière. Pour plus d’informations sur la définition des objets, consultez [Élément Object &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md).  
  
```  
<DBCC xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
     <Object>  
          <DatabaseID>AdventureWorksDW2014Multidimensional-EE</DatabaseID>  
          <CubeID>Adventure Works</CubeID>  
          <MeasureGroupID>Fact Internet Sales 1</MeasureGroupID>  
          <PartitionID>Internet_Sales_2006</PartitionID>  
     </Object>  
</DBCC>  
  
```  
  
 Pour exécuter DBCC sur des objets situés plus haut dans la chaîne d’objets, supprimez tout élément d’ID d’objet de niveau inférieur superflu :  
  
```  
<DBCC xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
     <Object>  
          <DatabaseID>AdventureWorksDW2014Multidimensional-EE</DatabaseID>  
          <CubeID>Adventure Works</CubeID>  
     </Object>  
</DBCC>  
  
```  
  
 Pour les bases de données x110 tabulaires, la syntaxe de la définition des objets est modélisée d’après la syntaxe de la commande Process (en particulier, concernant le mappage des tables aux dimensions et groupes de mesures).  
  
-   **CubeID** correspond à l’ID de modèle, qui est **Model**.  
  
-   **MeasureGroupID** correspond à un ID de table.  
  
-   **PartitionID** correspond à un ID de partition.  
  
## <a name="usage"></a>Utilisation  
 Dans SQL Server Management Studio, vous pouvez appeler DBCC à l’aide d’une fenêtre de requête MDX ou XMLA. En outre, vous pouvez utiliser des événements xEvent Analysis Services ou [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Profiler pour afficher la sortie de la commande DBCC. Notez que les messages DBCC SSAS ne sont pas envoyés au journal des événements Windows ou au fichier msmdsrv.log.  
  
 DBCC vérifie si la présence de membres orphelins dans un segment n’entraîne pas l’altération physique ou logique des données. Vous devez traiter une base de données avant d’exécuter DBCC. La commande ignore les partitions distantes, vides ou non traitées.  
  
 S’exécutant dans une transaction de lecture, elle peut être rejetée par le délai d’attente de validation de force. Les vérifications de partition sont exécutées en parallèle.  
  
 Un redémarrage du service peut être nécessaire pour récupérer les erreurs d’endommagement qui se sont produites depuis le dernier redémarrage du service. Rétablir la connexion au serveur n’est pas suffisant pour récupérer les modifications.  
  
### <a name="run-dbcc-commands-in-management-studio"></a>Exécuter des commandes DBCC dans Management Studio  
 Pour les requêtes ad hoc, ouvrez une fenêtre de requête MDX ou XMLA dans SQL Server Management Studio. Pour ce faire, cliquez sur la base de données avec le bouton droit | **Nouvelle requête** | **XMLA**pour exécuter la commande et lire la sortie.  
  
 ![Commande DBCC XML dans Management Studio](../../analysis-services/instances/media/ssas-dbcc-ssms.gif "commande DBCC XML dans Management Studio")  
  
 L’onglet Résultats indique un jeu de résultats vide (comme indiqué dans la capture d’écran) si aucun problème n’a été détecté.  
  
 L’onglet Messages fournit des informations détaillées, mais n’est pas toujours fiable pour les bases de données plus petites. Les messages d’état sont parfois réduits, indiquant que la commande est terminée, tout en étant dépourvus des messages de vérification d’état associés à chaque objet. Un rapport de message classique peut ressembler à celui illustré ci-dessous.  
  
 **Messages signalés par DBCC pour le contrôle de validation d’un cube**  
  
```  
Executing the query ...  
READS, 0  
READ_KB, 0  
WRITES, 0  
WRITE_KB, 0  
CPU_TIME_MS, 0  
ROWS_SCANNED, 0  
ROWS_RETURNED, 0  
  
<DBCC xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
<Object>  
<DatabaseID>AdventureWorksDW2014Multidimensional-EE</DatabaseID>  
<CubeID>Adventure Works</CubeID>  
</Object>  
</DBCC>  
Started checking segment indexes for the 'Internet_Sales_2011' partition.  
Started checking segment indexes for the 'Internet_Sales_2012' partition.  
Finished checking segment indexes for the 'Internet_Sales_2011' partition.  
Started checking segment indexes for the 'Internet_Sales_2013' partition.  
Finished checking segment indexes for the 'Internet_Sales_2012' partition.  
Started checking segment indexes for the 'Internet_Sales_2014' partition.  
Started checking segment indexes for the 'Internet_Orders_2011' partition.  
Finished checking segment indexes for the 'Internet_Sales_2014' partition.  
Started checking segment indexes for the 'Internet_Orders_2012' partition.  
Started checking segment indexes for the 'Internet_Orders_2013' partition.  
Finished checking segment indexes for the 'Internet_Orders_2012' partition.  
...   
Run complete  
  
```  
  
 **Sortie de l’exécution de DBCC sur une version antérieure d’Analysis Services**  
  
 La commande DBCC est uniquement prise en charge sur les bases de données exécutant une instance [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Exécuter la commande sur d’anciens systèmes retourne l’erreur ci-après.  
  
```  
Executing the query ...  
The DBCC element at line 7, column 87 (namespace http://schemas.microsoft.com/analysisservices/2003/engine) cannot appear under Envelope/Body/Execute/Command.  
Execution complete  
  
```  
  
### <a name="trace-dbcc-output-in-sql-server-profiler-2016"></a>Tracer la sortie DBCC dans SQL Server Profiler 2016  
 Vous pouvez afficher la sortie DBCC dans une trace Profiler qui inclut les événements des rapports de progression (Début du rapport de progression, Rapport de progression actuel, Fin du rapport de progression et Erreur du rapport de progression).  
  
1.  Démarrez une trace. Pour obtenir de l’aide sur l’utilisation de SQL Server Profiler avec Analysis Services, consultez [Use SQL Server Profiler to Monitor Analysis Services](../../analysis-services/instances/use-sql-server-profiler-to-monitor-analysis-services.md) .  
  
2.  Choisissez **Command Begin** et **Command End** ainsi que tout ou partie des événements **Progress Report** .  
  
3.  Exécutez la commande DBCC dans Management Studio, dans une fenêtre de requête XMLA ou MDX, à l’aide de la syntaxe fournie dans une section précédente.  
  
4.  Dans SQL Server Profiler, l’activité DBCC est indiquée par le biais d’événements **Command** ayant une sous-classe d’événements de DBCC :  
  
     ![ssas-dbcc-profiler-eventsubclass](../../analysis-services/instances/media/ssas-dbcc-profiler-eventsubclass.PNG "ssas-dbcc-profiler-eventsubclass")  
  
     Le code d’événement 32 est l’exécution de DBCC.  
  
     Le code d’événement 64 est un rapport de progression DBCC sur des objets individuels.  
  
     Le code d’événement 63 est une vérification de segment pour les objets multidimensionnels.  
  
     Pour les deux sous-classes d’événements, consultez les valeurs **TextData** , qui peuvent contenir des messages retournés par DBCC.  
  
     Messages d’état commencent par « la vérification de cohérence de \<objet > », « début de la vérification \<objet > », ou » terminé la vérification \<objet > ».  
  
    > [!NOTE]  
    >  Dans CTP 3.0, les objets sont identifiés par des noms internes. Par exemple, une hiérarchie Categories est articulée sous la forme H$ Categories -\<objectID >. Les noms internes devraient être remplacés par des noms conviviaux dans une prochaine version de CTP.  
  
     Les messages d’erreur sont répertoriés ci-dessous.  
  
### <a name="trace-dbcc-output-in-an-xevent-session-in-ssms"></a>Tracer la sortie de la commande DBCC dans une session xEvent au sein de SSMS  
 Les sessions d’événements étendus peuvent utiliser des événements Profiler ou xEvent. Reportez-vous à la section précédente pour obtenir des instructions sur l’ajout d’événements **Command** et **Progress Report** .  
  
1.  Démarrez une session en cliquant sur une base de données avec le bouton droit > **Gestion** >**Événements étendus** >  **Sessions** > **Nouvelle session**. Pour plus d’informations, consultez  [Monitor Analysis Services with SQL Server Extended Events](../../analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events.md) .  
  
2.  Choisissez tout ou partie des événements **Progress Report** (Rapport de progression) pour la catégorie d’événements Profiler ou des événements **RequestProgress** (Demander un rapport de progression) pour la catégorie PureXevent.  
  
3.  Exécutez la commande DBCC dans Management Studio, dans une fenêtre de requête XMLA ou MDX, à l’aide de la syntaxe fournie dans une section précédente.  
  
4.  Dans SSMS, actualisez le dossier Sessions. Cliquez avec le bouton droit sur le nom de session, puis sélectionnez **Observer les données actives à l’écran lors de leur capture**.  
  
5.  Examinez les valeurs TextData, qui peuvent contenir des messages retournés par DBCC.  TextData est une propriété d’un champ d’événement et affiche les messages d’état et d’erreur retournés par l’événement.  
  
     Messages d’état commencent par « la vérification de cohérence de \<objet > », « début de la vérification \<objet > », ou » terminé la vérification \<objet > ».  
  
     Les messages d’erreur sont répertoriés ci-dessous.  
  
## <a name="reference-consistency-checks-and-errors-for-multidimensional-databases"></a>Référence : erreurs et vérifications de cohérence pour les bases de données multidimensionnelles  
 Pour les bases de données multidimensionnelles, seuls les index de partition sont validés.  Pendant l’exécution, DBCC génère un index temporaire pour chaque partition et le compare à l’index persistant sur disque.  Créer un index temporaire requiert la lecture de toutes les données à partir des données de partition sur le disque, puis la conservation de l’index temporaire en mémoire en vue de la comparaison. Étant donné la charge de travail supplémentaire, votre serveur peut connaître un nombre élevé d’opérations d’E/S sur disque ou une consommation de mémoire significative pendant l’exécution d’une commande DBCC.  
  
 La détection de l’endommagement d’un index multidimensionnel inclut les vérifications suivantes. Les erreurs répertoriées dans ce tableau apparaissent dans les traces xEvent ou Profiler pour les échecs au niveau de l’objet.  
  
||||  
|-|-|-|  
|**Objet**|**Description de la vérification DBCC**|**Erreur en cas d’échec**|  
|Index de partition|Vérifie les index et les statistiques des segments.<br /><br /> Compare l’ID de chaque membre dans l’index de partition temporaire aux statistiques de partition stockées sur le disque.  Si un membre dans l’index temporaire a une valeur d’ID de données située en dehors de la plage stockée pour les statistiques d’index de partition sur le disque, les statistiques de l’index sont considérées comme endommagées.|Les statistiques des segments de partition sont endommagées.|  
|Index de partition|Valide les métadonnées.<br /><br /> Vérifie que chaque membre de l’index temporaire figure dans le fichier d’en-tête d’index pour le segment sur le disque.|Le segment de partition est endommagé.|  
|Index de partition|Analyse les segments à la recherche d’altérations physiques.<br /><br /> Lit le fichier d’index sur disque pour chaque membre de l’index temporaire, et vérifie que la taille des enregistrements de l’index correspond et que les mêmes pages de données sont marquées comme ayant des enregistrements pour le membre actuel.|Le segment de partition est endommagé.|  
  
## <a name="reference-consistency-checks-and-errors-for-tabular-databases"></a>Référence : erreurs et vérifications de cohérence pour les bases de données tabulaires  
 Le tableau suivant présente la liste de toutes les vérifications de cohérence effectuées sur les objets tabulaires, ainsi que les erreurs qui sont déclenchées si la vérification indique un endommagement. Les erreurs répertoriées dans ce tableau apparaissent dans les traces xEvent ou Profiler pour les échecs au niveau de l’objet.  
  
||||  
|-|-|-|  
|**Objet**|**Description de la vérification DBCC**|**Erreur en cas d’échec**|  
|Base de données|Vérifie le nombre de tables dans la base de données.  Une valeur inférieure à zéro indique un endommagement.|La couche de stockage est endommagée. La collection de tables de la base de données ’%{parent/}’ est endommagée.|  
|Base de données|Vérifie la structure interne utilisée pour tracer l’intégrité référentielle et lève une erreur si la taille est incorrecte.|Les fichiers de base de données n’ont pas réussi les vérifications de cohérence.|  
|Table|Vérifie la valeur interne utilisée pour déterminer si la table est une table de dimensions ou de faits.  Une valeur qui se situe en dehors de la plage connue indique un endommagement.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des tables.|  
|Table|Vérifie que le nombre de partitions dans le mappage des segments de la table correspond au nombre de partitions définies pour la table.|La couche de stockage est endommagée. La collection de partitions de la table ’%{parent/}’ est endommagée.|  
|Table|Si une base de données tabulaire a été créée ou importée à partir de PowerPivot pour Excel 2010 et qu’elle comporte plus d’une partition, une erreur est déclenchée, indiquant une corruption (la prise en charge des partitions a été ajoutée aux versions ultérieures).|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification du mappage de segments.|  
|Partition|Pour chaque partition, vérifie que le nombre de segments de données et le nombre d’enregistrements pour chaque segment de données dans le segment correspondent aux valeurs stockées dans l’index du segment.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification du mappage de segments.|  
|Partition|Génère une erreur si le nombre total d’enregistrements, de segments ou d’enregistrements par segment n’est pas valide (inférieur à zéro), ou que le nombre de segments ne correspond pas au nombre calculé de segments nécessaires selon le nombre total d’enregistrements.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification du mappage de segments.|  
|Relation|Génère une erreur si la structure utilisée pour stocker les données relatives à la relation ne contient aucun enregistrement ou que le nom de la table utilisée dans la relation est vide.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des relations.|  
|Relation|Vérifie que les noms de la table primaire, de la colonne primaire, de la table étrangère et de la colonne étrangère sont définis et que les colonnes et les tables impliquées dans la relation sont accessibles.<br /><br /> Vérifie que les types de colonnes impliqués sont valides et que l’index des valeurs clé étrangère-clé primaire aboutit à une structure de recherche valide.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des relations.|  
|Hiérarchie|Génère une erreur si l’ordre de tri pour la hiérarchie n’est pas une valeur reconnue.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification de la hiérarchie '%{hier/}'.|  
|Hiérarchie|Les vérifications effectuées sur la hiérarchie varient selon le type interne du schéma de mappage de la hiérarchie utilisé.<br /><br /> Pour toutes les hiérarchies, il est vérifié que l’état de traitement est correct, que la banque de hiérarchie existe et, le cas échéant, que les structures de données utilisées pour une conversion d’ID de données en position dans la hiérarchie existent.<br /><br /> En supposant que toutes ces vérifications sont validées, la structure de la hiérarchie est parcourue pour vérifier que chaque position dans la hiérarchie pointe vers le membre approprié.<br />Si l’un de ces tests échoue, une erreur est générée.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification de la hiérarchie '%{hier/}'.|  
|Hiérarchie définie par l’utilisateur|Vérifie que les noms de niveaux de hiérarchie sont définis.<br /><br /> Si la hiérarchie a été traitée, vérifie que la banque de données de hiérarchie interne a le format correct.  Vérifie que la banque de hiérarchie interne ne contient pas de valeurs de données non valides.<br /><br /> Si la hiérarchie est marquée comme non traitée, vérifie que cet état s’applique aux anciennes structures de données et que tous les niveaux de la hiérarchie sont marqués comme étant vides.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification de la hiérarchie '%{hier/}'.|  
|Colonne|Génère une erreur si l’encodage utilisé pour la colonne n’est pas défini sur une valeur connue.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des colonnes.|  
|Colonne|Vérifie si la colonne a été compressée par le moteur en mémoire ou non.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des colonnes.|  
|Colonne|Vérifie le type de compression sur la colonne pour les valeurs connues.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des colonnes.|  
|Colonne|Quand la « création de jetons » pour la colonne n’est pas définie sur une valeur connue, génère une erreur.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des colonnes.|  
|Colonne|Si la plage d’ID stockée pour un dictionnaire de données de colonnes ne correspond pas au nombre de valeurs dans le dictionnaire de données ou qu’elle se situe en dehors de la plage autorisée, génère une erreur.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification du dictionnaire de données.|  
|Colonne|Vérifie que le nombre de segments de données pour une colonne correspond au nombre de segments de données pour la table à laquelle elle appartient.|La couche de stockage est endommagée. La collection de segments de la colonne '%{parent/}' est endommagée.|  
|Colonne|Vérifie que le nombre de partitions pour une colonne de données correspond au nombre de partitions pour le mappage de segments de données pour la colonne.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification du mappage de segments.|  
|Colonne|Vérifie que le nombre d’enregistrements dans un segment de colonne correspond au nombre d’enregistrements stocké dans l’index pour ce segment de colonne.|La couche de stockage est endommagée. La collection de segments de la colonne '%{parent/}' est endommagée.|  
|Colonne|Si une colonne n’a aucune statistique de segment, génère une erreur.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des segments.|  
|Colonne|Si une colonne est dépourvue d’informations de compression ou de stockage de segment, génère une erreur.|Les fichiers de base de données n’ont pas réussi les vérifications de cohérence.|  
|Colonne|Signale une erreur si les statistiques de segment pour une colonne ne correspondent pas aux valeurs de colonne réelles pour l’ID de données minimal, l’ID de données maximal, le nombre de valeurs distinctes, nombre de lignes ou la présence de valeurs NULL.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des segments.|  
|Segment de colonne|Si les ID de données minimal ou maximal sont inférieurs à la valeur système réservée pour la valeur NULL, marque les informations de segment de colonne comme corrompues.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des segments.|  
|Segment de colonne|S’il n’y a aucune ligne pour ce segment, les valeurs de données minimale et maximale pour la colonne doivent être définies sur la valeur système réservée pour la valeur NULL.  Si la valeur n’est pas NULL, génère une erreur.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des segments.|  
|Segment de colonne|Si la colonne contient des lignes et au moins une valeur non NULL, vérifie que les ID de données minimal et maximal pour la colonne sont supérieurs à la valeur système réservée pour la valeur NULL.|Échec des vérifications de cohérence de la base de données (DBCC) lors de la vérification des statistiques des segments.|  
|Interne|Vérifie que l’indicateur de création de jetons pour la banque est défini et que, si la banque est traitée, il existe des pointeurs valides pour les tables internes.  Si la banque n’est pas traitée, vérifie que tous les pointeurs sont NULL.<br />Si ce n’est pas le cas, retourne une erreur DBCC générique.|Les fichiers de base de données n’ont pas réussi les vérifications de cohérence.|  
|Base de données DBCC|Génère une erreur si le schéma de la base de données ne comporte aucune table ou qu’une ou plusieurs tables sont inaccessibles.|La couche de stockage est endommagée. La collection de tables de la base de données ’%{parent/}’ est endommagée.|  
|Base de données DBCC|Génère une erreur si une table est marquée comme temporaire ou est de type inconnu.|Un type de table incorrect a été rencontré.|  
|Base de données DBCC|Génère une erreur si le nombre de relations pour une table a une valeur négative, ou si une relation est définie pour une table et qu’une structure de relations correspondante est introuvable.|La couche de stockage est endommagée. La collection de relations de la table '%{parent/}' est endommagée.|  
|Base de données DBCC|Si le niveau de compatibilité pour la base de données est 1050 (SQL Server 2008 R2/PowerPivot v1.0) et que le nombre de relations dépasse le nombre de tables dans le modèle, marque la base de données comme étant endommagée.|Les fichiers de base de données n’ont pas réussi les vérifications de cohérence.|  
|Table DBCC|Pour la table en cours de validation, vérifie si le nombre de colonnes est inférieur à zéro et génère une erreur si la valeur est true.  Une erreur se produit également si la banque des colonnes pour une colonne de la table est NULL.|La couche de stockage est endommagée. La collection de colonnes de la table '%{parent/}' est endommagée.|  
|Partition DBCC|Vérifie la table à laquelle appartient la partition en cours de validation et, si le nombre de colonnes de la table est inférieur à zéro, indique que la collection de colonnes est endommagée pour la table. Une erreur se produit également si la banque des colonnes pour une colonne dans la table est NULL.|La couche de stockage est endommagée. La collection de colonnes de la table '%{parent/}' est endommagée.|  
|Partition DBCC|Effectue une itération sur chaque colonne de la partition sélectionnée et vérifie que chaque segment de la partition possède un lien valide vers une structure de segment de colonne.  Si un segment possède un lien NULL, la partition est considérée comme endommagée.|La couche de stockage est endommagée. La collection de segments de la colonne '%{parent/}' est endommagée.|  
|Colonne|Retourne une erreur si le type de colonne n’est pas valide.|Un type de segment incorrect a été rencontré.|  
|Colonne|Renvoie une erreur si une colonne a un nombre négatif de segments dans une colonne ou que le pointeur vers la structure de segment de colonne pour un segment possède un lien NULL.|La couche de stockage est endommagée. La collection de segments de la colonne '%{parent/}' est endommagée.|  
|Commande DBCC|La commande DBCC signale plusieurs messages d’état au fil de son traitement par DBCC.  Avant de démarrer et à l’issue de chaque vérification d’objet, elle indique un message d’état qui inclut le nom de base de données, de table ou de colonne de l’objet.|Vérification de la cohérence de la \<nomobjet > \<typeobjet >. Phase : prévérification.<br /><br /> Vérification de la cohérence de la \<nomobjet > \<typeobjet >. Phase : post-vérification.|  
  
## <a name="common-resolutions-for-error-conditions"></a>Solutions courantes pour les conditions d’erreur  
 Les erreurs suivantes s’affichent dans SQL Server Management Studio ou dans les fichiers msmdsrv.log. Ces erreurs se produisent quand une ou plusieurs vérifications échouent. Selon l’erreur, la résolution recommandée consiste soit à traiter à nouveau un objet, à supprimer et à redéployer une solution, soit à restaurer la base de données.  
  
|Erreur|Problème|Résolution|  
|-----------|-----------|----------------|  
|**Erreurs dans le gestionnaire de métadonnées**<br /><br /> La référence d’objet '\<objectID >' n’est pas valide. Elle ne correspond pas à la structure de la hiérarchie de classes de métadonnées.|Commande mal constituée|Vérifiez la syntaxe de la commande. Très probablement, vous avez inclus un objet de niveau inférieur sans spécifier un ou plusieurs de ses objets parents.|  
|**Erreurs dans le gestionnaire de métadonnées**<br /><br /> Soit le \<objet > portant l’ID '\<objectID >' n’existe pas dans le \<parentobject > portant l’ID '\<parentobjectID >', ou l’utilisateur ne dispose pas des autorisations nécessaires pour accéder à l’objet.|Endommagement de l’index (multidimensionnel)|Retraitez l’objet et tous les objets dépendants.|  
|**Une erreur s’est produite pendant la vérification de la cohérence de la partition**<br /><br /> Une erreur s’est produite lors de la vérification de cohérence de la \<-nom de la partition > partition de la \<nom de groupe de mesures > groupe de mesures pour le \<-nom du cube > du cube à partir de la \<nom de la base de données > base de données. Retraitez la partition ou les index pour résoudre le problème.|Endommagement de l’index (multidimensionnel)|Retraitez l’objet et tous les objets dépendants.|  
|**Statistiques des segments de partition endommagées**|Endommagement de l’index (multidimensionnel)|Retraitez l’objet et tous les objets dépendants.|  
|**Segment de partition endommagé**|Endommagement des métadonnées (multidimensionnelles ou tabulaires)|Supprimez et redéployez le projet, ou effectuez une restauration à partir d’une sauvegarde et procédez de nouveau au traitement.<br /><br /> Pour obtenir des instructions, consultez le billet de blog [How to Deal with Corruption in Analysis Services](http://blogs.msdn.com/b/karang/archive/2010/08/11/how-to-deal-with-corruption-in-analysis-services.aspx) (Comment gérer l’endommagement dans Analysis Services).|  
|**Endommagement des métadonnées de table**<br /><br /> Table \<table-name > fichier de métadonnées est endommagé. La table principale ne se trouve pas sous le nœud DataFileList.|Endommagement des métadonnées (tabulaires uniquement)|Supprimez et redéployez le projet, ou effectuez une restauration à partir d’une sauvegarde et procédez de nouveau au traitement.<br /><br /> Pour obtenir des instructions, consultez le billet de blog [How to Deal with Corruption in Analysis Services](http://blogs.msdn.com/b/karang/archive/2010/08/11/how-to-deal-with-corruption-in-analysis-services.aspx) (Comment gérer l’endommagement dans Analysis Services).|  
|**Endommagement de la couche de stockage**<br /><br /> Une altération dans la couche de stockage : collection de \<type-name > dans \<parent-name > \<type de parent > est endommagé.|Endommagement des métadonnées (tabulaires uniquement)|Supprimez et redéployez le projet, ou effectuez une restauration à partir d’une sauvegarde et procédez de nouveau au traitement.<br /><br /> Pour obtenir des instructions, consultez le billet de blog [How to Deal with Corruption in Analysis Services](http://blogs.msdn.com/b/karang/archive/2010/08/11/how-to-deal-with-corruption-in-analysis-services.aspx) (Comment gérer l’endommagement dans Analysis Services).|  
|**Table système manquante**<br /><br /> Table système \<table-name > est manquant.|Endommagement des objets (tabulaires uniquement)|Retraitez l’objet et tous les objets dépendants.|  
|**Statistiques de table endommagées**<br /><br /> Statistiques de table système \<table-name > est manquant.|Endommagement des métadonnées (tabulaires uniquement)|Supprimez et redéployez le projet, ou effectuez une restauration à partir d’une sauvegarde et procédez de nouveau au traitement.<br /><br /> Pour obtenir des instructions, consultez le billet de blog [How to Deal with Corruption in Analysis Services](http://blogs.msdn.com/b/karang/archive/2010/08/11/how-to-deal-with-corruption-in-analysis-services.aspx) (Comment gérer l’endommagement dans Analysis Services).|  
  
## <a name="disable-automatic-consistency-checks-on-database-load-operations-through--the-msmdsrvini-configuration-file"></a>Désactiver les vérifications de cohérence automatiques sur les opérations de chargement de base de données par le biais du fichier de configuration msmdsrv.ini  
 Bien que cela ne soit pas recommandé, vous pouvez désactiver les vérifications de cohérence de base de données intégrées qui se produisent automatiquement sur les événements de chargement de base de données (sur les bases de données tabulaires uniquement). Pour ce faire, vous devez modifier un paramètre de configuration dans le fichier msmdsrv.ini :  
  
```  
<ConfigurationSettings>  
     <Vertipaq />  
          <DisableConsistencyChecks />  
```  
  
 Ce paramètre n’est pas présent dans le fichier de configuration et doit être ajouté manuellement.  
  
 Les valeurs valides sont les suivantes :  
  
-   **-2** : (valeur par défaut) DBCC est activé. Si le serveur peut résoudre logiquement l’erreur avec un degré élevé de certitude, un correctif est appliqué automatiquement. Sinon, une erreur est consignée.  
  
-   **-1** : DBCC est activé partiellement. Il est activé pour l’opération RESTORE et les validations qui vérifient l’état de la base de données à la fin d’une transaction, avant que celle-ci ne soit validée.  
  
-   **0** : DBCC est activé partiellement. Les vérifications de cohérence de la base de données sont effectuées au cours des opérations RESTORE, IMAGELOAD, LOCALCUBELOAD et ATTACH  
         .  
  
-   **1** : DBCC est désactivé. Les vérifications d’intégrité des données sont désactivées, mais les vérifications de la désérialisation se produisent toujours.  
  
> [!NOTE]  
>  Ce paramètre n’a aucun impact sur DBCC quand la commande est exécutée à la demande.  
  
## <a name="see-also"></a>Voir aussi  
 [Traiter une base de données, une table ou une partition &#40;Analysis Services&#41;](../../analysis-services/tabular-models/process-database-table-or-partition-analysis-services.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
 [Analyser une instance Analysis Services](../../analysis-services/instances/monitor-an-analysis-services-instance.md)   
 [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)   
 [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md)  
  
  
