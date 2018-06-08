---
title: Index sur les colonnes calculées | Microsoft Docs
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- computed columns, index creation
- index creation [SQL Server], computed columns
- imprecise columns
- persisted computed columns
- precise [SQL Server]
ms.assetid: 8d17ac9c-f3af-4bbb-9cc1-5cf647e994c4
caps.latest.revision: 41
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7cfaac5fd37f389db2569906a21b02627241176b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="indexes-on-computed-columns"></a>Index sur les colonnes calculées
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Vous pouvez définir des index sur des colonnes calculées si les règles suivantes sont respectées :  
  
-   Conditions requises liées à la propriété  
-   Conditions requises liées au déterminisme  
-   Conditions requises liées à la précision  
-   Conditions requises liées aux types de données  
-   Conditions requises liées à l'option SET  
  
**Ownership Requirements**  
  
Toutes les références de fonctions dans la colonne calculée doivent avoir le même propriétaire que la table.  
  
**Determinism Requirements**  
  
> [!IMPORTANT]  
>  Une expression est déterministe lorsqu'elle retourne toujours le même résultat pour un ensemble donné d'entrées. La propriété **IsDeterministic** de la fonction [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) indique si un paramètre *computed_column_expression* est déterministe ou non.  
  
 Le paramètre *computed_column_expression* doit être déterministe. Un paramètre *computed_column_expression* est déterministe quand une ou plusieurs des conditions suivantes sont remplies :  
  
-   Toutes les fonctions référencées par l'expression sont déterministes et précises, notamment les fonctions définies par l'utilisateur et les fonctions intégrées. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md). Les fonctions peuvent être imprécises si la colonne calculée est de type PERSISTED. Pour plus d’informations, consultez [Création d’index sur des colonnes calculées persistantes](#BKMK_persisted) , plus loin dans cette rubrique.  
  
-   Toutes les colonnes auxquelles l'expression fait référence proviennent de la table contenant la colonne calculée.  
  
-   Aucune référence de colonne n'extrait de données provenant de plusieurs lignes. Par exemple, certaines fonctions d’agrégation, telles que SUM ou AVG, dépendent de données provenant de plusieurs lignes et rendraient un paramètre *computed_column_expression* non déterministe.  
  
-   Le paramètre *computed_column_expression* n’a pas d’accès aux données système ni utilisateur.  
  
Une colonne calculée qui contient une expression CLR (Common Language Runtime) doit être déterministe et marquée comme PERSISTED avant de pouvoir être indexée. Les expressions de type CLR définies par l'utilisateur sont permises dans les définitions de colonnes calculées. Les colonnes calculées de type CLR définies par l'utilisateur peuvent être indexées à condition que le type soit comparable. Pour plus d’informations, consultez [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
> [!IMPORTANT]  
>  Lorsque vous faites référence aux littéraux de chaîne de type de données de date dans des colonnes calculées indexées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il est recommandé de convertir explicitement le littéral dans le type de date souhaité à l'aide d'un style de format de date déterministe. Pour obtenir la liste des styles de formats de date déterministes, consultez [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). 

> [!NOTE]
> Les expressions qui impliquent une conversion implicite des chaînes de caractères en type de données de date sont considérées comme non déterministes, à moins que le niveau de compatibilité de la base de données ne soit réglé sur 80 ou sur une valeur inférieure. Cela est dû au fait que les résultats dépendent des paramètres [LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) et [DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) de la session serveur. 
>
> Par exemple, les résultats de l'expression `CONVERT (datetime, '30 listopad 1996', 113)` dépendent du paramètre LANGUAGE, car la chaîne '`30 listopad 1996`' désigne des mois différents selon la langue. 
> De même, dans l'expression `DATEADD(mm,3,'2000-12-01')`, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] interprète la chaîne `'2000-12-01'` en se basant sur le paramètre DATEFORMAT.  
>   
> La conversion implicite de données de caractères non Unicode entre les classements est aussi considérée comme non déterministe, à moins que le niveau de compatibilité soit égal ou inférieur à 80.  
>   
> Lorsque le niveau de compatibilité de la base de données est égal à 90, vous ne pouvez pas créer d'index sur les colonnes calculées qui contiennent ces expressions. Cependant, les colonnes calculées existantes qui contiennent ces expressions provenant d'une base de données mise à niveau sont gérables. Si vous utilisez des colonnes calculées indexées contenant une chaîne implicite pour les conversions de dates, afin d'éviter tout endommagement éventuel d'index, assurez-vous que les paramètres LANGUAGE et DATEFORMAT sont cohérents dans vos bases de données et applications.  
  
 **Precision Requirements**  
  
 Le paramètre *computed_column_expression* doit être précis. Un paramètre *computed_column_expression* est précis quand une ou plusieurs des conditions suivantes sont remplies :  
  
-   Il ne s’agit pas d’une expression des types de données **float** ou **real** .  
-   Il n’utilise pas un type de données **float** ou **real** dans sa définition. Par exemple, dans l’instruction suivante, la colonne `y` est de type **int** et déterministe, mais pas précise.  
  
    ```sql  
    CREATE TABLE t2 (a int, b int, c int, x float,   
       y AS CASE x   
             WHEN 0 THEN a   
             WHEN 1 THEN b   
             ELSE c   
          END);  
    ```  
  
> [!NOTE]  
> Toute expression **float** ou **real** est considérée comme non précise et ne peut pas être une clé d’index. Autrement dit, une expression **float** ou **real** peut être utilisée dans une vue indexée mais pas en tant que clé. Ce point s'applique également aux colonnes calculées. Toute fonction, expression ou fonction définie par l’utilisateur est considérée imprécise si elle contient des expressions **float** ou **real** , même logiques (comparaisons).  
  
La propriété **IsPrecise** de la fonction COLUMNPROPERTY indique si un paramètre *computed_column_expression* est précis ou non.  
  
**Data Type Requirements**  
  
-   Le paramètre *computed_column_expression* défini pour la colonne calculée ne peut pas correspondre aux types de données **text**, **ntext**ou **image** .  
-   Les colonnes calculées dérivées des types de données **image**, **ntext**, **text**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** et **xml** peuvent être indexées tant que le type de données de lacolonne calculée est autorisé en tant que colonne clé d’index.  
-   Les colonnes calculées dérivées des types de données **image**, **ntext**et **text** peuvent être des colonnes (incluses) non-clés dans un index non-cluster tant que le type de données utilisé dans la colonne calculée lui permet d’être une colonne d’index non-clés.  
  
**SET Option Requirements**  
  
-   L'option de niveau de connexion ANSI_NULLS doit être activée (ON) lors de l'exécution de l'instruction CREATE TABLE ou ALTER TABLE qui définit la colonne calculée. La fonction [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md) indique si l’option est activée par le biais de la propriété **IsAnsiNullsOn** .  
-   La connexion sur laquelle l'index est créé, ainsi que toutes les connexions tentant d'exécuter des instructions INSERT, UPDATE ou DELETE appelées à modifier des valeurs de l'index, doivent comporter six options SET activées (ON) et une désactivée (OFF). L'optimiseur ignore un index sur une colonne calculée dès qu'une instruction SELECT est exécutée par une connexion qui ne respecte pas ces paramètres d'options.  
  
    -   L'option NUMERIC_ROUNDABORT doit être désactivée (OFF) et les options suivantes doivent être activées (ON) :  
    -   ANSI_NULLS  
    -   ANSI_PADDING  
    -   ANSI_WARNINGS  
    -   ARITHABORT  
    -   CONCAT_NULL_YIELDS_NULL  
    -   QUOTED_IDENTIFIER  
  
> [!NOTE]
> L'affectation de la valeur ON à ANSI_WARNINGS affecte de manière implicite la valeur ON à ARITHABORT, lorsque le niveau de compatibilité de la base de données est d'au moins 90.  
  
## <a name="BKMK_persisted"></a> Création d’index sur des colonnes calculées persistantes  
Vous pouvez créer un index sur une colonne calculée définie par une expression déterministe mais non précise si la colonne est marquée comme PERSISTED dans l'instruction CREATE TABLE ou ALTER TABLE. Cela signifie que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke les valeurs calculées dans la table et qu'il les met à jour lorsque les autres colonnes dont dépendent les colonnes calculées sont mises à jour. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise ces valeurs persistantes pour créer un index sur la colonne et lorsqu'une requête fait référence à l'index. Cette option vous permet de créer un index sur une colonne calculée lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas prouver avec certitude qu'une fonction qui retourne des expressions de colonnes calculées, en particulier une fonction CLR créée dans [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], est à la fois déterministe et précise.  
  
## <a name="related-content"></a>Contenu associé  
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)    
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
  
  
