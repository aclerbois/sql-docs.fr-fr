---
title: Créer un repère de plan pour les requêtes paramétrables | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- parameterized queries, plan guides for
- plan guides [SQL Server], parameterized queries
ms.assetid: b532ae16-66e7-4641-9bc8-b0d805853477
caps.latest.revision: 6
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 5b45a9adec50b1abc3c20b4d2ad56641db1d832f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="create-a-plan-guide-for-parameterized-queries"></a>Créer un repère de plan pour les requêtes paramétrables
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Un repère de plan TEMPLATE correspond à des requêtes autonomes paramétrables au format spécifié.  
  
 L'exemple suivant crée un repère de plan correspondant à la requête qui paramètre selon une forme donnée, et commande à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'imposer le paramétrage de la requête. La syntaxe des deux requêtes suivantes est équivalente, seules leurs valeurs littérales constantes diffèrent.  
  
```  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45639;  
  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45640;  
```  
  
 Voici le repère de plan pour la forme paramétrable de la requête :  
  
```  
EXEC sp_create_plan_guide   
    @name = N'TemplateGuide1',  
    @stmt = N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
              INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
                  ON h.SalesOrderID = d.SalesOrderID  
              WHERE h.SalesOrderID = @0',  
    @type = N'TEMPLATE',  
    @module_or_batch = NULL,  
    @params = N'@0 int',  
    @hints = N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 Dans l'exemple précédent, la valeur du paramètre `@stmt` correspond à la forme paramétrable de la requête. La procédure stockée système [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) est la seule méthode fiable pour obtenir cette valeur et pouvoir l’utiliser dans sp_create_plan_guide. Vous pouvez utiliser le script suivant à la fois pour obtenir la requête paramétrable et créer ensuite un repère de plan à partir de celle-ci.  
  
```  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
      INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
          ON h.SalesOrderID = d.SalesOrderID  
      WHERE h.SalesOrderID = 45639;',  
    @stmt OUTPUT,   
    @params OUTPUT  
EXEC sp_create_plan_guide N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
> [!IMPORTANT]  
>  Les valeurs littérales constantes du paramètre `@stmt` transmises à `sp_get_query_template` peuvent affecter le type de données choisi pour le paramètre qui remplace le littéral. Ceci va également affecter la mise en correspondance du repère de plan. Vous devrez peut-être créer plusieurs repères de plan pour traiter plusieurs plages de valeurs.  
  
 Vous pouvez combiner les repères de plan TEMPLATE et les repères de plan SQL. Ainsi, vous pouvez créer un repère de plan TEMPLATE pour vous assurer qu'une classe de requêtes est paramétrable, et ensuite créer un repère de plan SQL sur la forme paramétrable de cette requête.  
  
  
