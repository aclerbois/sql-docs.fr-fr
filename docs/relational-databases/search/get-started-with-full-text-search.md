---
title: Bien démarrer avec la recherche en texte intégral | Microsoft Docs
ms.date: 08/22/2016
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- full-text indexes [SQL Server], creating
- full-text search [SQL Server], about
- full-text search [SQL Server], setting up
ms.assetid: 1fa628ba-0ee4-4d8f-b086-c4e52962ca4a
caps.latest.revision: 76
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 749bbabf665c29afa048c806472f1d454142a601
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="get-started-with-full-text-search"></a>Commencer à utiliser la recherche en texte intégral
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Par défaut, les bases de données SQL Server prennent en charge le texte intégral. Cependant, avant de pouvoir exécuter des requêtes de texte intégral, vous devez créer un catalogue en texte intégral, puis créer un index en texte intégral sur les tables ou les vues indexées dans lesquelles vous souhaitez effectuer votre recherche.

## <a name="set-up-full-text-search-in-two-steps"></a>Configurer la recherche en texte intégral en deux étapes
La configuration de la recherche en texte intégral s’effectue en deux étapes :  
1.  Création d'un catalogue de texte intégral.  
2.  Créez un index de texte intégral sur les tables ou les vues indexées dans lesquelles vous souhaitez effectuer votre recherche. 

Chaque index de recherche en texte intégral doit appartenir à un catalogue de texte intégral. Vous pouvez créer un catalogue de texte séparé pour chaque index de recherche en texte intégral ou associer plusieurs index de recherche en texte intégral à un catalogue donné. Un catalogue de texte intégral est un objet virtuel qui n'appartient à aucun groupe de fichiers. Le catalogue est un concept logique qui fait référence à un groupe d'index de recherche en texte intégral.

> [!NOTE]
> Ces étapes supposent que les composants facultatifs de la recherche en texte intégral ont été installés lors de l’installation de SQL Server. Si ce n’est pas le cas, vous devez exécuter de nouveau le programme d’installation de SQL Server pour les ajouter.  

## <a name="set-up-full-text-search-with-a-wizard"></a>Configurer la recherche en texte intégral à l’aide d’un Assistant 
 
Pour configurer la recherche en texte intégral par le biais d’un Assistant, consultez [Utiliser l’Assistant Indexation de texte intégral](../../relational-databases/search/use-the-full-text-indexing-wizard.md).

## <a name="set-up-full-text-search-with-transact-sql"></a>Configurer la recherche en texte intégral avec Transact-SQL 
 L’exemple en deux parties ci-dessous crée d’abord un catalogue de texte intégral nommé `AdvWksDocFTCat` sur l’exemple de base de données AdventureWorks, puis crée un index de recherche en texte intégral sur la table `Document` dans cette même base de données. Cette instruction permet de créer le catalogue de texte intégral dans le répertoire par défaut qui est spécifié durant l’installation de SQL Server. Le dossier nommé `AdvWksDocFTCat` se trouve dans le répertoire par défaut.  
  
1.  Pour créer un catalogue de texte intégral nommé `AdvWksDocFTCat`, l’exemple utilise une instruction [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md) :  
  
    ```sql
    USE AdventureWorks;  
    GO  
    CREATE FULLTEXT CATALOG AdvWksDocFTCat;  
    ```  
    Pour plus d’informations, consultez [Créer et gérer des catalogues de texte intégral](../../relational-databases/search/create-and-manage-full-text-catalogs.md).
 
2.  Avant de créer un index de recherche en texte intégral sur la table Document, assurez-vous que la table dispose d'un index unique qui n'accepte pas les valeurs NULL et ne comporte qu'une seule colonne. L’instruction [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) suivante crée un index unique, `ui_ukDoc`, sur la colonne DocumentID de la table Document :  
  
    ```sql 
    CREATE UNIQUE INDEX ui_ukDoc ON Production.Document(DocumentID);  
    ```  

3.  Une fois que vous avez une clé unique, vous pouvez créer un index de recherche en texte intégral sur la table `Document` en utilisant l’instruction [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) .  
  
    ```sql  
    CREATE FULLTEXT INDEX ON Production.Document  
    (  
        Document                         --Full-text index column name   
            TYPE COLUMN FileExtension    --Name of column that contains file type information  
            Language 2057                 --2057 is the LCID for British English  
    )  
    KEY INDEX ui_ukDoc ON AdvWksDocFTCat --Unique index  
    WITH CHANGE_TRACKING AUTO            --Population type;  
    GO  
  
    ```  
  
     Le paramètre TYPE COLUMN défini dans cet exemple spécifie la colonne de type dans la table qui contient le type du document dans chaque ligne de la colonne Document (lequel est de type binaire). La colonne de type stocke l’extension de fichier (.doc, .xls, etc.) fournie par l’utilisateur du document dans une ligne donnée. Le moteur d'indexation et de recherche en texte intégral utilise l'extension de fichier dans une ligne donnée afin d'appeler le filtre approprié pour l'analyse des données de cette ligne. Une fois que le filtre a analysé les données binaires de la ligne, l’analyseur lexical spécifié analyse le contenu. (Dans cet exemple, l’analyseur lexical pour l’anglais (R.U.) est utilisé.) Pour plus d’informations, consultez [Configurer et gérer les extensions analytiques avancées](../../relational-databases/search/configure-and-manage-filters-for-search.md).  

    Pour plus d’informations, consultez [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md).

##  <a name="options"></a>Choisir des options pour un index de recherche en texte intégral 
  
### <a name="choose-a-language"></a>Choisir une langue  
 Pour plus d’informations sur la sélection du langage de colonne, consultez la section [Choisir une langue lors de la création d’un index de recherche en texte intégral](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md).  
  
### <a name="choose-a-filegroup"></a>Choisir un groupe de fichiers  
 Le processus de création d’un index de recherche en texte intégral est plutôt gourmand en e/s. En résumé, il consiste à lire des données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis à propager les données filtrées dans l’index de recherche en texte intégral. Comme meilleure pratique recommandée, localisez un index de recherche en texte intégral dans le groupe de fichiers de base de données qui convient le mieux pour optimiser les performances d'E/S ou localisez les index de recherche en texte intégral dans un groupe de fichiers différent sur un autre volume.
  
### <a name="choose-a-full-text-catalog"></a>Choisir un catalogue de texte intégral   
 
 Nous vous recommandons d'associer les tables ayant les mêmes caractéristiques de mise à jour (par exemple avec peu de modifications ou, au contraire, beaucoup de modifications, ou les tables fréquemment modifiées à un moment donné de la journée) dans le même catalogue de texte intégral. Cette manière de configurer la planification du remplissage du catalogue de texte intégral permet d'assurer la synchronisation perpétuelle des index de texte intégral et des tables, sans nuire à l'utilisation des ressources du serveur de base de données pendant les périodes de forte activité.  
  
 Respectez les consignes suivantes :  
  
-   Si vous indexez une table avec des millions de lignes, affectez la table sur son propre catalogue en recherche intégrale.  
  
-   Prenez en compte la quantité de modifications apparaissant dans les tables soumises à l’indexation de texte intégral ainsi que le nombre total de lignes concernées dans ces tables. Si le nombre total de lignes modifiées, auquel s’ajoute le nombre de lignes de table présentes au cours du dernier remplissage de texte intégral, s’élève à plusieurs millions, affectez la table à un catalogue de texte intégral qui lui est propre.  

### <a name="associate-a-unique-index"></a>Associer un index unique
Sélectionnez systématiquement le plus petit index unique disponible comme clé unique de texte intégral Il s'agira idéalement d'un index de quatre octets, basé sur des entiers. Cela réduit considérablement la quantité de ressources requise par la service Recherche de [!INCLUDE[msCoName](../../includes/msconame-md.md)] dans le système de fichiers. Si la clé primaire est volumineuse (plus de 100 octets), pensez à choisir un autre index unique pour la table (ou créez-le) comme clé unique de texte intégral. Dans le cas contraire, si la taille de la clé unique de texte intégral dépasse la taille maximale autorisée (900 octets), le remplissage de texte intégral est impossible.  
 
### <a name="associate-a-stoplist"></a>Associer une liste de mots vides   
  Une *liste de mots vides* est une liste contenant des mots vides, également appelés mots parasites. Une liste de mots vides est associée à chaque index de recherche en texte intégral, et les mots contenus dans cette liste de mots vides s'appliquent aux requêtes de texte intégral sur cet index. Par défaut, la liste de mots vides système est associée à un nouvel index de recherche en texte intégral. Vous pouvez créer et utiliser votre propre liste de mots vides.   
  
 Par exemple, l’instruction [CREATE FULLTEXT STOPLIST](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante crée une liste de mots vides de texte intégral nommée myStoplist en copiant la liste de mots vides système :  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist FROM SYSTEM STOPLIST;  
GO  
```  
  
 L’instruction [ALTER FULLTEXT STOPLIST](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] suivante modifie une liste de mots vides nommée myStoplist en ajoutant le mot « en » (d’abord pour l’espagnol, puis pour le français) :  
  
```sql  
ALTER FULLTEXT STOPLIST myStoplist ADD 'en' LANGUAGE 'Spanish';  
ALTER FULLTEXT STOPLIST myStoplist ADD 'en' LANGUAGE 'French';  
GO  
```  
Pour plus d’informations, consultez [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).

## <a name="update-a-full-text-index"></a>Mettre à jour un index de recherche en texte intégral  
 À l'instar des index [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] classiques, les index de recherche en texte intégral peuvent être automatiquement mis à jour à mesure que les données sont modifiées dans les tables associées. Il s'agit du comportement par défaut. Vous pouvez, également, garder manuellement vos index de recherche en texte intégral à jour ou à intervalles planifiés spécifiés. L’alimentation d’un index de recherche en texte intégral peut être longue et consommatrice de ressources. Ainsi, la mise à jour de l’index est habituellement effectuée en mode de processus asynchrone ; celui-ci s’exécute en arrière-plan et conserve l’index de recherche en texte intégral à jour après les modifications opérées dans la table de base. 
 
La mise à jour immédiate d’un index de recherche en texte intégral après chaque modification dans la table de base est également gourmande en ressources. Par conséquent, si vous avez un taux de mise à jour/insertion/suppression élevé, vous pouvez constater une baisse dans les performances des requêtes. Si cela se produit, pensez à planifier de temps à autre des mises à jour manuelles du suivi des modifications afin de ne pas perdre trace des nombreuses modifications, au lieu de mettre des requêtes en concurrence pour les ressources.  
  
Pour plus d’informations, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md). 

## <a name="next-steps"></a>Étapes suivantes
Après avoir configuré la recherche en texte intégral de SQL Server, vous êtes prêt à exécuter des requêtes de texte intégral. Pour plus d’informations, consultez [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md).
