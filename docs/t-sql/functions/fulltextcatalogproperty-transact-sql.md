---
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
caps.latest.revision: 50
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 58592c26b8d5747876eda424c225bc3eca013d60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie des informations concernant les propriétés de catalogue de texte intégral dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
## <a name="arguments"></a>Arguments  
  
> [!NOTE]  
>  Les propriétés suivantes seront supprimées dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **LogSize** et **PopulateStatus**. Évitez par conséquent d'utiliser ces propriétés dans un nouveau travail de développement et prévoyez la modification des applications qui les utilisent actuellement.  
  
 *CATALOG_NAME*  
 Expression contenant le nom du catalogue de texte intégral.  
  
 *property*  
 Expression contenant le nom de la propriété de catalogue de texte intégral. Le tableau donne la liste des propriétés et fournit les descriptions des informations renvoyées.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**AccentSensitivity**|Respect des accents.<br /><br /> 0 = Accents ignorés<br /><br /> 1 = Accents respectés|  
|**IndexSize**|Taille logique du catalogue de texte intégral en mégaoctets (Mo). Inclut la taille des index d'expression de clé sémantique et de ressemblance du document.<br /><br /> Pour plus d'informations, consultez la section « Remarques » plus loin dans cette rubrique.|  
|**ItemCount**|Nombre d'éléments indexés, y compris tous les index de recherche en texte intégral, d'expression de clé et de ressemblance du document dans un catalogue|  
|**LogSize**|Pris en charge pour la compatibilité descendante uniquement. Retourne toujours 0.<br /><br /> Taille en octets du jeu combiné de journaux d'erreurs associés à un catalogue de texte intégral du service de Recherche [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**MergeStatus**|Indique si une fusion principale est en cours.<br /><br /> 0 = Aucune fusion principale en cours.<br /><br /> 1 = Fusion principale en cours.|  
|**PopulateCompletionAge**|Différence en secondes entre la fin du remplissage du dernier index de texte intégral et le 01/01/1990 00:00:00<br /><br /> Propriété uniquement mise à jour pour des analyses complètes ou incrémentielles. Renvoie la valeur 0 si aucun remplissage ne s'est produit.|  
|**PopulateStatus**|0 = Inactif <br /><br /> 1 = Remplissage complet en cours<br /><br /> 2 = En pause <br /><br /> 3 = Accéléré<br /><br /> 4 = Récupération<br /><br /> 5 = Arrêt<br /><br /> 6 = Remplissage incrémentiel en cours<br /><br /> 7 = Indexation en cours<br /><br /> 8 = Disque plein Suspendu.<br /><br /> 9 = Suivi des modifications|  
|**UniqueKeyCount**|Nombre de clés uniques dans le catalogue de texte intégral.|  
|**ImportStatus**|Indique si le catalogue de texte intégral est en cours d'importation.<br /><br /> 0 = Indique que le catalogue de texte intégral n'est pas en cours d'importation.<br /><br /> 1 = Indique que le catalogue de texte intégral est en cours d'importation.|  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'un droit d'accès. Cela signifie que les fonctions intégrées générant des métadonnées, telles que FULLTEXTCATALOGPROPERTY, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d’informations, consultez [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md).  
  
## <a name="remarks"></a>Notes   
 FULLTEXTCATALOGPROPERTY ('*catalog_name*','**IndexSize**') examine uniquement les fragments ayant l’état 4 ou 6, comme indiqué dans [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md). Ces fragments font partie de l'index logique. Par conséquent, la propriété **IndexSize** renvoie uniquement la taille de l’index logique. Pendant une fusion d'index, toutefois, la taille d'index réelle peut être le double de sa taille logique. Pour rechercher la taille réelle consommée par un index de recherche en texte intégral pendant une fusion, utilisez la procédure stockée système [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md). Cette procédure examine tous les fragments associés à un index de recherche en texte intégral. Si vous limitez la croissance du fichier de catalogue de texte intégral et ne conservez pas suffisamment d'espace pour le processus de fusion, le remplissage de texte intégral peut échouer. Dans ce cas, FULLTEXTCATALOGPROPERTY ('catalog_name' ,'IndexSize') retourne 0 et l'erreur suivante est écrite dans le journal de texte intégral :  
  
 `Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
 Il est important que les applications n’attendent pas dans une boucle courte que la propriété **PopulateStatus** passe à l’état inactif (indiquant la fin d’un remplissage), car les cycles de microprocesseur sont utilisés en dehors des processus de la base de données et de la recherche en texte intégral, ce qui provoque des délais d’expiration. Il est d’ailleurs généralement plus intéressant de vérifier la propriété **PopulateStatus** correspondante au niveau de la table ou **TableFullTextPopulateStatus** dans la fonction système OBJECTPROPERTYEX. Cette propriété et d'autres propriétés de texte intégral nouvelles dans OBJECTPROPERTYEX fournissent des informations plus précises sur les tables d'indexation de texte intégral. Pour plus d’informations, consultez [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant renvoie le nombre d'éléments indexés en texte intégral dans un catalogue de texte intégral nommé `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
