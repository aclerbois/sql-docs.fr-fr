---
title: Vues de gestion dynamique de recherche en texte intégral et sémantique - fonctions | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- dynamic management objects [SQL Server], full-text search
- full-text search [SQL Server], dynamic management views
ms.assetid: 199dbd5a-29f6-4ef0-8e65-86e32c0aaa3a
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5aff261fa7697f3a3c7ba3693ce68ac11bee4362
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="full-text-and-semantic-search-dynamic-management-views---functions"></a>Vues de gestion dynamique de recherche en texte intégral et sémantique - fonctions
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Cette section contient les vues de gestion dynamique suivantes et les fonctions en rapport avec la recherche en texte intégral et la recherche sémantique.  
  
## <a name="full-text-search-dynamic-management-views-and-functions"></a>Fonctions et vues de gestion dynamique liées à la recherche en texte intégral  
 [sys.dm_fts_active_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
 Retourne des informations concernant les catalogues de texte intégral qui ont une activité de remplissage en cours sur le serveur.  
  
 [sys.dm_fts_fdhosts](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
 Retourne des informations sur l'activité actuelle de l'hôte ou des hôtes de démon de filtre sur l'instance de serveur.  
  
 [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
 Retourne des informations sur le contenu d'un index de recherche en texte intégral pour la table spécifiée.  
  
 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
 Retourne des informations sur le contenu de niveau document d'un index de recherche en texte intégral pour la table spécifiée. Un mot clé donné peut apparaître dans plusieurs documents.  
  
 [sys.dm_fts_index_keywords_by_property](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
 Retourne tout le contenu en rapport avec les propriétés dans l'index de recherche en texte intégral d'une table donnée. Cela inclut toutes les données qui appartiennent à une propriété inscrite par la liste de propriétés de recherche associée à cet index de recherche en texte intégral.  
  
 sys.dm_fts_index_keywords_position_by_document  
 Retourne la position des mots clés dans un document.  
  
 [sys.dm_fts_index_population](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
 Retourne des informations sur les remplissages d'index de texte intégral actuellement en cours.  
  
 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
 Retourne des informations sur les zones de mémoire tampon appartenant à un pool de mémoire spécifique et utilisées dans le cadre d'une analyse de texte intégral ou d'une plage d'analyses de texte intégral.  
  
 [sys.dm_fts_memory_pools](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
 Retourne des informations sur les pools de mémoire partagée disponibles pour le composant Full-Text Gatherer dans le cadre d'une analyse de texte intégral ou d'une étendue d'analyse de texte intégral.  
  
 [sys.dm_fts_outstanding_batches](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
 Retourne des informations à propos de chaque lot d'indexation de texte intégral.  
  
 [sys.dm_fts_parser](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
 Retourne le dernier résultat de segmentation du texte en unités lexicales après avoir appliqué une combinaison donnée d'analyseur lexical, de dictionnaire des synonymes et de liste de mots vides à l'entrée d'une chaîne de requête. La sortie est la même que si la chaîne de requête spécifiée avait été adressée au Moteur d'indexation et de recherche en texte intégral.  
  
 [sys.dm_fts_population_ranges](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
 Retourne des informations sur les plages spécifiques liées au remplissage en cours de l'index de recherche en texte intégral.  
  
## <a name="semantic-search-dynamic-management-views-and-functions"></a>Fonctions et vues de gestion dynamique liées à la recherche sémantique  
 [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
 Retourne une ligne d'informations d'état à propos du remplissage de l'index de ressemblance de document pour chaque index de ressemblance de chaque table associée à un index sémantique.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)  
  
  
