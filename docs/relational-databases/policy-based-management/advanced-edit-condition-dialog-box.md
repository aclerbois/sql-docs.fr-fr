---
title: Modification avancée (Condition), boîte de dialogue | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.condition.advancededit.f1
ms.assetid: a0bbe501-78c5-45ad-9087-965d04855663
caps.latest.revision: 44
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 375d21441757d274dbd62bcd94638b03a0ded57f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="advanced-edit-condition-dialog-box"></a>Boîte de dialogue Modification avancée (Condition)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La boîte de dialogue **Modification avancée** permet de créer des expressions complexes pour des conditions de la gestion basée sur des stratégies.  
  
## <a name="options"></a>Options  
 **Valeur de cellule**  
 Affiche la fonction ou l'expression qui sera utilisée pour la valeur de cellule lorsque vous la créez. Lorsque vous cliquez sur **OK**, la valeur de cellule apparaît dans la cellule **Champ** ou **Valeur** dans la zone d'expression de condition de la boîte de dialogue **Créer une nouvelle condition** ou **Ouvrir une condition** de la page **Général** .  
  
 **Fonctions et propriétés**  
 Affiche les fonctions et propriétés disponibles.  
  
 **Détails**  
 Affiche les informations relatives aux fonctions et propriétés, au format suivant : signature de fonction, description de fonction, valeur de retour et exemple.  
  
## <a name="syntax"></a>Syntaxe  
 Les expressions valides doivent avoir le format suivant :  
  
 `{property | function | constant}`  
  
 `{operator}`  
  
 `{property | function | constant}`  
  
## <a name="examples"></a>Exemples  
 Voici quelques exemples d'expressions valides :  
  
-   *Property1*> 5  
  
-   *Property1*=*Property2*  
  
-   Add(5, Multiply(.2,*Property1*))<*Property2*  
  
-   *Sometext* IN *Property1*  
  
-   *Property1*< Fn(*Property2*)  
  
-   BitwiseAnd(*Property1*,*Property2*)= 0  
  
## <a name="additional-function-information"></a>Informations supplémentaires sur les fonctions  
 Les sections suivantes fournissent des informations supplémentaires sur les fonctions que vous pouvez utiliser afin de créer des expressions complexes pour des conditions de la Gestion basée sur des stratégies.  
  
> **IMPORTANT !** Les fonctions que vous pouvez utiliser pour créer des conditions de Gestion basée sur des stratégies n'utilisent pas toujours la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] . Veillez à suivre l'exemple de syntaxe. Par exemple, lorsque vous utilisez les fonctions **DateAdd** ou **DatePart** , vous devez placer l'argument *datepart* entre guillemets simples.  
  
|Fonction|Signature|Description|Arguments|Valeur retournée| Exemple|  
|--------------|---------------|-----------------|---------------|------------------|-------------|  
|**Add()**|Numeric Add (Numeric *expression1*, Numeric *expression2*)|Additionne deux nombres.|*expression1* et *expression2* : expression valide de l’un des types de données de la catégorie numérique, à l’exception du type de données **datetime** . Peut être une constante, une propriété ou une fonction qui retourne un type numérique.|Retourne le type de données de l'argument ayant la précédence la plus élevée.|`Add(Property1, 5)`|  
|**Array()**|Array Array (VarArgs *expression*)|Crée un tableau à partir d'une liste de valeurs. Peut être utilisée avec des fonctions d'agrégation telles que Sum() et Count().|*expression* : expression destinée à être convertie en tableau.|Tableau|`Array(2,3,4,5,6)`|  
|**Avg()**|Numeric Avg (*VarArgs*)|Retourne la moyenne des valeurs dans la liste des arguments.|*VarArgs* : liste d’expressions de type Variant de la catégorie de types de données de valeur numérique exacte ou approximative, à l’exception du type de données **bit** .|Le type de retour est déterminé par le type du résultat de l’expression.<br /><br /> Si le résultat de l'expression est de catégorie **integer**, **decimal**, **money** et **smallmoney**, **float** et **real** , les types de retour sont **int**, **decimal**, **money**et **float**; respectivement.|`Avg(1.0, 2.0, 3.0, 4.0, 5.0)` retourne `3.0` dans cet exemple.|  
|**BitwiseAnd()**|Numeric BitwiseAnd (Numeric *expression 1*, Numeric *expression2*)|Effectue une opération AND logique au niveau du bit avec deux valeurs entières.|*expression1* et *expression2* : expression valide de l’un des types de données de la catégorie des types de données entier (Integer).|Retourne une valeur de catégorie de type de données Integer.|`BitwiseAnd(Property1, Property2)`|  
|**BitwiseOr()**|Numeric BitwiseOr (Numeric *expression1*, Numeric *expression2*)|Effectue une opération OR logique au niveau du bit avec deux valeurs entières spécifiées.|*expression1* et *expression2* : expression valide de l’un des types de données de la catégorie des types de données entier (Integer).|Retourne une valeur de catégorie de type de données Integer.|`BitwiseOr(Property1, Property2)`|  
|**Concatenate()**|String Concatenate (String *string1*, String *string2*)|Concatène deux chaînes.|*string1* et *string2* : les deux chaînes à concaténer. Peuvent être n'importe quelle chaîne valide non Null.|La chaîne concaténée, avec *string1* suivie de *string2*.|`Concatenate("Hello", " World` `")` retourne "`Hello World`".|  
|**Count()**|Numeric Count (*VarArgs*)|Retourne le nombre d'éléments dans la liste d'arguments.|*VarArgs* : expression de tout type, à l’exception de **text**, **image**et **ntext**.|Retourne une valeur de catégorie de type de données Integer.|`Count(1.0, 2.0, 3.0, 4.0, 5.0)` retourne `5` dans cet exemple.|  
|**DateAdd()**|DateTime DateAdd (String *datepart*, Numeric *number*, DateTime *date*)|Retourne une nouvelle valeur **datetime** basée sur l'ajout d'un intervalle à la date spécifiée.|*datepart* : paramètre qui spécifie dans quelle partie de la date retourner une nouvelle valeur. Parmi les types pris en charge, citons : year(yy, yyyy), month (mm, m) et dayofyear(dy, y). Pour plus d’informations, consultez [DATEADD &#40;Transact-SQL&#41;](../../t-sql/functions/dateadd-transact-sql.md).<br /><br /> *number* : valeur utilisée pour incrémenter *datepart*.<br /><br /> *date* : expression qui retourne une valeur **datetime** ou une chaîne de caractères dans un format de date.|La nouvelle valeur **datetime** basée sur l'ajout d'un intervalle à la date spécifiée.|**Exemple :** `DateAdd('day', 21, DateTime('2007-08-06 14:21:50'))` retourne `'2007-08-27 14:21:50'` dans cet exemple.<br /><br /> Voici les *dateparts* et les abréviations pris en charge par cette fonction :<br /><br /> **year**: yy, yyyy<br /><br /> **month**: mm, m<br /><br /> **dayofyear**: dy, y<br /><br /> **day**: dd, d<br /><br /> **week**: wk, ww<br /><br /> **weekday**: dw, w<br /><br /> **hour**: hh<br /><br /> **minute**: mi, n<br /><br /> **second**: ss, s<br /><br /> **millisecond**: ms|  
|**DatePart()**|Numeric DatePart (String *datepart*, DateTime *date*)|Retourne un entier qui représente la valeur *datepart* précisée de la date données.|*datepart* : paramètre qui spécifie la partie de la date à retourner. Parmi les types pris en charge, citons : year(yy, yyyy), month (mm, m) et dayofyear(dy, y). Pour plus d’informations, consultez [DATEPART &#40;Transact-SQL&#41;](../../t-sql/functions/datepart-transact-sql.md).<br /><br /> *date* : expression qui retourne une valeur **datetime** ou une chaîne de caractères dans un format de date.|Retourne la valeur d'une catégorie de type de données integer qui représente l'élément *datepart* spécifié de la date donnée.|`DatePart('month', DateTime('2007-08-06 14:21:50.620'))` retourne `8` dans cet exemple.|  
|**DateTime()**|DateTime DateTime (String *dateString*)|Crée une valeur datetime à partir d'une chaîne.|*dateString* : valeur datetime sous forme de chaîne.|Renvoie une valeur datetime créée à partir de la chaîne d'entrée.|`DateTime('3/12/2006')`|  
|**Divide()**|Numeric Divide (Numeric *expression_dividend*, Numeric *expression_divisor*)|Divise un nombre par un autre.|*expression_dividend* : expression numérique à diviser. Le dividende peut être toute expression valide de l'un des types de données de la catégorie des types de données numériques, sauf le type de données **datetime** .<br /><br /> *expression_divisor* : expression numérique par laquelle diviser le dividende. Le diviseur peut être toute expression valide de l'un des types de données de la catégorie des types de données numériques, sauf le type de données **datetime** .|Retourne le type de données de l'argument ayant la précédence la plus élevée.|**Exemple :** `Divide(Property1, 2)`<br /><br /> Remarque : il s'agit d'une double opération. Pour effectuer une comparaison d'entiers, vous devez combiner les résultats avec `Round()`. Par exemple : `Round(Divide(10, 3), 0) = 3`.|  
|**Enum()**|Numeric Enum (String *enumTypeName*, String *enumValueName*)|Crée une valeur enum à partir d'une chaîne.|*enumTypeName* : nom du type enum.<br /><br /> *enumValueName* : valeur de l’enum.|Retourne la valeur enum sous forme de valeur numérique.|`Enum('CompatibilityLevel','Version100')`|  
|**Escape()**|String Escape (String *replaceString*, String *stringToEscape*, String *escapeString*)|Place dans une séquence d'échappement une sous-chaîne de la chaîne d'entrée avec une chaîne d'échappement donnée.|*replaceString* - Chaîne d'entrée.<br /><br /> *stringToEscape* - Sous-chaîne de *replaceString*. Il s'agit de la chaîne devant laquelle ajouter une chaîne d'échappement.<br /><br /> *escapeString* – Chaîne d'échappement à ajouter devant chaque instance de *stringToEscape*.|Retourne un *replaceString* modifié dans lequel chaque instance de *stringToEscape* est précédée de *escapeString*.|`Escape("Hello", "l", "[")` retourne "`He[l[lo`".|  
|**ExecuteSQL()**|Variant ExecuteSQL (String *returnType*, String *sqlQuery*)|Exécute la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] contre le serveur cible.<br /><br /> Pour plus d’informations sur ExecuteSql(), consultez [Fonction ExecuteSql()](http://blogs.msdn.com/b/sqlpbm/archive/2008/07/03/executesql.aspx).|*returnType* : spécifie le type de retour des données retournées par l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les littéraux valides pour *returnType* sont les suivantes : **Numeric**, **String**, **Bool**, **DateTime**, **Array**et **Guid**.<br /><br /> *sqlQuery* : chaîne qui contient la requête à exécuter.||`ExecuteSQL ('Numeric', 'SELECT COUNT(*) FROM msdb.dbo.sysjobs') <> 0`<br /><br /> Exécute une requête Transact-SQL scalaire dans une instance cible de SQL Server. Une seule colonne peut être spécifiée dans une instruction `SELECT` ; les colonnes supplémentaires au delà de la première sont ignorées. La requête obtenue ne doit retourner qu'une seule ligne ; les lignes supplémentaires au-delà de la première sont ignorées. Si la requête retourne un jeu vide, l'expression de condition créée autour de `ExecuteSQL` prendra la valeur false. `ExecuteSql` prend en charge les modes d'évaluation **À la demande** et **Selon la planification** .<br /><br /> -`@@ObjectName`:<br />                      Correspond au champ de nom dans [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). La variable sera remplacée par le nom de l'objet actuel.<br /><br /> -`@@SchemaName`: correspond au champ de nom dans [sys.schemas](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md). La variable sera remplacée par le nom du schéma pour l'objet actuel, le cas échéant.<br /><br /> Remarque : pour inclure un guillemet simple dans une instruction ExecuteSQL, ajoutez au guillemet simple un deuxième guillemet simple. Par exemple, pour inclure une référence à un utilisateur nommé O'Brian, tapez O"Brian.|  
|**ExecuteWQL()**|Variant ExecuteWQL (string *returnType* , string *namespace*, string *wql*)|Exécute le script WQL sur l'espace de noms fourni. L'instruction Select ne peut contenir qu'une seule colonne de retour. Si plusieurs colonnes sont fournies, une erreur est générée.|*returnType* : spécifie le type de retour des données retournées par le WQL. Les littéraux valides sont **Numeric**, **String**, **Bool**, **DateTime**, **Array**et **Guid**.<br /><br /> *namespace* : espace de noms WMI sur lequel effectuer l’exécution.<br /><br /> *wql* : chaîne qui contient le WQL à exécuter.||`ExecuteWQL('Numeric', 'root\CIMV2', 'select NumberOfProcessors from win32_ComputerSystem') <> 0`|  
|**False()**|Bool False()|Retourne la valeur booléenne FALSE.|Aucune|Retourne la valeur booléenne FALSE.|`IsDatabaseMailEnabled = False()`|  
|**GetDate()**|DateTime GetDate()|Retourne la date système.|None|Retourne la date système sous forme de valeur DateTime.|`@DateLastModified = GetDate()`|  
|**Guid()**|Guid Guid(String *guidString*)|Retourne un GUID à partir d’une chaîne.|*guidString* : représentation sous forme de chaîne du GUID à créer.|Retourne le GUID créé à partir de la chaîne.|`Guid('12340000-0000-3455-0000-000000000454')`|  
|**IsNull()**|Variant IsNull (Variant *check_expression*, Variant *replacement_value*)|La valeur de *check_expression* est retournée si sa valeur est différente de NULL ; sinon, *replacement_value* est retourné. Si les types sont différents, *replacement_value* est converti implicitement dans le type de *check_expression*.|*check_expression* : expression dans laquelle la valeur NULL est recherchée. *check_expression* peut être de n’importe quel type pris en charge par la gestion basée sur des stratégies : Numeric, String, Bool, DateTime, Array et Guid.<br /><br /> *replacement_value* : expression à retourner si *check_expression* a la valeur NULL. *replacement_value* doit être d’un type qui est implicitement converti dans le type de *check_expression*.|Le type de retour est le type de *check_expression* si *check_expression* n’a pas la valeur NULL ; sinon le type de *replacement_value* est retourné.||  
|**Len()**|Numeric Len (*string_expression*)|Retourne le nombre de caractères, de l'expression de chaîne donnée, sans espaces blancs de fin.|*string_expression* : expression de chaîne à évaluer.|Retourne une valeur de catégorie de type de données Integer.|`Len('Hello')` retourne `5` dans cet exemple.|  
|**Lower()**|String Lower (String *_expression*)|Retourne la chaîne après la conversion de toutes les majuscules en minuscules.|*expression* : expression de chaîne source.|Retourne une chaîne qui représente l'expression de chaîne source une fois toutes les majuscules converties en minuscules.|`Len('HeLlO')` retourne `'hello'` dans cet exemple.|  
|**Mod()**|Numeric Mod (Numeric *expression_dividend*, Numeric *expression_divisor*)|Fournit le reste entier de la division de la première expression numérique par la deuxième expression numérique.|*expression_dividend* : expression numérique à diviser. *expression_dividend* doit être une expression valide de l’un des types de données des catégories de types de données integer (entier) ou numeric (numérique).<br /><br /> *expression_divisor* : expression numérique par laquelle diviser le dividende. *expression_divisor* doit être une expression valide de l’un des types de données des catégories de type de données integer (entier) ou numeric (numérique).|Retourne une valeur de catégorie de type de données Integer.|`Mod(Property1, 3)`|  
|**Multiply()**|Numeric Multiply (Numeric *expression1*, Numeric *expression2*)|Multiplie deux expressions.|*expression1* et *expression2* : expression valide de l’un des types de données de la catégorie numérique, à l’exception du type de données **datetime** .|Retourne le type de données de l'argument ayant la précédence la plus élevée.|`Multiply(Property1, .20)`|  
|**Power()**|Numeric Power (Numeric *numeric_expression*, Numeric *expression_power*)|Retourne la valeur de l'expression spécifiée à la puissance indiquée.|*numeric_expression* : expression de la catégorie de type de données numérique exacte ou approximative, à l’exception du type de données bit.<br /><br /> *expression_power* : puissance à laquelle élever *numeric_expression*. *expression_power* peut être une expression de la catégorie de type de données numérique exacte ou approximative, à l’exception du type de données **bit** .|Le type de retour est identique à celui de *numeric_expression*.|`Power(Property1, 3)`|  
|**Round()**|Numeric Round (Numeric *expression*, Numeric *expression_precision*)|Renvoie une expression numérique, arrondie à la longueur ou à la précision indiquée.|*expression* : expression de la catégorie de type de données numérique exacte ou approximative, à l’exception du type de données **bit** .<br /><br /> *expression_precision* : précision avec laquelle l’expression doit être arrondie. Quand *expression_precision* est un nombre positif, *numeric_expression* est arrondi au nombre de décimales indiqué par length. Quand *expression_precision* est un nombre négatif, *numeric_expression* est arrondi à gauche de la décimale, comme le spécifie *expression_precision*.|Retourne le même type que *numeric_expression*.|`Round(5.333, 0)`|  
|**String()**|String String (Variant *_expression*)|Convertit une variante en chaîne.|*expression* : expression de type Variant à convertir en chaîne.|Retourne la valeur de l'expression Variant sous forme de chaîne.|`String(4)`|  
|**Sum()**|Numeric Sum (*VarArgs*)|Retourne la somme de toutes les valeurs dans la liste d'arguments. Sum peut être utilisée avec des valeurs numériques.|*VarArgs*: liste d’expressions de type Variant de la catégorie de types de données de valeur numérique exacte ou approximative, à l’exception du type de données **bit** .|Retourne le total de toutes les valeurs d'expression dans le type de données d'expression le plus précis.<br /><br /> Si le résultat de l'expression est de catégorie **integer**, **numeric**, **money** et **small money**, **float** et **real** , les types de retour sont **int**, **numeric**, **money**et **float**; respectivement.|`Sum(1.0, 2.0, 3.0, 4.0, 5.0)` retourne `15` dans cet exemple.|  
|**True()**|Bool TRUE()|Retourne la valeur booléenne TRUE.||Retourne la valeur booléenne TRUE.|`IsDatabaseMailEnabled = True()`|  
|**Upper()**|String Upper (String *_expression*)|Retourne la chaîne après la conversion de toutes les minuscules en majuscules.|*expression* : expression de chaîne source.|Retourne une chaîne qui représente l'expression de chaîne source une fois toutes les minuscules converties en majuscules.|`Upper('HeLlO')` retourne `'HELLO'` dans cet exemple.|  
  
## <a name="see-also"></a>Voir aussi  
 [Boîte de dialogue Créer une nouvelle condition ou Ouvrir une condition, page Général](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md)   
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
  
