---
title: sp_cursor (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_cursor_TSQL
- sp_cursor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor
ms.assetid: 41ade0ca-5f11-469d-bd4d-c8302ccd93b3
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 36e188370f32702544ccef4b18d0398e5c8fddfd
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcursor-transact-sql"></a>sp_cursor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Demande des mises à jour positionnées. Cette procédure effectue des opérations sur une ou plusieurs lignes dans le tampon d'extraction d'un curseur. sp_cursor est appelée en spécifiant ID = 1 dans un paquet de stream (TDS) de données tabulaires.  
  
||  
|-|  
|**S’applique aux**: SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).|  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursor  cursor, optype, rownum, table  
    [ , value[...n]]]  
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Handle de curseur. *curseur* est un paramètre obligatoire qui demande un **int** valeur d’entrée. *curseur* est la *gérer* valeur générées par SQL Server et retournée par la procédure sp_cursoropen.  
  
 *optype*  
 Paramètre obligatoire qui désigne quelle opération le curseur effectuera. *optype* requiert l’une des opérations suivantes **int** valeurs d’entrée.  
  
|Valeur|Nom| Description|  
|-----------|----------|-----------------|  
|0X0001|UPDATE|Utilisée pour mettre à jour une ou plusieurs lignes dans le tampon d'extraction.  Les lignes spécifiées dans *rownum* nouvel accès et mises à jour.|  
|0x0002|DELETE|Utilisée pour supprimer une ou plusieurs lignes dans le tampon d'extraction. Les lignes spécifiées dans *rownum* sont accessibles de nouveau et supprimé.|  
|0X0004|INSERT|Insère des données sans générer SQL **insérer** instruction.|  
|0X0008|REFRESH|Utilisée pour remplir la mémoire tampon à partir de tables sous-jacentes et peut être utilisée pour actualiser la ligne si une mise à jour ou une suppression échoue en raison d'un contrôle d'accès concurrentiel optimiste, ou après une opération UPDATE.|  
|0X10|LOCK|Provoque un SQL Server U-acquisition du verrou sur la page contenant la ligne spécifiée. Ce verrou est compatible avec les verrous S-Lock mais pas avec les verrous X-Lock ou autres verrous U-Lock. Permet d'implémenter le verrouillage à court terme.|  
|0X20|SETPOSITION|Est utilisé uniquement lorsque le programme va émettre un serveur SQL suivantes positionné l’instruction DELETE ou UPDATE.|  
|0X40|ABSOLUTE|Peut uniquement être utilisée avec UPDATE ou DELETE.  ABSOLUTE est utilisée uniquement avec les curseurs KEYSET (est ignorée pour les curseurs DYNAMIC et les curseurs STATIC ne peuvent pas être mis à jour).<br /><br /> Remarque : Si ABSOLUTE est spécifiée sur une ligne dans le jeu de clés n’a pas été extrait, l’opération peut échouer le contrôle d’accès concurrentiel et le résultat obtenu ne peut pas être garanti.|  
  
 *ROWNUM*  
 Spécifie les lignes dans le tampon d'extraction sur lesquelles le curseur interviendra, qu'il mettra à jour ou supprimera.  
  
> [!NOTE]  
>  Cela n'affecte pas le point de départ de toute opération d'extraction RELATIVE, NEXT ou PREVIOUS, ni les mises à jour ou suppressions effectuées à l'aide de sp_cursor.  
  
 *ROWNUM* est un paramètre obligatoire qui demande un **int** valeur d’entrée.  
  
 1  
 Indique la première ligne dans le tampon d'extraction.  
  
 2  
 Indique la deuxième ligne dans le tampon d'extraction.  
  
 3, 4, 5  
 Indique la troisième ligne, etc.  
  
 n  
 Indique la énième ligne dans le tampon d'extraction.  
  
 0  
 Indique toutes les lignes dans le tampon d'extraction.  
  
> [!NOTE]  
>  Est uniquement valide pour une utilisation avec la mise à jour, DELETE, REFRESH ou LOCK *optype* valeurs.  
  
 *table*  
 Nom de la table qui identifie la table qui *optype* s’applique à lors de la définition de curseur implique une jointure ou de noms de colonnes ambigus sont retournés par le *valeur* paramètre. Si aucune table spécifique n'est désignée, la valeur par défaut est la première table dans la clause FROM. *table* est un paramètre optionnel qui requiert une valeur de chaîne d’entrée. La chaîne peut être spécifiée comme tout caractère ou type de données UNICODE. *table* peut être un nom de table en plusieurs parties.  
  
 *valeur*  
 Permet d'insérer ou de mettre à jour des valeurs. Le *valeur* paramètre de chaîne est utilisé uniquement avec UPDATE et INSERT *optype* valeurs. La chaîne peut être spécifiée comme tout caractère ou type de données UNICODE.  
  
> [!NOTE]  
>  Les noms de paramètre pour *valeur* peuvent être affectés par l’utilisateur.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Lorsque vous utilisez RPC, une opération DELETE ou UPDATE positionnée avec un numéro de la mémoire tampon 0 retournera un message DONE avec une *rowcount* 0 (échec) ou 1 (réussite) pour chaque ligne dans le tampon d’extraction.  
  
## <a name="remarks"></a>Notes  
  
## <a name="optype-parameter"></a>Paramètre optype  
 À l’exception des combinaisons de SETPOSITION avec UPDATE, DELETE, actualisation ou verrou ; ABSOLU avec UPDATE ou DELETE, ou le *optype* valeurs s’excluent mutuellement.  
  
 La clause SET de la valeur de la mise à jour est construite à partir de la *valeur* paramètre.  
  
 L’un des avantages de l’utilisation d’INSERT *optype* valeur est que vous pouvez éviter la conversion des données de l’autre qu’un caractère en format caractère pour les insertions. Les valeurs sont spécifiées de la même façon qu'UPDATE. Si une colonne requise n'est pas incluse, INSERT échoue.  
  
-   La valeur SETPOSITION n'affecte pas le point de départ de toute opération d'extraction RELATIVE, NEXT ou PREVIOUS, ni les mises à jour ou suppressions effectuées à l'aide de l'interface sp_cursor. Tout numéro qui ne spécifie pas de ligne dans le tampon d'extraction entraînera l'affectation de la valeur 1 à la position, sans aucune erreur retournée. Une fois SETPOSITION exécutée, la position reste effective jusqu'à la prochaine opération sp_cursorfetch, T-SQL **FETCH** opération ou sp_cursor SetPosition suivante via le même curseur. Une opération sp_cursorfetch suivante définira la position du curseur sur la première ligne dans le nouveau tampon d'extraction alors que d'autres appels de curseur n'affecteront pas la valeur de la position. SETPOSITION peut être liée par une clause OR avec REFRESH, UPDATE, DELETE ou LOCK pour définir la valeur de la position sur la dernière ligne modifiée.  
  
 Si une ligne dans le tampon d’extraction n’est pas spécifiée via la *rownum* paramètre, la position aura la valeur 1, sans aucune erreur retournée. Une fois la position définie, elle reste effective jusqu'à l'exécution de l'opération sp_cursorfetch, T-SQL FETCH ou sp_cursor SETPOSITION suivante sur le même curseur.  
  
 SETPOSITION peut être liée par une clause OR avec REFRESH, UPDATE, DELETE ou LOCK pour définir la position du curseur sur la dernière ligne modifiée.  
  
## <a name="rownum-parameter"></a>Paramètre rownum  
 Si spécifié, le *rownum* paramètre peut être interprété comme le numéro de ligne dans le jeu de clés au lieu du numéro de ligne dans le tampon d’extraction. L'utilisateur est chargé de garantir que le contrôle d'accès concurrentiel est tenu à jour. Cela signifie que, pour les curseurs SCROLL_LOCKS, vous devez maintenir indépendamment un verrou sur la ligne donnée (cette opération peut être effectuée via une transaction). Pour les curseurs OPTIMISTIC, vous devez avoir au préalable extrait la ligne pour effectuer cette opération.  
  
## <a name="table-parameter"></a>Paramètre de table  
 Si le *optype* valeur est mise à jour ou insérer une mise à jour complète ou une instruction insert est soumise en tant que le *valeur* paramètre, la valeur spécifiée pour *table* est ignoré.  
  
> [!NOTE]  
>  En ce qui concerne les vues, une seule table qui participe à la vue peut être modifiée. Le *valeur* les noms de colonne de paramètre doivent refléter les noms de colonnes dans la vue, mais le nom de table peut être celui de la table de base sous-jacente (auquel cas sp_cursor remplacera le nom de la vue).  
  
## <a name="value-parameter"></a>Paramètre value  
 Il existe deux alternatives pour les règles d’utilisation *valeur* comme indiqué précédemment dans la section Arguments :  
  
1.  Vous pouvez utiliser un nom qui est « @ » ajouté au nom de la colonne dans la liste de sélection pour une nommée *valeur* paramètres. Un avantage de cette alternative est que la conversion des données peut ne pas être nécessaire.  
  
2.  Utiliser un paramètre pour envoyer une instruction UPDATE ou INSERT complète, ou utilisez plusieurs paramètres pour envoyer des parties d’une instruction UPDATE ou INSERT sur lequel SQL Server sera puis créer dans une instruction complète. Vous trouverez des exemples dans la section Exemples plus loin dans cette rubrique.  
  
## <a name="examples"></a>Exemples  
  
### <a name="alternative-value-parameter-uses"></a>Autres utilisations de paramètres de valeurs  
 Pour UPDATE :  
  
 Lorsqu'un paramètre unique est utilisé, une instruction UPDATE peut être envoyée à l'aide de la syntaxe suivante :  
  
 `[ [ UPDATE <table name> ] SET ] {<column name> = expression} [,…n]`  
  
> [!NOTE]  
>  Si mise à jour \<nom_table > est spécifié, toute valeur spécifiée pour le *table* paramètre sera ignoré.  
  
 Lorsque plusieurs paramètres sont utilisés, le premier paramètre doit être une chaîne sous la forme suivante :  
  
 `[ SET ] <column name> = expression  [,...n]`  
  
 et les paramètres suivants doivent être sous la forme :  
  
 `<column name> = expression  [,...n]`  
  
 Dans ce cas, le \<nom de table > dans la mise à jour construite instruction est celui spécifié ou défini par défaut par le *table* paramètre.  
  
 Pour INSERT :  
  
 Lorsqu'un paramètre unique est utilisé, une instruction INSERT peut être envoyée à l'aide de la syntaxe suivante :  
  
 `[ [ INSERT [INTO] <table name> ] VALUES ] ( <expression> [,...n] )`  
  
> [!NOTE]  
>  Si INSERT  *\<nom de table >* est spécifié, toute valeur spécifiée pour le *table* paramètre sera ignoré.  
  
 Lorsque plusieurs paramètres sont utilisés, le premier paramètre doit être une chaîne sous la forme suivante :  
  
 `[ VALUES ( ] <expression>  [,...n]`  
  
 et les paramètres suivants doivent être sous la forme :  
  
 `expression [,...n]`  
  
 sauf lorsque VALUES a été spécifié, auquel cas une « ) » de fin est nécessaire après la dernière expression. Dans ce cas, le  *\<nom de table >* dans le UDPATE construite instruction est celui spécifié ou défini par défaut par le *table* paramètre.  
  
> [!NOTE]  
>  Il est possible d'envoyer un paramètre comme paramètre nommé, c'est-à-dire « `@VALUES` ». Dans ce cas, aucun autre paramètre nommé ne peut être utilisé.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
