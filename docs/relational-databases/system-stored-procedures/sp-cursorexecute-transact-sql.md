---
title: sp_cursorexecute (Transact-SQL) | Documents Microsoft
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
- sp_cursorexecute
- sp_cursorexecute_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_execute
ms.assetid: 6a204229-0a53-4617-a57e-93d4afbb71ac
caps.latest.revision: 7
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: cae17034cdcc2d048e539961bf07971acb3b3410
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcursorexecute-transact-sql"></a>sp_cursorexecute (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée et remplit un curseur basé sur le plan d'exécution créé par sp_cursorprepare. Cette procédure, couplée avec sp_cursorprepare, a la même fonction que sp_cursoropen, mais est fractionnée en deux phases. sp_cursorexecute est appelée en spécifiant ID = 4 dans un paquet de stream (TDS) de données tabulaires.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorexecute prepared_handle, cursor  
    [ , scrollopt[ OUTPUT ]  
    [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,bound param][,...n]]]]]  
```  
  
## <a name="arguments"></a>Arguments  
 *prepared_handle*  
 L’instruction préparée *gérer* valeur retournée par sp_cursorprepare. *prepared_handle* est un paramètre obligatoire qui demande un **int** valeur d’entrée.  
  
 *cursor*  
 Est l’identificateur de curseur généré par SQL Server. *curseur* est un paramètre obligatoire qui doit être fourni pour toutes les procédures suivantes qui agissent sur le curseur, par exemple sp_cursorfetch  
  
 *paramètres scrollopt*  
 Option de défilement. *paramètres scrollopt* est un paramètre optionnel qui requiert un **int** valeur d’entrée. Le sp_cursorexecute*scrollopt* paramètre a les mêmes options de valeur que celles pour sp_cursoropen.  
  
> [!NOTE]  
>  La valeur PARAMETERIZED_STMT n'est pas prise en charge.  
  
> [!IMPORTANT]  
>  Si un *scrollopt* valeur n’est pas spécifiée, la valeur par défaut est KEYSET indépendamment de *scrollopt* valeur spécifiée dans sp_cursorprepare.  
  
 *ccopt*  
 Option de contrôle de devise. *ccopt* est un paramètre optionnel qui requiert un **int** valeur d’entrée. Le sp_cursorexecute*ccopt* paramètre a les mêmes options de valeur que celles pour sp_cursoropen.  
  
> [!IMPORTANT]  
>  Si un *ccopt* valeur n’est pas spécifiée, la valeur par défaut est OPTIMISTIC indépendamment de *ccopt* valeur spécifiée dans sp_cursorprepare.  
  
 *nombre de lignes*  
 Paramètre optionnel qui indique le nombre de lignes de tampon d'extraction à utiliser avec AUTO_FETCH. La valeur par défaut est de 20 lignes. *nombre de lignes* se comporte différemment lorsque affecté comme une valeur d’entrée ou une valeur de retour.  
  
|Comme une valeur d'entrée|Comme une valeur de retour|  
|--------------------|---------------------|  
|Quand AUTO_FETCH est spécifiée avec les curseurs FAST_FORWARD *rowcount* représente le nombre de lignes à placer dans le tampon d’extraction.|Représente le nombre de lignes dans le jeu de résultats. Lorsque le *scrollopt* AUTO_FETCH valeur est spécifiée, *rowcount* retourne le nombre de lignes qui ont été extraites dans le tampon d’extraction.|  
  
 *bound_param*  
 Indique l'utilisation facultative de paramètres supplémentaires.  
  
> [!NOTE]  
>  Tous les paramètres après le cinquième sont passés au plan d'instruction comme paramètres d'entrée.  
  
## <a name="code-return-value"></a>Valeur de retour de code  
 *nombre de lignes* peut renvoyer les valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|-1|Nombre de lignes inconnues.|  
|-n|Un remplissage asynchrone est appliqué.|  
  
## <a name="remarks"></a>Notes  
  
## <a name="scrollopt-and-ccopt-parameters"></a>Paramètres scrollopt et ccopt  
 *paramètres scrollopt* et *ccopt* sont utiles lorsque les plans mis en cache sont anticipés pour le cache du serveur, ce qui signifie que que le descripteur préparé qui identifie l’instruction doit être recompilé. Le *scrollopt* et *ccopt* les valeurs de paramètre doivent correspondre à celles envoyées dans la demande d’origine à sp_cursorprepare.  
  
> [!NOTE]  
>  PARAMETERIZED_STMT ne doit pas être attribuée à *scrollopt*.  
  
 L'incapacité à fournir des valeurs correspondantes entraînera la recompilation des plans, annulant ainsi les opérations de préparation et d'exécution.  
  
## <a name="rpc-and-tds-considerations"></a>Éléments RPC et TDS à prendre en considération  
 Il est possible d'affecter à l'indicateur d'entrée RPC RETURN_METADATA la valeur 1 pour demander que les métadonnées relatives à la liste de sélection du curseur soient retournées dans le flux TDS.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
