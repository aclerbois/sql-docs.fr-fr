---
title: Activer la recherche sémantique sur les tables et les colonnes | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- semantic search [SQL Server], enabling
ms.assetid: 895d220c-6749-4954-9dd3-2ea4c6a321ff
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 67dff594a953d7650875e67bdb2ecc84703eca8d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="enable-semantic-search-on-tables-and-columns"></a>Activer la recherche sémantique sur les tables et les colonnes
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit la procédure d'activation ou de désactivation de l'indexation sémantique statistique sur des colonnes sélectionnées qui contiennent des documents ou du texte.  
  
 La recherche sémantique statistique utilise les index créés par la recherche en texte intégral et crée des index supplémentaires. Conséquence de cette dépendance sur la recherche en texte intégral, vous créez un index sémantique lorsque vous définissez un nouvel index de recherche en texte intégral ou lorsque vous modifiez un index de recherche en texte intégral existant. Vous pouvez créer un index sémantique à l'aide d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ou en utilisant l'Assistant Indexation de texte intégral et d'autres boîtes de dialogue de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], comme décrit dans cette rubrique.  
  
##  <a name="BasicEnabling"></a> Créer un index sémantique  
  
###  <a name="reqenable"></a> Exigences et restrictions concernant la création d'un index sémantique  
  
-   Vous pouvez créer un index sur des objets de base de données pris en charge pour l'indexation de texte intégral, notamment les tables et les vues indexées.  
  
-   Avant de pouvoir activer l'indexation sémantique pour des colonnes spécifiques, les conditions préalables suivantes doivent être remplies :  
  
    -   Un catalogue de texte intégral doit exister pour la base de données.  
  
    -   La table doit avoir un index de recherche en texte intégral.  
  
    -   Les colonnes sélectionnées doivent participer à l'index de recherche en texte intégral.  
  
     Vous pouvez créer et activer toutes ces conditions préalables en même temps.  
  
-   Vous pouvez créer un index sémantique sur des colonnes présentant l'un des types de données pris en charge pour l'indexation de texte intégral. Pour plus d’informations, consultez [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md).  
  
-   Vous pouvez spécifier tout type de document pris en charge pour l’indexation de texte intégral pour des colonnes **varbinary(max)** . Pour plus d'informations, consultez [Procédure : déterminer les types de documents pouvant être indexés](#doctypes) dans cette rubrique.  
  
-   L'indexation sémantique crée deux types d'index pour les colonnes que vous sélectionnez : un index d'expressions clés et un index de similarité de document. Vous ne pouvez pas sélectionner un seul type d'index lorsque vous activez l'indexation sémantique. En revanche, vous interroger ces deux index de manière indépendante. Pour plus d’informations, consultez [Rechercher des expressions clés dans les documents avec la recherche sémantique](../../relational-databases/search/find-key-phrases-in-documents-with-semantic-search.md) et [Rechercher des documents similaires ou connexes avec la recherche sémantique](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
-   Si vous ne spécifiez pas de LCID explicitement pour un index sémantique, seules la langue principale et ses statistiques linguistiques associées sont utilisées pour l'indexation sémantique.  
  
-   Si vous spécifiez une langue pour une colonne pour laquelle le modèle linguistique n'est pas disponible, la création de l'index échoue et retourne un message d'erreur.  
  
##  <a name="HowToEnableCreate"></a> Créer un index sémantique lorsqu’il n’y a pas d’index de recherche en texte intégral  
 Lorsque vous créez un index de recherche en texte intégral avec l’instruction **CREATE FULLTEXT INDEX** , vous avez la possibilité d’activer l’indexation sémantique au niveau de la colonne en spécifiant le mot clé **STATISTICAL_SEMANTICS** dans le cadre de la définition de la colonne. Vous pouvez également activer l'indexation sémantique lorsque vous utilisez l'Assistant Indexation de texte intégral pour créer un index de recherche en texte intégral.  
  
 ### <a name="create-a-new-semantic-index-by-using-transact-sql"></a>Créer un index sémantique à l'aide de Transact-SQL  
 
 Appelez l’instruction **CREATE FULLTEXT INDEX** et spécifiez **STATISTICAL_SEMANTICS** pour chaque colonne sur laquelle vous souhaitez créer un index sémantique. Pour plus d’informations sur toutes les options de cette instruction, consultez [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
 **Exemple 1 : créer un index unique, un index de recherche en texte intégral et un index sémantique**  
  
 L’exemple suivant crée un catalogue de texte intégral par défaut, **ft**. L’exemple crée ensuite un index unique sur la colonne **JobCandidateID** de la table **HumanResources.JobCandidate** de l’exemple de base de données AdventureWorks2012. Cet index unique est requis en tant que colonne clé pour un index de recherche en texte intégral. L’exemple crée ensuite un index de recherche en texte intégral et un index sémantique sur la colonne **Resume** .  
  
```sql  
CREATE FULLTEXT CATALOG ft AS DEFAULT  
GO  
  
CREATE UNIQUE INDEX ui_ukJobCand  
    ON HumanResources.JobCandidate(JobCandidateID)  
GO  
  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate  
    (Resume  
        Language 1033  
        Statistical_Semantics  
    )   
    KEY INDEX JobCandidateID   
    WITH STOPLIST = SYSTEM  
GO  
```  
  
 **Exemple 2 : créer un index sémantique et de recherche en texte intégral sur plusieurs colonnes avec remplissage différé de l'index**  
  
 L’exemple suivant crée un catalogue de texte intégral, **documents_catalog**, dans l’exemple de base de données AdventureWorks2012. L'exemple crée ensuite un index de recherche en texte intégral qui utilise ce nouveau catalogue. L’index de recherche en texte intégral est créé sur les colonnes **Title**, **DocumentSummary**et **Document** de la table **Production.Document** , tandis que l’index sémantique est uniquement créé sur la colonne **Document** . Cet index de recherche en texte intégral utilise le catalogue de texte intégral nouvellement créé et un index de clé unique existant, **PK_Document_DocumentID**. Comme recommandé, cette clé d'index est créée sur une colonne d'entiers, **DocumentID**. L'exemple spécifie le LCID de l'anglais, 1033, qui est la langue des données dans les colonnes.  
  
 Cet exemple spécifie également que le suivi des modifications est désactivé et sans remplissage. Plus tard, durant les heures creuses, cet exemple utilise une instruction **ALTER FULLTEXT INDEX** pour démarrer un remplissage complet sur le nouvel index et activer le suivi automatique des modifications.  
  
```sql  
CREATE FULLTEXT CATALOG documents_catalog  
GO  
  
CREATE FULLTEXT INDEX ON Production.Document  
    (   
    Title  
        Language 1033,   
    DocumentSummary  
        Language 1033,   
    Document   
        TYPE COLUMN FileExtension  
        Language 1033  
        Statistical_Semantics  
    )  
    KEY INDEX PK_Document_DocumentID  
        ON documents_catalog  
        WITH CHANGE_TRACKING OFF, NO POPULATION  
GO  
```  
  
 Plus tard, pendant une heure creuse, l'index est rempli :  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document SET CHANGE_TRACKING AUTO  
GO  
```  
  
### <a name="create-a-new-semantic-index-by-using-sql-server-management-studio"></a>Créer un nouvel index sémantique à l'aide de SQL Server Management Studio  
 Exécutez l’Assistant Indexation de texte intégral et activez **Sémantiques statistiques** dans la page **Sélectionner les colonnes de la table** pour chaque colonne sur laquelle vous souhaitez créer un index sémantique. Pour plus d’informations, notamment sur le démarrage de l’Assistant Indexation de texte intégral, consultez [Utiliser l’Assistant Indexation de texte intégral](../../relational-databases/search/use-the-full-text-indexing-wizard.md).  
  
##  <a name="HowToEnableAlter"></a> Créer un index sémantique lorsqu’un index de recherche en texte intégral existe  
 Vous pouvez ajouter l’indexation sémantique lorsque vous modifiez un index de recherche en texte intégral existant avec l’instruction **ALTER FULLTEXT INDEX** . Vous pouvez également ajouter l'indexation sémantique à l'aide de différentes boîtes de dialogue dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="add-a-semantic-index-by-using-transact-sql"></a>Ajouter un index sémantique à l'aide de Transact-SQL  
 Appelez l’instruction **ALTER FULLTEXT INDEX** avec les options décrites ci-dessous pour chaque colonne sur laquelle vous souhaitez ajouter un index sémantique. Pour plus d’informations sur toutes les options de cette instruction, consultez [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
 Les index sémantiques et de recherche en texte intégral sont à nouveau remplis après un appel à **ALTER**, sauf spécification contraire de votre part.  
  
-   Pour ajouter l’indexation de texte intégral uniquement à une colonne, utilisez la syntaxe **ADD** .  
  
-   Pour ajouter l’indexation sémantique et de texte intégral à une colonne, utilisez la syntaxe **ADD** avec l’option **STATISTICAL_SEMANTICS** .  
  
-   Pour ajouter l’indexation sémantique à une colonne dont l’indexation de texte intégral est déjà activée, utilisez l’option **ADD STATISTICAL_SEMANTICS** . Vous ne pouvez ajouter l'indexation sémantique qu'à une colonne dans une instruction **ALTER** individuelle.  
  
 **Exemple : ajouter l'indexation sémantique à une colonne dont l'indexation de texte intégral est déjà activée**  
  
 L’exemple suivant modifie un index de recherche en texte intégral existant sur la table **Production.Document** de l’exemple de base de données AdventureWorks2012. Il ajoute un index sémantique sur la colonne **Document** de la table **Production.Document** , qui comporte déjà un index de recherche en texte intégral. L'exemple spécifie que l'index ne sera pas rempli à nouveau automatiquement.  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document  
    ALTER COLUMN Document  
        ADD Statistical_Semantics  
    WITH NO POPULATION  
GO  
```  
  
### <a name="add-a-semantic-index-by-using-sql-server-management-studio"></a>Ajouter un index sémantique à l'aide de SQL Server Management Studio  
 Vous pouvez modifier les colonnes activées pour l’indexation de texte intégral et sémantique dans la page **Colonnes d’index de recherche en texte intégral** de la boîte de dialogue **Propriétés d’index de recherche en texte intégral** . Pour plus d’informations, consultez [Gérer les index de recherche en texte intégral](http://msdn.microsoft.com/library/28ff17dc-172b-4ac4-853f-990b5dc02fd1).  

## <a name="alter-a-semantic-index"></a>Modifier un index sémantique
  
###  <a name="addreq"></a> Exigences et restrictions concernant la modification d'un index existant  
  
-   Vous ne pouvez pas modifier un index existant pendant que le remplissage de l'index est en cours. Pour plus d’informations sur la surveillance de la progression du remplissage d’index, consultez [Gérer et surveiller la recherche sémantique](../../relational-databases/search/manage-and-monitor-semantic-search.md).  
  
-   Vous ne pouvez pas ajouter d'indexation à une colonne, ni modifier ou supprimer l'indexation pour la même colonne, dans un appel unique à l'instruction **ALTER FULLTEXT INDEX** .  
  
##  <a name="dropping"></a> Supprimer un index sémantique  
Vous pouvez supprimer l’indexation sémantique lorsque vous modifiez un index de recherche en texte intégral existant avec l’instruction **ALTER FULLTEXT INDEX** . Vous pouvez également supprimer l'indexation sémantique à l'aide de différentes boîtes de dialogue dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 ### <a name="drop-a-semantic-index-by-using-transact-sql"></a>Supprimer un index sémantique à l'aide de Transact-SQL  
Pour supprimer l’indexation sémantique d’une ou plusieurs colonnes, appelez l’instruction **ALTER FULLTEXT INDEX** avec l’option **ALTER COLUMN***nom_colonne***DROP STATISTICAL_SEMANTICS**. Vous pouvez supprimer l'indexation de plusieurs colonnes dans une instruction **ALTER** unique.  
  
```sql  
USE database_name  
GO  

ALTER FULLTEXT INDEX  
    ALTER COLUMN column_name  
    DROP STATISTICAL_SEMANTICS  
GO  
```  
  
Pour supprimer l’indexation de texte intégral et sémantique d’une colonne, appelez l’instruction **ALTER FULLTEXT INDEX** avec l’option **ALTER COLUMN***nom_colonne***DROP**.  
  
```sql  
USE database_name  
GO  
  
ALTER FULLTEXT INDEX  
    ALTER COLUMN column_name  
    DROP  
GO  
```  
  
 ### <a name="drop-a-semantic-index-by-using-sql-server-management-studio"></a>Supprimer un index sémantique à l'aide de SQL Server Management Studio  
 Vous pouvez modifier les colonnes activées pour l’indexation de texte intégral et sémantique dans la page **Colonnes d’index de recherche en texte intégral** de la boîte de dialogue **Propriétés d’index de recherche en texte intégral** . Pour plus d’informations, consultez [Gérer les index de recherche en texte intégral](http://msdn.microsoft.com/library/28ff17dc-172b-4ac4-853f-990b5dc02fd1).  
  
###  <a name="dropreq"></a> Requirements and restrictions for dropping a semantic index  
  
-   Vous ne pouvez pas supprimer l'indexation de texte intégral d'une colonne tout en conservant l'indexation sémantique. L'indexation sémantique dépend de l'indexation de texte intégral pour les résultats de ressemblance de document.  
  
-   Vous ne pouvez pas spécifier l'option **NO POPULATION** lorsque vous supprimez l'indexation sémantique de la dernière colonne d'une table pour laquelle l'indexation sémantique a été activée. Un cycle de remplissage est obligatoire pour supprimer les résultats indexés précédemment.  
  
## <a name="check-whether-semantic-search-is-enabled-on-database-objects"></a>Vérifier si la recherche sémantique est activée sur des objets de base de données  
### <a name="is-semantic-search-enabled-for-a-database"></a>La recherche sémantique est-elle activée pour une base de données ?
  
 Interrogez la propriété **IsFullTextEnabled** de la fonction de métadonnées [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md).  
  
 Une valeur de retour de 1 indique que la recherche en texte intégral et la recherche sémantique sont activées pour la base de données ; une valeur de retour de 0 indique qu'elles ne le sont pas.  
  
```sql  
SELECT DATABASEPROPERTYEX('database_name', 'IsFullTextEnabled')  
GO  
```  
  
### <a name="is-semantic-search-enabled-for-a-table"></a>La recherche sémantique est-elle activée pour une table ?  
 
 Interrogez la propriété **TableFullTextSemanticExtraction** de la fonction de métadonnées [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
 Une valeur de retour de 1 indique que la recherche sémantique est activée pour la table ; une valeur de retour de 0 indique qu'elle n'est pas activée.  
  
```scr  
SELECT OBJECTPROPERTYEX(OBJECT_ID('table_name'), 'TableFullTextSemanticExtraction')  
GO  
```  
  
 ### <a name="is-semantic-search-enabled-for-a-column"></a>La recherche sémantique est-elle activée pour une colonne ?
   
 Pour déterminer si la recherche sémantique est activée pour une colonne spécifique :  
  
-   Interrogez la propriété **StatisticalSemantics** de la fonction de métadonnées [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md).  
  
     Une valeur de retour de 1 indique que la recherche sémantique est activée pour la colonne ; une valeur de retour de 0 indique qu'elle n'est pas activée.  
  
    ```sql  
    SELECT COLUMNPROPERTY(OBJECT_ID('table_name'), 'column_name', 'StatisticalSemantics')  
    GO  
    ```  
  
-   Interrogez l’affichage catalogue [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) de l’index de recherche en texte intégral.  
  
     Une valeur 1 dans la colonne **statistical_semantics** indique que la colonne spécifiée est activée pour l’indexation sémantique en plus de l’indexation de texte intégral.  
  
    ```sql  
    SELECT * FROM sys.fulltext_index_columns WHERE object_id = OBJECT_ID('table_name')  
    GO  
    ```  
  
-   Dans l’Explorateur d’objets dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur une colonne, puis sélectionnez **Propriétés**. Dans la page **Général** de la boîte de dialogue **Propriétés de la colonne** , vérifiez la valeur de la propriété **Statistical Semantics** .  
  
     Une valeur True indique que la colonne spécifiée est activée pour l'indexation sémantique en plus de l'indexation de texte intégral.  
  
## <a name="determine-what-can-be-indexed-for-semantic-search"></a>Déterminer ce qui peut être indexé pour la recherche sémantique  
  
###  <a name="HowToCheckLanguages"></a> Vérifier quelles langues sont prises en charge pour la recherche sémantique  
  
> [!IMPORTANT]  
>  Moins de langues sont prises en charge pour l'indexation sémantique que pour l'indexation de texte intégral. Par conséquent, il peut y avoir des colonnes que vous pouvez indexer pour la recherche en texte intégral, mais pas pour la recherche sémantique.  
  
 Interrogez l’affichage catalogue [sys.fulltext_semantic_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md).  
  
```sql  
SELECT * FROM sys.fulltext_semantic_languages  
GO  
```  
  
 Les langues suivantes sont prises en charge pour l'indexation sémantique. Cette liste représente la sortie de l’affichage catalogue [sys.fulltext_semantic_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md), classée par LCID.  
  
|Langue|LCID|  
|--------------|----------|  
|German|1031|  
|Anglais (États-Unis)|1033|  
|Français|1036|  
|Italien|1040|  
|Portugais (Brésil)|1046|  
|Russe|1049|  
|Suédois|1053|  
|Anglais (Royaume-Uni)|2057|  
|Portugais (Portugal)|2070|  
|Espagnol|3082|  
  
###  <a name="doctypes"></a> Déterminer les types de documents pouvant être indexés  
 Interrogez l’affichage catalogue [sys.fulltext_document_types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md).  
  
 Si le type de document que vous souhaitez indexer ne figure pas dans la liste des types pris en charge, vous devrez peut-être rechercher, télécharger et installer des filtres supplémentaires. Pour plus d’informations, consultez [Afficher ou modifier des filtres et des analyseurs lexicaux inscrits](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md).  
  
##  <a name="BestPracticeFilegroup"></a> Best practice: Consider creating a separate filegroup for the full-text and semantic indexes  
 Créez un groupe de fichiers séparé pour les index sémantiques et de recherche en texte intégral si l'allocation d'espace disque pose problème. Les index sémantiques sont créés dans le même groupe de fichiers que l'index de recherche en texte intégral. Un index sémantique entièrement rempli peut contenir un grand nombre de données.  
 
##  <a name="IssueNoResults"></a> Problème : La recherche sur une colonne spécifique ne retourne aucun résultat  
 **Est-ce qu'un LCID non-Unicode a été spécifié pour une langue Unicode ?**  
 Il est possible d'activer l'indexation sémantique sur un type de colonne non-Unicode avec un LCID de langue qui comporte uniquement des termes Unicode, tels que LCID 1049 pour le russe. Dans ce cas, aucun résultat ne sera jamais retourné des index sémantiques sur cette colonne.  
  
  
