---
title: En général les commandes Shape | Documents Microsoft
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
- shape commands [ADO]
- data shaping [ADO], shape commands
ms.assetid: 1fac7831-a187-4b15-9b43-aad380c5556c
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6af36fbf7a3b60067c94f0d21aa6e7514df1a098
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="shape-commands-in-general"></a>En général, les commandes de forme
Mise en forme des données définit les colonnes d’une forme **Recordset**, les relations entre les entités représentées par les colonnes et la façon dont le **Recordset** remplie avec des données.  
  
 Une forme **Recordset** peut se composer des types de colonnes suivants.  
  
|Type de colonne| Description|  
|-----------------|-----------------|  
|données|Des champs une **Recordset** retournées par une commande de requête à un fournisseur de données, de table ou précédemment mise en forme **Recordset**.|  
|chapitre|Une référence à un autre **Recordset**, appelé un *chapitre*. Les colonnes de chapitres permettent de définir un *parent-enfant* relation où la *parent* est la **Recordset** qui contient la colonne de chapitre et le *enfant* est la **Recordset** représenté par le chapitre.|  
|agrégat|La valeur de la colonne est obtenue par l’exécution une *fonction d’agrégation* sur toutes les lignes ou une colonne de toutes les lignes d’un enfant **Recordset**. (Voir les fonctions d’agrégation dans la rubrique suivante, [les fonctions d’agrégation, la fonction de calcul et le mot clé NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md).)|  
|Expression calculée|La valeur de la colonne est dérivée en calculant une expression Visual Basic pour Applications sur les colonnes de la même ligne de la **Recordset**. L’expression est l’argument de la fonction de calcul. (Consultez Expression calculée dans la rubrique suivante, [les fonctions d’agrégation, la fonction de calcul et le mot clé NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md) et dans [Visual Basic pour Applications Functions](../../../ado/guide/data/visual-basic-for-applications-functions.md).)|  
|new|Champs vides et fabriqués, qui peuvent être remplis avec des données à une date ultérieure. La colonne est définie avec le mot clé NEW. (Voir le nouveau mot clé dans la rubrique suivante, [les fonctions d’agrégation, la fonction de calcul et le mot clé NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md).)|  
  
 Une commande shape peut contenir une clause qui spécifie une commande de requête à un fournisseur de données sous-jacent qui retournera un **Recordset** objet. Syntaxe de la requête varie selon les exigences du fournisseur de données sous-jacent. Il s’agit généralement de SQL, bien que ADO ne nécessite pas l’utilisation de n’importe quel langage de requête particulier.  
  
 Commandes Shape peuvent être émises par **Recordset** objets ou en définissant le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) propriété de la [commande](../../../ado/reference/ado-api/command-object-ado.md) objet, puis en appelant le [Execute ](../../../ado/reference/ado-api/execute-method-ado-command.md) (méthode).  
  
 Vous pouvez utiliser une clause JOIN SQL pour associer les deux tables ; Toutefois, une liste hiérarchique **Recordset** peut représenter les informations de manière plus efficace. Chaque ligne d’un **Recordset** créé par une jointure des informations se répète une des tables de façon redondante. Une liste hiérarchique **Recordset** n'a qu’un seul parent **Recordset** pour chacun de plusieurs enfants **Recordset** objets.  
  
 Commandes de forme peuvent être imbriquées. Autrement dit, le *parent-command* ou *commande enfant* peut se trouver dans une autre commande shape.  
  
 Le fournisseur shape retourne toujours un curseur client, même lorsque l’utilisateur spécifie un emplacement de curseur **adUseServer**.  
  
 Vous pouvez accéder à la **Recordset** composants de la forme **Recordset** par programme ou via un contrôle visuel approprié.  
  
 Microsoft fournit un outil visuel qui génère des commandes de forme (consultez la [données environnement concepteur](http://go.microsoft.com/fwlink/?LinkId=5689) dans la documentation de Visual Basic 6) et un autre affichage des curseurs hiérarchiques (voir « à l’aide du Microsoft hiérarchique Contrôle FlexGrid » dans la documentation de Visual Basic 6).  
  
 Pour plus d’informations sur la navigation dans une liste hiérarchique **Recordset**, consultez [accès aux lignes d’un objet Recordset hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 Pour obtenir des informations détaillées sur les commandes de forme correcte, consultez [formels forme grammaire](../../../ado/guide/data/formal-shape-grammar.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Fonctions d’agrégation, fonction CALC et mot clé NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)  
  
-   [Émission de commandes vers le fournisseur de données sous-jacent](../../../ado/guide/data/issuing-commands-to-the-underlying-data-provider.md)
