---
title: INSÉRER (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- INSERT INTO
- INSERT
- INSERT_INTO
dev_langs:
- DMX
helpviewer_keywords:
- SKIP (DMX)
- mapped model columns element
- source data query element
- <mapped model columns> element
- <source data query> element
- INSERT INTO statement
- mining models [Analysis Services], processing
- training mining models
- mining structures [DMX], processing
ms.assetid: 85eed207-396c-4a95-a74e-2acc1abc7e2c
caps.latest.revision: 49
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: f76a649664d5240d31b1fa5b69a5d3045a59de26
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="insert-into-dmx"></a>INSERT INTO (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Traite l'objet d'exploration de données spécifié. Pour plus d’informations sur le traitement des modèles d’exploration de données et les structures d’exploration de données, consultez [le traitement des exigences et les considérations &#40;d’exploration de données&#41;](../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md).  
  
 Si une structure d'exploration de données est spécifiée, l'instruction traite la structure et tous ses modèles d'exploration de données associés. Si un modèle d'exploration de données est spécifié, l'instruction traite uniquement le modèle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
INSERT INTO [MINING MODEL]|[MINING STRUCTURE] <model>|<structure> (<mapped model columns>) <source data query>  
INSERT INTO [MINING MODEL]|[MINING STRUCTURE] <model>|<structure>.COLUMN_VALUES (<mapped model columns>) <source data query>  
```  
  
## <a name="arguments"></a>Arguments  
 *model*  
 Identificateur du modèle  
  
 *structure*  
 Identificateur de la structure  
  
 *colonnes de modèle mappé*  
 Liste des identificateurs de colonnes et des identificateurs imbriqués séparés par une virgule.  
  
 *requête de source de données*  
 Requête source dans le format défini par le fournisseur  
  
## <a name="remarks"></a>Notes  
 Si vous ne spécifiez pas **modèle d’exploration de données** ou **STRUCTURE d’exploration de données**, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] recherche le type d’objet en fonction du nom et traite l’objet correct. Si le serveur contient une structure d'exploration de données et un modèle d'exploration de données portant le même nom, une erreur est retournée.  
  
 À l’aide de la deuxième forme de syntaxe, INSERT INTO*\<objet >*. COLUMN_VALUES, vous pouvez insérer des données directement dans les colonnes du modèle sans apprentissage du modèle. Cette méthode permet d'insérer des données de colonnes dans le modèle d'une manière concise et organisée, ce qui est utile lorsque vous utilisez des datasets contenant des hiérarchies ou des colonnes triées.  
  
 Si vous utilisez **INSERT INTO** avec un modèle d’exploration de données ou une structure et laissez le champ désactivé le \<mappé les colonnes du modèle > et \<requête de source de données > arguments, l’instruction se comporte comme **ProcessDefault**, à l’aide de liaisons qui existent déjà. Si les liaisons n'existent pas, l'instruction retourne une erreur. Pour plus d’informations sur **ProcessDefault**, consultez [les paramètres et les Options de traitement &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md). L’exemple suivant illustre la syntaxe :  
  
```  
INSERT INTO [MINING MODEL] <model>  
```  
  
 Si vous spécifiez **modèle d’exploration de données** et fournir des colonnes mappées et une requête de source de données, le modèle et une structure associée est traité.  
  
 Le tableau ci-dessous donne une description du résultat de différentes formes de l'instruction, en fonction de l'état des objets.  
  
|.|État des objets|Résultat|  
|---------------|----------------------|------------|  
|INSERT INTO MINING MODEL*\<modèle >*|La structure d'exploration de données est traitée.|Le modèle d'exploration de données est traité.|  
||La structure d'exploration de données n'est pas traitée.|Le modèle et la structure d'exploration de données sont traités.|  
||La structure d'exploration de données contient des modèles d'exploration de données supplémentaires.|Échec du traitement. Vous devez retraiter la structure et les modèles d'exploration de données associés.|  
|INSERT INTO MINING STRUCTURE*\<structure >*|La structure d'exploration de données est traitée ou non.|La structure d'exploration de données et les modèles d'exploration de données associés sont traités.|  
|INSERT INTO MINING MODEL*\<modèle >* qui contient une requête source<br /><br /> ou<br /><br /> INSERT INTO MINING STRUCTURE*\<structure >* qui contient une requête source|La structure ou le modèle contient déjà du contenu.|Échec du traitement. Vous devez supprimer les objets avant d’effectuer cette opération, à l’aide de [supprimer &#40;DMX&#41;](../dmx/delete-dmx.md).|  
  
## <a name="mapped-model-columns"></a>Mapped Model Columns  
 À l’aide de la \<mappé les colonnes du modèle > élément, vous pouvez mapper les colonnes de la source de données pour les colonnes dans votre modèle d’exploration de données. Le \<mappé les colonnes du modèle > élément a la forme suivante :  
  
```  
<column identifier> | SKIP | <table identifier> (<column identifier> | SKIP), ...  
```  
  
 À l’aide de **ignorer**, vous pouvez exclure certaines colonnes qui doivent exister dans la requête source, mais qui n’existent pas dans le modèle d’exploration de données. SKIP est utile lorsque vous ne contrôlez pas les colonnes incluses dans l'ensemble de lignes d'entrée. Si vous écrivez votre propre OPENQUERY, la meilleure pratique consiste à omettre la colonne de la liste de colonnes SELECT au lieu d'utiliser SKIP.  
  
 SKIP est également utile lorsqu'une colonne de l'ensemble de lignes d'entrée est nécessaire pour effectuer une jointure, mais que la colonne n'est pas utilisée par la structure d'exploration de données. En guise d'exemple typique, on peut citer une structure d'exploration de données et un modèle d'exploration de données qui contiennent une table imbriquée. L'ensemble de lignes d'entrée pour cette structure aura une colonne clé étrangère utilisée pour créer un ensemble de lignes hiérarchique à l'aide de la clause SHAPE, mais la colonne clé étrangère n'est presque jamais utilisée dans le modèle.  
  
 La syntaxe de SKIP requiert que vous insériez SKIP à la position de la colonne individuelle dans l'ensemble de lignes d'entrée qui n'a aucune colonne de structure d'exploration de données correspondante. Par exemple, dans l'exemple de table imbriquée ci-dessous, OrderNumber doit être sélectionné dans la clause APPEND afin de pouvoir être utilisé dans la clause RELATE pour spécifier la jointure ; toutefois, vous ne souhaitez pas insérer les données OrderNumber dans la table imbriquée dans la structure d'exploration de données. Par conséquent, l'exemple utilise le mot clé SKIP au lieu de OrderNumber dans l'argument INSERT INTO.  
  
## <a name="source-data-query"></a>Source Data Query  
 Le \<requête de source de données > élément peut inclure les types de source de données suivants :  
  
-   **OPENQUERY**  
  
-   **OPENROWSET**  
  
-   **FORME**  
  
-   Toute requête [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] retournant un ensemble de lignes  
  
 Pour plus d’informations sur les types de source de données, consultez [ &#60;requête de source de données&#62;](../dmx/source-data-query.md).  
  
## <a name="basic-example"></a>Exemple de base  
 L’exemple suivant utilise **OPENQUERY** pour former un modèle Naive Bayes basé sur les données de publipostage ciblées dans le [!INCLUDE[ssSampleDBDWobject](../includes/sssampledbdwobject-md.md)] base de données.  
  
```  
INSERT INTO NBSample (CustomerKey, Gender, [Number Cars Owned],  
    [Bike Buyer])  
OPENQUERY([AdventureWorksDW2012],'Select CustomerKey, Gender, [NumberCarsOwned], [BikeBuyer]   
FROM [vTargetMail]')  
```  
  
## <a name="nested-table-example"></a>Exemple de table imbriquée  
 L’exemple suivant utilise **forme** pour former un modèle d’exploration de données d’association qui contient une table imbriquée. Notez que la première ligne contient **ignorer** à la place OrderNumber, qui est requis dans le **SHAPE_APPEND** instruction mais n’est pas utilisée dans le modèle d’exploration de données.  
  
```  
INSERT INTO MyAssociationModel  
    ([OrderNumber],[Models] (SKIP, [Model])  
    )  
SHAPE {  
    OPENQUERY([AdventureWorksDW2012],'SELECT OrderNumber  
    FROM vAssocSeqOrders ORDER BY OrderNumber')  
} APPEND (  
    {OPENQUERY([AdventureWorksDW2012],'SELECT OrderNumber, model FROM   
    dbo.vAssocSeqLineItems ORDER BY OrderNumber, Model')}  
  RELATE OrderNumber to OrderNumber)   
AS [Models]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; instructions de définition de données](../dmx/dmx-statements-data-definition.md)   
 [Data Mining Extensions &#40;DMX&#41; instructions de Manipulation de données](../dmx/dmx-statements-data-manipulation.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
