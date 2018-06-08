---
title: Agréger des fonctions, la fonction de calcul et le mot clé NEW | Documents Microsoft
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
- data shaping [ADO], functions
- CALC function [ADO]
- NEW keyword [ADO]
- aggregate functions [ADO]
ms.assetid: 0590b466-2a36-49a2-868e-028ef5e49394
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1e3af3e37caacae09f4ee57bc251f7ebcabfb04e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="aggregate-functions-the-calc-function-and-the-new-keyword"></a>Fonctions d’agrégation, la fonction de calcul et le mot clé NEW
Mise en forme des données prend en charge les fonctions suivantes. Le nom attribué au chapitre contenant la colonne à traiter est le *alias-chapitre*.  
  
 Un alias-chapitre peut être complet, constitué de chaque nom de colonne de chapitre menant au chapitre contenant le *-nom de la colonne,* séparés par des points. Par exemple, si le chapitre parent, chap1, contient un chapitre enfant, chap2, qui possède une colonne de montant, amt, puis le nom qualifié serait Chap1.Chap2.mnt.  
  
|Fonctions d'agrégation| Description|  
|-------------------------|-----------------|  
|SUM (*alias-chapitre*. *nom de la colonne*)|Calcule la somme de toutes les valeurs dans la colonne spécifiée.|  
|AVG (*alias-chapitre*. *nom de la colonne*)|Calcule la moyenne de toutes les valeurs dans la colonne spécifiée.|  
|MAX (*alias-chapitre*. *nom de la colonne*)|Calcule la valeur maximale dans la colonne spécifiée.|  
|MIN (*alias-chapitre*. *nom de la colonne*)|Calcule la valeur minimale de la colonne spécifiée.|  
|NOMBRE (*alias-chapitre*[. *nom de la colonne*])|Compte le nombre de lignes dans l’alias spécifié. Si une colonne est spécifiée, seules les lignes pour lesquelles cette colonne n’est pas nulle sont inclus dans le nombre.|  
|STDEV (*alias-chapitre*. *nom de la colonne*)|Calcule l’écart type de la colonne spécifiée.|  
|N’importe quel (*alias-chapitre*. *nom de la colonne*)|Valeur de la colonne spécifiée. UNE a une valeur prévisible uniquement lorsque la valeur de la colonne est la même pour toutes les lignes dans le chapitre.<br /><br /> **Remarque** si la colonne ne contient pas la même valeur pour toutes les lignes dans le chapitre, la commande SHAPE retourne arbitrairement l’une des valeurs pour la valeur de la fonction de n’importe quel.|  
  
|Expression calculée| Description|  
|---------------------------|-----------------|  
|CALCUL (*expression*)|Calcule une expression arbitraire, mais uniquement sur la ligne de la **Recordset** contenant la fonction de calcul. Toute expression utilisant ces [Visual Basic pour Applications (VBA) fonctions](../../../ado/guide/data/visual-basic-for-applications-functions.md) est autorisée.|  
  
|NOUVEAU mot clé| Description|  
|-----------------|-----------------|  
|NOUVELLE *type de champ* [(*largeur* &#124; *échelle* &#124; *précision* &#124; *erreur*[, *échelle* &#124; *erreur*])]|Ajoute une colonne vide du type spécifié à la **Recordset**.|  
  
 Le *type de champ* passé avec le mot clé NEW peut être un des types de données suivants.  
  
|Types de données OLE DB|Type (s) données ADO|  
|-----------------------|-----------------------------------|  
|DBTYPE_BSTR|adBSTR|  
|DBTYPE_BOOL|adBoolean|  
|DBTYPE_DECIMAL|adDecimal|  
|DBTYPE_UI1|adUnsignedTinyInt|  
|DBTYPE_I1|adTinyInt|  
|DBTYPE_UI2|adUnsignedSmallInt|  
|DBTYPE_UI4|adUnsignedInt|  
|DBTYPE_I8|adBigInt|  
|DBTYPE_UI8|adUnsignedBigInt|  
|DBTYPE_GUID|adGuid|  
|DBTYPE_BYTES|adBinary, AdVarBinary, adLongVarBinary|  
|DBTYPE_STR|adChar, adVarChar, adLongVarChar|  
|DBTYPE_WSTR|adWChar, adVarWChar, adLongVarWChar|  
|DBTYPE_NUMERIC|adNumeric|  
|DBTYPE_DBDATE|adDBDate|  
|DBTYPE_DBTIME|adDBTime|  
|DBTYPE_DBTIMESTAMP|adDBTimeStamp|  
|DBTYPE_VARNUMERIC|adVarNumeric|  
|DBTYPE_FILETIME|adFileTime|  
|DBTYPE_ERROR|adError|  
  
 Lorsque le nouveau champ est de type decimal (OLE DB, DBTYPE_DECIMAL ou dans ADO, adDecimal), vous devez spécifier les valeurs de précision et l’échelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)   
 [Grammaire de mise en forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](../../../ado/guide/data/shape-commands-in-general.md)
