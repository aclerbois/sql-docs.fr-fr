---
title: Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 02/02/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], noise words
- noise words [full-text search]
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 43b5ce7b-9f09-4443-8a5b-c3da6eb28bcc
caps.latest.revision: 81
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3f76976b1dbb89027db6bb194b124f9813768398
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="configure-and-manage-stopwords-and-stoplists-for-full-text-search"></a>Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Pour éviter que l'index de recherche en texte intégral ne devienne encombré, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un mécanisme qui ignore les chaînes courantes qui ne sont d'aucune utilité pour la recherche. Ces chaînes ignorées sont appelées des *mots vides*. Pendant la création d'un index, le moteur de texte intégral omet les mots vides de l'index de recherche en texte intégral. Cela signifie que les requêtes de texte intégral ne rechercheront pas les mots vides.  
   
**Mots vides**. Un mot vide peut être un mot avec une signification dans une langue spécifique. Par exemple, en français, les mots tels que « un », « et », « est » ou « le » sont écartés de l'index de recherche en texte intégral, car ils sont inutiles dans le cadre d'une recherche. Un mot vide peut également être un *jeton* qui n’a pas de signification linguistique.  

**Listes de mots vides**. Les mots vides sont gérés dans des bases de données à l'aide d'objets appelés des listes de mots vides. Une *liste de mots vides* est une liste qui, associée à un index de recherche en texte intégral, s’applique aux requêtes de texte intégral sur cet index.
   
## <a name="use-an-existing-stoplist"></a>Utiliser une liste de mots vides existante  
 Vous pouvez utiliser une liste de mots vides existante comme suit :  
  
-   Utilisez la liste de mots vides fournie par le système dans la base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est fourni avec une liste de mots vides système qui contient les mots vides les plus couramment utilisés pour chaque langue prise en charge, c’est-à-dire pour chaque langue associée aux analyseurs lexicaux fournis par défaut. Vous pouvez copier la liste de mots vides système et personnaliser cette liste en ajoutant et en supprimant des mots vides.  
  
     La liste de mots vides système est installée dans la base de données [Resource](../../relational-databases/databases/resource-database.md) .  
  
-   Utilisez une liste de mots vides personnalisée issue d’une autre base de données dans l’instance de serveur actuelle, puis ajoutez ou supprimez des mots vides au besoin.  
  
## <a name="create-a-new-stoplist"></a>Créer une nouvelle liste de mots vides 
### <a name="create-a-new-stoplist-with-transact-sql"></a>Créer une nouvelle liste de mots vides avec Transact-SQL
Utilisez [CREATE FULLTEXT STOPLIST](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md).

### <a name="create-a-new-stoplist-with-management-studio"></a>Créer une nouvelle liste de mots vides avec Management Studio
  
1.  Dans l'Explorateur d'objets, développez le serveur.  
  
2.  Développez **Bases de données**, puis développez la base de données dans laquelle vous souhaitez créer la liste de mots vides de texte intégral.  
  
3.  Développez **Stockage**, puis cliquez avec le bouton droit sur **Listes de mots vides de texte intégral**.  
  
4.  Sélectionnez **Nouvelle liste de mots vides de texte intégral**.  
  
5.  Entrez le nom de votre nouvelle liste de mots vides.  
  
6.  Éventuellement, spécifiez une autre personne comme propriétaire de la liste de mots vides.  
  
7.  Sélectionnez l'une des options de création de liste de mots vides suivantes :  
  
    -   **Créer une liste de mots vides vide**  
  
    -   **Créer à partir de la liste de mots vides système**  
  
    -   **Créer à partir d'une liste de mots vides de texte intégral existante**  
  
     Pour plus d’informations, consultez [Nouvelle liste de mots vides de texte intégral &#40;page Général&#41;](http://msdn.microsoft.com/library/97f8e82d-82ab-4525-91c9-1ee3ae217309).  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="use-a-stoplist-in-full-text-queries"></a>Utiliser une liste de mots vides dans les requêtes de texte intégral  
 Pour utiliser une liste de mots vides dans des requêtes, vous devez l’associer à un index de recherche en texte intégral. Vous pouvez joindre une liste de mots vides à un index de recherche en texte intégral lorsque vous créez l'index, ou vous pouvez modifier ultérieurement l'index pour ajouter une liste de mots vides.  
  
### <a name="create-a-full-text-index-and-associate-a-stoplist-with-it"></a>Créer un index de recherche en texte intégral et lui associer une liste de mots vides
Utilisez [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).
  
### <a name="associate-or-disassociate-a-stoplist-with-an-existing-full-text-index"></a>Associer ou dissocier une liste de mots vides avec un index de recherche en texte intégral existant
Utilisez [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md). 
  
## <a name="change-the-stopwords-in-a-stoplist"></a>Changer les mots vides dans une liste de mots vides  
### <a name="add-or-drop-stopwords-from-a-stoplist-with-transact-sql"></a>Ajouter ou supprimer des mots vides dans une liste de mots vides avec Transact-SQL
Utilisez [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md).
  
### <a name="add-or-drop-stopwords-from-a-stoplist-with-management-studio"></a>Ajouter ou supprimer des mots vides dans une liste de mots vides avec Management Studio  
  
1.  Dans l'Explorateur d'objets, développez le serveur.  
  
2.  Développez **Bases de données**, puis développez la base de données.  
  
3.  Développez **Stockage**, puis sélectionnez **Listes de mots vides de texte intégral**.  
  
4.  Cliquez avec le bouton droit sur la liste de mots vides dont vous souhaitez modifier les propriétés, puis sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue [Propriétés de la liste de mots vides de texte intégral](http://msdn.microsoft.com/library/2e907f5b-0cf9-484a-afcf-a4e7f1e2f87f) :  
  
    1.  Dans la zone de liste **Action** , sélectionnez l’une des actions suivantes : **Ajouter un mot vide**, **Supprimer le mot vide**, **Supprimer tous les mots vides**ou **Effacer la liste de mots vides**.  
  
    2.  Si la zone de texte **Mot vide** est activée pour l’action sélectionnée, entrez un mot vide unique. Ce nouveau mot vide doit être unique ; autrement dit, il ne doit pas déjà figurer dans la liste de mots vides correspondant à la langue sélectionnée.  
  
    3.  Si la zone de liste **Langue de texte intégral** est activée pour l’action sélectionnée, sélectionnez une langue.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="manage-stoplists-and-their-usage"></a>Gérer les listes de mots vides et leur utilisation
  
### <a name="view-all-the-stopwords-in-a-stoplist"></a>Afficher tous les mots vides d’une liste de mots vides
Utilisez [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md). 
  
### <a name="get-info-about-all-the-stoplists-in-the-current-database"></a>Obtenir des informations sur toutes les listes de mots vides dans la base de données actuelle
Utilisez [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md) et [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md).
  
### <a name="view-the-tokenization-result-of-a-word-breaker-thesaurus-and-stoplist-combination"></a>Consulter le résultat de la création de jetons d’une combinaison d’analyseur lexical, de dictionnaire des synonymes et de liste de mots vides
Utilisez [sys.dm_fts_parser &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md).

### <a name="suppress-an-error-message-if-stopwords-cause-a-boolean-operation-on-a-full-text-query-to-fail"></a>Supprimer un message d’erreur si des mots vides provoquent l’échec d’une opération booléenne sur une requête de texte intégral
Utilisez l’option de configuration de serveur [Transformer les mots parasites](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md). 
   
## <a name="more-info-about-stopword-position"></a>Plus d’informations sur la position des mots vides
 Bien qu'il ignore l'inclusion des mots vides, l'index de recherche en texte intégral prend en considération leur position. Prenons l'exemple de l'expression suivante : « Instructions are applicable to these Adventure Works Cycles models ». Le tableau suivant décrit la position des mots dans l'expression :  
  
|Word|Position|  
|----------|--------------|  
|Instructions| 1|  
|are|2|  
|applicable|3|  
|to|4|  
|these|5|  
|Adventure|6|  
|Works|7|  
|Cycles|8|  
|modèles|9|  
  
 Les mots vides « are », « to » et « these » situés aux positions 2, 4 et 5 sont écartés de l'index de recherche en texte intégral. Cependant, les informations relatives à leur position sont conservées, sans affecter la position des autres mots de l'expression.   
  
## <a name="upgrade-noise-words-from-sql-server-2005"></a>Mettre à niveau des mots parasites à partir de SQL Server 2005  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Les mots parasites ont été remplacés par les mots vides. Lorsqu'une base de données est mise à niveau à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les fichiers de mots parasites ne sont plus utilisés. Toutefois, les fichiers de mots parasites sont stockés dans le dossier FTDATA\ FTNoiseThesaurusBak, et vous pouvez les utiliser ultérieurement lors de la mise à jour ou de la génération des listes de mots vides correspondantes. Pour plus d’informations sur la mise à niveau de fichiers de mots parasites en listes de mots vides, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  
  
  
