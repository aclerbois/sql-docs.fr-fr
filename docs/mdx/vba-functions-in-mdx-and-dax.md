---
title: Les fonctions VBA dans MDX et DAX | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a9764d4e302a663800bd71a5c7083d985ea230bc
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34582521"
---
# <a name="vba-functions-in-mdx-and-dax"></a>Fonctions VBA dans MDX et DAX
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Ce document contient une référence croisée de toutes les fonctions VBA disponibles dans [Visual Basic pour Applications fonctions](https://msdn.microsoft.com/vba/language-reference-vba/articles/functions-visual-basic-for-applications) qui sont pris en charge dans MDX ; en outre, la liste inclut une remarque lorsqu’il existe une équivalence fonctionnelle avec le langage DAX .  
  
## <a name="visual-basic-for-applications-functions-reference"></a>Référence des fonctions Visual Basic for Applications (VBA)  
  
|Nom de fonction|Pris en charge|Remarques|  
|-------------------|---------------|-----------|  
|Abs|DAX, MDX||  
|Tableau|Non pris en charge||  
|Asc|MDX uniquement||  
|AscW|MDX uniquement||  
|Atn|MDX uniquement||  
|CallByName|Non pris en charge||  
|CBool|MDX uniquement||  
|CByte|MDX uniquement||  
|CMonnaie|MDX uniquement||  
|CDate|MDX uniquement||  
|CDbl|MDX uniquement||  
|CDec|MDX uniquement||  
|Choose|MDX uniquement||  
|Chr|MDX uniquement||  
|CEnt|MDX uniquement||  
|CLong|MDX uniquement||  
|CLngLng|Non pris en charge||  
|CLngPtr|Non pris en charge||  
|Command|Non pris en charge||  
|Cos|MDX uniquement||  
|CreateObject|Non pris en charge||  
|CSmpl|MDX uniquement||  
|CChaîne|MDX uniquement||  
|RéperCour|Non pris en charge||  
|CVar|MDX uniquement||  
|CVErr|Non pris en charge||  
|Date|MDX uniquement|**Avertissement** DAX implémente une fonction différente avec le même nom ; la fonction DATE (année, mois, jour), utilisé pour générer une valeur de type date des arguments fournis|  
|AjDate|MDX uniquement|**Avertissement** DAX implémente une fonction différente avec le même nom ; la DATEADD (\<dates >, < number_of_intervals >,\<intervalle >) fonction, utilisée pour décaler les dates donnés en un nombre d’intervalles donnés|  
|DateDiff]|MDX uniquement||  
|PartDate|MDX uniquement||  
|SérieDate|MDX uniquement||  
|DateValue]|DAX, MDX||  
|Jour|DAX, MDX||  
|DDB|MDX uniquement||  
|Réper|Non pris en charge||  
|DoEvents|Non pris en charge||  
|Environ|Non pris en charge||  
|EOF|Non pris en charge||  
|Error|Non pris en charge||  
|Exp|DAX, MDX||  
|FileAttr|Non pris en charge||  
|DateHeureFich|Non pris en charge||  
|LongFich|Non pris en charge||  
|Filtrer|Non pris en charge|**Avertissement** MDX implémente une fonction différente avec le même nom ; la fonction FILTER (Set_Expression, Logical_Expression) retourne le jeu qui résulte du filtrage d’un jeu spécifié selon une condition de recherche à partir des arguments fournis<br /><br /> **Avertissement** DAX implémente une fonction différente avec le même nom ; le filtre (\<table >,\<filtre >) fonction retourne une table qui représente un sous-ensemble d’une autre table ou une expression des arguments fournis|  
|Fix|MDX uniquement||  
|Format (Visual Basic for Applications)|DAX, MDX||  
|FormatCurrency|Non pris en charge||  
|FormatDateTime|Non pris en charge||  
|FormatNumber|Non pris en charge||  
|FormatPercent|Non pris en charge||  
|FreeFile|Non pris en charge||  
|VC|MDX uniquement||  
|LireTousParam|Non pris en charge||  
|LireAttr|Non pris en charge||  
|GetObject|Non pris en charge||  
|GetSetting|Non pris en charge||  
|Hex|MDX uniquement||  
|Heure|DAX, MDX||  
|Iif|MDX uniquement|**Avertissement** DAX implémente une fonction similaire avec le nom : IF (logical_test, value_if_true, value_if_false) (fonction).|  
|IMEStatus|Non pris en charge||  
|Entrée|Non pris en charge||  
|InputBox|Non pris en charge||  
|InStr|MDX uniquement||  
|InStrRev|Non pris en charge||  
|Int|DAX, MDX||  
|IPmt|MDX uniquement||  
|IRR|MDX uniquement||  
|IsArray|MDX uniquement||  
|IsDateMDX uniquement||  
|IsEmpty|MDX uniquement||  
|IsError|DAX, MDX||  
|IsMissing|MDX uniquement||  
|IsNull|MDX uniquement||  
|IsNumeric|MDX uniquement||  
|IsObject|Non pris en charge||  
|Join|Non pris en charge||  
|LBound|Non pris en charge||  
|Minuscule|MDX uniquement||  
|Gauche|DAX, MDX||  
|NbCar|DAX, MDX||  
|Loc|Non pris en charge||  
|LOF|Non pris en charge||  
|Journal|MDX uniquement|**Important** DAX implémente une fonction différente avec le même nom ; la fonction LOG (number, base). Retourne le logarithme d'un nombre à la base spécifiée à partir des arguments fournis.|  
|SupprGauche|MDX uniquement||  
|MacID|Non pris en charge||  
|MacScript|Non pris en charge||  
|ExtracChaîne|DAX, MDX||  
|Minute|DAX, MDX||  
|MIRR|MDX uniquement||  
|Month|DAX, MDX||  
|MonthName|Non pris en charge||  
|MsgBox|Non pris en charge||  
|maintenant|DAX, MDX||  
|Npm]|MDX uniquement||  
|NPV|MDX uniquement||  
|Oct|MDX uniquement||  
|Partition|MDX uniquement||  
|Vpm|MDX uniquement||  
|PPmt|MDX uniquement||  
|PV|MDX uniquement||  
|RVBC|MDX uniquement||  
|Taux|MDX uniquement||  
|Remplacer|Non pris en charge||  
|RGB|MDX uniquement||  
|Droit|DAX, MDX||  
|Aléat|MDX uniquement||  
|Arrondi|DAX, MDX||  
|SupprDroite|MDX uniquement||  
|Seconde|DAX, MDX||  
|Seek|Non pris en charge||  
|Sgn|DAX, MDX||  
|Shell|Non pris en charge||  
|Sin|MDX uniquement||  
|AmorLin|MDX uniquement||  
|Espace|MDX uniquement||  
|Spc|Non pris en charge||  
|fractionnement|Non pris en charge||  
|Racine|MDX uniquement||  
|NumChaîne|MDX uniquement||  
|CompChaîne|MDX uniquement||  
|ConvChaîne|MDX uniquement||  
|Chaîne]|MDX uniquement||  
|StrReverse|Non pris en charge||  
|Commutateur|MDX uniquement||  
|SYD|MDX uniquement||  
|Onglet|Non pris en charge||  
|Tan|MDX uniquement||  
|Time|Non pris en charge||  
|Minuterie|MDX uniquement||  
|SérieHeure|MDX uniquement||  
|VHeure|DAX, MDX||  
|La fonction Trim]|DAX, MDX||  
|TypeName|MDX uniquement||  
|UBound|Non pris en charge||  
|UCase|MDX uniquement||  
|Val|MDX uniquement||  
|VarType|Non pris en charge||  
|JourSem|DAX, MDX||  
|WeekdayName|Non pris en charge||  
|Année|DAX, MDX||  
  
  
