---
title: Spécifier le comportement du paramétrage de requêtes grâce aux repères de plan | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- TEMPLATE plan guide
- PARAMETERIZATION FORCED option
- PARAMETERIZATION option
- PARAMETERIZATION SIMPLE option
- parameterization [SQL Server]
- overriding parameterization behavior
- plan guides [SQL Server], parameterization
- parameterized queries [SQL Server]
ms.assetid: f0f738ff-2819-4675-a8c8-1eb6c210a7e6
caps.latest.revision: 35
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 42d3e71f304ddc156a4b25e3039873c13b37a0a9
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="specify-query-parameterization-behavior-by-using-plan-guides"></a>Spécifier le comportement du paramétrage de requêtes grâce aux repères de plan
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Si l'option de base de données PARAMETERIZATION a la valeur SIMPLE, il se peut que l'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] opte pour le paramétrage des requêtes. En d'autres termes, toute valeur littérale contenue dans une requête est substituée par des paramètres. Ce processus s'appelle le paramétrage simple. Lorsque le paramétrage SIMPLE s'applique, vous ne pouvez pas décider des requêtes devant être paramétrables et de celles ne devant pas l'être. Vous pouvez cependant indiquer que toutes les requêtes d'une base de données soient paramétrables en affectant à l'option de base de données PARAMETERIZATION la valeur FORCED. Ce processus s'appelle le paramétrage forcé.  
  
 Vous pouvez outrepasser ce comportement de paramétrage d'une base de données par le biais de repères de plan des façons suivantes :  
  
-   Quand l'option de base de données PARAMETERIZATION a la valeur SIMPLE, vous pouvez spécifier que le paramétrage forcé doit être tenté sur une certaine classe de requêtes. Pour ce faire, créez un repère de plan TEMPLATE sur la forme paramétrable de la requête et spécifiez l’indicateur de requête PARAMETERIZATION FORCED dans la procédure stockée [sp_create_plan_guide](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md) . Ce type de repère de plan peut être considéré comme un moyen de forcer le paramétrage sur une classe de requêtes seulement plutôt que sur toutes les requêtes.  
  
-   Quand l'option de base de données PARAMETERIZATION a la valeur FORCED, vous pouvez spécifier que, pour une certaine classe de requêtes, seul un paramétrage simple est tenté, et non pas un paramétrage forcé. Pour ce faire, créez un repère de plan TEMPLATE sur la forme avec paramétrage forcé de la requête et spécifiez l’indicateur de requête PARAMETERIZATION SIMPLE dans la procédure stockée **sp_create_plan_guide**.  
  
 Supposons la requête suivante portant sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] :  
  
```  
SELECT pi.ProductID, SUM(pi.Quantity) AS Total  
FROM Production.ProductModel AS pm   
    INNER JOIN Production.ProductInventory AS pi   
        ON pm.ProductModelID = pi.ProductID   
WHERE pi.ProductID = 101   
GROUP BY pi.ProductID, pi.Quantity HAVING SUM(pi.Quantity) > 50;  
```  
  
 Vous pouvez choisir de ne pas activer le paramétrage forcé de toutes les requêtes portant sur la base de données à condition d'en être l'administrateur. En revanche, vous voulez éviter les coûts de compilation sur toutes les requêtes équivalentes dans leur syntaxe à la requête précédente, mais différant sur leurs valeurs littérales constantes uniquement. En d'autres termes, il peut être préférable que la requête soit paramétrable afin qu'un plan de requête correspondant à ce type de requête soit réutilisé. Dans ce cas, suivez les étapes ci-dessous :  
  
1.  Récupérez la forme paramétrable de la requête. La seule méthode sûre pour obtenir cette valeur et l’utiliser dans la procédure stockée **sp_create_plan_guide** consiste à passer par la procédure stockée système [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) .  
  
2.  Créez le repère de plan sur la forme paramétrable de la requête en spécifiant l'indicateur de requête PARAMETERIZATION FORCED.  
  
    > [!IMPORTANT]  
    >  Dans le cadre du paramétrage d'une requête, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte un type de données aux paramètres remplaçant les valeurs littérales, en fonction de la valeur et de la taille du littéral. Ce même procédé est utilisé sur la valeur des littéraux constants transmis au paramètre de sortie **@stmt** de la procédure stockée **sp_get_query_template**. Comme le type de données précisé dans l’argument **@params** de **sp_create_plan_guide** doit correspondre à celui de la requête tel qu’il est paramétré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il se peut que vous soyez amené à créer plusieurs repères de plan afin de couvrir l’ensemble des valeurs de paramètres possibles relatives à la requête.  
  
 Le script suivant peut être utilisé aussi bien pour obtenir la requête paramétrable que pour créer plus tard un repère de plan s'y rapportant :  
  
```  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT pi.ProductID, SUM(pi.Quantity) AS Total   
      FROM Production.ProductModel AS pm   
      INNER JOIN Production.ProductInventory AS pi ON pm.ProductModelID = pi.ProductID   
      WHERE pi.ProductID = 101   
      GROUP BY pi.ProductID, pi.Quantity   
      HAVING sum(pi.Quantity) > 50',  
    @stmt OUTPUT,   
    @params OUTPUT;  
EXEC sp_create_plan_guide   
    N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 De même, dans une base de données où le paramétrage forcé est déjà activé, vous pouvez vous assurer que la requête donnée en exemple et celles dont la syntaxe est équivalente, sauf pour leurs valeurs littérales constantes, sont paramétrables selon les règles du paramétrage simple. Pour ce faire, spécifiez PARAMETERIZATION SIMPLE plutôt que PARAMETERIZATION FORCED dans la clause OPTION.  
  
> [!NOTE]  
>  Les repères de plan TEMPLATE font correspondre les instructions aux requêtes envoyées par traitements qui sont composées d'une seule instruction. Les instructions à l'intérieur de traitements à instructions multiples ne peuvent pas être mises en correspondance avec les repères de plan TEMPLATE.  
  
  
