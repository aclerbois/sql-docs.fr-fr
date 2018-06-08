---
title: Instruction CREATE MEMBER (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4458554d8b3aa6b0cb87d59629c70a18b609df44
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579361"
---
# <a name="mdx-data-definition---create-member"></a>Définition de données MDX - créer des membres
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Crée un membre calculé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE [ SESSION ] [HIDDDEN] [ CALCULATED ] MEMBER CURRENTCUBE | Cube_Name.Member_Name   
   AS MDX_Expression  
      [,Property_Name = Property_Value, ...n]  
......[,SCOPE_ISOLATION = CUBE]  
```  
  
## <a name="arguments"></a>Arguments  
 *Cube_Name*  
 Expression de chaîne valide qui fournit le nom du cube où le membre sera créé.  
  
 *Member_Name*  
 Expression de chaîne valide qui spécifie le nom d'un membre. Spécifiez un nom complet pour créer un membre au sein d'une dimension autre que la dimension Mesures. Si vous ne fournissez pas un nom de membre complet, le membre sera créé dans la dimension Mesures.  
  
 *MDX_Expression*  
 Expression MDX (Multidimensional Expressions) valide.  
  
 *Property_name*  
 Chaîne valide qui précise le nom d'une propriété de membre calculé.  
  
 *Nom*  
 Expression scalaire valide qui définit la valeur de la propriété de membre calculé.  
  
## <a name="remarks"></a>Notes  
 L'instruction CREATE MEMBER définit les membres calculés disponibles tout au long de la session, et qui peuvent par conséquent être utilisés dans plusieurs requêtes au cours de la session. Pour plus d’informations, consultez [Creating Session-Scoped de membres calculés &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-calculated-members-session-scoped-calculated-members.md).  
  
 Vous pouvez également définir un membre calculé qui ne doit être utilisé que par une requête unique. Pour définir un membre calculé limité à une seule requête, utilisez la clause WITH de l'instruction SELECT. Pour plus d’informations, consultez [Creating Query-Scoped de membres calculés &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-calculated-members-query-scoped-calculated-members.md).  
  
 *Property_name* peuvent faire référence aux deux propriétés standard ou facultatives de membre calculé. Les propriétés de membre standard sont répertoriées plus loin dans cette rubrique. Les membres calculés créés avec CREATE MEMBER sans un **SESSION** valeur ont une portée de session. En outre, les chaînes contenues dans les définitions de membre calculé sont délimitées par des guillemets doubles. Ce comportement diffère de celui de la méthode définie par OLE DB, qui spécifie que les chaînes doivent être délimitées par des guillemets simples.  
  
 La spécification d'un cube autre que le cube actuellement connecté génère une erreur. C'est pourquoi vous devez utiliser CURRENTCUBE de préférence à un nom de cube, pour désigner le cube actuel.  
  
 Pour de plus amples informations sur les propriétés de membre définies par OLE DB, reportez-vous à la documentation qui l'accompagne.  
  
## <a name="scope"></a>Portée  
 Un membre calculé peut se produire au s'in d'une des portées répertoriées dans le tableau suivant.  
  
 Étendue de requête  
 La visibilité et la durée de vie du membre calculé sont limitées à la requête. Le membre calculé est défini dans une requête particulière. L'étendue de requête remplace l'étendue de session. Pour plus d’informations, consultez [Creating Query-Scoped de membres calculés &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-calculated-members-query-scoped-calculated-members.md).  
  
 Étendue de session  
 La visibilité et la durée de vie du membre calculé sont limitées à la session dans laquelle il a été créé. (La durée de vie est inférieure à la durée de la session si une instruction DROP MEMBER est émise sur le membre calculé.) L'instruction CREATE MEMBER crée un membre calculé limité à la session.  
  
### <a name="scope-isolation"></a>Isolement de la portée  
 Par défaut, lorsqu'un script MDX (Multidimensional Expressions) de cube contient des membres calculés, les membres calculés sont résolus avant que les calculs au niveau de la session soient résolus et avant que des calculs définis par requête soient résolus.  
  
> [!NOTE]  
>  Dans certains scénarios, le [Aggregate (MDX)](../mdx/aggregate-mdx.md) (fonction) et le [VisualTotals (MDX)](../mdx/visualtotals-mdx.md) ne présentent pas ce comportement.  
  
 Le comportement permet aux applications clientes génériques de travailler avec des cubes qui contiennent des calculs complexes, sans avoir à prendre en compte l'implémentation spécifique des calculs. Toutefois, dans certains scénarios, vous souhaiterez exécuter une session ou des membres calculés d’étendue de requête avant certains calculs dans le cube et aucun le **d’agrégation** fonction ni le **VisualTotals** (fonction) sont applicables. Pour ce faire, utilisez la propriété de calcul SCOPE_ISOLATION.  
  
#### <a name="example"></a>Exemple  
 Le script ci-dessous est un exemple de scénario dans lequel la propriété de calcul SCOPE_ISOLATION est requise pour obtenir le résultat correct.  
  
 **Script MDX du cube :**  
  
```  
CREATE MEMBER CURRENTCUBE.Measures.ProfitRatio AS 'Measures.[Store Sales]/Measures.[Store Cost]', SOLVE_ORDER = 10  
```  
  
 **Requête MDX :**  
  
```  
WITH MEMBER [Customer].[Customers].[USA]. USAWithoutWA AS  
[Customer].[Customers].[Country].&[USA] - [Customer].[Customers].[State Province.&[WA], SOLVE_ORDER=5  
SELECT {USAWithoutWA} ON 0 FROM SALES  
WHERE ProfitRatio  
```  
  
 Le résultat souhaité de la requête précédente est le pourcentage des ventes pour les USA sans WA, pour stocker le coût pour les USA sans WA. La requête précédente ne retourne pas le résultat souhaité. Elle retourne le pourcentage des USA moins le pourcentage de WA, ce qui représente un résultat dénué de sens. Pour obtenir le résultat souhaité, vous pouvez utiliser la propriété de calcul SCOPE_ISOLATION.  
  
 **Requête MDX à l’aide de la propriété de calcul SCOPE_ISOLATION :**  
  
```  
WITH MEMBER [Customer].[Customers].[USA]. USAWithoutWA AS  
[Customer].[Customers].[Country].&[USA] - [Customer].[Customers].[State Province.&[WA], SOLVE_ORDER=5  
,SCOPE_ISOLATION=CUBE  
SELECT {USAWithoutWA} ON 0 FROM SALES  
WHERE ProfitRatio  
```  
  
## <a name="standard-properties"></a>Propriétés standard  
 Chaque membre calculé possède un jeu de propriétés par défaut. Lorsqu’une application cliente est connectée à [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], les propriétés par défaut sont pris en charge, soit disponible pour être pris en charge, comme l’administrateur choisit.  
  
 Des propriétés de membre supplémentaires peuvent être disponibles, selon la définition du cube. Les propriétés suivantes représentent des informations relatives au niveau de dimension dans le cube.  
  
|Identificateur de propriété|Signification|  
|-------------------------|-------------|  
|SOLVE_ORDER|Ordre dans lequel le membre calculé sera résolu si un membre calculé fait référence à un autre membre calculé (c'est-à-dire à l'intersection des membres calculés).|  
|FORMAT_STRING|Chaîne de format de style [!INCLUDE[msCoName](../includes/msconame-md.md)] Office que l'application cliente peut utiliser lors de l'affichage de valeurs de cellule.|  
|VISIBLE|Valeur qui indique si le membre calculé est visible dans un ensemble de lignes de schéma. Calculés visibles membres peuvent être ajoutés à un jeu à le [AddCalculatedMembers](../mdx/addcalculatedmembers-mdx.md) (fonction). Une valeur autre que zéro indique que le membre calculé est visible. La valeur par défaut de cette propriété est *Visible*.<br /><br /> Les membres calculés qui ne sont pas visibles (possédant la valeur zéro) sont généralement utilisés comme étapes intermédiaires dans des membres calculés plus complexes. Ces membres calculés peuvent également être référencés par d'autres types de membres, tels que des mesures.|  
|NON_EMPTY_BEHAVIOR|Mesure ou jeu utilisé pour déterminer le comportement des membres calculés lors de la résolution des cellules vides.<br /><br /> **\*\* Avertissement \* \***  cette propriété est déconseillée. Évitez de l'utiliser. Pour plus d’informations, consultez [Fonctionnalités Analysis Services déconseillées dans SQL Server 2016](../analysis-services/deprecated-analysis-services-features-in-sql-server-2016.md).|  
|CAPTION|Chaîne que l'application cliente utilise à titre de légende du membre.|  
|DISPLAY_FOLDER|Chaîne qui identifie le chemin d'accès du dossier d'affichage que l'application cliente utilise pour afficher le membre. Le séparateur de niveau de dossier est défini par l'application cliente. Pour les outils et les clients fournis par [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], la barre oblique inverse (\\) est le séparateur de niveau. Pour fournir plusieurs dossiers d'affichage à un membre défini, utilisez un point-virgule (;) pour séparer les dossiers.|  
|ASSOCIATED_MEASURE_GROUP|Nom du groupe de mesures auquel ce membre est associé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Instruction de membre DROP &#40;MDX&#41;](../mdx/mdx-data-definition-drop-member.md)   
 [Instruction de mise à jour MEMBER &#40;MDX&#41;](../mdx/mdx-data-definition-update-member.md)   
 [Instructions MDX de définition de données &#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
  
