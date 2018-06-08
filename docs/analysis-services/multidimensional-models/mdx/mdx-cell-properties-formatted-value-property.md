---
title: LANGUAGE et FORMAT_STRING sur FORMATTED_VALUE | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1ad2038e28afb455dd1ad239a2bf02cab99ed4d9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-cell-properties---formattedvalue-property"></a>Propriétés de cellule MDX - FORMATTED_VALUE (propriété)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  La propriété FORMATTED_VALUE est basée sur les interactions des propriétés VALUE, FORMAT_STRING et LANGUAGE de la cellule. Cette rubrique explique comment ces propriétés interagissent pour générer la propriété FORMATTED_VALUE.  
  
## <a name="value-formatstring-language-properties"></a>Propriétés VALUE, FORMAT_STRING et LANGUAGE  
 La table suivante décrit ces propriétés afin de vous aider à les combiner.  
  
 VALUE  
 Valeur sans mise en forme de la cellule.  
  
 FORMAT_STRING  
 Modèle de mise en forme à appliquer à la valeur de la cellule pour générer la propriété FORMATTED_VALUE  
  
 LANGUAGE  
 Spécification de paramètres régionaux à appliquer dans toute la propriété FORMAT_STRING pour générer une version localisée de FORMATTED_VALUE  
  
## <a name="formattedvalue-constructed"></a>Création de FORMATTED_VALUE  
 La propriété FORMATTED_VALUE est créée en utilisant la valeur de la propriété VALUE et en appliquant le modèle de mise en forme spécifié dans la propriété FORMAT_STRING à cette valeur. De plus, chaque fois que la valeur de mise en forme est un **littéral de mise en forme nommée** , la spécification de propriété LANGUAGE modifie la sortie de FORMAT_STRING pour se conformer à l’usage de la langue pour la mise en forme nommée. Les littéraux de mise en forme nommée sont tous définis d'une façon permettant leur localisation. Par exemple, `"General Date"` est une spécification qui peut être localisée, par opposition au modèle suivant `"YYYY-MM-DD hh:nn:ss",` qui présente la date sous la forme définie par le modèle indépendamment de la spécification de langue.  
  
 S'il y a un conflit entre le modèle FORMAT_STRING et la spécification LANGUAGE, le modèle FORMAT_STRING remplace la spécification LANGUAGE. Par exemple, si FORMAT_STRING="$ #0" et LANGUAGE=1034 (Espagne), et si VALUE=123.456, puis FORMATTED_VALUE="$ 123" au lieu de FORMATTED_VALUE="€ 123", le format attendu est en euros car la valeur du modèle de mise en forme remplace la langue spécifiée.  
  
### <a name="examples"></a>Exemples  
 Les exemples suivants affichent la sortie obtenue lorsque LANGUAGE est associé à FORMAT_STRING.  
  
 Le premier exemple présente des valeurs numériques de mise en forme et le deuxième exemple présente des valeurs de date et d'heure de mise en forme.  
  
 Pour chaque exemple, le code MDX (Multidimensional Expressions) est fourni.  
  
 `with`  
  
 `member measures.A as 5040, FORMAT_STRING="Currency"`  
  
 `member measures.B as measures.A, LANGUAGE=1034`  
  
 `member measures.C as measures.A, LANGUAGE=1034 , FORMAT_STRING="$#,##0.00"`  
  
 `member measures.D as measures.A, FORMAT_STRING="Scientific"`  
  
 `member measures.E as measures.A, LANGUAGE=1034 , FORMAT_STRING="Scientific"`  
  
 `member measures.F as 0.5040, FORMAT_STRING="Percent"`  
  
 `member measures.G as measures.F, LANGUAGE=1034`  
  
 `member measures.H as 0, LANGUAGE=1034 , FORMAT_STRING="Yes/No"`  
  
 `member measures.I as 59, LANGUAGE=1034 , FORMAT_STRING="Yes/No"`  
  
 `member measures.J as 0, LANGUAGE=1034 , FORMAT_STRING="ON/OFF"`  
  
 `member measures.K as -312, LANGUAGE=1034 , FORMAT_STRING="ON/OFF"`  
  
 `Select {measures.A, measures.B, measures.C, measures.D, measures.E, measures.F, measures.G, measures.H, measures.I, measures.J, measures.K} on 0`  
  
 `from [Adventure Works]`  
  
 `cell properties VALUE, FORMAT_STRING, LANGUAGE, FORMATTED_VALUE`  
  
 Les résultats transposés obtenus sont les suivants quand la requête MDX ci-dessus est exécutée à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] sur un serveur et un client avec les paramètres régionaux 1033 :  
  
|Membre|FORMATTED_VALUE|Explication|  
|------------|----------------------|-----------------|  
|Objet|$5,040.00|FORMAT_STRING a pour valeur `Currency` et LANGUAGE a pour valeur `1033`, valeur héritée des paramètres régionaux système.|  
|B|€5.040,00|FORMAT_STRING a pour valeur `Currency` (hérité de A) et LANGUAGE a pour valeur explicite `1034` (Espagne), ce qui explique le symbole de l’euro, le séparateur décimal différent et le séparateur des milliers différent.|  
|C|$5.040,00|FORMAT_STRING a pour valeur `$#,##0.00` en remplacement de la devise héritée de A, et LANGUAGE a pour valeur explicite `1034` (Espagne). Étant donné que la propriété FORMAT_STRING a pour valeur explicite le symbole monétaire $, FORMATTED_VALUE est présenté avec le signe $. Toutefois, étant donné que `.` (point) et `,` (virgule) sont respectivement des espaces réservés pour le séparateur décimal et le séparateur des milliers, la spécification de langue les affecte et une sortie localisée pour les séparateurs décimal et des milliers est générée.|  
|D|5.04E+03|FORMAT_STRING a pour valeur `Scientific` et LANGUAGE a pour valeur `1033`, valeur héritée des paramètres régionaux système. Le signe `.` (point) est donc utilisé comme séparateur décimal.|  
|E|5,04E+03|FORMAT_STRING a pour valeur `Scientific` et LANGUAGE a pour valeur explicite `1034,` le signe `,` (virgule) est donc utilisé comme séparateur décimal.|  
|F|50.40%|FORMAT_STRING a pour valeur `Percent` et LANGUAGE a pour valeur `1033`, valeur héritée des paramètres régionaux système. Le signe `.` (point) est donc utilisé comme séparateur décimal.<br /><br /> Notez que VALUE est passé de 5040 à 0.5040|  
|G|50,40|FORMAT_STRING a pour valeur `Percent`, valeur héritée de F, et LANGUAGE a pour valeur explicite `1034` . Le signe `,` (virgule) est donc utilisé comme séparateur décimal.<br /><br /> Notez que VALUE a été hérité de la valeur F.|  
|H|Non|FORMAT_STRING a pour valeur `YES/NO`, VALUE a pour valeur 0 et LANGUAGE a pour valeur explicite `1034`. Étant donné qu’il n’y a aucune différence entre le NO anglais et le NO espagnol, l’utilisateur ne voit aucune différence au niveau de FORMATTED_VALUE.|  
|I|SI|FORMAT_STRING a pour valeur `YES/NO`, VALUE a pour valeur 59 et LANGUAGE a pour valeur explicite `1034`. Comme défini pour la mise en forme de YES/NO, toute valeur différente de zéro (0) est égale à YES et comme la langue est l’espagnol, FORMATTED_VALUE a pour valeur SI.|  
|J|Desactivado|FORMAT_STRING a pour valeur `ON/OFF`, VALUE a pour valeur 0 et LANGUAGE a pour valeur explicite `1034`. Comme défini pour la mise en forme de ON/OFF, toute valeur égale à zéro (0) est égale à OFF et comme la langue est l’espagnol, FORMATTED_VALUE a pour valeur Desactivado.|  
|K|Activado|FORMAT_STRING a pour valeur `ON/OFF`, VALUE a pour valeur -312 et LANGUAGE a pour valeur explicite `1034`. Comme défini pour la mise en forme de ON/OFF, toute valeur différente de zéro (0) est égale à ON et comme la langue est l'espagnol, FORMATTED_VALUE a pour valeur Activado.|  
  
 `with`  
  
 `member measures.A as 'CDate("1959-03-12 06:30")'`  
  
 `member measures.B as measures.A, FORMAT_STRING="Long Date"`  
  
 `member measures.C as measures.A, LANGUAGE=1034 , FORMAT_STRING="General Date"`  
  
 `member measures.D as measures.A, LANGUAGE=1034, FORMAT_STRING="Long Date"`  
  
 `member measures.E as measures.A, LANGUAGE=1041 , FORMAT_STRING="General Date"`  
  
 `member measures.F as measures.A, LANGUAGE=1041 , FORMAT_STRING="Long Date"`  
  
 `member measures.G as measures.A, FORMAT_STRING="Long Time"`  
  
 `member measures.H as measures.A, FORMAT_STRING="Short Time"`  
  
 `member measures.I as measures.A, LANGUAGE=1034 , FORMAT_STRING="Long Time"`  
  
 `member measures.J as measures.A, LANGUAGE=1034 , FORMAT_STRING="Short Time"`  
  
 `member measures.K as measures.A, LANGUAGE=1041 , FORMAT_STRING="Long Time"`  
  
 `member measures.L as measures.A, LANGUAGE=1041 , FORMAT_STRING="Short Time"`  
  
 `Select {measures.A, measures.B, measures.C, measures.D, measures.E, measures.F`  
  
 `, measures.G, measures.H, measures.I, measures.J, measures.K, measures.L} on 0`  
  
 `from [Adventure Works]`  
  
 `cell properties VALUE, FORMAT_STRING, LANGUAGE, FORMATTED_VALUE`  
  
 Les résultats transposés obtenus sont les suivants quand la requête MDX ci-dessus est exécutée à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] sur un serveur et un client avec les paramètres régionaux 1033 :  
  
|Membre|FORMATTED_VALUE|Explication|  
|------------|----------------------|-----------------|  
|Objet|3/12/1959 6:30:00 AM|La valeur de FORMAT_STRING est implicitement définie sur `General Date` par l’expression CDate() et LANGUAGE a pour valeur `1033` (anglais), valeur héritée des paramètres régionaux système.|  
|B|Thursday, March 12, 1959|FORMAT_STRING a pour valeur explicite `Long Date` et LANGUAGE a pour valeur `1033` (anglais), valeur héritée des paramètres régionaux système.|  
|C|12/03/1959 6:30:00|FORMAT_STRING a pour valeur explicite `General Date` et LANGUAGE a pour valeur explicite `1034` (espagnol).<br /><br /> Notez que le mois et le jour sont inversés par rapport au style de mise en forme américain.|  
|D|jueves, 12 de marzo de 1959|FORMAT_STRING a pour valeur explicite `Long Date` et LANGUAGE a pour valeur explicite `1034` (espagnol).<br /><br /> Notez que le mois et le jour de la semaine sont rédigés en espagnol.|  
|E|1959/03/12 6:30:00|FORMAT_STRING a pour valeur explicite `General Date` et LANGUAGE a pour valeur explicite `1041` (japonais).<br /><br /> Notez que la date est maintenant au format année/mois/jour heure:minutes:secondes.|  
|F|1959年3月12日|FORMAT_STRING a pour valeur explicite `Long Date` et LANGUAGE a pour valeur explicite `1041` (japonais).|  
|G|6:30:00 AM|FORMAT_STRING a pour valeur explicite `Long Time` et LANGUAGE a pour valeur `1033` (anglais), valeur héritée des paramètres régionaux système.|  
|H|06:30|FORMAT_STRING a pour valeur explicite `Short Time` et LANGUAGE a pour valeur `1033` (anglais), valeur héritée des paramètres régionaux système.|  
|I|6:30:00|FORMAT_STRING a pour valeur explicite `Long Time` et LANGUAGE a pour valeur explicite `1034` (espagnol).|  
|J|06:30|FORMAT_STRING a pour valeur explicite `Short Time` et LANGUAGE a pour valeur explicite `1034` (espagnol).|  
|K|6:30:00|FORMAT_STRING a pour valeur explicite `Long Time` et LANGUAGE a pour valeur explicite `1041` (japonais).|  
|L|06:30|FORMAT_STRING a pour valeur explicite `Short Time` et LANGUAGE a pour valeur explicite `1041` (japonais).|  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu de FORMAT_STRING & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-cell-properties-format-string-contents.md)   
 [À l’aide des propriétés de cellule & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-cell-properties-using-cell-properties.md)   
 [Création et utilisation des valeurs de propriété & #40 ; MDX & #41 ;](http://msdn.microsoft.com/library/0cafb269-03c8-4183-b6e9-220f071e4ef2)   
 [Principes de base de requête MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)  
  
  
