---
title: Grammaire de mise en forme formelle | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO], shape grammar
- data shaping [ADO], shape grammar
ms.assetid: ea691475-0f03-4abe-a785-b77e77712d1d
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d3658526e3b63069f3fce0dc1431804b5304070b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="formal-shape-grammar"></a>Grammaire de mise en forme formelle
Il s’agit de la grammaire formelle pour la création de n’importe quelle commande de la forme :  
  
-   Les termes grammaticaux obligatoires sont des chaînes de texte délimité par des crochets (« <> »).  
  
-   Termes facultatifs sont délimités par des crochets (« [] »).  
  
-   Alternatives sont indiquées par une barre verticale («&#124;»).  
  
-   Extensibles alternatives sont indiquées par les points de suspension («... »).  
  
-   *Alpha* indique une chaîne de caractères alphabétiques.  
  
-   *Chiffre* indique une chaîne de nombres.  
  
-   *Chiffres Unicode* indique une chaîne de chiffres unicode.  
  
 Toutes les autres conditions sont des littéraux.  
  
|Terme|Définition|  
|----------|----------------|  
|\<forme-commande >|FORME [\<table-exp > [[AS] \<alias >]] [\<forme action >]|  
|\<table-exp >|{\<texte de commande fournisseur >}&#124;<br /><br /> (\<forme-commande >)&#124;<br /><br /> TABLE \<quoted-name >&#124;<br /><br /> \<nom entre guillemets >|  
|\<forme action >|APPEND \<liste de champs d’un alias >&#124;<br /><br /> CALCUL \<liste de champs d’un alias > [BY \<liste de champs >]|  
|\<liste de champs d’un alias >|\<un alias-champ > [, \<champ alias... >]|  
|\<champ de l’alias >|\<champ-exp > [[AS] \<alias >]|  
|\<field-exp>|(\<relation-exp >)&#124;<br /><br /> \<exp calculée >&#124;<br /><br /> \<agrégat-exp >&#124;<br /><br /> \<nouveau-exp >|  
|< relation_exp >|\<table-exp > [[AS] \<alias >]<br /><br /> ASSOCIER \<relation-conditionnel-list >|  
|\<relation-cond-list>|\<condition de relation > [, \<conditionnel de relation >...]|  
|\<condition de relation >|\<nom du champ > TO \<enfant-ref >|  
|\<enfant-ref >|\<nom du champ >&#124;<br /><br /> PARAMÈTRE \<param-ref >|  
|\<Param-ref >|\<nombre >|  
|\<field-list>|\<nom du champ > [, \<-nom du champ >]|  
|\<aggregate-exp>|SUM (\<nom de champ qualifié >)&#124;<br /><br /> AVG (\<nom de champ qualifié >)&#124;<br /><br /> MIN (\<nom de champ qualifié >)&#124;<br /><br /> MAX (\<nom de champ qualifié >)&#124;<br /><br /> NOMBRE (\<alias qualifié > &#124; \<nom qualifié >)&#124;<br /><br /> STDEV (\<nom de champ qualifié >)&#124;<br /><br /> N’importe quel (\<nom de champ qualifié >)|  
|\<exp calculée >|CALCUL (\<expression >)|  
|\<nom de champ qualifié >|\<alias >. [\<alias >...] \<-nom du champ >|  
|\<alias >|\<nom entre guillemets >|  
|\<nom du champ >|\<Quoted-name > [[AS] \<alias >]|  
|\<nom entre guillemets >|«\<chaîne > »&#124;<br /><br /> '\<chaîne >'&#124;<br /><br /> [\<chaîne >]&#124;<br /><br /> \<Nom >|  
|\<nom qualifié >|alias [.alias...]|  
|\<Nom >|alpha [alpha &#124; chiffre &#124; _ &#124; # &#124; : &#124; ...]|  
|\<nombre >|chiffre [chiffres...]|  
|\<nouveau-exp >|NOUVELLE \<type de champ > [(\<nombre > [, \<nombre >])]|  
|\<field-type>|Un type de données OLE DB ou ADO.|  
|\<chaîne >|Unicode-char [unicode-char...]|  
|\<expression>|Une expression Visual Basic pour Applications dont les opérandes sont d’autres colonnes non calculées dans la même ligne.|  
  
## <a name="see-also"></a>Voir aussi  
 [L’accès aux lignes dans un jeu d’enregistrements hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [Vue d’ensemble de la mise en forme des données](../../../ado/guide/data/data-shaping-overview.md)   
 [Fournisseurs requis pour la mise en forme des données](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Clause APPEND de forme](../../../ado/guide/data/shape-append-clause.md)   
 [En général, les commandes de forme](../../../ado/guide/data/shape-commands-in-general.md)   
 [Clause COMPUTE de forme](../../../ado/guide/data/shape-compute-clause.md)   
 [Fonctions Visual Basic pour Applications](../../../ado/guide/data/visual-basic-for-applications-functions.md)
