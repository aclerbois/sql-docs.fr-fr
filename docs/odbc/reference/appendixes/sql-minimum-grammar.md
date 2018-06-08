---
title: Grammaire minimale SQL | Documents Microsoft
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
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 4f36d785-104f-4fec-93be-f201203bc7c7
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a3e31f53abf8d8788f719adc9e00e180ca7aa96f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-minimum-grammar"></a>Grammaire minimale SQL
Cette section décrit la syntaxe SQL minimale un pilote ODBC doit prendre en charge. La syntaxe décrite dans cette section est un sous-ensemble de la syntaxe au niveau d’entrée de SQL-92.  
  
 Une application peut utiliser une de la syntaxe de cette section et s’assurée que les pilotes compatibles ODBC prendra en charge cette syntaxe. Pour déterminer si les fonctionnalités supplémentaires de SQL-92 dans cette section sont pris en charge, l’application doit appeler **SQLGetInfo** avec le type d’informations SQL_SQL_CONFORMANCE. Même si le pilote n’est pas conforme à n’importe quel niveau de conformité SQL-92, une application peut toujours utiliser la syntaxe décrite dans cette section. Si un pilote est conforme à un niveau de SQL-92, en revanche, il prend en charge toute la syntaxe incluse dans ce niveau. Cela inclut la syntaxe de cette section, car la grammaire minimale décrite ici est un sous-ensemble de pur de la plus faible au niveau de conformité SQL-92. Une fois que l’application sache que le niveau de SQL-92 pris en charge, il peut déterminer si une fonctionnalité de niveau supérieur est pris en charge (le cas échéant) en appelant **SQLGetInfo** avec le type des informations correspondant à cette fonctionnalité.  
  
 Les pilotes fonctionnent uniquement avec les sources de données en lecture seule ne peuvent pas en charge les parties de la grammaire incluses dans cette section qui traitent de la modification de données. Une application peut déterminer si une source de données est en lecture seule en appelant **SQLGetInfo** avec le type d’informations SQL_DATA_SOURCE_READ_ONLY.  
  
## <a name="statement"></a>.  
 *instruction de table créer* :: =  
  
 CREATE TABLE *nom de table de base*  
  
 (*identificateur de la colonne type de données* [*, identificateur de la colonne type de données*]...)  
  
> [!IMPORTANT]  
>  Comme un *type de données* dans un *-table-instruction create*, les applications doivent utiliser un type de données à partir de la colonne TYPE_NAME du jeu de résultats retourné par **SQLGetTypeInfo**.  
  
 *recherche-instruction de suppression* :: =  
  
 DELETE FROM *-nom de la table* [où *condition de recherche*]  
  
 *instruction DROP-table* :: =  
  
 DROP TABLE *nom de table de base*  
  
 *instruction INSERT* :: =  
  
 INSERT INTO *-nom de la table* [( *identificateur de la colonne* [, *identificateur de la colonne*]...)]      VALEURS (*-valeur à insérer*[, *-valeur à insérer*]...)  
  
 *instruction SELECT* :: =  
  
 Sélectionnez [tous les &#124; DISTINCT] *liste de sélection*  
  
 À partir de *liste de références de table*  
  
 [Où *condition de recherche*]  
  
 [*clause order by*]  
  
 *instruction* :: = *-table-instruction create*  
  
 &#124;*recherche-instruction de suppression*  
  
 &#124;*instruction drop-table*  
  
 &#124;*-instruction insert*  
  
 &#124;*instruction select*  
  
 &#124;*recherche-instruction de mise à jour*  
  
 *recherche-instruction de mise à jour*  
  
 Mise à jour *-nom de la table*  
  
 Définissez *identificateur de la colonne* = {*expression* &#124; NULL}  
  
 [, *identificateur de la colonne* = {*expression* &#124; NULL}]...  
  
 [Où *condition de recherche*]  
  
 Cette section contient les rubriques suivantes.  
  
-   [Éléments utilisés dans les instructions SQL](../../../odbc/reference/appendixes/elements-used-in-sql-statements.md)  
  
-   [Prise en charge des types de données](../../../odbc/reference/appendixes/data-type-support.md)  
  
-   [Types de données de paramètre](../../../odbc/reference/appendixes/parameter-data-types.md)  
  
-   [Marqueurs de paramètres](../../../odbc/reference/appendixes/parameter-markers.md)
