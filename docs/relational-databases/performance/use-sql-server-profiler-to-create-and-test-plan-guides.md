---
title: Utiliser le Générateur de profils SQL Server pour créer et tester des repères de plan | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- checking plan guides
- plan guides [SQL Server], testing
- plan guides [SQL Server], SQL Server Profiler
- matching queries to plan guides [SQL Server]
- testing plan guides
- SQL Server Profiler, plan guides
- plan guides [SQL Server], creating
- capturing query text [SQL Server]
- verifying plan guides
- Profiler [SQL Server Profiler], plan guides
- query-to-plan guide matching [SQL Server]
ms.assetid: 7018dbf0-1a1a-411a-88af-327bedf9cfbd
caps.latest.revision: 31
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 9b35d7d96c21c4e3883f1f36df325d3128d9492f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="use-sql-server-profiler-to-create-and-test-plan-guides"></a>Utiliser le Générateur de profils SQL Server pour créer et tester des repères de plan
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lorsque vous créez un repère de plan, vous pouvez recourir à [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour capturer le texte de requête exact à utiliser dans l’argument *statement_text* de la procédure stockée **sp_create_plan_guide** . Ainsi, au moment de la compilation, le repère de plan correspondra à la requête. Une fois le repère de plan créé, vous pouvez également utiliser [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour tester la correspondance effective du repère de plan à la requête. En règle générale, vous devez tester les repères de plan à l'aide de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour vérifier que la requête correspond au repère de plan.  
  
## <a name="capturing-query-text-by-using-sql-server-profiler"></a>Capture du texte de requête à l'aide du Générateur de profils SQL Server  
 Si vous exécutez une requête et capturez le texte tel qu'il a été soumis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], vous pouvez créer un repère de plan de type SQL ou TEMPLATE qui corresponde exactement à ce texte. Cela permet de faire en sorte que le repère de plan soit utilisé par l'optimiseur de requête.  
  
 Imaginons la requête suivante soumise par une application en tant que traitement autonome :  
  
```  
SELECT COUNT(*) AS c  
FROM Sales.SalesOrderHeader AS h  
INNER JOIN Sales.SalesOrderDetail AS d  
  ON h.SalesOrderID = d.SalesOrderID  
WHERE h.OrderDate BETWEEN '20000101' and '20050101';  
```  
  
 Supposons que vous souhaitiez exécuter cette requête à l'aide d'une opération de jointure de fusion, mais que SHOWPLAN indique qu'elle n'est pas en train d'utiliser une jointure de fusion. Étant donné que vous ne pouvez pas modifier la requête directement dans l'application, vous créez un repère de plan pour spécifier que l'indicateur de requête MERGE JOIN soit ajouté à la requête au moment de la compilation.  
  
 Pour capturer le texte de la requête tel que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le reçoit, procédez comme suit :  
  
1.  Démarrez une trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , en veillant à ce que le type d'événement **SQL:BatchStarting** soit sélectionné.  
  
2.  Faites exécuter la requête par l'application.  
  
3.  Suspendez la trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
4.  Cliquez sur l'événement **SQL:BatchStarting** qui correspond à la requête.  
  
5.  Cliquez avec le bouton droit et sélectionnez **Extraire les données d’événement**.  
  
    > [!IMPORTANT]  
    >  N'essayez pas de copier le texte du traitement en le sélectionnant dans le volet inférieur de la fenêtre de trace du Générateur de profils. Le repère de plan que vous créez risque alors de ne pas correspondre au traitement d'origine.  
  
6.  Enregistrez les données d'événement dans un fichier. Il s'agit du texte de traitement.  
  
7.  Ouvrez le fichier du texte de traitement dans l'application Bloc-notes puis copiez le texte dans le Presse-papiers.  
  
8.  Créez le repère de plan, puis collez le texte copié entre les guillemets (**''**) de l’argument **@stmt** . Vous devez isoler tout guillemet simple contenu dans l'argument **@stmt** en le faisant précéder par un autre guillemet simple. Faites attention de ne pas ajouter ou supprimer d'autres caractères lorsque vous insérez ces guillemets simples. Par exemple, le littéral de date **'** 20000101 **'** doit être délimité de la façon suivante : **''** 20000101 **''**.  
  
 Voici le repère de plan :  
  
```  
EXEC sp_create_plan_guide   
    @name = N'MyGuide1',  
    @stmt = N'<paste the text copied from the batch text file here>',  
    @type = N'SQL',  
    @module_or_batch = NULL,  
    @params = NULL,  
    @hints = N'OPTION (MERGE JOIN)';  
```  
  
## <a name="testing-plan-guides-by-using-sql-server-profiler"></a>Test des repères de plan à l'aide du Générateur de profils SQL Server  
 Pour vérifier qu'un repère de plan correspond à une requête, procédez comme suit :  
  
1.  Démarrez une trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , en veillant à ce que le type d’événement **Showplan XML** , situé sous le nœud **Performances** , soit sélectionné.  
  
2.  Faites exécuter la requête par l'application.  
  
3.  Suspendez la trace du [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
4.  Recherchez l'événement **Showplan XML** de la requête affectée.  
  
    > [!NOTE]  
    >  L'événement **Showplan XML For Query Compile** ne peut pas être utilisé. **PlanGuideDB** n'existe pas dans cet événement.  
  
5.  Si le repère de plan est de type OBJECT ou SQL, vérifiez que l'événement **Showplan XML** contient les attributs **PlanGuideDB** et **PlanGuideName** du repère de plan qui doit correspondre à la requête. Ou, dans le cas d'un repère de plan TEMPLATE, vérifiez que l'événement **Showplan XML** contient les attributs **TemplatePlanGuideDB** et **TemplatePlanGuideName** du repère de plan prévu. Cette opération vérifie que le repère de plan fonctionne. Ces attributs sont contenus sous l’élément **\<StmtSimple>** du plan.  
  
## <a name="see-also"></a> Voir aussi  
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
  
  
