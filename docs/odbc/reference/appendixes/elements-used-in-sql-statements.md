---
title: Éléments utilisés dans les instructions SQL | Documents Microsoft
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
- SQL statements [ODBC], elements supported
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 85777525-1555-4731-8309-63a464c6b43a
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 77b7fdcfbc91e63451615b20c7eedf2748f687c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="elements-used-in-sql-statements"></a>Éléments utilisés dans les instructions SQL
Les éléments suivants sont utilisés dans les instructions SQL répertoriées précédemment.  
  
## <a name="element"></a>Élément  
 *identificateur de table de base de* :: = *nom défini par l’utilisateur*  
  
 *nom de table de base de* :: = *identificateur de table de base*  
  
 *booléen-facteur* :: = [NOT] *primaire booléen*  
  
 *principal de la valeur booléenne* :: = comparaison *-prédicat* &#124; ( *condition de recherche* )  
  
 *valeur booléenne à long terme* :: = *facteur de valeur booléenne* [AND *à long terme boolean*]  
  
 *littéral de chaîne de caractères* :: = '' {*caractère*}... ». (*caractère* est n’importe quel caractère dans le jeu de caractères de la source de données/pilote. Pour inclure un caractère de guillemet littéral (") dans un littéral de chaîne de caractères, utilisez deux guillemets littéraux ['' '].)  
  
 *identificateur de la colonne* :: = *nom défini par l’utilisateur*  
  
 *nom de la colonne* :: = [*-nom de la table*.] *identificateur de colonne*  
  
 *opérateur de comparaison* :: = < &#124; > &#124; \<= &#124; > = &#124; = &#124; <>  
  
 *prédicat de comparaison* :: = *expression* expression de l’opérateur de comparaison  
  
 *type de données* :: = *type de chaîne de caractères* (*type de chaîne de caractères* est n’importe quel type de données pour laquelle la colonne « « DATA_TYPE » » dans le jeu de résultats retournée par SQLGetTypeInfo est soit SQL_CHAR ou SQL_VARCHAR.)  
  
 *chiffre* :: = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9  
  
 *paramètre dynamique* :: = ?  
  
 *expression* :: = terme &#124; expression {+&#124;–} terme  
  
 *facteur* :: = [*+*&#124;*–*]*principal*  
  
 *valeur à insérer* :: =  
  
 *paramètre dynamique*  
  
 &#124;*littéral*  
  
 &AMP;#124;VALEUR NULL  
  
 &AMP;#124;UTILISATEUR  
  
 *lettre* :: = *minuscule scénarios lettres &#124; -supérieur-lettre*  
  
 *littéral* :: = *littéral de chaîne de caractères*  
  
 *lettre de scénarios minuscule* :: = un &#124; b &#124; c &#124; d &#124; e &#124; f &#124; g &#124; h &#124; i &#124; j &#124; k &#124; l &#124; m &#124; n &#124; o &#124; p &#124; q &#124; r &#124; s &#124; t &#124; u &#124; v &#124; w &#124; x &#124; y &#124; z  
  
 *clause Order by* :: = ORDER BY *spécification de tri* [, *spécification de tri*]...  
  
 *principal* :: = *-nom de la colonne*  
  
 &#124;*-paramètre dynamique*  
  
 &#124;*littéral*  
  
 &#124;( *expression* )  
  
 *condition de recherche* :: = *à long terme boolean* [ou *condition de recherche*]  
  
 *liste de sélection* :: = \* &#124; *sous-liste de select* [, *sous-liste de select*]...  (*liste de sélection* ne peut pas contenir de paramètres.)  
  
 *Sélectionnez-sous-liste* :: = *expression*  
  
 *spécification de tri* :: = {*entier non signé &#124; -nom de la colonne*} [*ASC &#124; DESC*]  
  
 *identificateur de la table* :: = *nom défini par l’utilisateur*  
  
 *nom de la table* :: = *identificateur de table*  
  
 *référence de table* :: = *-nom de la table*  
  
 *liste de références de table* :: = *référence de table* [,*référence de table*]...  
  
 *terme* :: = *facteur* &#124; *terme* {\*&#124;*/*} *facteur*  
  
 *entier non signé* :: = {*chiffre*}  
  
 *lettre de cas supérieur* :: = *A &#124; B &#124; C &#124; D &#124; E &#124; F &#124; G &#124; H &#124; I &#124; J &#124; K &#124; L &#124; M &#124; N &#124; O &#124; P &#124;Q &#124; R &#124; S &#124; T &#124; U &#124; V &#124; W &#124; X &#124; Y &#124; Z*  
  
 *nom défini par l’utilisateur* :: = *lettre*[*chiffre* &#124; *lettre* &#124; *_*]...
