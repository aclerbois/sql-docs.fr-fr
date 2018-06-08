---
title: SET STATISTICS XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_XML_TSQL
- SET STATISTICS XML
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- STATISTICS XML option
- SET STATISTICS XML statement
- statements [SQL Server], statistical information
- XML [SQL Server], statement execution information
ms.assetid: 2b6d4c5a-a7f5-4dd1-b10a-7632265b1af7
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0d60816f269e224ab99c07a4bc9b4eaf8e63ffe3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-statistics-xml-transact-sql"></a>SET STATISTICS XML (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Force Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à exécuter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et à générer des informations détaillées sur l'exécution de ces instructions, sous la forme d'un document XML correctement formé.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET STATISTICS XML { ON | OFF }  
```  
  
## <a name="remarks"></a>Notes   
 La définition de SET STATISTICS XML s'effectue au moment de l'exécution, et non au moment de l'analyse.  
  
 Lorsque SET STATISTICS XML a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des informations d'exécution sur chaque instruction exécutée. Une fois cette option activée, des informations sur toutes les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] consécutives sont retournées, jusqu'à ce que l'option soit désactivée (OFF). Notez qu'un traitement peut comporter d'autres instructions en plus de SET STATISTICS XML.  
  
 SET STATISTICS XML retourne une sortie de type **nvarchar(max)** pour des applications, par exemple l’utilitaire **sqlcmd**, où la sortie XML est ensuite utilisée par d’autres outils pour afficher et traiter les informations du plan de requête.  
  
 SET STATISTICS XML retourne des informations sous la forme de documents XML. Chaque instruction qui suit l'instruction SET STATISTICS XML ON est reflétée dans la sortie par un document unique. Chaque document contient le texte de l'instruction, suivi des détails des étapes de l'exécution. La sortie présente des informations d'exécution tels que les coûts, les index utilisés et les types d'opérations effectués, l'ordre de jointure, le nombre d'exécutions d'une opération physique, le nombre de lignes produites par chaque opérateur physique, etc.  
  
 Le document qui contient le schéma XML pour la sortie XML générée par SET STATISTICS XML est copié au cours de l'installation dans un répertoire local de l'ordinateur sur lequel Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé. Il est situé sur le lecteur contenant les fichiers d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'adresse :  
  
 \Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd  
  
 Le schéma Showplan est également disponible sur [ce site web](http://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
 SET STATISTICS PROFILE et SET STATISTICS XML sont complémentaires. La première instruction génère une sortie de texte et la seconde une sortie XML. Dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les nouvelles informations du plan d'exécution de requête ne seront affichées qu'avec l'instruction SET STATISTICS XML, pas avec l'instruction SET STATISTICS PROFILE.  
  
> [!NOTE]  
>  Si l’option **Inclure le plan d’exécution réel** est sélectionnée dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cette option SET ne produit aucune sortie de la représentation au format XML. Désactivez l’option **Inclure le plan d’exécution réel** avant d’utiliser l’option SET.  
  
## <a name="permissions"></a>Autorisations  
 Pour utiliser SET STATISTICS XML et afficher la sortie, les utilisateurs doivent bénéficier des autorisations suivantes :  
  
-   Autorisations adéquates pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Autorisation SHOWPLAN sur toutes les bases de données contenant des objets auxquels les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] font référence.  
  
 Pour les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui ne génèrent pas de jeux de résultats STATISTICS XML, seules les autorisations appropriées pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont nécessaires. Pour les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui génèrent des jeux de résultats STATISTICS XML, les vérifications concernant l'autorisation d'exécution des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et l'autorisation SHOWPLAN doivent être toutes deux positives sans quoi l'exécution de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est annulée et aucune information Showplan n'est créée.  
  
## <a name="examples"></a>Exemples  
 Les deux instructions suivantes utilisent les paramètres SET STATISTICS XML pour montrer comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] analyse et optimise l'utilisation des index dans les requêtes. La première requête utilise l'opérateur de comparaison Égal à (=) dans la clause WHERE sur une colonne indexée. La seconde requête utilise l'opérateur LIKE dans la clause WHERE. Cela oblige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser une analyse d'index cluster pour rechercher les données répondant à la condition spécifiée par la clause WHERE. Les valeurs des attributs **EstimateRows** et **EstimatedTotalSubtreeCost** sont moins élevées pour la première requête indexée, ce qui indique qu’elle a été traitée plus rapidement et qu’elle a utilisé moins de ressources que la requête non indexée.  
  
```  
USE AdventureWorks2012;  
GO  
SET STATISTICS XML ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, JobTitle   
FROM HumanResources.Employee  
WHERE JobTitle LIKE 'Production%';  
GO  
SET STATISTICS XML OFF;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [SET SHOWPLAN_XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-xml-transact-sql.md)   
 [sqlcmd Utility](../../tools/sqlcmd-utility.md)  
  
  
