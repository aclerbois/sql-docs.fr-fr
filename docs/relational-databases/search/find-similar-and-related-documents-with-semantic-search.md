---
title: Rechercher des documents similaires ou connexes avec la recherche sémantique | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- semantic search [SQL Server], document similarity queries
ms.assetid: 9f527883-031b-442f-8e95-24bc0151ecbf
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 15c7b1da3f3741e1b9e26763b19c00a289c3e158
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="find-similar-and-related-documents-with-semantic-search"></a>Rechercher des documents similaires ou connexes avec la recherche sémantique
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Explique comment rechercher des valeurs textuelles ou des documents similaires ou connexes et donne des informations sur leur similitude, dans des colonnes configurées pour l'indexation sémantique statistique.  
   
##  <a name="HowToQuerySimilar"></a> Rechercher des documents similaires ou connexes avec SEMANTICSIMILARITYTABLE  
 Pour identifier des documents similaires ou connexes dans une colonne spécifique, interrogez la fonction [semanticsimilaritytable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritytable-transact-sql.md).  
  
 **SEMANTICSIMILARITYTABLE** retourne une table de zéro, une ou plusieurs lignes pour les colonnes dont le contenu dans la colonne spécifiée est sémantiquement similaire au document spécifié. Cette fonction d'ensemble de lignes peut être référencée dans la clause FROM d'une instruction SELECT comme un nom de table classique.  
  
 Vous ne pouvez pas rechercher des documents similaires dans plusieurs colonnes. La fonction **SEMANTICSIMILARITYTABLE** récupère uniquement des résultats provenant de la même colonne que la colonne source, qui est identifiée par l’argument **source_key**.  
  
 Pour plus d’informations sur les paramètres requis par la fonction **SEMANTICSIMILARITYTABLE** et sur la table de résultats qu’elle retourne, consultez [semanticsimilaritytable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritytable-transact-sql.md).  
  
> [!IMPORTANT]  
>  L'indexation sémantique et de texte intégral doit être activée pour les colonnes que vous ciblez.  
  
###  <a name="HowToIdentifySimilar"></a> Example: Find the top documents that are similar to another document  
 L'exemple suivant récupère les 10 premiers candidats similaires au candidat spécifié par *@CandidateID* dans la table HumanResources.JobCandidate de l'exemple de base de données AdventureWorks2012.  
  
```scr  
SELECT TOP(10) KEY_TBL.matched_document_key AS Candidate_ID  
FROM SEMANTICSIMILARITYTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume,  
    @CandidateID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
GO  
```  
  
##  <a name="HowToQuerySimilarity"></a> Rechercher des informations sur la manière dont des documents sont similaires ou connexes avec SEMANTICSIMILARITYDETAILSTABLE  
 Pour obtenir des informations sur les expressions clés qui rendent des documents similaires ou connexes, vous pouvez interroger la fonction [semanticsimilaritydetailstable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritydetailstable-transact-sql.md).  
  
 **SEMANTICSIMILARITYDETAILSTABLE** retourne une table de zéro, une ou plusieurs lignes d’expressions clés communes dans deux documents (un document source et un document mis en correspondance) dont le contenu est similaire sémantiquement. Cette fonction d'ensemble de lignes peut être référencée dans la clause FROM d'une instruction SELECT comme un nom de table classique.  
  
 Pour plus d’informations sur les paramètres requis par la fonction **SEMANTICSIMILARITYDETAILSTABLE**, ainsi que sur la table de résultats qu’elle retourne, consultez [semanticsimilaritydetailstable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritydetailstable-transact-sql.md).  
  
> [!IMPORTANT]  
>  L'indexation sémantique et de texte intégral doit être activée pour les colonnes que vous ciblez.  
  
###  <a name="HowToSimilarPhrases"></a> Example: Find the top key phrases that are similar between documents  
 L'exemple suivant récupère les 5 expressions clés qui ont le score de similarité le plus élevé parmi les candidats spécifiés dans la table **HumanResources.JobCandidate** de l'exemple de base de données AdventureWorks2012.  
  
```sql  
SELECT TOP(5) KEY_TBL.keyphrase, KEY_TBL.score  
FROM SEMANTICSIMILARITYDETAILSTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume, @CandidateID,  
    Resume, @MatchedID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
GO  
```  
  
  
