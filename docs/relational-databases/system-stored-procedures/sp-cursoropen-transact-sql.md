---
title: sp_cursoropen (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_cursoropen
- sp_cursoropen_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoropen
ms.assetid: 16462ede-4393-4293-a598-ca88c48ca70b
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c6dae6b21a86c6cc68ab241328c5c190580888c6
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcursoropen-transact-sql"></a>sp_cursoropen (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ouvre un curseur. sp_cursoropen définit l’instruction SQL associée du curseur et les options de curseur, puis remplit le curseur. sp_cursoropen équivaut à la combinaison de la [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions DECLARE_CURSOR et OPEN. Cette procédure est appelée en spécifiant ID = 2 dans un paquet TDS (Tabular Data Stream).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursoropen cursor OUTPUT, stmt  
    [, scrollopt[ OUTPUT ] [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,boundparam][,...n]]] ]]  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Identificateur de curseur généré par SQL Server. *curseur* est un *gérer* valeur qui doit être fourni pour toutes les procédures suivantes impliquant le curseur, par exemple sp_cursorfetch. *curseur* est un paramètre obligatoire avec une **int** valeur de retour.  
  
 *curseur* permet à plusieurs curseurs d’être actives sur une connexion de base de données unique.  
  
 *stmt*  
 Paramètre obligatoire qui définit le jeu de résultats de curseur. Toute chaîne de requête valide (syntaxe et liaison) de n’importe quel type de chaîne (indépendamment des données Unicode, taille, etc.) peut servir valide *stmt* type valeur.  
  
 *paramètres scrollopt*  
 Option de défilement. *paramètres scrollopt* est un paramètre optionnel qui requiert l’une des opérations suivantes **int** valeurs d’entrée.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 En raison de la possibilité que la valeur demandée n’est pas appropriée pour le curseur défini par *stmt*, ce paramètre sert à la fois d’entrée et sortie. Dans de tels cas, SQL Server affecte une valeur appropriée.  
  
 *ccopt*  
 Option de contrôle en matière d'accès concurrentiel. *ccopt* est un paramètre optionnel qui requiert l’une des opérations suivantes **int** valeurs d’entrée.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (précédemment appelé LOCKCC)|  
|0x0004|**OPTIMISTE** (précédemment appelé optcc)|  
|0x0008|OPTIMISTIC (précédemment appelé OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Comme avec *scrollopt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent substituer demandé *ccopt* valeurs.  
  
 *nombre de lignes*  
 Nombre de lignes de tampon d'extraction à utiliser avec AUTO_FETCH. La valeur par défaut est de 20 lignes. *nombre de lignes* se comporte différemment lorsque affecté comme une valeur d’entrée ou une valeur de retour.  
  
|Comme une valeur d'entrée|Comme une valeur de retour|  
|--------------------|---------------------|  
|Lorsque le AUTO_FETCH *scrollopt* valeur est spécifiée *rowcount* représente le nombre de lignes à placer dans le tampon d’extraction.<br /><br /> Remarque : > 0 est une valeur valide quand AUTO_FETCH est spécifiée, mais il est ignoré.|Représente le nombre de lignes dans le résultat défini, sauf quand le *scrollopt* la valeur AUTO_FETCH est spécifiée.|  
  
-  
  
 *boundparam*  
 Indique l'utilisation de paramètres supplémentaires. *boundparam* est un paramètre optionnel qui doit être spécifié si le *scrollopt* PARAMETERIZED_STMT a la valeur ON.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Si aucune erreur n'est générée, sp_cursoropen retourne l'une des valeurs suivantes.  
  
 0  
 La procédure est exécutée avec succès.  
  
 0x0001  
 Une erreur s'est produite pendant l'exécution (une erreur mineure, pas assez grave pour générer une erreur dans l'opération).  
  
 0x0002  
 Une opération asynchrone est en cours.  
  
 0x0002  
 Une opération FETCH est en cours.  
  
 Un  
 Ce curseur a été libéré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et n'est pas disponible.  
  
 Lorsqu'une erreur est générée, les valeurs de retour peuvent être incohérentes et l'exactitude ne peut pas être garantie.  
  
 Lorsque le *rowcount* paramètre est spécifié comme valeur de retour, le jeu de résultats suivant se produit.  
  
 -1  
 Retourné si le nombre de lignes est inconnu ou non applicable.  
  
 -n  
 Retourné lorsqu'un remplissage asynchrone est appliqué. Représente le nombre de lignes qui ont été placés dans l’extraction de la mémoire tampon quand le *scrollopt* la valeur AUTO_FETCH est spécifiée.  
  
 Si l'appel de procédure distante est utilisé, les valeurs de retour sont les suivantes.  
  
 0  
 La procédure est réussie.  
  
 1  
 La procédure a échoué.  
  
 2  
 Un curseur de jeu de clés est généré de façon asynchrone.  
  
 16  
 Un curseur avance rapide a été automatiquement fermé.  
  
> [!NOTE]  
>  Si la procédure sp_cursoropen a été correctement exécutée, les paramètres de retour RPC et un jeu de résultats avec des informations de format de colonne TDS (messages 0xa0 & 0xa1) sont envoyés. En cas d'échec, un ou plusieurs messages d'erreur TDS sont envoyés. Dans les deux cas, aucune donnée de ligne ne s’affichera et le *fait* nombre de messages sera égal à zéro. Si vous utilisez une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à 7.0, 0xa0, 0xa1 (standard pour les instructions SELECT) sont retournés avec les flux de jetons 0xa5 et 0xa4. Si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, 0x81 est retourné (standard pour les instructions SELECT) avec les flux de jetons 0xa5 et 0xa4.  
  
## <a name="remarks"></a>Notes  
  
## <a name="stmt-parameter"></a>Paramètre stmt  
 Si *stmt* Spécifie l’exécution d’une procédure stockée, les paramètres d’entrée peuvent être définis en tant que constantes dans le cadre de la *stmt* de chaîne ou spécifié en tant que *boundparam* arguments. Les variables déclarées peuvent être passées comme paramètres liés de cette façon.  
  
 Le contenu autorisé du *stmt* paramètre varient en fonction du ou non le *ccopt* ALLOW_DIRECT retournent la valeur a été liée par OR au reste de la *ccopt* des valeurs, par exemple :  
  
-   Si ALLOW_DIRECT n’est pas spécifié, soit un [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT ou EXECUTE instruction appelant une procédure stockée contenant une instruction SELECT unique doit être utilisée. En outre, l’instruction SELECT doit obtenir la qualification de curseur ; Autrement dit, il ne peut pas contenir les mots clés SELECT INTO ou FOR BROWSE.  
  
-   Si ALLOW_DIRECT est spécifiée, cela peut générer une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], notamment celles qui, à leur tour, exécutent d'autres procédures stockées avec plusieurs instructions. Les instructions autres que SELECT ou toute instruction SELECT qui contient les mots clés SELECT INTO ou FOR BROWSE seront simplement exécutées et n'aboutiront pas à la création d'un curseur. Cela s'applique aussi à toute instruction SELECT incluse dans un lot de plusieurs instructions. Dans les cas où une instruction SELECT contient des clauses qui concernent uniquement les curseurs, ces clauses sont ignorées. Par exemple, lorsque la valeur de *ccopt* est 0 x 2002, il s’agit d’une demande pour :  
  
    -   un curseur avec des arrêts de défilement, si une seule instruction SELECT obtient la qualification de curseur, ou  
  
    -   une exécution d'instruction directe s'il existe plusieurs instructions, une instruction autre que SELECT unique ou une instruction SELECT qui n'obtient pas la qualification de curseur.  
  
## <a name="scrollopt-parameter"></a>Paramètre scrollopt  
 Les cinq premiers *scrollopt* valeurs (keyset, DYNAMIC, FORWARD_ONLY, STATIC et FAST_FORWARD) s’excluent mutuellement.  
  
 PARAMETERIZED_STMT et CHECK_ACCEPTED_TYPES peuvent être liés par OR à chacune des cinq premières valeurs.  
  
 AUTO_FETCH et AUTO_CLOSE peuvent être liés par OR à FAST_FORWARD.  
  
 Si check_accepted_types a la valeur ON, au moins une des cinq dernières *scrollopt* valeurs (KEYSET_ACCEPTABLE`,` DYNAMIC_ACCEPTABLE, FORWARD_ONLY_ACCEPTABLE, STATIC_ACCEPTABLE ou FAST_FORWARD_ACCEPTABLE) doit également avoir la valeur ON.  
  
 Les curseurs STATIC sont toujours ouverts comme READ_ONLY. Cela signifie que la table sous-jacente ne peut pas être mise à jour via ce curseur.  
  
## <a name="ccopt-parameter"></a>Paramètre ccopt  
 Les quatre premières *ccopt* valeurs (READ_ONLY, SCROLL_LOCKS et les deux valeurs OPTIMISTIC) s’excluent mutuellement.  
  
> [!NOTE]  
>  En choisissant une des quatre premières *ccopt* valeurs détermine si le curseur est en lecture seule, ou si les méthodes de verrouillage ou optimistes sont utilisées pour empêcher les mises à jour perdues. Si un *ccopt* valeur n’est pas spécifiée, la valeur par défaut est OPTIMISTIC.  
  
 ALLOW_DIRECT et CHECK_ACCEPTED_TYPES peuvent être liés par OR à chacune des quatre premières valeurs.  
  
 UPDT_IN_PLACE peut être lié par OR à READ_ONLY, SCROLL_LOCKS, ou l'une des valeurs OPTIMISTIC.  
  
 Si check_accepted_types a la valeur ON, au moins une des quatre dernières *ccopt* valeurs (READ_ONLY_ACCEPTABLE, SCROLL_LOCKS_ACCEPTABLE et une des valeurs OPTIMISTIC_ACCEPTABLE) doivent également avoir la valeur ON.  
  
 Les fonctions UPDATE et DELETE positionnées peuvent être effectuées uniquement dans le tampon d’extraction et uniquement si la *ccopt* valeur est égale à SCROLL_LOCKS ou OPTIMISTIC. Si SCROLL_LOCKS est la valeur spécifiée, la réussite de l'opération est garantie. Si OPTIMISTIC est la valeur spécifiée, l'opération échoue si la ligne a changé depuis la dernière extraction.  
  
 La raison de cet échec est que, lorsqu'OPTIMISTIC est la valeur spécifiée, une fonction de contrôle d'accès concurrentiel optimiste est exécutée en comparant les valeurs d'horodatage ou de somme de contrôle, comme déterminé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l'une de ces lignes ne correspond pas, l'opération échoue.  
  
 La spécification d'UPDT_IN_PLACE comme valeur de retour détermine les résultats suivants :  
  
-   S'il n'est pas défini lors de l'exécution d'une mise à jour positionnée sur une table avec un index unique, le curseur supprime la ligne de sa table de travail et l'insère à la fin de l'une des colonnes clés utilisées par le curseur, en modifiant ainsi ces colonnes.  
  
-   S'il a la valeur ON, le curseur met simplement à jour les colonnes clés dans la ligne d'origine de la table de travail.  
  
## <a name="boundparam-parameter"></a>Paramètre bound_param  
 Le nom du paramètre doit être *paramdef* lorsque PARAMETERIZED_STMT est spécifié, d’après le message d’erreur dans le code. Lorsque PARAMETERIZED_STMT n'est pas spécifié, aucun nom n'est spécifié dans le message d'erreur.  
  
## <a name="rpc-considerations"></a>Éléments RPC à prendre en considération  
 Il est possible d'affecter à l'indicateur d'entrée RPC RETURN_METADATA la valeur 0x0001 pour demander que les métadonnées relatives à la liste de sélection du curseur soient retournées dans le flux TDS.  
  
## <a name="examples"></a>Exemples  
  
### <a name="boundparam-parameter"></a>Paramètre bound_param  
 Tous les paramètres après le cinquième sont passés au plan d'instruction comme paramètres d'entrée. Le premier paramètre de ce type doit être une chaîne sous la forme suivante :  
  
 *{type de données de nom de variable locale} [,... n].*  
  
 Les paramètres suivants sont utilisés pour passer les valeurs à remplacer pour les *nom de variable locale* dans l’instruction.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
