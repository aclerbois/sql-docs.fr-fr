---
title: sp_cursorfetch (Transact-SQL) | Documents Microsoft
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
- sp_cursorfetch
- sp_cursorfetch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorfetch
ms.assetid: 14513c5e-5774-4e4c-92e1-75cd6985b6a3
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 9bbffe757b6b9c76bc1eb0b95e883f3d4d30b461
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcursorfetch-transact-sql"></a>sp_cursorfetch (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Extrait une mémoire tampon d'une ou de plusieurs lignes de la base de données. Le groupe de lignes dans cette mémoire tampon est appelé du curseur *tampon d’extraction*. sp_cursorfetch est appelée en spécifiant ID = 7 dans un paquet de stream (TDS) de données tabulaires.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorfetch cursor  
    [ , fetchtype[ , rownum [ , nrows] ]]   
```  
  
## <a name="arguments"></a>Arguments  
 *cursor*  
 Est un *gérer* valeur générée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et retournée par sp_cursoropen. *curseur* est un paramètre obligatoire qui demande un **int** valeur d’entrée. Pour plus d’informations, consultez la section Notes plus loin dans cette rubrique.  
  
 *FetchType*  
 Spécifie la mémoire tampon de curseur à extraire. *FetchType* est un paramètre optionnel qui requiert l’une des valeurs d’entrée entières suivantes.  
  
|Valeur|Nom| Description|  
|-----------|----------|-----------------|  
|0x0001|FIRST|Extrait la première mémoire tampon de *nrows* lignes. Si *nrows* est égal à 0, le curseur est positionné avant le jeu de résultats et ne retourne aucune ligne.|  
|0x0002|NEXT|Extrait la mémoire tampon suivante de *nrows* lignes.|  
|0x0004|PREV|Extrait la mémoire tampon précédente de *nrows* lignes.<br /><br /> Remarque : L’utilisation de PREV pour un curseur FORWARD_ONLY retourne un message d’erreur parce que FORWARD_ONLY prend uniquement en charge le défilement dans une direction.|  
|0x0008|LAST|Extrait la dernière mémoire tampon de *nrows* lignes. Si *nrows* est égal à 0, le curseur est positionné après le jeu de résultats et ne retourne aucune ligne.<br /><br /> Remarque : L’utilisation de LAST pour un curseur FORWARD_ONLY retourne un message d’erreur parce que FORWARD_ONLY prend uniquement en charge le défilement dans une direction.|  
|0x10|ABSOLUTE|Extrait une mémoire tampon de *nrows* lignes en commençant par le *rownum* ligne.<br /><br /> Remarque : L’utilisation d’ABSOLUTE pour un curseur dynamique ou un curseur FORWARD_ONLY retourne un message d’erreur parce que FORWARD_ONLY prend uniquement en charge le défilement dans une direction.|  
|0x20|RELATIVE|Extrait la mémoire tampon de *nrows* lignes en commençant par la ligne qui est spécifiée comme étant le *rownum* valeur des lignes à partir de la première ligne dans le bloc actuel. Dans ce cas *rownum* peut être un nombre négatif.<br /><br /> Remarque : L’utilisation de RELATIVE pour un curseur FORWARD_ONLY retourne un message d’erreur parce que FORWARD_ONLY prend uniquement en charge le défilement dans une direction.|  
|0x80|REFRESH|Remplit la mémoire tampon à partir de tables sous-jacentes.|  
|0x100|INFO|Récupère des informations sur le curseur. Ces informations sont retournées à l’aide de la *rownum* et *nrows* paramètres. Par conséquent, lorsque l’INFO est spécifiée, *rownum* et *nrows* deviennent des paramètres de sortie.|  
|0x200|PREV_NOADJUST|Est utilisée comme PREV. Toutefois, si les premiers résultats du jeu sont trouvés prématurément, les résultats peuvent varier.|  
|0x400|SKIP_UPDT_CNCY|Doit être utilisé avec une des autres *fetchtype* valeurs, à l’exception des informations.|  
  
> [!NOTE]  
>  Il n'existe aucune prise en charge de la valeur 0x40.  
  
 Pour plus d’informations, consultez la section Notes plus loin dans cette rubrique.  
  
 *ROWNUM*  
 Est un paramètre facultatif qui est utilisé pour spécifier la position de ligne pour ABSOLUTE et INFO *fetchtype* valeurs en utilisant uniquement des valeurs entières pour l’entrée ou sortie ou les deux. *ROWNUM* sert de décalage de lignes pour le *fetchtype* valeur RELATIVE de bit. *ROWNUM* est ignoré pour toutes les autres valeurs. Pour plus d’informations, consultez la section Notes plus loin dans cette rubrique.  
  
 *nRows*  
 Paramètre optionnel utilisé pour spécifier le nombre de lignes à extraire. Si *nrows* n’est pas spécifié, la valeur par défaut est 20 lignes. Pour définir la position sans retourner de données, spécifiez la valeur 0. Lorsque *nrows* est appliqué à la *fetchtype* interrogation, il retourne le nombre total de lignes dans cette requête.  
  
> [!NOTE]  
>  *nRows* est ignorée par l’actualisation *fetchtype* valeur en bits.  
>   
>  Pour plus d’informations, consultez la section Notes plus loin dans cette rubrique.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Lorsque vous spécifiez la valeur de bit INFO, les valeurs qui peuvent être retournées sont affichées dans les tableaux suivants.  
  
> [!NOTE]  
>  : Si vous ne retourne aucune ligne, le contenu de la mémoire tampon reste inchangé.  
  
|*\<ROWNUM >*|La valeur|  
|------------------|------------|  
|Si le curseur n'est pas ouvert|0|  
|Si le curseur est positionné avant le jeu de résultats|0|  
|Si le curseur est positionné après le jeu de résultats|-1|  
|Pour les curseurs KEYSET et STATIC|Numéro de ligne absolu de la position actuelle dans le jeu de résultats|  
|Pour les curseurs DYNAMIC|1|  
|Pour ABSOLUTE|-1 retourne la dernière ligne dans un jeu.<br /><br /> -2 retourne la deuxième à la dernière ligne dans un jeu, et ainsi de suite.<br /><br /> Remarque : Si plusieurs lignes est demandé à extraire dans ce cas, les deux dernières lignes du jeu de résultats sont retournés.|  
  
|*\<nRows >*|La valeur|  
|-----------------|------------|  
|Si le curseur n'est pas ouvert|0|  
|Pour les curseurs KEYSET et STATIC|En général, taille du jeu de clés actuel.<br /><br /> **– m** si le curseur est en création asynchrone avec *m* lignes trouvées à ce stade.|  
|Pour les curseurs DYNAMIC|-1|  
  
## <a name="remarks"></a>Notes  
  
## <a name="cursor-parameter"></a>Paramètre de curseur  
 Avant toute opération d'extraction, la position par défaut d'un curseur se situe avant la première ligne du jeu de résultats.  
  
## <a name="fetchtype-parameter"></a>Paramètre fetchtype  
 À l’exception de SKIP_UPD_CNCY, les *fetchtype* valeurs s’excluent mutuellement.  
  
 Lorsque SKIP_UPDT_CNCY est spécifié, les valeurs de colonne timestamp ne sont pas écrites dans la table de jeux de clés lorsqu'une ligne est extraite ou actualisée. Si la ligne de jeu de clés est mise à jour, les valeurs des colonnes timestamp restent inchangées. Si la ligne de jeu de clés est insérée, les valeurs des colonnes timestamp sont indéfinies.  
  
 Pour les curseurs KEYSET, cela signifie que les valeurs de la table de jeux de clés sont définies pendant la dernière opération FETCH non ignorée, si celle-ci a été effectuée. Sinon, les valeurs sont définies pendant le remplissage.  
  
 Pour les curseurs DYNAMIC, cela signifie que si l'opération Skip est effectuée avec une actualisation, elle produit les mêmes résultats que KEYSET. Pour tout autre type d'extraction, la table de jeux de clés est tronquée. Cela signifie que les lignes sont insérées et que les valeurs de la ou des colonnes timestamp sont indéfinies. Par conséquent, lorsque vous exécutez sp_cursorfetch pour les curseurs DYNAMIC, évitez d'utiliser SKIP_UPDT_CNCY pour toute opération autre que REFRESH.  
  
 Si une opération d'extraction échoue parce que la position de curseur demandée se situe au-delà du jeu de résultats, la position de curseur est définie juste après la dernière ligne. Si une opération d'extraction échoue parce que la position de curseur demandée se situe avant le jeu de résultats, la position de curseur est définie avant la première ligne.  
  
## <a name="rownum-parameter"></a>Paramètre rownum  
 Lorsque vous utilisez *rownum*, la mémoire tampon est remplie à partir de la ligne spécifiée.  
  
 Le *fetchtype* valeur ABSOLUTE fait référence à la position de *rownum* au sein de la totalité du résultat défini. Un nombre négatif avec ABSOLUTE spécifie que l'opération compte les lignes à partir de la fin du jeu de résultats.  
  
 Le *fetchtype* valeur RELATIVE fait référence à la position de *rownum* par rapport à la position du curseur au début de la mémoire tampon en cours. Un nombre négatif avec RELATIVE spécifie que le curseur retourne en arrière par rapport à sa position actuelle.  
  
## <a name="nrows-parameter"></a>Paramètre nrows  
 Le *fetchtype* valeurs REFRESH et INFO ignorent ce paramètre.  
  
 Lorsque vous spécifiez un *fetchtype* valeur First qui a un *nrow* la valeur 0, le curseur est positionnée avant le jeu de résultats qui n’a aucune ligne dans le tampon d’extraction.  
  
 Lorsque vous spécifiez un *fetchtype* valeur Last qui a un *nrow* la valeur 0, le curseur est positionnée après le jeu de résultats qui ne comporte aucune ligne dans le tampon d’extraction actuel.  
  
 Pour le *fetchtype* valeurs de suivant, PREV, ABSOLUTE, RELATIVE et PREV_NOADJUST, une *nrow* la valeur 0 n’est pas valide.  
  
## <a name="rpc-considerations"></a>Éléments RPC à prendre en considération  
 L'état de retour RPC indique si le paramètre de taille du jeu de clés est définitif ou pas, en d'autres termes si le jeu de clés ou la table temporaire est rempli de façon asynchrone.  
  
 L'une des valeurs affichées dans le tableau suivant est affectée au paramètre d'état RPC.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|La procédure a été correctement exécutée.|  
|0x0001|La procédure a échoué.|  
|0x0002|Une extraction dans une direction négative a provoqué la définition de la position du curseur au début du jeu de résultats, alors que l'extraction aurait dû se situer logiquement avant les résultats.|  
|0x10|Un curseur avance rapide a été fermé automatiquement.|  
  
 Les lignes sont retournées comme un jeu de résultats classique, à savoir format de colonne (0x2a), lignes (0xd1), suivi de l'indication de fin (0xfd). Les jetons de métadonnées sont envoyés sous le même format que celui spécifié pour sp_cursoropen, à savoir 0x81, 0xa5 et 0xa4 pour les utilisateurs SQL Server 7.0, et ainsi de suite. Les indicateurs d'état de ligne sont envoyés en tant que colonnes masquées, comme en mode BROWSE, à la fin de chaque ligne avec le rowstat du nom de la colonne et le type de données INT4. Cette colonne rowstat comporte l'une des valeurs affichées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0x0001|FETCH_SUCCEEDED|  
|0x0002|FETCH_MISSING|  
  
 Étant donné que le protocole TDS n'offre aucun moyen d'envoyer la colonne d'état de fin sans envoyer les colonnes précédentes, les données fictives sont envoyées à la place des lignes manquantes (champs Nullable avec une valeur Null, champs de longueur fixe avec une valeur 0, valeur vide, ou valeur par défaut pour cette colonne, selon le cas).  
  
 Le nombre de lignes DONE sera toujours égal à zéro. Le message DONE contient le nombre de lignes de jeu de résultats réel et des messages d'erreur ou d'information peuvent apparaître entre les messages TDS.  
  
 Pour demander que les métadonnées relatives à la liste de sélection du curseur soient retournées dans le flux TDS, affectez à l'indicateur d'entrée RPC RETURN_METADATA la valeur 1.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-prev-to-change-a-cursor-position"></a>A. Utilisation de PREV pour modifier une position de curseur  
 Imaginons qu'un curseur h2 produit un jeu de résultats avec le contenu suivant dont la position actuelle est indiquée ci-dessous :  
  
```  
row 1 contents      
row 2 contents  
row 3 contents  
row 4 contents  <-- current position  
row 5 contents   
row 6 contents  
```  
  
 Ensuite, sp_cursorfetch PREV qui a un *nrows* valeur 5 positionne logiquement les curseur deux lignes avant la première ligne du jeu de résultats. Dans ces cas, le curseur est ajusté pour démarrer à la première ligne et retourner le nombre de lignes demandé. Cela signifie souvent qu'il retourne des lignes qui étaient dans le tampon d'extraction PRIOR.  
  
> [!NOTE]  
>  Il s'agit précisément du cas où le paramètre d'état RPC a la valeur 2.  
  
### <a name="b-using-prevnoadjust-to-return-fewer-rows-than-prev"></a>B. Utilisation de PREV_NOADJUST pour retourner moins de lignes que PREV  
 PREV_NOADJUST n'inclut jamais aucune des lignes au niveau de la position de curseur actuelle ou après celle-ci dans le bloc des lignes qu'il retourne. Dans les cas où PREV retourne des lignes après la position actuelle, PREV_NOADJUST retourne moins de lignes que prévu dans *nrows*. Étant donné l’actuel positionner dans l’exemple A plus haut, lorsque PREV est appliqué, sp_cursorfetch (h2, 4, 1, 5) extrait les lignes suivantes :  
  
```  
row1 contents   
row2 contents  
row3 contents  
row4 contents  
row5 contents  
```  
  
 Toutefois, lorsque PREV_NOADJUST est appliqué, sp_cursorfetch (h2, 512, 6, 5) extrait uniquement les lignes suivantes :  
  
```  
row1 contents   
row2 contents  
row3 contents   
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
