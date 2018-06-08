---
title: sp_fulltext_load_thesaurus_file (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fulltext_load_thesaurus_file
- sp_fulltext_load_thesaurus_file_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_load_thesaurus_file
- full-text indexes [SQL Server], thesaurus files
- thesaurus [full-text search], editing
ms.assetid: 73a309c3-6d22-42dc-a6fe-8a63747aa2e4
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5e3b6cd075ab2b0fb11d013ee7af2973007d40ef
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfulltextloadthesaurusfile-transact-sql"></a>sp_fulltext_load_thesaurus_file (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Oblige l'instance de serveur à analyser et charger les données à partir du fichier de dictionnaire des synonymes qui correspond à la langue dont le LCID est spécifié. Cette procédure stockée est utile après la mise à jour d'un fichier de dictionnaire des synonymes. L’exécution de **sp_fulltext_load_thesaurus_file** provoque la recompilation des requêtes de texte intégral qui utilisent le dictionnaire des synonymes du LCID spécifié.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_fulltext_load_thesaurus_file lcid [ , @loadOnlyIfNotLoaded  = action ]   
```  
  
## <a name="arguments"></a>Arguments  
 *lcid*  
 Entier de mappage de l’identificateur de paramètres régionaux (LCID) de la langue pour laquelle vous souhaitez lade la définition XML du dictionnaire des synonymes. Pour obtenir les LCID des langues qui sont disponibles sur une instance de serveur, utilisez le [sys.fulltext_languages &#40;Transact-SQL&#41; ](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md) affichage catalogue.  
  
 **@loadOnlyIfNotLoaded** = *action*  
 Spécifie si le fichier de dictionnaire des synonymes doit être chargé dans les tables internes du dictionnaire des synonymes même s'il a déjà été chargé. *action* est un des :  
  
|Valeur|Définition|  
|-----------|----------------|  
|**0**|Charge le fichier de dictionnaire des synonymes, qu'il ait ou non été déjà chargé. Il s’agit du comportement par défaut de **sp_fulltext_load_thesaurus_file**.|  
|1|Charge le fichier de dictionnaire des synonymes uniquement s'il n'est pas encore chargé.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Les fichiers d'un dictionnaire des synonymes sont chargés automatiquement par les requêtes de texte intégral qui l'utilisent. Pour éviter cet impact sur les performances de la première fois sur les requêtes de recherche en texte intégral, nous vous recommandons d’exécuter **sp_fulltext_load_thesaurus_file**.  
  
 Utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)'**update_languages**' pour mettre à jour la liste des langues inscrites avec la recherche en texte intégral.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou l’administrateur système peut exécuter le **sp_fulltext_load_thesaurus_file** procédure stockée.  
  
 Seuls des administrateurs système peuvent mettre à jour, modifier ou supprimer des fichiers de dictionnaire des synonymes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-load-a-thesaurus-file-even-if-it-is-already-loaded"></a>A. Chargement d'un fichier de dictionnaire des synonymes même s'il a déjà été chargé  
 L'exemple suivant analyse et charge le fichier de dictionnaire des synonymes anglais.  
  
```  
EXEC sys.sp_fulltext_load_thesaurus_file 1033;  
GO  
```  
  
### <a name="b-load-a-thesaurus-file-only-if-it-is-not-yet-loaded"></a>B. Chargement d'un fichier de dictionnaire des synonymes uniquement s'il n'est pas encore chargé  
 L'exemple suivant analyse et charge le fichier de dictionnaire des synonymes arabe, sauf s'il est déjà chargé.  
  
```  
EXEC sys.sp_fulltext_load_thesaurus_file 1025, @loadOnlyIfNotLoaded = 1;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurer et gérer des fichiers de dictionnaire des synonymes pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)  
  
  
