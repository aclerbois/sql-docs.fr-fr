---
title: 'Annexe b : Tables de Transition d’état ODBC | Documents Microsoft'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- state transitions [ODBC]
- transitioning states [ODBC], about state transitions
- state transitions [ODBC], about state transitions
ms.assetid: 15088dbe-896f-4296-b397-02bb3d0ac0fb
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a0d114dd574faf2909fa200f7c24ab0ccaa07804
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="appendix-b-odbc-state-transition-tables"></a>Annexe b : Tables de Transition d’état ODBC
Les tableaux de cette annexe montrent comment les fonctions ODBC provoquent des transitions de l’environnement, connexion, l’instruction et états du descripteur. L’état de l’environnement, une connexion, une instruction ou une descripteur dicte généralement lorsque les fonctions qui utilisent le type de handle (environnement, connexion, instruction ou descripteur) correspondant peuvent être appelées. Les États de l’environnement, connexion, l’instruction et descripteur se chevauchent à peu près comme indiqué dans les illustrations suivantes. Par exemple, le chevauchement exact de connexion indique C5 et C6 et états d’instruction que s1 à S12 est source de données, car les transactions commencent à des moments différents sur différentes sources de données, et l’état descripteur D1i (implicitement alloué descripteur) dépend de l’état de l’instruction qui le descripteur est associé, tandis que l’état D1e (explicitement alloué descripteur) est indépendante de l’état de n’importe quelle instruction. Pour obtenir une description de chaque état, consultez [Transitions d’environnement](../../../odbc/reference/appendixes/environment-transitions.md), [connexion Transitions](../../../odbc/reference/appendixes/connection-transitions.md), [Transitions de l’instruction](../../../odbc/reference/appendixes/statement-transitions.md), et [descripteur Transitions](../../../odbc/reference/appendixes/descriptor-transitions.md), plus loin dans cette annexe.  
  
 Les États de l’environnement et de connexion se chevauchent comme suit :  
  
 ![Chevauchement des États d’environnement et de connexion](../../../odbc/reference/appendixes/media/app01.gif "app01")  
  
 Les États de connexion et d’instruction se chevauchent comme suit :  
  
 ![Chevauchement des États de connexion et instruction](../../../odbc/reference/appendixes/media/app02.gif "app02")  
  
 Les États d’instruction et le descripteur de chevauchement comme suit :  
  
 ![États d’instruction et le descripteur se chevauchent](../../../odbc/reference/appendixes/media/app03.gif "app03")  
  
 Les États de connexion et de descripteur se chevauchent comme suit :  
  
 ![Chevauchement des États de connexion et descripteur](../../../odbc/reference/appendixes/media/app04.gif "app04")  
  
 Chaque entrée dans une table de transition peut être une des valeurs suivantes :  
  
-   **--** : L’état reste inchangé après l’exécution de la fonction.  
  
-   **E**  
     ***n*** , **C*n***, **S*n***, ou **D*n*** : l’état de l’environnement, connexion, instruction ou descripteur se déplace vers le état spécifié.  
  
-   **(INCLUENT)** : Un handle non valide a été passé à la fonction. Si le handle a été un handle null ou un handle valide d’un type incorrect, par exemple, un handle de connexion a été passé lorsqu’un descripteur d’instruction était nécessaire, la fonction retourne SQL_INVALID_HANDLE ; Sinon, le comportement est indéfini et probablement irrécupérable. Cette erreur apparaît uniquement lorsqu’il est le résultat n’est possible de l’appel de la fonction dans l’état spécifié. Cette erreur ne modifie pas l’état et est toujours détectée par le Gestionnaire de pilotes, comme indiqué par des parenthèses.  
  
-   **NS** : état suivant. La transition de l’instruction est la même que si l’instruction n’avait pas subi les États asynchrones. Par exemple, une instruction qui crée un jeu de résultats en état F11 à partir de l’état S1 car **SQLExecDirect** retourné SQL_STILL_EXECUTING. La notation NS dans état F11 signifie que les transitions de l’instruction sont les mêmes que celles d’une instruction dans un état S1 qui crée un jeu de résultats. Si **SQLExecDirect** retourne une erreur, l’instruction reste en état S1 ; si elle réussit, l’instruction se déplace à l’état S5 ; si elle a besoin de données, l’instruction se déplace à l’état S8 ; et si elle est en cours d’exécution, il reste en état F11.  
  
-   ***XXXXX*** ou **(*XXXXX*)** : une valeur SQLSTATE qui est lié à la table de transition. SQLSTATE détectée par le Gestionnaire de pilotes est placés entre parenthèses. La fonction a renvoyé SQL_ERROR et la valeur SQLSTATE spécifiée, mais l’état ne change pas. Par exemple, si **SQLExecute** est appelé avant **SQLPrepare**, elle retourne SQLSTATE HY010 (erreur de séquence de fonction).  
  
> [!NOTE]  
>  Les tables de ne pas affichent les erreurs non liés aux tables qui ne changent pas l’état de transition. Par exemple, lorsque **SQLAllocHandle** est appelée dans l’état de l’environnement E1 et retourne SQLSTATE HY001 (erreur d’allocation de mémoire), l’environnement reste en état E1 ; cela n’est pas affiché dans le tableau de transition d’environnement pour **SQLAllocHandle**.  
  
 Si l’environnement, connexion, instruction ou le descripteur peut atteindre plusieurs États, chaque état possible est indiqué, et un ou plusieurs des notes expliquent les conditions dans lesquelles chaque transition a lieu. Les notes suivantes peuvent apparaître dans n’importe quelle table.  
  
|Pied de page|Signification|  
|--------------|-------------|  
|b|Avant ou après. Le curseur est positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|c|Fonction actuelle. La fonction actuelle en cours d’exécution en mode asynchrone.|  
|d|Besoin de données. La fonction a renvoyé SQL_NEED_DATA.|  
|e|Erreur. La fonction a renvoyé SQL_ERROR.|  
|i|Ligne non valide. Le curseur est positionné sur une ligne dans le résultat de jeu et la ligne avaient été supprimées ou une erreur s’est produite dans une opération sur la ligne. Si le tableau d’état de ligne existe, la valeur dans le tableau d’état de ligne de la ligne a été SQL_ROW_DELETED ou SQL_ROW_ERROR. (Le tableau d’état de ligne est indiqué par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.)|  
|NF|Introuvable. La fonction a retourné SQL_NO_DATA. Cela ne s’applique pas quand **SQLExecDirect**, **SQLExecute**, ou **SQLParamData** retourne SQL_NO_DATA après l’exécution d’une recherche mettre à jour ou supprimer l’instruction.|  
|np|Pas préparée. L’instruction n’a pas été préparée.|  
|nr|Aucun résultat. L’instruction n’est pas ou n’a pas créé un jeu de résultats.|  
|o|Autre fonction. Une autre fonction en cours d’exécution en mode asynchrone.|  
|p|Préparée. L’instruction a été préparée.|  
|r|Résultats. L’instruction est ou n’a créé un jeu de résultats (probablement vide).|  
|s|Réussite. La fonction a retourné SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS.|  
|v|Ligne valide. Le curseur est positionné sur une ligne du jeu de résultats et la ligne avait été insérée, mises à jour avec succès, ou une autre opération sur la ligne avait été exécutée correctement. Si le tableau d’état de ligne existe, la valeur dans le tableau d’état de ligne de la ligne a été SQL_ROW_ADDED, SQL_ROW_SUCCESS ou SQL_ROW_UPDATED. (Le tableau d’état de ligne est indiqué par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.)|  
|x|En cours d'exécution. La fonction a renvoyé SQL_STILL_EXECUTING.|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 Dans cet exemple, la ligne de transition d’état de l’environnement de table pour **SQLFreeHandle** lorsque *HandleType* SQL_HANDLE_ENV est comme suit.  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT)|E0|(HY010)|  
  
 Si **SQLFreeHandle** est appelée dans l’état de l’environnement E0 avec *HandleType* défini à SQL_HANDLE_ENV, le Gestionnaire de pilotes retourne SQL_INVALID_HANDLE. Si elle est appelée avec état E1 *HandleType* défini à SQL_HANDLE_ENV, l’environnement passe à l’état E0 si la fonction réussit et qu’il reste en état E1 si la fonction échoue. Si elle est appelée avec état E2 *HandleType* défini à SQL_HANDLE_ENV, le Gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction) et l’environnement reste en état E2.  
  
 Pour comprendre les tables de transition d’état, il est nécessaire de comprendre à quel élément (environnement, connexion, instruction ou descripteur) qu’ils font référence à. Supposons qu’une fonction accepte le handle d’un élément de type X. Le tableau de transition d’état X pour cette fonction décrit comment appeler la fonction, avec le handle d’un élément de type X, a une incidence sur cet élément. Par exemple, **SQLDisconnect** accepte un handle de connexion. Le tableau de transition d’état connexion pour **SQLDisconnect** décrit comment **SQLDisconnect** affecte l’état de la connexion pour laquelle elle est appelée.  
  
 Supposons qu’une fonction accepte le handle d’un élément de type Y, où Y n’est pas égale à X. Le tableau de transition d’état X pour cette fonction décrit comment appeler la fonction, avec un descripteur de type X qui est associé à l’élément de type Y, affecte l’élément de type Y. Par exemple, l’instruction transition table d’état pour **SQLDisconnect** décrit comment **SQLDisconnect** affecte l’état d’une instruction lorsqu’elle est appelée avec le handle de la connexion à laquelle l’instruction est associée.  
  
 Cette annexe contient les rubriques suivantes.  
  
-   [Transitions d’environnement](../../../odbc/reference/appendixes/environment-transitions.md)  
  
-   [Transitions de connexion](../../../odbc/reference/appendixes/connection-transitions.md)  
  
-   [Transitions d’instruction](../../../odbc/reference/appendixes/statement-transitions.md)  
  
-   [Transitions de descripteur](../../../odbc/reference/appendixes/descriptor-transitions.md)
